// Data structure for brooder schedule
//*****************************************************************************
typedef struct {
	unsigned short start_day;		// start day for segment
	unsigned short end_day;			// end day
	unsigned short heat_target;		// min/heat target
	unsigned short cool_target;		// cool target
	unsigned char on_hour;			// lights on/off time
	unsigned char on_min;
	unsigned char off_hour;
	unsigned char off_min;
	unsigned short sec_sens[2];		// secondary targets
	unsigned char level[5];			// Light level (default 100)
} BROOD_TYPE;
