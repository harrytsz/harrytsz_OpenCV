# Lesson 00: 概述与环境搭建

```cmd
pip install opencv-python
```

**OpenCV 模块图：**    

![2019-06-12_132838.png-236.8kB][1]

**Demo00:**   
从本地读取图片

```python
import cv2 as cv

src = cv.imread('D:/images/demo.png')
cv.namedWindow('input image', cv.WINDOW_AUTOSIZE)
cv.imshow('input image', src)
cv.destroyAllWindows()

```


  [1]: http://static.zybuluo.com/harrytsz/50k6l3cex9v7y29rtrqbgg97/2019-06-12_132838.png
