# Perceptual loss experiment
After building and testing our MonetGAN model, we hypothesized that adding high-level artistic details, such as brushstroke marks, could further improve the stylization of the generated Monet-like paintings. 
To test this, we explored the addition of a perceptual loss function, leveraging a pretrained VGG-19 model to extract high-level feature maps from real Monet paintings. The goal was for the generator to match the feature representations of these paintings in the VGG network's intermediate layers, which would ideally help capture finer, more abstract stylistic details.
# Visual Results
# FID Score
# Conclusion
