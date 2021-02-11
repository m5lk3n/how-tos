# How to set up a Pi Sense HAT (a.k.a. "Astro-Pi") on Ubuntu 20.04

## The Hardware

- Raspberry Pi 400
- Raspberry Pi Sense HAT (Astro-Pi)

## The Environment

```bash
$ uname -a
Linux raspi400 5.8.0-1011-raspi #14-Ubuntu SMP PREEMPT Tue Dec 15 08:53:29 UTC 2020 aarch64 aarch64 aarch64 GNU/Linux
```

## The Problems

```bash
$ sudo apt-get install sense-hat
Reading package lists... Done
Building dependency tree       
Reading state information... Done
E: Unable to locate package sense-hat
```

```bash
$ # running Python examples with sensors, ending up in e.g.:
OSError: Humidity Init Failed
```

## The Installation/Solution

From [EnvTrackerNode - The Environment Tracking Server](https://github.com/J-Pai/EnvTrackerNode):

"*Create the file /etc/modprobe.d/blacklist-industialio.conf with the following contents:*"

```bash
blacklist st_magn_spi
blacklist st_pressure_spi
blacklist st_sensors_spi
blacklist st_pressure_i2c
blacklist st_magn_i2c
blacklist st_pressure
blacklist st_magn
blacklist st_sensors_i2c
blacklist st_sensors
blacklist industrialio_triggered_buffer
blacklist industrialio
```

```bash
$ sudo apt install python3-pip i2c-tools
$ sudo pip3 install sense-hat
$ git clone git@github.com:RPi-Distro/RTIMULib.git
$ cd RTIMULib-master/Linux/python/
$ python3 setup.py build
$ sudo python3 setup.py install # sudo required to install built package under /usr/local/lib
$ sudo addgroup $USERNAME input
$ sudo addgroup $USERNAME i2c
# reboot
```

## The Test Run

```bash
# after reboot, come back to this repo folder and run:
$ python3 main.py
```

## The Research

- [Raspberry Pi Sense HAT tutorial](https://thepihut.com/blogs/raspberry-pi-tutorials/43316292-raspberry-pi-sense-hat-tutorial)
- [Can't Install SenseHat Support Software on Ubuntu](https://www.raspberrypi.org/forums/viewtopic.php?t=280081)
- [All sensors output 0.0](https://github.com/astro-pi/python-sense-hat/issues/79#issuecomment-720109545)
- [EnvTrackerNode - The Environment Tracking Server](https://github.com/J-Pai/EnvTrackerNode)

## The Outlook

- [Getting started with the Sense HAT](https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat)
- [Learn Sense HAT with Raspberry Pi](https://magpi.raspberrypi.org/articles/learn-sense-hat-with-raspberry-pi)
