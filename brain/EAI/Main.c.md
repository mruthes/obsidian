**MAIN.C** is the **entry point** and **main control module** for the **AEI Integra** poultry automation system:

**🎯 Primary Functions:**

**🚀 System Initialization:**
- Sets up **CVI runtime** environment  
- **Prevents duplicate instances** from running
- **Allocates memory buffers** for data structures
- **Initializes hardware** (comm ports, devices)
- **Loads configuration** from registry/database

**🖥️ User Interface Management:**
- **Loads main GUI panel** (`aei.uir`)
- **Manages button navigation** (Config, Environment, Egg Flow, Processing, etc.)
- **Handles screen rotation** between different views every 30 seconds
- **Processes keyboard shortcuts** (F1=Help, F8=Messages, F10=Processing)

**⏰ Main Control Loop:**
- **Continuous processing** of system operations
- **Updates headers/displays** (time, date, outside temperature, alarms)  
- **Processes system events** periodically
- **Manages timers** for screen rotation and data updates

**🔧 Feature Management:**
- **Option flags** from registry (Quality, Utility, Reports, Debug modes)
- **Multi-version support** (V204, V205, Viewer, Dashboard variants)
- **Network capabilities** (TCP, UDP, shared memory)
- **Print configuration** and panel printing

**📊 Integration Points:**
- Calls **`process()`** - main data processing loop
- Calls **`restore()`** - loads historical data  
- Manages **child panels** for different operational screens
- **Alarm banner** management and display

**🏭 Industrial Context:**
This is the **main orchestrator** for a complete poultry farm automation system managing environment, egg collection, processing, and monitoring across multiple zones/houses.