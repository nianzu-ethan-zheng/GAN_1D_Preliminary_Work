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

![3](https://sebastianraschka.com/images/blog/2014/parzen-rosenblatt/parzen_eq_06.png)


扩展概念（立方体核，hyercube window）我们可以得到下面的概念：<br>
通过下面的公式可以获得落入该区域的样本个数Kn<br>
![2](https://sebastianraschka.com/images/blog/2014/parzen-rosenblatt/parzen_eq_07.png)

进而我们可以的得到Rn的概率<br>
![1](https://sebastianraschka.com/images/blog/2014/parzen-rosenblatt/parzen_eq_08.png)

而将将简单的立方体核函数替换掉，采用高斯核：<br>
![guassian](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%202.png)<br>
and ![guassian2](https://github.com/DreamPurchaseZnz/Picture/blob/master/Image%203.png)
![效果图](https://upload.wikimedia.org/wikipedia/commons/4/41/Comparison_of_1D_histogram_and_KDE.png)
## 对parzen_ll的理解
终于到了主题：
```
def theano_parzen(mu, sigma):
    """
    Credit: Yann N. Dauphin
    """

    x = T.matrix()
    mu = theano.shared(mu)
    a = ( x.dimshuffle(0, 'x', 1) - mu.dimshuffle('x', 0, 1) ) / sigma
    E = log_mean_exp(-0.5*(a**2).sum(2))
    Z = mu.shape[1] * T.log(sigma * numpy.sqrt(numpy.pi * 2))

    return theano.function([x], E - Z)
```
这个函数用于创造一个parzen的计算器，其中x的大小为2D向量，通过dimshuffule来变为Ax1xB 这样的向量，mu为图像的所有像素的值，sigma对于所有维都是固定的均为sigma。通过创造一个log_mean_exp函数计算平均值，这是最诡异的地方。你们感受下，T.log与T.exp难道不是一对相反的操作，等于没有操作，然后减去max加上max这是要干嘛？？

```
def log_mean_exp(a):
    """
    Credit: Yann N. Dauphin
    """

    max_ = a.max(1)

    return max_ + T.log(T.exp(a - max_.dimshuffle(0, 'x')).mean(1))
```
定义好Parzen函数之后，通过get_nll函数获得似然值，其中的操作是每次输入一个batch_size,最后对各个batch_size求取平均值
```
def get_nll(x, parzen, batch_size=10):
    """
    Credit: Yann N. Dauphin
    """

    inds = range(x.shape[0])
    n_batches = int(numpy.ceil(float(len(inds)) / batch_size))

    times = []
    nlls = []
    for i in range(n_batches):
        begin = time.time()
        nll = parzen(x[inds[i::n_batches]])
        end = time.time()
        times.append(end-begin)
        nlls.extend(nll)

        if i % 10 == 0:
            print i, numpy.mean(times), numpy.mean(nlls)

    return numpy.array(nlls)
```
其中可以追查mu的来源,mu ==samples ,于是在下面我们可以发现出mu的蛛丝马迹，reshape函数将三个通道的图片扁平为一维向量，而整个空间定格为整个图片的像素点数。
```
samples = model.generator.sample(args.num_samples).eval()
    output_space = model.generator.mlp.get_output_space()
    if 'Conv2D' in str(output_space):
        samples = output_space.convert(samples, output_space.axes, ('b', 0, 1, 'c'))
        samples = samples.reshape((samples.shape[0], numpy.prod(samples.shape[1:])))
    del model
    gc.collect()
```
值得一提的是为了获得较好的估计值，他采用验证集来确定最大的似然值对应的sigma,验证集采用的是50000-60000的MNIST字体。
```
def cross_validate_sigma(samples, data, sigmas, batch_size):

    lls = []
    for sigma in sigmas:
        print sigma
        parzen = theano_parzen(samples, sigma)
        tmp = get_nll(data, parzen, batch_size = batch_size)
        lls.append(numpy.asarray(tmp).mean())
        del parzen
        gc.collect()

    ind = numpy.argmax(lls)
    return sigmas[ind]
```
Parzen Window的公式大致对的上，明确了输入（对于MNIST）是60000x1x784的一个批次，大约10个，主要可能考虑到对于高达784的多变量高斯分布计算量必然很大。
然后考虑到likelihood的计算公式![4](https://wikimedia.org/api/rest_v1/media/math/render/svg/d1dfbf94c2412b4a52dc41c91044495c24ed2dee)
具体公式复杂度和parzen器的定义对不上，因此无法从公式上对应，大致上是对的。



