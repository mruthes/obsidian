
```mermaid
graph TD

    A[Program Start] --> B[InitCVIRTE]

    B --> C[CheckForDuplicateAppInstance]

    C --> D[MallocBuffers]

    D --> E[init Hardware & Variables]

    E --> F[CheckOptionFlags Registry]

    F --> G[LoadPanel aei.uir]

    G --> H[restore Load Database]

    H --> I[Main Control Loop]

    I --> J[ScrollScreen Every 30s]

    I --> K[process Main Processing]

    I --> L[ProcessSystemEvents Every 10 Cycles]

    I --> M[HeaderUpdate]

    K --> N[CheckClock]

    N --> O{tock?}

    O -->|Yes| P[Secs Every Second]

    O -->|No| Q[Idle Background]

    P --> R{tic_min?}

    R -->|Yes| S[by_minute]

    R -->|No| T{tic_hour?}

    T -->|Yes| U[hourly]

    T -->|No| V{tic_day?}

    V -->|Yes| W[daily]

    M --> X[DispOutsideData]

    M --> Y[DispTime]

    M --> Z[DispAlarmBanner]

    AA[MenuButton Events] --> BB[CNFG_BUTTON]

    AA --> CC[ENV_BUTTON]  

    AA --> DD[EGG_BUTTON]

    AA --> EE[PROCESS_BUTTON]

    AA --> FF[ALARM_BUTTON]

    AA --> GG[HELP_BUTTON]

    BB --> HH[Load menusecr.uir]

    CC --> II[Load overenv1.uir]

    DD --> JJ[Load overegg1.uir]

    EE --> KK[Load ProcRm.uir]

    I --> LL{done?}

    LL -->|No| I

    LL -->|Yes| MM[Cleanup & Exit]

    classDef initClass fill:#e8f5e8,stroke:#4caf50,stroke-width:2px

    classDef loopClass fill:#e3f2fd,stroke:#2196f3,stroke-width:2px

    classDef uiClass fill:#fff3e0,stroke:#ff9800,stroke-width:2px

    classDef processClass fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px

    class A,B,C,D,E,F,G,H initClass

    class I,J,K,L,M,LL loopClass

    class AA,BB,CC,DD,EE,FF,GG,HH,II,JJ,KK uiClass

    class N,O,P,Q,R,S,T,U,V,W processClass
```



# 📋 **MAIN.C - Detailed Architecture Explanation**

**MAIN.C** is the **master control center** and **application orchestrator** for the AEI Integra poultry automation system. It serves as the **entry point, UI manager, and main coordinator** for all system operations.

## 🚀 **System Architecture Overview:**

### **Phase 1: System Initialization (Lines 705-843)**

**1️⃣ Runtime Initialization:**
```c
InitCVIRTE(0, argv, 0)           // Initialize CVI runtime environment
CheckForDuplicateAppInstance()    // Prevent multiple instances
```

**2️⃣ Memory & Hardware Setup:**
```c
MallocBuffers()                  // Allocate heap memory for data structures
init()                          // Initialize hardware, comm ports, variables
CheckOptionFlags()              // Read registry settings and user preferences
```

**3️⃣ User Interface Setup:**
```c
LoadPanel(0, "aei.uir", PANEL)  // Load main GUI panel
DisplayPanel(mainPanel)          // Show main interface
restore()                       // Load historical database from disk
```

### **Phase 2: Main Control Loop (Lines 890-916)**

``` mermaid
flowchart TD

    A[Main Control Loop] --> B{Screen Auto-Scroll?}

    B -->|Yes & Timer Expired| C[ScrollScreen]

    B -->|No| D[process]

    C --> D

    D --> E[CheckClock]

    E --> F{tock = TRUE?}

    F -->|Yes| G[Secs Timer Updates]

    F -->|No| H[Idle Background Processing]

    G --> I[Zone Processing]

    H --> I

    I --> J[Sensor Reading]

    I --> K[Environmental Control]

    I --> L[Egg Flow Management]

    L --> M[Count Cycles]

    M --> N{count >= 10?}

    N -->|Yes| O[ProcessSystemEvents]

    N -->|No| P[HeaderUpdate]

    O --> Q[do_auto_hist]

    Q --> P

    P --> R[DispOutsideData]

    P --> S[DispTime]

    P --> T[DispAlarmBanner]

    T --> U{done = TRUE?}

    U -->|No| A

    U -->|Yes| V[System Shutdown]

    classDef loopClass fill:#e3f2fd,stroke:#2196f3,stroke-width:2px

    classDef processClass fill:#f3e5f5,stroke:#9c27b0,stroke-width:2px

    classDef displayClass fill:#fff3e0,stroke:#ff9800,stroke-width:2px

    class A,B,C,M,N,U,V loopClass

    class D,E,F,G,H,I,J,K,L processClass

    class O,P,Q,R,S,T displayClass
```
### **Phase 3: User Interface Management**

**🎛️ Menu Button System (`MenuButton()` Function):**

Each button loads different operational screens:

| Button | Panel Loaded | Function |
|--------|--------------|----------|
| **CNFG** | `menusecr.uir` | System configuration |
| **ENV** | `overenv1.uir` / `overenv8.uir` | Environmental overview |
| **FW** | `overf&w.uir` / `overf&w8.uir` | Feed & Water overview |
| **EGG** | `overegg1.uir` / `overegg8.uir` | Egg production overview |
| **PROCESS** | `ProcRm.uir` | Grader processing status |
| **ALARM** | `alarm.uir` | Alarm management |
| **TREND** | `trend.uir` | Historical trends |
| **UTILITY** | Via DLL | Utility systems |
| **QUALITY** | Via DLL | Quality control |

### **Phase 4: Real-Time Display Management**

**📊 Header Information System (`HeaderUpdate()`):**

``` mermaid
flowchart LR

    A["HeaderUpdate"] --> B["Site Name Display"] & C["Outside Data Rotation"] & D["Date/Time Display"] & E["Alarm Banner"]

    C --> F["Weather Station Data"] & G["Outside Temperature"] & H["Humidity"] & I["Wind Speed/Direction"] & J["Rain Rate"] & K["Barometric Pressure"]

    F --> L["15 Different Parameters"]

    L --> M["5-Second Rotation"]

    E --> N["Highest Priority Alarm"] & O["Scrolling Alarm Messages"]

  

     A:::headerClass

     B:::headerClass

     C:::weatherClass

     D:::headerClass

     E:::alarmClass

     F:::weatherClass

     G:::weatherClass

     H:::weatherClass

     I:::weatherClass

     J:::weatherClass

     K:::weatherClass

     L:::weatherClass

     M:::weatherClass

     N:::alarmClass

     O:::alarmClass

    classDef headerClass fill:#e8f5fe,stroke:#03a9f4,stroke-width:2px

    classDef weatherClass fill:#e8f5e8,stroke:#4caf50,stroke-width:2px

    classDef alarmClass fill:#ffebee,stroke:#f44336,stroke-width:2px

```

**🌡️ Outside Data Display (`DispOutsideData()`):**

Cycles through **15+ different weather parameters** every 5 seconds:
- **Outside Temperature** (with negative handling)
- **Humidity** (relative humidity %)  
- **Rain Rate** (inches/hour)
- **Wind Speed & Direction** (MPH + compass direction)
- **Barometric Pressure** (inches Hg)
- **Heat Index & Wind Chill** (calculated values)
- **Dewpoint & Sunrise/Sunset** times

### **Phase 5: System Integration Points**

**🔗 Key Integration Calls:**

```c
// Main processing engine (from PROCESS.C)
process()                    // Manages all zones, sensors, outputs

// Time-based functions (from TIMELY.C)  
Secs()                      // Every second: timers, counters
by_minute()                 // Every minute: averages, control
hourly()                    // Every hour: history, reports  
daily()                     // Every day: summaries, cleanup

// Serial communication (from LOOP.C)
SendSerialData()            // DGP communication management

// Historical data (from other modules)
do_auto_hist()             // Automatic history file generation
```

### **Phase 6: Specialized System Variants**

**🎯 Conditional Compilation Features:**

| Build Flag | Purpose | Key Features |
|------------|---------|--------------|
| **VIEW** | Remote Viewer | Read-only monitoring system |
| **NETVAR** | Network Variable | NetVar protocol support |
| **DASHBOARD** | Web Dashboard | Real-time web interface |
| **V204/V205** | Version Variants | Support for 2-5 graders |
| **PROCVIEW** | Processing Only | Limited to processing screen |

### **Phase 7: Advanced Features**

**🎛️ Screen Auto-Rotation (`ScrollScreen()`):**
- **30-second timer** automatically switches between screens
- **Environmental Overview** → **Processing** → **Egg Flow**
- Configurable via `logic_defs[10]->xtra_2` bit setting

**⌨️ Keyboard Shortcuts (`MainProcess()`):**
- **F1**: Help system
- **F8**: Message notepad  
- **F10**: Processing screen
- **Ctrl+C**: Stop current printing
- **Ctrl+P**: Print active panel

**🔧 Registry-Based Configuration:**
```c
CheckOptionFlags()          // Reads from HKEY_CURRENT_USER
// Enables/disables: Quality, Utility, Reports, Debug modes
// Configures: Printer settings, Resolution adjustment
// Controls: Menu visibility, Keypad support
```

## 📈 **Performance Characteristics:**

- **Main Loop**: Executes continuously (~10ms cycles)
- **UI Updates**: Every 10th cycle to balance performance
- **Display Refresh**: Headers updated selectively vs. full refresh
- **Memory Management**: Static allocation for real-time performance
- **Event Processing**: Non-blocking UI event handling

**MAIN.C** essentially functions as the **operating system kernel** for the poultry automation system, coordinating between **hardware I/O**, **user interface**, **data processing**, and **communication systems** while maintaining **real-time performance** critical for industrial automation.



```mermaid
graph TD
    A[Ideia Inicial] --> B(Pesquisa)
    B --> C{É viável?}
    C -->|Sim| D[Implementar]
    C -->|Não| E[Reavaliar]
    D --> F[Testar]
    F --> G[Concluído]
```
