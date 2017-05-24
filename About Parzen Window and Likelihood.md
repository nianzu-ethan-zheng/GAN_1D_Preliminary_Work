About Parzen Window and Likelihood
========
[Generative Adversarial Nets](https://arxiv.org/pdf/1406.2661.pdf) code[http://www.github.com/goodfeli/adversarial]


对Ian提到的采用Gassian Parzen Window 去拟合G产生的样本并估计对数似然性(即给予观察样本，判断模型正确的可能性)如何实现的原理很感兴趣？
于是找到源代码[parzen_ll.py](https://github.com/goodfeli/adversarial/blob/master/parzen_ll.py)来看，
但是苦于并没有学过theano及pylearn2所以看起来不是明白，本来想尝试安装py2learn来测试，结果看了官网的安装要求，请卸载python，清除所有组件。这个代价有点大
，毕竟我是要做Tensorflow的。因此只能破罐破摔来，硬着头皮看代码，有些东西只能靠google，看了一个大概。首先来说一下Parzen Window

## [Gassian Parzen Window](http://sebastianraschka.com/Articles/2014_kernel_density_est.html) 
这种方法最基本的思想是给定一个特定的区域（**Window**）对落入其中的样本进行计数，可以得到样本落入该区域的概率大约为：

<img src="http://www.sciweavers.org/tex2img.php?eq=p%28x%29%3D%5Cfrac%7B%5C%23%20of%20samples%5C%20%20in%5C%20%20R%7D%7Btotal%20%5C%20%20samples%7D&bc=White&fc=Black&im=png&fs=12&ff=mathpple&edit=0" align="center" border="0" alt="p(x)=\frac{\# of samples\  in\  R}{total \  samples}" width="181" height="42" />
