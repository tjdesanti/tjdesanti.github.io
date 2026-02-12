---
title: Module's Selected Major Components
---

## Module's Selected Major Components

The following sections are the selected major components necessary for  .....

>**For each of the following sections, use <ins>one of the two styles</ins> given near the end. *REMOVE THIS NOTE***

### Power Management

(**remove this note/placeholder**: this is where your 3.3 volt switching regulator, any other needed power regulator, and power source {if applicable} **THAT WERE SELECTED**)

For more details, review the ["Appendix - Component Selection Process - Power Mangement"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/Appendix/01-Componet-Selection/Component-Selection-Process/#power-management) selection.

### Sensor

(**remove this note/placeholder**: if applicable, this is where your  **SELECTED** sensor is shown. Otherwise, remove this section.)

For more details, review the ["Appendix - Component Selection Process - Sensor"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/Appendix/01-Componet-Selection/Component-Selection-Process/#sensor) selection.

### Actuator

(**remove this note/placeholder**: if applicable, this is where your **Selected** the actuator items go, which includes both the driver and motor. Otherwise, remove this section.)

For more details, review the ["Appendix - Component Selection Process - Actuator"](https://embedded-systems-design.github.io/EGR314DataSheetTemplate/Appendix/01-Componet-Selection/Component-Selection-Process/#actuator) selection.

-----------
> Remove the following before submitting! Use them to present the selected components

### Style 1

> This is the example found in the assignment, uses more html

*Table 1: Example component selection*



**Motor Driver**

1. IC MTR DRV BIPLR 2.5-5.5V TSOT23

   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/259d9ca6-6d01-4b70-9611-f4e3c30ed2c5" />


    * $0.83/each
    * [link to product](https://www.digikey.com/en/products/detail/monolithic-power-systems-inc/MP6513LGJ-Z/7361426)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Inexpensive                               | Max Voltage supply is 5.5V                                       |
    | Has thermal, current, and short circuit projection  | Low Max Current                                        |
    | Has EDA/CAD file                          | More for smaller applications like cameras and toys              |

2. IC BRUSHED MOTOR DRVR 8TSSOP

   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/63ffbf3b-72db-4a41-874d-3367f02622ed" />

   * $0.83/each
   * [link to product](https://www.digikey.com/en/products/detail/toshiba-semiconductor-and-storage/TB67H450AFNG-EL/15995284)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Inexpensive                               | Only for DC brushed motors                                       |
    | High Voltage & Current intake             | Sensitive thermal conditions                                     |
    | Has EDA/CAD file                          | Motor output current setting pin might be difficult to set up    |                    

3. 12-V, 1.76-A BRUSHED DC MOTOR DR
   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/606ccb0f-e37b-4324-836b-c4fcbfb50ea0" />

   * $0.69/each
   * [link to product](https://www.digikey.com/en/products/detail/texas-instruments/DRV8210DRLR/15286847)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | cheapest option                           | Only for DC brushed motors                                       |
    | Protection Features                       | under 12V                                                        |
    | Has EDA/CAD file                          | Motor output current setting pin might be difficult to set up    |   






   
**Rationale:** The IC BRUSHED MOTOR DRVR 8TSSOP seems like the best option. It can take in high voltage and current for the motor. It is compatible 
with both stepper and non-stepper DC motors. It seems like a versatile driver compared to the others that can't supply more than 12V.

**DC Motors**

1. STANDARD MOTOR 6600 RPM 12V

   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/427120ef-6669-4ab0-9350-a32cda85099a" />

   * $2.75/each
   * [link to product](https://www.digikey.com/en/products/detail/sparkfun-electronics/11696/6163657)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | cheap                                     | Low torque, gear ratio may be needed                             |
    | can intake 12V                            | need custom footprint                                            |
    | 2-wire setup                              | Little data on the datasheet                                     |

2. GEARMOTOR 35 RPM 12V MICRO METAL

<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/5d55a898-66d3-45e2-bcfa-d092ef51ee8b" />

   * $37.95
   * [link to product](https://www.digikey.com/en/products/detail/pololu/3046/10450048)
    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | uses 12V                                  | High power                                                       |
    | high torque                               | need custom footprint                                            |
    | simple 2-wire connection                  | Might be too slow for the drive train                            |

3. GEARMOTOR 220 RPM 12V MICR METAL

<img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/b3d44045-bbae-4623-bf88-854a9ccc35e4" />


   * $26.45
   * [link to product](https://www.digikey.com/en/products/detail/pololu/3042/10450044)
    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | uses 12V                                  |  may not be enough torque or fast enough                         |
    | steady speed                               | need custom footprint                                           |
    | simple 2-wire connection                  | may not be suitable for a drive train                            |
