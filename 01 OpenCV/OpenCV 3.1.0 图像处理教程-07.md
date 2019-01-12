## OpenCV 3.1.0 图像处理教程-07

### 七、绘制形状和文字

#### 1.使用cv::Point与cv::Scalar

**1.1** Point表示2D平面上的一个点(x, y)

Point p; 		p.x = 10;		p.y = 8;

Point p = Point(10, 8);

**1.2** Scalar表示4个元素的向量

Scalar(a, b, c); //a = blue, b = green, c = red表示RGB三个通道



#### 2.绘制线、矩形、圆、椭圆等基本几何体形状

**2.1** 画线cv::line (LINE_4/LINE_8/LINE_AA)

**2.2** 画椭圆cv::ellipse

**2.3** 画矩形cv::rectangle

**2.4** 画圆cv::circle

**2.5** 画填充cv::fillPoly



#### 3.随机数生成cv::RNG

**3.1** 生成高斯随机数guassian(double sigma)

**3.2** 生成正态分布随机数uniform(int a, int b)



#### 4.绘制添加文字

**4.1**putText函数中设置fontFace(cv::HersheyFonts)

* fontFace, CV_FONT_HERSHEY_PLAIN
* fontScale, 1.0, 2.0~8.0

#### 5.代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace std;
using namespace cv;

Mat bgImage;
const char* drawdemo_win = "draw shapes and text demo";
void MyLines();
void MyRectangle();
void MyEllipse();
void MyCircle();
void MyPolygon();
void RandomLineDemo();

int main(int argc, char** argv) {
	bgImage = imread("D:/vcprojects/images/test1.png");
	if (!bgImage.data) {
		printf("could not load image...\n");
		return -1;
	}
	//MyLines();
	//MyRectangle();
	//MyEllipse();
	//MyCircle();
	//MyPolygon();

	//putText(bgImage, "Hello OpenCV", Point(300, 300), CV_FONT_HERSHEY_COMPLEX, 1.0, Scalar(12, 23, 200), 3, 8);
	//namedWindow(drawdemo_win, CV_WINDOW_AUTOSIZE);
	//imshow(drawdemo_win, bgImage);

	RandomLineDemo();
	waitKey(0);
	return 0;
}

void MyLines() {
	Point p1 = Point(20, 30);
	Point p2;
	p2.x = 400;
	p2.y = 400;
	Scalar color = Scalar(0, 0, 255);
	line(bgImage, p1, p2, color, 1, LINE_AA);
}

void MyRectangle() {
	Rect rect = Rect(200, 100, 300, 300);
	Scalar color = Scalar(255, 0, 0);
	rectangle(bgImage, rect, color, 2, LINE_8);
}

void MyEllipse() {
	Scalar color = Scalar(0, 255, 0);
	ellipse(bgImage, Point(bgImage.cols / 2, bgImage.rows / 2), Size(bgImage.cols / 4, bgImage.rows / 8), 90, 0, 360, color, 2, LINE_8);
}

void MyCircle() {
	Scalar color = Scalar(0, 255, 255);
	Point center = Point(bgImage.cols / 2, bgImage.rows / 2);
	circle(bgImage, center, 150, color, 2, 8);
}

void MyPolygon() {
	Point pts[1][5];
	pts[0][0] = Point(100, 100);
	pts[0][1] = Point(100, 200);
	pts[0][2] = Point(200, 200);
	pts[0][3] = Point(200, 100);
	pts[0][4] = Point(100, 100);

	const Point* ppts[] = { pts[0] };
	int npt[] = { 5 };
	Scalar color = Scalar(255, 12, 255);

	fillPoly(bgImage, ppts, npt, 1, color, 8);
}

void RandomLineDemo() {
	RNG rng(12345);
	Point pt1;
	Point pt2;
	Mat bg = Mat::zeros(bgImage.size(), bgImage.type());
	namedWindow("random line demo", CV_WINDOW_AUTOSIZE);
	for (int i = 0; i < 100000; i++) {
		pt1.x = rng.uniform(0, bgImage.cols);
		pt2.x = rng.uniform(0, bgImage.cols);
		pt1.y = rng.uniform(0, bgImage.rows);
		pt2.y = rng.uniform(0, bgImage.rows);
		Scalar color = Scalar(rng.uniform(0, 255), rng.uniform(0, 255), rng.uniform(0, 255));
		if (waitKey(50) > 0) {
			break;
		}
		line(bg, pt1, pt2, color, 1, 8);
		imshow("random line demo", bg);
	}
}
```

