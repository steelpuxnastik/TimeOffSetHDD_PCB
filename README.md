# TimeOffSetHDD_PCB
Old notebook Acer 5560G has problem in using second HDD, which connects to the slot of the DVD drive. PC will not start, if HDD connected, but PC and second HDD works fine if HDD will be conected after Power-On Self-Test (POST). More info here: https://habr.com/ru/post/215839/ (here is described an example of creating a relay-based power delay circuit).

But the above scheme has several disadvantages, some of which inability to reboot (notebook will not pass POST) and some bulkiness and design complexity. In addition, the author of the above article himself says that he offers all this as an example, confirming that the method itself is working and that simpler implementations are possible.

Field notes: as I can see, the problem lies in the fact that the BIOS, in principle, is not provided for booting with two hard drives, since there is only one HDD0 and when notebook detects second HDD - it cannot determine what exactly HDD is primary and what HDD is second because BIOS knows only 1 HDD it can operate, so after that it has problem with POST, because it do not knows how to solve that problem.

That is why I tried to make much smaller and more controlled scheme on Attiny13A and transistor IRF7204, which controls power supply through the transistor (transistor opens in 10 sec after applying power to the MCU and turning on the second HDD). 
Parts used:
1. ATTINY13A-SU
2. IRF7204 Transistor
3. VT 10uF 25V 4 * 5.4 105C SMD Capacitor x1 
4. 5.6K 0603 5% SMD Resistor x2
5. XH-03A connector (to connect input 5V, output 5V and ground from HDD caddy)
6. Pin header 2x3pin 2.54mm for SPI MCU programming (the arrangement of contacts is duplicated from Arduino ICSP)

Result:
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/1.JPG)
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/2.JPG)
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/3.JPG)
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/4.JPG)

Program can be flashed through Arduino IDE as Arduino SPI programmer.
You need to download this archive ( https://github.com/orlv/at13/archive/master.zip ) and unzip it into your default folder with scatches, usually it's "\Documents\Arduino\hardware\" (in result it must be something like this "C:\Users\Администратор\Documents\Arduino\hardware\attiny13\avr\cores\core13").
Then flash your Arduino with scatch from Arduino IDE Samples - ArduinoISP
Afrer that connect ATtiny13 to Arduino similar to the ICSP block on Arduino, but last RESET pin on device connect to pin 10 on Arduino.
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/5.JPG)
Choose the board Attiny13 in menu Tools and I would recommend to choose "Frequency: 128kHz", because we do not need more - MCU literally will do nothing more, but it will help tremendously reduce its consumption of power.
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/6.JPG)
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/7.JPG)

Then choose "Arduino as ISP" in "Tools - Programmer" (or "Arduino as ISP (ATmega32U4)" if you have Leonardo like me, simply on "Arduino as ISP" my Leonardo is not flashing MCU) press "Burn bootloader". This will flash to MCU settings that we selected earlier.
After that you can flash the programm.

![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/IMG_20200120_005445.jpg)
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/IMG_20200120_005646.jpg)
Device works well only if notebook starts from total turn off (best variants is to turn off by button after set "restart" in Windows, because notebook will not start after that, so you need to turn it off by button and then turn it on and then notebook will start well with second HDD). Delay time was shorted to 2 sec - it's minimum delay time needed to start.
