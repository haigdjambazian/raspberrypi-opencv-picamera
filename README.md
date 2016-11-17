# raspberrypi-opencv-picamera

Capturing to an OpenCV object
=============================
Following: https://picamera.readthedocs.io/en/release-1.12/recipes2.html#capturing-to-an-opencv-object

Start Python3
------------
```
source ~/.profile
workon cv3
python
```

```
import time
import picamera
import numpy as np
import cv2

with picamera.PiCamera() as camera:
    camera.resolution = (320, 240)
    camera.framerate = 24
    time.sleep(2)
    image = np.empty((240, 320, 3), dtype=np.uint8)
    camera.capture(image, 'bgr')
```
