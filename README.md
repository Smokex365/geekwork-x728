Forked from original with updated scripts to work with other RPi distros such as Ubuntu.

Will look into switching from rc.local to systemd at a later time.

User guide: https://wiki.geekworm.com/X728-Software

X728-Software
Jump to navigationJump to search

The following test is base on 2022-01-28-raspios-bullseye-armhf.zip

Python version is 3;

1. Enable I2C funcion on Raspbian:

reter to How to enable I2C
2. Login via teminal window, then update & upgrade & install necessary software (python and i2c tool library)

sudo apt-get update
sudo apt-get upgrade
sudo apt-get -y install i2c-tools python3-smbus python-smbus
2.1 Once you have logged into your Raspberry Pi from the command line, run the command to see all the connected devices

sudo i2cdetect -y 1

![i2c](https://wiki.geekworm.com/images/b/b9/X728x-i2c.png)

#36 - the address of the battery fuel gauging chip
#68 - the address of the RTC chip
#Different x728 versions may have different values
3. Download x728 setup scripts:

cd ~
git clone https://github.com/Smokex365/geekwork-x728
4. Install script&reboot:

Firstly please select your x728 version, please use x728-v2.1.sh for x728 v2.1 and v2.2

sudo bash x728-v2.1.sh
#New add buzzer support
or

sudo bash x728-v2.0.sh
or

sudo bash x728-v1.0.sh
then

sudo reboot
You can get the following python file in /home/pi/ fold:

x728bat.py # Reading battery voltage
x728pld.py # Testing AC power off/loss or power adapter failure detection, added buzzer function on v2.1
x728plsd.py # Testing Auto shutdown when AC power loss or power adapter failure
5. Set and Read the RTC time

#If you need to set the system time for any reason you can use the following command :  
date -s "5 MAR 2019 13:00:00"
#Write the system date and time to the RTC module after your correct the system date and time :  
sudo hwclock -w
#Read the date and time back from the RTC module:  
sudo hwclock -r
6. How to reading battery voltage and percentage, this is the sample code, you can modify it by your request.

sudo python /home/pi/x728bat.py
X728-bat-2.jpg

User Guide: https://github.com/geekworm-com/x728 to know more details;

But we hope that the script can be executed automatically when the Raspberry Pi board boots, we can use crontab system command to achieve it. Refer to How to add crontab job or refer to the following:

sudo crontab -e
Crontab-step1.png

Choose "`1`" then press Enter

Add a line at the end of the file that reads like this:

@reboot python /home/pi/x728bat.py
X728-crontab.png

Save and exit. In nano, you do that by hitting CTRL + X, answering Y and hitting Enter when prompted.

7. Power off command on Raspbian from software

x728off
x728off is safe shutdown command
press on-board blue button 1-2 seconds to reboot
press on-board blue button 3 seconds to safe shutdown,
press on-board blue button 7-8 seconds to force shutdown.
8. Testing AC power off/loss or power adapter failure detection (need to short the 'PLD' pin on v1.x), also test the buzzer function on v2.1

cd ~
sudo python3 x728pld.py
or 
sudo python x728pld.py
X728-pld-2.jpg

PS:

1. X728pld.py is ONLY a sample code file, you can modify it according to your own needs

2. The buzzer is controlled by software. It is found that some customers do not know how to turn off the buzzer's warning sound. Here is a reminder.

X728-buzzer.png

9. Testing Auto shutdown when AC power loss or power adapter failure

cd ~
sudo python3 x728pld.py
or 
sudo python x728plsd.py
