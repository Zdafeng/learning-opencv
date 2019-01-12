## OpenCV 3.1.0 图像处理教程-06

### 六、调整图像亮度和对比度

#### 1.理论

**1.1** 图像变换可以看作如下操作：

* 像素变换——点操作
* 邻域变换——区域操作

**1.2** 调整图像亮度和对比度属于像素变换——点操作
$$
g(i, j) = \alpha f(i, j)+\beta \quad其中\alpha>0,\beta 是增益变量
$$

#### 2.重要API

**2.1** Mat new_image = Mat::zeros(image.size(), image.type());

创建一张跟原图大小和类型一致的空白图像、像素值初始化为0；

**2.2** saturate_cast<uchar>(value);

确保像素值大小范围在0～255之间

**2.3** Mat.at<Vec3b>(y, x)[index] = valua;

给每个像素点每个通道赋值（rgb图像）



#### 3.代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>
using namespace std;
using namespace cv;
int main(int argc, char** argv) {
	Mat src, dst;
	src = imread("D:/vcprojects/images/test.png");
	if (!src.data) {
		printf("could not load image...\n");
		return -1;
	}
	char input_win[] = "input image";
	cvtColor(src, src, CV_BGR2GRAY);
	namedWindow(input_win, CV_WINDOW_AUTOSIZE);
	imshow(input_win, src);

	// contrast and brigthtness changes 
	int height = src.rows;
	int width = src.cols;
	dst = Mat::zeros(src.size(), src.type());
	float alpha = 1.2;
	float beta = 30;

	Mat m1;
	src.convertTo(m1, CV_32F);
	for (int row = 0; row < height; row++) {
		for (int col = 0; col < width; col++) {
			if (src.channels() == 3) {
				float b = m1.at<Vec3f>(row, col)[0];// blue
				float g = m1.at<Vec3f>(row, col)[1]; // green
				float r = m1.at<Vec3f>(row, col)[2]; // red

				// output
				dst.at<Vec3b>(row, col)[0] = saturate_cast<uchar>(b*alpha + beta);
				dst.at<Vec3b>(row, col)[1] = saturate_cast<uchar>(g*alpha + beta);
				dst.at<Vec3b>(row, col)[2] = saturate_cast<uchar>(r*alpha + beta);
			}
			else if (src.channels() == 1) {
				float v = src.at<uchar>(row, col);
				dst.at<uchar>(row, col) = saturate_cast<uchar>(v*alpha + beta);
			}
		}
	}

	char output_title[] = "contrast and brightness change demo";
	namedWindow(output_title, CV_WINDOW_AUTOSIZE);
	imshow(output_title, dst);

	waitKey(0);
	return 0;
}
```

