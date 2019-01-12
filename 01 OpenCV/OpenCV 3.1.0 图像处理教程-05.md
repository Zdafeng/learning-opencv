## OpenCV 3.1.0 图像处理教程-05

### 五、图像混合

#### 1.理论-线性混合操作

$$
g(x) = (1 - \alpha)f_0(x)+\alpha f_1(x) \quad(0 \leq \alpha \leq 1)
$$

​	

#### 2.相关API(addWeighted)

| void cv::addWeighted(InputArray       src1,<br />                                           double              alpha,<br />                                           InputArray       src2,<br />                                           double               beta,<br />                                           double               gamma,<br />                                           OutputArray    dst,<br />                                            int                      dtype = -1) | 参数1：输入图像Mat src1<br />参数2：输入图像src1的alpha值<br />参数3：输入图像Mat src2<br />参数4：输入图像src2的beta值<br />参数5：gamma值<br />参数6：输出混合图像<br />参数7：默认类型 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **ps:**两张图像的大小和类型必须一致才可以                    |                                                              |

$$
dst(I) = sature(src1(I) * alpha + src2(I) * beta + gamma)
$$

#### 3.代码

```c++
#include <opencv2/opencv.hpp>
#include <iostream>

using namespace std;
using namespace cv;

int main(int argc, char** argv) {
	Mat src1, src2, dst;
	src1 = imread("D:/vcprojects/images/LinuxLogo.jpg");
	src2 = imread("D:/vcprojects/images/win7logo.jpg");
	if (!src1.data) {
		cout << "could not load image Linux Logo..." << endl;
		return -1;
	}
	if (!src2.data) {
		cout << "could not load image WIN7 Logo..." << endl;
		return -1;
	}

	double alpha = 0.5;
	if (src1.rows == src2.rows && src1.cols == src2.cols && src1.type() == src2.type()) {
		// addWeighted(src1, alpha, src2, (1.0 - alpha), 0.0, dst);
		// multiply(src1, src2, dst, 1.0);
		
		imshow("linuxlogo", src1);
		imshow("win7logo", src2);
		namedWindow("blend demo", CV_WINDOW_AUTOSIZE);
		imshow("blend demo", dst);
	}
	else {
		printf("could not blend images , the size of images is not same...\n");
		return -1;
	}

	waitKey(0);
	return 0;
}
```

