# IoT-Temperature-and-Humidity-Monitoring-System
IoT Temperature and Humidity Monitoring System
Name: Vincent Rosencrantz
Student Credentials: Vr222ga


## Project description
This project involves building an IoT-based temperature and humidity monitoring system using a DHT11 sensor and Pico Raspberry Pi W microcontroller. The data is transmitted to the ThingSpeak platform for real-time monitoring and visualization. The project is estimated to take around 3 hours, including hardware setup, coding, and testing.

## Objective
his project was selected to learn about IoT technology and its applications in environmental monitoring.
The project aims to monitor the temperature and humidity levels in a specific environment and analyze the data for trends and anomalies.
Insights: The data will provide insights into the environmental conditions over time, helping identify patterns such as daily temperature variations and humidity levels.


## List of Materials
Materials | materials | 
------------- | ------------- |
Raspberry Pi pico w Microcontroller | 
DHT11 Temperature and Humidity Sensor | 
Breadboard and Jumper Wires | 
LED and 330Ω Resistor |

## Computer Setup
Setup| 
------------- | 
Install Thonny IDE| 
Flash MicroPython firmware onto pico raspberry pi w 
Write and test the code in Thonny| 
Upload the code to the microcontroller via USB |

## Putting Everything Together
<img width="537" alt="Skärmavbild 2024-06-29 kl  13 36 34" src="https://github.com/stoked1999/IoT-Temperature-and-Humidity-Monitoring-System/assets/119662935/68dddc7a-1d50-4d45-82a5-c4f6eb251ddf">

#### Connections
DHT11 Sensor:
- VCC to 3.3V on Pico W
- GND to GND on Pico W
- Data pin to GPIO 15
#### LED:
- Anode to GPIO 4
- Cathode to 330Ω resistor, then to GND


## Platform
ThingSpeak is chosen for its easy-to-use interface and real-time data visualization capabilities. It supports data storage, visualization, and analysis.

Comparison: Compared to other platforms like Blynk or Adafruit IO, ThingSpeak offers a straightforward approach with extensive support for MATLAB analytics.

# Code

## Import necessary libraries for networking, sensor reading, and HTTP requests
```
import network
from machine import Pin
from time import sleep
import dht
import requests
```

## Define WiFi credentials
```
SSID = 'WIFI_NAME'
PASSWORD = 'YOUR_WIFI_PASSWORD'
```

## Initialize the DHT11 sensor on GPIO pin 15 and the LED on GPIO pin 2
```
sensor = dht.DHT11(Pin(15))
led = Pin(2, Pin.OUT)
```

## ThingSpeak API endpoint and API key
```
THINGSPEAK_URL = "https://api.thingspeak.com/update"
THINGSPEAK_API_KEY = "YOUR_API_KEY"  # Replace with your API key
```

## Function to connect to WiFi
```
def connect_wifi(ssid, password):
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)

    while not wlan.isconnected():
        print('Connecting to network...')
        sleep(1)
    print('Connected to network:', wlan.ifconfig())
```

## Function to send sensor data to ThingSpeak
```
def send_to_thingspeak(temp, hum, temp_f):
    try:
        response = urequests.get(
            f"{THINGSPEAK_URL}?api_key={THINGSPEAK_API_KEY}&field1={temp}&field2={hum}&field3={temp_f}")
        response.close()
    except Exception as e:
        print('Failed to send data to ThingSpeak:', e)

connect_wifi(SSID, PASSWORD)
```

## Main loop to read sensor data and send it to ThingSpeak
```
while True:
    try:
        sleep(300)  # Wait for 5 minutes (300 seconds) before taking a new reading
        sensor.measure()  # Measure temperature and humidity
        temp = sensor.temperature()  # Get temperature in Celsius
        hum = sensor.humidity()  # Get humidity in percentage
        
        # Print sensor readings to the console
        print('Temperature: %3.1f C' % temp)
        print('Humidity: %3.1f %%' % hum)

        # Turn on the LED if temperature is above 24°C, otherwise turn it off
        if temp > 24:
            led.on()
        else:
            led.off()
        
        # Send the sensor data to ThingSpeak
        send_to_thingspeak(temp, hum, temp_f)
    except OSError as e:
        print('Failed to read sensor:', e)

```

## Transmitting the Data / Connectivity
   - Data Transmission Frequency: Data is sent every 5 minutes.
   - Wireless Protocols: Wi-Fi is used for data transmission.
   - Transport Protocols: HTTP protocol is used for sending data to ThingSpeak.
   - Design Choices: 
     - Wi-Fi provides sufficient range and bandwidth for indoor use.
     - HTTP is simple and effective for periodic data updates.
     - Security considerations include secure Wi-Fi configuration and ThingSpeak API keys.
    
## Presenting the Data

    Dashboard Examples:
<img width="1071" alt="Skärmavbild 2024-07-02 kl  10 16 01" src="https://github.com/stoked1999/IoT-Temperature-and-Humidity-Monitoring-System/assets/119662935/f86740ea-f08f-4c09-ae22-736011722d0d">

- The data sent from the Raspberry Pi is displayed in the Thingspeak channels. Data is sen from the raspberry pi every 5 minutes. 

# Finalizing the Design
    - Final Results: The project successfully monitors and visualizes temperature and humidity data in real-time.
    - Final Thoughts: The project was educational and provided insights into IoT and data analytics. Future improvements could include battery power for portability and additional sensors for more comprehensive monitoring.
    
<img width="525" alt="Skärmavbild 2024-07-01 kl  12 52 11" src="https://github.com/stoked1999/IoT-Temperature-and-Humidity-Monitoring-System/assets/119662935/94f23404-689b-4407-a660-88f1ab71dda4">



