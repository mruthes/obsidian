

**PROCESS.C** is the **heart** of the Integra automation system - it's the **main processing engine** that continuously manages all farm operations. Here's a detailed breakdown:

graph TD

    A[process Main Entry] --> B[CheckClock]

    B --> C{tock = TRUE?}

    C -->|Yes| D[Secs Every Second Tasks]

    C -->|No| E[Idle Background Processing]

    E --> F[External Sensors Processing]

    E --> G[Zone Processing Loop]

    G --> H[Group Processing]

    H --> I[Sensor Reading]

    H --> J[Average Calculations]

    H --> K[Output Control]

    H --> L[Input Processing]

    D --> M{tic_min?}

    M -->|Yes| N[by_minute Minute Tasks]

    M -->|No| O{tic_hour?}

    O -->|Yes| P[hourly Hour Tasks]

    O -->|No| Q{tic_day?}

    Q -->|Yes| R[daily Day Tasks]

    F --> S[Temperature Sensors]

    F --> T[Weather Station]

    F --> U[Static Pressure]

    I --> V[Analog Sensors]

    I --> W[Digital Counters]

    I --> X[Egg Counters]

    K --> Y[Fan Control]

    K --> Z[Heater Control]

    K --> AA[Baffle Control]

    style A fill:#e1f5fe

    style D fill:#f3e5f5

    style E fill:#e8f5e8

    style G fill:#fff3e0


## 🔧 **Core Functions:**

### **⏰ Main Processing Loop (`process()`)**
- **Timing Control**: Uses `CheckClock()` to manage second-by-second execution
- **Hierarchical Processing**: Calls different functions based on time intervals:
  - **Every second**: `Secs()` - timer updates, counters
  - **Every minute**: `by_minute()` - averages, alarms, control decisions  
  - **Every hour**: `hourly()` - historical data, reports
  - **Every day**: `daily()` - daily summaries, file rotations

### **🔄 Background Processing (`Idle()`)**
Uses **state machines** to process zones efficiently:

**Zone Processing States:**
- **ZoneExtTemp**: Process external sensors (outside temp, weather)
- 
- **ZoneGroup**: Process all groups within a zone
- **ZoneBaffle**: Process zone-level controls and cleanup
- **ZoneControl**: Final zone calculations

**Group Processing States:**
- **GrpSensor**: Read all sensors in the group
- **GrpAverage**: Calculate group temperature averages
- **GrpOutput**: Control outputs (fans, heaters, etc.)
- **GrpInput**: Process digital inputs and counters
- **GrpCleanup**: Final group calculations

## 🌡️ **Sensor Management:**

### **Analog Sensors (`ProcessSensors()`)**
- **Temperature sensors** (thermistors, RTDs)
- **Static pressure** (for ventilation control)
- **Humidity sensors**
- **Feed monitoring** sensors
- **Light sensors** for verification
- **CT sensors** (current transformers)

### **Digital Counters (`ProcessInputs()`)**
- **Egg counters** (primary production tracking)
- **Feed counters** 
- **Water usage** counters
- **Fan RPM** monitoring
- **Belt speed** counters

## 🎛️ **Output Control:**

### **Environmental Control (`ProcessOutputs()`)**
- **Fan staging** based on temperature differentials
- **Heater control** for cold weather
- **Baffle/inlet control** for ventilation
- **Lighting control** with schedules
- **Alarm outputs** for critical conditions

### **Advanced Control Features:**
- **PID fan control** for precise temperature management
- **Tunnel/Turbo mode switching** for different ventilation strategies
- **Static pressure** compensation
- **Wind speed adjustment** for outdoor conditions

## 🏭 **Zone Management:**

### **Multi-Zone Processing:**
- Supports **up to 48 zones** (poultry houses)
- Each zone has **multiple groups** (sections of the house)
- **Hierarchical averaging**: sensor → group → zone → complex
- **Independent control** per zone with global coordination

### **Specialized Processing:**
- **Quality control** sensors (egg washing, cooling)
- **Utility systems** (boilers, generators)
- **Weather station** integration
- **Bird scale** monitoring

## 📊 **Data Management:**

### **Real-time Processing:**
- **Continuous sensor reading** every processing cycle
- **Rate calculations** for counters (eggs/minute, feed consumption)
- **Alarm detection** and notification
- **Historical data** collection

### **Production Tracking:**
- **Egg flow monitoring** across multiple graders
- **Production efficiency** calculations
- **Belt timing** and coordination
- **Daily/hourly totals** and averages

## 🔧 **Advanced Features:**

### **Touch Screen Integration:**
- **IntraTouch** support for house-level control
- **Manual overrides** and setpoint adjustments
- **Maintenance tracking** and scheduling
- **Employee access control**

### **Network Communication:**
- **Viewer integration** for remote monitoring
- **Dashboard** data streaming  
- **Alarm redistribution** to multiple systems
- **File synchronization** between systems

**PROCESS.C** is essentially a **real-time operating system** for poultry farms, managing thousands of I/O points, complex control algorithms, and ensuring optimal environmental conditions for maximum production efficiency while maintaining animal welfare standards.