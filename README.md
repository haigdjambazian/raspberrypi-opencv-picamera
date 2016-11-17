# raspberrypi-opencv-picamera

Start Python3
------------
```
source ~/.profile
workon cv3
python
```
Capturing to an OpenCV object
-----------------------------
Following: https://picamera.readthedocs.io/en/release-1.12/recipes2.html#capturing-to-an-opencv-object

```python
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

Crude motion detection system using PiMotionAnalysis
----------------------------------------------------
Following: https://picamera.readthedocs.io/en/release-1.12/api_array.html#picamera.array.PiMotionAnalysis

```python
import picamera
import picamera.array
import numpy as np

class MyMotionDetector(picamera.array.PiMotionAnalysis):
    def analyse(self, a):
        a = np.sqrt(
            np.square(a['x'].astype(np.float)) +
            np.square(a['y'].astype(np.float))
            ).clip(0, 255).astype(np.uint8)
        # If there're more than 10 vectors with a magnitude greater
        # than 60, then say we've detected motion
        if (a > 60).sum() > 10:
            print('Motion detected!')

with picamera.PiCamera() as camera:
    camera.resolution = (640, 480)
    camera.framerate = 30
    camera.start_recording(
        '/dev/null', format='h264',
        motion_output=MyMotionDetector(camera)
        )
    camera.wait_recording(30)
    camera.stop_recording()
```
    
