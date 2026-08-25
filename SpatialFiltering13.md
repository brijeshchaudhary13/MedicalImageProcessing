# Chapter 13 — Spatial Filtering

We continue **strictly according to your index**.

Chapter 13 topics:

1. Smoothing filters
2. Gaussian filter
3. Median filter
4. Bilateral filter
5. Edge-preserving smoothing
6. Sharpening
7. High-pass filter
8. Low-pass filter
9. Unsharp masking
10. Laplacian filter
11. High-boost filtering

Chapter 12 gave us the mathematical foundation of convolution.
Now we use that foundation to understand **practical spatial filters**.

---

# 13.1 What Is Spatial Filtering?

Spatial filtering means modifying an image by operating directly on its spatial pixels.

General idea:

[
g(x,y)=F\left(f(x,y),\mathcal{N}(x,y)\right)
]

where:

* (f(x,y)) = input image
* (\mathcal N) = neighborhood
* (F) = filtering operation
* (g(x,y)) = output

Pipeline:

```text
Input Image
     ↓
Neighborhood
     ↓
Filter
     ↓
Output Image
```

---

# 13.2 Main Categories

Spatial filters can broadly be divided into:

```text
Spatial Filtering
       │
       ├── Smoothing
       │     ├── Box
       │     ├── Gaussian
       │     ├── Median
       │     └── Bilateral
       │
       └── Sharpening
             ├── High-pass
             ├── Laplacian
             ├── Unsharp masking
             └── High-boost
```

---

# 13.3 Smoothing Filters

Smoothing filters reduce rapid intensity variations.

Typical purposes:

* noise reduction
* preprocessing
* reducing high-frequency detail
* preparing for segmentation
* preparing for edge detection

Conceptually:

```text
Noise
 ↓
Smoothing
 ↓
Reduced high-frequency variation
```

But smoothing has a fundamental trade-off:

[
\boxed{
Noise\downarrow
\quad\text{but potentially}\quad
Detail\downarrow
}
]

---

# 13.4 Low-Pass Filtering

A low-pass filter allows relatively low spatial frequencies to remain while reducing high-frequency components.

Conceptually:

```text
Low frequency  → KEEP
High frequency → REDUCE
```

Since:

* smooth regions → low frequency
* edges/noise → high frequency

low-pass filtering tends to smooth the image.

Therefore:

[
\boxed{
\text{Low-pass} \approx \text{Smoothing}
}
]

---

# 13.5 High-Pass Filtering

A high-pass filter emphasizes high-frequency components.

Conceptually:

```text
Low frequency  → REDUCE
High frequency → KEEP
```

Edges contain strong high-frequency information.

Therefore:

[
\boxed{
\text{High-pass} \approx \text{Edge/detail emphasis}
}
]

---

# 13.6 Spatial Frequency

Don't confuse spatial frequency with image intensity.

Spatial frequency describes **how quickly intensity changes over space**.

### Smooth region

```text
100 101 100 101 100
```

Low spatial frequency.

### Edge

```text
0 0 0 255 255 255
```

High spatial frequency.

### Fine texture/noise

```text
100 150 90 170 80 ...
```

Also high spatial frequency.

Therefore:

```text
Spatial frequency
      │
      ├── Low → smooth structures
      │
      └── High → edges + fine detail + noise
```

---

# 13.7 Gaussian Filter

We already introduced Gaussian convolution in Chapter 12.

Now let's understand it as a practical spatial filter.

Gaussian:

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

controls the amount of smoothing.

---

# 13.8 Gaussian Filter Intuition

A Gaussian filter gives:

```text
Center       → highest weight
Near center  → medium weight
Farther      → smaller weight
```

Example:

[
\frac1{16}
\begin{bmatrix}
1&2&1\
2&4&2\
1&2&1
\end{bmatrix}
]

Visually:

```text
1 2 1
2 4 2
1 2 1
```

The center has the strongest contribution.

---

# 13.9 Effect of Sigma

Small (\sigma):

```text
Small smoothing
More detail retained
```

Large (\sigma):

```text
Strong smoothing
More detail removed
```

Conceptually:

```text
σ small
  ↓
light blur

σ large
  ↓
heavy blur
```

---

# 13.10 Gaussian vs Box Filter

| Property          | Box              | Gaussian                  |
| ----------------- | ---------------- | ------------------------- |
| Weights           | Equal            | Distance weighted         |
| Center importance | Same             | Highest                   |
| Smoothness        | Basic            | More natural              |
| Separable         | Yes              | Yes                       |
| Noise reduction   | Good             | Good                      |
| Common use        | Simple smoothing | General-purpose smoothing |

---

# 13.11 Median Filter

Median is a nonlinear spatial filter.

For each neighborhood:

```text
Collect values
      ↓
Sort
      ↓
Choose middle
```

Example:

```text
10 10 10
10 255 10
10 10 10
```

Sorted:

```text
10 10 10 10 10 10 10 10 255
```

Median:

[
10
]

So the isolated `255` is removed.

---

# 13.12 Why Median Is Different

Gaussian:

[
g=K*f
]

is a linear convolution.

Median:

[
g=median(\mathcal N)
]

is nonlinear.

Therefore median can remove isolated outliers while preserving edges better than simple averaging in many situations.

---

# 13.13 Bilateral Filter

The bilateral filter is one of the most important **edge-preserving smoothing** filters.

Instead of considering only spatial distance, it considers:

1. spatial distance
2. intensity similarity

So nearby pixels with very different intensities receive less influence.

---

# 13.14 Bilateral Filter Intuition

Suppose:

```text
100 100 100 | 250 250 250
             ↑
            edge
```

A normal Gaussian filter sees nearby pixels and may blur across the edge.

A bilateral filter asks:

```text
Is the neighbor close spatially?
AND
Is its intensity similar?
```

If the answer to the second question is no:

```text
Reduce its weight
```

Therefore the edge can be preserved better.

---

# 13.15 Bilateral Filter Formula

A simplified bilateral filter is:

[
\boxed{
I'(p)=
\frac{
\sum_{q\in N}
G_s(|p-q|)
G_r(|I(p)-I(q)|)
I(q)
}{
\sum_{q\in N}
G_s(|p-q|)
G_r(|I(p)-I(q)|)
}
}
]

There are two Gaussian terms.

### Spatial Gaussian

[
G_s
]

depends on distance.

### Range Gaussian

[
G_r
]

depends on intensity difference.

---

# 13.16 Spatial Weight

Two pixels that are far apart spatially receive less weight.

```text
Close → higher weight
Far   → lower weight
```

This is similar to Gaussian filtering.

---

# 13.17 Range Weight

Two pixels with very different intensities receive less weight.

Example:

```text
Pixel = 100
Neighbor = 105
```

Difference:

[
5
]

High similarity.

But:

```text
Pixel = 100
Neighbor = 250
```

Difference:

[
150
]

Low similarity.

Therefore the second neighbor contributes much less across a strong edge.

---

# 13.18 Bilateral Filter Mental Model

```text
              Neighbor
                  │
       ┌──────────┴──────────┐
       ↓                     ↓
Spatial distance       Intensity difference
       │                     │
       ↓                     ↓
Spatial weight          Range weight
       │                     │
       └──────────┬──────────┘
                  ↓
              Combined
                weight
                  ↓
             Weighted sum
```

---

# 13.19 Bilateral vs Gaussian

| Feature                   | Gaussian | Bilateral |
| ------------------------- | -------- | --------- |
| Uses spatial distance     | ✓        | ✓         |
| Uses intensity difference | ✗        | ✓         |
| Edge preservation         | Moderate | Better    |
| Noise reduction           | ✓        | ✓         |
| Complexity                | Lower    | Higher    |
| Nonlinear                 | No       | Yes       |

---

# 13.20 Edge-Preserving Smoothing

The goal is:

[
\boxed{
\text{Reduce noise}
+
\text{Preserve important edges}
}
]

This is difficult because:

```text
Noise
+
Edges
```

can both contain high-frequency information.

A simple Gaussian cannot inherently know:

> "This high-frequency variation is noise, but this one is an anatomical boundary."

Edge-preserving methods attempt to use additional information to distinguish them.

---

# 13.21 Why This Matters in Medical Imaging

Medical images contain many important boundaries:

```text
Bone / soft tissue
Organ / background
Vessel / tissue
Lesion / surrounding tissue
```

An aggressive smoothing filter can weaken these boundaries.

Therefore:

```text
Medical Image
     ↓
Noise reduction
     +
Structure preservation
```

is usually more desirable than maximum smoothing.

---

# 13.22 Sharpening

Sharpening emphasizes local intensity differences.

Conceptually:

```text
Blurred / smooth
      ↓
Enhance differences
      ↓
Sharper appearance
```

A simple sharpening kernel:

[
\boxed{
\begin{bmatrix}
0&-1&0\
-1&5&-1\
0&-1&0
\end{bmatrix}
}
]

---

# 13.23 Sharpening Kernel Explained

The center has:

[
5
]

while neighbors have:

[
-1
]

So:

[
Output =
5(center)
---------

neighbors
]

This emphasizes local differences.

For a constant region:

```text
100 100 100
100 100 100
100 100 100
```

output:

[
5(100)-4(100)=100
]

So a uniform region remains approximately unchanged.

At an edge, the result becomes different.

---

# 13.24 High-Pass Filter

A high-pass filter emphasizes rapid changes.

One simple high-pass kernel:

[
\boxed{
\begin{bmatrix}
-1&-1&-1\
-1&8&-1\
-1&-1&-1
\end{bmatrix}
}
]

Notice:

[
\sum K=0
]

This is important.

A constant region produces approximately zero.

---

# 13.25 Why Does the High-Pass Kernel Produce Zero on Flat Regions?

Suppose every pixel is:

[
100
]

Then:

[
8(100)-8(100)=0
]

Therefore:

```text
Uniform region
      ↓
High-pass
      ↓
Near zero
```

But at an edge:

```text
Large local difference
      ↓
Strong response
```

---

# 13.26 Low-Pass vs High-Pass

| Property       | Low-pass  | High-pass              |
| -------------- | --------- | ---------------------- |
| Smooth regions | Preserve  | Suppress               |
| Edges          | Reduce    | Emphasize              |
| Noise          | Reduce    | Often emphasize        |
| Typical use    | Smoothing | Edge/detail extraction |

---

# 13.27 Unsharp Masking

Despite the name, unsharp masking is actually a sharpening technique.

Pipeline:

```text
Original
   │
   ├──────────────┐
   │              ↓
   │           Blur
   │              │
   └───────┬──────┘
           ↓
      Original - Blur
           ↓
      Detail component
           ↓
     Add to Original
           ↓
        Sharper
```

Formula:

[
\boxed{
D=I-I_{blur}
}
]

Then:

[
\boxed{
I_{sharp}=I+kD
}
]

where (k) controls sharpening strength.

---

# 13.28 Why Is It Called "Unsharp Mask"?

The blurred image acts as the "unsharp" component.

We subtract it from the original:

[
I-I_{blur}
]

which extracts high-frequency information.

Then we add that high-frequency component back.

---

# 13.29 Example

Suppose:

[
I=100
]

and:

[
I_{blur}=90
]

Then:

[
D=100-90=10
]

If:

[
k=1
]

then:

[
I_{sharp}=100+10=110
]

The local detail is enhanced.

---

# 13.30 Sharpening Strength

If:

[
k=0
]

then:

[
I_{sharp}=I
]

No sharpening.

If:

[
k=0.5
]

moderate sharpening.

If:

[
k=2
]

strong sharpening.

But excessive (k) can cause:

```text
Halos
Ringing
Noise amplification
Artificial appearance
```

---

# 13.31 Laplacian Filter

The Laplacian is a second-order derivative operator.

Continuous form:

[
\boxed{
\nabla^2f=
\frac{\partial^2f}{\partial x^2}
+
\frac{\partial^2f}{\partial y^2}
}
]

It detects rapid intensity changes.

A common discrete kernel is:

[
\boxed{
\begin{bmatrix}
0&1&0\
1&-4&1\
0&1&0
\end{bmatrix}
}
]

Another convention reverses the sign:

[
\begin{bmatrix}
0&-1&0\
-1&4&-1\
0&-1&0
\end{bmatrix}
]

Both conventions exist.

---

# 13.32 Why Can Laplacian Be Used for Sharpening?

The Laplacian extracts high-frequency information.

Conceptually:

```text
Image
 ↓
Laplacian
 ↓
Detail / edge component
 ↓
Combine with original
 ↓
Sharpened image
```

The exact sign used in the combination depends on the kernel convention.

---

# 13.33 Laplacian vs First Derivative

A first derivative measures:

[
\frac{\partial f}{\partial x}
]

or:

[
\frac{\partial f}{\partial y}
]

The Laplacian combines second derivatives:

[
\frac{\partial^2 f}{\partial x^2}
+
\frac{\partial^2 f}{\partial y^2}
]

So:

```text
First derivative
 ↓
Gradient / direction

Second derivative
 ↓
Laplacian
```

---

# 13.34 Laplacian and Noise

Second derivatives are sensitive to high-frequency variations.

Noise is high frequency.

Therefore:

[
\boxed{
Laplacian
\rightarrow
Noise\ can\ be\ strongly\ amplified
}
]

A common strategy is:

```text
Image
 ↓
Gaussian smoothing
 ↓
Laplacian
```

This reduces some noise before derivative processing.

---

# 13.35 Laplacian of Gaussian

This leads to:

[
\boxed{
LoG = Laplacian\ of\ Gaussian
}
]

Pipeline:

```text
Image
 ↓
Gaussian
 ↓
Laplacian
 ↓
LoG response
```

This combines smoothing with second-derivative edge detection.

---

# 13.36 High-Boost Filtering

High-boost filtering generalizes unsharp masking.

First create a blurred image:

[
I_{blur}
]

Then:

[
\boxed{
I_{highboost}
=============

A I-I_{blur}
}
]

where:

[
A>1
]

is the amplification factor.

---

# 13.37 Relationship Between High-Boost and Unsharp Masking

Starting from:

[
I_{sharp}=I+k(I-I_{blur})
]

expand:

[
I_{sharp}
=========

(1+k)I-kI_{blur}
]

Define:

[
A=1+k
]

Then:

[
\boxed{
I_{sharp}
=========

AI-I_{blur}
}
]

So high-boost filtering is closely related to unsharp masking.

---

# 13.38 Example

Suppose:

[
I=100
]

[
I_{blur}=90
]

and:

[
A=1.5
]

Then:

[
I_{highboost}
=============

1.5(100)-90
]

[
=60
]

That looks surprising because we need to be careful about the exact formulation and desired normalization.

A more common high-boost formulation is:

[
\boxed{
I_{HB}=A I-(A-1)I_{blur}
}
]

Then:

[
1.5(100)-0.5(90)
]

[
=150-45
]

[
=105
]

which correctly adds some high-frequency detail.

This is an important implementation detail.

---

# 13.39 High-Boost Correct Form

Using:

[
A>1
]

the common form is:

[
\boxed{
I_{HB}
======

AI-(A-1)I_{blur}
}
]

Rearrange:

[
I_{HB}
======

I+(A-1)(I-I_{blur})
]

So:

[
k=A-1
]

and therefore it is directly related to unsharp masking.

---

# 13.40 High-Boost Behavior

### (A=1)

[
I_{HB}=I
]

No sharpening.

### (A>1)

Sharpening increases.

### Very large (A)

Potentially:

```text
Noise ↑
Halos ↑
Artifacts ↑
```

---

# 13.41 Filter Frequency View

A useful mental model:

```text
                Spatial Filters
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
       Low-pass               High-pass
          │                       │
          ↓                       ↓
       Smooth                  Detail
          │                       │
     ┌────┼────┐             ┌────┼────┐
     ↓    ↓    ↓             ↓    ↓    ↓
    Box Gaussian Median     Laplacian Sharpen
                            High-boost
```

Bilateral is special because it is **edge-preserving and nonlinear**, rather than simply being a standard linear low-pass convolution.

---

# 13.42 Filter Comparison

| Filter     | Type       | Main Purpose                | Edge Preservation   |
| ---------- | ---------- | --------------------------- | ------------------- |
| Box        | Linear     | Smoothing                   | Low                 |
| Gaussian   | Linear     | Smoothing                   | Moderate            |
| Median     | Nonlinear  | Impulse-noise reduction     | Good                |
| Bilateral  | Nonlinear  | Edge-preserving smoothing   | High                |
| High-pass  | Linear     | Detail/edge extraction      | N/A                 |
| Laplacian  | Derivative | Edge/detail                 | N/A                 |
| Unsharp    | Sharpening | Detail enhancement          | Depends on strength |
| High-boost | Sharpening | Stronger detail enhancement | Depends on strength |

---

# 13.43 Medical Imaging Filter Selection

Suppose your image contains:

```text
Noise
+
small anatomical structures
+
important edges
```

A reasonable decision process is:

```text
What noise?
     │
     ├── Impulse noise
     │       ↓
     │     Median
     │
     ├── Gaussian-like noise
     │       ↓
     │     Gaussian
     │
     └── Need edge preservation
             ↓
          Bilateral /
          advanced edge-preserving filter
```

Then if sharpening is required:

```text
Smoothed image
      ↓
Controlled sharpening
```

---

# 13.44 Medical Imaging Warning

Enhancement is not the same as information creation.

For example:

```text
Sharpening
    ↓
Existing edge becomes stronger
```

but aggressive sharpening can produce:

```text
Halo
Overshoot
Noise
Artificial edges
```

Therefore an enterprise medical viewer should make processing:

* explicit
* reproducible
* parameterized
* reversible

---

# 13.45 Display Processing vs Quantitative Data

This is especially important for your DICOM viewer.

Use:

```text
Original pixel data
       │
       ├──────────────→ Preserve
       │
       ↓
Display pipeline
       ↓
Optional spatial filtering
       ↓
Rendered image
```

Do not silently replace the original quantitative image with a sharpened/filtered representation.

---

# 13.46 Filter Pipeline Example

For an X-ray viewer:

```text
Raw / DICOM
     ↓
Decode
     ↓
Normalize / physical conversion
     ↓
Window / display range
     ↓
Optional Gaussian / bilateral
     ↓
Optional sharpening
     ↓
LUT
     ↓
Display
```

The exact ordering depends on the imaging modality and intended operation.

---

# 13.47 Why Filter Ordering Matters

Consider:

```text
Gaussian
 ↓
Sharpen
```

versus:

```text
Sharpen
 ↓
Gaussian
```

They are not equivalent.

First:

```text
Noise reduction
 ↓
Sharpen remaining structures
```

often gives a different result from:

```text
Sharpen noise
 ↓
Blur everything
```

Therefore:

[
\boxed{
A\rightarrow B
\neq
B\rightarrow A
}
]

in general for nonlinear or practical image-processing pipelines.

---

# 13.48 C++ Filter Architecture

For an enterprise implementation:

```text
ISpatialFilter
      │
 ┌────┼──────────────┐
 ↓    ↓              ↓
Gaussian Median   Bilateral
 │
 ├── parameters
 ├── border policy
 ├── datatype
 └── execution backend
```

For example:

```cpp
class ISpatialFilter
{
public:
    virtual ~ISpatialFilter() = default;

    virtual void apply(
        const Image& input,
        Image& output) = 0;
};
```

Then:

```cpp
class GaussianFilter : public ISpatialFilter
{
public:
    void apply(
        const Image& input,
        Image& output) override;
};
```

This keeps UI code separate from image-processing algorithms.

---

# 13.49 Parameterization

A Gaussian filter should not have hidden values.

For example:

```cpp
struct GaussianParameters
{
    double sigmaX = 1.0;
    double sigmaY = 1.0;

    int kernelWidth = 5;
    int kernelHeight = 5;
};
```

Bilateral:

```cpp
struct BilateralParameters
{
    double sigmaSpatial;
    double sigmaRange;
    int radius;
};
```

Sharpening:

```cpp
struct SharpenParameters
{
    double amount;
};
```

This makes processing reproducible.

---

# 13.50 GPU Consideration

Spatial filtering is highly parallel.

For example:

```text
Pixel (100,100)
Pixel (101,100)
Pixel (102,100)
...
```

can often be processed independently once boundary/input access is handled.

Therefore:

```text
CPU
 ↓
Multithreading

or

GPU
 ↓
Thousands of parallel threads
```

can accelerate large images and volumes.

This will become important when we discuss enterprise medical-imaging architecture.

---

# 13.51 2D vs 3D Filtering

For a CT volume:

```text
Slice Z-1
Slice Z
Slice Z+1
```

a 3D filter can use all three.

But if slice thickness is much larger than in-plane spacing:

```text
0.5 × 0.5 × 5.0 mm
```

then an isotropic 3D filter may be inappropriate.

Possible approach:

```text
2D filtering
```

or:

```text
anisotropic 3D filtering
```

depending on the task.

---

# 13.52 Important Difference: Median vs Bilateral

### Median

Uses rank ordering.

```text
Collect
 ↓
Sort
 ↓
Middle
```

### Bilateral

Uses weighted averaging.

```text
Spatial similarity
+
Intensity similarity
 ↓
Weighted average
```

So:

```text
Median
→ nonlinear rank filter

Bilateral
→ nonlinear weighted filter
```

---

# 13.53 Chapter 13 Mental Model

Memorize:

```text
                  SPATIAL FILTERING
                         │
          ┌──────────────┴──────────────┐
          ↓                             ↓
       SMOOTHING                     SHARPENING
          │                             │
    ┌─────┼─────┐                ┌─────┼─────┐
    ↓     ↓     ↓                ↓     ↓     ↓
  Box  Gaussian Median       High-pass Laplacian
    │     │      │                │       │
    └─────┴──┐   │                └──┬────┘
             ↓   ↓                   ↓
          Bilateral              Unsharp
             │                     Masking
             ↓                       │
       Edge-preserving              ↓
                                  High-boost
```

---

# 13.54 Key Formulas

### Gaussian

[
\boxed{
G(x,y)=
\frac{1}{2\pi\sigma^2}
e^{-\frac{x^2+y^2}{2\sigma^2}}
}
]

### Bilateral

[
\boxed{
I'(p)=
\frac{
\sum_q
G_s(|p-q|)
G_r(|I(p)-I(q)|)
I(q)
}{
\sum_q
G_s(|p-q|)
G_r(|I(p)-I(q)|)
}
}
]

### High-pass example

[
\boxed{
\begin{bmatrix}
-1&-1&-1\
-1&8&-1\
-1&-1&-1
\end{bmatrix}
}
]

### Laplacian

[
\boxed{
\nabla^2f=
f_{xx}+f_{yy}
}
]

### Unsharp masking

[
\boxed{
I_{sharp}=I+k(I-I_{blur})
}
]

### High-boost

[
\boxed{
I_{HB}=AI-(A-1)I_{blur}
}
]

where:

[
A>1
]

---

# 13.55 Knowledge Check

### Smoothing

1. What is spatial smoothing?
2. Why does smoothing reduce noise?
3. Why can smoothing remove details?
4. What is a low-pass filter?

### Gaussian

5. What does (\sigma) control?
6. Why does Gaussian give higher weight to nearby pixels?
7. Why is Gaussian usually preferred over a simple box filter?

### Median

8. Why is median nonlinear?
9. What type of noise is median particularly useful for?
10. Why can a large median kernel remove small structures?

### Bilateral

11. What are the two types of similarity used by bilateral filtering?
12. Why can bilateral preserve edges better than Gaussian?
13. Why is bilateral computationally more expensive?

### Sharpening

14. What is a high-pass filter?
15. What is a Laplacian filter?
16. Why can Laplacian processing amplify noise?
17. What is unsharp masking?
18. What is high-boost filtering?

### Medical Imaging

19. Why must filtering parameters be reproducible?
20. Why does voxel spacing matter for 3D filtering?
21. Why should enhancement not overwrite original quantitative data?
22. Why can aggressive sharpening be dangerous in medical images?

---

# 13.56 Practical Exercise

Given:

```text
100 100 100
100 200 100
100 100 100
```

Apply:

### A. Gaussian

[
\frac1{16}
\begin{bmatrix}
1&2&1\
2&4&2\
1&2&1
\end{bmatrix}
]

### B. High-pass

[
\begin{bmatrix}
-1&-1&-1\
-1&8&-1\
-1&-1&-1
\end{bmatrix}
]

### C. Sharpen

[
\begin{bmatrix}
0&-1&0\
-1&5&-1\
0&-1&0
\end{bmatrix}
]

Compare the three outputs and explain what each filter is trying to accomplish.

---

# 13.57 Medical Imaging Exercise

Suppose a CT image has:

```text
Moderate noise
+
sharp bone boundaries
+
small anatomical structures
```

Choose between:

* Gaussian
* Median
* Bilateral
* Unsharp masking
* High-boost

For each, explain:

1. What it improves.
2. What it can damage.
3. Whether it is linear/nonlinear.
4. Whether it is mainly smoothing or sharpening.
5. Whether you would use it directly on quantitative CT data or only as a display-processing step.

---

# 13.58 Chapter 13 Complete

The central idea is:

[
\boxed{
Spatial\ Filtering
==================

Control\ of\ local\ spatial\ frequencies
}
]

Remember the main map:

```text
LOW-PASS
   ↓
Smooth
   ├── Box
   └── Gaussian

NONLINEAR / EDGE-PRESERVING
   ↓
   ├── Median
   └── Bilateral

HIGH-PASS
   ↓
Edges / details
   ├── High-pass
   └── Laplacian

SHARPENING
   ↓
   ├── Unsharp masking
   └── High-boost
```

And the most important medical-imaging principle:

[
\boxed{
\text{Noise reduction must be balanced against anatomical-detail preservation.}
}
]

**Chapter 13 complete.**

### Next, strictly according to your index:

# Chapter 14 — Frequency Domain

Topics:

* Spatial vs frequency domain
* Fourier transform
* DFT
* FFT
* Frequency spectrum
* Magnitude
* Phase
* Frequency shifting
* Low-pass filtering
* High-pass filtering
* Frequency-domain filtering
* Gaussian frequency filter
* Ideal filter
* Butterworth filter
* Ringing artifacts
