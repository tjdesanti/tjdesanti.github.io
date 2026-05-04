---
title: Hardware V2.0
---
## Hardware V2.0

The following section outlines the improvements that would be made to the PCB design in a 
second hardware revision. These changes are based on lessons learned during the assembly, 
testing, and debugging phases of the project.

### What Would Be Improved

**1. ESP32 Pinout Verification**
In the next revision, more time would be spent thoroughly reviewing the ESP32 pinout 
before routing the PCB. Specifically, identifying the exact pins eligible for SPI 
communication with the motor driver and the correct pinout for the micro USB port. 
This would eliminate the need to cut traces and run physical jumper wires, resulting 
in a much cleaner and more reliable PCB.

**2. Button Pull-Up Resistors**
The current design uses capacitors to pull down the buttons, which required the use of 
an external solder breadboard to compensate. In Version 2.0, pull-up resistors would be 
used instead, allowing the buttons to function correctly directly on the PCB without any 
additional external components or workarounds.

**3. Better Utilization of Under-Traces**
The trace routing in the current design could be significantly improved by making better 
use of the back copper layer. In the next revision, under-traces would be used more 
strategically to organize routing, reduce clutter on the top layer, and produce a more 
compact and professional PCB layout.

**4. Current Regulator for Power Distribution**
A current regulator would be added in Version 2.0 to better manage power distribution 
across the system. This would allow power to be safely shared with teammates' subsystems 
without the risk of feeding too much current into sensitive components. Additionally, 
incorporating a regulated power supply would make it feasible to power the system from 
a battery, improving the mobility and portability of the overall project.

**5. Repositioned and Mirrored Ribbon Cable Ports**
The two 8-pin ribbon cable ports in the current design are positioned in a way that 
places the pins underneath the PCB, making connections difficult and potentially 
unreliable. In Version 2.0, these ports would be spread apart and mirrored so that 
all pins are accessible from the top side of the board, improving ease of assembly 
and connection reliability.

**6. Additional Test Points and Through-Holes**
Extra test points and through-holes would be added at each component port in the next 
revision. If a trace ever needs to be cut during debugging, these test points would 
allow for quick jumper wire connections without having to solder wires directly onto 
physical component leads, significantly improving the debuggability of the board.
