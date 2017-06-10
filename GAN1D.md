# The application of GAN in Small samples of spectrum
-----------------------------------------
## Prior to instruction
This question have been proved.samll samples do nothing with convergence,but is relevant to accuracy.

The test is based on the spectrum of 40 samples of grape wine.


## Regression
All I have done is classifying. There are still some difference in regression. so how to construst 
a reasonable structure is very important to neural network

* First step : use spectrum dataset to do regression and see the effect of the strucure
  * without conv:  just dense layer
  * with conv : there are some questions in using conv1D or con2D, in fact we should use conv1D ,however need experience 
  
* Second step : use the structure and input data to design GAN  and classifier
* compare two method and get the result
# Structure of GAN1D

![structure](https://github.com/DreamPurchaseZnz/Picture/blob/master/gan1D/structure.JPG)

# Trainning process of GAN1D

![tranning](https://github.com/DreamPurchaseZnz/Picture/blob/master/gan1D/train_precess.JPG)

# How to use GAN1D

![classifier](https://github.com/DreamPurchaseZnz/Picture/blob/master/gan1D/classifier.JPG)


# Discussion and Result

Basically the test have proved gan's effectiveness in spectrum. As the loss plot shows, The process can be divided into six stages.Every stage have a gif to show what happen and we can draw some conclusion about the competation of  generator and dicriminator.

![loss](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/Spectrum_loss.png)

<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/0_0.png" width="450" height="450" />
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/500_0.png" width="450" height="450"/>
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/1158_0.png" width="450" height="450"/>
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/1487_0.png" width="450" height="450"/>
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/1689_0.png" width="450" height="450" />
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/code100_epoch1999_0.png" width="450" height="450" />
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/true.png" width="450" height="450" />

* Stage 1: Discriminator can easily judge which is true and which is false,so generator loss become very big and then while discriminator is in optimum , Generator has been optimised and then loss rapidly reduce，so discriminator loss increases.
* Stage 2: Discriminator stay in the stage of optimization and Generator work hard to reduce the loss.however,the result is not good.
* Stage 3: Generator obtain some specific feature and the result is getting better.Meanwhile Discriminator loss increase slightly.
* Stage 4: Generator continue to escape the watch of Dicriminator.
* Stage 5: Generator come to the begin point of convergence.
* Stage 6: We can see this stage as approximate convergence. The trend show us that 'the two player game' come to saddle point.

One of the most insteresting piont is the code length,which refers to the noise size. At beginning of the test ,the size of code is setted as **200** in order to cover the enough information in spectrum,which have 1868 dimensions and the enough epochs(10000) is given.Unfortunatorly the training is stuck in aroud 2300 and can not move on. The result picture show the competation is completely abnormal.for the picture below，we can see that there are only a horizontal line. The following process is just change the level of the line. **However** when the code reset as **100** , the incredible thing happened. just as you see,it provides us a new way to establish model. Also it leaves us lots of quention, **what is the reason for this phenamenon?** .we can investigate later.

<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/code100_epoch1999_0.png" width="450" height="450" />
<img src="https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/code200_epoch2000_0.png" width="450" height="450" />

# Classifier test

![loss](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/spectrum_loss_5000.png)
![dcm](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/spectrum_dcm_5000.png)

we can sample from the trainning process.At 1000 epochs, take weight to classifier and the result can converge ,however it's unbelievable that it can't be train to do regression ,taking the weights from 5000 epochs.

![clf](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/spectrum_clf_1000.png)
![clf_4000](https://github.com/DreamPurchaseZnz/DC_GAN/blob/master/gan1D/spectrum_clf_5000.png)

Neither is the optimum state ,which we want. There may be some reason for discriminator's mistake. Maybe,Genarator is too strong that Dicrimator can't compete with it.


