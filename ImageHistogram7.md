# Chapter 7 — Image Histogram

We now move to **Chapter 7 exactly according to your index**.

Topics:

1. Histogram definition
2. Histogram computation
3. Histogram interpretation
4. Histogram equalization
5. Histogram matching
6. CDF
7. PDF
8. Histogram stretching
9. Adaptive histogram equalization
10. CLAHE



---

# 7.1 What Is an Image Histogram?

An **image histogram** describes how frequently different intensity values occur in an image.

For a grayscale image:

[
H(k)=\text{number of pixels having intensity }k
]

For an 8-bit image:

[
k\in[0,255]
]

So the histogram contains up to 256 bins.

Conceptually:

```text
Number of pixels
       ↑
       │       ███
       │    ███████
       │  ██████████
       │ ███████████
       └──────────────────→
          Intensity
        0             255
```

---

# 7.2 Simple Example

Consider:

```text
10 10 10
20 20 30
30 40 40
```

Count each intensity:

```text
10 → 3 pixels
20 → 2 pixels
30 → 2 pixels
40 → 2 pixels
```

Therefore:

[
H(10)=3
]

[
H(20)=2
]

[
H(30)=2
]

[
H(40)=2
]

All other bins have:

[
H(k)=0
]

---

# 7.3 Histogram Axes

A standard histogram has two axes.

### X-axis

Intensity value:

```text
0 ─────────────────── 255
```

### Y-axis

Number of pixels:

```text
frequency
```

Therefore:

```text
       Frequency
          ↑
          │
          │ ███
          │ █████
          │███████
          └────────────────→
             Intensity
```

---

# 7.4 Histogram Computation

To compute a histogram, we can initialize bins and count each pixel.

For an 8-bit image:

```text
histogram[0]
histogram[1]
histogram[2]
...
histogram[255]
```

Initially:

```text
histogram[k] = 0
```

Then for every pixel:

```text
histogram[pixel_value]++;
```

---

# 7.5 C++ Histogram Example

```cpp
#include <array>
#include <cstdint>
#include <iostream>
#include <vector>

int main()
{
    std::vector<std::uint8_t> image =
    {
        10, 10, 10,
        20, 20, 30,
        30, 40, 40
    };

    std::array<std::size_t, 256> histogram{};

    for (std::uint8_t pixel : image)
    {
        ++histogram[pixel];
    }

    for (int i = 0; i < 256; ++i)
    {
        if (histogram[i] != 0)
        {
            std::cout
                << i << " -> "
                << histogram[i] << '\n';
        }
    }

    return 0;
}
```

Output conceptually:

```text
10 -> 3
20 -> 2
30 -> 2
40 -> 2
```

---

# 7.6 Python Histogram Example

Using NumPy:

```python
import numpy as np

image = np.array([
    [10, 10, 10],
    [20, 20, 30],
    [30, 40, 40]
], dtype=np.uint8)

histogram = np.bincount(
    image.ravel(),
    minlength=256
)

print(histogram[10])
print(histogram[20])
print(histogram[30])
print(histogram[40])
```

The important operation is:

```text
image
 ↓
flatten / ravel
 ↓
count intensity values
 ↓
histogram
```

---

# 7.7 Why Do We Need Histograms?

A histogram helps us understand the **distribution of intensities**.

It can reveal whether an image is:

* mostly dark
* mostly bright
* low contrast
* high contrast
* concentrated around certain values
* composed of multiple intensity populations

But remember:

> A histogram tells us about intensity distribution, not spatial arrangement.

---

# 7.8 Dark Image Histogram

Suppose an image is mostly dark.

Its histogram will be concentrated toward the lower intensity values:

```text
Frequency
   ↑
   │████
   │█████
   │██████
   │███████
   │██
   └────────────────────→
    0                  255
```

Most pixels are near:

[
0
]

rather than near 255.

---

# 7.9 Bright Image Histogram

For a mostly bright image:

```text
Frequency
   ↑
   │                    ████
   │                  ██████
   │                ███████
   │
   └────────────────────────→
    0                     255
```

The histogram is concentrated toward higher intensity values.

---

# 7.10 Low-Contrast Image

A low-contrast image may use only a small part of the available range.

For example:

[
80\ldots120
]

instead of:

[
0\ldots255
]

Histogram:

```text
0                 80    120             255
|------------------████████----------------|
```

The available intensity range is poorly utilized.

---

# 7.11 High-Contrast Image

A high-contrast image may use a much wider range:

```text
0       50       100       150       200      255
██████████████████████████████████████████████
```

More of the available intensity range is being used.

---

# 7.12 Histogram Interpretation

A histogram can give us useful information.

### Concentrated near 0

```text
Mostly dark
```

### Concentrated near 255

```text
Mostly bright
```

### Narrow distribution

```text
Potentially low contrast
```

### Broad distribution

```text
Potentially higher contrast
```

### Multiple peaks

```text
Potentially multiple intensity populations
```

But these are interpretations, not absolute rules.

---

# 7.13 Histogram Peaks

A **peak** represents an intensity region containing many pixels.

For example:

```text
Frequency
   ↑
   │      ███
   │      ███
   │ ███  ███       ███
   │ ███  ███       ███
   └──────────────────────→
       50   120       200
```

There are several dominant intensity regions.

In medical images, these may sometimes correspond to different tissue/material populations, although real CT/MRI histograms can be much more complicated.

---

# 7.14 Histogram Is Not a Spatial Map

Consider Image A:

```text
100 100
200 200
```

Image B:

```text
100 200
100 200
```

Both have:

```text
100 → 2
200 → 2
```

So their histograms are identical.

But their spatial structures are different.

Therefore:

[
\boxed{\text{Histogram does not contain spatial location information}}
]

---

# 7.15 Probability Density Function — PDF

A normalized histogram can be interpreted as a probability distribution.

For discrete image intensities:

[
p(k)=\frac{H(k)}{N}
]

where:

* (H(k)) = number of pixels with intensity (k)
* (N) = total number of pixels

This is the **probability mass function** for discrete intensity values.

It is often loosely referred to as the intensity PDF in image-processing discussions.

---

# 7.16 PDF Example

Suppose the image has 10 pixels:

```text
10 → 3 pixels
20 → 4 pixels
30 → 3 pixels
```

Then:

[
p(10)=\frac{3}{10}=0.3
]

[
p(20)=\frac{4}{10}=0.4
]

[
p(30)=\frac{3}{10}=0.3
]

And:

[
0.3+0.4+0.3=1
]

So the normalized histogram describes the probability distribution of intensities.

---

# 7.17 Histogram vs PDF

| Histogram              | Normalized Distribution              |
| ---------------------- | ------------------------------------ |
| Counts pixels          | Represents proportions/probabilities |
| Absolute frequency     | Relative frequency                   |
| (H(k))                 | (p(k))                               |
| Sum = number of pixels | Sum = 1                              |

Relationship:

[
p(k)=\frac{H(k)}{N}
]

---

# 7.18 Cumulative Distribution Function — CDF

The **CDF** accumulates probabilities from lower intensities up to a particular intensity.

Mathematically:

[
CDF(k)=\sum_{j=0}^{k}p(j)
]

So:

```text
PDF
 ↓
Cumulative sum
 ↓
CDF
```

---

# 7.19 CDF Example

Suppose:

```text
Intensity    PDF

10           0.2
20           0.3
30           0.1
40           0.4
```

Then:

### At 10

[
CDF(10)=0.2
]

### At 20

[
CDF(20)=0.2+0.3=0.5
]

### At 30

[
CDF(30)=0.5+0.1=0.6
]

### At 40

[
CDF(40)=0.6+0.4=1.0
]

So:

```text
Intensity    CDF

10           0.2
20           0.5
30           0.6
40           1.0
```

The CDF always increases or stays constant and eventually reaches 1.

---

# 7.20 Why CDF Matters

CDF is fundamental to:

* histogram equalization
* histogram matching
* probability-based intensity transformations

The key relationship is:

```text
Histogram
   ↓
Normalize
   ↓
PDF
   ↓
Cumulative sum
   ↓
CDF
```

---

# 7.21 Histogram Equalization

Histogram equalization attempts to redistribute intensity values so that the available intensity range is used more effectively.

The basic idea is:

```text
Original image
      ↓
Histogram
      ↓
PDF
      ↓
CDF
      ↓
Intensity mapping
      ↓
New image
```

It can improve contrast, particularly when intensities are concentrated in a limited range.

---

# 7.22 Histogram Equalization Formula

For an image with (L) possible intensity levels, a common discrete mapping is:

[
s_k=(L-1),CDF(k)
]

where:

* (k) = original intensity
* (CDF(k)) = cumulative distribution
* (L) = number of intensity levels
* (s_k) = transformed intensity

For an 8-bit image:

[
L=256
]

so:

[
s_k=255,CDF(k)
]

with practical implementation often involving rounding.

---

# 7.23 Simple Equalization Example

Suppose:

```text
Intensity    CDF

10           0.2
20           0.5
30           0.6
40           1.0
```

For an 8-bit output:

[
s=255\times CDF
]

Then approximately:

```text
10 → 51
20 → 128
30 → 153
40 → 255
```

The original values:

```text
10,20,30,40
```

are spread across:

```text
51,128,153,255
```

---

# 7.24 Why Equalization Improves Contrast

Suppose the original image uses:

```text
50 ───────── 100
```

mostly.

Histogram equalization can redistribute the values over a wider range:

```text
20 ───────────────────────── 240
```

Conceptually:

```text
Narrow intensity distribution
          ↓
Histogram equalization
          ↓
Broader intensity distribution
          ↓
Potentially higher contrast
```

---

# 7.25 Important Limitation of Global Equalization

Histogram equalization operates on the **whole image**.

Suppose an image contains:

```text
Bright region
+
Dark region
```

A global histogram may be dominated by one region.

Then equalization may:

* over-enhance some areas
* under-enhance other areas
* amplify noise
* produce unnatural appearance

This motivates **adaptive histogram equalization**.

---

# 7.26 Histogram Stretching

Histogram stretching is different from histogram equalization.

Suppose the actual image range is:

[
I_{min}=50
]

[
I_{max}=150
]

We want:

[
0\ldots255
]

The transformation is:

[
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
\times255
]

So:

[
I'=255
\frac{I-50}{100}
]

---

# 7.27 Stretching Example

For:

[
I=50
]

[
I'=0
]

For:

[
I=100
]

[
I'=127.5
]

For:

[
I=150
]

[
I'=255
]

Therefore:

```text
50  → 0
100 → 128
150 → 255
```

---

# 7.28 Stretching vs Equalization

### Histogram stretching

Maps the existing minimum/maximum range to a desired range.

```text
[50,150]
    ↓
[0,255]
```

### Histogram equalization

Uses the **CDF** to construct a nonlinear intensity mapping.

```text
Histogram
   ↓
CDF
   ↓
Nonlinear mapping
```

Therefore:

> Stretching is generally a simpler range expansion; equalization uses the distribution of intensities.

---

# 7.29 Histogram Matching

Histogram matching is also called **histogram specification**.

The goal is:

> Transform an image so that its intensity distribution approximately follows a target distribution.

Conceptually:

```text
Source Image
     ↓
Source Histogram
     ↓
Transformation
     ↓
Output Image
     ↓
Histogram similar to Target
```

---

# 7.30 Histogram Matching Example

Suppose:

```text
Source image
Histogram A
```

and:

```text
Reference image
Histogram B
```

We want:

```text
Histogram A
    ↓
Mapping
    ↓
Histogram approximately B
```

This is useful when images need intensity normalization relative to a reference.

---

# 7.31 Histogram Matching Using CDF

The basic concept is:

```text
Source PDF
    ↓
Source CDF

Target PDF
    ↓
Target CDF

Source CDF
    ↓
Map to corresponding target intensity
```

Conceptually:

[
CDF_{source}(r)
\approx
CDF_{target}(z)
]

Then the source intensity (r) is mapped to a target intensity (z) with a corresponding cumulative probability.

---

# 7.32 Global vs Local Histogram Processing

### Global

Uses the histogram of the entire image:

```text
Entire Image
     ↓
One Histogram
     ↓
One Transformation
```

### Local

Processes smaller regions:

```text
Image
 ├── Region 1 → histogram
 ├── Region 2 → histogram
 ├── Region 3 → histogram
 └── ...
```

This can enhance local details more effectively.

---

# 7.33 Adaptive Histogram Equalization

**Adaptive Histogram Equalization (AHE)** performs histogram equalization locally rather than using one global histogram.

Conceptually:

```text
Image
 ↓
Divide into local regions
 ↓
Histogram for each region
 ↓
Equalize each region
 ↓
Combine result
```

This can improve local contrast.

---

# 7.34 Why AHE Helps

Consider:

```text
Large dark image
+
small bright structure
```

A global histogram may not adequately enhance the small structure.

AHE examines local neighborhoods:

```text
Local region
     ↓
Local histogram
     ↓
Local contrast enhancement
```

So details that are hidden globally may become more visible locally.

---

# 7.35 Problem With AHE

AHE can strongly amplify noise.

For example:

```text
Original
   ↓
Small local intensity variations
   ↓
AHE
   ↓
Variations amplified
   ↓
Noise becomes visible
```

This is especially problematic in medical imaging because noise can be mistaken for anatomical structures.

Therefore a modified technique is commonly used:

> **CLAHE**

---

# 7.36 CLAHE

CLAHE means:

> **Contrast Limited Adaptive Histogram Equalization**

It combines:

* local histogram equalization
* contrast limiting

The key idea is:

```text
AHE
 ↓
Local enhancement
 +
Contrast limit
 ↓
CLAHE
```

---

# 7.37 Why Contrast Limiting?

AHE can produce very large histogram peaks.

CLAHE limits the contribution of those peaks.

Conceptually:

```text
AHE:

Very high histogram peak
       ↓
Strong amplification
       ↓
Noise amplification


CLAHE:

Histogram peak
       ↓
Clip/limit
       ↓
Redistribute excess
       ↓
More controlled enhancement
```

---

# 7.38 CLAHE Pipeline

```text
Input Image
     ↓
Divide into tiles
     ↓
Compute local histogram
     ↓
Clip histogram
     ↓
Redistribute excess
     ↓
Compute local mapping
     ↓
Interpolate between regions
     ↓
Enhanced image
```

The interpolation step helps reduce visible boundaries between neighboring tiles.

---

# 7.39 Why Tiles Are Used

Instead of applying one histogram to the entire image, CLAHE divides the image into small regions or **tiles**.

Example:

```text
+---------+---------+---------+
| Tile 1  | Tile 2  | Tile 3  |
+---------+---------+---------+
| Tile 4  | Tile 5  | Tile 6  |
+---------+---------+---------+
| Tile 7  | Tile 8  | Tile 9  |
+---------+---------+---------+
```

Each tile gets its own local histogram.

---

# 7.40 Medical Imaging Use

CLAHE is often useful for enhancing local contrast in images such as:

* X-ray
* mammography
* MRI
* CT-derived images
* microscopy

But enhancement must be used carefully.

In medical imaging:

> An enhancement technique should improve visualization without creating misleading structures.

---

# 7.41 Histogram Processing Hierarchy

Keep this structure:

```text
                 HISTOGRAM
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
       Global                  Local
          │                     │
    ┌─────┴─────┐          ┌────┴─────┐
    ↓           ↓          ↓          ↓
Stretching  Equalization   AHE       CLAHE
                │                     │
                ↓                     ↓
               CDF              Contrast limit
```

Histogram matching is another global distribution-based technique:

```text
Source Histogram
       ↓
Source CDF
       ↓
Mapping
       ↓
Target Histogram
```

---

# 7.42 Histogram Processing Does Not Change Geometry

Histogram operations generally modify intensity values.

They do not inherently move pixels.

For example:

```text
Before:

100 100
200 200
```

After intensity transformation:

```text
50  50
240 240
```

The pixels remain in the same locations.

Therefore:

```text
Histogram processing
       ↓
Intensity transformation
       ↓
Spatial coordinates unchanged
```

---

# 7.43 Histogram Processing vs Spatial Filtering

This distinction will become important in later chapters.

### Histogram operation

Generally depends on:

```text
pixel intensity distribution
```

### Spatial filtering

Depends on:

```text
neighboring pixels
```

For example:

```text
Histogram operation:
pixel → intensity mapping
```

while:

```text
Spatial filter:
neighbors → new pixel value
```

We will study filtering in later chapters.

---

# 7.44 Medical Imaging Example

Imagine a low-contrast X-ray:

```text
Original
   ↓
Histogram concentrated in narrow range
   ↓
Contrast enhancement
   ↓
Broader useful range
   ↓
Improved visibility
```

For a local enhancement:

```text
Original
   ↓
CLAHE
   ↓
Local structures become more visible
```

However, excessive enhancement can also amplify noise and artifacts.

---

# 7.45 C++ Conceptual Histogram Class

A simple enterprise-style starting point could be:

```cpp
class ImageHistogram
{
public:

    void compute(const std::uint8_t* data,
                 std::size_t count);

    const std::vector<std::size_t>& bins() const;

private:

    std::vector<std::size_t> m_bins;
};
```

The conceptual responsibility is:

```text
ImageHistogram
      │
      ├── Input image
      │
      ├── Histogram computation
      │
      └── Histogram data
```

Later, this can become part of your image-processing architecture.

---

# 7.46 C++ CDF Calculation

Given a normalized histogram:

```cpp
std::vector<double> pdf;
std::vector<double> cdf;

double cumulative = 0.0;

for (double value : pdf)
{
    cumulative += value;
    cdf.push_back(cumulative);
}
```

This demonstrates the fundamental relationship:

[
CDF(k)=\sum_{j=0}^{k}PDF(j)
]

---

# 7.47 Histogram Equalization Concept in C++

Conceptually:

```cpp
double cdfValue = cdf[inputIntensity];

double output =
    cdfValue * 255.0;
```

Then convert/clamp appropriately.

This is the mathematical heart of histogram equalization.

A production implementation needs to carefully handle:

* empty histogram bins
* minimum nonzero CDF
* integer rounding
* data type
* output range
* performance

---

# 7.48 Python Example — Histogram

```python
import numpy as np

image = np.array([
    [10, 10, 20],
    [20, 30, 30],
    [40, 40, 50]
], dtype=np.uint8)

hist = np.bincount(
    image.ravel(),
    minlength=256
)

pdf = hist / image.size

cdf = np.cumsum(pdf)
```

Now:

```text
hist
 ↓
pdf
 ↓
cdf
```

---

# 7.49 Chapter 7 Core Mental Model

Memorize this:

```text
                  IMAGE
                    │
                    ↓
                Histogram
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
        Count             Normalize
                              │
                              ↓
                             PDF
                              │
                              ↓
                             CDF
                              │
             ┌────────────────┼───────────────┐
             ↓                ↓               ↓
         Equalization     Matching       Analysis
             │
             ↓
        Local Methods
             │
          ┌──┴──┐
          ↓     ↓
         AHE   CLAHE
```

---

# 7.50 Key Formulas

### Histogram

[
H(k)=#{pixels\ with\ intensity\ k}
]

### Normalized histogram / probability

[
p(k)=\frac{H(k)}{N}
]

### CDF

[
CDF(k)=\sum_{j=0}^{k}p(j)
]

### Histogram equalization

[
s_k=(L-1)CDF(k)
]

### Contrast stretching

[
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
(O_{max}-O_{min})+O_{min}
]

---

# 7.51 Chapter 7 Knowledge Check

### Histogram

1. What is an image histogram?
2. What does the X-axis represent?
3. What does the Y-axis represent?
4. How do you compute a histogram?
5. What does a histogram tell us?
6. What information does a histogram not contain?

### PDF / CDF

7. What is the normalized histogram?
8. What is the difference between histogram and PDF?
9. What is CDF?
10. How is CDF calculated from PDF?
11. Why does CDF eventually reach 1?

### Equalization

12. What is histogram equalization?
13. Why is CDF used in histogram equalization?
14. Write the basic histogram equalization formula.
15. What is a limitation of global histogram equalization?

### Stretching

16. What is histogram stretching?
17. How is stretching different from equalization?
18. Given:

[
I_{min}=20,\quad I_{max}=220
]

map the range to:

[
0\ldots255
]

What is the output for (I=120)?

### Matching

19. What is histogram matching?
20. Why is CDF useful for histogram matching?
21. What is the difference between histogram equalization and histogram matching?

### AHE / CLAHE

22. What is adaptive histogram equalization?
23. Why does AHE improve local contrast?
24. What is the major problem with AHE?
25. What does CLAHE stand for?
26. Why does CLAHE use contrast limiting?
27. What is a tile in CLAHE?
28. Why is interpolation used between neighboring tiles?

---

# Practical Exercise

Given:

```text
Image:

10 10 20
20 30 30
40 40 50
```

### Step 1

Calculate the histogram.

### Step 2

Calculate the normalized histogram:

[
PDF(k)=\frac{H(k)}{N}
]

### Step 3

Calculate the CDF.

### Step 4

For an 8-bit output, calculate:

[
s_k=255\times CDF(k)
]

### Step 5

Construct the equalized image.

### Step 6

Explain why histogram equalization changes intensity values but does not change the pixel positions.

---

# Medical Imaging Exercise

Consider a CT image where most voxels belong to a narrow intensity range, but a small structure contains subtle differences.

Compare the expected behavior of:

```text
1. Global histogram equalization
2. Histogram stretching
3. AHE
4. CLAHE
```

For each one, explain:

* what it does
* what happens to contrast
* whether local detail is enhanced
* whether noise can be amplified

---

## Critical Takeaway

The most important chain from this chapter is:

[
\boxed{
Histogram
\rightarrow
PDF
\rightarrow
CDF
\rightarrow
Intensity\ Mapping
}
]

And for local enhancement:

[
\boxed{
AHE
\rightarrow
Local\ Histograms
\rightarrow
Local\ Contrast
}
]

while:

[
\boxed{
CLAHE
=====

AHE+\text{Contrast Limiting}
}
]

---

**Chapter 7 is complete.**

The next chapter in your exact index is:

# Chapter 8 — Image Enhancement

Topics:

* Point processing
* Neighborhood processing
* Spatial enhancement
* Contrast enhancement
* Sharpening
* Smoothing
* Noise reduction
* Edge enhancement
* Image enhancement pipelines.
