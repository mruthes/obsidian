typedef struct {
	char name[ZONE_NAME_LEN+1];
	unsigned char num_grps;				/* groups configured for zone */
	GROUP_TYPE *group[MAX_GRP];
	BAF_TYPE baffle[MAX_BAFFLES];		/* baffle definition table */
	TC_TYPE time_cod;					// Time schedules for zone
	CYC_TYPE cyc_tmr;					// Cycle timers for zone
	ALM_DEF_TYPE alm_def[NMBR_IDEF+2];	/* where to send this zone's alarms */
	unsigned short hi_alm_guard;		/* how many degrees over outside temp to alarm */
	unsigned char fans_on;				/* current fans active */
	unsigned char num_fans;				/* total possible fans active */
	unsigned short zon_lo;				/* extreme individual temp readings */
	unsigned short zon_hi;
	unsigned short lo_avg;				/* low group average in the zone */
	unsigned short zon_avg;				/* zone average temp */
	unsigned short hi_avg;				/* high group average in the zone */
	unsigned char state;				/* alarm state */
	unsigned short z_hi_lim;			/* zone temp alarm thresholds */
	unsigned short z_lo_lim;
	unsigned char alarm_on;				// bit 0=zone alarm, 1=system alarm
	unsigned short z_egg_lo;			// Lowest egg counter value
	unsigned short z_egg_avg;			// Average egg count value
	unsigned short z_egg_hi;			// Highest egg counter value
	unsigned long zEggTotal;			// Todays total
	unsigned long zEggTotalYest;		// Yesterdays total
	unsigned short zEggRPM;				// Egg rate
	unsigned short optimum_target;		// Target when temp optimum on
	unsigned short max_oset;			// Min target when no schedule selected
	unsigned short sec_target;			// gas_in[0] target
	unsigned short terc_target;			// gas_in[1] target
	unsigned char sched_style;			// No schedule, 99 day schedule, hourly selection
	unsigned short b_HeatOffsetDown;	// Differences between optimum and target temp
	unsigned short b_CoolOffsetDown;
	unsigned short b_HeatOffsetUp;
	unsigned short b_CoolOffsetUp;
	short sec_sens_oset[2];				// Offset for secondary targets
	unsigned short room_air;			// N/U
	unsigned short tempFlags;			// Temporary flags
										// bit 0 - Set if cool cells on "CC1" (limits inlet opening)
										// bit 1 - Set when bird scales need updating
	ADR_TYPE light_adr;					// Old light address, typically not used
	EGG_TYPE eggs_out;					// Egg parameters
	EGGLIFT_TYPE salmet[8];	// Set at 8 to support 8 tiers
	unsigned char liftMoving;			// True when egg lift moving
	BATCH_TYPE batch;					// Batch flow settings
	unsigned short store_it[30];
/*  Old store_it assignments:
augerOffWeight		0.  Weight to shut off fill auger
midnightOnHr/Min	1.  Start time for midnight feeding
midnightOffHr/Min	2.  End time for midnight feeding
max_h2o_rate		3.  Max water usage per minute per meter
-					4.	Static Pressure limits
staticDiv			5:	Static Pressure divisor
maxBafPos			6.	Max baffle position
-					7.	Baffle #9 on time
flags.0				9.  if = 1 allow feed correction for zone
maxPID				10. Max PID fan setting
flowScreen			12. Screen # in egg flow
bafAddSec			15. > 2 References add this # of seconds.
flags.1				16. Enable or Disable Temperature Optimization for zone, 0 = enable, 1 = disable
					17. DGP for processing machine selection (V204)
flockNmbr			18. Flock Number
bafDeviation		19. Baffle deviation (new control)
bafTunnelFans		20. low byte is # Fans on for tunnel mode
bin_inv[4]			22. Feed Bin inventory 5
bin_inv[3]			23. Feed Bin inventory 4
bin_inv[2]			24. Feed Bin inventory 3
monitorType			25. Remote Type (Monitor, Bluetooth, TouchScrn)
bafModeTemp			26. Temperature for tunnel/normal control
bafCtrlMode			27. Divided into 2-bits per baffle to select tunnel vs. normal control
-					28. Baffle #9 open dgp/ch
-					29. Baffle #9 close dgp/ch
New assignments for Version V20:
0.
1.
2.
3. Belt on/off dgp/ch
4. Wind Setpt
5. Press Setpt
6. House for Network Variable
7. Max tunnel position
8.
9.
8. Ramp up/down for additional lights.
11.
12.
13.
14.
15.
16.
9. DGP for processing machine selection in V204, split-off in V20
10. Bird Scale Array Start (96 bits)
11. Bird Scales continued
12. Bird Scales continued
13. Bird Scales continued
14. Bird Scales continued
15. Bird Scales continued
16. Schedule for blinking lights
17. Schedule for Touch screen
18. Hatch month/day
19. Hatch year
28.
20. Number of Tiers

END OF STORE_IT VARIABLES */

	unsigned char num_consum;				/* # of consumptions */
	unsigned char max_h2o_rate;				// Max water usage per minute per meter
	CONSUM_TYPE consum_hist[MAX_CONSUM];
	BROOD_TYPE brood_table[30];				// 999 day schedule
	unsigned char midnightOnHr;				// Addiitonal light settings
	unsigned char midnightOnMin;
	unsigned char midnightOffHr;
	unsigned char midnightOffMin;
	unsigned char start_month;				// Housing month (day 1)
	unsigned char start_day;				// Housing day
	unsigned short start_year;				// Housing year
	unsigned char start_hour;				// Hout shedule changes
	unsigned char sched_index;				// Current schedule segment
	unsigned short day_of_sched;			// Day on schedule
	unsigned short timeOff;					// calculated lights off time
	unsigned short timeOn;					// calculated lights on time
	ADR_TYPE static_gauge;					/* address for static pressure */
	unsigned char static_value;				/* value of static pressure gauge */
	unsigned char static_low;				// Static limits
	unsigned char static_hi;
	unsigned short staticDiv;				// Static Pressure divisor
	unsigned char rs232_adr;				/* address for house monitor */
	unsigned short rs232_bdr;				/* baud rate for house monitor */
	ZONE_HOUR_HIST zoneHourHist[60];		// History for hour
	unsigned short zone_analog;				/* N/U */
	char d_file_name[D_FILE_NAME_LEN+1];	/* data file name */
	char d_file_start[D_FILE_EXT_LEN+1];	/* date file extension */
	unsigned char d_file_freq;				/* number of minutes between writes */
	unsigned char d_file_cnt;				/* counter til write */
	unsigned char d_file_flag;				/* always set to 0 */
	OPT_TYPE optimizer;						// For a specific input type, not actually doing anything
	S_TIMER_TYPE second[MAX_SEC_TIMERS];	// Second timers for zone
	unsigned char zone_priority;			// Priority for processing this zone
	unsigned char zone_countdown;			// Counter for priority
	unsigned char zone_regular;				// Zone to process next
	unsigned char min_baff_posn;			// Old min position (replaced by individual values)
	unsigned char min_baff_temp;			// Min F to open
	unsigned char min_read;					// Min reading for Primary alarm delay
	unsigned char read_change;				// # readings before change
	unsigned short abs_hi_zone;				// Absolute zone high temp
	unsigned short abs_hi_group;			// Absolute group high temp
	unsigned short cbelt_secs;				// # seconds in slowdown
	unsigned short cbelt_intvl;
	unsigned char vs_zone_ctrl;				// Force zone to use zone average
	unsigned char rs232_passes;				// Minute counter for RS232 monitors
	unsigned char manure_dgp;				// DGP assignment for manure control
	unsigned short bin_inv[MAX_BINS];		// Saved bin inventory
	unsigned char offset_schedno;			// Selected offset schedule
	unsigned short vs_avg;					// Average speed of all VS fans
	unsigned short vs_total;				// Total speed of all VS fans
	unsigned short vs_num;					// # VS fans
	unsigned char prim_data_read;			// Allow reading when group disabled
	unsigned char report_set;
									// bit 0 - Temp report
									// bit 1 - Consumption report
									// bit 2 - Egg report
									// bit 3 - Summary report
									// bit 4 - Egglift report
									// bit 5 - Archive sensors
	unsigned char prim_alm_reads;			// # reads before alarm
	unsigned char set_alarm_on;				// Alarms enabled/disabled
	unsigned long bin_weight[MAX_BINS];		// storage for bin weights during feedings
	unsigned short tier_feed;				// tier feed at the start of a feeding
	unsigned short total_feed;				// feed totals at the start of a feeding
	unsigned short augerOffWeight;			// Weight to shut off fill auger
	unsigned short flockNmbr;				// Flock number for reports
	unsigned short flowScreen;				// Which egg flow screen (no longer used)
	unsigned short maxPID;					// Max PID fan setting
	unsigned short maxBafPos;				// Max baffle position
	unsigned short bafAddSec;				// > 2 References add this # of seconds.
	unsigned char bafDeviationLow;			// Baffle deviation (new control)
	unsigned char bafDeviationHi;
	unsigned char bafModeTempLow;			// Low temperature for tunnel/normal control
	unsigned char bafModeTempHi;			// Hi temperature for tunnel/normal control
	unsigned long bafCtrlMode;				// Divided into 2-bits per baffle to select tunnel vs. normal control
	unsigned char bafTunnelFans;			// # Fans on for tunnel mode
	unsigned char monitorType;				// Remote Type (Monitor, Bluetooth, TouchScrn)
	unsigned short flags;			// Bit 0: Feed Correction
									// Bit 1: Temp Optimization
									// Bit 2: Feed Sum
									// Bit 3: Ramp schedule
									// Bit 4: Lbs/grams switch
	PID_DATA_TYPE *pidDataPtr;		// Pointer to PID data structure
	MANUAL_DATA_TYPE manualData;			// Manual data structure
	BIRD_SCALE_TYPE birdScale[16];			// Bird scale data
	unsigned char writeSchedules;			// Not used
	BAFFLE_ACTION baffleMode;				// Tunnel/Turbo
	tBaffleTable *baffleTable;				// Table for baffles
} ZONE_TYPE;