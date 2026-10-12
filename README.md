# Smart Security System with Laser using Logic Gates

This is a laser tripwire alarm that I built using only logic gate ICs, with no microcontroller and no code. A laser beam points at a light sensor across a doorway or a cabinet. When someone breaks the beam, the alarm turns on, and it stays on until it is reset by hand.

I made it for my Digital Logic Design course to see how basic gates can be used to build something that actually works in real life.

## How it works

The laser module shines on an LDR sensor module placed on the opposite side. While the light reaches the sensor, nothing happens. When the beam is blocked, the sensor output goes HIGH and the logic circuit turns on a buzzer and a red LED.

The alarm has a latch made from an OR gate and an AND gate with feedback. This means the alarm keeps ringing even after the beam is restored, so an intruder can't stop it just by walking away. The only way to stop it is the reset button.

There is also an arm switch. When it is off, the circuit ignores the beam, so someone authorized can walk through without setting off the alarm.

In short, the logic is:
- Trigger = beam blocked AND arm switch ON
- Alarm = (Trigger OR previous alarm) AND NOT reset

## Components

- IC 7404 (NOT gate)
- IC 7408 (AND gate)
- IC 7432 (OR gate)
- Laser diode module (5V)
- LDR sensor module
- BC547 NPN transistor
- Active buzzer (5V)
- Red LED with a 330 ohm resistor
- 1k ohm resistor for the transistor base
- Two 10k ohm pull-down resistors
- Slide switch (arm) and push button (reset)
- Breadboard, jumper wires and a 5V supply

## Connections

All three ICs get +5V on pin 14 and GND on pin 7. Unused gate inputs are tied to GND.

- LDR module DO pin goes to 7408 pin 1
- Arm switch goes to 7408 pin 2, with a 10k resistor to GND
- 7408 pin 3 (trigger) goes to 7432 pin 1
- 7408 pin 6 (alarm output) goes back to 7432 pin 2 for the latch
- 7432 pin 3 goes to 7408 pin 4
- Reset button goes to 7404 pin 1, with a 10k resistor to GND
- 7404 pin 2 goes to 7408 pin 5
- 7408 pin 6 also drives the LED through the 330 ohm resistor
- 7408 pin 6 also goes through the 1k resistor to the base of the BC547, which switches the buzzer on

The buzzer goes between +5V and the transistor collector, and the emitter goes to GND. I used a transistor because a TTL gate cannot supply enough current to a buzzer on its own.

If your sensor module gives the opposite output (LOW when the beam is blocked), put one extra NOT gate between its DO pin and 7408 pin 1.

## Truth table

I is the sensor output (1 means beam blocked), ARM is the arm switch, RESET is the reset button.

| I | ARM | RESET | Previous alarm | New alarm | State |
| - | --- | ----- | -------------- | --------- | ----- |
| 0 | 1 | 0 | 0 | 0 | Normal |
| 1 | 0 | 0 | 0 | 0 | Disarmed |
| 1 | 1 | 0 | 0 | 1 | Alarm on |
| 0 | 1 | 0 | 1 | 1 | Latched |
| 0 | 1 | 1 | 1 | 0 | Reset |

## Setting it up

Build the circuit on a breadboard, then fix the laser and the sensor so the beam falls straight on the LDR. Turn the small potentiometer on the sensor module until its output stays LOW with the beam on it and goes HIGH as soon as you block the beam with your hand. Avoid direct sunlight on the sensor, and never point the laser at anyone's eyes.

## Limitations

- One beam only covers one line, so someone can step over or around it
- The laser and sensor must be aligned well, and bumping them can cause false alarms
- Strong light can affect the sensor
- The reset button is on the same board, so anyone who finds it can stop the alarm

## Ideas for later

- Use mirrors to make a zig-zag of beams across the area
- Add a 555 timer so the buzzer stops by itself after some time
- Use a key switch instead of a push button for reset
- Send an SMS or a phone notification when the alarm goes off

## Report

The full project report is included in this repository as Smart_Security_System_Laser_Report.docx.

## Author

Muneeb Ur Rehman
