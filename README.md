# ConnectedBirdHouse

Requirements : 

sudo raspi-config // enable the camera

sudo apt-get install update // enable the update

sudo apt-get install upgrade // enable the upgrade


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
