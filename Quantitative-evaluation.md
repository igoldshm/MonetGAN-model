# Quantitative Evaluation with FID Score

In addition to visual inspection of generated images, we performed a quantitative evaluation using the **Frechet Inception Distance (FID)** score. FID is a widely used metric in generative models that compares the distribution of generated images to real images in a high-dimensional feature space derived from a pre-trained Inception network.

While FID is useful for tracking model performance and comparing different training configurations, its reliability depends heavily on the **sample size** used to compute it. In our case, the dataset consists of only **300 Monet paintings** as the target domain, which introduces important limitations in the evaluation process.

### Limitations Due to Small Dataset

FID computation relies on estimating the **mean and covariance** of image features. With only 300 images:
- The **covariance matrix becomes noisy**, making the metric unstable.
- The **inception feature distribution is under-sampled**, which can lead to skewed statistical estimates.
- This under-sampling often results in **artificially high FID scores**, even when the generated images are visually convincing.

### Example Result

In our model trained for **195 epochs** with:
- **Cycle consistency lambda = 10**
- **Identity loss lambda = 1.5**

We got an FID score of 127.2188, which is on the higher side. When we increased the identity loss λ, the generator started holding back on stylization, it got more conservative and stuck closer to the original photo. 
That’s because a higher identity loss penalizes big changes, so the model avoids strong, expressive transformations. therefor, the results looked more content-preserving, and the FID score went up, likely because the images didn’t match the distribution of the bold style of real Monet paintings but became more realistically looking.
