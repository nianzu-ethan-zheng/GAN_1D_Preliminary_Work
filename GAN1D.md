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


    Stage 1: Discriminator can easily judge which is true and which is false,so generator loss become very big and then while discriminator is in optimum , Generator has been optimised and then loss rapidly reduce，so discriminator loss increases.
    Stage 2: Discriminator stay in the stage of optimization and Generator work hard to reduce the loss.however,the result is not good.
    Stage 3: Generator obtain some specific feature and the result is getting better.Meanwhile Discriminator loss increase slightly.
    Stage 4: Generator continue to escape the watch of Dicriminator.
    Stage 5: Generator come to the begin point of convergence.
    Stage 6: We can see this stage as approximate convergence. The trend show us that 'the two player game' come to saddle point.


