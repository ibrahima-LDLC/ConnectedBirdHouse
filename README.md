# ConnectedBirdHouse

Requirements : 

sudo apt-get install update // enable the update

sudo apt-get install upgrade // enable the upgrade

First, connect the camera module to the board, please follow the official guide: https://www.raspberrypi.org/documentation/usage/camera/README.md 

Raspicam commands:

raspistill, raspivid and raspi yuv are command line tools to use the camera module.

Camera activation:

Before using any of the Raspicam applications , the camera must be activated.

On the desktop:

Select Preferences and Raspberry Pi Configuration from the desktop menu: a window appears. Select the Interface tab, then click on Enable camera option. Click on OK. You will need to reboot for the changes to take effect.

Using the command line:

Open the raspi-config tool from the terminal with the command:

sudo raspi-config

Select Interfacing Options then Camera and press Enter. Choose Yes or Ok. Go to Finished you will be prompted to reboot.

To test that the system is installed and running, try the following command:

raspistill -v -o test.jpg

The screen should display a five-second preview of the camera, then take a picture, saved in the test.jpg file, while displaying various informational messages.

raspistill :

raspistill is the command line tool for capturing photos with a Raspberry Pi camera module.

Basic raspistill usage:

With a camera module connected and enabled, enter the following command in the terminal to take a picture:

raspistill -o cam.jpg

Upside down photo:

In this example, the camera has been positioned upside down. If the camera is placed in this position, the image must be flipped to appear in the correct direction.

Vertical flip and horizontal flip:

With the camera placed upside down, the image must be rotated 180° to display correctly. The way to correct this is to apply both a vertical and horizontal flip by passing the -vfand :-hf flags

raspistill -vf -hf -o cam2.jpg

Vertical and horizontal flipped photo
Now the photo has been captured correctly.

Resolution:

The camera module takes photos at a resolution of 2592 x 19445 038 848 pixels or 5 megapixels.

File size:

A photo taken with the camera module will weigh approximately 2.4 MB. That's about 425 photos per GB.

Taking 1 photo per minute would take 1 GB in about 7 hours. This is a throughput of about 144 MB per hour or 3.3 GB per day.

Bash script:

You can create a Bash script that takes a picture with the camera. To create a script, open the editor of your choice and write the following sample code:

#!/bin/bash

DATE=$(date +"%Y-%m-%d_%H%M")
raspistill -vf -hf -o /home/pi/camera/$DATE.jpg

This script will take a picture and name the file with a timestamp.

You will also need to make sure the path exists by creating a camera folder:

mkdir camera
Let's say we saved it as camera.sh, we would first make the file executable:

chmod +x camera.sh

Then run with :

./camera.sh

More options:

For a full list of possible options, run Raspistill without arguments.

raspivid:

raspivid is the command line tool for capturing video with a Raspberry Pi camera module.

Basic use of raspivid:

With a camera module connected and enabled , record a video using the following command:

raspivid -o vid.h264

Don't forget to use -hfet -vf to return the image if necessary, like with raspistill

This will save a 5 second video file in the path indicated here as vid.h264(default length).

Specify the duration of the video:

To specify the duration of the video taken, pass the -tdrapeau with a number of milliseconds. For example:

raspivid -o video.h264 -t 10000

This will record 10 seconds of video.

More options :

For a complete list of possible options, run raspivid without arguments

MP4 video format:

The Raspberry Pi captures video as a raw H 264 video stream. Many media players will refuse to play it, or will play it at an incorrect speed, unless it is "wrapped" in an appropriate container format such as MP4. The easiest way to get an MP4 file from the raspivid command is to use MP4Box.

Install MP4Box with this command:

sudo apt install -y gpac

Capture your raw video with raspivid and pack it into an MP4 container like this:

# Capture 30 seconds of raw video at 640x480 and 150kBps bit rate into a pivideo.h264 file:
raspivid -t 30000 -w 640 -h 480 -fps 25 -b 1200000 -p 0,0,640,480 -o pivideo.h264
# Wrap the raw video with an MP4 container:
MP4Box -add pivideo.h264 pivideo.mp4
# Remove the source raw file, leaving the remaining pivideo.mp4 file to play
rm pivideo.h264

You can also use MP4 around your existing raspivid output, like this:

MP4Box -add video.h264 video.mp4

raspiyuv :

raspi yuv has the same feature set as raspistill but instead of producing standard image files such as .jpgs, it generates YUV420 or RGB888 image files from the camera's ISP output.

In most cases, using raspistill is the best option for standard image capture, but using YUV can be beneficial in some circumstances. For example, if you just need an uncompressed black and white image for computer vision applications, you can simply use the Y channel of a YUV capture.

Some specific points about YUV420 files are necessary to use them properly. The line stride (or pitch) is a multiple of 32, and each YUV plane is a multiple of 16 in height. This can mean that there may be extra pixels at the end of lines or gaps between planes, depending on the resolution of the captured image. These gaps are unused.


If there are no errors and you can see the result, you are ready to use the camera module.

What is SSH?
Before we look at how to control the Raspberry Pi with SSH, let's see exactly what SSH is.

SSH (for Secure SHell) is both a software program and a computer communication protocol. This protocol has the particularity of being fully encrypted. This means that all the commands you execute via SSH will be completely secret!

SSH was created in 1995 with the main goal of allowing remote control of a machine through a command line interface.

Today, SSH is mainly used through the free OpenSSH implementation which is present in most Linux distributions.

How does SSH work?
We won't go into cryptographic and other details here. We'll just give you a quick overview so that you can understand a little better how to use SSH.

Generally speaking, SSH allows you to connect remotely to a machine using a user account on that machine.

To do this, the computer that needs to connect to the remote machine will provide it with the name of the user to be used and its password. In some cases, it is possible to use a set of certificates on the computer and the remote machine, thus allowing a secure connection without having to type a password. This is a more advanced use case that we will not cover here.

By default SSH only offers command line control. It is possible in some cases to add a graphical interface but this is a more complex method that we will not see here.

Installing SSH to take control of your Raspberry Pi
Now that we know a little more about SSH, let's see how to set it up to control your Raspberry Pi!

First of all, you need to know that the installation of SSH is divided into two parts. You will need an SSH server on your Raspberry Pi and an SSH client on your computer. The first will receive the commands to be issued while the second will send them.



How to enable SSH on the Raspberry Pi.

The easiest way to control a Raspberry Pi remotely is to use SSH. But it must be enabled, which is no longer the case by default since 2016 and a massive attack on connected objects.

In this tutorial we will see how we can enable SSH on the Raspberry Pi, with or without a keyboard!

The hardware to activate SSH
To activate SSH we will only need a little hardware. Two solutions are possible.

First solution, activate SSH from the Raspberry Pi, in this case you will need a screen and a keyboard.

Second solution, activate SSH from your computer by editing a file on the SD card. In this case, you will need your PC and an SD card reader.

In any case, we will need a Raspberry Pi (any model), its power supply, and an SD card with Raspbian installed.

Activate SSH from the Raspberry Pi with raspi-config.
First solution to activate SSH, your Raspberry Pi has a screen and a keyboard. In this case, we will use the command line tool raspi-config directly.

Open a terminal, and type the following command:

sudo raspi-config

If you haven't done so, I advise you to start by changing the default password (first line, "Change user password") of the Raspberry Pi since it will now be accessible via SSH.

Then, once the password is changed, go to the "Interfacing Options" section and choose the SSH line, then "Yes".

And that's it, your SSH server is activated!

Activate SSH without screen or keyboard, from your PC.
Second solution to activate SSH, you do not have a keyboard or screen connected to your Raspberry Pi and you want to manage it entirely remotely.

Don't panic, the Raspberry Pi foundation has a solution, we just need to create a file on the SD card of the Raspberry, and it will automatically activate SSH at the next boot.

So insert the SD card of your Pi in your PC, and go to the boot partition, which is the only one accessible from Windows.

Once done, create a file named ssh in the boot partition of the card. No extension, no content, just an empty file named ssh.

Remove the card from the PC, put it back in the Pi, turn it on, and voilà, SSH is activated!

Conclusion
Enabling SSH on the Raspberry Pi is not that complicated.

You still need to connect the Raspberry Pi to your box, either by Wi-Fi or by Ethernet.

Note that we have only seen how to activate SSH. To learn how to use it, see our tutorial on how to connect to the Raspberry with SSH.


Usage :

sudo raspistill -t 0 // start the camera module

from picamera import PiCamera
from time import sleep

camera = PiCamera()

# resolution setting
camera.resolution = (1024,768)

# image rotation (useful if the camera is upside down)
camera.rotation = 180 

# preview, but only in a part of the screen
camera.start_preview(fullscreen = False, window = (50,50,640,480))

# a delay is needed to give the sensors time to adjust
sleep(5)

# save the file to the desktop
camera.capture('/home/pi/Pictures/image.jpeg')

# make the preview disappear.
camera.stop_preview()
