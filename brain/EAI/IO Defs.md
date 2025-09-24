

para a logica, existe uma estrutura para configurar a logica das saidas (Data structure for output logic), segue abaixo a estrutura:


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

### **Structure: `LOGIC_TYPE`**

The `LOGIC_TYPE` structure defines the configuration and control parameters for output logic in the system.  
It contains identifiers, primary/secondary logic references, timing mechanisms, special functions, and external dependencies.

#### **Fields**

- **`char name[LOGIC_NAME_LEN+1]`**  
    String that stores the logic name (with space for the null terminator).
    
- **`unsigned char prim_sens`**  
    Primary logic selector (main input or condition).
    
- **`unsigned char sec_sens[2]`**  
    Secondary logic selectors (two additional inputs or conditions).
    
- **`unsigned char cyc_timer`**  
    Cycle timer value for periodic operations.
    
- **`unsigned char clock`**  
    Time schedule reference (links to time-based triggers).
    
- **`unsigned char xtra_1`**  
    Reserved for special function codes.
    
- **`unsigned char outLogic`**  
    Defines the output logic behavior.
    
- **`unsigned char out_sen`**  
    Associated external sensor for output conditions.
    
- **`unsigned char xtra_2`**  
    Extra storage (reserved, purpose defined later in implementation).
    
- **`unsigned char xtra_3`**  
    Extra storage (reserved, purpose defined later in implementation).
    
- **`unsigned char out_dat`**  
    Output data type (digital, analog, or mixed).
    
    - When set to **3**, it indicates the use of the `IDEF` table.
        
- **`unsigned char table[MAX_TABLE]`**  
    Table used for IDEF logic mapping (active when `out_dat == 3`).





