---
title: Module's Selected Major Components
---

## Module's Selected Major Components

The following sections are the selected major components of the drive train for the scavenger tank.


**Mircocontroller**
1. ESP32-S3-WROOM-1-N4

   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/5cd892b2-e550-4b43-9d77-9b9d1a0d370f" />

 * $5.06/each
 * [link to product](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639)


    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | Inexpensive                               | might have to create a circuit with it to make it easier to program |
    | Has many pins to use                      | Limited Pins                                       |
    | Has EDA/CAD file                          |                                                                  |
   
**Rationale:** The ESP32 is a familiar component used in a previous class. The only issue is creating the PCB for all the components since i cant use the dev board with it. But it will be capable of working with the other microcontrollers to send signals back and forth.

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

3. IFX9201SGAUMA1
   <img width="200" height="200" alt="image" src="https://github.com/user-attachments/assets/dbdd3c4c-24d0-4077-8f2b-f57478658c6c" />



   * $3.55/each
   * [link to product](https://www.digikey.com/en/products/detail/infineon-technologies/IFX9201SGAUMA1/5415542?s=N4IgTCBcDaIJIDEAaBOMAGAjAZQOIEEBVAWX0xAF0BfIA)

    | Pros                                      | Cons                                                             |
    | ----------------------------------------- | ---------------------------------------------------------------- |
    | familiar component                        | Only for DC brushed motors                                       |
    | Protection Features                       |  12V                                                        |
    | Has EDA/CAD file                          | Motor output current setting pin might be difficult to set up    |   






   
**Rationale:** IFX9201SGAUMA1 is a part we used in class. Already familiar with setting up and coding. Know it uses I^2C while the other components are unsure if they use I^2C.

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

**Rationale:** The GEARMOTOR 220 RPM 12V MICR METAL seems to be the best fit for the drive train system. Not only does it use 12V, but it has a balance of torque and speed that should be able to haul the weight of the rest of the scavenger tank.


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
