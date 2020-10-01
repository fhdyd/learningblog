盲水印学习笔记

频域盲水印，比起在空域上直接修改图片加水印，这种方法更加难以被发现，抵抗对图片修改的能力更强。

对于一张图片，可以把它当作一份二维空域信号，通过傅里叶变换可以把这种信号转化为频域信号，然后通过频域添加编码后的水印，再进行傅里叶逆变换得到修改以后的图像，从而获得一张添加过水印的图片。要想获得水印，首先对图片进行傅里叶逆变换，得到频域图，然后对其减去原图的频域，再对图像进行水印编码的逆操作后即可获得水印。
因为把水印编码后分散到频域图的各个区域，而图像的能量主要集中在低频区域，所以比较难发现对原图的修改。水印有一定鲁棒性，对原图进行一定破坏也不会让水印消失，保证了水印的安全。

对傅里叶变换的学习

对于一个信号可以把他分解成直流加正弦信号的组合，即：

![f](https://github.com/fhdyd/learningblog/blob/gh-pages/images/f.png)

将上式变为：

![a](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E5%9B%BE%E7%89%8720200930220245.png)

通过an和bn即可得到我们所要的对应频率的振幅和相位。振幅是an和bn的平方和开方，相位是-an/bn的反正切。

引入欧拉公式：

![欧拉](https://github.com/fhdyd/learningblog/blob/gh-pages/images/%E6%AC%A7%E6%8B%89%E5%85%AC%E5%BC%8F.png)

欧拉公式以复数形式可以同时正余弦函数，进而使表示更加方便。
得到傅里叶变换：

![欧拉](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E5%9B%BE%E7%89%8720200930221325.png)

其中，设系数：

![欧拉](https://github.com/fhdyd/learningblog/blob/gh-pages/images/ak.png)

即可得对应频率的信息。
求对应系数方法：

![.](https://github.com/fhdyd/learningblog/blob/gh-pages/images/image1.png)

将频域信号再变回去只要进行傅里叶逆变换即可。

对于计算机而言，其中存储的信号只能是离散引号，因此要得到频域图只能进行离散傅里叶变换(DFT)。
将连续的t变为间断的n，可得：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/image.png)

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/iamge3.png)

DFT和FFT

对于取样长度为N的函数对其进行DFT，其算法时间复杂度为O(N^2)，算法较为复杂，消耗较多的性能，由此产生了更加有效的FFT算法。
通过式子转化，一个DFT变换可以变成两个DFT变化，直至只剩两个点的DFT，进而使时间复杂度降低到O(NlogN)。


