# Chapter 16 — Image Restoration & Noise

We continue **strictly according to your index**.

Chapter 16 topics:

1. Image degradation model
2. Noise model
3. Gaussian noise
4. Salt-and-pepper noise
5. Poisson noise
6. Speckle noise
7. Uniform noise
8. Rayleigh noise
9. Periodic noise
10. Restoration vs enhancement
11. Mean filters
12. Median filters
13. Adaptive filtering
14. Wiener filtering
15. Inverse filtering
16. Deconvolution
17. PSF
18. MSE / PSNR
19. SSIM

This chapter is especially important for **medical imaging**, because restoration asks a different question from enhancement:

> **How can we estimate the original image from a degraded observation?**

---

# 16.1 Restoration vs Enhancement

These terms are related but different.

### Image enhancement

Goal:

> Make an image more useful or visually interpretable.

Examples:

```text
Window/Level
Contrast enhancement
Sharpening
Brightness adjustment
```

It is often subjective.

---

### Image restoration

Goal:

> Recover an estimate of the original image from a known or modeled degradation process.

It is more model-based.

```text
Original
   ↓
Degradation
   ↓
Observed image
   ↓
Restoration
   ↓
Estimated original
```

Therefore:

[
\boxed{
Enhancement = improve appearance/usefulness
}
]

[
\boxed{
Restoration = estimate degraded original
}
]

---

# 16.2 Image Degradation Model

A fundamental model is:

[
\boxed{
g(x,y)=h(x,y)*f(x,y)+n(x,y)
}
]

where:

* (f(x,y)) = original image
* (h(x,y)) = degradation function / PSF
* (*) = convolution
* (n(x,y)) = noise
* (g(x,y)) = observed degraded image

Conceptually:

```text id="4a5k2e"
Original Image
      │
      ▼
    Blur
      │
      ▼
   + Noise
      │
      ▼
Observed Image
```

---

# 16.3 What Causes Degradation?

Possible causes include:

* motion
* optical blur
* detector limitations
* reconstruction effects
* electronic noise
* photon statistics
* acquisition artifacts
* transmission errors

For medical imaging, the exact degradation mechanism depends strongly on the modality and acquisition system.

---

# 16.4 Degradation Function

The degradation function describes how the imaging system modifies the original image.

In spatial domain:

[
g=h*f+n
]

In frequency domain:

[
\boxed{
G(u,v)=H(u,v)F(u,v)+N(u,v)
}
]

where:

* (H(u,v)) = frequency response of degradation
* (F(u,v)) = original spectrum
* (N(u,v)) = noise spectrum

---

# 16.5 Noise

Noise is unwanted variation introduced into the image.

General model:

[
\boxed{
g=f+n
}
]

when there is no blur/degradation other than additive noise.

Example:

```text id="m8d8g6"
Original:
100 100 100 100

Noisy:
98 104 101 96
```

The underlying signal is approximately constant, but observed values fluctuate.

---

# 16.6 Why Noise Matters in Medical Imaging

Noise can affect:

* visibility of small structures
* contrast
* edge detection
* segmentation
* measurement
* quantitative analysis

But noise is not simply "random garbage."

Different modalities produce different statistical noise behavior.

Therefore identifying the noise model is important.

---

# 16.7 Gaussian Noise

Gaussian noise follows a normal distribution:

[
\boxed{
p(n)=
\frac{1}{\sqrt{2\pi\sigma^2}}
e^{-\frac{(n-\mu)^2}{2\sigma^2}}
}
]

where:

* (\mu) = mean
* (\sigma^2) = variance

Often:

[
\mu=0
]

so:

[
\boxed{
n\sim N(0,\sigma^2)
}
]

---

# 16.8 Gaussian Noise Appearance

Conceptually:

```text id="j6n2l5"
Original
████████████

Gaussian noise
▓█▓██▓█▓▓██▓
```

Values fluctuate around the underlying signal.

Gaussian noise is widely used as a mathematical approximation, but the true noise distribution of a medical modality should be understood before assuming Gaussian behavior.

---

# 16.9 Gaussian Noise Parameters

### Mean

[
\mu
]

controls the center of the distribution.

### Variance

[
\sigma^2
]

controls spread.

Larger:

[
\sigma
]

means stronger noise.

```text id="f7n4mp"
σ small
 ↓
small fluctuations

σ large
 ↓
large fluctuations
```

---

# 16.10 Salt-and-Pepper Noise

Also called **impulse noise**.

Some pixels are replaced by extreme values.

Example:

```text id="o2h7p3"
Normal:
100 100 100 100 100

Noisy:
100 255 100 0 100
```

Typically:

* salt → bright pixels
* pepper → dark pixels

---

# 16.11 Why Median Works Well

Median filtering is particularly effective because extreme values do not necessarily dominate the median.

Example:

```text id="7k0g0y"
100 100 100
100 255 100
100 100 100
```

Median:

[
100
]

Therefore:

```text id="j1a3e4"
255 → removed
```

This is why median filtering is a classic impulse-noise restoration technique.

---

# 16.12 Poisson Noise

Poisson noise is associated with counting processes.

It is often relevant when the number of detected events follows Poisson statistics.

For Poisson random variable:

[
\boxed{
P(k)=
\frac{\lambda^k e^{-\lambda}}{k!}
}
]

where:

[
\lambda
]

is the expected count.

A key property is:

[
\boxed{
Variance=\lambda
}
]

Therefore:

[
\sigma=\sqrt{\lambda}
]

---

# 16.13 Why Poisson Noise Is Important

Poisson statistics occur naturally in situations involving:

* photon counting
* detected particles/events
* low-count imaging

The noise strength depends on the signal level.

This is different from simple additive constant-variance Gaussian noise.

Conceptually:

```text id="5s2r1c"
Low signal
 ↓
Low counts
 ↓
Strong relative uncertainty

High signal
 ↓
More counts
 ↓
Lower relative uncertainty
```

---

# 16.14 Speckle Noise

Speckle is commonly modeled as **multiplicative noise**.

Instead of:

[
g=f+n
]

we can model:

[
\boxed{
g=f(1+n)
}
]

or equivalently:

[
g=f+fn
]

Conceptually:

```text id="7n7q7s"
Original
   ×
Noise
   ↓
Observed
```

This is fundamentally different from additive noise.

---

# 16.15 Additive vs Multiplicative Noise

### Additive

[
g=f+n
]

Noise is added independently.

### Multiplicative

[
g=f(1+n)
]

Noise magnitude depends on signal.

So:

```text id="y9l1u3"
Additive:
signal + constant-type disturbance

Multiplicative:
signal × disturbance
```

This distinction affects restoration strategy.

---

# 16.16 Uniform Noise

Uniform noise has equal probability over an interval.

For:

[
a\le n\le b
]

the probability density is:

[
\boxed{
p(n)=
\frac{1}{b-a}
}
]

inside the interval and zero outside.

Conceptually:

```text id="o8h0ck"
Probability
   │
   │ ┌─────────────┐
   │ │             │
   │ │             │
   └─┴─────────────┴────
     a             b
```

---

# 16.17 Rayleigh Noise

Rayleigh distribution:

[
\boxed{
p(z)=
\frac{2(z-a)}{b}
e^{-\frac{(z-a)^2}{b}}
}
]

for:

[
z\ge a
]

and zero otherwise.

Rayleigh distributions can arise in certain magnitude-related statistical models.

The exact suitability depends on the imaging modality and processing stage.

---

# 16.18 Periodic Noise

Periodic noise has a repeating pattern.

Example:

```text id="5b5r7q"
++++----++++----++++----
```

or:

```text id="r3f8n2"
vertical stripes
```

Unlike random noise, periodic noise often produces distinct peaks in the frequency domain.

Therefore:

[
\boxed{
Frequency\ domain
\rightarrow
particularly useful
}
]

for identifying periodic noise.

---

# 16.19 Periodic Noise in Fourier Spectrum

Suppose the image contains periodic stripes.

Its Fourier spectrum may show:

```text id="h2v7z4"
       •
       |
   • --●-- •
       |
       •
```

The center:

```text id="7w0m6h"
●
```

is the low-frequency/DC area.

Additional symmetric peaks can indicate periodic components.

---

# 16.20 Restoration Strategy

A general restoration process:

```text id="v8f2r6"
Observed image
      ↓
Identify degradation
      ↓
Estimate noise / blur model
      ↓
Choose restoration method
      ↓
Restore
      ↓
Evaluate
```

The crucial step is:

> **Understand the degradation before choosing the filter.**

---

# 16.21 Mean Filters for Restoration

Mean filtering can reduce additive random noise.

For a neighborhood:

[
\boxed{
g(x,y)=
\frac1N
\sum_{p\in N}p
}
]

But it has a major drawback:

```text id="d6x0jh"
Noise ↓
Edges ↓
Fine detail ↓
```

Therefore it is not universally appropriate.

---

# 16.22 Median Restoration

Median filtering:

```text id="n3x4wv"
Neighborhood
 ↓
Sort
 ↓
Middle value
```

is particularly useful for:

[
\boxed{
Salt-and-pepper noise
}
]

because it removes isolated extremes without averaging them into neighboring pixels.

---

# 16.23 Adaptive Filtering

Fixed filters use the same behavior everywhere.

For example:

```text id="5x1n4k"
3×3 Gaussian
```

is applied everywhere.

Adaptive filters change behavior based on local image statistics.

Conceptually:

```text id="4o6d3h"
Smooth region
   ↓
More smoothing

Edge region
   ↓
Less smoothing
```

This can improve the trade-off between noise reduction and detail preservation.

---

# 16.24 Local Mean and Variance

Adaptive filters may calculate:

[
\mu_L
]

and:

[
\sigma_L^2
]

for the local neighborhood.

Then compare:

```text id="o9v2jh"
Local variance
     ↓
Low?
     → homogeneous area

High?
     → edge / texture / structure
```

The filter can adjust its behavior accordingly.

---

# 16.25 Why Adaptive Filtering?

Consider:

```text id="7p2y3g"
Smooth tissue
```

and:

```text id="6x8f1m"
Bone boundary
```

A fixed strong blur treats both similarly.

An adaptive filter can attempt:

```text id="b7f5t8"
Smooth region
 → stronger noise reduction

Boundary
 → preserve structure
```

This is often more desirable in medical imaging.

---

# 16.26 Wiener Filtering

The Wiener filter is a classical statistical restoration technique.

It attempts to minimize the mean-square error between the restored image and the original image.

The frequency-domain formulation is commonly expressed as:

[
\boxed{
\hat F(u,v)
===========

\frac{H^*(u,v)}
{|H(u,v)|^2+
\frac{S_n(u,v)}
{S_f(u,v)}
}
G(u,v)
}
]

where:

* (H) = degradation function
* (H^*) = complex conjugate
* (S_n) = noise power spectrum
* (S_f) = original-image power spectrum
* (G) = observed image spectrum

---

# 16.27 Why Wiener Is Important

Simple inverse filtering tries:

[
\hat F=\frac{G}{H}
]

But if:

[
H\approx0
]

then:

[
\frac1H
]

becomes extremely large.

Noise gets amplified.

Wiener filtering introduces a noise/statistical term to control this instability.

Conceptually:

```text id="f7p0g3"
Inverse filtering
 ↓
Try to undo blur
 ↓
Can amplify noise badly

Wiener
 ↓
Undo blur
+
Account for noise
 ↓
More stable
```

---

# 16.28 Inverse Filtering

Suppose:

[
G=HF
]

Ignoring noise:

[
F=\frac{G}{H}
]

Therefore:

[
\boxed{
\hat F=
\frac{G}{H}
}
]

This is inverse filtering.

---

# 16.29 Problem With Inverse Filtering

Suppose:

[
H=0.001
]

Then:

[
\frac1H=1000
]

A tiny noise component becomes huge.

So:

```text id="b1o2k5"
Small H
 ↓
Large 1/H
 ↓
Noise amplification
```

This is the fundamental weakness of naive inverse filtering.

---

# 16.30 Deconvolution

The degradation model:

[
g=h*f+n
]

contains convolution with:

[
h
]

Deconvolution attempts to reverse that convolution.

Conceptually:

```text id="1zq1j6"
Original
   ↓
Blur kernel
   ↓
Blurred image

Deconvolution
   ↓
Estimate original
```

---

# 16.31 Why Deconvolution Is Difficult

Because:

1. noise exists
2. information may have been lost
3. (H) may contain very small values
4. sampling limits information
5. the PSF may not be known exactly

Therefore:

[
\boxed{
Restoration\ is\ usually\ estimation,\ not\ perfect\ recovery
}
]

---

# 16.32 PSF

PSF means:

[
\boxed{
Point\ Spread\ Function
}
]

It describes the image of an ideal point after passing through an imaging system.

Imagine:

```text id="1v5b8d"
Ideal point:

      •
```

After imaging:

```text id="k4x8b3"
     . .
   .  .  .
     . .
```

The point spreads out.

That spread is characterized by the PSF.

---

# 16.33 Why PSF Is Important

The degradation model:

[
g=h*f+n
]

uses:

[
h
]

as the PSF/degradation kernel.

Therefore:

```text id="q0o1x9"
PSF
 ↓
Model system blur
 ↓
Restoration / deconvolution
```

If the PSF is inaccurate, restoration quality can degrade.

---

# 16.34 PSF in Medical Imaging

Different imaging systems have different spatial-resolution characteristics.

A PSF can conceptually represent effects from:

* detector response
* reconstruction
* optics
* motion
* system geometry

In medical imaging, PSF modeling must be modality-specific.

For example, a CT reconstruction's resolution behavior is not identical to optical camera blur.

---

# 16.35 MSE

Mean Squared Error measures average squared difference between reference and reconstructed image.

For (N) pixels:

[
\boxed{
MSE=
\frac1N
\sum_{i=1}^{N}
(I_i-\hat I_i)^2
}
]

where:

* (I) = reference
* (\hat I) = reconstructed/restored image

Lower MSE generally means smaller numerical error.

---

# 16.36 Example MSE

Reference:

```text id="1p8s4u"
100 100 100
```

Restored:

```text id="0f8d9k"
90 100 110
```

Errors:

```text id="n8b6e4"
10, 0, -10
```

Squared:

```text id="1d8b9w"
100, 0, 100
```

Therefore:

[
MSE=
\frac{200}{3}
]

[
\approx66.67
]

---

# 16.37 RMSE

Root Mean Squared Error:

[
\boxed{
RMSE=\sqrt{MSE}
}
]

It has the same units as the image intensity.

This can make interpretation easier.

---

# 16.38 PSNR

PSNR means:

[
\boxed{
Peak\ Signal-to-Noise\ Ratio
}
]

For maximum pixel value (MAX_I):

[
\boxed{
PSNR=
10\log_{10}
\left(
\frac{MAX_I^2}{MSE}
\right)
}
]

If the image is 8-bit:

[
MAX_I=255
]

---

# 16.39 PSNR Interpretation

Higher PSNR generally means:

```text id="4l2d9m"
Lower MSE
 ↓
Higher PSNR
```

But:

> High PSNR does not automatically mean clinically better medical images.

This is extremely important.

---

# 16.40 Why PSNR Is Limited

Suppose two images have similar pixel-wise error.

One may preserve:

```text id="d6b1u8"
anatomical boundaries
```

better than another.

MSE/PSNR may not fully capture that perceptual/structural difference.

Therefore we also use structural metrics such as SSIM.

---

# 16.41 SSIM

SSIM means:

[
\boxed{
Structural\ Similarity\ Index
}
]

It compares images using:

* luminance
* contrast
* structure

A simplified formulation:

[
\boxed{
SSIM(x,y)=
\frac{
(2\mu_x\mu_y+C_1)
(2\sigma_{xy}+C_2)
}{
(\mu_x^2+\mu_y^2+C_1)
(\sigma_x^2+\sigma_y^2+C_2)
}
}
]

where:

* (\mu_x,\mu_y) = local means
* (\sigma_x^2,\sigma_y^2) = local variances
* (\sigma_{xy}) = covariance
* (C_1,C_2) = stabilization constants

---

# 16.42 SSIM Intuition

Instead of asking only:

> "Are the pixels numerically close?"

SSIM asks more about:

```text id="j4y7m9"
Brightness
+
Contrast
+
Structural relationship
```

Therefore it often correlates better with perceived structural similarity than MSE alone.

---

# 16.43 SSIM Range

SSIM is typically interpreted around:

[
-1 \text{ to } 1
]

with:

[
\boxed{
1
}
]

representing identical images under the metric's formulation.

In many practical image comparisons, values closer to 1 indicate stronger structural similarity.

---

# 16.44 MSE vs PSNR vs SSIM

| Metric | Measures              | Better |
| ------ | --------------------- | ------ |
| MSE    | Pixel error           | Lower  |
| RMSE   | Pixel error           | Lower  |
| PSNR   | Signal/error ratio    | Higher |
| SSIM   | Structural similarity | Higher |

---

# 16.45 Important Medical Imaging Point

Suppose restoration A has:

```text id="8s9kq4"
Better PSNR
```

but restoration B has:

```text id="h7l0x2"
Better edge preservation
```

B might be more useful for a clinical visualization task even if its PSNR is slightly lower.

Therefore:

[
\boxed{
Metric\ optimization
\neq
automatic\ clinical\ optimization
}
]

---

# 16.46 Noise Model Comparison

| Noise           | Main Characteristic       | Common Model      |
| --------------- | ------------------------- | ----------------- |
| Gaussian        | Additive random variation | (g=f+n)           |
| Salt-and-pepper | Impulse/extreme values    | Pixel replacement |
| Poisson         | Counting statistics       | Signal-dependent  |
| Speckle         | Multiplicative            | (g=f(1+n))        |
| Uniform         | Equal probability range   | Bounded           |
| Rayleigh        | Asymmetric distribution   | Statistical       |
| Periodic        | Repeating pattern         | Frequency peaks   |

---

# 16.47 Restoration Method Selection

A useful decision tree:

```text id="5y4j7n"
What degradation?
       │
 ┌─────┼─────────────┐
 ↓     ↓             ↓
Noise Blur      Periodic
 │      │             │
 ↓      ↓             ↓
Filter Deconvolution Frequency
 │      │             │
 ├── Median          Notch
 ├── Adaptive        filtering
 └── Wiener
```

This is a conceptual framework, not a universal modality-specific prescription.

---

# 16.48 Noise vs Blur

Noise:

```text id="e6p2s0"
Random intensity variation
```

Blur:

```text id="3v8lq2"
Loss of spatial detail
```

They require different strategies.

```text id="9c0x1a"
Noise
 ↓
Denoising

Blur
 ↓
Deblurring / deconvolution
```

If both exist:

```text id="4x6z7m"
Blur + Noise
 ↓
Joint restoration
```

---

# 16.49 Restoration Pipeline

A complete conceptual pipeline:

```text id="c7k9s1"
Observed Image
      ↓
Noise Analysis
      ↓
Blur / PSF Analysis
      ↓
Choose Model
      ↓
Restoration
      ↓
Quality Evaluation
      ↓
Restored Image
```

---

# 16.50 Medical Imaging Example

Suppose a medical image has:

```text id="h6v4r2"
Blur
+
Gaussian-like noise
```

A possible conceptual approach:

```text id="0f7g3b"
Observed
   ↓
Estimate PSF
   ↓
Estimate noise
   ↓
Wiener / regularized restoration
   ↓
Evaluate
```

But actual clinical use requires modality-specific validation and careful verification.

---

# 16.51 Why Restoration Can Create Artifacts

Restoration attempts to recover information that has been weakened.

If the model is wrong:

```text id="e4n6c8"
Wrong PSF
   ↓
Wrong inverse
   ↓
Artifacts
```

If noise is underestimated:

```text id="c7h3m1"
Aggressive restoration
 ↓
Noise amplification
```

If restoration is too strong:

```text id="8v2g7m"
Ringing
Overshoot
Artificial structures
```

Therefore restoration must be controlled.

---

# 16.52 Regularization

When inverse problems are unstable, we often introduce additional constraints.

Instead of:

[
\hat F=\frac GH
]

we solve a more controlled optimization problem.

Conceptually:

[
\boxed{
Data\ fidelity
+
Regularization
}
]

The idea is:

> Find an image that explains the observed data while avoiding unrealistic solutions.

Regularization becomes a major topic in advanced image reconstruction.

---

# 16.53 Enterprise Architecture

For your medical image-processing architecture:

```text id="8k4s0x"
ImageRestoration
      │
      ├── NoiseModel
      │     ├── Gaussian
      │     ├── Poisson
      │     ├── Impulse
      │     └── Speckle
      │
      ├── DegradationModel
      │     └── PSF
      │
      ├── RestorationAlgorithm
      │     ├── Mean
      │     ├── Median
      │     ├── Adaptive
      │     ├── Wiener
      │     ├── Inverse
      │     └── Deconvolution
      │
      └── QualityMetrics
            ├── MSE
            ├── PSNR
            └── SSIM
```

This is a clean separation of responsibilities.

---

# 16.54 C++ Interface Design

For example:

```cpp id="d4k2s6"
class IRestorationAlgorithm
{
public:
    virtual ~IRestorationAlgorithm() = default;

    virtual Image restore(
        const Image& degraded) = 0;
};
```

Noise model:

```cpp id="k9n3j7"
class INoiseModel
{
public:
    virtual ~INoiseModel() = default;

    virtual double likelihood(
        double value) const = 0;
};
```

Quality metric:

```cpp id="1v5g9m"
class IImageMetric
{
public:
    virtual ~IImageMetric() = default;

    virtual double compute(
        const Image& reference,
        const Image& result) const = 0;
};
```

Implementations:

```text id="c1b7x0"
IRestorationAlgorithm
        │
        ├── WienerRestoration
        ├── MedianRestoration
        ├── AdaptiveRestoration
        └── Deconvolution

IImageMetric
        │
        ├── MSE
        ├── PSNR
        └── SSIM
```

---

# 16.55 Restoration and Your DICOM Viewer

For your viewer architecture:

```text id="s7w3j9"
DICOM Loader
      ↓
Original Pixel Data
      │
      ├──────────────────────┐
      │                      │
      ↓                      ↓
Display Processing      Restoration
      │                      │
Window/Level          Optional derived result
      │                      │
      └──────────┬───────────┘
                 ↓
               Viewer
```

The original DICOM pixel data should remain available.

A restoration result should generally be represented as a derived image rather than silently replacing the source.

---

# 16.56 Quantitative Medical Data

This is especially important for CT.

If a restoration filter changes pixel values, those values may no longer represent the same quantitative measurement characteristics as the original acquisition.

Therefore:

```text id="f0b7k4"
Original quantitative data
        ↓
Preserve
```

and:

```text id="j5w8h3"
Restored/enhanced image
        ↓
Derived representation
```

should be conceptually separated.

---

# 16.57 Chapter 16 Mental Model

Memorize:

```text id="2g6w4p"
                  IMAGE RESTORATION
                         │
                         ▼
                  DEGRADATION MODEL
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
             Blur                  Noise
              │                     │
             PSF              Noise Model
              │                     │
              └──────────┬──────────┘
                         ↓
                   RESTORATION
                         │
         ┌───────────────┼───────────────┐
         ↓               ↓               ↓
      Median          Wiener       Deconvolution
         │               │               │
         └───────────────┴───────────────┘
                         ↓
                     EVALUATION
                         │
              ┌──────────┼──────────┐
              ↓          ↓          ↓
             MSE        PSNR       SSIM
```

---

# 16.58 Key Formulas

### Degradation model

[
\boxed{
g=h*f+n
}
]

### Frequency-domain degradation

[
\boxed{
G=HF+N
}
]

### Poisson

[
\boxed{
P(k)=
\frac{\lambda^ke^{-\lambda}}{k!}
}
]

### Inverse filtering

[
\boxed{
\hat F=\frac GH
}
]

### Wiener

[
\boxed{
\hat F=
\frac{H^*}
{|H|^2+S_n/S_f}
G
}
]

### MSE

[
\boxed{
MSE=
\frac1N
\sum(I-\hat I)^2
}
]

### PSNR

[
\boxed{
PSNR=
10\log_{10}
\left(
\frac{MAX_I^2}{MSE}
\right)
}
]

### SSIM

[
\boxed{
SSIM=
\frac{
(2\mu_x\mu_y+C_1)
(2\sigma_{xy}+C_2)
}{
(\mu_x^2+\mu_y^2+C_1)
(\sigma_x^2+\sigma_y^2+C_2)
}
}
]

---

# 16.59 Knowledge Check

### Degradation

1. What is image restoration?
2. What is the difference between restoration and enhancement?
3. Write the image degradation model.
4. What does the PSF represent?
5. Why is restoration usually an estimation problem?

### Noise

6. What is Gaussian noise?
7. What is salt-and-pepper noise?
8. Why is median filtering effective for impulse noise?
9. What is Poisson noise?
10. Why is Poisson variance signal-dependent?
11. What is multiplicative noise?
12. What is speckle noise?
13. What is uniform noise?
14. What is Rayleigh noise?
15. Why is periodic noise suitable for frequency-domain analysis?

### Restoration

16. What is inverse filtering?
17. Why is inverse filtering unstable?
18. What is deconvolution?
19. What is Wiener filtering?
20. Why is Wiener generally more stable than naive inverse filtering?
21. What is adaptive filtering?
22. Why is adaptive filtering useful around edges?

### Evaluation

23. What is MSE?
24. What is RMSE?
25. What is PSNR?
26. What does SSIM measure?
27. Why can PSNR alone be insufficient for medical images?

### Medical Imaging

28. Why should restoration results be treated as derived data?
29. Why can aggressive deconvolution create artificial structures?
30. Why must the degradation model be modality-specific?

---

# 16.60 Practical Exercise — Noise

Take:

```text id="n0p4g8"
100 100 100
100 255 100
100 100 100
```

Identify the likely noise type.

Then calculate the output using a (3\times3) median filter.

---

# 16.61 Practical Exercise — MSE / PSNR

Reference:

```text id="7m3w5z"
100 100 100
100 100 100
100 100 100
```

Restored:

```text id="2h7p5c"
90 100 110
100 105 95
98 102 100
```

Calculate:

1. MSE
2. RMSE
3. PSNR assuming 8-bit data

Then explain why these metrics alone do not establish clinical usefulness.

---

# 16.62 Practical Exercise — Restoration Decision

Suppose you observe:

```text id="q5w8s3"
Image
+
blur
+
Gaussian-like noise
```

Choose between:

* Median
* Gaussian
* Inverse filter
* Wiener filter

Explain why.

A strong answer should recognize:

```text id="3n4z7k"
Median
 → impulse noise

Gaussian
 → smoothing

Inverse
 → deblur but noise-sensitive

Wiener
 → deblur + statistical noise consideration
```

---

# 16.63 Medical Imaging Exercise

Consider a CT image with:

```text id="f7q8d2"
Low-dose acquisition
+
noise
+
small anatomical structures
```

Design a restoration strategy.

Your answer should consider:

```text id="5r9k3n"
Noise model
      ↓
Spatial resolution
      ↓
Edge preservation
      ↓
Quantitative integrity
      ↓
Validation metrics
```

Do **not** simply choose:

> "Strongest denoising."

The goal is a controlled trade-off between noise reduction and preservation of diagnostically relevant information.

---

# 16.64 Chapter 16 Complete

The central model is:

[
\boxed{
Observed
========

Blur(Original)
+
Noise
}
]

or:

[
\boxed{
g=h*f+n
}
]

Then:

```text id="j0h5n8"
Observed Image
      ↓
Understand degradation
      ↓
Model noise / PSF
      ↓
Choose restoration algorithm
      ↓
Restore
      ↓
Measure MSE / PSNR / SSIM
      ↓
Validate result
```

The most important conceptual distinction is:

[
\boxed{
\text{Enhancement improves appearance}
}
]

[
\boxed{
\text{Restoration estimates the degraded original}
}
]

And for medical imaging:

[
\boxed{
\text{A visually better image is not automatically a more accurate image.}
}
]

**Chapter 16 complete.**

### Next, strictly according to your index:

# Chapter 17 — Morphological Image Processing

Topics:

* Binary morphology
* Structuring element
* Erosion
* Dilation
* Opening
* Closing
* Morphological gradient
* Boundary extraction
* Hit-or-miss
* Thinning
* Thickening
* Skeletonization
* Morphological reconstruction
* Grayscale morphology
* Medical-image applications
