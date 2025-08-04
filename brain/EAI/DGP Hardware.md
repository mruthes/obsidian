
The Systems I uses DGP boards (Data Gathering Panel) for interfacing to all the controls and sensors in a house. There are two versions, a full DGP or partial DGP. The partial has no analog input or relay output circuitry and is normally used to support additional counters or scales. Each board is powered from a 12V supply with on-board regulation to 5V. There are several versions of software that can be installed on a DGP board. There are two baud rates 9600 and 19,200 (19.2K). The firmware installed on each board depends on the daughter card to be plugged onto the DGP. All firmware versions support counters on the daughter card. The standard DGP version (SCURATE) supports analog outputs, an RS232 version (RS232) supports a RS232 channel and a relay output version (SCUSWITCH) supports additional relay outputs. Newer versions exist that support both the standard and RS232 versions in the same software (SCU9600 or SCU19200). A special variation of this software was created for breaker support and Weltech scale support (SCU9600R or SCU19200R).

**_

Full DGP

_**

The full DGP board has 8 analog inputs for interfacing to temperature sensors, switch closures, water pressure, or any other analog input. There are different input modules depending on the type of signal to be monitored. Each input consists of three terminals: input, common, and shield. (Reference drawing number D1006, "DGP Including Inputs and Outputs", on page 8.) The input leg receives the signal from a sensor. The common leg is usually power to the sensor. The shield leg is the ground connection. Signals from the sensors are then routed through an amplifier. The amplifier modifies the gain (voltage value) of the signal into the proper range required for the analog to digital, A/D, converter. The A/D converter changes 0 to 5-volt analog signals from the sensors into digital signals with values of 0 to 255. The amplifier voltage range is limited such that the digital values from the A/D converter will be limited to 5 - 215. Most input modules just scale the voltage to the correct range for the A/D converter. The switch module is different as it monitors an open/close state for a relay or other contact closure. When open, the input will read a value of 215 and when closed, a value of 5. Placing a meter on the input will read a voltage over 1V when open and 0V when closed.

Each full DGP has 9 output relays located on the right side of the board. The first eight relays receive an on/off signal from the board’s processor and are used to control the larger 24V relays located in the Relay Control Unit (RCU) or Motor Control Unit (MCU). The DGP output relays are simple relays with an input terminal and both normally open and normally closed contact terminals. An LED indicated On/Off while a small green fuse inserted to the left of each relay protects each of these relays. (Reference drawing number D1004, "DGP Component Designations", on page 7.) These fuses are socket-inserted types for easy replacement. Normally, the input terminal is wired to 24Vac, which is then switched through the relay to control circuits external to the DGP cabinet. The ninth relay is used for the watchdog circuit. An extended loss of communications to the PC or a blown fuse will drop this relay out and allow the house to go into backup. This is accomplished by running the signal from the fail-safe relay through the watchdog relay on each DGP used for environmental control before connection to the watchdog inputs in the RCU and MCU cabinets. This routing causes the house to enter backup when either the fail-safe trips, 24Vac is lost or when any DGP watchdog drops.

A daughter card can be plugged into the DGP for additional functions described below.

**_

Partial DGP

_**

The partial DGP is normally used to support additional counters or RS232 scales and monitors. Normally, a house needs a few DGPs for control and may need 7 - 10 for counting. The partial is less expensive then a full DGP and thus used for all additional boards beyond what is required for control.



**_

DGP Daughter Board

_**

There are 4 different daughter cards that can be installed on a DGP board. Each has a slightly different function, although all have 8 counter inputs on the left side. They are each described below:

**_

Counter Only

_**

The counter only board (Model CNT/0) is just that, no other circuitry is installed beyond the counters. Each counter can count and also supply the counts per minute back to the system (PC). These boards have 8 counting inputs, along their left side. These inputs interface to devices that give a pulse output, such as: egg counters, water meters, and dump scales. A board counts each time it reads a switch closure between an input pin and the ground pin. (Reference drawing number D1046, "Counter Only Board")

**_

Counter/Analog Out

_**

The Counter/Analog Out board (Model CNT/A#) has analog output channels on the right side of the board in addition to the counters on the left side. The # denotes the number (2,4, or 8) of analog outputs available on the expansion board. These outputs are capable of creating a 0 to 10 VDC variable control signal. The outputs are located along the right hand side of the board. (Reference drawing number D1047, "Counter/Analog Board"). Counter inputs on these boards perform the same functions as the Counter Only Board inputs. Outputs from these boards are used to run variable speed fan and egg collection motors.

**_

Counter/RS232

_**

The Counter/RS232 board (Model CNT/RS232) has counters and an RS232 channel for connection to scales or ASCII terminals. (Reference drawing number D1048, "Counter/Serial Port Board").

**_

Counter/Relay Out

_**

This module isn't used frequently. It has 8 relays to provide additional outputs beyond the relays on the full DGP. There is no watchdog for these outputs, so use is limited and not recommended for new installations.




