# Lesson 02: Numpy 数组操作

```python
"""
Lensson02 : Numpy 数组操作
Created By : Harrytsz
Time : 2019-6-12
"""
import cv2 as cv
import numpy as np


def access_pixels(image):
    print(image.shape)
    height = image.shape[0]
    width = image.shape[1]
    channels = image.shape[2]
    print('Height: {}, Width: {}, Channels: {}'.format(height, width, channels))

    #-------------------------------# 颜色反转
    for h in range(height):
        for w in range(width):
            for c in range(channels):
                pv = image[h, w, c]
                image[h, w, c] = 255 - pv
    cv.imshow('pixels_demo', image)


def create_image():

    ################## 三通道
    # img = np.zeros([400, 400, 3], np.uint8)
    # img[:, :, 0] = np.ones([400, 400])*255  # Blue
    # img[:, :, 1] = np.ones([400, 400])*255 # Green
    # img[:, :, 2] = np.ones([400, 400])*255 # Red

    ################## 单通道
    img = np.zeros([400, 400, 1], np.uint8)
    img = img * 127 # Gray
    img[:, :, 0] = np.ones([400, 400])*127


    cv.imshow('new image', img)


src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE) # blue, green, red
cv.imshow('input image', src)
t1 = cv.getTickCount()
# access_pixels(src)
create_image()
t2 = cv.getTickCount()
print("Cost Time: {:.4f} seconds".format((t2-t1)/cv.getTickFrequency()))
cv.waitKey(0)
cv.destroyAllWindows()
```
