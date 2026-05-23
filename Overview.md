# Electric Dice Project

## 1. Project Overview

The Electric Dice is an Arduino-Nano based system that simulates the roll of a standard six sided die. When the user presses the
push button, the microcontroller generates a random number between 1 to 6 and displays the corresponding dot pattern on seven LEDs
arranged on a 3x3 die face-grid. Main objective of the task is to understand the circuit and then design a Printed Circuit Board
for the same in KiCad software.

### Components at a glance
- Microcontroller : Arduino_Nano_Every
- Supply Voltage : +5V DC (via USB)
- Input Device : 1 x SW1 tactile puch button  + 10 k ohm resistor
- Output devices : 7 x LEDs, each limited by a 220 ohm resistor

## 2. Microcontroller Pin Map

Pin number of Nano -> Power direction -> Connected to

- 5V -> Power Out -> SW1 one terminal
- GND -> Power ref -> All LED cathodes
- D2 -> OUTPUT -> Top left LED anode
- D3 -> OUTPUT -> Top right LED anode
- D4 -> OUTPUT -> Middle right LED anode
- D5 -> OUTPUT -> Middle LED anode
- D6 -> OUTPUT -> Middle left LED anode
- D7 -> OUTPUT -> Bottom right LED anode
- D8 -> OUTPUT -> Bottom left LED anode
- D9 -> INPUT -> SW1 output (via 10k resistor)

## 3. Circuit Explanation

### Push Button Circuit

The components involved are SW1 push button, 10k pull down resistor and the digital pin D9 configured as INPUT pin.
One terminal of the button is connected to the +5V source and the other simultaneously connects pinD9 and the pull down resistor.
Further the resistor connects to the ground. When the button is not pressed, there is no current in resistor and D9 pin reads LOW.
However, when button is pressed current starts flowing in the resistor and to the ground. This switches the pin to HIGH and 
triggers a new roll of die in the system.

### LED Drive Circuit

The components involved are 7 LEDs (forward voltage 2V), 7 220 ohm current limiting resistors and Arduino digital pins from D2-D8
configured as OUTPUT pins. The firmware / code decides which LED needs to be ON in case of displaying which number. After deciding
that the firmware writes a particular pin (D2-D8) as HIGH (5V). The current flows from the pin -> resistor (220 ohm) -> LED anode
-> LED Cathode -> GND. When the firmware writes LOW, the LED goes dark as no current flows through it.


## 4. Software Explanation

- IDLE - All LEDs remain in their previous state. (all OFF at startup)
- DETECT - D2 reads high, button press detected
- RANDOMISE - random(1,7) generates an integer from 1 to 6. 
- DISPLAY - All LED pins are first set low. Then only the pins corresponding to the die face pattern are set HIGH (governed by set of if-else statements)
- RETURN - the loop repeats from step 1
