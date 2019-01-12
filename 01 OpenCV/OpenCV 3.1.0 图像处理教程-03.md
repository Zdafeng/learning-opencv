## OpenCV 3.1.0 图像处理教程-03

### 三、Mat对象

#### 1.Mat对象与IplImage对象

**1.1** Mat对象是OpenCV2.0以后引进的图像数据结构，自动分配内存、不存在内存泄漏的问题，是面向对象的数据结构；

**1.2** Mat对象分为两个部分，头部和数据部分；

**1.3** IplImage是从2001年OpenCV发布之后就一直存在，是C语言风格的数据结构，需要开发者自己分配和管理内存，对大的程序使用它容易导致内存泄漏问题；



#### 2.Mat对象构造函数与常用方法

**2.1** 构造函数

Mat()

Mat(int rows, int cols, int type)

Mat(Size size, int type)

Mat(int rows, int cols, int type, const Scalar &s)

Mat(int ndims, const int *sizes, int type)

Mat(int ndims, const *sizes, int type, const Scalar &s)

**2.2** 常用方法

void copyTo(Mat mat)

**void** cinvertTo(Mat dst, int type)

Mat clone()

int channels()

int depth()

bool empty()

uchar* ptr(i=0)



#### 3.Mat对象使用

**3.1** 部分复制：一般情况下只会复制Mat对象的头和指针部分，不会复制数据部分；

​	Mat A = imread(imgFilePath);

​	Mat B(A);  //部分复制

**3.2** 完全复制：如果想把Mat对象的头和数据部分一起复制，可以通过如下两个API实现；

​	Mat F = A.colne();

​	Mat G; A.copyTo(G);



#### 4.Mat对象使用四个要点

* 使用图像的内存是自动分配的
* 使用OpenCV的C++接口，不需要考虑内存分配问题
* 赋值操作和拷贝构造函数只会复制头部分
* 使用clone和copyTo两个函数实现数据完全复制



#### 5.Mat对象创建

**5.1** cv::Mat::Mat构造函数

Mat M(2, 2, CV_8UC3, Scalar(0, 0, 255));

* 前两个参数分别表示行(row)和列(column)；
* 第三个参数CV_8UC3中8表示每个通道占8位，U表示无符号，C表示Char类型，3表示通道数目为3；
* 第四个参数是向量便是初始化每个像素值使多少，向量长度对应通道数一致；

**5.2** 创建多维数组cv::Mat::create

int sz[3] = {2, 2, 2};

Mat L(3, sz, CV_8UC1, Scalar::all(0));

**5.3** cv::Mat::create实现

Mat M;

M.create(4, 3, CV_8UC2);

M = Scalar(127, 127);

cout << "M = " << endl << "" << M << endl << endl;

uchar* firstRow = M.prt<uchar>(0);

printf("%d", *firstRow);

**5.4** 定义小组数

Mat C = (Mat_<double>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);

cout << "C=" << endl << "" << C << endl << endl;

#### 6.代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace std;
using namespace cv;

int main(int argc, char** argv) {
	Mat src;
	src = imread("D:/vcprojects/images/test.png");
	if (src.empty()) {
		cout << "could not load image..." << endl;
		return -1;
	}
	namedWindow("input", CV_WINDOW_AUTOSIZE);
	imshow("input", src);

	/*Mat dst;
	dst = Mat(src.size(), src.type());
	dst = Scalar(127, 0, 255);
	namedWindow("output", CV_WINDOW_AUTOSIZE);
	imshow("output", dst);*/
	Mat dst;
	//src.copyTo(dst);
	namedWindow("output", CV_WINDOW_AUTOSIZE);

	cvtColor(src, dst, CV_BGR2GRAY);
	printf("input image channels : %d\n", src.channels());
	printf("output image channels : %d\n", dst.channels());

	int cols = dst.cols;
	int rows = dst.rows;

	printf("rows : %d cols : %d\n", rows, cols);
	const uchar* firstRow = dst.ptr<uchar>(0);
	printf("fist pixel value : %d\n", *firstRow);

	Mat M(100, 100, CV_8UC1, Scalar(127));
	//cout << "M =" << endl << M << endl;

	Mat m1;
	m1.create(src.size(), src.type());
	m1 = Scalar(0, 0, 255);

	Mat csrc;
	Mat kernel = (Mat_<char>(3, 3) << 0, -1, 0, -1, 5, -1, 0, -1, 0);
	filter2D(src, csrc, -1, kernel);

	Mat m2 = Mat::eye(2, 2, CV_8UC1);
	cout << "m2 =" << endl << m2 << endl;

	imshow("output", m2);
	waitKey(0);
	return 0;
}
```

