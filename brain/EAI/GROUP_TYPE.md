typedef struct {
	unsigned char enabled;				/* disabled group takes no output action */
	char name[GROUP_NAME_LEN+1];
	unsigned char num_sens;				/* temp sensors configured */
	INP_TYPE sensor[MAX_SENS];			/* primary sensors (temperature) */
	INP_TYPE gas_in[2];					/* secondary sensors, ie. water, scale */
	unsigned char num_outs;				/* heat/cool outputs */
	OUT_TYPE output[MAX_OUT];			// Output data
	unsigned char num_inps;				/* inputs, alarms, counters */
	INP_TYPE auxinp[MAX_AUX];			// General input data
	unsigned char num_cnts;				/* alarm inputs */
	unsigned short offsetSchd;			// schedule during which to use offsets
	unsigned short offsetHeat;			// seperate offsets for heat and cool
	unsigned short offsetCool;
	unsigned short currentOffsetHeat;	// current values to offset
	unsigned short currentOffsetCool;
	unsigned short avg_val;				/* group average temp */
	unsigned short sum_read;			/* temp storage for average calculation */
	unsigned char tot_read;
	unsigned short grp_hi;				/* extreme readings in group */
	unsigned short grp_lo;
	unsigned char fans_on;				/* number of fans active */
	unsigned char fans_strt;			// # fans started this hour
	unsigned char heat_on;				// # heaters on
	unsigned char heat_strt;			// # heaters started this hour
	unsigned char state;				/* alarm condition */
	unsigned short g_hi_lim;			// group alarm limits
	unsigned short g_lo_lim;
	unsigned char alarm_on;				// bit 1=zone alarm, 2=system alarm
	unsigned short g_egg_lo;			// low egg count
	unsigned short g_egg_avg;			// avg egg count
	unsigned short g_egg_hi;			// high egg count
	unsigned long g_egg_total;			// total today
	unsigned long g_egg_tld;			// total yest
	unsigned short g_egg_rpm;			// egg rate
	unsigned char g_min_rpm;			// min rate
	unsigned long t_doz;				// week total
	PID_DATA_TYPE *pidDataPtr;			// Pointer to PID structure
} GROUP_TYPE;