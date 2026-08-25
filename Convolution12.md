# Chapter 12 — Convolution

We continue **strictly according to your index**.

Chapter 12 topics:

1. Convolution concept
2. Kernel
3. Kernel size
4. Kernel weights
5. 2D convolution
6. Padding
7. Stride
8. Correlation vs convolution
9. Separable convolution
10. Convolution examples
11. Gaussian blur
12. Box blur
13. Sharpening kernels

---

# 12.1 What Is Convolution?

In Chapter 11, we learned:

> An output pixel can depend on a neighborhood.

Convolution provides a **mathematical framework for combining a neighborhood with a set of weights**.

For an image (f) and kernel (K):

[
\boxed{
g(x,y)=
\sum_i\sum_j
K(i,j)f(x-i,y-j)
}
]

Conceptually:

```text id="m2q7pd"
                 IMAGE
                   │
                   ▼
             Neighborhood
                   │
                   +
                   │
                Kernel
                   │
                   ▼
              Weighted Sum
                   │
                   ▼
              Output Pixel
```

---

# 12.2 Why Convolution?

A mean filter says:

```text id="6fz7qt"
Take neighborhood
 ↓
Give every pixel equal weight
 ↓
Average
```

Convolution generalizes this:

```text id="e7k6wb"
Take neighborhood
 ↓
Give pixels different weights
 ↓
Weighted sum
```

This allows us to create:

* blur filters
* edge detectors
* sharpening filters
* gradient filters
* Gaussian filters
* feature detectors

---

# 12.3 What Is a Kernel?

A **kernel** is a small matrix of weights.

For example:

[
\boxed{
K=
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
}
]

or:

[
\boxed{
K=
\begin{bmatrix}
0&-1&0\
-1&5&-1\
0&-1&0
\end{bmatrix}
}
]

The kernel defines how neighboring pixels contribute to the output.

Think:

```text id="8g2p7y"
Kernel
   ↓
"What importance should each neighbor have?"
```

---

# 12.4 Kernel Size

Common kernel sizes:

```text id="11u0a5"
3 × 3
5 × 5
7 × 7
9 × 9
```

A (3\times3) kernel:

```text id="0whv1r"
[ a b c ]
[ d e f ]
[ g h i ]
```

contains:

[
9
]

weights.

A (5\times5) kernel contains:

[
25
]

weights.

---

# 12.5 Why Kernel Size Matters

Small kernel:

```text id="r4smce"
3×3
 ↓
small neighborhood
 ↓
fine local effects
```

Large kernel:

```text id="fptc9j"
9×9
 ↓
larger neighborhood
 ↓
broader effect
```

Larger kernels generally require more computation.

For a (K\times K) kernel:

[
K^2
]

weights are applied per output pixel in a naive implementation.

---

# 12.6 Kernel Weights

The values inside the kernel determine the operation.

Example:

```text id="k4y8z7"
1 1 1
1 1 1
1 1 1
```

All pixels have equal weight.

Compare:

```text id="a8u5zy"
0 1 0
1 4 1
0 1 0
```

The center gets greater importance.

So:

> **Kernel values determine the behavior of the filter.**

---

# 12.7 Weighted Sum

Suppose:

```text id="9xv6n5"
Image neighborhood:

10 20 30
40 50 60
70 80 90
```

and:

```text id="m8uj7x"
Kernel:

1 0 0
0 1 0
0 0 1
```

Then the weighted sum is:

[
10(1)+20(0)+30(0)
]

[
+40(0)+50(1)+60(0)
]

[
+70(0)+80(0)+90(1)
]

Therefore:

[
10+50+90=150
]

The output center becomes:

[
150
]

before any additional scaling/clamping.

---

# 12.8 2D Convolution

For an image (f(x,y)) and kernel (h(i,j)):

[
\boxed{
g(x,y)=
\sum_i\sum_j
h(i,j)f(x-i,y-j)
}
]

For a (3\times3) kernel:

[
g(x,y)=
\sum_{i=-1}^{1}
\sum_{j=-1}^{1}
h(i,j)f(x-i,y-j)
]

The exact indexing convention depends on implementation, but the mathematical principle is a weighted neighborhood sum.

---

# 12.9 The Convolution Process

Suppose:

```text id="2x3p4v"
Image

1 2 3 4 5
6 7 8 9 10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25
```

Kernel:

```text id="0o4p1a"
1 1 1
1 1 1
1 1 1
```

Place kernel over the neighborhood:

```text id="6v08fs"
1  2  3
6  7  8
11 12 13
```

Multiply corresponding values:

```text id="k2v6n0"
1×1 + 2×1 + 3×1
6×1 + 7×1 + 8×1
11×1 + 12×1 + 13×1
```

Sum:

[
1+2+3+6+7+8+11+12+13
]

[
=63
]

Then move the kernel to the next position.

---

# 12.10 Convolution and the Mean Filter

Remember Chapter 11's mean filter:

[
\frac{1}{9}
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
]

This is actually a convolution kernel.

Therefore:

[
\boxed{
\text{Mean Filter}
==================

\text{Convolution with Box Kernel}
}
]

This is an important connection between Chapters 11 and 12.

---

# 12.11 Box Blur

A box blur uses equal weights.

For (3\times3):

[
\boxed{
K=
\frac{1}{9}
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
}
]

It smooths the image.

Example concept:

```text id="9j8bq3"
Sharp image
    ↓
Box convolution
    ↓
Smoothed image
```

---

# 12.12 Why Does Box Blur Blur?

Each output pixel becomes an average of neighboring pixels.

Sharp transitions are therefore spread over multiple pixels.

For example:

```text id="8q0q87"
0 0 0 255 255 255
```

After averaging, the transition becomes more gradual.

Conceptually:

```text id="0tq0bk"
Sharp edge
    ↓
Neighbor averaging
    ↓
Smoother transition
```

---

# 12.13 Kernel Normalization

For many smoothing filters, kernel weights are normalized so that:

[
\boxed{
\sum K(i,j)=1
}
]

For example:

[
\frac{1}{9}
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
]

has:

[
9\times\frac19=1
]

This helps preserve the average brightness for a uniform image.

---

# 12.14 What Happens If Kernel Sum Is Not 1?

Suppose:

[
K=
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
]

without dividing by 9.

For a constant image:

```text id="4u6p20"
100 100 100
100 100 100
100 100 100
```

output:

[
9\times100=900
]

So the image becomes much brighter and may overflow an 8-bit output.

Therefore:

> **Smoothing kernels are often normalized.**

---

# 12.15 Padding

What happens at image boundaries?

A (3\times3) kernel centered at a corner needs pixels outside the image.

We need **padding**.

Conceptually:

```text id="w9o8g7"
Original Image
      ↓
Padding
      ↓
Convolution
```

---

# 12.16 Common Padding Methods

### 1. Zero padding

Outside values are:

[
0
]

```text id="q6p7fk"
0 0 0 0 0
0 A B C 0
0 D E F 0
0 G H I 0
0 0 0 0 0
```

---

### 2. Replicate padding

Repeat border values.

```text id="z1m1wv"
A A B C C
A A B C C
D D E F F
G G H I I
G G H I I
```

---

### 3. Reflect padding

Reflect the image around the boundary.

---

### 4. Circular / wrap padding

The opposite edge wraps around.

```text id="e6z7g5"
left ↔ right
top ↔ bottom
```

---

# 12.17 Padding and Medical Imaging

Padding isn't just a technical detail.

Different padding methods can create different values near image borders.

For a medical image:

```text id="q0k1m5"
Anatomy near border
       ↓
Padding choice
       ↓
Different result
```

Therefore a production medical-image-processing pipeline should explicitly define the boundary policy.

---

# 12.18 Stride

Stride determines how far the kernel moves between operations.

### Stride = 1

Move one pixel:

```text id="4s5f3k"
→
→
→
```

### Stride = 2

Move two pixels:

```text id="l4d1a8"
→→
→→
```

For normal image filtering, stride is usually:

[
\boxed{1}
]

---

# 12.19 Why Stride Matters

Stride changes the output dimensions.

Suppose input width is:

[
W
]

kernel width:

[
K
]

padding:

[
P
]

stride:

[
S
]

Then:

[
\boxed{
W_{out}
=======

\left\lfloor
\frac{W+2P-K}{S}
\right\rfloor+1
}
]

Similarly:

[
\boxed{
H_{out}
=======

\left\lfloor
\frac{H+2P-K}{S}
\right\rfloor+1
}
]

This formula is especially important in CNNs later.

---

# 12.20 Example of Output Size

Input:

[
5\times5
]

Kernel:

[
3\times3
]

Padding:

[
0
]

Stride:

[
1
]

Then:

[
W_{out}
=======

\frac{5-3}{1}+1
]

[
=3
]

Therefore:

[
\boxed{3\times3}
]

output.

---

# 12.21 Same Padding

If we use:

[
P=1
]

for a (3\times3) kernel and:

[
S=1
]

then:

[
W_{out}
=======

\frac{5+2-3}{1}+1
]

[
=5
]

So:

[
5\times5
\rightarrow
5\times5
]

This is often called **same-size convolution**.

---

# 12.22 Correlation vs Convolution

This is a very important distinction.

### Correlation

The kernel is applied without flipping.

[
g(x,y)=
\sum_{i,j}
K(i,j)f(x+i,y+j)
]

### Convolution

The kernel is mathematically flipped before application.

[
g(x,y)=
\sum_{i,j}
K(i,j)f(x-i,y-j)
]

For a 2D kernel, flipping means:

```text id="r8pp3u"
180° rotation
```

---

# 12.23 Kernel Flipping Example

Original:

```text id="z8d4vx"
1 2 3
4 5 6
7 8 9
```

Flipped:

```text id="l9k6hv"
9 8 7
6 5 4
3 2 1
```

This is the mathematical convolution kernel.

---

# 12.24 Why Does This Matter?

Many image-processing libraries call their operation "convolution" even though internally they perform correlation.

For **symmetric kernels**, there is no difference.

Example:

```text id="p4a1mm"
1 2 1
2 4 2
1 2 1
```

Flipping gives the same kernel.

Therefore:

[
\boxed{
\text{Symmetric kernel: correlation = convolution}
}
]

But for directional kernels, the distinction matters.

---

# 12.25 Edge Detection Example

Consider:

[
K=
\begin{bmatrix}
-1&0&1
\end{bmatrix}
]

This detects horizontal intensity changes.

Its flipped version is:

[
\begin{bmatrix}
1&0&-1
\end{bmatrix}
]

The response changes sign.

So for edge detection, convolution vs correlation can matter.

---

# 12.26 Separable Convolution

A 2D kernel is **separable** if it can be represented as:

[
K=u,v^T
]

For example:

[
K=
\begin{bmatrix}
1\
2\
1
\end{bmatrix}
\begin{bmatrix}
1&2&1
\end{bmatrix}
]

which gives:

[
K=
\begin{bmatrix}
1&2&1\
2&4&2\
1&2&1
\end{bmatrix}
]

---

# 12.27 Why Is Separable Convolution Important?

A (3\times3) convolution needs:

[
9
]

kernel operations per pixel.

A separable implementation can perform:

```text id="y3w0j1"
Horizontal 1×3
       ↓
Vertical 3×1
```

Only:

[
3+3=6
]

kernel weights per pixel instead of 9, ignoring implementation overhead and boundary details.

For larger kernels, the savings become much more significant.

For a (K\times K) separable kernel:

```text id="o9x9e8"
K² operations
     ↓
approximately 2K
```

per pixel.

---

# 12.28 Gaussian Kernel

A Gaussian blur uses a Gaussian-shaped weighting function.

In 2D:

[
\boxed{
G(x,y)=
\frac{1}{2\pi\sigma^2}
e^{-\frac{x^2+y^2}{2\sigma^2}}
}
]

where:

[
\sigma
]

controls the spread.

---

# 12.29 Gaussian Blur Intuition

The center receives the largest weight.

Nearby pixels receive progressively smaller weights.

Conceptually:

```text id="m1u8v5"
small     medium     small
   \         |         /
      \      |      /
         CENTER
      /      |      \
   /         |         \
small       medium     small
```

So Gaussian blur is generally smoother and more natural than a simple box blur.

---

# 12.30 Example Gaussian Kernel

A common approximate (3\times3) Gaussian kernel is:

[
\boxed{
\frac{1}{16}
\begin{bmatrix}
1&2&1\
2&4&2\
1&2&1
\end{bmatrix}
}
]

Sum:

[
1+2+1+2+4+2+1+2+1
=16
]

Therefore:

[
\frac{1}{16}
]

normalizes the kernel.

---

# 12.31 Box Blur vs Gaussian Blur

### Box blur

```text id="4ih3e1"
1 1 1
1 1 1
1 1 1
```

All neighbors have equal importance.

### Gaussian

```text id="e4f6od"
1 2 1
2 4 2
1 2 1
```

Center and nearby pixels have greater importance.

Therefore:

```text id="8y6p1u"
Box
 ↓
uniform averaging

Gaussian
 ↓
distance-weighted averaging
```

---

# 12.32 Why Gaussian Blur Is Important

Gaussian filtering is widely used for:

* noise reduction
* smoothing
* preprocessing
* scale-space
* edge detection preparation
* image pyramids

It is also closely related to:

* Sobel edge detection
* Canny edge detection
* computer vision pipelines

---

# 12.33 Gaussian Blur and Medical Imaging

Gaussian filtering can help reduce high-frequency noise.

But:

```text id="s3l5pm"
Too much σ
   ↓
Too much smoothing
   ↓
Fine anatomical detail lost
```

Therefore (\sigma) must be chosen carefully.

In medical imaging, the physical scale of the blur matters.

If pixel spacing differs between images, the same pixel-based sigma does not represent the same physical blur.

---

# 12.34 Sharpening

Sharpening tries to emphasize high-frequency components and edges.

One simple sharpening kernel is:

[
\boxed{
\begin{bmatrix}
0&-1&0\
-1&5&-1\
0&-1&0
\end{bmatrix}
}
]

The center has a strong positive weight.

Surrounding pixels have negative weights.

---

# 12.35 How Sharpening Works

Conceptually:

```text id="e4w9gh"
Original Image
      │
      ├───────────────┐
      │               │
      │           Blur Image
      │               │
      └───────┬───────┘
              ↓
        High-frequency
           component
              ↓
           Add back
              ↓
          Sharpened
```

Another way to think about it:

[
\boxed{
Sharpened
=========

Original
+
k(Original-Blur)
}
]

This is the basis of **unsharp masking**.

---

# 12.36 Why Does Sharpening Emphasize Edges?

Edges contain rapid intensity changes.

A sharpening filter increases local differences.

Example:

```text id="3m4e2p"
Smooth:
100 100 100 100

Edge:
100 100 200 200
```

The transition becomes more pronounced after sharpening.

---

# 12.37 Sharpening Can Amplify Noise

Noise also contains high-frequency components.

Therefore:

```text id="w5n6ay"
Sharpening
     ↓
Edges ↑
+
Noise ↑
```

This is why a common pipeline is:

```text id="v9y5oe"
Noise reduction
      ↓
Sharpening
```

rather than aggressively sharpening a noisy image directly.

---

# 12.38 Convolution Pipeline

The complete process:

```text id="c7f1y7"
Input Image
     ↓
Choose Kernel
     ↓
Choose Padding
     ↓
Choose Stride
     ↓
Slide Kernel
     ↓
Multiply
     ↓
Sum
     ↓
Output Pixel
     ↓
Repeat
```

---

# 12.39 C++ Generic 2D Convolution

Conceptually:

```cpp id="x8o8w8"
double convolve(
    const Image& image,
    const Kernel& kernel,
    int x,
    int y)
{
    double sum = 0.0;

    for (int ky = 0;
         ky < kernel.height();
         ++ky)
    {
        for (int kx = 0;
             kx < kernel.width();
             ++kx)
        {
            double pixel =
                image.at(x + kx, y + ky);

            sum +=
                pixel * kernel.at(kx, ky);
        }
    }

    return sum;
}
```

A production implementation must add:

* border handling
* kernel centering
* convolution flipping if required
* stride
* datatype conversion
* clamping
* optimization

---

# 12.40 C++ Box Blur Kernel

```cpp id="c1u0xv"
const double kernel[3][3] =
{
    {1.0 / 9.0, 1.0 / 9.0, 1.0 / 9.0},
    {1.0 / 9.0, 1.0 / 9.0, 1.0 / 9.0},
    {1.0 / 9.0, 1.0 / 9.0, 1.0 / 9.0}
};
```

Or more cleanly:

```cpp id="7w6n6u"
const double kernel[3][3] =
{
    {1, 1, 1},
    {1, 1, 1},
    {1, 1, 1}
};
```

and divide the result by:

```cpp id="1g2i4k"
9.0
```

---

# 12.41 C++ Gaussian Kernel

```cpp id="l4qj73"
const double kernel[3][3] =
{
    {1.0 / 16.0, 2.0 / 16.0, 1.0 / 16.0},
    {2.0 / 16.0, 4.0 / 16.0, 2.0 / 16.0},
    {1.0 / 16.0, 2.0 / 16.0, 1.0 / 16.0}
};
```

The sum is:

[
1
]

so a uniform image remains approximately unchanged.

---

# 12.42 Kernel Design Mental Model

Instead of memorizing random matrices, ask:

### What do I want?

```text id="4c5zv4"
Blur?
   ↓
Positive smoothing weights

Edge?
   ↓
Positive + negative directional weights

Sharpen?
   ↓
Strong center + negative neighbors

Gaussian?
   ↓
Distance-weighted smoothing
```

This is much more useful than memorizing kernels.

---

# 12.43 Convolution and Edge Detection

A directional kernel can calculate intensity gradients.

For example:

[
G_x=
\begin{bmatrix}
-1&0&1\
-2&0&2\
-1&0&1
\end{bmatrix}
]

This is the Sobel (x)-direction kernel.

And:

[
G_y=
\begin{bmatrix}
-1&-2&-1\
0&0&0\
1&2&1
\end{bmatrix}
]

These estimate gradients.

We will study edge detection systematically later in the index.

---

# 12.44 Convolution and Deep Learning

Convolution is also the mathematical foundation of CNNs.

A CNN applies learned kernels:

```text id="8h0h3m"
Image
 ↓
Learned Kernel
 ↓
Feature Map
 ↓
More Kernels
 ↓
More Feature Maps
```

The difference is that in traditional image processing:

```text id="eq2n1d"
Human designs kernel
```

while in a CNN:

```text id="nq0f69"
Training learns kernel weights
```

The underlying local weighted-operation idea is closely related.

---

# 12.45 2D vs 3D Convolution

For medical imaging, 3D convolution is important.

A 2D kernel:

[
3\times3
]

works inside one image slice.

A 3D kernel:

[
3\times3\times3
]

works across:

```text id="r3w6h8"
Previous slice
Current slice
Next slice
```

This is relevant for:

* CT volumes
* MRI volumes
* PET volumes
* 3D segmentation
* volumetric deep learning

---

# 12.46 Physical Spacing Again

Suppose:

```text id="t4ez9b"
X = 0.5 mm
Y = 0.5 mm
Z = 5.0 mm
```

A (3\times3\times3) kernel covers approximately:

```text id="0x4j5m"
1.5 mm × 1.5 mm × 15 mm
```

not a cube.

Therefore blindly applying the same 3D kernel across axes can have very different physical effects.

This is a crucial medical-image-processing consideration.

---

# 12.47 Convolution Performance

For an image of:

[
N
]

pixels and (K\times K) kernel:

[
O(NK^2)
]

naively.

For large images and volumes, this can become expensive.

Optimization strategies include:

```text id="q3w6o4"
Separable kernels
     ↓
SIMD
     ↓
Multithreading
     ↓
GPU
     ↓
CUDA / OpenCL
```

For your medical software architecture, this is where CPU/GPU processing can eventually become important.

---

# 12.48 Convolution vs Neighborhood Filters

We can now connect Chapters 11 and 12:

```text id="8f1khy"
Neighborhood Processing
       │
       ├── Mean
       ├── Median
       ├── Min
       └── Max
       
Convolution
       │
       ├── Box blur
       ├── Gaussian
       ├── Sharpen
       └── Edge kernels
```

Important:

> **Median, min, and max are nonlinear filters and are not ordinary linear convolutions.**

Mean, Gaussian, and many sharpening/edge filters are linear convolution operations.

---

# 12.49 Linear vs Nonlinear

### Linear convolution

If:

[
g=K*f
]

then linearity means:

[
K*(af+bg)
=========

a(K*f)+b(K*g)
]

### Median

Does not satisfy this linearity property.

Therefore:

```text id="9q0xk6"
Mean / Gaussian
 ↓
Linear

Median / Min / Max
 ↓
Nonlinear
```

This distinction is very important in signal and image processing.

---

# 12.50 Chapter 12 Mental Model

Memorize this:

```text id="v0j8e6"
                    CONVOLUTION
                         │
                         ▼
                      KERNEL
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
          Weights      Size        Shape
             │
             ▼
        Sliding Window
             │
       ┌─────┴─────┐
       ↓           ↓
   Multiply       Sum
       │           │
       └─────┬─────┘
             ↓
         Output Pixel
             │
       ┌─────┼─────┐
       ↓     ↓     ↓
     Blur   Edge  Sharpen
```

---

# 12.51 Key Formulas

### 2D convolution

[
\boxed{
g(x,y)=
\sum_i\sum_j
K(i,j)f(x-i,y-j)
}
]

### Output size

[
\boxed{
W_{out}
=======

\left\lfloor
\frac{W+2P-K}{S}
\right\rfloor+1
}
]

[
\boxed{
H_{out}
=======

\left\lfloor
\frac{H+2P-K}{S}
\right\rfloor+1
}
]

### Gaussian

[
\boxed{
G(x,y)=
\frac{1}{2\pi\sigma^2}
e^{-\frac{x^2+y^2}{2\sigma^2}}
}
]

### Unsharp-style sharpening

[
\boxed{
I_{sharp}
=========

I+k(I-I_{blur})
}
]

---

# 12.52 Knowledge Check

### Fundamentals

1. What is convolution?
2. What is a kernel?
3. What is a kernel weight?
4. Why does kernel size matter?
5. Why are smoothing kernels often normalized?

### Convolution

6. Explain the steps of 2D convolution.
7. What is padding?
8. Why do we need padding?
9. What is stride?
10. What happens when stride increases?

### Correlation

11. What is the difference between correlation and convolution?
12. Why does kernel flipping matter?
13. Why are symmetric kernels special?

### Separable

14. What is a separable kernel?
15. Why is separable convolution faster?

### Filters

16. What is a box blur?
17. What is a Gaussian blur?
18. Why does Gaussian blur weight the center more heavily?
19. What is a sharpening kernel?
20. Why can sharpening amplify noise?

### Medical imaging

21. Why does pixel spacing matter when choosing a kernel?
22. What is the difference between 2D and 3D convolution?
23. Why can a (3\times3\times3) kernel be physically anisotropic?
24. Why might GPU acceleration be useful for 3D convolution?

---

# 12.53 Practical Exercise — Box Blur

Given:

```text id="j0r6jo"
10 20 30
20 30 40
30 40 50
```

Apply:

[
K=
\frac19
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
]

Calculate the center output.

---

# 12.54 Practical Exercise — Gaussian

Use:

[
K=
\frac1{16}
\begin{bmatrix}
1&2&1\
2&4&2\
1&2&1
\end{bmatrix}
]

on:

```text id="f4x1i0"
10 20 30
20 40 60
30 60 90
```

Calculate the center output.

---

# 12.55 Practical Exercise — Sharpening

Use:

[
K=
\begin{bmatrix}
0&-1&0\
-1&5&-1\
0&-1&0
\end{bmatrix}
]

on:

```text id="m4g7xk"
100 100 100
100 120 100
100 100 100
```

Calculate the center output.

Then explain why the center becomes stronger.

---

# 12.56 Medical Imaging Exercise

Suppose you have a CT image with:

```text id="a8y5uk"
Fine anatomical details
+
random noise
```

Design two pipelines.

### Pipeline A — Noise reduction

```text id="1j4k3c"
CT
 ↓
Gaussian
 ↓
Output
```

### Pipeline B — Edge-preserving approach

```text id="w2f7bg"
CT
 ↓
Median / later advanced filter
 ↓
Output
```

Explain:

* what information each filter preserves
* what information each may remove
* why kernel size matters
* why physical spacing matters

---

# 12.57 Enterprise Implementation Architecture

For your medical-image-processing library, a clean architecture can eventually look like:

```text id="4v9s4f"
Image
  │
  ▼
Neighborhood / Kernel
  │
  ▼
Convolution Engine
  │
  ├── BoxFilter
  ├── GaussianFilter
  ├── SharpenFilter
  ├── SobelFilter
  └── CustomKernelFilter
  │
  ▼
Output Image
```

Then optimization can be separated:

```text id="2m4h6v"
Convolution Engine
       │
       ├── CPU
       ├── SIMD
       ├── Multithreaded
       └── GPU
```

This is a much better enterprise architecture than embedding every filter directly inside the UI.

---

## Chapter 12 Complete

The most important concept is:

[
\boxed{
\text{Neighborhood}
+
\text{Kernel Weights}
\rightarrow
\text{Weighted Sum}
\rightarrow
\text{Output Pixel}
}
]

Remember these relationships:

```text id="9n3q5w"
Mean
 ↓
Box convolution

Gaussian
 ↓
Weighted smoothing

Sharpen
 ↓
High-frequency enhancement

Edge kernel
 ↓
Intensity gradient
```

And:

```text id="q1x3ab"
Neighborhood processing
        ↓
Convolution
        ↓
Image filtering
        ↓
Blur / Sharpen / Edge Detection
```

**Chapter 12 complete.**

### Next, strictly according to your index:

# Chapter 13 — Spatial Filtering

Topics:

* Smoothing filters
* Gaussian filter
* Median filter
* Bilateral filter
* Edge-preserving smoothing
* Sharpening
* High-pass filter
* Low-pass filter
* Unsharp masking
* Laplacian filter
* High-boost filtering
