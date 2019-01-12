## OpenCV 3.1.0 图像处理教程-01

### 一、加载、修改、保存图像

#### 1. 加载图像（cv::imread）

**1.1** imread的功能是：加载图像文件成为一个Mat对象

第一个参数表示图像文件名称；

第二个参数表示图像的类型，支持常见的三个参数值：

* IMREAD_UNCHANGED(<0)表示加载原图，不做任何改变；
* IMREAD_GRAYSCALE(0)表示把原图作为灰度图像加载进来；
* IMREAD_COLOR(>0)表示把原图作为RGB图像加载进来。

**1.2** OpenCV支持JPG, PNG, TIFF等常见格式图像文件加载



#### 2.显示图像（cv::namedWindow与cv::imshow）

**2.1** namedWindow的功能是：创建一个OpenCV窗口，它是由OpenCV自动创建与释放，无需人为销毁；

**2.2** 常见用法namedWindow("Window Title", WINDOW_AUTOSIZE);

* WINDOW_AUTOSIZE会自动根据图像大小，显示窗口大小，不能人为改变窗口大小；
* WINDOW_NORMAL允许修改窗口大小；

**2.3** imshow根据窗口名称显示图像到指定的窗口上去

* 第一个参数是窗口名称
* 第二个参数是Mat对象



#### 3.修改图像（cv::cvtColor）

**3.1** cvtColor的功能是：把图像从一个色彩空间转换到另一个色彩空间

* 第一个参数表示源图像；
* 第二个参数表示色彩空间转换后的图像；
* 第三个参数表示源和目标色彩空间，如：COLOR_BGR2HLS、COLOR_BGR2GRAY;

**3.2** cvtColor(image, gray_image, COLOR_BGR2GRAY);



#### 4.保存图像（cv::imwrite）

**4.1** imwrite的功能是：保存图像文件到指定目录；

**4.2** 只有8位、16位的PNG、JPG、TIFF文件格式而且是单通道或者三通道的BRG图像才能通过这种方式保存；

**4.3** 保存PNG格式的图像时可以保存透明通道的图片；

**4.4** 可以指定压缩参数

#### 5.代码

```c++
#include<opencv2\opencv.hpp>
#include<highgui.h>
using namespace cv;
int main(int argc, char** argv)
{
	// read image
	Mat image = imread("test.jpg");

	// 对图像进行所有像素用 （255- 像素值）
	Mat invertImage;
	image.copyTo(invertImage);

	// 获取图像宽、高
	int channels = image.channels();
	int rows = image.rows;
	int cols = image.cols * channels;
	if (image.isContinuous()) {
		cols *= rows;         
		rows = 1;
	}

	// 每个像素点的每个通道255取反
	uchar* p1;
	uchar* p2;
	for (int row = 0; row < rows; row++) {
		p1 = image.ptr<uchar>(row);// 获取像素指针
		p2 = invertImage.ptr<uchar>(row);
		for (int col = 0; col < cols; col++) {
			*p2 = 255 - *p1; // 取反
			p2++;
			p1++;
		}
	}

	// create windows
	namedWindow("My Test", CV_WINDOW_AUTOSIZE);
	namedWindow("My Invert Image", CV_WINDOW_AUTOSIZE);

	// display image
	imshow("My Test", image);
	imshow("My Invert Image", invertImage);

	// 关闭
	waitKey(0);
	destroyWindow("My Test");
	destroyWindow("My Invert Image");
	return 0;
}
```

