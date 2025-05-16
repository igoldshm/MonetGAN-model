# Quantitative Evaluation with FID Score

In addition to visual inspection of generated images, we performed a quantitative evaluation using the **Frechet Inception Distance (FID)** score. FID is a widely used metric in generative models that compares the distribution of generated images to real images in a high-dimensional feature space derived from a pre-trained Inception network.

While FID is useful for tracking model performance and comparing different training configurations, its reliability depends heavily on the **sample size** used to compute it. In our case, the dataset consists of only **300 Monet paintings** as the target domain, which introduces important limitations in the evaluation process.

### Limitations Due to Small Dataset

FID computation relies on estimating the **mean and covariance** of image features. With only 300 images:
- The **covariance matrix becomes noisy**, making the metric unstable.
- The **inception feature distribution is under-sampled**, which can lead to skewed statistical estimates.
- This under-sampling often results in **artificially high FID scores**, even when the generated images are visually convincing.

### Results

Our models were trained for **195 epochs** with:
- **Cycle consistency lambda = 10**
- **Adversarial lambda = 1**

| Identity Loss λ | FID Score |
| --------------- | --------- |
| 1.5             | 127.2188    |
| 4.5             | 114.87    |

- Two experiments were conducted, each trained for 195 epochs, with no VGG loss.

| Identity Loss λ | VGG Loss (lambda=0.5) | FID Score |
| --------------- | -------- | --------- |
| 1.5             | ❌ No     | 127.2188    |
| 1.5             | ✅ Yes    | 128.0403    |

- Two experiments were conducted, each trained for 195 epochs, one with VGG loss and one without.

### Conclusion

#### Increasing Identity Loss λ 

When we increased the identity loss λ, the generator became more conservative, it held back on stylization and stayed closer to the original photo. This happens because a higher identity loss penalizes large changes, discouraging strong, expressive transformations. As a result, the outputs looked more content-preserving and realistic.

#### Adding Perceptual Loss

Interestingly, the FID score went up. This is likely because the images moved away from the bold, stylized distribution of real Monet paintings. However, since we were working with a small dataset, the FID at this level is quite noisy. With high scores and limited data, even a one point fluctuation could be caused by randomness, data loading differences, or noise in the Inception feature space.

