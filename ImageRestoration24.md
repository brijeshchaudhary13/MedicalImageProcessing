# Chapter 24 — Image Restoration

We continue **strictly according to your index**.

Chapter 24 is different from ordinary image enhancement.

The central question is:

> **Can we estimate the original image from a degraded observation?**

---

# 24.1 Image Restoration vs Image Enhancement

This distinction is very important.

### Image Enhancement

Goal:

> Make the image look better or more useful.

Examples:

```text
Contrast enhancement
Sharpening
Brightness adjustment
Histogram equalization
```

### Image Restoration

Goal:

> Recover an estimate of the original image from a known or modeled degradation process.

Examples:

```text
Deblurring
Noise removal
Motion-blur correction
Inverse filtering
Wiener filtering
```

Therefore:

[
\boxed{
Enhancement = improve appearance
}
]

[
\boxed{
Restoration = estimate degraded original
}
]

---

# 24.2 Basic Restoration Model

The classical degradation model is:

[
\boxed{
g(x,y)=h(x,y)*f(x,y)+\eta(x,y)
}
]

where:

* (f(x,y)) = original image
* (h(x,y)) = degradation function / PSF
* (*) = convolution
* (\eta(x,y)) = noise
* (g(x,y)) = observed degraded image

Conceptually:

```text
Original Image
     ↓
   Blur
     ↓
   Noise
     ↓
Observed Image
```

---

# 24.3 Restoration Pipeline

```text
Original
   ↓
Degradation
   ├── Blur
   └── Noise
   ↓
Observed Image
   ↓
Restoration Algorithm
   ↓
Estimated Original
```

The important point is that the restoration algorithm tries to reverse the degradation.

---

# 24.4 What Is Degradation?

Degradation means information in the original image has been altered.

Examples:

```text
Noise
Blur
Motion
Defocus
Sampling
Sensor limitations
```

---

# 24.5 Common Medical Image Degradation

Medical images can contain:

```text
CT
 ├── quantum noise
 ├── reconstruction artifacts
 └── motion artifacts

MRI
 ├── thermal noise
 ├── motion
 └── acquisition artifacts

X-ray
 ├── quantum noise
 ├── detector noise
 └── motion

Ultrasound
 ├── speckle
 └── acquisition noise
```

The exact noise characteristics depend on the acquisition system and reconstruction method.

---

# 24.6 Noise

Noise is unwanted variation added to an image.

Model:

[
\boxed{
g=f+\eta
}
]

where:

* (f) = original
* (\eta) = noise
* (g) = observed

---

# 24.7 Gaussian Noise

Gaussian noise follows approximately:

[
\boxed{
p(z)=
\frac{1}{\sqrt{2\pi\sigma^2}}
e^{-\frac{(z-\mu)^2}{2\sigma^2}}
}
]

where:

* (\mu) = mean
* (\sigma^2) = variance

---

# 24.8 Gaussian Noise Appearance

Gaussian noise looks like random small intensity fluctuations:

```text
Original:

████████████
████████████
████████████

Noisy:

█▓███▓█████
███▓████▓██
▓██████▓███
```

The image remains recognizable but becomes grainy.

---

# 24.9 Gaussian Noise Parameters

Two important parameters:

[
\boxed{
\mu
}
]

and:

[
\boxed{
\sigma
}
]

Higher (\sigma):

```text
More noise
```

Lower (\sigma):

```text
Less noise
```

---

# 24.10 Salt-and-Pepper Noise

Salt-and-pepper noise produces extreme pixels.

```text
Salt → white
Pepper → black
```

Example:

```text
Normal:

████████████
████████████
████████████

Noisy:

███●████████
████████○███
██○██████●██
```

where:

```text
● → bright pixel
○ → dark pixel
```

---

# 24.11 Causes of Salt-and-Pepper-Like Noise

It can arise from:

* transmission errors
* faulty sensor elements
* bit errors
* data corruption

It is characterized by sparse extreme-valued pixels.

---

# 24.12 Median Filter

The median filter is particularly useful for impulse noise.

Example:

```text
10  11  12
10 255  11
12  10  11
```

The center pixel:

[
255
]

is an outlier.

Sort neighborhood:

```text
10 10 10 11 11 11 12 12 255
```

Median:

[
\boxed{11}
]

Replace:

```text
255 → 11
```

---

# 24.13 Why Median Filtering Works

Unlike the mean, the median is robust to extreme outliers.

Mean:

```text
10,10,10,11,11,11,12,12,255
```

gets pulled strongly upward.

Median:

[
11
]

remains representative.

---

# 24.14 Median vs Mean Filter

| Mean                  | Median                       |
| --------------------- | ---------------------------- |
| Linear                | Nonlinear                    |
| Sensitive to outliers | Robust to outliers           |
| Smooths noise         | Good for impulse noise       |
| Can blur edges        | Often preserves edges better |

---

# 24.15 Poisson Noise

Poisson noise is associated with counting processes.

A Poisson random variable has:

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

Importantly:

[
\boxed{
Variance=\lambda
}
]

for an ideal Poisson process.

---

# 24.16 Why Poisson Noise Matters in Medical Imaging

Many imaging systems involve counting detected events/photons.

Examples can include:

```text
X-ray
CT
PET
Nuclear imaging
```

The exact noise behavior in a clinical system can be more complicated because of detector physics and reconstruction.

---

# 24.17 Speckle Noise

Speckle is commonly associated with coherent imaging such as ultrasound.

Unlike simple additive Gaussian noise, speckle is often modeled as **multiplicative**:

[
\boxed{
g=f\cdot n
}
]

or more generally:

[
g=f\cdot n+\eta
]

depending on the model.

---

# 24.18 Speckle Appearance

Ultrasound:

```text
Smooth region
     ↓
grainy texture
```

This grain-like pattern is called:

[
\boxed{
Speckle
}
]

It is not simply ordinary additive noise.

---

# 24.19 Log Transformation for Multiplicative Noise

If:

[
g=f\cdot n
]

take logarithms:

[
\log g=\log f+\log n
]

Multiplicative noise becomes additive in the log domain.

Conceptually:

```text
Multiplicative
     ↓
Log transform
     ↓
Additive
     ↓
Filter
     ↓
Inverse log
```

This technique can be useful in certain speckle-reduction workflows.

---

# 24.20 Periodic Noise

Periodic noise appears as repeated patterns.

Example:

```text
Original
 ↓
Interference
 ↓
Repeating pattern
```

In an image it may appear as:

```text
||||||||||||||
||||||||||||||
||||||||||||||
```

or repeating waves.

---

# 24.21 Fourier Domain for Periodic Noise

Periodic noise can often be easier to identify in the frequency domain.

Pipeline:

```text
Image
 ↓
FFT
 ↓
Frequency spectrum
 ↓
Identify noise peaks
 ↓
Notch filter
 ↓
Inverse FFT
```

---

# 24.22 Noise Summary

| Noise           | Typical Model        | Useful Technique           |
| --------------- | -------------------- | -------------------------- |
| Gaussian        | Additive             | Gaussian/Wiener/NLM etc.   |
| Salt-and-pepper | Impulsive            | Median                     |
| Poisson         | Counting/statistical | Variance-aware methods     |
| Speckle         | Often multiplicative | Speckle-specific methods   |
| Periodic        | Structured           | Frequency-domain filtering |

---

# 24.23 Blur

Blur occurs when image details are spread spatially.

Examples:

```text
Defocus
Motion
Optical blur
Reconstruction blur
```

Conceptually:

```text
Sharp edge:

████|░░░░

Blurred:

███▓▒░░░
```

---

# 24.24 Point Spread Function

PSF means:

[
\boxed{
Point\ Spread\ Function
}
]

It describes how an imaging system represents an ideal point.

An ideal point:

```text
     ●
```

might become:

```text
    ░░
   ░██░
    ░░
```

The resulting shape is the PSF.

---

# 24.25 PSF and Blur

If:

[
h(x,y)
]

is the PSF, then:

[
\boxed{
g=h*f
}
]

ignoring noise.

Therefore the PSF describes how the imaging system blurs the original.

---

# 24.26 Degradation Function

In frequency domain:

[
\boxed{
G(u,v)=H(u,v)F(u,v)+N(u,v)
}
]

where:

* (F) = original image spectrum
* (H) = degradation transfer function
* (N) = noise spectrum
* (G) = observed spectrum

---

# 24.27 Why Fourier Domain?

Convolution becomes multiplication:

[
h*f
]

becomes:

[
H\cdot F
]

Therefore:

```text
Spatial domain
h * f

       ↓ FFT

Frequency domain
H × F
```

This can make some restoration problems easier to formulate.

---

# 24.28 Inverse Filtering

Ignoring noise:

[
G=HF
]

Therefore:

[
\boxed{
F=\frac{G}{H}
}
]

This leads to inverse filtering:

[
\boxed{
\hat F=
\frac{G}{H}
}
]

---

# 24.29 Problem With Inverse Filtering

Suppose:

[
H\approx0
]

Then:

[
\frac{1}{H}
]

becomes very large.

Any small noise gets amplified dramatically.

Example:

```text
H = 0.001
Noise = 0.01
```

Then dividing by (H):

[
\frac{0.01}{0.001}=10
]

Huge amplification.

Therefore naive inverse filtering is highly sensitive to noise.

---

# 24.30 Wiener Filtering

Wiener filtering attempts to balance:

```text
Deblurring
+
Noise suppression
```

A common frequency-domain form is:

[
\boxed{
\hat F(u,v)=
\frac{H^*(u,v)}
{|H(u,v)|^2+\frac{S_N(u,v)}{S_F(u,v)}}
G(u,v)
}
]

where:

* (H^*) = complex conjugate
* (S_N) = noise power spectrum
* (S_F) = original image power spectrum

---

# 24.31 Wiener Intuition

Inverse filter:

```text
Remove blur aggressively
```

but:

```text
May amplify noise
```

Wiener:

```text
Remove blur
+
Consider noise
```

Therefore:

[
\boxed{
Wiener = blur\ correction + noise\ tradeoff
}
]

---

# 24.32 Constrained Least Squares

Another restoration approach is constrained least-squares filtering.

Conceptually:

```text
Fit observed image
+
Penalize undesirable solutions
```

It attempts to find an image that:

1. agrees with the observed degraded image
2. remains reasonably smooth/regularized

This is another example of **regularized inverse problems**.

---

# 24.33 Regularization

The inverse problem:

[
g=Hf+n
]

may be ill-conditioned.

Instead of directly solving:

[
Hf=g
]

we introduce a constraint or penalty.

Conceptually:

[
\boxed{
Data\ fidelity
+
Regularization
}
]

This prevents unstable solutions.

---

# 24.34 Why Restoration Is an Inverse Problem

Forward problem:

```text
Original
 ↓
Imaging system
 ↓
Degraded image
```

Restoration:

```text
Degraded image
 ↓
Estimate imaging system effects
 ↓
Original estimate
```

Therefore:

[
\boxed{
Restoration = inverse\ problem
}
]

---

# 24.35 Deblurring

Deblurring attempts to reverse blur.

Pipeline:

```text
Blurred Image
 ↓
Estimate PSF
 ↓
Restoration Algorithm
 ↓
Deblurred Image
```

Possible algorithms:

```text
Inverse filtering
Wiener filtering
Regularized restoration
Blind deconvolution
```

---

# 24.36 Blind Deconvolution

Sometimes the PSF is unknown.

Then we have:

```text
Unknown image
+
Unknown PSF
 ↓
Observed image
```

We try to estimate both.

This is called:

[
\boxed{
Blind\ deconvolution
}
]

It is more difficult than restoration with a known PSF.

---

# 24.37 Known vs Unknown PSF

### Known PSF

```text
g
+
known h
 ↓
restore f
```

### Unknown PSF

```text
g
 ↓
estimate h
+
estimate f
```

The second problem is substantially more challenging.

---

# 24.38 Gaussian Blur

A Gaussian blur uses:

[
\boxed{
G(x,y)=
\frac{1}{2\pi\sigma^2}
e^{-\frac{x^2+y^2}{2\sigma^2}}
}
]

Convolution:

[
g=G_\sigma*f
]

produces smoothing.

---

# 24.39 Motion Blur

Motion blur can occur when:

```text
Object moves
OR
scanner/patient moves
```

during acquisition.

The PSF becomes directional.

Conceptually:

```text
Point:

●

Motion blur:

────●────
```

---

# 24.40 Medical Motion Artifacts

Motion can affect:

* MRI
* CT
* X-ray
* fluoroscopy
* other acquisitions

Depending on the modality, the artifact can appear differently.

---

# 24.41 Restoration vs Motion Correction

Do not confuse:

```text
Restoration
```

with:

```text
Registration
```

Restoration attempts to correct degradation such as blur/noise.

Registration aligns different images.

```text
Restoration:
bad image → better estimate

Registration:
image A + image B → spatial alignment
```

---

# 24.42 Adaptive Filtering

A fixed filter uses the same parameters everywhere.

An adaptive filter changes behavior based on local image statistics.

Example:

```text
Smooth region
 ↓
More smoothing

Edge region
 ↓
Less smoothing
```

This can preserve details better than a uniform blur.

---

# 24.43 Adaptive Median Filter

Adaptive median filtering changes the neighborhood size depending on local conditions.

Conceptually:

```text
Small window
   ↓
Still noisy?
   ↓
Larger window
   ↓
Filter
```

It can be useful for severe impulse noise.

---

# 24.44 Non-Local Means

Although not in the classical minimum list, modern restoration commonly uses non-local methods.

The idea:

> Similar patches elsewhere in the image can help denoise the current patch.

Conceptually:

```text
Current patch
      ↓
Find similar patches
      ↓
Weighted combination
      ↓
Denoised patch
```

This can preserve repetitive structures better than simple local filters.

---

# 24.45 Restoration and Edges

The major challenge is:

```text
Noise removal
      VS
Edge preservation
```

Aggressive smoothing:

```text
Noise ↓
Edges ↓
```

Weak smoothing:

```text
Noise remains
Edges preserved
```

A good restoration algorithm seeks a useful balance.

---

# 24.46 Example

Suppose:

```text
Original:

██████░░░░░░
██████░░░░░░
██████░░░░░░
```

with noise added.

A strong Gaussian blur may produce:

```text
████▓▒░░░░░░
████▓▒░░░░░░
████▓▒░░░░░░
```

The boundary becomes less sharp.

An edge-preserving method tries to produce:

```text
██████░░░░░░
██████░░░░░░
██████░░░░░░
```

with less noise.

---

# 24.47 CT Restoration

CT images contain noise influenced by:

* photon statistics
* dose
* reconstruction method
* detector characteristics

Modern CT reconstruction may already incorporate sophisticated statistical/model-based processing.

Therefore a viewer should not blindly apply a generic denoising filter to diagnostic CT images.

---

# 24.48 MRI Restoration

MRI can contain:

* thermal noise
* motion artifacts
* intensity nonuniformity
* acquisition-specific artifacts

Different artifacts require different correction strategies.

For example:

```text
Noise
→ denoising

Motion
→ motion correction / reconstruction approaches

Bias field
→ intensity correction
```

These are different problems.

---

# 24.49 X-Ray Restoration

X-ray images can contain:

```text
Noise
Blur
Detector artifacts
Motion
```

Restoration can improve visualization, but again the processing must be validated before being used for diagnostic purposes.

---

# 24.50 Ultrasound Restoration

Ultrasound is particularly associated with:

```text
Speckle
```

and:

```text
Low contrast
```

Speckle reduction methods need to balance:

```text
Noise suppression
+
Anatomical boundary preservation
```

---

# 24.51 Restoration Pipeline for Your Software

A good architecture:

```text
DICOM / Raw Image
       ↓
Pixel Decode
       ↓
Image Model
       ↓
RestorationEngine
       │
       ├── Denoising
       ├── Deblurring
       ├── Noise Reduction
       ├── Artifact Reduction
       └── Specialized Filters
       ↓
Processed Image
       ↓
Window/Level
       ↓
Color Mapping
       ↓
Display
```

---

# 24.52 Do Not Destroy Original Data

This is extremely important for a medical viewer/editor.

Use:

```text
Original Image
      │
      ├──────────────→ Display
      │
      └──────────────→ Processing
                           ↓
                     Derived Image
```

rather than:

```text
Original
 ↓
Overwrite
 ↓
Restored
```

Prefer a non-destructive workflow.

---

# 24.53 Image Processing Pipeline

For your application:

```text
Original
  ↓
Clone / Derived representation
  ↓
Restoration
  ↓
Enhancement
  ↓
Visualization
```

The user should be able to:

```text
Reset
Undo
Redo
Compare
```

without losing original data.

---

# 24.54 C++ Restoration Interface

A clean interface:

```cpp
class IRestorationFilter
{
public:
    virtual ~IRestorationFilter() = default;

    virtual Image apply(
        const Image& input) = 0;
};
```

Implementations:

```text
GaussianDenoiser
MedianDenoiser
WienerFilter
DeblurFilter
AdaptiveMedian
```

---

# 24.55 Parameter Object

Avoid passing many parameters individually.

Use:

```cpp
struct RestorationParameters
{
    double sigma = 1.0;
    int kernelSize = 3;
    double noiseVariance = 0.0;
};
```

Then:

```cpp
Image apply(
    const Image& input,
    const RestorationParameters& params);
```

This makes the API easier to extend.

---

# 24.56 Strategy Pattern

Your architecture can use a strategy pattern:

```text
RestorationEngine
       │
       ├── MedianStrategy
       ├── WienerStrategy
       ├── GaussianStrategy
       └── DeblurStrategy
```

The controller selects the strategy.

This fits well with your existing software-architecture learning.

---

# 24.57 QML Integration

Example UI:

```text
Restoration
 ├── Method
 │    ├── Median
 │    ├── Wiener
 │    ├── Denoise
 │    └── Deblur
 │
 ├── Strength
 ├── Kernel Size
 ├── Noise Level
 └── Apply
```

Architecture:

```text
QML
 ↓
RestorationController
 ↓
RestorationEngine
 ↓
OpenCV / ITK / custom algorithm
 ↓
Derived Image
 ↓
Viewer
```

---

# 24.58 OpenCV Role

OpenCV can provide many practical image-processing operations:

```text
OpenCV
 ├── Median filtering
 ├── Gaussian filtering
 ├── FFT-based operations
 ├── Morphology
 ├── image transforms
 └── other restoration tools
```

For medical-specific volumetric processing, ITK can be more appropriate.

---

# 24.59 ITK Role

ITK is useful for:

```text
2D images
3D volumes
Medical image filtering
Registration
Segmentation
Resampling
```

Architecture:

```text
DICOM
 ↓
ITK image
 ↓
Restoration
 ↓
Medical processing
```

---

# 24.60 VTK Role

VTK is primarily useful for visualization and visualization-related processing:

```text
Restored volume
 ↓
VTK
 ↓
Volume rendering
 ↓
3D visualization
```

It is not necessary to force VTK to perform every restoration operation.

---

# 24.61 Restoration Quality Evaluation

Possible metrics include:

```text
MSE
PSNR
SSIM
```

But for medical imaging also consider:

```text
Edge preservation
Structure preservation
Quantitative accuracy
Clinical task performance
```

---

# 24.62 PSNR Reminder

[
MSE=
\frac1N\sum_i(I_i-\hat I_i)^2
]

Then:

[
PSNR=
10\log_{10}
\left(
\frac{MAX_I^2}{MSE}
\right)
]

Higher PSNR generally indicates smaller pixel-wise error.

---

# 24.63 SSIM Reminder

SSIM considers:

```text
Luminance
Contrast
Structure
```

It can be more informative than pure MSE for visual similarity, but neither SSIM nor PSNR alone establishes clinical suitability.

---

# 24.64 Restoration Validation

For an enterprise medical system:

```text
Input
 ↓
Known degradation
 ↓
Restoration
 ↓
Output
 ↓
Compare against reference
 ↓
Quantitative metrics
 ↓
Visual assessment
 ↓
Clinical/task-specific validation
```

---

# 24.65 Restoration vs Enhancement Table

| Restoration                | Enhancement                       |
| -------------------------- | --------------------------------- |
| Based on degradation model | Often appearance-driven           |
| Attempts recovery          | Improves visualization            |
| Deblurring                 | Sharpening                        |
| Wiener filtering           | Contrast enhancement              |
| Inverse filtering          | Histogram equalization            |
| Noise estimation important | Visual preference often important |

---

# 24.66 Restoration vs Denoising

Denoising is a subset of restoration when the degradation is modeled primarily as noise.

```text
Restoration
 │
 ├── Denoising
 ├── Deblurring
 ├── Deconvolution
 └── Artifact correction
```

---

# 24.67 Restoration vs Reconstruction

This distinction matters in medical imaging.

### Reconstruction

Raw acquisition data:

```text
Raw scanner data
 ↓
Reconstruction algorithm
 ↓
Image
```

### Restoration

Already reconstructed image:

```text
Degraded image
 ↓
Restoration
 ↓
Improved estimate
```

They are not necessarily the same operation.

---

# 24.68 Important Medical Principle

Do not assume:

[
\boxed{
Sharper = Better
}
]

An aggressive sharpening/restoration algorithm can introduce:

```text
False edges
Ringing
Noise amplification
Artificial structures
```

Therefore medical image processing must prioritize faithful representation of the underlying data.

---

# 24.69 Restoration Architecture

```text
                   RESTORATION ENGINE
                          │
             ┌────────────┼─────────────┐
             ↓            ↓             ↓
          Denoise      Deblur       Artifact
             │            │             │
       ┌─────┼─────┐      │             │
       ↓     ↓     ↓      ↓             ↓
    Median Gaussian Wiener  PSF       Specialized
                          │
                          ↓
                    Deconvolution
                          │
                          ↓
                     Derived Image
```

---

# 24.70 Chapter 24 Mental Model

```text
                 IMAGE RESTORATION
                        │
                        ↓
                 Degradation Model
                        │
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
        Noise          Blur        Artifacts
          │             │
    ┌─────┼─────┐      PSF
    ↓     ↓     ↓       │
 Gaussian Impulse Poisson
    │      │      │      │
    ↓      ↓      ↓      ↓
  Wiener  Median  Special  Deblur
                        │
                        ↓
                Inverse Problem
                        │
              ┌─────────┴─────────┐
              ↓                   ↓
        Inverse Filter          Wiener
              │                   │
              └─────────┬─────────┘
                        ↓
                 Restored Image
                        ↓
                Quality Evaluation
                        ↓
                  Medical Viewer
```

---

# 24.71 Key Formulas

### Degradation model

[
\boxed{
g=h*f+\eta
}
]

### Frequency-domain model

[
\boxed{
G=HF+N
}
]

### Inverse filter

[
\boxed{
\hat F=\frac{G}{H}
}
]

### Gaussian distribution

[
\boxed{
p(z)=
\frac{1}{\sqrt{2\pi\sigma^2}}
e^{-\frac{(z-\mu)^2}{2\sigma^2}}
}
]

### Poisson distribution

[
\boxed{
P(k)=\frac{\lambda^ke^{-\lambda}}{k!}
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

---

# 24.72 Most Important Concepts

Remember:

[
\boxed{
g=h*f+\eta
}
]

means:

```text
Observed
=
Blurred original
+
Noise
```

Then:

[
\boxed{
Restoration
===========

Estimate\ original
from\ observed
}
]

And:

```text
Inverse filtering
```

is powerful but unstable when:

[
H\approx0
]

while:

```text
Wiener filtering
```

adds a noise-aware tradeoff.

---

# 24.73 Knowledge Check

### Fundamentals

1. What is image restoration?
2. How is restoration different from enhancement?
3. What is the degradation model?
4. What is a PSF?
5. What is an inverse problem?

### Noise

6. What is Gaussian noise?
7. What parameters describe Gaussian noise?
8. What is salt-and-pepper noise?
9. Why is median filtering effective against impulse noise?
10. What is Poisson noise?
11. Why is Poisson noise relevant to imaging?
12. What is speckle noise?
13. Why is speckle often modeled multiplicatively?
14. What is periodic noise?

### Filtering

15. What is a median filter?
16. What is Gaussian filtering?
17. What is adaptive filtering?
18. Why can strong smoothing destroy edges?
19. What is Wiener filtering?
20. What is inverse filtering?
21. Why does inverse filtering amplify noise?
22. What is regularization?
23. What is constrained least squares?

### Deblurring

24. What is deblurring?
25. What is a PSF?
26. What is motion blur?
27. What is blind deconvolution?
28. What is the difference between known and unknown PSF?

### Medical imaging

29. What types of noise can appear in CT?
30. What types of degradation can occur in MRI?
31. Why is speckle important in ultrasound?
32. Why shouldn't restoration overwrite original medical data?
33. Why is "sharper" not necessarily "more accurate"?
34. Why must restoration algorithms be validated?
35. What is the difference between reconstruction and restoration?

---

# 24.74 Practical Exercise — Median Filter

Given:

```text
10  10  10
10 255  11
12  10  11
```

Calculate the median of the 3×3 neighborhood and determine the restored center pixel.

---

# 24.75 Practical Exercise — Degradation Model

Suppose:

[
g=h*f+n
]

Explain each component:

```text
g
h
f
n
```

Then explain what happens if:

[
n=0
]

and what happens if:

[
h=\delta
]

where (\delta) represents an ideal impulse response.

---

# 24.76 Practical Exercise — Inverse Filtering

Suppose:

[
G=HF
]

and:

[
H=0.5
]

[
G=20
]

Calculate:

[
F=\frac GH
]

Then repeat with:

[
H=0.001
]

and explain why small values of (H) make inverse filtering unstable in the presence of noise.

---

# 24.77 Practical Exercise — Medical Viewer

Design this feature:

```text
Image Restoration
 ├── Original
 ├── Median Denoise
 ├── Gaussian Denoise
 ├── Wiener
 ├── Deblur
 ├── Before/After
 ├── Split View
 ├── Reset
 └── Save Derived Image
```

Architecture:

```text
QML
 ↓
RestorationController
 ↓
RestorationEngine
 ↓
Filter Strategy
 ↓
Derived Image
 ↓
Viewer
```

The original DICOM pixel data must remain untouched.

---

# 24.78 Chapter 24 Complete

The core progression is:

```text
Degraded Image
      ↓
Understand Degradation
      ↓
Model Noise / Blur
      ↓
Estimate PSF / Noise
      ↓
Restoration Algorithm
      ↓
Denoised / Deblurred Image
      ↓
Validation
```

The four concepts you should remember most strongly are:

[
\boxed{
g=h*f+\eta
}
]

[
\boxed{
PSF = how\ an\ imaging\ system\ spreads\ a\ point
}
]

[
\boxed{
Inverse\ filtering = direct\ reversal,\ but\ noise\ sensitive
}
]

[
\boxed{
Wiener\ filtering = restoration\ with\ noise\ consideration
}
]

And for your medical imaging software:

```text
DICOM
 ↓
Decode
 ↓
Original Image Model
 ├──────────────→ Original Display
 │
 └──────────────→ RestorationEngine
                       ↓
                  Derived Image
                       ↓
                 Window / Level
                       ↓
                    LUT
                       ↓
                 QML / VTK
```

**Chapter 24 complete.**

### Next, strictly according to your index:

# Chapter 25 — Image Segmentation

Topics:

* What is segmentation?
* Segmentation vs detection
* Segmentation vs classification
* Thresholding
* Global thresholding
* Otsu thresholding
* Adaptive thresholding
* Region growing
* Region splitting
* Region merging
* Edge-based segmentation
* Watershed
* K-means segmentation
* Active contours
* Level sets
* Graph cuts
* Morphological segmentation
* Connected components
* 2D vs 3D segmentation
* Medical image segmentation
* CT/MRI/PET segmentation
* Organ segmentation
* Tumor segmentation
* AI segmentation overview
