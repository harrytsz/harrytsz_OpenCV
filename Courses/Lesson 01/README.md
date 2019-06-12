# Lesson 01: 图像加载与保存

## 加载与保存
* 什么是图像?
* 加载与保存.

![image_1dd53as0e19541go21ikk1vtb6t2l.png-180.2kB][1]

图像在人类的眼中是具有高度语义信息的画面，但是图像在机器眼中则是一系列结构化的数据信息。

## 代码层面知识点
* OpenCV Python 中加载图像的模块与 API.
* 支持常见格式.

```python
"""
Lesson 01:  图像加载与保存
Created By : Harrytsz
Time: 2019-6-12
"""

import cv2 as cv
import numpy as np


# 获取图像信息
def get_image_info(image):
    print(type(image))
    print(image.shape)
    print(image.size)
    print(image.dtype)
    pixel_data = np.array(image)
    print(pixel_data)


# 获取视频
def get_video_demo():
    capture = cv.VideoCapture(0)
    while True:
        ret, frame = capture.read()
        frame = cv.flip(frame, 1) # 画面镜像翻转
        cv.imshow('video', frame)
        c = cv.waitKey(50)
        if c == 27:
            break


# 加载图像或视频
src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE)
cv.imshow('input image', src)

############# 获取图像
get_image_info(src)
gray = cv.cvtColor(src, cv.COLOR_BGR2GRAY) # 保存为灰度图
cv.imwrite('D:/images/result.png', gray)
cv.waitKey(0)

############# 获取视频
# get_video_demo()

cv.destroyAllWindows()
```

  [1]: http://static.zybuluo.com/harrytsz/e8vixm87hziuq4ovjumyiq18/image_1dd53as0e19541go21ikk1vtb6t2l.png
