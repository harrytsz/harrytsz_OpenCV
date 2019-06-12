# Lesson 00: 概述与环境搭建

```cmd
pip install opencv-python
```

**Demo00:**       
```python
import cv2 as cv

src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE)
cv.imshow('input image', src)
cv.destroyAllWindows()

```
