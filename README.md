# ESP32 Flood Monitor

This project implements a flood monitoring system using an ESP32 microcontroller. It measures the water level using an analog sensor, calculates the flood risk, displays the information on an LCD screen, provides visual risk indication using LEDs, and sounds an alarm with a buzzer for high-risk situations. The system also publishes the monitoring data to an MQTT broker.

## Features

*   **Water Level Measurement:** Reads analog data from a water level sensor.
*   **Risk Assessment:** Determines flood risk (LOW, MEDIUM, HIGH) based on the measured water level.
*   **LCD Display:** Shows real-time flood monitoring information (Location, Level, Risk).
*   **LED Indicators:** Green, Yellow, and Red LEDs visually represent LOW, MEDIUM, and HIGH risk levels, respectively.
*   **Buzzer Alarm:** Activates a buzzer when the risk level is HIGH.
*   **MQTT Communication:** Publishes flood monitoring data (location, level, risk) to a specified MQTT topic.
*   **Configurable:** Easily change sensor location, Wi-Fi credentials, and MQTT server settings in the code.

## Hardware Requirements

*   ESP32 Development Board
*   Analog Water Level Sensor
*   I2C LCD Display (20x4 or compatible)
*   Green, Yellow, and Red LEDs
*   Buzzer
*   Resistors (for LEDs, if necessary)
*   Jumper Wires
*   Breadboard (optional)

## Circuit Diagram

Connect the components as shown in the diagram below:

![Circuit Scheme](ESP32scheme.png)

**Pin Connections:**

*   Water Level Sensor Signal Pin -> ESP32 GPIO 36 (VP)
*   I2C LCD SDA -> ESP32 GPIO 21
*   I2C LCD SCL -> ESP32 GPIO 22
*   Green LED -> ESP32 GPIO 25
*   Yellow LED -> ESP32 GPIO 26
*   Red LED -> ESP32 GPIO 27
*   Buzzer -> ESP32 GPIO 13
*   Water Level Sensor VCC -> ESP32 3.3V/5V
*   Water Level Sensor GND -> ESP32 GND
*   LCD VCC -> ESP32 5V
*   LCD GND -> ESP32 GND
*   LEDs and Buzzer: Connect the other leg through a resistor to GND (LEDs) or directly to GND (Buzzer, if active buzzer).

## Software Requirements

*   Arduino IDE or VS Code with PlatformIO extension
*   ESP32 Board Support Package for Arduino IDE
*   Required Libraries:
    *   `Wire.h` (usually built-in)
    *   `LiquidCrystal_I2C.h`
    *   `WiFi.h` (usually built-in)
    *   `PubSubClient.h`

You can install the required libraries through the Arduino IDE Library Manager or PlatformIO library management.

## Node-RED Integration

This project can integrate with Node-RED to visualize the data and create dashboards. You will need the following Node-RED nodes installed:

*   `node-red-contrib-aedes` (MQTT Broker)
*   `node-red-contrib-mqtt-broker` (Another MQTT Broker node, choose one based on your needs)
*   `node-red-dashboard`
*   `node-red-node-serialport` (If you need to read serial data, though this project uses MQTT)

## Setup and Usage

1.  **Open the Project:** Open the `main.ino` file in your Arduino IDE or PlatformIO project.
2.  **Install Libraries:** Ensure you have installed the `LiquidCrystal_I2C` and `PubSubClient` libraries.
3.  **Configure Settings:**
    *   Update the `sensorID` variable with the desired location name.
    *   Enter your Wi-Fi SSID and password in the `ssid` and `password` variables.
    *   Verify or change the `mqttServer` and `mqttPort` if you are using a different MQTT broker.
4.  **Upload the Code:** Select the correct ESP32 board and port, then upload the code to your ESP32.
5.  **Monitor:**
    *   Observe the water level, location, and risk level on the LCD.
    *   Note the LED indicators and buzzer for risk levels.
    *   Monitor the serial output for detailed information, including the JSON data being published.
    *   Subscribe to the MQTT topic `flood-monitor/data` on your specified MQTT broker to receive the published data.

## Calibration

The mapping of analog sensor values to centimeters (`level_cm = map(waterLevel, 0, 4095, 0, 30);`) assumes a linear relationship and a maximum level of 30 cm corresponding to the maximum analog reading (4095). You may need to adjust the `0` and `30` values in the `map()` function based on your specific sensor and the range you want to measure.

The risk thresholds (30% and 70%) can also be adjusted in the `loop()` function to match your desired risk levels.

## Screenshots

Here are some screenshots showing the LCD display at different water levels and risk levels:

**Medium Risk (13.0 cm)**
![Medium Risk](Screenshot%202025-06-06%20at%202.43.15%E2%80%AFPM.png)

**High Risk (30.0 cm)**
![High Risk](Screenshot%202025-06-06%20at%202.44.47%E2%80%AFPM.png)

**Low Risk (0.0 cm)**
![Low Risk](Screenshot%202025-06-06%20at%202.45.20%E2%80%AFPM.png)