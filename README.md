## Project goal
My inspiration is the [esphomelib](https://esphomelib.com/) project, which makes it easy to integrated DIY hardware with [Home-Assistant](https://www.home-assistant.io/). Where my project differs, is that I will use Micropython/Circuitpython instead of C++, and put an emphasis on developing novel code/solutions rather that focus on turn-key solutions to common problems. For Circuitpython projects, Home-Assistant will provide an IDE and backend support for long term data logging, and networking with other services already exposed to home assistant made available to the Circuitpython board. This should get a lot of the tedious 'plumbing' aspects of projects out of the way, so users can focus on making cool hardware for their home automation projects.

## Home-Assistant setup
I am running [Hassbian](https://www.home-assistant.io/docs/installation/hassbian/) distribution of [Home-Assistant](https://www.home-assistant.io/) (henceforth HA) on a pi 3B+ and am using the Cloud9 IDE addon, with setup process described [here](https://www.hackster.io/robin-cole/tensorflow-object-detection-with-home-assistant-7cc04b).

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/setup.jpg" width="700">
</p>

## Circuitpython REPL from HA
General advice on connecting to a Circuitpython REPL over serial (USB) is [here](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux). The following advice is specific to connecting to the board via the Cloud9 IDE, and assumes you are logged in as the default `pi` user.

1. `sudo apt-get install screen` -> install [screen](https://www.gnu.org/software/screen/), if you have never done so before.
2. `ls /dev/ttyACM*` -> list connected boards, I have 1 board connected and this returns `/dev/ttyACM0`
3. `screen /dev/ttyACM0 115200` -> connect to the board via screen. If you get the error `Cannot open your terminal '/dev/pts/2' - please check.`, run `script /dev/null` and try again.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/circuitpython_ha.png" width="1000">
</p>

## Circuitpython files
Circuitpython boards show up as an external disk when plugged in via USB on a regular computer. Unfortunately we cannot browse external filesystems from the Cloud9 addon for Home-Assistant. However Adafruit publish a python package [ampy](https://github.com/adafruit/ampy) which we can use to explore files on the board, and post files to the board.

1. `sudo pip3 install adafruit-ampy` -> to install, if you haven't before. Note sudo is required
2. `ampy --port /dev/ttyACM0 ls` -> list files, note that screen cannot be running (unplug and replug the board).
3. Create a `main.py` file in Cloud9 and post it.

<p align="center">
<img src="https://github.com/robmarkcole/circuitpython-on-home-assistant/blob/master/images/ampy.png" width="400">
</p>

## Data logging
Not all projects will require data logging or plotting, but those projects are where integration with Home-Assistant can add the most value. Time series data will be logged to a `.csv` file on the board, and the project will provide tools to extract that data and post it to a SQL database for long term storage. One challenge is that low cost boards typically do not have an onboard clock, and a therefore don't know the time. One option is manually keep track of the time, recording a reading every second, then calculating back to the start time. This approach requires probably a fair amount of boilerplate code, and could be prone to errors. Another option is to use a dedicated real-time clock ([RTC](https://en.wikipedia.org/wiki/Real-time_clock)), such as [this one](https://learn.adafruit.com/adafruit-pcf8523-real-time-clock/rtc-with-circuitpython). This adds a hardware requirement, but should be robust. A complete project write-up with this approach is at https://learn.adafruit.com/data-logging-with-feather-and-circuitpython

## Data plotting
Once captured data is entered to the SQL database, Home-Assistant could be used for plotting the data. However Home-Assistant plottting is pretty basic, so this project will also develop tools for plotting data, for example using Bokeh.  
