# RetroPie-How-to
A basic readme to get a Paspberry Pi 3b or Raspberry Pi Zero up and running RetroPie/EmulationStation

Note - 
ALWAYS SAVE WITH CHANGES

These are great commands to know.

ifconfig - will tell you what is connected (Internet/LAN)
sudo lsusb - will list all usb connections

sudo raspi-config
sudo apt-up update
sudo apt-get dist-upgrade #upgrades OS?
sudo apt-get ugrade
sudo apt-get clean
sudo apt-get autoremove


------------------------
This script is for adding old-time crt look to retro pi.
https://github.com/biscuits99/rp-video-manager/



For info more on this shrinking script. Be sure to look through the comments. All the way>
http://sirlagz.net/2013/03/10/script-automatic-rpi-image-downsizer/
-------------
For anyone that has issues with black borders, in my case in /boot/config.txt all I did was uncomment

disable_overscan=1 



----------------------------------------------------
Setting up WIFI -Pi Zero

#I don't know if these matter anymore. 
(etc/network/interfaces or 
sudo nano etc/network/interfaces)

#yep - this works for pixel. ver 2.1.0 - retro pi?
etc/wpa_supplicant/wpa_supplicant.conf or 
sudo nano etc/wpa_supplicant/wpa_supplicant.conf

And add to the end -
network={
    ssid="your ssid"
    psk="your psk"
    key_mgmt=WPA-PSK
}
*Ref - https://davidmaitland.me/2015/12/raspberry-pi-zero-headless-setup/

-------------------------------------------
new idea - https://www.raspberrypi.org/magpi/wifi-detector/
        Seems like a cool idea and there is a cool auto reboot on loss of wifi code.

---------------------------------
Overclocking - Retro Pie
Overclocked the Pi3 using https://github.com/retropie/RetroPie-Setup/wiki/Overclocking and also looking at the config.txt man file from the rasperry pi site. good info. oh, and it works. seens to. I used their suggested levels. n64 seems to crank. better anyway.quite a bit better.
This link is aso worth reading. https://www.raspberrypi.org/documentation/configuration/config-txt.md

the file to modify is - /boot/config.txt - use this as -  sudo nano /boot/config.txt -
 - - - - these are the setting that I used to overclock. (It does make it speedier) - just add this at the end of the file.
For Pi3b -

#Overclock added - date
arm_freq=1300
gpu_freq=500
core_freq=500
sdram_freq=500
sdram_schmoo=0x02000020
over_voltage=2
sdram_over_voltage=2
#End Overclock

For
PiZero -

#Overclock added - date
arm_freq=1000
gpu_freq=500
core_freq=500
sdram_freq=500
sdram_schmoo=0x02000020
over_voltage=2
sdram_over_voltage=2
#End Overclock

htop is a great way of looking at the processes. top also works and i think it is installed.


Updating -
Also by the way of updating, you may need to update the pi to the most current packages. to do that
sudo apt-get update
and
sudo apt-get dist-upgrade
*****important ****
sudo apt-get clean - will clean out the old packages and free up file space.


Im bringing this back to the top as I may have found a quick solution to all this issue. I wanted to make a plug and play system. I was not able but after some work, I think I may have found a resolution.

1st issue is the keyboard. The GB vs US think has just about killed me. I went to google late (I have no idea why it took so long) THe answer is in a simple google of the difference between the BG vs US keyboard - duh.

Adding extra file for retro pie is the best - see below. wifikeyfile.txt is golden. This was implemented in a later version 4.1 i think. May not (won't) work in earlier.

##### With the simple above system, I was able to connect on port 22 via two different services. Both ssh and shftp via firezilla.

-------Okay, you really NEED to set up the keyboard to the US style. There are things that may need to be edited and scripts that need to be typed that will kill you otherwise.
So we are back to setting up the keyboard...

Ref - https://thepihut.com/blogs/raspberry-pi-tutorials/25556740-changing-the-raspberry-pi-keyboard-layout
sudo raspi-config (Note - this is also where you turn on the SSH and VNC)
select change keyboard Layout - just click sudo raspberry k through - hit okay
next select other
select english US
then select english us
hit okay and click through the default setting. (Just hit okay)
(works)

=============================
getting back to roms
this link - http://raspberrypi.stackexchange.com/questions/9282/retropie-not-detecting-my-roms-even-after-tweaking-settings-in-emulationstati#9288 - says that there is a need to un-zip the file into a smb. Hmmm. I thought they worked off of a zip. Maybe. Actually it is .7z not .zip.



sepths scraper or a spelling version thereof







Hints and Helps -
For creating a disk image of a drive/card use Disks app under Accessories in Linux Mint 17.3

This is for setting up a Raspberry Pi Zero 1.3 with Retro Pie 4.1 including wifi and controller.
    Changing keyboard from the default of GB to US. (This is necessary to re-map keyboard from British layout to the keyboard I have, US layout.)
    Adding wifi connectivity to your device by filling in network credentials. (For scraping and SSH.)
    Turning on SSH through raspi-config menu.
    Adding controller device


Okay, copying a 16gb image to a 16gb sd card sounds pretty straightforward but it is not. Even if the card that was copied only had a few gigs, the image will contain all of the cards blocks including even the free space. So, 8gb image is fine on a 16gb card as would be 16gb image to a 32gb card. But same size to same size really never works.

So now that we have stuff set up; retropie, keyboard, wifi, ssh, controller, roms, etc., it's time to make a copy that can be backed up to the same size sd card should something blow up or we wanted to share the setup with others or just not ever having to remember command line wifi ever again.
    Reference for this information can be found at Shrinking images on Linux - Softwarebakery

    This will go a bit easier if you change your directory to where your image is located.
    cd Desktop maybe? ls is your friend. Also all this need to be don in sudo.

sudo modprobe loop
sudo losetup -f
sudo losetup /dev/loop0 myimage.img (replace myimage with your image's name.)
--
sudo gparted /dev/loop0
    Select the partition and click Resize/Move. A window similar to the following will popup:
    Drag the right bar to the left as much as possible. But leave a little room.



*Add things I learned section.
SSH into the device is pretty easy. Use putty. Input device ip. User = pi Pass = raspberry
    sudo ifconfig will show you the wlano and show if you are connected. If your not then no SSH.
    sudo raspi-config
        To change the host name of the device use Hostname under Advance options

**More related/unrelated to setup stuff. I wanted to pre-configure a complete working (hey, plug this in and everything is done out of the box device. Because of this, I wanted to also configure the controller. Well, ain't happening. It seems that currently (01132017) this is not possible. I tracked a post from a seemingly developer of Emulation Station that says, "ES currently does wipe its own controller config during an install, so you have to reconfigure. It won't get rid of the retroarch-joypads config though - emulationstation doesn't use that - that's for retroarch emulators.

I will probably change the ES install script to preserve existing configs, but in the meantime, just reconfigure etc." - https://github.com/RetroPie/RetroPie-Setup/issues/966
Good to know.

Something else. I was receiving a no controller available error on the start up of Emulation Station. I thought I screwed up something in the USB. Now seems more like a voltage issue or a connection issue with the usb hub that I was using. So check your connections.

And Updating - No one wants to provide stuff that is not as up to date as you can get and in order to do that, you have to access - sudo ~/RetroPie-Setup/retropie_setup.sh - And select the Update All Installed Packages. And be prepared to wait a bit.

***Overclock to 1000/500/500 - https://github.com/RetroPie/RetroPie-Setup/wiki/Overclocking -
/boot/config.txt

May or may have not had a problem with this overclock. Issue seems to have been about loading roms. Transferred roms via ssh to device and nothing. No roms. Loaded roms directly via sd and removed overclock and voila. So maybe this is a bust.

Raspberry Pi Zero

arm_freq=1000
gpu_freq=500
core_freq=500
sdram_freq=500
sdram_schmoo=0x02000020
over_voltage=2
sdram_over_voltage=2

***Super handy - vcgencmd measure_temp -
htop for cpu useage

*-*-*
New setup for internet using Retropie wifi thingie. There is an option for allowing setup through a file and it seemed to work. However it did also allow a second ip to be set with the router. So two ip's. Not good. Seems to make the device run hot. But it was scraping so maybe thats it (yup). Also scraping sucks. But I'm working on it.

It seems that selecting /boot/wifikeyfile.txt looks for the infor and automatically sets it all up. My issue may be due to the manual config already having been set. So maybe that's the reason for the two ip's.  ---> All we need is to locate the bash file posted above into RetroPiemenu! So we need NO keyboard to import setted wifi keys. NO KEYBOARD!
--This seems to work --

sudo nano /boot/wifikeyfile.txt
ssid="NETWORK_NAME"
psk="NETWORK_PASSWORD"
save the file
reboot
(hopefully you have a connection.)
Most likely will not work for andy other than retropie as the file resides only within retropie. no raspbian stuff.

 If you want to configure your system for multiple wifi networks you can do so manually by editing /etc/wpa_supplicant/wpa_supplicant.conf and add some additional network blocks. Via - https://retropie.org.uk/forum/topic/4691/solved-feature-request-wifi-key-import-via-boot-wifikeyfile-txt/15

/etc/wpa_supplicant/wpa_supplicant.conf is where the some of the instances are stored

_+_+_+_
Obviously, if there is not a controller attached to retro pie on boot, you will not get beyoun the set up a controller screen even if there is a previously configure controller. Kinda doesnt make sense as a thought but logically it does. If there is no way of navigating the menus of the UX then why bother.

====Had a heck of a time with the keyboard. Really. I had some characters in my password that were not straight mapped the british keyboard keys. This caused a no ending obsession with not having to figure this out. But through time and google there is this - http://blogs.transparent.com/english/differences-between-the-us-and-uk-computer-keyboard/

    an AltGr key is added to the right of the space bar
    the # symbol is replaced by the £ symbol and a 102nd key is added next to the Enter key to accommodate the displaced #
    @ and ” are swapped
    the ~ is moved to the # key, and is replaced by a ¬ symbol on the backquote (`) key
    the \ key is moved to the left of the Z key
    the Enter key spans two rows, and is narrower to accommodate the # key
    on laptop computers, the | and \ key is often placed next to the space bar
Just go to the website or wikipedia


SIZER ______________________ ________________________
this is seemingly to work...
sudo ./pishrink.sh image.img #note - changed fro just shrink to pishrink! erf.


(just need to make sure your in the same directory with everything. the bash script and the img file. --nav to the desktop and put everything on the desktop.)

-------------SIZER_________via http://smartretro.co.uk/forums/viewtopic.php?t=58

If you want to do this in Linux there is an auto script that does it for you here:
http://sirlagz.net/2013/03/10/script-au ... downsizer/

Some useful links:
http://raspberrypi.stackexchange.com/a/7192/20719
https://github.com/Drewsif/PiShrink
https://github.com/dweeber/rpiwiggle
http://softwarebakery.com/shrinking-images-on-linux
http://sirlagz.net/2013/03/04/how-to-re ... le-part-2/
https://cygwin.com/faq/faq.html#faq.using.not-found

------------How to turn off screen saver - good information
==== TYPE = THIS = TO = NIX = THE = SCREEN = SAVER ====
xset s 0 0 s off s noblank s noexpose -dpms
at a command prompt,
or make it stick by placing that line in an init file. 
 - via - https://groups.google.com/forum/#!topic/comp.sys.raspberry-pi/vNIOExgLOAM
