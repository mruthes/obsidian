

typedef struct {
	char name[INP_NAME_LEN+1];
	ADR_TYPE address;			// DGP address
	unsigned char dev_type;		// analog/switch,counter, etc
	unsigned short hi_lim;		// alarm thresholds
	unsigned short lo_lim;
	unsigned short value;		// last read converted to correct units
	unsigned char state;		// alarm condition
	unsigned char a_count;		// alarm count
	unsigned char alarm_on;		// bit 1=zone alarm, 2=system alarm
	unsigned char baffle;		// TRUE if tied to a baffle
	unsigned char tc;			// time schedule
	unsigned char store;		// TRUE if save info set to "Y"
} INP_TYPE;