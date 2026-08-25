# Chapter 14 — Frequency Domain

We continue **strictly according to your index**.

Chapter 14 topics:

1. Spatial vs frequency domain
2. Fourier transform
3. DFT
4. FFT
5. Frequency spectrum
6. Magnitude
7. Phase
8. Frequency shifting
9. Low-pass filtering
10. High-pass filtering
11. Frequency-domain filtering
12. Gaussian frequency filter
13. Ideal filter
14. Butterworth filter
15. Ringing artifacts

This chapter is important because until now we mostly processed images **directly in pixel space**. Now we transform the image into a representation based on its **spatial frequencies**.

---

# 14.1 Spatial Domain vs Frequency Domain

There are two major ways to look at an image.

### Spatial domain

We directly work with pixels:

[
f(x,y)
]

Example:

```text id="q2l5g7"
Pixel
  ↓
Neighborhood
  ↓
Filter
  ↓
New Pixel
```

Examples:

* Gaussian blur
* Median filter
* Sharpening
* Laplacian

---

### Frequency domain

We transform the image into frequency components:

[
F(u,v)
]

Then manipulate those components.

Pipeline:

```text id="m9v0y5"
Image
  ↓
Fourier Transform
  ↓
Frequency Representation
  ↓
Modify Frequencies
  ↓
Inverse Fourier Transform
  ↓
Image
```

---

# 14.2 What Is Spatial Frequency?

Spatial frequency describes **how quickly image intensity changes across space**.

Consider:

```text id="kq7g6s"
100 100 100 100 100
```

Very little variation.

Therefore:

```text
Low spatial frequency
```

Now:

```text id="8b0u8u"
0 255 0 255 0 255
```

Intensity changes rapidly.

Therefore:

```text
High spatial frequency
```

---

# 14.3 Intuitive Example

Imagine a smooth gradient:

```text id="b5g3f4"
10 20 30 40 50 60 70
```

This changes slowly.

It contains mostly low frequencies.

Now imagine:

```text id="f8f4g1"
0 255 0 255 0 255 0
```

This changes rapidly.

It contains strong high-frequency components.

Therefore:

```text id="r2h5h8"
Smooth structures
       ↓
Low frequency

Edges / fine details / noise
       ↓
High frequency
```

---

# 14.4 Why Fourier Transform?

The Fourier transform decomposes an image into sinusoidal frequency components.

Instead of asking:

> "What is the intensity at this pixel?"

we can ask:

> "What spatial frequencies make up this image?"

Conceptually:

```text id="m8y3x2"
Complex Image
     ↓
Many frequency components
     ↓
Low + medium + high frequencies
```

---

# 14.5 Fourier Transform

For a continuous 2D function:

[
\boxed{
F(u,v)=
\int\int
f(x,y)
e^{-j2\pi(ux+vy)}
dx,dy
}
]

Don't worry about memorizing the integral immediately.

The important concept is:

[
\boxed{
f(x,y)
\longleftrightarrow
F(u,v)
}
]

Spatial domain ↔ frequency domain.

---

# 14.6 Inverse Fourier Transform

After modifying the frequency representation, we need to reconstruct the image.

The inverse transform is:

[
\boxed{
f(x,y)=
\int\int
F(u,v)
e^{j2\pi(ux+vy)}
du,dv
}
]

So the complete process is:

```text id="6z0t6h"
f(x,y)
   ↓
Fourier Transform
   ↓
F(u,v)
   ↓
Filtering
   ↓
F'(u,v)
   ↓
Inverse Fourier Transform
   ↓
f'(x,y)
```

---

# 14.7 What Are DFT and FFT?

For digital images, we usually work with a **Discrete Fourier Transform**.

DFT:

> Discrete Fourier Transform

FFT:

> Fast Fourier Transform

Important:

[
\boxed{
FFT \neq different\ transform
}
]

FFT is an efficient algorithm for calculating the DFT.

---

# 14.8 2D DFT

For an (M\times N) image:

[
\boxed{
F(u,v)=
\sum_{x=0}^{M-1}
\sum_{y=0}^{N-1}
f(x,y)
e^{-j2\pi
\left(
\frac{ux}{M}
+
\frac{vy}{N}
\right)}
}
]

The output is a complex-valued frequency representation.

---

# 14.9 Why Is the Fourier Output Complex?

A Fourier coefficient contains:

* magnitude
* phase

So:

[
\boxed{
F(u,v)=
|F(u,v)|e^{j\phi(u,v)}
}
]

where:

[
|F|
]

is magnitude and:

[
\phi
]

is phase.

---

# 14.10 Magnitude

Magnitude tells us the strength of a frequency component.

For complex number:

[
z=a+jb
]

magnitude:

[
\boxed{
|z|=\sqrt{a^2+b^2}
}
]

For Fourier coefficient:

[
F(u,v)=R+jI
]

then:

[
|F(u,v)|
========

\sqrt{R^2+I^2}
]

---

# 14.11 Phase

Phase tells us the positional/structural relationship of frequency components.

For:

[
z=a+jb
]

phase:

[
\boxed{
\phi=\operatorname{atan2}(b,a)
}
]

This is extremely important.

A common beginner mistake is:

> "Magnitude contains all the image information."

It does not.

Phase carries crucial structural information.

---

# 14.12 Magnitude vs Phase

Think:

```text id="9xq7tb"
Fourier Transform
       │
       ├──────────────┐
       ↓              ↓
    Magnitude        Phase
       │              │
   Strength       Position/
   of frequency    structure
```

Both together reconstruct the image.

---

# 14.13 Frequency Spectrum

The Fourier transform is often visualized as a frequency spectrum.

Usually we calculate:

[
|F(u,v)|
]

and display it using logarithmic scaling:

[
\boxed{
S(u,v)=
\log(1+|F(u,v)|)
}
]

Why?

Because Fourier magnitudes can have a very large dynamic range.

Without logarithmic scaling, weaker frequencies may be invisible.

---

# 14.14 Frequency Spectrum Visualization

Typically, after shifting low frequencies to the center:

```text id="d6x5kq"
+-----------------------+
|                       |
|       High            |
|        ↓              |
|     · · · ·           |
|    ·  LOW  ·          |
|     · · · ·           |
|                       |
+-----------------------+
```

Center:

```text
Low frequencies
```

Farther away:

```text
High frequencies
```

---

# 14.15 Frequency Shifting

The raw DFT arrangement usually places the zero-frequency component at a corner.

For visualization, we often shift it to the center.

This is commonly called:

[
\boxed{
FFTShift
}
]

Conceptually:

```text id="x7s8f2"
Before:

[ Low | High ]
[ High| High ]


After:

[ High | High ]
[ High | Low  ]
```

More intuitively:

```text id="j2v7l3"
Raw spectrum
      ↓
Shift quadrants
      ↓
Low frequencies at center
```

---

# 14.16 Why Put Low Frequencies in the Center?

It makes the spectrum easier to interpret.

Then:

```text id="8j7q4b"
Center
 ↓
Low frequency

Distance from center
 ↓
Increasing frequency
```

So a circular low-pass filter becomes easy to visualize.

---

# 14.17 Low-Pass Filtering in Frequency Domain

A low-pass frequency filter keeps low frequencies and suppresses high frequencies.

Mathematically:

[
\boxed{
G(u,v)=H(u,v)F(u,v)
}
]

where:

* (F) = Fourier transform of image
* (H) = filter transfer function
* (G) = filtered spectrum

Then:

[
g(x,y)=\mathcal F^{-1}{G(u,v)}
]

---

# 14.18 Frequency-Domain Filtering Pipeline

```text id="9b9k31"
Image
 ↓
DFT / FFT
 ↓
F(u,v)
 ↓
Create H(u,v)
 ↓
Multiply
 ↓
G(u,v)
 ↓
Inverse FFT
 ↓
Filtered Image
```

This is the frequency-domain equivalent of spatial filtering.

---

# 14.19 Ideal Low-Pass Filter

The simplest low-pass filter uses a hard cutoff.

Let:

[
D(u,v)
]

be distance from the frequency center.

Then:

[
\boxed{
H(u,v)=
\begin{cases}
1,&D(u,v)\le D_0\
0,&D(u,v)>D_0
\end{cases}
}
]

where:

[
D_0
]

is the cutoff frequency.

---

# 14.20 Ideal Low-Pass Visualization

```text id="c2u0tq"
High frequencies
     ↓
XXXXXXXXXXXXXXX
XXX   LOW   XXX
XX   █████   XX
XXX   ███   XXX
XXXXXXXXXXXXXXX
```

The central circle is retained.

Everything outside is removed.

---

# 14.21 What Does Increasing (D_0) Do?

Small cutoff:

```text id="x3w1g5"
Few frequencies retained
 ↓
Strong blur
```

Large cutoff:

```text id="g8m4j1"
More frequencies retained
 ↓
Less blur
```

So:

[
D_0\uparrow
\Rightarrow
\text{less smoothing}
]

approximately.

---

# 14.22 Why Is the Ideal Filter Problematic?

The cutoff is abrupt:

```text id="4x8q7j"
1 ──────────┐
            │
            └──────── 0
             cutoff
```

This sharp transition in frequency domain can cause oscillations in the spatial domain.

These are called:

[
\boxed{
Ringing\ artifacts
}
]

---

# 14.23 Ringing Artifacts

Near a sharp edge, ideal frequency filtering can produce:

```text id="f9u5z4"
-----~~~~~────────
     ↑
  oscillations
```

Instead of:

```text id="v4q7pj"
-----─────────────
```

These oscillations are often called **Gibbs-type ringing**.

---

# 14.24 Why Ringing Happens

A sharp cutoff in frequency domain corresponds to a long oscillatory response in spatial domain.

Conceptually:

```text id="9w0z4v"
Abrupt frequency cutoff
        ↓
Oscillatory spatial response
        ↓
Ringing
```

This is one reason practical filters use smoother transitions.

---

# 14.25 Butterworth Low-Pass Filter

Butterworth provides a smoother transition.

A common form:

[
\boxed{
H(u,v)=
\frac{1}
{1+
\left(
\frac{D(u,v)}{D_0}
\right)^{2n}
}
}
]

where:

* (D_0) = cutoff
* (n) = filter order

---

# 14.26 Butterworth Order

Small order:

```text id="6d7d7h"
Smooth transition
```

Large order:

```text id="x2s8w4"
Sharper transition
```

As (n) becomes very large, the behavior approaches an ideal cutoff.

So:

```text id="7c4fcm"
Low n
 ↓
smooth transition

High n
 ↓
sharper transition
 ↓
more ringing risk
```

---

# 14.27 Gaussian Frequency Filter

A Gaussian low-pass frequency response is:

[
\boxed{
H(u,v)=
e^{-\frac{D^2(u,v)}
{2D_0^2}}
}
]

It decreases smoothly as frequency distance increases.

Unlike the ideal filter:

```text id="g4w1nt"
No abrupt cutoff
```

Therefore it tends to produce less ringing.

---

# 14.28 Ideal vs Butterworth vs Gaussian

| Filter      | Transition  | Ringing  | Control        |
| ----------- | ----------- | -------- | -------------- |
| Ideal       | Abrupt      | High     | Cutoff         |
| Butterworth | Adjustable  | Moderate | Cutoff + order |
| Gaussian    | Very smooth | Low      | Sigma/cutoff   |

Mental model:

```text id="3v9p7n"
Ideal
█████████|░░░░░░
          ↑
       abrupt

Butterworth
██████████▓▓░░░░
          ↑
       gradual

Gaussian
█████████▓▓▒▒░░
        smooth
```

---

# 14.29 High-Pass Filtering in Frequency Domain

A high-pass filter does the opposite.

It removes low frequencies and keeps high frequencies.

One simple relationship is:

[
\boxed{
H_{HP}=1-H_{LP}
}
]

where:

* (H_{LP}) = low-pass filter
* (H_{HP}) = corresponding high-pass filter

---

# 14.30 High-Pass Frequency Spectrum

```text id="0h1y5m"
XXXXXXXXXXXXXXX
XXX █████ █████
XX ███████████ XX
XXX █████ █████
XXXXXXXXXXXXXXX
```

Here the center is suppressed.

The outer frequencies remain.

---

# 14.31 What Does High-Pass Do to an Image?

Because high frequencies contain:

* edges
* fine details
* noise

high-pass filtering produces an image emphasizing local changes.

Conceptually:

```text id="ny4r9a"
Image
 ↓
High-pass
 ↓
Edges + fine details + noise
```

This is why high-pass filtering can be used for edge extraction and sharpening.

---

# 14.32 Frequency-Domain Sharpening

We can construct:

[
H_{sharp}(u,v)
]

that preserves more high-frequency content.

One conceptual approach:

[
\boxed{
H_{sharp}=1+kH_{HP}
}
]

Then:

[
G=H_{sharp}F
]

and:

[
g=\mathcal F^{-1}(G)
]

This increases high-frequency components.

---

# 14.33 Frequency Domain vs Spatial Domain

These two approaches can produce related operations.

For linear shift-invariant filters:

[
\boxed{
Convolution\ in\ spatial\ domain
\longleftrightarrow
Multiplication\ in\ frequency\ domain
}
]

This is the **convolution theorem**.

---

# 14.34 Convolution Theorem

If:

[
g=f*h
]

then:

[
\boxed{
G=F\cdot H
}
]

where:

* (f) = image
* (h) = spatial filter
* (F) = Fourier transform of image
* (H) = Fourier transform of filter
* (G) = Fourier transform of output

This is one of the most important equations in image processing.

---

# 14.35 Why Is This Useful?

Spatial convolution:

[
g=f*h
]

can be expensive for very large kernels.

Frequency-domain filtering:

```text id="z1q6k8"
FFT(image)
      ↓
multiply by filter
      ↓
Inverse FFT
```

can be advantageous for certain large-kernel problems.

But FFT itself has computational and memory overhead, so it isn't automatically faster for every filter.

---

# 14.36 DFT Complexity

A direct 1D DFT is approximately:

[
O(N^2)
]

For 2D direct DFT, the cost is correspondingly large.

FFT algorithms reduce this substantially.

For a 1D FFT:

[
\boxed{
O(N\log N)
}
]

A 2D FFT can be computed efficiently by applying 1D FFTs along rows and columns.

---

# 14.37 2D FFT Concept

Instead of directly calculating every 2D frequency:

```text id="z8z6ck"
2D FFT
```

we can conceptually do:

```text id="8p4wq2"
Image
 ↓
1D FFT on every row
 ↓
1D FFT on every column
 ↓
2D FFT
```

This is one common implementation strategy.

---

# 14.38 Why FFT Is Important for Medical Imaging

Frequency-domain processing can be useful for:

* large-kernel filtering
* reconstruction algorithms
* image analysis
* periodic noise removal
* MRI-related reconstruction concepts
* computational imaging

But not every medical-image operation should be moved into frequency space.

---

# 14.39 Periodic Noise

Frequency domain is especially useful for periodic noise.

Suppose an image has:

```text id="r8c7z1"
repeating stripes
```

This produces concentrated frequency components.

In the spectrum:

```text id="d4x6z1"
      •
       \
        CENTER
       /
      •
```

Distinct peaks may appear away from the center.

These can potentially be attenuated with notch filters.

Notch filtering is a later frequency-domain concept.

---

# 14.40 Frequency Spectrum Interpretation

### Bright center

Strong low frequencies.

Typical of:

```text
smooth intensity variations
```

### Bright outer regions

Strong high frequencies.

Typical of:

```text
edges
fine texture
noise
```

### Directional structures

Can create directional patterns in the spectrum.

This is useful for analyzing periodic or oriented structures.

---

# 14.41 Phase Importance

Let's emphasize this again.

Suppose:

[
F=M e^{j\phi}
]

The magnitude:

[
M
]

describes frequency strength.

The phase:

[
\phi
]

contains positional relationships.

If you preserve magnitude but destroy phase, the reconstructed image can lose recognizable structure.

Therefore:

[
\boxed{
Phase\ is\ essential
}
]

---

# 14.42 Fourier Transform of a Constant Image

Consider:

```text id="r4x8x0"
100 100 100
100 100 100
100 100 100
```

This image has only a DC/zero-frequency component.

So most energy is concentrated at:

```text id="6z2p2y"
zero frequency
```

After shifting:

```text id="0p0z8n"
center = very bright
```

This is a useful way to understand the spectrum.

---

# 14.43 DC Component

The zero-frequency component is often called the:

[
\boxed{
DC\ component
}
]

It relates to the image's average intensity.

For an image with (N) pixels:

[
F(0,0)
]

is proportional to the sum of pixel values.

Therefore:

```text id="e6n5s2"
Image average brightness
        ↓
DC component
```

---

# 14.44 Frequency Filter Design

To design a frequency filter:

### Step 1

Choose desired behavior.

```text
Blur?
Sharpen?
Remove periodic noise?
```

### Step 2

Define:

[
H(u,v)
]

### Step 3

Calculate:

[
F(u,v)
]

### Step 4

Multiply:

[
G=HF
]

### Step 5

Inverse transform.

```text id="kj2b7g"
Image
 ↓
FFT
 ↓
H(u,v)
 ↓
Multiply
 ↓
IFFT
```

---

# 14.45 Ideal Filter Problem

The ideal filter's response is:

```text id="p6w0d9"
1 ──────────┐
            │
            └────────── 0
```

The abrupt discontinuity causes ringing.

Therefore:

```text id="0j9v2w"
Ideal
 ↓
Sharp frequency boundary
 ↓
Potential ringing
```

---

# 14.46 Gaussian Filter Advantage

Gaussian:

```text id="x4u1x7"
1 ───────╲
          ╲
           ╲____ 0
```

Smooth transition.

Therefore:

```text id="y7m5h9"
Smooth frequency transition
       ↓
Less ringing
```

This is one reason Gaussian filters are widely used.

---

# 14.47 Butterworth Advantage

Butterworth gives adjustable transition steepness:

[
n
]

controls how abrupt the transition is.

Therefore:

```text id="k2v6jp"
n small
 ↓
smooth

n medium
 ↓
moderately sharp

n large
 ↓
near-ideal
```

This gives more control than Gaussian.

---

# 14.48 Frequency Domain in Medical Image Enhancement

For a medical viewer:

```text id="j9x0f7"
DICOM
  ↓
Pixel data
  ↓
FFT
  ↓
Frequency analysis
  ↓
Optional filter
  ↓
IFFT
  ↓
Display pipeline
```

But for routine window/level operations, frequency-domain processing is unnecessary.

Window/level is fundamentally an intensity mapping.

---

# 14.49 Important Distinction

Do not confuse:

```text id="e7j8s4"
Window/Level
```

with:

```text
Low-pass / High-pass
```

Window/level changes intensity mapping.

Frequency filtering changes spatial-frequency content.

So:

[
\boxed{
Window/Level \neq Frequency Filtering
}
]

---

# 14.50 CT Example

Suppose a CT image contains:

```text id="w0u5e8"
Soft tissue
Bone edges
Noise
```

A low-pass frequency filter:

```text id="x8s3h1"
Low-pass
 ↓
Noise ↓
Fine edges ↓
```

A high-pass:

```text id="p6s7f4"
High-pass
 ↓
Edges ↑
Noise ↑
```

Therefore filtering must be chosen according to the imaging goal.

---

# 14.51 Frequency-Domain Pipeline Architecture

For your enterprise image-processing system:

```text id="3x4z1w"
FrequencyProcessor
       │
       ├── ForwardTransform
       ├── Spectrum
       ├── Magnitude
       ├── Phase
       ├── Shift
       ├── Filter
       │     ├── Ideal
       │     ├── Gaussian
       │     └── Butterworth
       └── InverseTransform
```

This should be separate from the QML/UI layer.

---

# 14.52 C++ Conceptual Structure

```cpp id="9o6s8p"
class FrequencyProcessor
{
public:

    ComplexImage forward(
        const Image& image);

    Image inverse(
        const ComplexImage& spectrum);

    Image magnitude(
        const ComplexImage& spectrum);

    Image phase(
        const ComplexImage& spectrum);
};
```

Then:

```cpp id="b4p2b8"
class FrequencyFilter
{
public:

    virtual ~FrequencyFilter() = default;

    virtual void apply(
        ComplexImage& spectrum) = 0;
};
```

Implementations:

```text id="g5m7p0"
IdealLowPass
GaussianLowPass
ButterworthLowPass
IdealHighPass
GaussianHighPass
ButterworthHighPass
```

---

# 14.53 Important Numerical Issues

Real implementations must consider:

* complex floating-point values
* FFT normalization conventions
* image dimensions
* padding
* frequency indexing
* FFT shift
* inverse transform scaling
* numerical precision
* negative values after inverse FFT
* output clipping
* memory consumption

Different FFT libraries can use different normalization conventions.

Therefore don't assume:

> "Forward FFT + inverse FFT always automatically returns exactly the original array."

You must understand the library's normalization rules.

---

# 14.54 Frequency Domain and FFT Libraries

In production medical software, you generally shouldn't write a high-performance FFT implementation from scratch unless there is a strong reason.

Libraries such as:

* FFTW
* Intel oneMKL
* vendor/GPU FFT libraries

can provide optimized implementations.

For your broader medical-imaging architecture, FFT should be treated as an algorithmic service rather than something coupled directly to QML.

---

# 14.55 Spatial vs Frequency Domain — Final Comparison

| Property             | Spatial Domain          | Frequency Domain             |
| -------------------- | ----------------------- | ---------------------------- |
| Works with           | Pixels                  | Frequency components         |
| Basic operation      | Neighborhood processing | Spectrum manipulation        |
| Typical filters      | Gaussian, median        | Ideal, Gaussian, Butterworth |
| Nonlinear filters    | Easy                    | Less direct                  |
| Large linear kernels | Can be expensive        | FFT may help                 |
| Interpretation       | Direct/intuitive        | More mathematical            |
| Edge/noise analysis  | Local                   | Frequency-based              |
| Periodic noise       | Less convenient         | Very useful                  |

---

# 14.56 Chapter 14 Mental Model

Memorize this:

```text id="7q8g0d"
                    IMAGE
                      │
                      ▼
                    FFT
                      │
                      ▼
              FREQUENCY DOMAIN
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Magnitude     Phase      Spectrum
                      │
                      ↓
                  FFT Shift
                      │
             ┌────────┴────────┐
             ↓                 ↓
         Low-pass           High-pass
             │                 │
       ┌─────┼─────┐       ┌───┼────┐
       ↓     ↓     ↓       ↓   ↓    ↓
     Ideal Gaussian BW   Ideal Gaussian BW
             │                 │
             └────────┬────────┘
                      ↓
                    IFFT
                      ↓
                 Filtered Image
```

---

# 14.57 Key Formulas

### DFT

[
\boxed{
F(u,v)=
\sum_x\sum_y
f(x,y)e^{-j2\pi(\frac{ux}{M}+\frac{vy}{N})}
}
]

### Inverse DFT

[
\boxed{
f(x,y)=
\frac{1}{MN}
\sum_u\sum_v
F(u,v)e^{j2\pi(\frac{ux}{M}+\frac{vy}{N})}
}
]

### Magnitude

[
\boxed{
|F|=\sqrt{Re(F)^2+Im(F)^2}
}
]

### Phase

[
\boxed{
\phi=\operatorname{atan2}(Im(F),Re(F))
}
]

### Frequency filtering

[
\boxed{
G=HF
}
]

### Gaussian low-pass

[
\boxed{
H=
e^{-\frac{D^2}{2D_0^2}}
}
]

### Butterworth low-pass

[
\boxed{
H=
\frac{1}
{1+(D/D_0)^{2n}}
}
]

### High-pass

[
\boxed{
H_{HP}=1-H_{LP}
}
]

---

# 14.58 Knowledge Check

### Fundamentals

1. What is the difference between spatial and frequency domains?
2. What is spatial frequency?
3. What does the Fourier transform do?
4. What is the DFT?
5. What is the FFT?

### Spectrum

6. What is magnitude?
7. What is phase?
8. Why is phase important?
9. Why do we use logarithmic magnitude visualization?
10. What is the DC component?
11. Why do we shift the spectrum?

### Filtering

12. What is frequency-domain filtering?
13. What does a low-pass filter do?
14. What does a high-pass filter do?
15. What is the difference between Ideal, Gaussian, and Butterworth filters?

### Artifacts

16. What is ringing?
17. Why does an ideal filter cause ringing?
18. How does Gaussian filtering reduce ringing?
19. How does Butterworth order affect the transition?

### Advanced

20. State the convolution theorem.
21. Why can FFT-based filtering be useful for large kernels?
22. Why isn't FFT automatically faster for every filter?
23. Why are complex numbers required?
24. What is the difference between FFT and DFT?

### Medical Imaging

25. Why might frequency filtering be useful for periodic noise?
26. Why is frequency filtering different from window/level?
27. Why must FFT normalization conventions be checked?
28. Why does 3D medical imaging make FFT processing more computationally demanding?

---

# 14.59 Practical Exercise — Frequency Concept

Consider:

```text id="6z5t1d"
Image A:

100 100 100 100
100 100 100 100
100 100 100 100
100 100 100 100
```

Question:

Where should most of its Fourier energy occur?

**Answer:** At the zero/low-frequency component because the image is constant.

---

# 14.60 Practical Exercise — High Frequency

Consider:

```text id="6g4k2r"
0 255 0 255
0 255 0 255
0 255 0 255
0 255 0 255
```

This has rapid spatial variation.

Therefore its frequency spectrum contains strong higher-frequency components.

---

# 14.61 Practical Exercise — Filter Selection

You have three problems:

### Problem A

Image contains Gaussian-like random noise.

Choose:

**Gaussian low-pass**

### Problem B

Image contains periodic stripe noise.

Choose a frequency-domain approach and investigate the spectrum for distinct peaks.

### Problem C

You want to emphasize edges.

Choose:

**high-pass / sharpening**

But remember:

```text
edge enhancement
+
noise enhancement
```

can occur together.

---

# 14.62 Medical Imaging Exercise

Suppose you are processing a CT image:

```text id="e5x7v8"
CT
 ↓
FFT
 ↓
Spectrum
```

You see:

```text
Strong central component
+
several symmetrical bright peaks
```

What might those off-center peaks suggest?

They may indicate **periodic or directional structures/noise**, depending on the image and acquisition.

Do not automatically classify them as artifacts without investigating the image and acquisition context.

---

# 14.63 Chapter 14 Complete

The fundamental relationship is:

[
\boxed{
Spatial\ Domain
\overset{FFT}{\longrightarrow}
Frequency\ Domain
}
]

and:

[
\boxed{
Frequency\ Domain
\overset{IFFT}{\longrightarrow}
Spatial\ Domain
}
]

The most important equation in this chapter is:

[
\boxed{
G(u,v)=H(u,v)F(u,v)
}
]

And the most important conceptual relationship is:

```text id="6h2s9x"
Low frequency
   ↓
Smooth / slowly changing structures

High frequency
   ↓
Edges + fine detail + noise
```

Finally:

```text id="n1f2c5"
Ideal
   ↓
Sharp cutoff
   ↓
Ringing risk

Butterworth
   ↓
Adjustable transition

Gaussian
   ↓
Smooth transition
   ↓
Low ringing
```

**Chapter 14 complete.**

### Next, strictly according to your index:

# Chapter 15 — Edge Detection

Topics:

* Edge concept
* Edge types
* Gradient
* First derivative
* Second derivative
* Sobel
* Prewitt
* Roberts
* Laplacian
* Laplacian of Gaussian
* Canny edge detector
* Non-maximum suppression
* Hysteresis thresholding
