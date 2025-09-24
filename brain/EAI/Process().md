
## Summary
Function `process()` is the central periodic scheduler. It checks time via `CheckClock()` and, based on compile-time flags (`VIEW`, `NETVAR`), executes per-second (`Secs()`), per-minute (`by_minute()`), hourly (`hourly()`), and daily (`daily()`) tasks. In viewer builds it also aggregates egg rates across zones into per-machine totals, updates viewer-side controls and speeds, converts inline egg counts to cases, and redistributes/logs data. In non-viewer builds it services serial I/O (`SendSerialData()`), background work (`Idle()`), and optional quality/utility minute hooks gated by `optSystem` bits. The function returns no value and operates through globals and subsystem calls.
## Code Analysis
### Inputs
- None (function takes no parameters).
- Uses global flags and data: `tock`, `tic_min`, `tic_hour`, `tic_day`, `skipSecsComm`, `passed_comm`, `eggFlowExist`, `num_zones`, `zone[]`, `egg_machine[]`, `belt_time`, `viewFlags`, `eggCountDef`, `input_defs`, `optSystem`, and time/date (`c_time`, `c_date`).
- Uses subsystem APIs: `CheckClock()`, `Secs()`, `SecsComm()`, `by_minute()`, `hourly()`, `daily()`, `SendSerialData()`, `Idle()`, and viewer/NETVAR functions when enabled.
### Flow
1. Call `CheckClock()`. If `VIEW`: on `tock`, call `Secs()` and aggregate `zEggRPM` across zones into per-machine `egg_rate`, storing into `egg_machine[*].recd_cpm`; rotate `z`.
2. If `NETVAR` and `VIEW`: when `viewFlags` bit 2 is set, send mode switch updates; if `belt_time > 1`, send speed and reset `belt_time`; accumulate inline totals per machine and convert to cases (`t_inline_cs`).
3. If `tic_min`, clear it; call `by_minute()` and (in `VIEW`+`NETVAR`) send processing, write 10-minute history/inventory, and redistribute; if `VIEW` and not `NETVAR`, read viewer data.
4. In non-`VIEW` builds: service serial (`SendSerialData()`), `Idle()`, run `Secs()` on `tock` if comm not in progress, and `SecsComm()` when `skipSecsComm != 0`; on `tic_min`, call `by_minute()` and optional `QualityMinute`/`UtilityMinute` based on `optSystem`.
5. Independently handle `tic_hour` → `hourly()` and `tic_day` → `daily()`.
### Outputs
- Return: void.
- Side effects:
  - Updates `egg_machine[*].recd_cpm` and per-machine inline case totals.
  - Sends viewer/NETVAR items (mode switch, speeds, processing), writes history/inventory, and redistributes data (when enabled).
  - Advances timers/counters and executes minute/hour/day subsystems.
  - Performs serial servicing and background tasks in non-viewer builds.
