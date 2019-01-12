## OpenCV 3.1.0 图像处理教程-02

### 二、矩阵的掩膜操作

#### 1.获取图像像素指针

**1.1** CV_Assert(myImage.depth() == CV_8U);

**1.2** Mat.ptr<uchar>(int i = 0)获取像素矩阵的指针，索引i表示第几行，从0开始计行数；

**1.3** 获取当前行指针 const uchar* current = myImage.ptr<uchar>(row);

**1.4** 获取当前像素点p(row, col)的像素值p(row, col) = current[col]; 



#### 2.像素范围处理saturate_cast<uchar>

saturate_cast<uchar>的功能是：确保RGB值的范围在0~255之间

* saturate_cast<uchar> (-100), 返回0；
* saturate_cast<uchar> (200), 返回255；
* saturate_cast<uchar> (100), 返回100；



#### 3.掩膜操作实现图像对比度调整

**3.1** 矩阵的掩膜操作十分简单，根据掩膜来重新计算每个像素的像素值，掩膜（mask，kernel）

**3.2** 通过掩膜操作实现图像对比度提高
$
\left[
 \begin{matrix}
   0 & -1 & 0 \\
   -1 & 5 & -1 \\
   0 & -1 & 0
  \end{matrix}
  \right]
$

$
I(i, j) = 5 * I(i, j) - [I(i-1, j) + I(i+1, j) + I(i, j+1) + I(i, j-1)]
$

#### 4.函数调用filter2D功能

**4.1** 定义掩膜：Mat kernel = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);

**4.2** filter2D(src, dst, src.depth(), kernel);

* src和dst是Mat类型变量；
* src.depth表示位图深度，有32， 24， 8等

#### 5.代码

**5.1** 代码一

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
#include <math.h>

using namespace cv;

int main(int argc, char** argv) {
	Mat src, dst;
	src = imread("D:/vcprojects/images/test.png");
	if (!src.data) {
		printf("could not load image...\n");
		return -1;
	}
	namedWindow("input image", CV_WINDOW_AUTOSIZE);
	imshow("input image", src);
	
	/*
	int cols = (src.cols-1) * src.channels();
	int offsetx = src.channels();
	int rows = src.rows;

	dst = Mat::zeros(src.size(), src.type());
	for (int row = 1; row < (rows - 1); row++) {
		const uchar* previous = src.ptr<uchar>(row - 1);
		const uchar* current = src.ptr<uchar>(row);
		const uchar* next = src.ptr<uchar>(row + 1);
		uchar* output = dst.ptr<uchar>(row);
		for (int col = offsetx; col < cols; col++) {
			output[col] = saturate_cast<uchar>(5 * current[col] - (current[col- offsetx] + current[col+ offsetx] + previous[col] + next[col]));
		}
	}
	*/
	double t = getTickCount();
	Mat kernel = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
	filter2D(src, dst, src.depth(), kernel);
	double timeconsume = (getTickCount() - t) / getTickFrequency();
	printf("tim consume %.2f\n", timeconsume);

	namedWindow("contrast image demo", CV_WINDOW_AUTOSIZE);
	imshow("contrast image demo", dst);

	waitKey(0);
	return 0;
}
```

**5.2** 代码二

```c++
#include<opencv2\opencv.hpp>
#include<highgui.h>
using namespace cv;
int main(int argc, char** argv)
{
	// 加载图像
	Mat testImage = imread("D:/test.jpg");
	CV_Assert(testImage.depth() == CV_8U);
	namedWindow("test_image", CV_WINDOW_AUTOSIZE);
	imshow("test_image", testImage);

	// 使用Filter2D函数
	Mat result;
	Mat kern = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
	filter2D(testImage, result, testImage.depth(), kern);

	// 显示结果
	namedWindow("result_image", CV_WINDOW_AUTOSIZE);
	imshow("result_image", result);
	waitKey(0);
	return 0;
}
```

