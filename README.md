# IoT-Temperature-and-Humidity-Monitoring-System
IoT Temperature and Humidity Monitoring System
Name: Vincent Rosencrantz
Student Credentials: Vr222ga


## Project description
Description: This project involves building an IoT-based temperature and humidity monitoring system using a DHT11 sensor and pico raspberry pi w microcontroller. The data is transmitted to the ThingSpeak platform for real-time monitoring and visualization. The project is estimated to take around 10 hours, including hardware setup, coding, and testing.

## List of Materials
Materials | 
------------- | 
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
LED and 330Ω Resistor |
Upload the code to the microcontroller via USB |

## Platform
ThingSpeak is chosen for its easy-to-use interface and real-time data visualization capabilities. It supports data storage, visualization, and analysis.

Comparison: Compared to other platforms like Blynk or Adafruit IO, ThingSpeak offers a straightforward approach with extensive support for MATLAB analytics.

# Code

```
import network
from machine import Pin
from time import sleep
import dht
import urequests

SSID = 'WIFI_NAME'
PASSWORD = 'YOUR_WIFI_PASSWORD'
sensor = dht.DHT11(Pin(15))
led = Pin(2, Pin.OUT)

THINGSPEAK_URL = "https://api.thingspeak.com/update"
THINGSPEAK_API_KEY = "YOUR_API_KEY"  # Replace with your API key

def connect_wifi(ssid, password):
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, password)

    while not wlan.isconnected():
        print('Connecting to network...')
        sleep(1)
    print('Connected to network:', wlan.ifconfig())

def send_to_thingspeak(temp, hum, temp_f):
    try:
        response = urequests.get(
            f"{THINGSPEAK_URL}?api_key={THINGSPEAK_API_KEY}&field1={temp}&field2={hum}&field3={temp_f}")
        response.close()
    except Exception as e:
        print('Failed to send data to ThingSpeak:', e)

connect_wifi(SSID, PASSWORD)

while True:
    try:
        sleep(10)  
        sensor.measure()  
        temp = sensor.temperature()  
        hum = sensor.humidity()  
        temp_f = temp * (9/5) + 32.0
        print('Temperature: %3.1f C' % temp)
        print('Humidity: %3.1f %%' % hum)
        print('Temperature: %3.1f F' % temp_f)

        if temp > 24:
            led.on() 
        else:
            led.off()  
        send_to_thingspeak(temp, hum, temp_f)
    except OSError as e:
        print('Failed to read sensor:', e)


```

## Transmitting the Data / Connectivity
   - Data Transmission Frequency: Data is sent every 10 seconds.
   - Wireless Protocols: Wi-Fi is used for data transmission.
   - Transport Protocols: HTTP protocol is used for sending data to ThingSpeak.
   - Design Choices: 
     - Wi-Fi provides sufficient range and bandwidth for indoor use.
     - HTTP is simple and effective for periodic data updates.
     - Security considerations include secure Wi-Fi configuration and ThingSpeak API keys.
    
## Presenting the Data


