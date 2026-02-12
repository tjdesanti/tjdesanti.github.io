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


# Hall Effect Sensors

## 1. AS5600-ASOM – ams-OSRAM USA INC.

<img width="200" height="200" alt="AS5600-ASOM" src="https://github.com/user-attachments/assets/d7a010bf-4280-4f01-a04b-e3f10b7f98b4" />

* $3.17/each  
* [Link to Product](https://www.digikey.com/en/products/detail/ams-osram/AS5600-ASOM/4914332)  
* [Datasheet](https://www.digikey.com/html/datasheets/productinfo/1647438/0/0/1/as5600-datasheet.pdf)

| Pros | Cons |
|------|------|
| Have worked with this before | Harder to integrate |
| Multiple output modes | More pins, harder to wire |
| Reliable angular sensing | Most expensive option |

---

## 2. TMAG5273 Low-Power Linear 3D Hall-Effect Sensor – Texas Instruments

<img width="200" height="200" alt="TMAG5273" src="https://github.com/user-attachments/assets/28da5486-a231-4f36-b95a-c55579f82a12" />

* $1.24/each  
* [Link to Product](https://www.mouser.com/ProductDetail/Texas-Instruments/TMAG5273D1QDBVR?q=IKkN%2F947nDf5iy%252BbYuEbTg%3D%3D)  
* [Datasheet](https://www.ti.com/lit/ds/symlink/tmag5273.pdf)

| Pros | Cons |
|------|------|
| Fewer pins (simpler wiring) | Lower magnetic sensitivity |
| Inexpensive | Fewer output modes |
| Integrates well with PIC MCU |  |
| Very low power consumption |  |

---

## 3. TLE5009 – Infineon Technologies

<img width="200" height="200" alt="TLE5009" src="https://github.com/user-attachments/assets/e3df8eee-b0c7-499a-b3e3-261530e28e23" />

* $2.73/each  
* [Link to Product](https://www.mouser.com/ProductDetail/Infineon-Technologies/TLE5009-E20102qs=%252BwNEOWq1JvGhyLjFiYi7Q%3D%3D)  
* [Datasheet](https://www.mouser.com/datasheet/3/70/1Infineon-TLE5009_EXXXX-DataSheet-v01_01-EN.pdf)

| Pros | Cons |
|------|------|
| Operates at 3.3V and 5V | Does not use I2C |
| Low output current | More pins, harder to wire |
|  | More expensive |
|  | No configurable address |

---

### Rationale

**Choice 2 – TMAG5273** is the most suitable option because it requires minimal wiring, interfaces efficiently with the PIC microcontroller, and operates at very low power. In comparison, Choice 1 is more difficult to implement, while Choice 3 does not support I²C communication and involves more complex wiring.
---

# 12V 2A AC-DC Wall Power Supplies

## 1. L6R24-120 – Tri-Mag, LLC

<img width="200" height="200" alt="L6R24-120" src="https://github.com/user-attachments/assets/851bcc5a-d03c-4438-b80b-d4cf19db04fa" />

* $10.38/each  
* [Link to Product](https://www.digikey.com/en/products/detail/tri-mag-llc/L6R24-120/7682639)  
* [Datasheet](https://www.tri-mag.com/wp-content/uploads/2021/05/L6R24-L6R30_Series_2021-02.pdf)

| Pros | Cons |
|------|------|
| Inexpensive | Short cord |
| Low noise output |  |
| Average efficiency |  |

---

## 2. WSU120-2000 – Triad Magnetics

<img width="200" height="200" alt="WSU120-2000" src="https://github.com/user-attachments/assets/969ed2ea-2765-4eba-a302-e7469fd92a16" />

* $15.07/each  
* [Link to Product](https://www.digikey.com/en/products/detail/triad-magnetics/WSU120-2000/3094983)  
* [Datasheet](https://catalog.triadmagnetics.com/Asset/WSU120-2000.pdf)

| Pros | Cons |
|------|------|
| Average efficiency | More expensive |
| Reliable output | Higher noise compared to Choice 1 |

---

## 3. MDS-030AAC12 AB – Delta Electronics

<img width="200" height="200" alt="MDS-030AAC12-AB" src="https://github.com/user-attachments/assets/49f281c1-7b19-4a5e-b7aa-e597b33bff10" />

* $29.26/each  
* [Link to Product](https://www.digikey.com/en/products/detail/delta-electronics/MDS-030AAC12-AB/6150232)  
* [Datasheet](https://psu.deltaww.com/en/products/download/Datasheet/MDS-030AAC05)

| Pros | Cons |
|------|------|
| Medical-grade power supply | Very expensive |
| Reliable and stable output | Large form factor |
| Average efficiency |  |

**Rationale:** Choice 1 is the preferred power supply because it offers low output noise while remaining cost-effective. Although the other options provide similar performance, Choice 2 has higher noise levels, and Choice 3 is unnecessarily specialized for this application, as it is designed for medical-grade equipment.
