## OpenCV 3.1.0 图像处理教程-08

### 八、图像模糊

#### 1.模糊原理

**1.1**  Smooth/Blur

* 图像处理中最简单和常用的操作之一

* 使用该操作的原因之一是为了给图像预处理时减少噪声

* 使用Smooth/Blur操作其背后是数学的卷积计算
![](https://latex.codecogs.com/gif.latex?g(i,&space;j)&space;=&space;\sum_{k,&space;l}f(i&plus;k,&space;j&plus;l)h(k,&space;l))
  $$
  g(i, j) = \sum_{k, l}f(i+k, j+l)h(k, l)
  $$

* 通常这些卷积算子计算都是线性操作，所以又叫线性滤波

**1.2** 归一化盒子滤波（均值滤波）
$$
K = \frac{1}{K_{width}·K_{height}}\left[
\begin{matrix}
1 & 1 & ... & 1 \\
1 & 1 & ... & 1 \\
. & . & ... & . \\
1 & 1 & ... & 1 
\end{matrix} \right]
$$
**1.3** 高斯滤波
$$
G_0(x, y) = Ae^{\frac{-(x-\mu_x)^2}{2\sigma_x^2}+\frac{-(y-\mu_y)^2}{2\sigma_y^2}}
$$


#### 2.相关API

**2.1** 均值滤波

blur(Mat src, Mat dst, Size(xradius, yradius), Point(-1, -1));

**2.2** 高斯模糊

GaussianBlur(Mat src, Mat dst, Size(11, 11), sigmax, sigmay);



#### 3.代码一

```c++
#include <opencv2/opencv.hpp> 
#include <iostream> 
using namespace cv;

int main(int argc, char** argv) {
	Mat src, dst;
	src = imread("D:/vcprojects/images/test.png");
	if (!src.data) {
		printf("could not load image...\n");
		return -1;
	}
	char input_title[] = "input image";
	char output_title[] = "blur image";
	namedWindow(input_title, CV_WINDOW_AUTOSIZE);
	namedWindow(output_title, CV_WINDOW_AUTOSIZE);
	imshow(input_title, src);

	blur(src, dst, Size(11, 11), Point(-1, -1));
	imshow(output_title, dst);

	Mat gblur;
	GaussianBlur(src, gblur, Size(11, 11), 11, 11);
	imshow("gaussian blur", gblur);

	waitKey(0);
	return 0;
}
```



#### 4.中值滤波

**4.1** 统计排序滤波器

**4.2** 中值滤波对椒盐噪声有很好的抑制作用

![1547276176614](https://github.com/Zdafeng/learning-opencv/blob/master/01%20OpenCV/img/8.1.png)

#### 5.双边滤波

**5.1** 均值模糊无法克服边缘像素信息丢失的缺陷。原因是均值滤波是基于平均权重；

**5.2** 高斯模糊部分克服了该缺陷，但是无法完全避免，因为没有考虑像素值的不同；

**5.3** 高斯双边模糊是边缘保留的滤波方法，避免了边缘信息的丢失，保留了图像轮廓不变；

![1547276452134](https://github.com/Zdafeng/learning-opencv/blob/master/01%20OpenCV/img/8.2.png)

#### 6.相关API

**6.1** 中值滤波

medianBlur(Mat src, Mat dst, ksize);

* ksize的大小必须大于1，且必须是奇数；

**6.2** 双边滤波

bilateralFilter(src, dst, d = 15, 150, 3);

* -15：d 计算的半径，半径之内的像素都会被纳入计算；如果提供 -1，则根据sigma space参数取值；
* -150：sigma color 决定多少差值之内的像素会被计算；
* -3：sigma space 如果d的值大于0，则声明无效；否则d的值根据sigma space来计算；



#### 7.代码二

```C++
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace std;
using namespace cv;

int main(int argc, char** argv){
    Mat src, dst1, dst2, dst3;
    src = imread("test.jpg", 1);
    if(!src.data){
        cout << "could not load image .." << endl;
        return -1;
    }
    nameWindow("input image", CV_WINDOW_AUTOSIZE);
    imshow("input image", src);
    
    medianBlur(src, dst1, 3);
    nameWindow("medianBlur image", CV_WINDOW_AUTOSIZE);
    imshow("medianBlur image", dst1);
    
    bilateraFilter(src, dst2, 15, 150, 3);
    nameWindow("bilateraFilter image", CV_WINDOW_AUTOSIZE);
    imshow("bilateraFilter image", dst2);
    
    Mat kernel = (Mat_<int>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
    filter2D(src, dst3, -1, kernel, Point(-1, -1), 0);
    nameWindow("filter2D image", CV_WINDOW_AUTOSIZE);
    imshow("filter2D image", dst2);
    
    waitKey(0);
    return 0;
}
```

