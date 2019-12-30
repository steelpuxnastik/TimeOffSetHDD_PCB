# TimeOffSetHDD_PCB
Old notebook Acer 5560G has problem in using second HDD, which connects to the slot of the DVD drive. PC will not start, if HDD connected, but PC and second HDD works fine if HDD will be conected after Power-On Self-Test (POST). More info here: https://habr.com/ru/post/215839/ (here is described an example of creating a relay-based power delay circuit).

But the above scheme has several disadvantages, some of which inability to reboot (notebook will not pass POST) and some bulkiness and design complexity. In addition, the author of the above article himself says that he offers all this as an example, confirming that the method itself is working and that simpler implementations are possible.

That is why I tried to make much smaller and more controlled scheme on Attiny13A and transistor IRF7204, which controls power supply through the transistor (transistor opens in 10 sec after applying power to the MCU). 
Parts used:
1. ATTINY13A-SU
2. IRF7204 Transistor
3. VT 10uF 25V 4 * 5.4 105C SMD Capacitor x1 
4. 5.6K 0603 5% SMD Resistor x2
5. XH-03A connector (to connect input 5V, output 5V and ground from HDD caddy)
6. Pin header 2x3pin 2.54mm for SPI MCU programming (the arrangement of contacts is duplicated from Arduino ICSP)

Result:
![Иллюстрация к проекту](https://github.com/steelpuxnastik/TimeOffSetHDD_PCB/blob/master/images/1.JPG)
