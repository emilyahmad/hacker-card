# October 17: Idea & Project Setup

Some inspo:
![hackaday](https://hackaday.com/wp-content/uploads/2019/12/linuxcard_feat.jpg?w=600&h=450)
![purdue](https://cdn.vigue.me/assets/28b00078-e255-44a4-9118-7c86210aac37.png)
![embossed?](https://cdn-blog.adafruit.com/uploads/2021/12/Untitled-45.png)
![simple](https://user-images.githubusercontent.com/22257322/212734680-56e4b8f4-9d41-4294-b3af-e1afb594dcd1.png)

I'll loosely follow the [hack club jam](https://jams.hackclub.com/jam/hacker-card) for this.

Components:
+ capacitor, resistor

+ 2V LED, search for the illumination color you want [here](https://jlcpcb.com/parts/2nd/Optoelectronics/LED_Indication_Discrete_71)

Some colors need 3.3V to turn on (not enough from an NFC reading)
The NFC chip's max output voltage is 3.3V, but increasing the output current decreases the NFC communication range ([Datasheet](https://www.nxp.com/docs/en/data-sheet/NT3H2111_2211.pdf))

+ NFC chip ([NT3H2111W0FHKH](https://jlcpcb.com/partdetail/NxpSemicon-NT3H2111W0FHKH/C710403)) - [download](https://www.snapeda.com/parts/NT3H2111W0FHKH/NXP%20USA/view-part/?ref=search&t=NT3H2111W0FHKH&ab_test_case=b)

Parts aren't already part of Kicad library like they are on EasyEDA, so I'll grab them off SnapEDA.
+ When I download the footprint/symbol for a part does it have to match the manufacturer?
Nope just has to have the same physical properties.

Because I'm limited to 1-2 LEDs turning on (low current), I want to try doing a sequence with the LEDs, so I can have many but they can be turned off. Maybe a line of LEDs, once the card gets scanned, they turn on/off (chase pattern).

Claude recommends:
Low-power MCU (GPIO drives each LED through its own resistor)	~0.5-3 mA while awake	Most flexible: any pattern, and it can respond to the NFC chip’s field-detect pin. But the MCU’s own draw comes out of the same budget.
CMOS 555 timer + 4017 decade counter	~100-200 µA total	No code. The timer clocks the counter, which lights outputs 0-9 in sequence. Use a CMOS 555 (TLC555, LMC555, ICM7555), not a classic bipolar NE555, which draws several mA.
Self-flashing LEDs	Varies	Usually designed for 3-5V and draw more than a plain LED. Not ideal here.

Board measurements
Width: 88.9mm
Height: 50.8mm
Radius: 4mm

I want the design to be similar to this:
![mockup](/images/mockup.png)
The front, with my information and LinkedIn qr code, and the back red LEDs oriented to be a heart and the NFC chip