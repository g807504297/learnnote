# 图像梯度  
* 参考链接  
https://blog.csdn.net/poem_qianmo/article/details/25560901  
https://blog.csdn.net/saltriver/article/details/78987096  
https://zhuanlan.zhihu.com/p/47017516  
https://www.aiuai.cn/aifarm482.html  
https://www.jianshu.com/p/effb2371ea12  



## 基本原理
### 关于边缘检测  
具体步骤：
- (1)滤波：边缘能检测的算法主要是基于图像强度的一阶和二阶导数，但导数通常对噪声很敏感，因此必须采用滤波器来改善与噪声有挂你的边缘检测器的性能。常见的滤波方法主要有高斯滤波，即采用离散化的高斯函数产生一组归一化的高斯核，然后基于高斯核函数，对图像灰度矩阵的每一点进行加权求和。
- (2)增强：增强边缘的基础是确定图像各点邻域强度的变化值。增强算法可以将图像灰度点邻域强度值有显著变换的点凸显出来。在具体编程实现时，可通过计算梯度值来确定。
- (3)检测：经过增强的图像，往往邻域中有很多店的梯度值比较大，而在特定的应用中，这些点并不是我们要找的边缘点，所以应该采用某种方法来对这些点进行取舍。实际工程中，常用的方法是通过"阈值化"方法来检测

### Canny算子的步骤  
1.将彩色图像转化为灰度图  
2.使用高斯滤波器平滑图像  
3.计算图像梯度的赋值和方向  
4.对梯度值进行非极大值抑制  
5.使用双阈值进行边缘的检测和链接  

### sobel边缘检测算法  
索贝儿算子主要用作边缘检测，在技术上，它是离散性差分算子，用来运算图像亮度函数的灰度之近似值。在图像的任何一点使用此算子，将会产生对应的灰度矢量。
### Laplace边缘检测算法  
- 引言  
图像锐化处理的作用是使灰度反差增强，从而使模糊图像变得更清晰。图形模糊的实质就是图像受到平均运算或者积分运算，因此可以对图像进行逆运算，如微分运算能够突出图像的细节，使图像变得更清晰。由于拉普拉斯算子是一种微分算子，他的应用可以增强图像中灰度突变的区域，减弱灰度缓慢变化的区域。  
- 基本理论
拉普拉斯算子是最简单的各向同性的微分算子，具有旋转不变性。一个二维图像函数的拉普拉斯是各向同性的二阶导数。
它是n维欧几里得空间中的一个二阶微分算子，定义为梯度grad()的三度div()。

## 相关API  

### cv2.canny()函数
```py
edge = cv2.Canny(image, threshold1, threshold2[, edges[, apertureSize[, L2gradient ]]])
```
- image:输入图片，必须为单通道的灰度图
- threshold1:阈值minVal
- threshold2：阈值maxVal
- apertureSize:用于计算图片提取的Sobel kernel尺寸，默认为3.
- L2gradient:指定计算梯度的等式。若为True，会用到sqrt(Gx^2 + Gy^2);若为False，会用到|Gx| + |Gy|，后者计算量会小很多

### cv.Sobel()函数  
```py
dst = cv2.Sobel(src, ddepth, dx, dy[, dst[, ksize[, scale[, delta[, borderType]]]]])
```
前四个是必须的参数： 
- src：输入图片
- ddepth:图像的深度，-1表示采用的是与原图像相同的深度。目标图像的深度必须大于等于原图像的深度； 
- dx和dy：求导的阶数，一般为0、1、2；其中0表示这个方向没有求导，
可选参数：
- dest:
- ksize是Sobel算子的大小，必须为1,3,5,7
- scale:缩放导数的比例常数，默认情况下没有伸缩系数
- delta:可选增量，将会加到最终的dst中，默认，没有额外值加到dst中
- borderType:判断图像边界的模式，默认值为cv2.BORDER_DEFAULT    

### cv.Laplacian()函数  
```py
dst = cv2.Laplacian(src, ddepth[, dst[, ksize[, scale[, delta[, borderType]]]]])
``` 
-src:原图像
-ddepth:图像的深度
【可选参数】
- dst：目标图像
- ksize:算子的大小，必须为1,3,5,7。默认为1
- scale:是缩放导数的比例常数，默认情况下没有伸缩系数
- delta:是一个可选的增量，将会加到最终的dst中，默认没有额外值加入
- borderType:是判断图像边界的模式。默认值为cv2.BORDER_DEFAULT
  

## 测试  
```py
import cv2 as cv

#图像梯度，索贝尔算子
def sobel_image(image):
    grad_x = cv.Sobel(image,cv.CV_32F,1,0)
    grad_y = cv.Sobel(image,cv.CV_32F,0,1)
    gradx = cv.convertScaleAbs(grad_x)
    grady = cv.convertScaleAbs(grad_y)

    cv.imshow("x1",gradx)#颜色变化在水平分层
    cv.imshow("y1",grady)#颜色变化在垂直分层

    gradxy = cv.addWeighted(gradx,0.5,grady,0.5,0)
    cv.imshow('add1',gradxy)
# scharr算子：增强边缘
def schar_image(image):
    grad_x = cv.Scharr(image,cv.CV_32F,1,0)#x方向导数
    grad_y = cv.Scharr(image,cv.CV_32F,0,1)#y方向导数
    gradx = cv.convertScaleAbs(grad_x)
    grady = cv.convertScaleAbs(grad_y)
    cv.imshow("x2",gradx)#颜色变化在水平分层
    cv.imshow("y2",grady)#颜色变化在垂直分层
    gradxy = cv.addWeighted(grad_x,0.5,grad_y,0.5,0)
    cv.imshow("add2",gradxy)
# 拉普拉斯算子
def lapalian_image(image):
    dst = cv.Laplacian(image,cv.CV_32F)
    lpls = cv.convertScaleAbs(dst)
    cv.imshow("lapalian",lpls)

src = cv.imread("e:/opencv_work/source/wujing.jpg")
cv.imshow('wujng.jpg',src)
#sobel_image(src)
#schar_image(src)
lapalian_image(src)
cv.waitKey(0)
cv.destroyAllWindows()
```
## 测试结果
- schar算子测试  
![20190722143241.png](https://i.loli.net/2019/07/22/5d3558883e63216875.png)  
- 索贝尔算子  
![20190722143445.png](https://i.loli.net/2019/07/22/5d3559037c5d037440.png)  
- 拉普拉斯算子  
![20190722143537.png](https://i.loli.net/2019/07/22/5d3559370113c99287.png)