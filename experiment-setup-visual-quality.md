# Visual Quality Tuning – MonetGAN

## Objective

This experiment aims to tune the loss function weights for a customized CycleGAN-based model (**MonetGAN**) with a focus on **optimizing visual quality**.
The goal is to find the best balance between **cycle consistency**, **identity preservation**, **adversarial realism**, and **high-level perceptual features** based on **human visual inspection**.
(*Quantitative evaluations, such as FID scores, will be documented separately.*)

---

## Initial Setup

* **Cycle-consistency loss weight (`cycle_lambda`)**: `10`
  (following the original CycleGAN paper)
* **Adversarial loss weight (`adversarial_lambda`)**: `1`
* **Perceptual loss weight (`perceptual_lambda`)**: `0` (initially disabled)
* **Identity loss weight (`identity_lambda`)**: *variable* (tuned during experiments)

> **Note:** The perceptual loss was implemented using feature maps extracted from a **pretrained VGG19** network.

---

## Tuning Steps

### 1. Identity Loss Tuning

* With cycle and adversarial losses active and perceptual loss disabled (`perceptual_lambda = 0`), different identity loss weights were tested:

  * `identity_lambda = 4.5`
  * `identity_lambda = 1.5`
  * `identity_lambda = 0.5`

**Observations:**

* Higher identity loss weights caused outputs to remain overly similar to the input domain (insufficient stylization).
* After visual inspection, **`identity_lambda = 1.5`** was selected as the optimal setting, offering a good balance between structure preservation and effective style transfer.

---

### 2. Perceptual Loss Tuning

* After fixing the cycle, adversarial, and identity loss settings, perceptual loss was introduced and tuned:

  * Tested values: `perceptual_lambda ∈ [0.1, 1.0]`

**Observations:**

* Even the smallest tested value (`perceptual_lambda = 0.1`) caused visible artifacts.
* Instead of enhancing high-level artistic features (like brush strokes), perceptual loss initially degraded the overall visual output.
* As a result, **perceptual loss was disabled** (`perceptual_lambda = 0`) for the final visual-quality-focused model.

---

## Summary of Current Optimal Settings (Visual Focus)

| Loss Type         | Lambda Value |
| ----------------- | ------------ |
| Cycle Consistency | 10           |
| Identity          | 1.5          |
| Adversarial       | 1            |
| Perceptual        | 0 (disabled) |

---

## Next Steps

* Investigate causes of perceptual loss instability (e.g., choice of feature layers, feature scaling).
* Test different designs for perceptual loss (e.g., multi-layer or style loss variants).
* Perform **quantitative** evaluation using FID, LPIPS, and other metrics in a separate experimental setup.

---

## Notes

* **Visual quality** was judged subjectively (human inspection) for this phase.
* Experiments were conducted on images with a resolution of **256×256** pixels.
* No numerical metrics (e.g., FID) were used in this round, a separate file will cover quantitative evaluation.

---
