## Project goal
My inspiration is the [esphomelib](https://esphomelib.com/) project, which makes it easy to integrated DIY hardware with [Home-Assistant](https://www.home-assistant.io/). Where my project differs, is that I will use Circuitpython instead of C++, and put an emphasis on developing novel code/solutions rather that focus on turn-key solutions to common problems. For Circuitpython projects, Home-Assistant will provide an IDE and integration with other services already exposed to Home-Assistant. This should get a lot of the tedious 'plumbing' aspects of projects out of the way, so users can focus on making cool hardware for their home automation projects. Note that I've selected Circuitpython over Micropython as the Circuitpython ecosystem (code and docs) are well maintained and curated by Adafrtuit, whilst Micropython is more a wild-west of code and documents, resulting in frequent incompatibility issues and frustrations.  

## Home-Assistant setup
I am running [Hassbian](https://www.home-assistant.io/docs/installation/hassbian/) distribution of [Home-Assistant](https://www.home-assistant.io/) (henceforth HA) on a pi 3B+ and am using the Cloud9 IDE addon, with setup process described [here](https://www.hackster.io/robin-cole/tensorflow-object-detection-with-home-assistant-7cc04b).

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/setup.jpg" width="700">
</p>

## Circuitpython REPL from HA
General advice on connecting to a Circuitpython REPL over serial (USB) is [here](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux). The following advice is specific to connecting to the board via the Cloud9 IDE, and assumes you are logged in as the default `pi` user.

1. `sudo apt-get install screen` -> install [screen](https://www.gnu.org/software/screen/), if you have never done so before.
2. `ls /dev/tty*` -> list connected devices, and I identified the esp by plugging and unplugging the board and checking what was added to this list. My boards shows up as `/dev/ttyUSB0`
3. `screen /dev/ttyUSB0 115200` -> connect to the board via screen. If you get the error `Cannot open your terminal '/dev/pts/2' - please check.`, run `script /dev/null` and try again.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/circuitpython_ha.png" width="1000">
</p>

## Circuitpython files
CircuitPython boards show up as an external disk when plugged in via USB on a regular computer. Unfortunately we cannot browse external filesystems from the Cloud9 addon for Home-Assistant. However Adafruit publish a python package [ampy](https://github.com/adafruit/ampy) which we can use to explore files on the board, and post files to the board.

1. `sudo pip3 install adafruit-ampy` -> to install, if you haven't before. Note sudo is required
2. `ampy --port /dev/ttyUSB0 ls` -> list files, note that screen cannot be running (unplug and replug the board to close screen and reset the connection).
3. Create a `main.py` file in Cloud9 and post it.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/ampy.png" width="400">
</p>

## Data logging & plotting
Not all projects will require data logging or plotting, but those projects are where integration with Home-Assistant can add the most value. Home-Assistant integration will allow logging of board data to a SQL database for long term storage. Home-Assistant can be used for plotting this data, but its plotting is pretty basic, so this project will also develop tools for plotting board data, for example using Bokeh.  

## CircuitPython for ESP8266
Most of the currently available CircuitPython boards lack wifi, which is a real limitation for home automation projects. Fortunately we can use CircuitPython on the ESP8266 boards which have both wifi. We then get the benefits of the nicely curated CircuitPython ecosystem, as well as low cost hardware with wifi. CircuitPython can run on both the official feather-huzzah-esp8266 board, but also a raw ESP8266 module (but with incorrectly mapped pins). I will develop this project using the feather-huzzah-esp8266 board, and then add the raw ESP8266 board to the docs later.

## Adafruit feather huzzah esp8266
The official esp8266 board supported by CircuitPython is the [feather-huzzah-esp8266](https://learn.adafruit.com/adafruit-feather-huzzah-esp8266). This board has connection for a battery, and the pinouts are correct, so its the baseline board for this project.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/huzzah.jpg" width="700">
</p>
