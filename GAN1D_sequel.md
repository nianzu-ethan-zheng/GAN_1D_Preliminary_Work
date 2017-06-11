Maybe Opitimizer have some problem.

![gif](gan1D/saddle_point_evaluation_optimizers.gif)
![gif2](gan1D/contours_evaluation_optimizers.gif)

I can't understand what happen to GAN. Even training Discriminator twice than Generator, it can't reduce the loss of discriminator.

![gen](gan1D/spectrum_k2_4000.png) 
![dcm](gan1D/spectrum_dcm_k2_4000.png)

it seems that the better the generator produce a spectrum, the worse the discrimiator used as classifier predict.

![loss](gan1D/Trained_k2_weight.png)

so what's the problem with it??
there are some reason to verify:

* maybe 40 samples just not enough. -----> solution :use more eamples.
* maybe disciminator trainning loss curve may be a bit odd. you can see ,it increases.

* maybe it is completely unrealistic, it works well in classification,however maybe it's not suitable for regression.
* maybe there are another unknown reason for it.
so what exactly is the reason can account for this unthinkable phenamenon?
