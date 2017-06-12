# The result
Just post the picture and don't wanna say a word

![loss](Gan1D_sequel/oil_loss.png)
![loss](Gan1D_sequel/oil_loss_dcm.png)

![True](Gan1D_sequel/oil_true.png)

![False](Gan1D_sequel/2000_00.png)

![GIF](Gan1D_sequel/wave_process.gif)

One possible reason:
## : Track failures early

- D loss goes to 0: failure mode
- check norms of gradients: if they are over 100 things are screwing up
- when things are working, D loss has low variance and goes down over time vs having huge variance and spiking
- if loss of generator steadily decreases, then it's fooling D with garbage (says martin)

