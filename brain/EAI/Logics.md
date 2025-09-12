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

![[Pasted image 20250912144854.png]]