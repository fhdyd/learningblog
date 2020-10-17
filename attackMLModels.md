在同一个模型下，对一张图片添加一些肉眼难以发现的噪点，可能会得到不同的预测结果。
故意添加噪点来攻击模型的对抗样本技术，对样本的攻击分为无目标攻击和有目标攻击。无目标攻击添加杂质以后要求所得结果离真实结果越远越好，有目标攻击不仅要使结果远离真实结果，同时也要让其接近目标结果。
所以无目标攻击的损失函数为：
![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E6%88%AA%E5%9B%BE20201017162331.png)

有目标攻击的损失函数为：
![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E6%88%AA%E5%9B%BE20201017162426.png)

但跟训练模型不一样的是，这次要调节的参数是输入的图片而不是神经网络的参数。不是损失函数越小越好，为了得到最小的损失函数而过度调整图片会产生明显的影响容易被人察觉，所以要对图片进行一定的限制，即：
![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E6%88%AA%E5%9B%BE20201017163844.png)

常用的求d的方法有两种，即L2-norm和L-infinity，对于用L2-norm和L-infinity计算得到同样值的两张图片，显然前者更容易被人眼察觉，所以用L-infinity限制更好。
新的图片满足方程![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E6%88%AA%E5%9B%BE20201017165221.png)

通过梯度下降就可以得到攻击图片，在梯度下降的过程中如果限制条件不满足就对图片调整到满足条件再进行下一次梯度下降。
使用FGSM方法只用更新一次梯度就可以得到攻击图片，原理如下：

![](https://github.com/fhdyd/learningblog/blob/gh-pages/images/QQ%E6%88%AA%E5%9B%BE20201017170138.png)

攻击未知的模型，可以通过自己训练一个模型再生成攻击图片，这种方法可能对未知模型同样有效。对抗样本在现实中同样有效。
对对抗样本的防御分为主动和被动。被动防御对图片加一层滤镜，将图片模糊化或者对图片进行大小的修改，补零等，对模型的攻击就很有可能失效。主动防御在训练过程中添加攻击样本使模型能够有防御攻击图像的能力。
