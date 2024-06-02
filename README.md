# Color-Based Object Tracking using OpenCV

![occode](https://github.com/Syed-Basila/object-tracking-based-on-color-using-OpenCV/assets/123718024/ffb15282-2fa4-447f-8352-f5c8e4e5c9bf)

## Overview

This project demonstrates how to track objects based on color using OpenCV and Python. It detects and tracks objects of a specific color in real-time using the webcam feed.

## Features

- Real-time color-based object tracking
- Displays bounding circles around detected objects
- Outputs direction based on object position

## Demo


https://github.com/Syed-Basila/object-tracking-based-on-color-using-OpenCV/assets/123718024/46f68fef-791b-48d2-a744-0fcec625600e


## Prerequisites

Ensure you have the following installed:

- Python 3.x
- OpenCV
- imutils

You can install the required packages using pip:

```sh
pip install opencv-python imutils
```
## Usage
1.Clone the repository

```sh
git clone https://github.com/yourusername/color-object-tracking.git
cd color-object-tracking
```
2.Run the script

```sh
python color_tracking.py
```

## Code Explanation
Here's a brief explanation of the main parts of the code:

1.Import necessary libraries
```sh
import imutils  # For resizing the image
import cv2  # For image processing
```
2.Define the color range for tracking
```sh
Lower = (0, 180, 0)  # Lower bound of the color (HSV)
Upper = (179, 255, 255)  # Upper bound of the color (HSV)
```
3.Initialize the camera and process the video feed

- Read frames from the camera
- Resize the frame
- Apply Gaussian blur to smooth the image
- Convert the frame to HSV color space
- Create a mask for the specified color
- Erode and dilate the mask to remove noise
- Find contours and draw circles around the detected object
- Print direction based on object position and radius

```sh  
camera = cv2.VideoCapture(0)  # Camera initialization
while True:
    (grabbed, frame) = camera.read()  # Reading frame from the camera
    frame = imutils.resize(frame, width=600)  # Resizing the frame
    blurred = cv2.GaussianBlur(frame, (11, 11), 0)  # Smoothing
    hsv = cv2.cvtColor(blurred, cv2.COLOR_BGR2HSV)  # Convert BGR to HSV color format
    mask = cv2.inRange(hsv, Lower, Upper)  # Mask the specified color
    mask = cv2.erode(mask, None, iterations=2)
    mask = cv2.dilate(mask, None, iterations=2)
    cnts = cv2.findContours(mask.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)[-2]
    center = None
    if len(cnts) > 0:
        c = max(cnts, key=cv2.contourArea)
        ((x, y), radius) = cv2.minEnclosingCircle(c)
        M = cv2.moments(c)
        center = (int(M["m10"] / M["m00"]), int(M["m01"] / M["m00"]))
        if radius > 10:
            cv2.circle(frame, (int(x), int(y)), int(radius), (0, 255, 255), 2)
            cv2.circle(frame, center, 5, (0, 0, 255), -1)
            if radius > 250:
                print("stop")
            else:
                if(center[0] < 150):
                    print("Left")
                elif(center[0] > 450):
                    print("Right")
                elif(radius < 250):
                    print("Front")
                else:
                    print("Stop")
    cv2.imshow("Frame", frame)
    key = cv2.waitKey(1) & 0xFF
    if key == ord("q"):
        break
camera.release()
cv2.destroyAllWindows()
```
## Workflow
- Capture video from the webcam.
- Resize and smooth the frame.
- Convert the frame to HSV color space.
- Create a mask for the specified color.
- Apply erosion and dilation to reduce noise.
- Find contours of the masked color.
- Draw bounding circles around the detected object.
- Print the direction based on the object's position and size.
