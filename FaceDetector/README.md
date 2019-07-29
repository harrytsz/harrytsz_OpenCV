## OpenCV人脸检测-Haar级联和LBP

**人脸检测-Haar级联**

**概述**

✔️ Haar 级联检测器，OpenCV 中的 Haar 级联检测器支持人脸检测、微笑、眼睛与嘴巴检测等，通过加载这些预先训练的 Haar 模型数据可以实现相关的对象检测。

**Haar特征**

✔️ Haar 小波基函数，因为其满足对称性，因此对人脸这种生物对称性良好的对象特别适合用来做检测。

![image_1dgu20f2oj22jkm8ut1r4krv3p.png-109.6kB][1]
小波函数

常见的Haar特征分为三类：

- 边缘特征；
- 线性特征；
- 中心特征和对角线特征。

**Haar特征**

✔️ 不同特征可以进行多种组合，生成更加复杂的级联特征，特征模板内有白色和黑色两种矩形，并定义该模板的特征值为白色矩形像素和减去黑色矩形像素和，Haar特征值反映了图像的对比度与梯度变化。

**级联**

✔️ 级联分类器相当于一个决策树，层级判断，更加准确。

✔️ 训练的时候用的照片一般都是25*25左右的小图片，所以对于大的人脸，还需要进行多尺度的检测，多尺度检测机制一般有两种策略： - 不改变搜索窗口的大小，而不断缩放图片，这种方法显然需要对每个缩放后的图片进行区域特征值的运算，类似于制作图像金字塔的流程，效率不高； - 不断初始化搜索窗口size为训练时的图片大小，不断扩大搜索窗口，进行搜索，解决了第一种方法的弱势。

✔️ OpenCV中HAAR特征计算是积分图技术，可以非常快速高效的开窗检测， HAAR级联检测器具备有如下特性： - 高类间变异性 - 低类内变异性 - 局部强度差 - 不同尺度 - 计算效率高

![image_1dgu24onh1ftab741llifq31euq16.png-208kB][2]
级联判断

**函数**

预先定义加载face(eye,smile,etc)分类器。
```python
face = face_detector.detectMultiScale(image, scaleFactor, minNeighbors, minSize, maxSize)
```

**输入**

- Image --> 输入图像
- ScaleFactor --> 放缩比率, default为1.1
- minNeighbors --> 表示最低相邻矩形框, - - default为3
- minSize --> 可以检测的最小人脸
- maxSize --> 可以检测的最大人脸

**输出**

- face --> 人脸的位置 (x, y, w, h)

**代码示例**

### 1.人脸检测
```python
import cv2 as cv

face_detector = cv.CascadeClassifier(cv.data.haarcascades + "haarcascade_frontalface_alt2.xml")

image = cv.imread('people.jpg')
faces = face_detector.detectMultiScale(image, scaleFactor=1.05, minNeighbors=1, 
                                       minSize=(100, 100), maxSize=(500, 500))
for x, y, width, height in faces:
    cv.rectangle(image, (x, y), (x+width, y+height), (0, 0, 255), 2, cv.LINE_8, 0)
cv.imshow("faces", image)
cv.imwrite('face.jpg', image)
cv.waitKey(0)
cv.destroyAllWindows()
```
![image_1dgu29i911gor1bierphn8118bi1j.png-174.6kB][3]
人脸检测

### 2.眼睛检测
```python
eye_detector = cv.CascadeClassifier(cv.data.haarcascades + "haarcascade_eye.xml")

# 基于前面的人脸检测
for x, y, width, height in faces:
    cv.rectangle(image, (x, y), (x+width, y+height), (0, 0, 255), 2, cv.LINE_8, 0)

    # 提取人脸
    roi = image[y:y+height,x:x+width]
    # 检测人眼
    eyes = eye_detector.detectMultiScale(roi, scaleFactor=1.1, minNeighbors=5,
                                         minSize=(20, 20), maxSize=(30,30))
    # 绘制人眼                                  
    for ex, ey, ew, eh in eyes:
        cv.rectangle(roi, (ex, ey), (ex + ew, ey + eh), (255, 0, 0), 2)
```
![image_1dgu2akmk1n5q4lv116b1n6f1pf720.png-178.4kB][4]
眼睛检测

### 3.微笑检测
```python
smile_detector = cv.CascadeClassifier(cv.data.haarcascades + "haarcascade_smile.xml")

# 基于前面的人脸检测
for x, y, width, height in faces:
    cv.rectangle(image, (x, y), (x+width, y+height), (0, 0, 255), 2, cv.LINE_8, 0)

    # 提取人脸
    roi = image[y:y+height,x:x+width]
    # 检测微笑
    smiles = smile_detector.detectMultiScale(roi, scaleFactor=1.05, minNeighbors=2,
                                             minSize=(20, 20))
    # 绘制微笑框                                
    for sx, sy, sw, sh in smiles:
        cv.rectangle(roi, (sx, sy), (sx + sw, sy + sh), (0, 255, 0), 2)
        cv.putText(image, 'Smile', (x+10, y-7), cv.FONT_HERSHEY_SIMPLEX, 0.8, (0, 255, 0), 2)
```
![image_1dgu2bilt19du11pv1iopfvj92a2d.png-184.5kB][5]
微笑检测

###  4.人脸检测-LBP

**概述**

LBP局部二值模式(Local Binary Pattern)主要用来实现2D图像纹理分析。

基本思想：
用每个像素跟它周围的像素相比较得到局部图像结构，假设中心像素值大于相邻像素值则则相邻像素点赋值为1，否则赋值为0，最终对每个像素点都会得到一个二进制八位的表示，比如11100111。假设3x3的窗口大小，这样对每个像素点来说组合得到的像素值的空间为[0~2^8]。
![image_1dgu2crjka5eic7139i1ch412ig2q.png-92.4kB][6]
LBP局部二值

**LBP算子：**

OpenCV内使用的圆形LBP算子，由于使用圆形来作为窗口，不能保证所有像素点都落在圆内，因此需要使用插值算法对没有完全落在像素位置点的点进行计算灰度值，opencv使用的就是双线性插值算法。

LBP模式：

![image_1dgu2dpvdg2v1fipu30t0bhi637.png-62.1kB][7]
LBP模式

**优势：**

OpenCV中使用LBP特征数据检测人脸比使用Haaris数据要快，原因在于LBP特征不会产生小数数据，避免了浮点数计算开销。

**函数和示例**

LBP的函数调用和HAAR级联一样，只是加载的xml不同。 python代码：

```python
# 加载LBP算法
detector = cv.CascadeClassifier("lbpcascade_frontalface_improved.xml")
image = cv.imread('people.jpg')
# 检测人脸
faces = detector.detectMultiScale(image, scaleFactor=1.05, minNeighbors=1,
                                  minSize=(30, 30), maxSize=(200, 200))
# 绘制矩形框                            
for x, y, width, height in faces:
    cv.rectangle(image, (x, y), (x+width, y+height), (0, 0, 255), 2, cv.LINE_8, 0)
```
![image_1dgu2eu411arc1dde6irpolllj3k.png-171.5kB][8]
LBP人脸检测

https://github.com/JimmyHHua/opencv_tutorials.github.com

**参考**

👍👍👍- OpenCV Tutorial 官网

👍👍👍- cv2级联分类器CascadeClassifier

👍👍👍- 基于Haar特征的Adaboost级联人脸检测分类器

👍👍👍-详解LBP特征与应用(人脸识别)


  [1]: http://static.zybuluo.com/harrytsz/3e5nawfcw2l86yc1oa1901o6/image_1dgu20f2oj22jkm8ut1r4krv3p.png
  [2]: http://static.zybuluo.com/harrytsz/fpb7qc9zry79tgt6c35zt202/image_1dgu24onh1ftab741llifq31euq16.png
  [3]: http://static.zybuluo.com/harrytsz/nx1qgwi6njdkduffpqo7uzsr/image_1dgu29i911gor1bierphn8118bi1j.png
  [4]: http://static.zybuluo.com/harrytsz/jvibtw5ra7qsy3vjfceooknx/image_1dgu2akmk1n5q4lv116b1n6f1pf720.png
  [5]: http://static.zybuluo.com/harrytsz/i83m25o9qqzc1xyontj7i86h/image_1dgu2bilt19du11pv1iopfvj92a2d.png
  [6]: http://static.zybuluo.com/harrytsz/smn2mmdn7dq6z39lvhc5s5wg/image_1dgu2crjka5eic7139i1ch412ig2q.png
  [7]: http://static.zybuluo.com/harrytsz/v1dnx769d678bm9xvsj4fs32/image_1dgu2dpvdg2v1fipu30t0bhi637.png
  [8]: http://static.zybuluo.com/harrytsz/ggcwh1tdsbdpx4a8w98w444k/image_1dgu2eu411arc1dde6irpolllj3k.png

