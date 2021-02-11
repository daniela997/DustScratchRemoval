# DustScratchRemoval
Level 4 Individual Project (2020)

This project addresses a less popular case of image restoration - namely, film dust and scratch artifact removal - by adapting a U-Net architecture [used for colourisation and superresolution](https://github.com/jantic/DeOldify) to the task. We leverage transfer learning to provide the network with a learned natural image prior with the aim utilising that feature space for the task of identifying and repairing unnatural artifacts - namely dust and scratches. 

We trained each restoration network with three different perceptual loss functions, all based on the VGG16 architecture:

* Perceptual Loss Model 1: We assigned the layer weights as used in the DeOldify project, i.e. 0, 0, 20, 70, 10 corresponding to layers 5, 12, 22, 32, 42 from VGG16 (ReLUs). We also used the same distance function, MAE for both style and content loss terms, as well as for the image distance term. 
* Perceptual Loss Model 2 : Based on our experiment investigating the sensitivity of each of the five ReLUs, we assign the following weights to them: 2, 4, 5, 6, 6. In other words, we give some weight to the first two layers, which were previously ignored, and progressively increase the weights for deeper layers. Again, we use MAE as the distance measure for both style loss and content loss. 
* Perceptual Loss Model 3: We train with the weights of Model 2, and also add our own distance metric - SSIM loss with a window size of 5 by 5 pixels. We use the SSIM loss as a distance measure between the prediction and target images, as well as the distance function in the content loss term; for the style loss term, we use MAE, and we also measure the MAE distance between the target and the prediction. 

## Examples of dust removal by our models

| Input                                                   | Model 1 Prediction                                        | Model 2 Prediction                                   | Model 3 Prediction                                              | Target                                                      |
|---------------------------------------------------------|-----------------------------------------------------------|------------------------------------------------------|-----------------------------------------------------------------|-------------------------------------------------------------|
| ![Dustified image](/dissertation/images/ex_1_input.png) | ![DeOldify Weights](/dissertation/images/ex_1_pred_1.png) | ![Our Weights](/dissertation/images/ex_1_pred_2.png) | ![Our Weights + Our Loss](/dissertation/images/ex_1_pred_3.png) | ![Clean ground truth](/dissertation/images/ex_1_target.png) |
| ![Dustified image](/dissertation/images/ex_2_input.png) | ![DeOldify Weights](/dissertation/images/ex_2_pred_1.png) | ![Our Weights](/dissertation/images/ex_2_pred_2.png) | ![Our Weights + Our Loss](/dissertation/images/ex_2_pred_3.png) | ![Clean ground truth](/dissertation/images/ex_2_target.png) |
