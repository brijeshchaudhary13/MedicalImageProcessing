# Chapter 11 — Neighborhood Processing

We continue **strictly according to your index**.

Chapter 11 topics:

1. Neighborhood concept
2. 4-connected neighborhood
3. 8-connected neighborhood
4. Local operations
5. Mean filter
6. Median filter
7. Min / Max filter
8. Rank filters
9. Local statistics
10. Sliding window

---

# 11.1 What Is Neighborhood Processing?

In Chapters 7–10, we mainly worked with:

```text
Single pixel
      ↓
Transformation
      ↓
Output pixel
```

Neighborhood processing is different.

Now the output pixel depends on **nearby pixels**.

Mathematically:

[
g(x,y)=F\left(\mathcal{N}(x,y)\right)
]

where:

* (g(x,y)) = output pixel
* (\mathcal{N}(x,y)) = neighborhood around ((x,y))
* (F) = local operation

Conceptually:

```text
        Neighbor
           ↓
Neighbor → PIXEL ← Neighbor
           ↑
        Neighbor

          ↓

     Local Operation
          ↓
      Output Pixel
```

This is the foundation for:

* noise reduction
* smoothing
* edge detection
* sharpening
* local contrast
* morphology
* convolution

---

# 11.2 Why Do We Need Neighborhoods?

A single pixel does not contain information about its surrounding structure.

For example:

```text
Pixel = 100
```

doesn't tell us whether it is:

```text
80  90  100
90 100  110
100 110 120
```

or:

```text
100 100 100
100 100 100
100 100 100
```

The center pixel is identical, but its context is completely different.

Neighborhood processing uses that context.

---

# 11.3 The Neighborhood

For a center pixel:

```text
a b c
d e f
g h i
```

the center is:

[
e
]

and the surrounding pixels are:

```text
a b c
d   f
g h i
```

A common neighborhood is a:

[
3\times3
]

window.

---

# 11.4 4-Connected Neighborhood

The 4-connected neighborhood uses:

* up
* down
* left
* right

For center pixel (P):

```text
    N
    │
W ─ P ─ E
    │
    S
```

Coordinates:

[
(x-1,y)
]

[
(x+1,y)
]

[
(x,y-1)
]

[
(x,y+1)
]

plus the center itself when defining a neighborhood structure.

---

# 11.5 8-Connected Neighborhood

The 8-connected neighborhood also includes diagonals.

```text
NW   N   NE
 │   │   │
 W ─ P ─ E
 │   │   │
SW   S   SE
```

Therefore:

[
8
]

neighbor positions surround the center.

A complete (3\times3) window contains:

[
9
]

pixels including the center.

---

# 11.6 4-Connected vs 8-Connected

| Property       | 4-connected | 8-connected |
| -------------- | ----------- | ----------- |
| Up             | ✓           | ✓           |
| Down           | ✓           | ✓           |
| Left           | ✓           | ✓           |
| Right          | ✓           | ✓           |
| Diagonals      | ✗           | ✓           |
| Neighbor count | 4           | 8           |

Think:

```text
4-connected:

  X
X P X
  X


8-connected:

X X X
X P X
X X X
```

---

# 11.7 Why Connectivity Matters

Connectivity becomes important when dealing with:

* segmentation
* connected components
* region growing
* binary masks
* object extraction

For example:

```text
1 0
0 1
```

Under 4-connectivity:

```text
two separate regions
```

Under 8-connectivity:

```text
one connected diagonal region
```

So the connectivity definition can change the result of an algorithm.

---

# 11.8 Local Operations

A local operation calculates a pixel using pixels around it.

General form:

[
g(x,y)=F(
f(x+i,y+j)
)
]

for offsets (i,j) inside the selected neighborhood.

Examples:

```text
Neighborhood
     ↓
 ┌─────────┐
 │ 3 × 3   │
 └─────────┘
     ↓
   Function
     ↓
Center output
```

---

# 11.9 The Sliding Window

The most important implementation concept in this chapter is the **sliding window**.

Suppose the image is:

```text
1  2  3  4  5
6  7  8  9 10
11 12 13 14 15
16 17 18 19 20
21 22 23 24 25
```

A (3\times3) window starts around a pixel:

```text
1  2  3
6  7  8
11 12 13
```

Then moves:

```text
   ↓
2  3  4
7  8  9
12 13 14
```

Then:

```text
3  4  5
8  9 10
13 14 15
```

and so on.

This is why it is called a **sliding window**.

---

# 11.10 Sliding Window Mental Model

```text
Image
  ↓
┌─────────┐
│ Window  │
└─────────┘
     ↓
Compute
     ↓
Output center
     ↓
Move window
     ↓
Compute again
```

This pattern appears throughout image processing.

---

# 11.11 Mean Filter

The mean filter calculates the average of the neighborhood.

For a (3\times3) window:

[
\boxed{
g(x,y)=
\frac{1}{9}
\sum_{i=-1}^{1}
\sum_{j=-1}^{1}
f(x+i,y+j)
}
]

The kernel is:

[
\boxed{
\frac{1}{9}
\begin{bmatrix}
1&1&1\
1&1&1\
1&1&1
\end{bmatrix}
}
]

---

# 11.12 Mean Filter Example

Suppose:

```text
10 20 30
20 30 40
30 40 50
```

Mean:

[
\frac{
10+20+30+
20+30+40+
30+40+50
}{9}
]

[
=\frac{270}{9}
]

[
=30
]

So the center becomes:

```text
30
```

---

# 11.13 Why Mean Filtering?

Mean filtering smooths local variations.

It can reduce:

* random noise
* small intensity fluctuations

But it also tends to blur:

* edges
* fine details
* small structures

So:

```text
Mean Filter
     ↓
Noise ↓
     +
Sharpness ↓
```

---

# 11.14 Mean Filter Kernel

For (3\times3):

```text
1/9  1/9  1/9
1/9  1/9  1/9
1/9  1/9  1/9
```

Every pixel contributes equally.

This is a **linear filter**.

---

# 11.15 Mean Filter and Medical Images

Mean filtering can be useful for reducing certain random noise patterns.

But in medical imaging:

```text
Small structure
     ↓
Mean filter
     ↓
Potential loss of detail
```

This can be undesirable for:

* small vessels
* fine bone structures
* microcalcifications
* subtle lesions

Therefore filter selection matters.

---

# 11.16 Median Filter

The median filter is a nonlinear local filter.

For a (3\times3) neighborhood:

1. Collect the 9 values.
2. Sort them.
3. Select the middle value.

Example:

```text
10  20  30
20 255  40
30  40  50
```

Values:

```text
10,20,30,20,255,40,30,40,50
```

Sorted:

```text
10,20,20,30,30,40,40,50,255
```

Median:

[
30
]

So:

```text
255 → 30
```

---

# 11.17 Why Median Filter?

Median filtering is particularly effective against **salt-and-pepper noise**.

Example:

```text
Normal pixels
+
isolated 0 / 255 noise
```

Mean filtering can be strongly affected by the extreme value.

Median filtering is much more robust.

---

# 11.18 Mean vs Median

Suppose:

```text
10 10 10
10 255 10
10 10 10
```

Mean:

[
\frac{80+255}{9}
]

[
\approx38.33
]

Median:

```text
10,10,10,10,10,10,10,10,255
```

Median:

[
10
]

So:

```text
Mean
 ↓
38.3

Median
 ↓
10
```

The median rejects the isolated extreme value much better.

---

# 11.19 Median Filter Properties

Median filtering:

* is nonlinear
* preserves edges better than mean filtering in many cases
* removes isolated outliers
* is useful for impulse noise

But it can still remove small structures if the filter window is too large.

---

# 11.20 Min Filter

The minimum filter selects the smallest value:

[
\boxed{
g(x,y)=\min(\mathcal{N}(x,y))
}
]

Example:

```text
20 30 40
10 50 60
70 80 90
```

Minimum:

[
10
]

---

# 11.21 Max Filter

The maximum filter selects:

[
\boxed{
g(x,y)=\max(\mathcal{N}(x,y))
}
]

Example:

```text
20 30 40
10 50 60
70 80 90
```

Maximum:

[
90
]

---

# 11.22 Min / Max Intuition

### Min filter

Tends to emphasize dark regions.

### Max filter

Tends to emphasize bright regions.

Conceptually:

```text
Min
 ↓
dark structures expand


Max
 ↓
bright structures expand
```

These operations are closely related to mathematical morphology, which you will study later.

---

# 11.23 Rank Filters

A rank filter sorts neighborhood values and chooses a specific position.

Suppose the neighborhood contains:

```text
10 20 30
40 50 60
70 80 90
```

Sorted:

```text
10 20 30 40 50 60 70 80 90
```

Then:

```text
Rank 1 → 10
Rank 2 → 20
Rank 5 → 50
Rank 9 → 90
```

For a (3\times3) window:

[
\text{Median}=\text{Rank 5}
]

---

# 11.24 Rank Filter Generalization

Therefore:

```text
Rank 1
 ↓
Minimum

Middle rank
 ↓
Median

Last rank
 ↓
Maximum
```

So median, min, and max can be understood as members of the rank-filter family.

---

# 11.25 Why Rank Filters?

They provide flexible nonlinear neighborhood processing.

For example:

```text
Rank 2
```

may suppress certain low-valued outliers without behaving exactly like a minimum filter.

This gives you more control over local statistics.

---

# 11.26 Local Statistics

Instead of just calculating mean/median/min/max, we can calculate statistics over each neighborhood.

For example:

### Local mean

[
\mu(x,y)
]

### Local variance

[
\sigma^2(x,y)
]

### Local standard deviation

[
\sigma(x,y)
]

### Local minimum

[
min(x,y)
]

### Local maximum

[
max(x,y)
]

So the output is not one global statistic.

It is an **image of local statistics**.

---

# 11.27 Local Mean

For neighborhood (N):

[
\boxed{
\mu_N=
\frac{1}{|N|}
\sum_{p\in N}p
}
]

If we calculate this at every pixel:

```text
Image
 ↓
3×3 neighborhoods
 ↓
Mean for each neighborhood
 ↓
Mean image
```

---

# 11.28 Local Variance

The local variance is:

[
\boxed{
\sigma_N^2=
\frac{1}{|N|}
\sum_{p\in N}
(p-\mu_N)^2
}
]

This tells us how much intensity varies within the neighborhood.

Low variance:

```text
Uniform region
```

High variance:

```text
Edges / texture / detail
```

---

# 11.29 Local Standard Deviation

[
\boxed{
\sigma_N=\sqrt{\sigma_N^2}
}
]

A local standard deviation image can highlight areas containing more intensity variation.

Conceptually:

```text
Smooth region
     ↓
Low local variance


Edge / texture
     ↓
High local variance
```

This is useful for texture analysis and adaptive enhancement.

---

# 11.30 Local Statistics in Medical Imaging

Local statistics can help characterize:

* homogeneous tissue
* edges
* texture
* local contrast
* noise
* anatomical structures

For example:

```text
CT
 ↓
Local variance
 ↓
Texture / structural information
```

But again, statistical enhancement must not be confused with diagnostic truth.

---

# 11.31 Border Problem

What happens when the sliding window reaches the edge?

Suppose:

```text
1 2 3
4 5 6
7 8 9
```

At pixel `1`, a full (3\times3) neighborhood would require pixels outside the image.

We need a **border policy**.

Common choices:

1. Ignore borders
2. Zero padding
3. Replicate
4. Reflect
5. Circular/wrap
6. Constant padding

---

# 11.32 Zero Padding

Imagine adding zeros:

```text
0 0 0 0 0
0 1 2 3 0
0 4 5 6 0
0 7 8 9 0
0 0 0 0 0
```

Now every pixel has a full neighborhood.

But zero padding can introduce artificial dark values near the borders.

---

# 11.33 Replicate Padding

Repeat the nearest border:

```text
1 1 2 3 3
1 1 2 3 3
4 4 5 6 6
7 7 8 9 9
7 7 8 9 9
```

This avoids introducing zeros.

---

# 11.34 Reflect Padding

Reflect the image around the border.

Conceptually:

```text
... 3 2 | 1 2 3 | 2 1 ...
```

Reflection often provides smoother boundary behavior.

The exact reflection convention can differ between libraries, so production software must define it explicitly.

---

# 11.35 Why Border Policy Matters

Two implementations can use the same mean filter but produce different output near edges because of different border handling.

Therefore:

> **A neighborhood filter is not fully specified until its border policy is defined.**

This matters for reproducible medical image processing.

---

# 11.36 Sliding Window Algorithm

For a (3\times3) filter:

```text
for each pixel:
    collect 3×3 neighborhood
    calculate operation
    write result
```

Conceptually:

```text
for y:
    for x:
        neighborhood = image[y-1:y+2,
                             x-1:x+2]

        output[y][x] =
            operation(neighborhood)
```

---

# 11.37 C++ Mean Filter

A simple implementation:

```cpp
double mean3x3(
    const std::vector<std::vector<double>>& image,
    int x,
    int y)
{
    double sum = 0.0;

    for (int j = -1; j <= 1; ++j)
    {
        for (int i = -1; i <= 1; ++i)
        {
            sum += image[y + j][x + i];
        }
    }

    return sum / 9.0;
}
```

This assumes:

* valid interior pixel
* no border handling yet

A production implementation must explicitly handle boundaries.

---

# 11.38 C++ Median Filter Concept

```cpp
double median3x3(
    std::vector<double> values)
{
    std::sort(
        values.begin(),
        values.end());

    return values[values.size() / 2];
}
```

For a (3\times3) neighborhood:

```text
9 values
 ↓
sort
 ↓
index 4
 ↓
median
```

---

# 11.39 Mean Filter Complexity

For a (3\times3) window:

* 9 values per output pixel
* (N) pixels

Naively:

[
O(9N)
]

Since 9 is constant:

[
O(N)
]

For a (K\times K) window:

[
O(K^2N)
]

naively.

---

# 11.40 Why Optimization Matters

Medical images can be large:

```text
512 × 512
1024 × 1024
2048 × 2048
```

and 3D volumes can contain:

```text
hundreds or thousands of slices
```

So a naive nested neighborhood loop can become expensive.

Later you will learn:

* separable filters
* integral images
* optimized histogram algorithms
* SIMD
* multithreading
* GPU acceleration

---

# 11.41 Mean Filter Optimization

A (3\times3) mean filter can be separated:

[
\frac{1}{9}
\begin{bmatrix}
1\1\1
\end{bmatrix}
\begin{bmatrix}
1&1&1
\end{bmatrix}
]

Instead of 9 multiplications/additions per pixel, we can perform:

```text
Horizontal 1×3
      ↓
Vertical 3×1
```

This is called a **separable filter**.

You will study this more deeply in the convolution chapter.

---

# 11.42 Sliding Window and Running Statistics

For some filters, recomputing every neighborhood from scratch is wasteful.

Suppose:

```text
Window 1:
[1 2 3]

Window 2:
[2 3 4]
```

Only one value leaves and one enters.

We can maintain:

```text
running sum
```

instead of recalculating everything.

This idea is important for high-performance image processing.

---

# 11.43 Mean vs Median vs Min vs Max

| Filter | Operation    | Noise Handling                                 | Edge Preservation      |
| ------ | ------------ | ---------------------------------------------- | ---------------------- |
| Mean   | Average      | Good for random noise                          | Poorer                 |
| Median | Middle value | Excellent for impulse noise                    | Usually better         |
| Min    | Minimum      | Removes bright outliers / expands dark regions | Morphological behavior |
| Max    | Maximum      | Removes dark outliers / expands bright regions | Morphological behavior |

---

# 11.44 Example Comparison

Neighborhood:

```text
10 10 10
10 255 10
10 10 10
```

### Mean

[
37.22
]

approximately.

### Median

[
10
]

### Min

[
10
]

### Max

[
255
]

Therefore:

```text
Mean   → influenced by outlier
Median → rejects outlier
Min    → selects darkest
Max    → selects brightest
```

---

# 11.45 Neighborhood Processing vs Intensity Transformation

This distinction is extremely important.

### Intensity transformation

[
g(x,y)=T(f(x,y))
]

Only the current pixel matters.

### Neighborhood processing

[
g(x,y)=F(
f(x-1,y-1),\ldots,f(x+1,y+1)
)
]

Neighbors matter.

Therefore:

```text
Chapter 8
     ↓
Point operation

Chapter 11
     ↓
Local operation
```

---

# 11.46 Medical Image Example

Suppose an X-ray contains small random noise:

```text
Original
 ↓
3×3 median
 ↓
Reduced isolated noise
```

But if the filter is:

```text
11×11 median
```

then small anatomical structures may disappear.

Therefore:

[
\boxed{
\text{Filter size is an imaging decision}
}
]

not merely a programming parameter.

---

# 11.47 Local Contrast

Local statistics can also support local contrast enhancement.

For example:

```text
Image
 ↓
Local mean
 ↓
Local standard deviation
 ↓
Adaptive enhancement
```

A simplified concept is:

[
g(x,y)=
\frac{f(x,y)-\mu(x,y)}
{\sigma(x,y)+\epsilon}
]

where:

* (\mu(x,y)) = local mean
* (\sigma(x,y)) = local standard deviation
* (\epsilon) prevents division by zero

This is a conceptual foundation for adaptive contrast methods.

---

# 11.48 Neighborhood Size

Common sizes:

```text
3×3
5×5
7×7
9×9
```

Larger neighborhood:

```text
More context
+
More smoothing
+
More computation
```

Smaller neighborhood:

```text
Less context
+
More local detail
+
Less computation
```

---

# 11.49 Neighborhood Size in Medical Imaging

A filter size should be related to the physical scale of the feature you care about.

For example:

```text
Very small structure
       ↓
Large filter
       ↓
Potential disappearance
```

This is why medical imaging often requires knowledge of:

* pixel spacing
* slice thickness
* voxel spacing
* physical feature size

A (5\times5) kernel does not have the same physical size across two images if their pixel spacing differs.

---

# 11.50 2D vs 3D Neighborhoods

So far we've discussed 2D neighborhoods.

For volumetric medical images, you can have:

```text
3×3
```

in one slice, or:

```text
3×3×3
```

across multiple slices.

Conceptually:

```text
Slice Z-1
   ↓
Slice Z
   ↓
Slice Z+1
```

A 3D neighborhood uses information from neighboring slices.

This is very important for:

* CT
* MRI
* PET
* volumetric segmentation

---

# 11.51 Anisotropic Voxels

Suppose:

```text
Pixel spacing = 0.5 × 0.5 mm
Slice thickness = 5.0 mm
```

Then:

```text
X/Y resolution ≠ Z resolution
```

A (3\times3\times3) neighborhood is not physically isotropic.

Therefore, medical 3D processing must consider voxel spacing.

---

# 11.52 Enterprise Architecture

For a reusable image-processing library, you could separate:

```text
NeighborhoodProcessor
        │
        ├── MeanFilter
        ├── MedianFilter
        ├── MinFilter
        ├── MaxFilter
        └── RankFilter
```

and:

```text
Neighborhood
        │
        ├── 4-connected
        ├── 8-connected
        ├── Custom kernel
        └── 3D neighborhood
```

This keeps algorithms modular.

---

# 11.53 Processing Pipeline

```text
Image
  ↓
Select Neighborhood
  ↓
Choose Border Policy
  ↓
Sliding Window
  ↓
Local Operation
  ↓
Output Pixel
  ↓
Next Pixel
```

That is the basic architecture behind a large class of image filters.

---

# 11.54 Chapter 11 Mental Model

Memorize:

```text
                     IMAGE
                       │
                       ▼
                SLIDING WINDOW
                       │
            ┌──────────┼──────────┐
            ↓          ↓          ↓
          Mean       Median     Rank
            │          │          │
            │       ┌──┴──┐       │
            │       ↓     ↓       │
            │      Min   Max      │
            │                      │
            └──────────┬───────────┘
                       ↓
                Local Statistics
                       │
              ┌────────┴────────┐
              ↓                 ↓
          Local Mean       Local Variance
```

---

# 11.55 Key Formulas

### Local operation

[
\boxed{
g(x,y)=F(\mathcal N(x,y))
}
]

### Mean

[
\boxed{
g(x,y)=
\frac{1}{N}
\sum_{p\in\mathcal N}p
}
]

### Median

[
\boxed{
g(x,y)=median(\mathcal N)
}
]

### Minimum

[
\boxed{
g(x,y)=\min(\mathcal N)
}
]

### Maximum

[
\boxed{
g(x,y)=\max(\mathcal N)
}
]

### Local variance

[
\boxed{
\sigma^2=
\frac{1}{N}
\sum_{p\in N}(p-\mu)^2
}
]

### Local standard deviation

[
\boxed{
\sigma=\sqrt{\sigma^2}
}
]

---

# 11.56 Knowledge Check

### Neighborhood

1. What is neighborhood processing?
2. What is a 4-connected neighborhood?
3. What is an 8-connected neighborhood?
4. Why does connectivity matter in segmentation?

### Filters

5. What does a mean filter calculate?
6. Why does mean filtering blur edges?
7. What is a median filter?
8. Why is median filtering good for salt-and-pepper noise?
9. What does a min filter do?
10. What does a max filter do?

### Rank

11. What is a rank filter?
12. Which rank corresponds to the median in a (3\times3) window?
13. How are min and max related to rank filtering?

### Local statistics

14. What is local mean?
15. What is local variance?
16. What does high local variance generally indicate?
17. Why can local statistics be useful in medical images?

### Sliding window

18. What is a sliding window?
19. Why do border policies matter?
20. Name four border-handling strategies.

### Performance

21. What is the naive complexity of a (K\times K) filter?
22. What is a separable filter?
23. Why is optimization important for 3D medical volumes?

---

# 11.57 Practical Exercise

Given:

```text
10  10  10  10  10
10  20  30  40  10
10  30 255  50  10
10  40  50  60  10
10  10  10  10  10
```

Use the center (3\times3) neighborhood:

```text
20 30 40
30 255 50
40 50 60
```

Calculate:

### Exercise 1

Mean.

### Exercise 2

Median.

### Exercise 3

Minimum.

### Exercise 4

Maximum.

### Exercise 5

Rank 3.

### Exercise 6

Local variance.

---

# 11.58 Medical Imaging Exercise

Consider an X-ray containing:

```text
Fine anatomical structures
+
salt-and-pepper noise
```

Compare:

### Mean 3×3

### Median 3×3

### Mean 7×7

Explain:

* noise reduction
* edge preservation
* fine-detail preservation
* computational cost

Then decide which is most appropriate and **why**.

---

# 11.59 Very Important Medical Imaging Principle

Never choose a neighborhood size only because:

> "7×7 gives more smoothing."

Instead ask:

```text
What feature am I trying to preserve?
        ↓
What is its physical size?
        ↓
What is the pixel/voxel spacing?
        ↓
What noise am I trying to remove?
        ↓
What filter size is appropriate?
```

This is how image processing becomes **medical image processing** rather than generic image manipulation.

---

## Chapter 11 Complete

The core concept is:

[
\boxed{
\text{Output Pixel}
===================

F(\text{Neighborhood})
}
]

The most important mental progression is:

```text
Single Pixel
    ↓
Neighborhood
    ↓
Sliding Window
    ↓
Local Operation
    ↓
Mean / Median / Min / Max
    ↓
Local Statistics
```

And remember:

```text
4-connected → cross

8-connected → full 3×3 neighborhood
```

**Next, strictly according to your index:**

# Chapter 12 — Convolution

Topics:

* Convolution concept
* Kernel
* Kernel size
* Kernel weights
* 2D convolution
* Padding
* Stride
* Correlation vs convolution
* Separable convolution
* Convolution examples
* Gaussian blur
* Box blur
* Sharpening kernels
