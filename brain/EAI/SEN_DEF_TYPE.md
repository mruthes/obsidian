
//*****************************************************************************
// S E N _ D E F _ T Y P E
//
// Last modified: 02/17/14 KPL
// Author:
//
// Parameters for input definitions (meaning depends on an_dig_sw value)
//*****************************************************************************
typedef struct {
	unsigned short slope[MAX_TABLE];/* storage for idef table (p1 = p2 = 255) */
	short p1;						/* PARAM A */
	short p2;						/* PARAM B,
										# TIMES To ALARM for an_dig_sw == 3 */
	short p3;						/* PARAM C, decimal place for analog,
										REVERSE TIMECODE for an_dig_sw == 3,
										Minimum (units) for an_dig_sw >= 5 */
	char units[SEN_UNITS_LEN+1];	/* DISPLAY UNITS */
	char lo_msg[SEN_MSG_LEN+1];		/* LO ALARM MESSAGE,
										CLOSED ALARM MESSAGE for an_dig_sw == 3 */
	char hi_msg[SEN_MSG_LEN+1];		/* HI ALARM MESSAGE,
										OPEN ALARM MESSAGE for an_dig_sw == 3 */
	unsigned char priorty;			/* ALARM PRIORITY */
	char name[SEN_NAME_LEN+1];		/* name assigned to this definition */
	unsigned char min_cnt;			/* MIN CNTS */
	unsigned char an_dig_sw;		/* DEVICE TYPE */
	unsigned char f;				/* set to 0 */
} SEN_DEF_TYPE;