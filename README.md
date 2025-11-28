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
i2cscan
status 8
```

```sh
# using this
template {"NAME":"M5StickC","GPIO":[7840,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,0,1,0,6210,0,0,0,640,608,7872,7968,1,32,0,33],"FLAG":0,"BASE":1}
template {"NAME":"M5StickC","GPIO":[1,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,0,1,0,0,0,0,0,640,608,1,0,1,32,6210,33],"FLAG":0,"BASE":1}
template {"NAME":"AtomS3 Disp","GPIO":[1,0,0,0,0,768,1056,576,0,736,0,704,0,0,1024,0,0,641,609,800,0,260,1,0,0,0,259,0,640,608,1,0,1,32,6210,33],"FLAG":0,"BASE":1}
```

## Matter

```sh
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