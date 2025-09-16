
# Idle()
## Summary
Function `Idle` is a cooperative, non-blocking scheduler that advances zone processing using two
nested state machines. It iterates through zones and their groups over successive calls, executing
in order: external sensors, group sensors, group averages, outputs, inputs, and group cleanup, then
zone-level cleanup (baffles, alarms, egg flow). When a zone pass completes or a restart is pending,
it reinitializes per-pass and per-zone aggregators, clears DGP flags, optionally processes quality
and utilities, and prepares indices for the next pass.
## Code Analysis
### Inputs
- No parameters. Relies on globals such as:
  - `exercise`, `adr_non0()`, `cycle_dgp()` for DGP exercising.
  - `z`, `num_zones`, `zone[]` configuration and runtime data.
  - Aggregators: `highZone`, `lowZone`, `highZoneAvg`, `lowZoneAvg`, `sumAvg`, `nmbrAvg`.
  - Baffle and egg-flow structures: `baffleGrp`, `zone[z]->baffle[]`, egg counters and totals.
  - `dgpData[]`, `MAX_DGP`, `MACRO_CLEAR_BIT` for DGP flags.
  - Options/pointers: `optSystem`, `qualityPtr`, `utilityPtr`.
  - Control flags/timers: `restart_process`, `rs232_time`, `scan_time`, `comm_errs`, etc.
### Flow
1. If `exercise` is defined, call `cycle_dgp()`. If the current zone is valid and not restarting,
   advance the zone-level state machine.
2. ZoneExtTemp: zero baffle group mapping and process external sensors; move to ZoneGroup.
3. ZoneGroup: run the group-level state machine across groups — Sensors → Average → Outputs →
   Inputs → GroupCleanup. Initialize per-zone/group aggregators at the start of a group.
4. When all groups finish, run `ZoneCleanup()` (baffles, alarms, egg flow), then reset to
   ZoneExtTemp (the next call continues the cycle).
5. If the pass is done or restarting: clear DGP flags, bump RS232 pass counters, optionally process
   quality/utilities, reset egg/zone aggregators and timers, and if restarting, clear current
   zone’s runtime fields and reset indices.
### Outputs
- Advances internal static indices (`g`, `s`, `p`, `i`) and state enums (`grpState`, `zoneState`).
- Updates per-zone/group aggregators and baffle counters; resets them at appropriate boundaries.
- Clears DGP flags each pass end; increments `rs232_passes` when timed.
- Calls processing hooks: `ProcessExternalSensors`, `ProcessSensors`, `ProcessAvg`, `ProcessOutputs`,
  `ProcessInputs`, `GroupCleanup`, `ZoneCleanup`, and optionally `ProcessQuality`/`ProcessUtilities`.
- Reinitializes egg-flow totals and communication/scan statistics at pass boundaries.


# Functions State Machine - Idle

# ProcessSensor()

## Summary
Function that processes a single sensor `s` of group `g` in the current zone `z`. On the first
sensor of a group (`s == 0`), it resets per-pass aggregates (group sum/count and differential
halves; min/max trackers `lowSen`/`highSen`). For valid devices/addresses, it updates digital
rates, reads temperature-type sensors, aggregates readings, computes half-group sums for
differential analysis, dispatches egg-related processing, invokes user calculations, updates
group-wide min/max, and raises analog alarms when enabled and limits are set. Invalid or
missing readings are marked with 65534; a sentinel 32200 skips aggregation.

## Code Analysis
### Inputs
- `g` (unsigned char): group index within the current zone `z`.
- `s` (unsigned char): sensor index within group `g`.
- Uses globals/externals: `z`, `zone[z]->group[g]`, `input_defs`, and file-scope aggregates such as
  `lowSen`, `highSen`, `sumDiff1/2`, `nmbrDiff1/2`.

### Flow
1. On `s == 0`, reset per-group pass aggregates and min/max trackers.
2. If the sensor address is undefined, set `iPtr->value` to 65534 (N/A) and return.
3. For valid device types, update digital rates; for temperature-type sensors, read and store
   the value unless a sentinel (32200) indicates no update.
4. If a valid update occurred, aggregate sum/count, split into first/second-half sums for
   differential analysis, run egg counter processing (types 4, 19, 24), and optional `user_calc`.
5. Update `lowSen`/`highSen`; if enabled and limits exist, evaluate analog alarms with
   offset-adjusted thresholds.

### Outputs
- No return value; side effects include:
- Updates `iPtr->value` (65534 on invalid/missing; otherwise latest reading).
- Aggregates into `grpPtr->sum_read` and `grpPtr->tot_read`; updates `sumDiff1/2` and `nmbrDiff1/2`.
- Updates pass-level min/max trackers `lowSen` and `highSen`.
- Executes `ProcessEggCounter` for egg-related sensors and `user_calc` for user-defined types.
- May trigger analog alarms via `AnalogAlarm` when enabled and thresholds are configured.

# ProcessAvg()

## Summary
Function that finalizes per-group processing: it updates secondary sensors (two entries in `grpPtr->gas_in`),
computes the group's average temperature and differential averages, evaluates and sets/clears group-level
alarms, updates hourly history extrema, and aggregates zone-wide statistics. It handles digital sensors
(rates/counters), egg counting (counters/dozens/camera), optional user calculations, and analog alarm
thresholds with zone offsets and optional scaling.

## Code Analysis
### Inputs
- `g`: index of the target group within the current zone (`z` is a global).
- Uses global/config structures: `zone[z]`, `input_defs[]`, `hour_hist[z]`, and static accumulators
  like `sumDiff1/2`, `nmbrDiff1/2`, `highSen/lowSen`.

### Flow
1. For each of the two secondary inputs (`gas_in[0..1]`): read/update digital or counter values, run
   egg-counter processing for designated types, and invoke `user_calc` when configured.
2. If applicable, compute analog alarm thresholds (with zone offsets and optional scale) and call
   `AnalogAlarm`.
3. Compute the group average (`avg_val`) from `sum_read/tot_read` and differential averages
   (`avgDiff1/2`) when enough samples exist.
4. Evaluate group low/high temperature alarms using dynamic offsets/guards; set or clear alarms and
   optionally ring the bell.
5. Update hourly history (group hi/lo temp and gas inputs), store group hi/lo sensors, and roll up
   zone-wide min/max/average aggregates.

### Outputs
- Side effects only (void function):
  - `grpPtr->avg_val`, `grpPtr->grp_hi`, `grpPtr->grp_lo`, and `grpPtr->gas_in[ii].value/a_count`.
  - Alarm state via `set_alarm`/`clr_alarm` and `grpPtr->alarm_on`; possible `con_bell` change.
  - Hourly history fields: `hour_hist[z]->hi_g`, `lo_g`, `hi_num`, `lo_num`, and `hi_gas_in[]/lo_gas_in[]`.
  - Differential averages: `avgDiff1`, `avgDiff2`.
  - Zone aggregates: `highZoneAvg`, `lowZoneAvg`, `highZone`, `lowZone`, `nmbrAvg`, `sumAvg`.

# ProcessOutputs()

## Summary
Function that computes and applies the control action for one output identified by (`z`,`g`,`p`).
It evaluates configured logic (primary sensor source, secondary sensors, differential averages,
time schedules, cycle timers, outside sensors) to decide ON/OFF or an analog level, with special
handling for PID-controlled outputs, light-level ramping, and variable-speed (VS) fans. It then
writes the digital or analog command to hardware and updates runtime state, timers, and flags.

## Code Analysis
### Inputs
- `z`: zone index (unsigned char).
- `g`: group index within the zone (unsigned char).
- `p`: output index within the group (unsigned char).
- `avgDiff1`, `avgDiff2`: differential readings used when primary mode requires differential logic (unsigned short).

### Flow
1. Resolve pointers (`gPtr`, `oPtr`, `lPtr`) and verify the output function is defined; if PID (func 23/24), call `ProcPidFans` and, if active, assert the relay bit.
2. For non-PID, compute the primary action from `prim_sens` (group/zone avg, individual, or differential) via `OutputAction`/`AnalogAction`, honoring test mode and heating/cooling offsets.
3. Merge secondary sensors using `decide`; evaluate time schedules (including 999-day light state and ramp up/down), cycle timers, and optional outside sensor—combining each with primary via `decide`.
4. Drive the output according to `out_dat`: digital (set/clear), analog linear, table, or inverted table; handle VS anticipation, zone-average coordination, and accumulate VS totals.
5. Maintain auxiliary behavior: create one-shot second timers when needed, clear/disable timers when not enabled, force OFF when group disabled, and toggle `tempFlags` bit 0 when the output name is "CC1".

### Outputs
- No return value; effects are via side effects:
  - Hardware relay asserted/deasserted with `port0_setbit`/`port0_clrbit`, or analog level written with `anlog_out`.
  - `oPtr->state` updated to reflect OFF/ON, intermediate codes (e.g., TIME_ON, INT_ON, SECONDS), device source, or percentage.
  - Optional scheduling of second-level timers (`zone[z]->second[]`) and updates to VS aggregation (`vs_total`, `vs_num`).
  - `zone[z]->tempFlags` bit 0 set/cleared for output named "CC1".

# ProcessInputs()

## Summary
Function `ProcessInputs` processes one auxiliary input (`auxinp[i]`) of a group `g` in the
current zone `z`. It reads/scales the input, updates counters (egg, dozen, egg camera),
runs user-calculations, applies time/schedule gating, performs special handling
(manure belt, verify, RPM, light/CT, VS fan verify), and manages alarm state with
debounce and priority. On changes or violations, it calls `set_alarm`/`clr_alarm`,
updates `iPtr->state/a_count/value`, and accumulates tier counts for egg lift.

## Code Analysis
### Inputs
- `g`: group index within the current zone `z`.
- `i`: auxiliary input index within `zone[z]->group[g]->auxinp`.
- Uses configured metadata in `iPtr` and defs:
  - `iPtr->dev_type`, `iPtr->address`, `iPtr->tc`, `iPtr->lo_lim`, `iPtr->hi_lim`,
    `iPtr->a_count`, `iPtr->value`, `iPtr->baffle`.
  - `input_defs[dev_type-1].an_dig_sw`, `priorty`, `p1`, `p2`, `p3`, messages.
  - Group outputs `grpPtr->output[...]` and zone timing/schedules.

### Flow
1. Read/scale the input: for analog/switch/verify/RPM/min-baffle/feed-monitor/light/CT, calls
   `DigitalSensorRead` and stores a non-negative value.
2. If the input is digital (counter), compute rate (`CounterRate`), handle egg/dozen/camera
   counting (accumulate tier deltas and call `ProcessEggCounter`).
3. Run `user_calc` for types with `an_dig_sw > 6`; detect manure belt conditions via
   `user_belt_calc` when configured.
4. Gate alarms via debounce (`a_count`), priority, and schedules (`tc`, light schedule),
   then evaluate special cases: Verify Input (invert limits by output state), RPM,
   Light/CT sensor (compare to output state), and VS fan verify (compare to target % with tolerance).
5. If alarm conditions met and enabled, raise low/high alarms with `set_alarm`; otherwise,
   clear with `clr_alarm` when back within limits and reset counters/state accordingly.

### Outputs
- Side effects only; no return value.
- Updates `iPtr->value`, `iPtr->a_count`, `iPtr->state`, `iPtr->alarm_on`.
- May update tier `rowCount` for egg lift accumulation.
- May set global flags like `alm_act` and `con_bell` and invoke `set_alarm`/`clr_alarm`.

# GroupCleanup()

## Summary
Function `GroupCleanup` finalizes per-group processing at the end of a group pass. It writes computed egg-counter statistics into the group's structure, safely accumulates these into zone-level totals (with overflow guards), resets per-group egg accumulators, and analyzes each group output to count active devices, fan/heat starts, and baffle participation. It maps baffles to their owning group, updates baffle demand (`num_on`) based on output states (including VS logic and static pressure checks), and tallies fans/heaters on. Finally, it derives the group's overall alarm state by reducing all sensor, gas, and auxiliary input alarm states via `GetHighestAlarmState`.

## Code Analysis
### Inputs
- `g` (unsigned char): group index within current zone `z`.
- Implicit context: uses global/extern state such as `z`, `zone[z]`, precomputed egg accumulators (`nmbrEggCnts`, `lowEggCnt`, `highEggCnt`, `sumEggCnts`, `sumDozYest`, `sumEggsPerMin`), zone totals (`sumZoneEggCnts`, `sumZoneDozYest`, etc.), baffle and fan tracking (`baffleGrp`, `vs_fan_zone`, `fansOn`, `nmbrFans`), and alarm aggregator `group_state`.

### Flow
1. If any egg counters were tallied, write min/avg/max/total/rpm into the group, compute `g_min_rpm`, and roll these into zone-level aggregates with overflow checks; otherwise set `num_cnts` to 0.
2. Normalize `g_egg_lo` if it was unset, then reset all per-group egg accumulators for the next group.
3. Iterate group outputs: map baffles to the group, count active outputs, exclude non-counting cases (purge, schedule-only, PID/idef, heat), update baffle `num_on` based on output state and logic, track VS fan context and static-pressure behavior, and count fans/heaters on.
4. Update fan-starts/heat-starts deltas into `hour_hist[z]`, store current `fans_strt`, `fans_on`, and `heat_on/heat_strt` in the group.
5. Reduce all sensor/gas/aux input alarm states into `group_state` and store it as the group's final `state`.

### Outputs
- Side effects on `zone[z]->group[g]`: `num_cnts`, `g_egg_lo/avg/hi/total/tld/rpm/min_rpm`, `fans_strt`, `fans_on`, `heat_on`, `heat_strt`, and `state`.
- Updates zone-level aggregates: `highZoneEggCnt`, `lowZoneEggCnt`, `sumZoneEggCnts`, `sumZoneDozYest`, `sumZoneEggsPerMin`, `nmbrZoneEggCnts`.
- Resets per-group egg accumulators (`lowEggCnt`, `highEggCnt`, `avgEggCnt`, `sumEggCnts`, `sumDozYest`, `sumEggsPerMin`, `nmbrEggCnts`).
- Baffle mapping and demand: `baffleGrp[*]`, `zone[z]->baffle[*].num_on`, and VS context `vs_fan_zone`.
- Hourly stats: `hour_hist[z]->fan_starts`, `hour_hist[z]->heat_starts`; zone fan counts `fansOn`, `nmbrFans`.


