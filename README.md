# Design
Depicts vaious design aspects associates with this project.

Table of contents
=================

<!--ts-->
   * [Case](#Case)
      * [Case BOM](#CaseBOM)
   * [Keyboard Module](#Keyboard)
      * [Keyboard BOM](#KeyboardBOM)
   * [NumPad Module](#NumPad)
      * [NumPad BOM](#NumPadBOM)
   * [Joystick/D-PAD Module](#Joystick)
      * [Joystick BOM](#JoystickBOM)
<!--te-->


Case
====

![Rendered Calculator](KeyShot_Renders/Renders/Case/Case_2.46.png)
|:--:| 
| *Rendering of Casing for Calculator.* |

## AutoCAD Drawings
![Case Outline AutoCAD](Fusion360_MODEL/Images/Case_Outline.png)
|:--:| 
| *2D Outline of Case in AutoCAD* |

Mocked up in AutoCAD to determine layout, espcially in regards to PCB orientation and configuration. Exported as a dxf, and utilized to build enclosure in Fusion360 from this initial sketching. All dimensions for the Fusion360 model determined from this intial sketch.

![Outline Comparision](Fusion360_MODEL/Images/Outline_Compare.png)
|:--:| 
| *Comparision between the initial 2D AutoCAD sketch, and the finalized model in Fusion360.* |

## Fusion360 Models
### Case Bottom
![Case Bottom](Fusion360_MODEL/Images/Bottom_Case_2.png)
|:--:|
| *Bottom Case for Calculator.* |

![Case Bottom with PCB](Fusion360_MODEL/Images/PCB_Housing.png)
|:--:|
| *Bottom Case for Calculator with PCB's included.* |

### Case Top
![Case Top](Fusion360_MODEL/Images/Top_Case.png)
|:--:|
| *Top Case for Calculator.* |

### Case All
![Case Top](Fusion360_MODEL/Images/Full.png)
|:--:|
| *Full Case Design for Calculator.* |


Case BOM
========

All casing produced and manufactured by PCBWay's Stereolithography (SLA) Printing Service[^1]. This SLA service allows for precise and accurate feature resolution. For this project, the material of choice was the standard white material (UTR 8360), providing great dimensinoal accuracy while also being durable. Pricing for each part of the enclosure is shown in the table below.

| Component | Cost |
| --- | --- |
| Display Top | $4.98 |
| Display Bottom | $8.46 |
| Case Top | $16.68 |
| Case Bottom | $17.42 |


Keyboard Module
===============

The keyboard module is responsible for alphabetical input from the user. This is handled by a full qwerty keyboard, in addition to other keys aswell.

![Keybaord Module Schmatic](KiCAD_PCB/Images/Keyboard_Module_Schematic.png)
|:--:|
| *Keyboard Module KiCAD Schmeatic.* |

![Keybaord Module PCB](KiCAD_PCB/Images/Keyboard_Module_PCB.png)
|:--:|
| *Keyboard Module PCB Routing and Component Placement.* |






## Single Board Computer
| Name | Cost | Link |
| --- | --- | --- |
| Raspberry Pi Zero | $(FREE) | https://www.raspberrypi.com/products/raspberry-pi-zero/ |

## Screen 
| Screen | Cost | Link |
| --- | --- | --- |
| 4.0" TFT Display | $54.95 | https://www.adafruit.com/product/3932 |

## Keyboard 

###### RP2040 Board
| Component | Cost | Link |
| --- | --- | --- |
| RP2040 Stamp | $12.00 | https://www.tindie.com/products/arturo182/rp2040-stamp/ |

###### USB Micro B
| Component | Cost | Link |
| --- | --- | --- |
| 10103594-0001LF | $0.77 | https://www.digikey.com/en/products/detail/amphenol-cs-fci/10103594-0001LF/2350351 |

###### Switches & Caps
| Component | Cost | Link |
| --- | --- | --- |
| KSA1M331LFT SPST-NO | $0.91 | https://www.digikey.com/en/products/detail/c-k/KSA1M331LFT/1003897 |
| Tactile Cap Oval (BLACK) | $0.39 | https://www.digikey.com/en/products/detail/c-k/BTNK0390/559405 |
| D-Pad Button(Black) | $3.40 | https://www.ebay.com/itm/182260383344?hash=item2a6f90be70:g:KzUAAOSwFV9XxCYp |

###### Joystick
| Component | Cost | Link |
| --- | --- | --- |
| Mini Analog Thumbstick | $2.50 | https://www.adafruit.com/product/2765?gclid=CjwKCAjw6raYBhB7EiwABge5Kk9TZn8ilWmRzUdasBMNh74FXIZQBJh3K6sDLgbHT71pnA3cYCJrNRoC1E8QAvD_BwE |

# Example Hardware Code

## Joystick

```
"""
The joystick module is an Analog device, comprising of two 10kΩ potentiometers. 
X & Y will be determined via two analog reading pins on the microcontroller. 
Pins 34/32/31 are designated ADC pins on RP2040. 
"""
import board
from analogio import AnalogIn
import usb_hid
from adafruit_hid.mouse import Mouse

mouse = Mouse(usb_hid.devices)
xAxis = AnalogIn(board.A1)
yAxis = AnalogIn(board.A0)

in_min,in_max,out_min,out_max = (0, 65000, -5, 5)
filter_joystick_deadzone = lambda x: int((x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min) if abs(x - 32768) > 500 else 0


while True:
    x_offset = filter_joystick_deadzone(xAxis.value) * -1 #Invert axis
    y_offset = filter_joystick_deadzone(yAxis.value)
    mouse.move(x_offset, y_offset, 0)
```

[^1]: https://www.pcbway.com/rapid-prototyping/3D-Printing/3D-Printing-SLA.html
