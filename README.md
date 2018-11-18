## Home-Assistant setup
I am running [Hassbian](https://www.home-assistant.io/docs/installation/hassbian/) distribution of [Home-Assistant](https://www.home-assistant.io/) (henceforth HA) on a pi 3B+ and am using the Cloud9 IDE addon, with setup process described [here](https://www.hackster.io/robin-cole/tensorflow-object-detection-with-home-assistant-7cc04b).

## Circuitpython REPL from HA
General advice on connecting to a Circuitpython REPL over serial (USB) is [here](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux). The following advice is specific to connecting to the board via the Cloud9 IDE, and assumes you are logged in as the default `pi` user.

1. `sudo apt-get install screen` -> install [screen](https://www.gnu.org/software/screen/), if you have never done so before.
2. `ls /dev/ttyACM*` -> list connected boards, I have 1 board connected and this returns `/dev/ttyACM0`
3. `screen /dev/ttyACM0 115200` -> connect to the board via screen. If you get the error `Cannot open your terminal '/dev/pts/2' - please check.`, run `script /dev/null` and try again.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/circuitpython_ha.png" width="1000">
</p>

## Circuitpython files
Circuitpython boards show up as an external disk when plugged in via USB. Unfortunately we cannot browse external filesystems from Cloud9 currently. However Adafruit publish a python package [ampy](https://github.com/adafruit/ampy) which we can use.

1. `sudo pip3 install adafruit-ampy` -> to install, if you haven't before. Note sudo is required
2. `ampy --port /dev/ttyACM0 ls` -> list files, note that screen cannot be running (unplug and replug the board).
