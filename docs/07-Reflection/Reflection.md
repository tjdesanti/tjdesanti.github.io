---
title: Reflection
---
## Reflection

### Review of Module's Success

**Requirements Accomplished:**

The following module requirements were accomplished during the course of this project:

- **Motor Direction Control:** Successfully implemented bidirectional motor control that 
responds to multiple input sources. The motor was able to change directions based on 
pressing the debugging button on the board, as well as receiving information through 
message compliance from a teammate's subsystem via the communication protocol.

- **Pinout Corrections:** Successfully identified and corrected pinout issues on several 
components. Pins on the ESP32 that were originally connected to functions they were not 
capable of running were rerouted, including the micro USB port, the enable/boot buttons, 
and the SPI pins for the motor controller. This required cutting traces and soldering 
jumper wires directly to the board to fix the connections.

**Requirements Struggled With:**

- **ESP32 SPI Pinout:** Ensuring the correct SPI-capable pins were used on the ESP32 for 
Communication with the motor driver was one of the most challenging aspects of the project. 
This was not caught until testing, which required physical trace cuts and wire modifications 
to resolve.

- **Soldering Fixes:** Soldering wires onto existing traces and physical component leads to 
correct routing errors proved to be tedious and difficult, reinforcing the importance of 
thorough pinout verification before PCB fabrication.

---

### Microcontroller/Module Startup Tips

The following is a list of tips and advice based on lessons learned during the startup and 
debugging phases of this project. These are things that would have saved significant time 
and effort if followed from the beginning:

1. **Always double-check datasheets and pinouts before routing your PCB.** Many issues 
encountered in this project stemmed from pins being assigned to functions the ESP32 was 
not capable of running. Verify every pin's capabilities against the datasheet before 
finalizing your schematic.

2. **Add extra test points and through-holes connected to each component.** Having dedicated 
test points at every major component port makes debugging significantly easier. If a trace 
needs to be cut, a nearby through-hole allows for a quick jumper wire fix instead of 
soldering directly onto a component lead.

3. **Always use pull-up resistors on buttons.** Using capacitors to pull buttons down 
created issues that required an external solder breadboard to fix. Pull-up resistors are 
the cleaner and more reliable solution and should be the default approach.

4. **Separate the power connection of ribbon cable connectors from the main power trace 
using a jumper.** This reduces the risk of accidentally shorting the board when connecting 
or disconnecting ribbon cables and gives you more control over power distribution during 
testing.

5. **Invest time early in understanding power conversion.** A better understanding of 
Voltage regulation and current limiting from the start would have allowed more power to 
be safely distributed to teammates' boards without the risk of overloading components 
on your own board. Consider adding a current regulator in the initial design rather than 
as an afterthought.

## Lessons Learned

1. Double-check pinouts for each microcontroller, since not every variant of the ESP32 has the same pinouts.
2. resisotrs --> power, capacitors --> ground.

## Recommendations for Future Students

1. If you are not ahead, you are behind.
2. Start working on the physical project once PCBs are ordered.
3. Spend more time and always ask for help.
