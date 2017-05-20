Install_tensorflow_gpu version in detailed 
====
摸爬滚打查找问题之路,从师兄手中借来GPU跑GAN，中途被发现并没有使用GPU，尴尬的是，这个情况没有报错是系统采用CPU来代替。
有一个大坑是，我采用的是IPython notebook来作为调试平台，强制使用GPU，直接导致python内核崩溃，系统不报错，原因无从下手。中途几度想放弃。

介绍一下配置：
* Win7系统
* GTX1080
* Anaconda
* python3.5
* cuda 8.0
* cudnn 5.1(**very important**)   not ~~cudnn6.0~~
* visual studio 2015


下面具体介绍安装：（可参考这篇博客：[ Win10 TensorFlow（gpu）安装详解](http://blog.csdn.net/sb19931201/article/details/53648615'Detailed'))
## First step: install cuda 8.0
> Go to here [cuda](https://developer.nvidia.com/cuda-downloads) 下载Cuda，解压安装.
  Next.....

Tips：

    大约会花很长一段时间，其中中间会有一些其他驱动的安装，一定要及时确定，否则会一直卡在一个位置

#### Test 测试cuda 是否可以使用 

[Verify cuda installation](http://xcat-docs.readthedocs.io/en/stable/advanced/gpu/nvidia/verify_cuda_install.html)
通常的跑几个案例：
* Run the deviceQuery sample
* Run the bandwidthTest sample

## Second step: install cudnn 5.1
cudnn 5.1[cudnn](https://developer.nvidia.com/cudnn)，注册账号，可以下载一个压缩包54M，然后解压文件

* 将对应文件夹的文件复制到cuda源文件夹中即可


## Third step : intall tensorflow-gpu
当然可以采用anaconda进行安装或者采用pip3[tensorflow officely install](https://www.tensorflow.org/install/install_windows)
>pip3 install --upgrade tensorflow-gpu(推荐虽然有些慢)

> pip install --ignore-installed --upgrade https://storage.googleapis.com/tensorflow/windows/gpu/tensorflow_gpu-1.1.0-cp35-cp35m-win_amd64.whl 

## Four step : testing above
example 测试：[tensorflow源码](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples).
[下载整个压缩包](https://github.com/tensorflow/tensorflow)

## few very important point
1. 推荐使用highlight the point:cudnn 5.1
2. 一个错误的解决办法： I c:\tf_jenkins\home\workspace\release-win\device\gpu\os\windows\tensorflow\stream_executor\dso_loader.cc:119] Couldn't open CUDA library cupti64_80.dll  ----------------->将NVIDIA GPU Computing Toolkit\CUDA\v8.0\extras\CUPTI\libx64文件夹下的文件copy到CUDA\v8.0\bin文件夹下。
3. 推荐调试平台： 一定采用command line 来调试程序，否则一些错误无法被捕捉

