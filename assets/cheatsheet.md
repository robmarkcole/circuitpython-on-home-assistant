# Cheatsheet
Cheatsheet of commands

## Flashing firmware

esptool.py --port /dev/ttyUSB0 erase_flash

##### feather_huzzah
esptool.py --port /dev/ttyUSB0 --baud 115200 write_flash --flash_size=detect 0 adafruit-circuitpython-feather_huzzah-3.1.1.bin

##### raw esp
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect -fm dio 0 adafruit-circuitpython-feather_huzzah-3.1.1.bin

## Connect via screen
screen /dev/ttyUSB0 115200
