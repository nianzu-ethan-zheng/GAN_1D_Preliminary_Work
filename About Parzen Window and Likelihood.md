About Parzen Window and Likelihood
========
[Generative Adversarial Nets](https://arxiv.org/pdf/1406.2661.pdf) code[http://www.github.com/goodfeli/adversarial]


对Ian提到的采用Gassian Parzen Window 去拟合G产生的样本并估计对数似然性(即给予观察样本，判断模型正确的可能性)如何实现的原理很感兴趣？
于是找到源代码[parzen_ll.py](https://github.com/goodfeli/adversarial/blob/master/parzen_ll.py)来看，
但是苦于并没有学过theano及pylearn2所以看起来不是明白，本来想尝试安装py2learn来测试，结果看了官网的安装要求，请卸载python，清除所有组件。这个代价有点大
，毕竟我是要做Tensorflow的。因此只能破罐破摔来，硬着头皮看代码，有些东西只能靠google，看了一个大概。首先来说一下Parzen Window

## [Gassian Parzen Window](http://sebastianraschka.com/Articles/2014_kernel_density_est.html) 
这种方法最基本的思想是给定一个特定的区域（**Window**）对落入其中的样本进行计数，可以得到样本落入该区域的概率大约为：[公式编辑](http://www.sciweavers.org/free-online-latex-equation-editor)

<img src="http://www.sciweavers.org/tex2img.php?eq=p%28x%29%3D%5Cfrac%7B%5C%23%20of%20samples%20in%20R%7D%7B%20total%20samples%7D&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="p(x)=\frac{\# of samples in R}{ total samples}" width="199" height="47" />

一种方法是固定体积（volume）如下图，原理是取一个固定大小的区域R，观察有多少样本落入其中。<br>
![fixed volume](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%201.png)

另外一种方法是样本数目K固定；k近邻算法就是这个原理，即是，对于一定大小的样本总数，我们取能框住k个样本的区域R<br>
![fixed k](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%204.png)

## The window function
对于上述的想法，有一个更加数学的表示：

<img src="http://www.sciweavers.org/tex2img.php?eq=%20%5CPhi%20%28u%29%3D%5Cbegin%7Bcases%7D%0A1%20%26%20%20%7Cu_j%7C%20%5Cleq%20d%20%20%5Cquad%20%5Cforall%20j%5C%5C%0A0%20%26%20otherwise%0A%5Cend%7Bcases%7D%20&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt=" \Phi (u)=\begin{cases}1 &  |u_j| \leq d  \quad \forall j\\0 & otherwise\end{cases} " width="196" height="47" />

扩展概念（立方体核，hyercube window）我们可以得到下面的概念：<br>
通过下面的公式可以获得落入该区域的样本个数Kn

<img src="http://www.sciweavers.org/tex2img.php?eq=k_n%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20%5Cphi%28%5Cfrac%7Bx-x_i%7D%7Bh_n%7D%29&bc=White&fc=Black&im=jpg&fs=12&ff=arev&edit=0" align="center" border="0" alt="k_n = \sum_{i=1}^{n} \phi(\frac{x-x_i}{h_n})" width="143" height="50" />

进而我们可以的得到Rn的概率

而将将简单的立方体核函数替换掉，采用高斯核：<br>
![guassian](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%202.png)<br>
and ![guassian2](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%203.png)
![效果图](https://upload.wikimedia.org/wikipedia/commons/4/41/Comparison_of_1D_histogram_and_KDE.png)
## 对parzen_ll的理解









