# Octoprint Instructions raspberry pi
Instructions to replicate and use the raspbarry pi with octoprint image to 3D print models remotely.

Check Videos Demo and Images for more help.

by [Gregorio Orlando](https://github.com/GRINGOLOCO7)
 <br>

# Introduction

In this project we are making the 3D printers in the lab more clever.

Using a **Raspberry pi**, we installed the **Octoprint** image in it and we are now able to controll 2 3D printers remotely. Moreover whe have 2 webcams for each printer, we cna
<br>

# Connect 2 cameras
Resources:
- https://www.youtube.com/watch?v=n9smmmH1O7Y
- https://community.octoprint.org/t/camera-streamer-configuration-on-the-new-camera-stack-for-octopi/49950
- https://hatoum.com/blog/2020/5/27/install-both-a-raspberry-pi-camera-and-a-usb-camera-on-octopi-017

1. **See what cams you have: Go to config files of default**
```
ls /boot/camera-streamer/
```
2. **See attribute on one cam:**
```
cat /boot/camera-streamer/usb-default
```
config
Default cam is in port 8080

3. **Check on this camera:**
```
sudo systemctl status camera-streamer
```
See that stream is up
```
ps -aux | grep camera
```

4. **Remove default config files and add my cams**:
```
sudo remove-usb-camera default
```
-> no longer cam on 8080
```
rm /boot/camera-streamer libcam
```
remove raspberry cam file

5. **Plug in camera:**
```
sudo add-usb-camera <name> <port>
```
<img src="Images/AddFirstCam.png" alt="AddFirstCam" width="250" height="100" />

<small><i>We added a default camera to webcams. We called it "webcam_edu" and automatically set it to port 8080, which is the first available.</i></small>

6. **Check first camera**:

<img src="Images\WebcamInOctoprintGUI.png" alt="AddFirstCam" width="200" height="100" />

<small><i>To do so we go to our AP address and see if the default camera pops out. We are using the "Classic Webcam" plugin for now.</i></small>

7. **Add second webcam:** Plug in the second camera and do the same comand as before:
```
sudo add-usb-camera <name> <port>
```
Automaticly it will addres the second webcam to the next port avaible. (because 8080 is the first, this one will be 8081)
<img src="Images\AddSeconfCam.png" alt="AddFirstCam" width="200" height="100" />

8. **Modify the config files just created**:
> In both files substitute YUYV with MJPEG

> In the cam in port 8081 add next to option this line to listen all the interfaces: OPTIONS='--http-listen="0.0.0.0"'

> **In my case**, both webcams collided on under the same name in *cd /dev/v41/by-id* so I also edited the DEVICE variable to */dev/v41/by-path/path_for_video0_and_video2* (to find the path for video0 and video2 just type *ls -l /dev/v41/by-path*)


9. **View both webcams**: Now if you go on a new page and search for
> http://<AP_address>/webcam/?action=stream -> to see default one (in port 8080)

> http://<AP_address>>:8081/?action=stream

10. **MultiCam pluggin**: Download Multicam Pluggin and add this 2 links to your webcams.

<br>

# Connect 2 3D printers

1. Go to Octoprint “settings”
2. Press “printers profile”
3. Press “add printer”
4. Set dimensions, name, and details of that printer

Plug in the printers with the USB cable to Raspberry pi.

We can then add a printer profile trough settings in Octoprint.

In the lab we have a Ultimaker2+ and a Witbox2.

I added both profiles, so you can switch from one to the other.

To do so, look at left part of Octoprint GUI. In *connection* chose the printer name and for the other options set AUTO.

Then connect to the printer.

<br>

# How to do a good slice

**3D printer tutorial:**
- rescue video: https://www.youtube.com/watch?v=KDDfhqc57BI&t=375s
- resource page: https://support.ultimaker.com/s/article/1667337576725

**Prepare & slice settings:**
- _Layer Height_: How thin each layer => quality => approx. 0.1/0.08 (very precise)
- _Initial/Top Layer Height_: approx. 0.2 (need to be thick)

- _Speed_: slow => more precise (20mm/s)
- _Infill Speed_: can be a bit faster than speed (ex: 25 mm/s)
- _Travel Speed_: for small & fragile things low (ex: 30 mm/s)

- _Top Bottom_: value is a multiple of nozzle (ex: nozzle = 0.4 => Top = 0.8 or 1.2)

- _Infill_: percentage of filling approx. 20%/10%

- _Line width_: the lower, the most precise, 75% of nozzle size < x < 125% of nozzle size

**Prepare .gcode:**

Once your model is ready, you can import it in Cura, tune the settings as you prefer and then save the file as a .gcode.
You are now ready to drag the file in the web page with Octoprint running.
On the left side you save the file in Octoprint, on the right side you save the file in the SD card inserted in the 3D printer (if any).
Now you can start printing.
<br>

# Usage & 3D print your model

Tune_printer_model then drag the .gcode file or press upload file in Octoprint interface

# USAGE:

1. Enter:
```
http://achilles
```
to access in to the raspberry pi

2. It will ask for a username and pasward, we have created a Guest account for everyone:
- username: user
- password: ieroboticslab

3. Now you can drag the .gcode file you prepared in Cura and start printing.

4. Advices:
- slice in cura tuning the settings as best.
- add glue on the plate of the printer before priting. It will keep the object stick on the printer plate.
- TO CHECK YOUR PRINTER FROM HOME AND FROM MOBILE GO TO:
```
https://octoeverywhere.com
https://octoeverywhere.com/dashboard
```
> email: suzan.awinat@ie.edu

> password: ieroboticslab


# !!Attention!!

printing without monitoring is really dangerous. it is always adviced to check the printer in person and avoid leave it without supervision.

MANTAIN THE PRINTER IN GOOD STATUS!! TAKE CARE, CLEAN AND DON'T SWITCH THEM OFF.


