# DC_GAN
##  Structure of Generator and Generator

![structure](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/STRUCTURE.JPG)

## Training

![train](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/Train%20Process.JPG)

## Small samples
在小样本情况下，设置样本数为400个，DC_GAN在经过57个周期时有出现数字的端倪，100个周期产生的样本已经很清晰。只不过数字表达的含义无法辨识。200个周期，有点像小孩子的手写字体。在进行分类器实验过程中，经过10个周期Accuracy达到0.95
![GIF](https://github.com/DreamPurchaseZnz/Picture/blob/master/small.gif)
![process_tracker](https://github.com/DreamPurchaseZnz/Picture/blob/master/process_train_loss.jpg)
![result](https://github.com/DreamPurchaseZnz/Picture/blob/master/generated_image.png)


## MLP
![MLP](https://github.com/DreamPurchaseZnz/Keras/blob/master/MLP.JPG)
