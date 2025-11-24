# Analog Sensor Threshold Controller

This Arduino project reads data from an analog sensor (connected to Pin A0) and activates different digital output pins (LEDs, Relays, or Buzzers) based on specific value ranges. It also logs the real-time sensor values to the Serial Monitor.

## 🚀 Features
* **Real-time Monitoring:** Reads analog values every 2ms.
* **Serial Logging:** Outputs sensor readings to the Serial Monitor at 9600 baud.
* **Multi-Stage Indicators:** Triggers different output pins based on three distinct sensitivity levels.
* **Safety/Idle State:** Automatically turns off all outputs if the sensor value falls outside the defined active ranges.

## 🛠️ Hardware Requirements
* **Microcontroller:** Arduino Uno, Nano, or compatible board.
* **Input:** Analog Sensor (e.g., Potentiometer, LDR, Soil Moisture Sensor, Gas Sensor).
* **Output:** 4x LEDs (or other actuators).
* **Resistors:** 220Ω (for LEDs) and any necessary pull-up/down resistors for your specific sensor.
* **Jumper Wires & Breadboard.**

## 🔌 Circuit Connections

| Component | Arduino Pin | Description |
| :--- | :--- | :--- |
| **Sensor Output** | **A0** | Analog Input Signal |
| **Indicator 1** | **D2** | Active range: 100 - 600 |
| **Indicator 2** | **D3** | Active range: 601 - 625 |
| **Indicator 3** | **D4** | Active range: 626 - 700 |
| **Indicator 4** | **D5** | Active range: 626 - 700 |

## ⚙️ Logic & Thresholds

The system categorizes the analog input (0-1023) into the following logic states:

| Sensor Value | Action | Active Pins |
| :--- | :--- | :--- |
| **100 to 600** | Low/Medium Range | **Pin 2 HIGH** |
| **601 to 625** | Narrow Mid Range | **Pin 3 HIGH** |
| **626 to 700** | High Range | **Pin 4 & 5 HIGH** |
| **< 100 or > 700** | Out of Range / Idle | **All Pins LOW** |

## 💻 Installation & Usage

1.  **Connect the hardware** according to the circuit connections table above.
2.  **Open the code** in the Arduino IDE.
3.  **Select your board** and COM port.
4.  **Upload** the sketch to your Arduino.
5.  Open the **Serial Monitor** (`Ctrl` + `Shift` + `M`) and set the baud rate to **9600**.
6.  Observe the `sensor = [value]` output and watch the LEDs react to changes in the sensor input.

## 📄 Code

```cpp
const int analogInPin = A0;
int sensorValue = 0;

void setup() {
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  sensorValue = analogRead(analogInPin);
  Serial.print("sensor = ");
  Serial.println(sensorValue);
  delay(2);

  // Logic Control
  if((sensorValue >= 100) && (sensorValue <= 600)){
    digitalWrite(2, HIGH);
    delay(100);
  }
  else if((sensorValue >= 601) && (sensorValue <= 625)){
    digitalWrite(3, HIGH);
    delay(100);
  }  
  else if((sensorValue >= 626) && (sensorValue <= 700)){
    digitalWrite(4, HIGH);
    digitalWrite(5, HIGH);
  }
  else{
    // Reset all pins if out of range
    digitalWrite(2, LOW);
    digitalWrite(3, LOW);
    digitalWrite(4, LOW);
    digitalWrite(5, LOW);
    delay(100);
  }
}
```
