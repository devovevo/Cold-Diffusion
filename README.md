## Introduction 
We re-implemented the model architecture depicted in "Cold Diffusion: Inverting Arbitrary Image Transforms Without Noise", a paper written by Timo Schick, Jane Dwivedi-Yu, Roberto Dessì, Roberta Raileanu, Maria Lomeli, Luke
Zettlemoyer, Nicola Cancedda, Thomas Scialom which was presented at NeurIPS. The core contribution of this paper is the development of the Cold Diffusion method, which provides a new way to reverse image transformations. Traditional techniques often involve adding noise to the image and then applying a denoising process to recover the original image. In contrast, Cold Diffusion achieves this inversion directly, without the intermediate step of noise addition, which can lead to loss of details. The Cold Diffusion method could potentially be applied in image restoration in forensic analysis, medical imaging (such as MRI and CT scans), and more without decreasing the quality of the images.

## Chosen Result
In this project, we aim to (1) reproduce the experiment where Cold Diffusion is used to invert image transformations such as blurring, specifically Figure 3 and Table 1 in the original paper. We give our attempt at re-implementing these pictures in the '/results' folder. These results demonstrate Cold Diffusion's ability to handle a common type of transformations on images. It's also possible to reproduce the results since it is trained on public datasets like MNIST and CIFAR-10. The research team also made their code available on GitHub.

## Re-implementation Details
Much like in the original paper, we utilize a UNet architecture which we train on a Gaussian Diffusion task where we use a blur operator coupled with Guassian kernels. Our network has a series of down-samples with attention blocks, all with residual connections, followed by a series of up-samples with attention blocks. We test our model on the MNIST dataset, which the paper does, though they mainly focus on the celebrity face datset to show off their results. All required libraries are included in the prologue of our notebook, and all datasets will be downloaded when the corresponding cell is run. A T100 GPU for about 1 hours should be sufficient to run 10,000 epochs of our model, which will give some results, though they are lacking. Realistically, only at around 100,000 epochs will the model perform relatively well, which corresponds to around 10 or more hours of using a T100 GPU.

## Results and Analysis
After viewing our reconstructed images, we can observe that our models latent space does not give equal weighting to all of the digits, and seems to nearly always choose to generate 0s. The paper's results, on the other hand, achieve a healthy variety of all the digits. Furthermore, we sometimes have completely black or smudged digits which are undecipherable, which the paper does not mention.

While the authors of the paper did not discuss their model architecture in their paper (they only linked to a GitHub repository), we do not know how long they trained their model for. However, given the impressive results we see, it seems clear they must have trained the model for much longer than we were able to. We can see the beginnings of the results they are seeing, but it seems quite likely we just haven't trained our model for long enough to become realistic.

On the other hand, while we could not recover their performance, we were still able to create a model which could accurately capture the key aspects of a digit in around 10 hours of training, which is pretty impressive. This just goes to show how powerful diffusion-like methods are, in that with even limited resources one can create generative models. Comparing our results to what we would expect with purely Guassian noise as our distortion process, we can see that sometimes the blur distortion seems harder to recover. This is because we can end up with completely black generations, which correspond to nothing. This is much less likely to happen with Gaussian noise as we will start to predict variation in the image, instead of it all being the same color.

## Conclusion and Future Work
