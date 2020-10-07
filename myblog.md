盲水印学习笔记

频域盲水印，比起在空域上直接修改图片加水印，这种方法更加难以被发现，抵抗对图片修改的能力更强。

对于一张图片，可以把它当作一份二维空域信号，通过傅里叶变换可以把这种信号转化为频域信号，然后通过频域添加水印，再进行傅里叶逆变换得到修改以后的图像，从而获得一张添加过水印的图片。要想获得水印，只要对图片进行傅里叶逆变换。
因为把水印叠加在高频率区域，而图像的能量主要集中在低频区域，所以比较难发现对原图的修改。水印有一定鲁棒性，对原图进行一定破坏也不会让水印消失，保证了水印的安全。

对傅里叶变换的学习

对于一个信号可以把他分解成直流加正弦信号的组合，即：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/f.png)

将上式变为：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E5%9B%BE%E7%89%8720200930220245.png)

通过an和bn即可得到我们所要的对应频率的振幅和相位。振幅是an和bn的平方和开方，相位是-an/bn的反正切。

引入欧拉公式：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/%E6%AC%A7%E6%8B%89%E5%85%AC%E5%BC%8F.png)

欧拉公式以复数形式可以同时正余弦函数，进而使表示更加方便。
得到傅里叶变换：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E5%9B%BE%E7%89%8720200930221325.png)

其中，设系数：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/ak.png)

即可得对应频率的信息。
求对应系数方法：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/image1.png)

将频域信号再变回去只要进行傅里叶逆变换即可。

对于计算机而言，其中存储的信号只能是离散引号，因此要得到频域图只能进行离散傅里叶变换(DFT)。
将连续的t变为间断的n，可得：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/image.png)

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/iamge3.png)

若采样频率为fs,采样点数N，基波频率为fs/N。

DFT和FFT

对于取样长度为N的函数对其进行DFT，其算法时间复杂度为O(N^2)，算法较为复杂，消耗较多的性能，由此产生了更加有效的FFT算法。
通过式子转化，一个DFT变换可以变成两个DFT变化，直至只剩两个点的DFT，进而使时间复杂度降低到O(NlogN)。

图片的信号可以当成二维函数，把它转化成频域图就要进行如下的二维傅里叶变换。

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/image4.png)

添加盲水印后的图片和原图差别不大，增强了其隐匿性。同时因为水印分散在图片空域各处，对图片进行简单的操作如剪裁和涂画很难影响水印。

下面开始对代码的学习：

第一个程序是在图片上添加简单文字水印。了解了一下程序中函数的用法：imread读取图像，imshow显示图像，putText在图像上绘制文字，imwrite将图像保存。通过调整putText函数参数可以改变文字的各种属性，我尝试改变了文字的位置和大小。

第二个程序是给图片加上图片水印。imageWatermark是原图上离下边和右边各10的大小和水印一样的区域，用copyTo函数把水印复制到该区域。

第三个程序利用泊松融合添加水印。掩模图像可以对图片部分提取，seamlessclone添加的水印边界不会太明显。  

第四个程序LSB水印。加密图像每个通道的前四位存储原图的前四位，后四位取水印图的通道前四位。前四位保存了图片主要信息，因为只修改后四位所以图片修改不太明显，后四位用来保存水印图的主要信息。提取水印时提取图片后四位并前移四位，后四位补零就可以得到水印图片。实验一二对比把图片保存成不同格式提取的水印效果，bmp效果好。

第五个程序傅里叶变换水印。对于灰度图只有一个通道，dft变成实数复数双通道，在上面添加文字水印两次，我试了下只添加一次提出来的水印会变模糊，然后进行逆变换生成水印图片。提取水印的时候进行傅里叶逆变换。

没整明白这段代码是干嘛的

//

double minv = 0.0, maxv = 0.0;
	double* minp = &minv;
	double* maxp = &maxv;
	cv::minMaxIdx(complete, minp, maxp);
	std::cout << minv << "  " << maxv << std::endl;

	// 添加水印文字————中心对称
	int meanvalue = cv::mean(complete)[0], num;
	std::cout << meanvalue << std::endl;
	if (meanvalue > 128)
	{
		num = -log(abs(minv));
	}
	else
	{
		num = log(abs(maxv));
	}
