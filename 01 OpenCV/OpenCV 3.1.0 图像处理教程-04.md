### OpenCV 3.1.0 图像处理教程-04

### 四、图像操作

#### 1.读写图像

**1.1** imread可以指定加载为灰度或者RGB图像；

**1.2** imwrite保存图像文件，类型由扩展名决定；



#### 2.读写像素

**2.1** 读一个GRAY像素点的像素值(CV_8UC1)

Scalar intensity = img.at<uchar>(y, x);

Scalar intensity = img.at<uchar>(Point(x, y));

**2.2** 读一个RGB像素点的像素值

Vec3f intensity = img.at<Vec3f>(y, x);

float blue = intensity.val[0];

float green = intensity.val[1];

float red = intensity.val[2];



#### 3.修改像素值

**3.1** 灰度图像

img.at<uchar>(y, x) = 128;

**3.2** RGB三通道图像

img.at<Vec3b>(y, x)[0] = 128; //blue

img.at<Vec3b>(y, x)[1] = 128; //green

img.at<Vec3b>(y, x)[2] = 128; //red

**3.3** 空白图像赋值

img = Scalar(0);

**3.4** ROI选择

Rect r(10, 10, 100, 100);

Mat samllImg = img(r);



#### 4.Vec3b和Vec3F

**4.1** Vec3b对应三通道的顺序是blue， green， red的uchar类型数据；

**4.2** Vec3f对应三通道的float类型数据

**4.3** 把CV_8UC1转换到CV_32F1实现如下：src.convetTo(dst, CV_32F);



#### 5.代码

```c++
#include <opencv2/core/core.hpp> 
#include <opencv2/imgcodecs.hpp> 
#include <opencv2/opencv.hpp>
#include <opencv2/highgui/highgui.hpp>
#include <iostream>

using namespace cv;
using namespace std;
int main(int argc, char** args) {
	Mat image = imread("D:/test.jpg", IMREAD_COLOR);
	if (image.empty()) {
		cout << "could not find the image resource..." << std::endl;
		return -1;
	}

	Mat grayImg;
	cvtColor(image, grayImg, COLOR_BGR2GRAY);
	Mat sobelx; 
	Sobel(grayImg, sobelx, CV_32F, 1, 0);
	double minVal, maxVal;
	minMaxLoc(sobelx, &minVal, &maxVal); //find minimum and maximum intensities
	Mat draw;
	sobelx.convertTo(draw, CV_8U, 255.0 / (maxVal - minVal), -minVal * 255.0 / (maxVal - minVal));
	/*
	int height = image.rows;
	int width = image.cols;
	int channels = image.channels();
	printf("height=%d width=%d channels=%d", height, width, channels);
	for (int row = 0; row < height; row++) {
		for (int col = 0; col < width; col++) {
			if (channels == 3) {
				image.at<Vec3b>(row, col)[0] = 0; // blue
				image.at<Vec3b>(row, col)[1] = 0; // green
			}
		}
	}
	*/
	namedWindow("My Image", CV_WINDOW_AUTOSIZE);
	imshow("My Image", draw);
	waitKey(0);
	return 0;
}
```

