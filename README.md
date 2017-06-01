# DC_GAN
##  Structure of Generator and Generator

![structure](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/STRUCTURE.JPG)

## Training

![train](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/Train%20Process.JPG)

## Small samples
在小样本情况下，设置样本数为400个，DC_GAN在经过57个周期时有出现数字的端倪，100个周期产生的样本已经很清晰。只不过数字表达的含义无法辨识。200个周期，有点像小孩子的手写字体。在进行分类器实验过程中，经过10个周期Accuracy达到0.95

![process_tracker](https://github.com/DreamPurchaseZnz/Picture/blob/master/process_train_loss-300.jpg)
![gif](https://github.com/DreamPurchaseZnz/Picture/blob/master/small-300.gif)


![result](https://github.com/DreamPurchaseZnz/Picture/blob/master/generated_image-300.png)

后续进行1000个周期与1500个周期的实验，大约在8000个周期的时候模型训练崩溃，学习不到任何东西。
<img src='https://github.com/DreamPurchaseZnz/Picture/blob/master/process_train_loss-1000.jpg'/>
<img src='https://github.com/DreamPurchaseZnz/Picture/blob/master/process_train_loss-1500.jpg'/>
<img src='https://github.com/DreamPurchaseZnz/Picture/blob/master/process_train_loss-20000.jpg'/>

<img src='https://github.com/DreamPurchaseZnz/Picture/blob/master/small_classifier.png'/>
<img src='https://github.com/DreamPurchaseZnz/Picture/blob/master/small_classifier-1500.png'/>

>结论：
* 在小样本的情况下，生成对抗模型训练依然可以进行，效果出乎意料，训练出类似大样本的形态
* 将模型迁移到分类模型中，模型经过一个周期的训练，达到的初始精度为0.60左右，说明在小样本下，模型可以获取特征，并用于分类；10个周期快速收敛，达到理想精度。
* 总的来说，小样本不影响模型的收敛性，只影响模型的精度问题。想要提高其精度必须增加相应的样本。

## MLP
![MLP](https://github.com/DreamPurchaseZnz/Keras/blob/master/MLP.JPG)
