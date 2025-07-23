# 🔋 Latching Power Switch Circuit with Arduino 

This repository contains two distinct latching switch power on/off circuits created in Tinkercad. The first circuit is a hardware-only implementation using a 555 timer IC. The second circuit utilizes a P-channel MOSFET and an N-channel transistor, controlled by an Arduino Uno, which also interfaces with an ultrasonic sensor for distance measurement and features an automatic power-off function.

## Project 1: 555 Timer Latching Switch
This project is a simple yet effective hardware-based latching switch. A single press of the pushbutton turns the LED on, and another press turns it off. This circuit is ideal for applications where a microcontroller is not required.

Circuit Diagram:
<img width="1706" height="727" alt="Amazing Crift-Jaagub" src="https://github.com/user-attachments/assets/a6202338-38fb-4aa5-85ce-39a233f4cbee" />

# Components
1 x 555 Timer IC

1 x Pushbutton

1 x LED

1 x Capacitor (e.g., 1µF)

Resistors (e.g., 2 x 10kΩ, 1 x 470Ω for the LED)

1 x Power Supply (e.g., 5V)

Breadboard and Jumper Wires

# How It Works

This circuit uses the 555 timer in a bistable multivibrator configuration, which means it has two stable states: on and off.

1- Initial State: When power is first applied, the capacitor connected to the trigger (pin 2) and threshold (pin 6) pins is discharged, holding the output (pin 3) in a low state, and the LED is off.

2- First Press (Turn On): Pressing the pushbutton momentarily grounds the trigger pin (pin 2). This negative pulse on the trigger pin sets the internal flip-flop of the 555 timer, causing the output (pin 3) to go high. This turns on the LED. The output will remain high even after the button is released.

3- Second Press (Turn Off): When the output is high, the capacitor charges through a resistor. Pressing the pushbutton again connects the charged capacitor to the threshold pin (pin 6). When the voltage at the threshold pin exceeds 2/3 of the supply voltage, it resets the internal flip-flop, causing the output to go low and turning off the LED.


## Project 2: MOSFET Latching Switch with Arduino and Ultrasonic Sensor

This project demonstrates a more advanced latching power switch using a P-channel MOSFET and an N-channel transistor. The circuit is controlled by an Arduino Uno, which also measures distance using an HC-SR04 ultrasonic sensor. A key feature of this project is the automatic power-off functionality, where the circuit will turn itself off after 10 seconds of operation.

Circuit Diagram:
<img width="1706" height="727" alt="Ingenious Kup" src="https://github.com/user-attachments/assets/15ba4ddd-4121-45ce-91ae-3382d012245e" />

# Components
1 x Arduino Uno

1 x P-channel MOSFET

1 x N-channel Transistor (BJT or MOSFET)

1 x HC-SR04 Ultrasonic Sensor

1 x LED

1 x Diode

1 x Pushbutton

Resistors (for pull-ups, pull-downs, and current limiting)

Breadboard and Jumper Wires

Power Supply

# How It Works
This circuit cleverly uses a combination of a P-channel MOSFET for high-side switching and an N-channel transistor to control the latching mechanism.

1- Turning On: When the pushbutton is pressed, it pulls the gate of the P-channel MOSFET to the ground, turning it on. This allows power to flow to the Arduino.

2- Latching: As soon as the Arduino powers up, it sets a designated pin to HIGH. This pin is connected to the base/gate of the N-channel transistor, turning it on. The activated N-channel transistor then takes over pulling the P-channel MOSFET's gate to the ground, thus keeping the power latched on even after the pushbutton is released.

3- Ultrasonic Measurement: The Arduino continuously triggers the HC-SR04 ultrasonic sensor and reads the echo to calculate the distance to an object. The measured distance is printed to the Serial Monitor.

4- Auto Power-Off: The Arduino code includes a timer that starts as soon as it powers on. After 10 seconds, the Arduino sets the pin connected to the N-channel transistor to LOW. This turns off the N-channel transistor, which in turn stops pulling the P-channel MOSFET's gate to the ground. A pull-up resistor on the P-channel MOSFET's gate then pulls it high, turning the MOSFET off and cutting power to the entire circuit.

```cpp

const int powerLatch = 4;
const int TRIG_PIN = 9;
const int ECHO_PIN = 10;

void setup() {
  Serial.begin(9600);
  pinMode(powerLatch, OUTPUT);   
  digitalWrite(powerLatch, HIGH);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  Serial.println("Sensor is now ON. Taking a measurement...");
  delay(10000);
  // Turns the power latch circuit off
  digitalWrite(powerLatch, LOW);

  takeMeasurement();
  Serial.println("Task finished. Powering off now.");
  delay(100); 
 
}

void loop() {}

void takeMeasurement() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH);

  int distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  delay(2000);
}

```

