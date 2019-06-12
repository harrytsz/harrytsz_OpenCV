# Lesson 03 : 色彩空间

![image_1dd5fcsqblcuihj1qrn4fp3u62i.png-223.2kB][1]

![image_1dd5fev8qbpjuep1584dmobr3f.png-192.4kB][2]

![image_1dd5fiqpo1mlfap417d14rq1pe95s.png-248.8kB][3]

## 色彩空间相互转换：

```python
"""
Lesson03 : 色彩空间
Created By : Harrytsz
Time : 2019-6-12
"""

import cv2 as cv


# cv 自带颜色反转 API 0.106s
def inverse(image):
    dst = cv.bitwise_not(image)
    cv.imshow('inverse demo', dst)


# 色彩空间相互转换
def color_space_demo(image):
    gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY) # 转到 GRAY
    cv.imshow('gray', gray)
    hsv = cv.cvtColor(image, cv.COLOR_BGR2HSV)   # 转到 HSV
    cv.imshow('hsv', hsv)
    yuv = cv.cvtColor(image, cv.COLOR_BGR2YUV)   # 转到 YUV
    cv.imshow('yuv', yuv)
    Ycrcb = cv.cvtColor(image, cv.COLOR_BGR2YCrCb) # 转到 YCrCb
    cv.imshow('ycrcb', Ycrcb)


src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE)
cv.imshow('input image', src)
t1 = cv.getTickCount()
# inverse(src)
color_space_demo(src)
t2 = cv.getTickCount()
print('Cost Time: {:.4f} s'.format((t2-t1)/cv.getTickFrequency()))
cv.waitKey(0)
cv.destroyAllWindows()
```

![image_1dd5g57smg0j1gq81akf1nso65a69.png-205.4kB][4]

最常见的有两个：      
* HSV --> RGB
* YUV --> RGB

<font color='red'>**Note:** RGB --> HSV 的转换特别重要！</font>

```python
"""
Lesson03 : 色彩空间
Created By : Harrytsz
Time : 2019-6-12
"""

import cv2 as cv
import numpy as np


# cv 自带颜色反转 API 0.106s
def inverse(image):
    dst = cv.bitwise_not(image)
    cv.imshow('inverse demo', dst)


# 色彩空间相互转换
def color_space_demo(image):
    gray = cv.cvtColor(image, cv.COLOR_BGR2GRAY) # 转到 GRAY
    cv.imshow('gray', gray)
    hsv = cv.cvtColor(image, cv.COLOR_BGR2HSV)   # 转到 HSV
    cv.imshow('hsv', hsv)
    yuv = cv.cvtColor(image, cv.COLOR_BGR2YUV)   # 转到 YUV
    cv.imshow('yuv', yuv)
    Ycrcb = cv.cvtColor(image, cv.COLOR_BGR2YCrCb) # 转到 YCrCb
    cv.imshow('ycrcb', Ycrcb)


def extrace_object_demo():
    capture = cv.VideoCapture("D:/images/video_00.mp4")
    while True:
        ret, frame = capture.read()
        if not ret: break  # 如果 ret == False 则退出
        hsv = cv.cvtColor(frame, cv.COLOR_BGR2HSV)
        lower_hsv = np.array([35, 43, 46])
        upper_hsv = np.array([77, 255, 255])
        mask = cv.inRange(hsv, lowerb=lower_hsv, upperb=upper_hsv)
        cv.imshow('video', frame)
        cv.imshow('mask', mask)
        c = cv.waitKey(40)
        if c == 27:  # ‘27’代表 exit 退出的意思
            break

# 分离 3 通道
def channel_split(image):
    b, g, r = cv.split(image)
    cv.imshow('Blue', b)
    cv.imshow('Green', g)
    cv.imshow('Red', r)
    return b, g, r


# 合并 3 通道
def merge(r, g, b):
    img = cv.merge([r, g, b])
    cv.imshow('Merge image', img)


src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE)
cv.imshow('input image', src)
t1 = cv.getTickCount()
# inverse(src)
# color_space_demo(src)
# extrace_object_demo()
b, g, r = channel_split(src)
merge(b, g, r)
t2 = cv.getTickCount()

print('Cost Time: {:.4f} s'.format((t2-t1)/cv.getTickFrequency()))
cv.waitKey(0)
cv.destroyAllWindows()
```

  [1]: http://static.zybuluo.com/harrytsz/oyqqxyqdctcrk6r6mc51svyy/image_1dd5fcsqblcuihj1qrn4fp3u62i.png
  [2]: http://static.zybuluo.com/harrytsz/4407cvr51wcyactcbu4jin1m/image_1dd5fev8qbpjuep1584dmobr3f.png
  [3]: http://static.zybuluo.com/harrytsz/8qq5ntirzke157dxk0a4rv6c/image_1dd5fiqpo1mlfap417d14rq1pe95s.png
  [4]: http://static.zybuluo.com/harrytsz/7fswa62xwn9o7n1q3d8qe34f/image_1dd5g57smg0j1gq81akf1nso65a69.png
