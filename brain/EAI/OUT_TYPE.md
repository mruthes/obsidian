// Data structure for individual outputs
//*****************************************************************************
typedef struct {
	char name[OUT_NAME_LEN+1];
	ADR_TYPE address;			// DGP address
	unsigned char flags;		// bit 0: 0=HEAT, 1=COOL
								// bit 1: 0=on time, 1=off time for lights
								// bit 2: 1=allow output to use additional light time even if offsets used
	unsigned short on_temp;		// temp activate thresholds
	unsigned short off_temp;
	unsigned short on_sec_sen[2];	// On value for secondary sensors
	unsigned short off_sec_sen[2];	// Off value for secondary sensors
	short outside_on;			// On temp when using outside sensor
	short outside_off;			// Off temp when using outside sensor
	unsigned short minutesOn;	// minutes output is on each day
	unsigned short power;		// Measure the energy usage
	unsigned char func;			// logic type
	unsigned char state;		// on or off
	unsigned char baf_num;		// which baffle to relate to
	unsigned char cyc_timr;		// cycle timer #
	unsigned char timer;		// time schedule #
	unsigned char a_index;		// index into antic table for output
} OUT_TYPE;