## Project goal
My inspiration is the [esphomelib](https://esphomelib.com/) project, which makes it easy to integrate DIY hardware with [Home-Assistant](https://www.home-assistant.io/). Where my project differs, is that I will use Circuitpython instead of C++, meaning that users only need to know python and they also enjoy the rapid development cycle possible with a REPL. Home-Assistant will provide an IDE and tools to make it easy to flash firmware to the board, and provide a framework for integrating the board with other services already exposed to Home-Assistant. This should get a lot of the tedious 'plumbing' aspects of projects out of the way, so users can focus on making cool hardware for their home automation projects. Note - I've selected Circuitpython over Micropython as the Circuitpython ecosystem (code and docs) are well maintained and curated by Adafrtuit, whilst Micropython is more a wild-west of code and documents.  

## Home-Assistant setup
I am running [Hassbian](https://www.home-assistant.io/docs/installation/hassbian/) distribution of [Home-Assistant](https://www.home-assistant.io/) on a pi 3B+ and am using the Cloud9 IDE addon, with setup process described [here](https://www.hackster.io/robin-cole/tensorflow-object-detection-with-home-assistant-7cc04b).

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/setup.jpg" width="700">
</p>

## CircuitPython for ESP8266
Most of the currently available CircuitPython boards lack wifi, which is a limitation for some home automation projects. Fortunately we can use CircuitPython on the ESP8266 boards which have both wifi and bluetooth (we will be using wifi and MQTT for posting data to Home-Assistant). We therefor get the benefits of the nicely curated CircuitPython ecosystem, as well as low cost hardware with wifi. CircuitPython can run on both the official feather-huzzah-esp8266 board, but also a raw ESP8266 module (but with incorrectly mapped pins). I will develop this project using the feather-huzzah-esp8266 board, and then later add specific instructions for using the the raw ESP8266 board. However since most of the instructions will be identical, I will just refer to 'the ESP8266 board' in the following instructions.

## Adafruit feather huzzah esp8266
The official esp8266 board supported by CircuitPython is the [feather-huzzah-esp8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266). This board has connection for a battery, and the pinouts are correct, so it is the baseline board for this project.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/huzzah.jpg" width="700">
</p>

## Flashing firmware
The official instructions for flashing the board firmware are [here](https://learn.adafruit.com/welcome-to-circuitpython/circuitpython-for-esp8266). We use the [esptool](https://github.com/espressif/esptool) for this process, and I created a new directory `circuitpython` using the IDE:

1. `sudo pip3 install esptool` -> install the esptool and check it was successfull by entering `esptool.py -h` which prints the help info.
2. `sudo curl -OL https://github.com/adafruit/circuitpython/releases/download/3.1.1/adafruit-circuitpython-feather_huzzah-3.1.1.bin` -> download the firmware `.bin` file, here for Circuitpython release 3.1.1. Check [here](https://github.com/adafruit/circuitpython/releases/latest) for the URL to the latest Circuitpython release.
3. .. to add

## Circuitpython REPL from Home-Assistant
General advice on connecting to a Circuitpython REPL over serial (USB) is [here](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux). The following advice is specific to connecting to the ESP8266 board via the Cloud9 IDE, and assumes you are logged in as the default `pi` user.

1. `sudo apt-get install screen` -> install [screen](https://www.gnu.org/software/screen/), if you have never done so before.
2. `ls /dev/tty*` -> list available devices/terminals, and I identified the ESP8266 by plugging and unplugging the board and checking what was added to this list. My boards shows up as `/dev/ttyUSB0`
3. `screen /dev/ttyUSB0 115200` -> connect to the board via screen. If you get the error `Cannot open your terminal '/dev/pts/2' - please check.`, run `script /dev/null` and try again.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/circuitpython_ha.png" width="1200">
</p>

## Circuitpython files
Adafruit publish a python package [ampy](https://github.com/adafruit/ampy) which we can use to explore files on the board, as well as post `.py` files to the board.

1. `sudo pip3 install adafruit-ampy` -> to install, if you haven't before. Note sudo is required
2. `ampy --port /dev/ttyUSB0 ls` -> list files, note that screen cannot be running (unplug and replug the board to close screen and reset the connection).
3. Create a `main.py` file in Cloud9, then post and view it using ampy:

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/ampy.png" width="700">
</p>

## Data plotting
Home-Assistant integration will allow logging of board data to a SQL database for long term storage. Home-Assistant can also be used for plotting this data, but its plotting capabilities are pretty basic. Therefore the scope of this project will also include the development of tools for plotting board data, for example using Bokeh.  
