

**1. Overall Purpose:**

This C file, `init.c`, is part of a larger system (likely an industrial controller or data acquisition system, given the references to Modbus, serial ports, DGP boards, and shared memory) designed to:
*   Initialize the system's core components.
*   Read and parse a configuration file (`config.ini` or `config.csv`) to set up communication ports (serial, TCP), define data mappings, and configure operational parameters like watchdog timers and interrupt handlers.
*   Set up data structures for Modbus communication (both master and potentially slave roles).
*   Manage non-volatile storage (`nvdata.bin`) for settings like serial port parameters and passwords.
*   Interface with external systems like a Weather Station via shared memory.
*   Prepare the system for runtime operation based on the configuration.

**2. Key Components and Functions:**

*   **Includes:** Standard C (`ansi_c.h`, `windows.h`, `stdio.h` implicitly via functions like `printf`, `utility.h`), National Instruments CVI specific headers (`userint.h`, `asynctmr.h`), potentially VISA for serial communication (`visa.h` if `VISA` is defined), and project-specific headers (`const.h`, `types.h`, `defines.h`, `ICL.h`, `init.h`, `ioread.h`, `tcp.h`, `touchdll.h`).
*   **Global Variables (via `#include "vars.h"`):**
    *   `section`: Tracks the current section being parsed in the config file.
    *   `Section[]`: A dictionary mapping section header strings (like `[Model]`, `[Ports]`) to enum values.
    *   `lock`, `hMemMapFile`, `commMemory`: For shared memory handling.
    *   `mcu[]`: Array for Modbus events related to MCU (Motor Control Unit?) outputs, likely for safety timing.
    *   `LOCK_NAME`, `W_MEMORY_NAME`, `W_MEMORY_SIZE`: Constants for shared memory.
    *   `wMemory`: Pointer to shared memory for Weather Station data.
    *   `neTrig[]`: Array for manually triggered Modbus events.
    *   `camera`: Flag indicating if an Egg Camera is configured.
*   **`GetWeatherData()`:**
    *   Reads data from the `wMemory` shared memory block (presumably populated by the `Davis.exe` weather station program).
    *   Writes this data to specific Modbus registers (starting at `WEATHER_ADDR`).
    *   If `wMemory` isn't available, writes default/invalid values.
*   **`InterruptTimer()`:**
    *   Callback function for a periodic (1.0 second) asynchronous timer.
    *   Increments `interruptTimerValue`.
*   **`WatchdogTimer()`:**
    *   Callback function for a periodic (0.1 second) asynchronous timer.
    *   Increments `watchdogTimerValue`, `modbusDelayTimer`, and `mcuTimerValue`.
*   **`CreateOrOpenMapFile()`:**
    *   Helper function using Windows API (`CreateFileMapping`, `OpenFileMapping`, `MapViewOfFile`) to create or access a named shared memory region.
*   **`InitSystem()`:**
    *   **Core Initialization Function.**
    *   Prints the program version.
    *   Initializes `pcPort`, `house`, `schedule`.
    *   **Non-Volatile Storage (`nvStorage`):**
        *   Allocates memory for `nvStorage`.
        *   Attempts to load settings from `nvdata.bin`. If it fails or the data is invalid (`validCode != 0x5A5A`), it initializes with default values (e.g., baud rates, data bits, parity, stop bits, passwords).
        *   Ensures password strings are null-terminated.
    *   Initializes `dgpArray` (DGP board configurations) with default values.
    *   Initializes `brickPort` (communication port structures) with default values.
    *   Initializes `watchDogAddr` and `ignore1Addr`/`ignore2Addr` arrays.
    *   Calls `RegAlloc` to allocate Modbus register banks for DI, DO, AI, AO.
    *   Gets and prints timer resolution, then sleeps for 3 seconds.
    *   Initializes various timer values (`interruptTimerValue`, `watchdogTimerValue`, etc.).
    *   Creates the `interruptTimerID` (1.0 sec) and `watchdogTimerID` (0.1 sec) using `NewAsyncTimer`.
    *   Initializes `zoneButton` array with default zone numbers ("01", "02", ...).
    *   Reads `zonenmbr.txt` to potentially override default zone numbers.
    *   Writes the program's version to a specific Modbus address (`TOUCH_VERSION`).
    *   Checks for `Davis.exe` (weather station program). If found, launches it and attempts to open the shared memory (`W_MEMORY_NAME`) for weather data.
    *   Sets `goodData` to 0, indicating the system waits for valid initial data before allowing certain operations.
*   **`OpenConfigFile()`:**
    *   Tries to open a configuration file in the following order: `d:\config.ini`, `d:\config.csv`, `config.ini`, `config.csv`.
    *   Returns a `FILE*` pointer to the first file found, or `NULL` if none are found.
*   **`ReadModbusAddr()`:**
    *   Parses a string representation of a Modbus address (e.g., "30001", "40010").
    *   Splits it into `Type` (coil=0, discrete input=1, input register=3, holding register=4) and `Index` (offset within the type).
*   **`ReadConfigFile()`:**
    *   **Main Configuration Parsing Function.**
    *   **Two-Pass Parsing:**
        *   **Pass 1 (Counting):** Reads the config file to count the number of Modbus records (serial and TCP) to determine how much memory to allocate for `netSes` and `nsTcp` arrays.
        *   Calls `AllocateRecords` and `AllocateTCPRecords` based on the counts.
        *   **Pass 2 (Setup):** Rewinds the file and reads it again to perform the actual configuration.
    *   **Parsing Logic:**
        *   Skips lines starting with `!` (comments).
        *   Identifies sections based on `[SectionName]` headers using the `Section[]` dictionary.
        *   Processes lines within sections:
            *   **`[Model]`, `[IOScale]`:** Currently empty/ignored in the code shown.
            *   **`[Ports]`:**
                *   Parses comma-separated values representing port configurations.
                *   Handles different protocols: `MODBUS_MASTER`, `MODBUS_TCP`, `MODBUS_CAMERA`, `MODBUS_FIRE`, `ASCII_PLC`.
                *   **Special Ports:**
                    *   `port=255`: Often indicates PC communication (shared memory server or TCP client).
                    *   `port=0`: Sets up a shared memory server.
                *   **Serial Port Setup (VISA or RS232):**
                    *   Opens and configures serial ports (`COMx`) with specified or default parameters (baud, data bits, parity, stop bits). Uses VISA if `VISA` is defined.
                    *   Stores parameters in `nvStorage`.
                *   **DGP Serial Port Setup:** Configures parameters for DGP boards connected via serial.
                *   **Modbus Master Setup:**
                    *   Allocates memory for `tNetSession` (`netSes[0]`, `netSes[1]`) for serial Modbus masters.
                    *   Creates `tNetEvent` structures (`NE_Event...Create`) for cyclic, timed, or triggered read/write operations between Modbus devices and the local register banks (DI, DO, AI, AO).
                    *   Manages `mcu[]` array for specific Modbus write events (likely for safety timers on outputs).
                    *   Manages `neTrig[]` array for manually triggered events.
                *   **Modbus TCP/Camera/Fire Setup:**
                    *   Allocates memory for `tNetSession` (`nsTcp[]`) for TCP-based connections.
                    *   Calls `SetupTCPConnection` to establish TCP links.
                    *   Creates `tNetEvent` structures similar to Modbus Master for TCP devices.
                    *   Sets `camera` flag if `MODBUS_CAMERA` is found.
            *   **`[DGP]`:**
                *   Parses comma-separated values for DGP board configurations (address, channel, offset, multiplier, divisor, Modbus mapping addresses for inputs, relays, outputs, counters, rates).
                *   Stores these in the `dgpArray`.
                *   Updates `maxAI` if AI channels > 8 are found.
            *   **`[Interrupt]`:**
                *   Reads the Modbus address for the interrupt signal and stores it in `interruptAddr`.
            *   **`[Watchdog]`:**
                *   Reads one or more Modbus addresses that, when written to, reset the watchdog timer. Stores them in `watchDogAddr`.
            *   **`[Ignore]`, `[Ignore1]`, `[Ignore2]`:**
                *   Reads Modbus addresses (up to 256) that should *not* cause the watchdog to trigger on communication failure. Stores flags in `ignore1Addr` and `ignore2Addr`.
    *   **Error Handling:** Prints messages to the console if config file parsing fails, memory allocation fails, or VISA operations fail. Exits if the config file is not found.

**3. Key Concepts and Technologies:**

*   **Modbus:** Central communication protocol for interacting with external devices (sensors, actuators, PLCs). Supports RTU (serial) and TCP variants.
*   **Configuration File (`config.ini`):** Drives the system's behavior, defining ports, mappings, and operational parameters.
*   **Shared Memory:** Used for inter-process communication (IPC), specifically with the Weather Station (`Davis.exe`) and potentially for PC communication.
*   **Asynchronous Timers:** Used for periodic tasks like the interrupt timer and watchdog timer.
*   **DGP Boards:** Specific hardware devices (likely data acquisition or control modules) that the system communicates with, configured via the `[DGP]` section.
*   **VISA vs. RS232:** Conditional compilation (`#ifdef VISA`) allows using either the National Instruments VISA library or older RS232 functions for serial communication.
*   **Memory Management:** Uses `malloc`, `calloc` for dynamic allocation of structures like `nvStorage`, `netSes`, `nsTcp`, and Modbus event lists.

**4. Summary:**

The `init.c` file is responsible for bootstrapping the system. It loads persistent settings, sets up communication infrastructure (serial, TCP, shared memory), parses the configuration file to define how the system interacts with external devices (DGP boards, Modbus slaves), initializes data structures for Modbus registers, starts necessary background timers (interrupt, watchdog), and prepares the system for its main operational loop. It heavily relies on configuration data and integrates with various hardware/software components via Modbus and shared memory.