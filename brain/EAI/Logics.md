static LOGIC_TYPE *logic_defs;


typedef struct {

    char name[LOGIC_NAME_LEN+1];
    unsigned char prim_sens;        // Primary Logic
    unsigned char sec_sens[2];      // Second Logic 1 & 2
    unsigned char cyc_timer;        // Cycle Timer
    unsigned char clock;            // Time Schedule
    unsigned char xtra_1;           // Special Function
    unsigned char outLogic;         // Outside Logic
    unsigned char out_sen;          // Outside sensor
    unsigned char xtra_2;           // see below (extra storage)
    unsigned char xtra_3;           // see below
    unsigned char out_dat;          // Dig-An
    unsigned char table[MAX_TABLE]; // IDEF table (out_dat == 3)

} LOGIC_TYPE;

Output definitions - House Avg Temp
Primary logic = 4 - Antic/ZoneAvg


a media da zona é calculada em
void ZoneCleanUp(void)
...
    if (nmbrAvg > 0)

        zone[z]->zon_avg = SignedRoundDivision (sumAvg, nmbrAvg);

    else

        zone[z]->zon_avg = 0;

main->process->idle->ZoneCleanup
tem uma maquina de stado no idle, no estado case ZoneBaffle:

o sumAvg é acumlado em void ProcessAvg (unsigned char g), chamada na maquina de estado do idle no estado case GrpAverage:




aqu sao as definicoes das logicas



#define MAX_GRP				18
#define MAX_OUT				16
#define MAX_AUX				24
#define MAX_SENS			    16
#define MAX_CONSUM			23
#define MAX_CYC_TIMERS		8
#define MAX_SEC_TIMERS		16
#define MAX_TIME_SCHED		16
#define MAX_VS_PID_FANS		12
#define MAX_1S_PID_FANS		32
#define NMBR_IDEF			36
#define NMBR_LDEF			30
#define MAX_BINS			5
#define MAX_BAFFLES			16
#define MAX_TABLE			220







// Data structure for output logic
//*****************************************************************************
typedef struct {
	char name[LOGIC_NAME_LEN+1];
	unsigned char prim_sens;		// Primary Logic
	unsigned char sec_sens[2];		// Second Logic 1 & 2
	unsigned char cyc_timer;		// Cycle Timer
	unsigned char clock;			// Time Schedule
	unsigned char xtra_1;			// Special Function
	unsigned char outLogic;			// Outside Logic
	unsigned char out_sen;			// Outside sensor
	unsigned char xtra_2;			// see below (extra storage)
	unsigned char xtra_3;			// see below
	unsigned char out_dat;			// Dig-An
	unsigned char table[MAX_TABLE];	// IDEF table (out_dat == 3)
} LOGIC_TYPE;


	GLOBAL SEN_DEF_TYPE input_defs[NMBR_IDEF];	// Input definitions
	GLOBAL LOGIC_TYPE logic_defs[NMBR_LDEF];	// Output definitions





ProcessOutput - > processa e calcula os valores onTemp, offTemp baseado na logica e alguns offsets



typedef enum: char {noAct, onAct, offAct, initAct} ACTION;					// output action

/*****************************************************************************
 * O u t p u t A c t i o n ( )
 *
 * Last modified: 11/27/06 KPL
 * Author:
 *
 * This routine decides if a fan or other output should be turned on, off, or
 * if no action should be taken, based on the the following arguments:
 * on: Temperture, ammonia level, etc, to turn on the output
 * off: Temperature, ammonia level, etc, to turn off the output
 * value: Input temperature, ammonia level, etc.
 * logic: Logic used to decide output based on how values are handled
 *****************************************************************************/
ACTION OutputAction (int onTemp, int offTemp, int value, unsigned char logic)



nesta parte do codigo em processOutput que escreve na dgp
//linha 678
if (prim == offAct)
							if (!TestState (oPtr->state)) {
								if (adr_non0 (oPtr->address))
									(void)port0_clrbit (oPtr->address.dgp, oPtr->address.channel);
								oPtr->state = OFF_STATE;
							}






void InitReservedOutputNames (void)

{

    strcpy (logic_defs[22].name, "1-Speed PID Fan ");

    logic_defs[22].out_dat = 1; // default to digital output

    strcpy (logic_defs[23].name, "VS PID Fan      ");

    logic_defs[23].out_dat = 3; // default to analog table output

    strcpy (logic_defs[24].name, "Fan Purging     ");

    strcpy (logic_defs[29].name, "Reserved        ");

}

![[Pasted image 20250912144854.png]]