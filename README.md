# Smart Cultivation Water Controller using IoT (ESP8266 + Blynk)

This project is a smart water quality monitoring and control system for crab farming (budidaya kepiting). It utilizes various sensors connected to an ESP8266 to measure **pH level**, **turbidity**, and **temperature**, and sends real-time data to the **Blynk IoT platform** while also displaying it on a local **LCD I2C 16x2** display. Based on sensor readings, the system automatically controls **two relays** for operating solenoid valves to manage water conditions effectively.

---

## 📡 Features

- **Real-time Monitoring via Blynk App**
- **Temperature Detection** using DS18B20 sensor
- **pH Level Measurement** via ADS1115 analog interface
- **Turbidity Detection** to indicate water clarity (NTU level)
- **Automatic Relay Control** for solenoid valves:
  - Based on water pH: Acidic, Neutral, or Alkaline
  - Based on turbidity: Clear, Moderate, or Cloudy
  - Based on temperature: Cold, Normal, or Hot
- **LCD I2C 16x2** displays data locally and rotates readings
- Built-in logic for water quality classification and relay actuation

---

## 🔩 Hardware Used

| Component                | Quantity | Function                                        |
|--------------------------|----------|-------------------------------------------------|
| ESP8266 NodeMCU          | 1        | WiFi-enabled microcontroller                    |
| ADS1115 Module           | 1        | 4-channel ADC to read analog pH and turbidity   |
| pH Sensor                | 1        | Measures pH level via ADS1115                   |
| Turbidity Sensor         | 1        | Measures water clarity via ADS1115              |
| DS18B20 Temperature Sensor | 1      | 1-Wire waterproof temperature sensor            |
| LCD I2C 16x2             | 1        | Displays pH, NTU, and temperature               |
| 2-Channel Relay Module   | 1        | Controls solenoid valves                        |
| Solenoid Valves          | 2        | Opens/closes based on water condition           |
| Power Supply             | 1        | 5V or 3.3V (depending on components)            |

---

## 🔌 Wiring Connections

| Device/Module              | ESP8266 Connection Pin |
|---------------------------|------------------------|
| LCD I2C (SDA)              | D2                     |
| LCD I2C (SCL)              | D1                     |
| ADS1115 (SDA)              | D2 (shared I2C line)   |
| ADS1115 (SCL)              | D1 (shared I2C line)   |
| Turbidity Sensor (ADS A0) | A0 on ADS1115          |
| pH Sensor (ADS A1)        | A1 on ADS1115          |
| DS18B20 Temperature Sensor| D4                     |
| Relay Module IN1          | D6                     |
| Relay Module IN2          | D7                     |

> ⚠️ *Make sure to use logic level shifters or dividers if any sensors operate at 5V to avoid damaging the ESP8266 which uses 3.3V logic.*

---

## 🖥️ Blynk Configuration

Create a new Blynk template and configure:

- **Template ID:** `TMPL67a1oj8VY`
- **Template Name:** `Pengontrol air budidaya kepiting`
- **Auth Token:** *(Use your own token from Blynk dashboard)*

### Virtual Pins Setup

| Virtual Pin | Description                  |
|-------------|------------------------------|
| V0          | Water Temperature (°C)       |
| V1          | pH Value                     |
| V2          | Turbidity (NTU)              |
| V3          | Turbidity + Relay Status     |
| V4          | pH + Relay Status            |

---

## ⚙️ System Logic

- **pH Logic:**
  - `pH < 6`: Acidic → Solenoids ON
  - `6 ≤ pH ≤ 8`: Neutral → Solenoids OFF
  - `pH > 8`: Basic → Solenoids OFF

- **Turbidity Logic:**
  - `NTU < 24.70`: Clear → Solenoids ON
  - `24.70 < NTU < 71.48`: Moderate → Solenoids OFF
  - `NTU > 71.48`: Cloudy → Solenoids OFF

- **Temperature Logic:**
  - `< 30°C`: Cold → Solenoids ON
  - `30–39°C`: Normal → Solenoids OFF
  - `> 39°C`: Hot → Solenoids ON

All status outputs are shown on the LCD and pushed to Blynk for remote monitoring.

---

## 🛠️ Setup & Upload Instructions

1. **Wiring:** Connect all modules to ESP8266 as described in the wiring table above.
2. **Libraries Required:**
   - `ESP8266WiFi`
   - `Blynk`
   - `Adafruit_ADS1X15`
   - `LiquidCrystal_I2C`
   - `OneWire`
   - `DallasTemperature`
3. **Board Selection:** `NodeMCU 1.0 (ESP-12E Module)`
4. Paste your **WiFi SSID**, **password**, and **auth token** in the code
5. Upload the code via **Arduino IDE**
6. Open **Serial Monitor** to observe sensor data for testing

---

## 📲 Using the System

- Upload the code to your ESP8266
- Connect to WiFi and open the Blynk App
- Watch live data for **pH**, **TDS**, and **temperature**
- Observe **relay activity** based on water quality conditions
- LCD rotates values for local monitoring

---

Let me know if you'd like the wiring diagram visualized as an image or the flow logic as a chart.
