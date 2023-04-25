# Pypyueye (stripped down)

Convenience wrapper around [pyueye](https://pypi.python.org/pypi/pyueye) API for IDS uEye cameras.

This is a stripped down version of [pypyueye](https://github.com/galaunay/pypyueye) by Gaby Launay, with no PyQt based GUI and its related threads. This version consists of a single file that you can easily drop in your codebase.

Pypyueye (stripped down) allows to easily manage an IDS uEye camera using higher level function than the ones provided by `pyueye` and the official `ueye` packages.

## Installation

You have to install the IDS driver for the camera you intend to use. Then in your Python environment make sure you have installed the modules `pyueye` and `numpy`.

## Usage

The following script allows to display the live video:

```Python
import cv2 # usign OpenCV to display the captured image
from pypyueye import Camera
from pyueye import ueye

with Camera(device_id=0, buffer_count=10) as cam:
    # setup the camera
    cam.set_colormode(ueye.IS_CM_BGR8_PACKED) # format for OpenCV
    cam.set_aoi(0, 0, 800, 600)
    print(f"INITIAL VALUES")
    print(f'fps: {cam.get_fps()}')
    print(f'Available fps range: {cam.get_fps_range()}')
    print(f'Pixelclock: {cam.get_pixelclock()}')
    cam.set_pixelclock(100)
    cam.set_fps(20)
    print("")
    print(f"MODIFIED VALUES")
    print(f'fps: {cam.get_fps()}')
    print(f'Available fps range: {cam.get_fps_range()}')
    print(f'Pixelclock: {cam.get_pixelclock()}')

    # start non-blocking live stream
    cam.capture_video(wait=False)
    
    while True:
        # capture a single frame
        frame = cam.get_frame()

        if frame is None:
            break

        cv2.imshow('Live Camera', frame)
    
        # press ESC to stop
        if cv2.waitKey(1) & 0xFF == 27:
            break
     
    # stop the live stream
    cam.stop_video()
```
