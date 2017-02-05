# About

rcswitch-pi is for controlling rc remote controlled power sockets 
with the raspberry pi. Kudos to the projects [rc-switch](http://code.google.com/p/rc-switch)
and [wiringpi](https://projects.drogon.net/raspberry-pi/wiringpi).
I just adapted the rc-switch code to use the wiringpi library instead of
the library provided by the arduino.

This fork is an update of the [domfi/rcswitch-pi library](https://github.com/domfi/rcswitch-pi) with the updated version of the [RCSwitch-Library](https://github.com/sui77/rc-switch/) to allow simple control of power switches using other protocols and code. 

# Usage

First you have to install the [wiringpi](https://projects.drogon.net/raspberry-pi/wiringpi/download-and-install/) library.
After that you can compile the example program *send* by executing *make*. 
You may want to changet the used GPIO pin before compilation in the send.cpp source file.


##Hardware
- Raspberry Pi 
- 433 MHz RF Transmitter Module
 
The 433 MHz Transmitter has three ports: VCC, GND and Data. Connect 
- GND -> GND (e. g. Port 6 on RPi3)
- VCC -> 5V Power (e. g. Port 2 on RPi3)
- Data -> GPIO 17 (Port 11 on RPi3 or change the port in the code)
 
##Installation
RCSwitch is based on wiringPi, so you first need to install wiringPi:
```
sudo mkdir /opt/rc-switch
cd /opt/rc-switch
sudo git clone git://git.drogon.net/wiringPi
cd wiringPi
sudo ./build
```

Then install this library:
```
cd /opt/rc-switch
sudo git clone https://github.com/thinkmobilede/rcswitch-pi.git
cd rcswitch-pi
sudo make
```

Then type `./send` or `sudo ./send` and you will get a list of possible command options.

Type `sudo ./send 5 000000000001010100010001` to send the binary code with protocol 5. 

#Integration in OpenHAB

This tool can be executed from OpenHAB with the exec-Library. Running on OpenHAB2 I could not make the new exec2-Library running, but exec1 is still running on OpenHAB2 with legacy-support enabled. If someone has the right configuration for exec2, any contribution is welcome.

##Allow execution of the script
enter `sudo visudo` and edit the configuration file. Add the line to allow OpenHAB to execute a sudo command:
```
/#User privilege specification
root    ALL=(ALL:ALL) ALL
openhab ALL=NOPASSWD: /opt/rc-switch/switch
*```

##Configuration
you need an item that has a command for on and off, example:
`Switch MyLamp1 "Door lamp" {exec=">[OFF:sudo /opt/rc-switch/rcswitch-pi/send 5 000000000001010100010001] >[ON:sudo /opt/rc-switch/rcswitch-pi/send 5 000000000001010100010000]"}`

and add your Switch to the sitemap:
`Switch MyLamp1`

##Trouble shooting
If you are facing problems, first check that the command on the shell is executed correctly and the switch is working. If this is working but it does not work on OpenHAB, check the rights and the user that OpenHAB is running on. 
