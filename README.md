# esptool

Find USB devices:

```sh
ls -la /dev/tty.* /dev/cu.* 2>/dev/null | grep -E "(tty|cu)" | head -20
```

Install:

```sh
esptool --chip esp32s3 --port /dev/tty.usbmodem1101 --baud 921600 write-flash -z 0x0 tasmota32s3.factory.bin
# watch the boot up
screen /dev/tty.usbmodem1101 115200
esptool --chip esp32s3 --port /dev/tty.usbmodem1101 --baud 921600 write-flash -z 0x0 tasmota32s3.factory.bin
esptool --chip esp32s3 --port /dev/tty.usbmodem1101 --baud 921600 write-flash -z 0x0 tasmota32-lvgl-sensors.factory.bin
esptool --port /dev/tty.usbserial-ED5138CBD9 --baud 921600 write-flash -z 0x0 tasmota32-lvgl-sensors.factory.bin
esptool --port /dev/tty.usbserial-ED5138CBD9 --baud 115200 write-flash -z 0x0 tasmota32-lvgl-sensors.factory.bin
screen /dev/tty.usbserial-ED5138CBD9 115200
```

## Check i2c Drivers

```sh
I2cDriver  # You should see 15 and 88 in the list.
i2cscan 1
i2cscan 2
status 8
SetOption8 1  # degrees in F
```

## MStack Devices

### AtomS3

Sensors on GPIO1 & GPIO2

- GPIO1 - I2C SCL 1
- GPIO2 - I2C SDA 1

```sh
# latest working w/o display
{"NAME":"AtomS3 Disp","GPIO":[1,609,641,0,0,1,1,1,0,1,0,1,1056,0,0,768,416,736,1,1,0,704,800,8736,0,0,1,640,608,1,32,0,1,1,1,1,0,0],"FLAG":0,"BASE":1}
{"NAME":"M5Stack AtomS3","GPIO":[32,0,0,0,0,0,0,0,0,0,0,0,0,0,262,3552,224,0,0,0,225,0,0,0,0,0,0,0,0,0,0,0,0,263,269,0,0,0],"FLAG":0,"BASE":1}
# Non display blakcadder - https://templates.blakadder.com/m5stack_atoms3_lite.html
{"NAME":"M5 AtomS3 Lite","GPIO":[0,1,1,0,1057,1,1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1376,0,0,1,1,0,32,0,0,0,0,0,0,0],"FLAG":0,"BASE":1}
# Gemini-3-Pro suggestions
template {"NAME":"AtomS3","GPIO":[0,643,642,0,0,0,0,0,0,0,0,0,1056,0,0,672,704,224,0,0,0,225,0,0,0,0,0,0,0,0,0,0,0,673,674,0,0,0,1216,1248,0,32,0,0,0,0],"FLAG":0,"BASE":1}
template {"NAME":"AtomS3 Disp","GPIO":[1,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,260,1,0,0,0,259,0,640,608,1,0,1,32,6210,33],"FLAG":0,"BASE":1}
template {"NAME":"AtomS3","GPIO":[1,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,260,1,0,0,0,259,0,640,608,1,0,1,32,6210,33],"FLAG":0,"BASE":1}
Template {"NAME":"AtomS3-Lite","GPIO":[0,5090,5088,0,5616,0,0,0,0,0,0,0,0,0,0,0,0,0,560,0,0,0,0,0,0,0,0,0,0,0,0,0,0,5760,0],"FLAG":0,"BASE":1}
```

```sh
dimmer 100
DisplayText [z]Hello World
```

### AtomS3 w/Display

### M5Stick C



```sh
# using this
template {"NAME":"M5StickC","GPIO":[7840,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,0,1,0,6210,0,0,0,640,608,7872,7968,1,32,0,33],"FLAG":0,"BASE":1}
template {"NAME":"M5StickC","GPIO":[1,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,0,1,0,0,0,0,0,640,608,1,0,1,32,6210,33],"FLAG":0,"BASE":1}
```

## Matter

```sh
UfsDelete /.settings.lkg
SetOption151 1
Restart 1
matter
status 1
```

## Build

```sh
pio run -e tasmota32-lvgl-sensors -t clean
pio run -e tasmota32-lvgl-sensors
```

### Prompt for AtomS32 display with ENV III

Gemini-3-pro seems to do well

```txt
I'm running tasmota and this AtomS32 device

<div class="product-description rte" itemprop="description">
                <h5>PRODUCT DETAILS</h5>
<h2 id="description">Description</h2>
<p>AtomS3 is a highly integrated programmable controller that harnesses the power of the ESP32-S3 chip. This compact controller boasts a range of features, including integrated WiFi functionality and 8MB of on-chip FLASH memory. With the <strong>ESP32-S3</strong> chip, AtomS3 delivers exceptional performance and versatility for a variety of applications.<br><br>AtomS3 features a sleek <strong>0.85-inch IPS</strong> screen, providing clear visual feedback and user interaction. The bottom of the screen is equipped with programmable buttons, allowing for customizable functions and enhanced user control. It comes with a built-in 5V to 3.3V circuit, ensuring stable power supply for reliable operation. The controller incorporates a 6-axis gyro sensor <strong>MPU6886</strong>, enabling precise motion detection and orientation tracking. On-board Type-C interface facilitates both power supply and firmware download processes. Additionally, AtomS3 offers one HY2.0-4P expansion port, six GPIOs, and power pins reserved at the bottom for seamless integration and expansion of various applications.<br><br>AtomS3 is designed with compactness in mind, measuring at only 24*24*13 mm in size. Its small form factor makes it suitable for a wide range of embedded smart device applications. Whether it's robotics, IoT devices, or other embedded systems, AtomS3 provides a powerful and flexible solution to meet the project requirements.</p>
<ul>
<li><code>Reminder: If you need to burn the firmware, please press and hold the reset button (about 2 seconds) until the internal green LED lights up, then you can release it, at this time the device has entered download mode and waited for burning.</code></li>
</ul>
<h2 id="features">Features</h2>
<ul>
<li>Developed based on ESP32-S3FN8</li>
<li>Comes with USB download function</li>
<li>Built-in 3-axis gyroscope and 3-axis accelerometer (MPU6886)</li>
<li>Programmable keys</li>
<li>0.85 inch LCD screen</li>
<li>Infrared emission function</li>
<li>Expandable pins and interfaces</li>
<li>Development platform: Arduino, UIFlow</li>
</ul>
<h2 id="includes">Includes</h2>
<ul>
<li>1x AtomS3</li>
</ul>
<h2 id="applications">Applications</h2>
<ul>
<li>IoT node</li>
<li>Microcontroller</li>
<li>Wearables</li>
</ul>
<h2 id="specification">Specification</h2>
<table>
<thead>
<tr>
<th>Resources</th>
<th>Parameters</th>
</tr>
</thead>
<tbody>
<tr>
<td>MCU</td>
<td>ESP32-S3FN8</td>
</tr>
<tr>
<td>DCDC</td>
<td>SY8089</td>
</tr>
<tr>
<td>IMU</td>
<td>MPU6886</td>
</tr>
<tr>
<td>LCD</td>
<td>N085-1212TBWIG06-C08</td>
</tr>
<tr>
<td>Operating temperature</td>
<td>0°C ~ 40°C</td>
</tr>
<tr>
<td>Resolution</td>
<td>128(H)RGB x 128(V)</td>
</tr>
<tr>
<td>support voltage</td>
<td>5V</td>
</tr>
<tr>
<td>Power supply mode</td>
<td>TYPE C</td>
</tr>
<tr>
<td>output voltage</td>
<td>3.3V</td>
</tr>
<tr>
<td>IO interface × 6</td>
<td>G5/G6/G7/G8/G38G39</td>
</tr>
<tr>
<td>Screen communication protocol</td>
<td>SPI</td>
</tr>
<tr>
<td>Product Size</td>
<td>24mm × 24mm × 13mm</td>
</tr>
<tr>
<td>Package Size</td>
<td>65mm × 44.5mm × 14mm</td>
</tr>
<tr>
<td>Product Weight</td>
<td>6.8g</td>
</tr>
<tr>
<td>Package Weight</td>
<td>10.9g</td>
</tr>
</tbody>
</table>
<h2 id="learn-and-documents"><span>LEARN AND DOCUMENTS</span></h2>
<li><a href="https://docs.m5stack.com/en/core/AtomS3?ref=blakadder&amp;sub_id=" target="_blank">DOCUMENTS</a></li> 
      </div>

I want help configuring the tasmota template for this device. Here's what I'm currently using: {"NAME":"AtomS3 Disp","GPIO":[1,608,640,0,0,768,1056,576,0,736,0,704,0,0,0,0,0,641,609,800,0,260,1,0,0,0,259,0,1,1,1,0,1,32,6210,33,0,0],"FLAG":0,"BASE":1}

It shows toggle switches that this device doesn't seem to have. It's also missing sensors I think this has related to MPU6886. It also has a screen.

I also have an ENVIII attached and the SHT3X sensor is working. 

<div class="product-description rte" itemprop="description">
                <h5>PRODUCT DETAILS</h5>
<h2 id="description">Description</h2>
<p><strong>ENV III Unit</strong> is an environmental sensor that combines the SHT30 and QMP6988 sensors to measure temperature, humidity, and atmospheric pressure data. The unit is designed to provide accurate and reliable environmental sensing capabilities for various projects and applications.<br><br>The SHT30 sensor integrated into ENV III unit is a high-precision and low-power digital temperature and humidity sensor. It utilizes the I2C interface for communication and supports the address 0x44. The SHT30 sensor offers excellent accuracy and stability, making it suitable for temperature and humidity measurements in a wide range of environments.<br><br>The QMP6988 sensor, on the other hand, is an absolute air pressure sensor specifically designed for mobile applications. It provides high accuracy and stability in measuring atmospheric pressure. The QMP6988 sensor also utilizes the I2C interface for communication and uses the address 0x70.<br><br>ENV III unit can find applications in weather monitoring systems, environmental control systems, IoT projects, and other scenarios where accurate environmental data is essential. With its integrated sensors and I2C interface, it offers a convenient and straightforward solution for environmental sensing.</p>
<h2 id="product-features">Product Features</h2>
<ul>
<li>Simple and easy to use</li>
<li>High accuracy</li>
<li>I2C communication interface</li>
<li>HY2.0-4P interface, support platform <a href="http://flow.m5stack.com/?ref=blakadder&amp;sub_id=" target="_blank"> UIFlow </a>, <a href="http://www.arduino.cc/?ref=blakadder&amp;sub_id=" target="_blank"> Arduino </a>
</li>
<li>2x LEGO compatible holes</li>
</ul>
<h2 id="include">Include</h2>
<ul>
<li>1x ENV-III Unit</li>
<li>1x HY2.0-4P Cables</li>
</ul>
<h2 id="applications">Applications</h2>
<ul>
<li>Weather station</li>
<li>Storage barn environment monitoring</li>
</ul>
<h2 id="specification">Specification</h2>
<table>
<thead>
<tr>
<th>Resources</th>
<th>Parameter</th>
</tr>
</thead>
<tbody>
<tr>
<td>Maximum temperature measurement range</td>
<td>-40 ~ 120 ℃</td>
</tr>
<tr>
<td>Highest measurement accuracy</td>
<td>0 ~ 60 ℃/±0.2℃</td>
</tr>
<tr>
<td>Humidity measurement range/error</td>
<td>10 ~ 90 %RH / ±2%</td>
</tr>
<tr>
<td>Maximum measured value of air pressure/resolution/error</td>
<td>300 ~ 1100hPa / 0.06Pa / ±3.9Pa</td>
</tr>
<tr>
<td>Communication protocol</td>
<td>I2C: SHT30(0x44), QMP6988(0x70)</td>
</tr>
<tr>
<td>Working temperature</td>
<td>32°F to 104°F (0°C to 40°C )</td>
</tr>
<tr>
<td>Net weight</td>
<td>5g</td>
</tr>
<tr>
<td>Gross weight</td>
<td>17g</td>
</tr>
<tr>
<td>Product Size</td>
<td>24.2*32.2*8.1mm</td>
</tr>
<tr>
<td>Package Size</td>
<td>67*53*12mm</td>
</tr>
<tr>
<td>Case Material</td>
<td>Plastic ( PC )</td>
</tr>
</tbody>
</table>
<h2 id="learn-and-documents"><span>LEARN AND DOCUMENTS</span></h2>
<li><a href="https://docs.m5stack.com/en/unit/envIII?ref=blakadder&amp;sub_id=" target="_blank">DOCUMENTS</a></li>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1189890/_S4O1bruFEX.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="M5stack based PFD (Primary Flight Display)" class="lazypreload"> <a href="https://www.hackster.io/willembraa/m5stack-based-pfd-primary-flight-display-8a8ea5?ref=blakadder&amp;sub_id=" target="_blank"> M5stack based PFD (Primary Flight Display) </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/willembraa/m5stack-based-pfd-primary-flight-display-8a8ea5?ref=blakadder&amp;sub_id=" target="_blank"> M5stack based PFD (Primary Flight Display) </a></div>
<div class="learn-desc">An attempt to create a realtime "Artificial Horizon" on the M5.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1109439/fbda5a02-786e-4e97-8edc-5896ef503806_bkFuAdY5rL.gif?auto=format%2Ccompress&amp;gifq=35&amp;w=400&amp;h=300&amp;fit=min" alt="Matrix Shelly Thermostat: A Serverless Approach" class="lazypreload"> <a href="https://www.hackster.io/alessandro-polselli/matrix-shelly-thermostat-a-serverless-approach-7ec5d2?ref=blakadder&amp;sub_id=" target="_blank"> Matrix Shelly Thermostat: A Serverless Approach </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/alessandro-polselli/matrix-shelly-thermostat-a-serverless-approach-7ec5d2?ref=blakadder&amp;sub_id=" target="_blank"> Matrix Shelly Thermostat: A Serverless Approach </a></div>
<div class="learn-desc">A modular Wi-Fi smart thermostat that doesn't need you to maintain any infrastructure: deploy it everywhere, integrate with everything.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1250169/_MfcKqCUVek.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="M5Stack COM. LoRaWAN Using Arduino IDE" class="lazypreload"> <a href="https://www.hackster.io/kehosoftware/m5stack-com-lorawan-using-arduino-ide-38ab24?ref=blakadder&amp;sub_id=" target="_blank"> M5Stack COM. LoRaWAN Using Arduino IDE </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/kehosoftware/m5stack-com-lorawan-using-arduino-ide-38ab24?ref=blakadder&amp;sub_id=" target="_blank"> M5Stack COM. LoRaWAN Using Arduino IDE </a></div>
<div class="learn-desc">M5Stack recently released an updated LoRaWAN module. This tutorial teaches you how to connect it to The Things Network using Arduino IDE.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1017923/_PUFthCK0bd.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="Smart Plant with M5Stack" class="lazypreload"> <a href="https://www.hackster.io/makerfactory_team/smart-plant-with-m5stack-89212e?ref=blakadder&amp;sub_id=" target="_blank"> Smart Plant with M5Stack </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/makerfactory_team/smart-plant-with-m5stack-89212e?ref=blakadder&amp;sub_id=" target="_blank"> Smart Plant with M5Stack </a></div>
<div class="learn-desc">Make your plants smart and monitor water, temperature and humidity!</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1119729/_zpLHgG4xHE.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="M5StickC with HomeKit Automation" class="lazypreload"> <a href="https://www.hackster.io/vany5921/m5stickc-with-homekit-automation-63acb9?ref=blakadder&amp;sub_id=" target="_blank"> M5StickC with HomeKit Automation </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/vany5921/m5stickc-with-homekit-automation-63acb9?ref=blakadder&amp;sub_id=" target="_blank"> M5StickC with HomeKit Automation </a></div>
<div class="learn-desc">Used StickC with ESPHap library to connect to HomeKit</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1006384/1571711554666761_CjW8O5vLl6.gif?auto=format%2Ccompress&amp;gifq=35&amp;w=400&amp;h=300&amp;fit=min" alt="Smart Planting System" class="lazypreload"> <a href="https://www.hackster.io/sophiaxue/smart-planting-system-a842d7?ref=blakadder&amp;sub_id=" target="_blank"> Smart Planting System </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/sophiaxue/smart-planting-system-a842d7?ref=blakadder&amp;sub_id=" target="_blank"> Smart Planting System </a></div>
<div class="learn-desc">Building a smart planting system using M5Stack to monitor the lights, humidity, temperature, watering your plant automatically.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1264431/_UMWGX284gA.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="3 in 1 Sanitizer Dispenser" class="lazypreload"> <a href="https://www.hackster.io/386917/3-in-1-sanitiser-dispenser-d18432?ref=blakadder&amp;sub_id=" target="_blank"> 3 in 1 Sanitizer Dispenser </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/386917/3-in-1-sanitiser-dispenser-d18432?ref=blakadder&amp;sub_id=" target="_blank"> 3 in 1 Sanitizer Dispenser </a></div>
<div class="learn-desc">We have devised with an innovation that dispenses hand sanitizer, measures the temperature of user, shows the count of people in vicinity.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1256124/_JUQxx3QiJk.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="CovidStop" class="lazypreload"> <a href="https://www.hackster.io/hans-angelo_/covidstop-e9e64b?ref=blakadder&amp;sub_id=" target="_blank"> CovidStop </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/hans-angelo_/covidstop-e9e64b?ref=blakadder&amp;sub_id=" target="_blank"> CovidStop </a></div>
<div class="learn-desc">A technology which increases efficiency in signing in or out during peak hour areas when entering the campus while the SafeEntry implies.</div>
</div>
</div>
<div class="learn">
<div class="learn-img">
<img src="https://hackster.imgix.net/uploads/attachments/1255277/_zELd2Tpa8J.blob?auto=compress%2Cformat&amp;w=400&amp;h=300&amp;fit=min" alt="SEAM [Safe-Entry Access Machine]" class="lazypreload"> <a href="https://www.hackster.io/seam/seam-safe-entry-access-machine-de834d?ref=blakadder&amp;sub_id=" target="_blank"> SEAM [Safe-Entry Access Machine] </a>
</div>
<div class="learn-contents">
<div class="learn-title"><a href="https://www.hackster.io/seam/seam-safe-entry-access-machine-de834d?ref=blakadder&amp;sub_id=" target="_blank"> SEAM [Safe-Entry Access Machine] </a></div>
<div class="learn-desc">An automatic system to perform the tasks of Safe Entry. It consist an adjustable Thermometer, RFID / QR Code Check in and Counting.</div>
</div>
</div> 
      </div>
```
