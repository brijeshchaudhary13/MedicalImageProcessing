# Chapter 9 — Histograms

We now continue **strictly according to your master index**.

Your index defines Chapter 9 as:

1. Histogram concept
2. Histogram calculation
3. Histogram visualization
4. Histogram statistics
5. Histogram stretching
6. Histogram equalization
7. CLAHE
8. Histogram matching
9. Histogram specification 

> **Important:** We already touched histograms while discussing Chapter 6 statistics and Chapter 8 transformations. Here we study histograms as a complete topic, including the algorithms and implementation.

---

# 9.1 What Is a Histogram?

An image histogram represents the **frequency distribution of pixel intensities**.

For a grayscale image:

[
H(k)=\text{number of pixels whose intensity is }k
]

For an 8-bit image:

[
k=0,1,2,\ldots,255
]

So there are normally **256 histogram bins**.

```text
Frequency
   ↑
   │
   │          ███
   │       ███████
   │     █████████
   │  █████████████
   └────────────────────→
     0              255
          Intensity
```

---

# 9.2 Why Do We Need a Histogram?

A raw image may contain millions of pixels.

Looking at every pixel individually doesn't immediately tell us the overall intensity distribution.

The histogram compresses that information:

```text
Image
  ↓
Millions of pixels
  ↓
Histogram
  ↓
Intensity distribution
```

It helps answer questions such as:

* Is the image dark?
* Is it bright?
* Is contrast low?
* Is contrast high?
* Which intensities dominate?
* Are there multiple intensity populations?
* Are there extreme outliers?

---

# 9.3 Histogram Does NOT Describe Location

This is fundamental.

Consider:

```text
Image A

10 10
20 20
```

and:

```text
Image B

10 20
10 20
```

Both contain:

```text
10 → 2 pixels
20 → 2 pixels
```

Therefore both have the same histogram.

But the spatial arrangement is different.

So:

[
\boxed{
Histogram = intensity information
}
]

but:

[
\boxed{
Histogram \neq spatial information
}
]

---

# 9.4 Histogram Calculation

For an 8-bit image, create:

```cpp
std::array<std::size_t, 256> histogram{};
```

Initially:

```text
histogram[0]   = 0
histogram[1]   = 0
...
histogram[255] = 0
```

Then process every pixel:

```cpp
++histogram[pixel];
```

That's the fundamental algorithm.

---

# 9.5 Example

Given:

```text
10 10 20
20 30 30
40 40 50
```

Histogram:

| Intensity | Count |
| --------: | ----: |
|        10 |     2 |
|        20 |     2 |
|        30 |     2 |
|        40 |     2 |
|        50 |     1 |

All other bins:

[
H(k)=0
]

---

# 9.6 C++ Histogram Implementation

```cpp
#include <array>
#include <cstddef>
#include <cstdint>
#include <vector>

using Histogram = std::array<std::size_t, 256>;

Histogram calculateHistogram(
    const std::vector<std::uint8_t>& image)
{
    Histogram histogram{};

    for (std::uint8_t pixel : image)
    {
        ++histogram[pixel];
    }

    return histogram;
}
```

Complexity:

[
O(N)
]

where (N) is the number of pixels.

Memory:

[
O(256)
]

for the histogram itself.

---

# 9.7 Histogram for Medical Images

Here's an important difference.

Normal photographs often use:

```text
UINT8
0 → 255
```

Medical images can use:

```text
INT16
UINT16
FLOAT32
```

For example, CT data may contain signed values.

Therefore, you cannot always simply use:

```cpp
histogram[pixel]
```

because an `INT16` value may be negative.

Instead, you need to define:

* histogram range
* bin width
* number of bins
* mapping from physical intensity to bin

---

# 9.8 Binned Histogram

Suppose CT values are:

[
-1000\ldots3000
]

and we want 400 bins.

Bin width:

[
\frac{3000-(-1000)}{400}
]

[
=10
]

So each bin represents approximately 10 HU.

Conceptually:

```text
-1000
  ↓
Bin 0

-990
  ↓
Bin 1

...

2990
  ↓
Bin 399
```

This is more appropriate for continuous/wide-range medical data than assuming 256 integer bins.

---

# 9.9 Histogram Visualization

A histogram is usually displayed as:

```text
Frequency
   ↑
   │
   │          █
   │        ███
   │      █████
   │   ████████
   └─────────────────→
        Intensity
```

### X-axis

Intensity.

### Y-axis

Frequency.

---

# 9.10 Histogram Normalization

Raw histogram:

[
H(k)
]

counts pixels.

Normalized histogram:

[
\boxed{
p(k)=\frac{H(k)}{N}
}
]

where:

[
N=\text{total number of pixels}
]

Then:

[
\sum_k p(k)=1
]

This gives us a probability distribution.

---

# 9.11 Histogram Statistics

We can derive useful information from a histogram.

For example:

### Mean

[
\mu=\sum_k k,p(k)
]

### Variance

[
\sigma^2=
\sum_k(k-\mu)^2p(k)
]

### Cumulative distribution

[
CDF(k)=\sum_{j=0}^{k}p(j)
]

So:

```text
Histogram
    ↓
Normalized Histogram
    ↓
Probability Distribution
    ↓
CDF
```

---

# 9.12 Histogram Mean

Suppose:

| Intensity | Probability |
| --------: | ----------: |
|        10 |         0.2 |
|        20 |         0.3 |
|        30 |         0.5 |

Then:

[
\mu=
10(0.2)+20(0.3)+30(0.5)
]

[
=2+6+15
]

[
=23
]

So the histogram itself can be used to calculate image statistics.

---

# 9.13 Histogram and Image Brightness

Suppose the histogram is concentrated near zero:

```text
Frequency
   ↑
   │███
   │████
   │█████
   │
   └────────────────────→
     0                255
```

The image is likely dark.

If concentrated near 255:

```text
Frequency
   ↑
   │                  ███
   │                █████
   │               ██████
   └────────────────────→
     0                255
```

the image is likely bright.

---

# 9.14 Histogram and Contrast

A narrow histogram:

```text
        █████
       ███████
       ███████
```

often indicates a limited intensity range.

A broad histogram:

```text
██  ███   ██ ███  ██  ███
```

indicates that more of the available intensity range is being used.

This is one reason histograms are useful when evaluating contrast.

---

# 9.15 Histogram Stretching

Histogram stretching expands the intensity range.

Suppose:

[
r_{min}=50
]

[
r_{max}=150
]

and desired output:

[
0\ldots255
]

Use:

[
\boxed{
s=
\frac{r-r_{min}}
{r_{max}-r_{min}}
\times255
}
]

Therefore:

```text
50  → 0
100 → 128
150 → 255
```

---

# 9.16 Why Stretching?

Before:

```text
Input
50 ───────────── 150
```

Only part of the available range is used.

After:

```text
Output
0 ─────────────────── 255
```

The useful intensity differences become more visible.

---

# 9.17 Percentile-Based Stretching

Using absolute:

```text
Min
Max
```

can be dangerous when outliers exist.

Instead, use:

[
P_2
]

and:

[
P_{98}
]

for example.

Then:

[
r_{min}=P_2
]

[
r_{max}=P_{98}
]

This is particularly useful in medical images containing extreme values.

Your uploaded image-analysis work already uses P2/P98 for robust intensity analysis and window estimation. 

---

# 9.18 Histogram Equalization

Histogram equalization attempts to redistribute intensities using the cumulative distribution.

Pipeline:

```text
Histogram
    ↓
Normalize
    ↓
PDF
    ↓
CDF
    ↓
Intensity Mapping
    ↓
Output Image
```

For (L) intensity levels:

[
\boxed{
s_k=(L-1)CDF(k)
}
]

For 8-bit:

[
s_k=255,CDF(k)
]

with practical integer rounding.

---

# 9.19 Why CDF?

Suppose:

```text
Intensity
10 → 0.2
20 → 0.3
30 → 0.1
40 → 0.4
```

CDF becomes:

```text
10 → 0.2
20 → 0.5
30 → 0.6
40 → 1.0
```

This provides a monotonically increasing mapping.

```text
Input intensity
      ↓
CDF
      ↓
Output intensity
```

---

# 9.20 Equalization Example

For:

```text
CDF(10)=0.2
CDF(20)=0.5
CDF(30)=0.6
CDF(40)=1.0
```

8-bit mapping:

[
10\rightarrow51
]

[
20\rightarrow128
]

[
30\rightarrow153
]

[
40\rightarrow255
]

Thus the original intensity distribution is transformed.

---

# 9.21 Histogram Equalization: Important Limitation

Global equalization uses one histogram for the entire image.

Suppose:

```text
+----------------------+
| Very bright region   |
|                      |
|       Dark detail    |
+----------------------+
```

One global histogram may be dominated by the large bright region.

Then the dark detail may not receive enough useful enhancement.

This leads to local methods such as **CLAHE**.

---

# 9.22 CLAHE

CLAHE:

> **Contrast Limited Adaptive Histogram Equalization**

Break the image into tiles:

```text
+---------+---------+---------+
| Tile 1  | Tile 2  | Tile 3  |
+---------+---------+---------+
| Tile 4  | Tile 5  | Tile 6  |
+---------+---------+---------+
| Tile 7  | Tile 8  | Tile 9  |
+---------+---------+---------+
```

For each tile:

```text
Tile
 ↓
Histogram
 ↓
Clip histogram
 ↓
Redistribute excess
 ↓
Equalization
```

Then neighboring mappings are interpolated to reduce tile boundaries.

---

# 9.23 Why "Contrast Limited"?

Ordinary AHE can produce very large peaks in local histograms.

Those peaks can cause excessive contrast enhancement and noise amplification.

CLAHE limits histogram peaks.

Conceptually:

```text
AHE:

Peak ███████████████████
     ↓
Strong enhancement


CLAHE:

Peak ███████
     ↓
Clip
     ↓
Redistribute excess
```

---

# 9.24 CLAHE Medical Application

CLAHE can be useful when local anatomical detail is important.

Examples include:

* X-ray
* mammography
* CT visualization
* MRI visualization

But:

> Enhancement should not create artificial structures or be mistaken for diagnostic information.

This is particularly important in medical software.

---

# 9.25 Histogram Matching

Histogram matching is also called **histogram specification** in many contexts, although the terms can be distinguished conceptually.

The goal is:

> Transform a source image so its intensity distribution resembles a specified target distribution.

Pipeline:

```text
Source Image
     ↓
Source Histogram
     ↓
Source CDF
     ↓
Mapping
     ↓
Target CDF
     ↓
Target intensity
```

---

# 9.26 Why Histogram Matching?

Suppose we have:

```text
Image A
```

with:

```text
dark intensity distribution
```

and:

```text
Reference Image B
```

with:

```text
desired intensity distribution
```

We want:

```text
A
 ↓
Transformation
 ↓
A'
```

where:

[
Histogram(A')\approx Histogram(B)
]

---

# 9.27 Histogram Matching Concept

Let:

[
F(r)
]

be the source CDF.

Let:

[
G(z)
]

be the target CDF.

We want:

[
F(r)\approx G(z)
]

So for a source intensity (r):

1. Calculate source CDF.
2. Find its cumulative probability.
3. Find target intensity with approximately the same target CDF.
4. Map source intensity to that target intensity.

Conceptually:

```text
Source intensity
      ↓
Source CDF
      ↓
Probability
      ↓
Target CDF
      ↓
Target intensity
```

---

# 9.28 Histogram Specification

Histogram specification is the broader idea of **specifying a desired histogram/distribution** rather than merely trying to flatten the histogram.

Example target:

```text
Desired distribution
       ↓
   ███████
 ███████████
   ███████
```

The source image is transformed toward that distribution.

Histogram equalization can be viewed as a special case where the desired distribution is approximately uniform, subject to discrete implementation limitations.

---

# 9.29 Equalization vs Matching

| Technique               | Goal                                              |
| ----------------------- | ------------------------------------------------- |
| Histogram stretching    | Expand intensity range                            |
| Histogram equalization  | Redistribute toward a more uniform distribution   |
| Histogram matching      | Match a target distribution                       |
| Histogram specification | Specify the desired distribution                  |
| CLAHE                   | Local contrast enhancement with contrast limiting |

---

# 9.30 Histogram Processing Comparison

```text
                  HISTOGRAM
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
    Stretching   Equalization    Matching
        │             │             │
        ↓             ↓             ↓
      Range          CDF        Target CDF
                    mapping        mapping
                      │
                      ↓
                    CLAHE
                      │
                Local version
                + contrast limit
```

---

# 9.31 Global vs Local Histogram

### Global histogram

One histogram for the whole image:

```text
Image
 ↓
One histogram
 ↓
One mapping
```

### Local histogram

Many histograms:

```text
Image
 ↓
Tiles
 ↓
Histogram per tile
 ↓
Local mappings
 ↓
Interpolation
```

Global:

```text
Fast
Simple
Global enhancement
```

Local:

```text
Better local contrast
More computation
Potential noise amplification
```

---

# 9.32 Histogram Processing Pipeline for a Medical Viewer

A professional viewer can use:

```text
DICOM / RAW
     ↓
Original image
     ↓
Intensity conversion
     ↓
Window / Level
     ↓
Optional histogram analysis
     ↓
Optional stretching / equalization
     ↓
Optional CLAHE
     ↓
LUT
     ↓
Display
```

The original quantitative data should remain separate from the display-enhanced representation.

---

# 9.33 Histogram and CT

CT is particularly interesting because voxel values have physical meaning after appropriate DICOM rescaling.

For example:

```text
Raw stored value
      ↓
Rescale Slope/Intercept
      ↓
HU
      ↓
CT intensity distribution
      ↓
Histogram
```

You should not casually histogram raw stored values and assume they are already HU.

This distinction will become important when we reach DICOM.

---

# 9.34 Histogram and X-Ray

For an X-ray:

```text
Detector data
    ↓
Intensity distribution
    ↓
Histogram
    ↓
Exposure / contrast analysis
    ↓
Display mapping
```

Your uploaded X-ray enhancement work uses several intensity-distribution-based stages, including normalization and CLAHE, demonstrating how histogram-driven enhancement fits into a larger pipeline. 

---

# 9.35 C++ Histogram Class

A clean starting architecture:

```cpp
class Histogram
{
public:

    void compute(
        const std::uint8_t* data,
        std::size_t count);

    std::size_t bin(
        std::size_t index) const;

    const std::array<
        std::size_t,
        256>& bins() const;

private:

    std::array<std::size_t, 256> m_bins{};
};
```

Responsibility:

```text
Histogram
   │
   ├── compute
   ├── store bins
   └── expose statistics
```

Later, for medical images, you can generalize this to arbitrary numeric ranges and bin counts.

---

# 9.36 CDF Implementation

```cpp
std::vector<double> calculateCDF(
    const std::array<std::size_t, 256>& histogram,
    std::size_t totalPixels)
{
    std::vector<double> cdf(256);

    if (totalPixels == 0)
        return cdf;

    double cumulative = 0.0;

    for (std::size_t i = 0; i < 256; ++i)
    {
        cumulative +=
            static_cast<double>(histogram[i])
            / static_cast<double>(totalPixels);

        cdf[i] = cumulative;
    }

    return cdf;
}
```

Notice the pipeline:

```text
Histogram
 ↓
Normalize
 ↓
Cumulative sum
 ↓
CDF
```

---

# 9.37 Histogram Equalization LUT

Once we have the CDF:

```cpp
std::array<std::uint8_t, 256>
createEqualizationLUT(
    const std::vector<double>& cdf)
{
    std::array<std::uint8_t, 256> lut{};

    for (std::size_t i = 0; i < 256; ++i)
    {
        const double value =
            cdf[i] * 255.0;

        lut[i] =
            static_cast<std::uint8_t>(
                std::clamp(value, 0.0, 255.0));
    }

    return lut;
}
```

Then:

```text
Input pixel
    ↓
LUT[input]
    ↓
Equalized pixel
```

---

# 9.38 Why LUT Is Useful Here

Histogram equalization calculates a mapping from input intensity to output intensity.

Once the mapping is known:

```text
Input 0   → Output ...
Input 1   → Output ...
...
Input 255 → Output ...
```

So instead of recalculating the CDF for every pixel, we calculate it once and use a LUT.

This is:

```text
Fast
Predictable
Easy to implement
```

---

# 9.39 Python Histogram

```python
import numpy as np

image = np.array([
    [10, 10, 20],
    [20, 30, 30],
    [40, 40, 50]
], dtype=np.uint8)

histogram = np.bincount(
    image.ravel(),
    minlength=256
)

print(histogram[10])
print(histogram[20])
print(histogram[30])
```

---

# 9.40 Python CDF

```python
pdf = histogram / image.size

cdf = np.cumsum(pdf)
```

Now:

```text
histogram
    ↓
pdf
    ↓
cdf
```

---

# 9.41 Python Equalization Concept

```python
lut = np.floor(
    255 * cdf
).astype(np.uint8)

equalized = lut[image]
```

This is the basic mathematical implementation.

For production image processing, edge cases, CDF normalization choices, datatype handling, and library behavior must be considered carefully.

---

# 9.42 Histogram Matching Algorithm

Suppose we have:

```text
Source image
Target image
```

Algorithm:

### Step 1

Calculate source histogram.

### Step 2

Normalize source histogram.

### Step 3

Calculate source CDF.

### Step 4

Calculate target histogram.

### Step 5

Normalize target histogram.

### Step 6

Calculate target CDF.

### Step 7

For every source intensity, find the target intensity whose CDF is closest.

### Step 8

Create mapping LUT.

### Step 9

Apply LUT.

```text
Source
  ↓
Source CDF ─────┐
                │
                ↓
             Mapping
                ↑
                │
Target CDF ─────┘
                ↓
              LUT
                ↓
         Output Image
```

---

# 9.43 Histogram Matching Pseudocode

```text
sourceHistogram = histogram(source)

targetHistogram = histogram(target)

sourceCDF = CDF(sourceHistogram)

targetCDF = CDF(targetHistogram)

for each source intensity r:

    p = sourceCDF[r]

    find target intensity z
    where targetCDF[z] is closest to p

    LUT[r] = z

output = LUT[source]
```

This is a key algorithm to understand before relying on a library function.

---

# 9.44 Histogram Specification

If you are given a desired distribution rather than an actual reference image:

```text
Desired histogram
       ↓
Desired PDF
       ↓
Desired CDF
       ↓
Mapping
       ↓
Source image
```

So:

```text
Matching
 ↓
Target image provides distribution

Specification
 ↓
You explicitly provide desired distribution
```

---

# 9.45 Important Limitation of Histogram Matching

Matching histograms does **not** guarantee that images become visually or anatomically equivalent.

Two images can have identical histograms while having completely different spatial structures.

Remember:

[
\boxed{
Histogram \neq Anatomy
}
]

Therefore histogram matching should not be interpreted as image registration or anatomical normalization.

---

# 9.46 Histogram Processing and Noise

Suppose an image contains noise:

```text
Signal
+
Noise
```

Equalization may amplify the noise because it changes intensity differences.

CLAHE can also amplify local noise if its settings are aggressive.

Therefore:

```text
Enhancement
   ↓
Potentially better visibility
   +
Potentially more visible noise
```

This tradeoff is extremely important in medical imaging.

---

# 9.47 Histogram Processing and Diagnostic Images

For clinical software:

```text
Raw / quantitative image
          │
          ├──────────────→ Preserve
          │
          ↓
     Display pipeline
          ↓
     Histogram method
          ↓
      Enhanced view
```

The enhanced image should not silently replace the original diagnostic data.

A professional viewer should clearly distinguish:

* original image
* processed image
* display settings
* processing parameters

---

# 9.48 Histogram Chapter Mental Model

Memorize:

```text
                       IMAGE
                         │
                         ▼
                    HISTOGRAM
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
      Statistics      Stretching    Equalization
          │                             │
          │                             ↓
          │                            CDF
          │                             │
          │                         Global mapping
          │                             │
          │                             ↓
          │                           CLAHE
          │                             │
          │                        Local mapping
          │
          └──────────────────────┐
                                 ↓
                           Matching /
                          Specification
                                 │
                                 ↓
                            Target CDF
```

---

# 9.49 Comparison Table

| Method        | Main Idea                   | Global/Local   | Main Use           |
| ------------- | --------------------------- | -------------- | ------------------ |
| Histogram     | Count intensities           | Global         | Analysis           |
| Stretching    | Expand range                | Global         | Contrast           |
| Equalization  | Redistribute using CDF      | Global         | Contrast           |
| CLAHE         | Local equalization + limit  | Local          | Local contrast     |
| Matching      | Match target distribution   | Global         | Standardization    |
| Specification | Define desired distribution | Usually global | Controlled mapping |

---

# 9.50 Key Formulas

### Histogram

[
\boxed{
H(k)=#{pixels=k}
}
]

### Normalized histogram

[
\boxed{
p(k)=\frac{H(k)}{N}
}
]

### CDF

[
\boxed{
CDF(k)=\sum_{j=0}^{k}p(j)
}
]

### Histogram stretching

[
\boxed{
s=
\frac{r-r_{min}}
{r_{max}-r_{min}}
(s_{max}-s_{min})
+s_{min}
}
]

### Histogram equalization

[
\boxed{
s_k=(L-1)CDF(k)
}
]

### LUT

[
\boxed{
output=LUT[input]
}
]

---

# 9.51 Chapter 9 — Knowledge Check

### Histogram

1. What is a histogram?
2. What does a histogram's X-axis represent?
3. What does its Y-axis represent?
4. Why does a histogram not contain spatial information?
5. What is a histogram bin?

### Calculation

6. Calculate the histogram of:

```text
10 10 20
20 30 30
40 40 50
```

7. What is the complexity of calculating a histogram for (N) pixels?

### Statistics

8. How do you obtain a normalized histogram?
9. What is the relationship between histogram and PDF?
10. How is CDF calculated?

### Stretching

11. What is histogram stretching?
12. Why might P2/P98 be preferable to Min/Max?
13. What happens to contrast after stretching?

### Equalization

14. Why does histogram equalization use the CDF?
15. What is the basic equalization formula?
16. What is a limitation of global equalization?

### CLAHE

17. What does CLAHE stand for?
18. Why does CLAHE use tiles?
19. Why is contrast limiting necessary?
20. Why can CLAHE amplify noise?

### Matching

21. What is histogram matching?
22. How does the source CDF participate in matching?
23. How does the target CDF participate?
24. What is histogram specification?
25. What is the difference between matching and specification?

---

# 9.52 Practical Exercise

Given:

```text
Image:

10  20  20
30  40  40
40  50  60
```

### Exercise 1

Calculate the histogram.

### Exercise 2

Calculate the normalized histogram.

### Exercise 3

Calculate the CDF.

### Exercise 4

For an 8-bit output, calculate:

[
s_k=255CDF(k)
]

### Exercise 5

Create the equalization LUT.

### Exercise 6

Generate the equalized image.

---

# 9.53 Medical Imaging Exercise

Imagine a CT slice containing:

```text
Large soft-tissue region
+
small high-density structure
+
noise
```

Compare:

### A. Histogram stretching

### B. Global histogram equalization

### C. CLAHE

For each explain:

* global or local?
* effect on contrast?
* effect on noise?
* computational cost?
* suitability for visualization?
* should original CT values be modified?

---

# 9.54 Interview Questions

### Beginner

**Q1. What is an image histogram?**

**Q2. What is the difference between histogram and normalized histogram?**

**Q3. What is CDF?**

**Q4. Why does histogram equalization improve contrast?**

### Intermediate

**Q5. Histogram stretching vs histogram equalization?**

**Q6. Why can global equalization fail for images with uneven illumination?**

**Q7. How does CLAHE solve this problem?**

### Advanced

**Q8. Explain histogram matching mathematically.**

**Q9. Why is CDF used for histogram matching?**

**Q10. Why might histogram equalization amplify noise?**

### Medical Imaging

**Q11. Why should histogram processing not overwrite original CT values?**

**Q12. Why can P2/P98 be more useful than Min/Max for CT display?**

**Q13. What is the difference between histogram processing and DICOM window/level?**

---

# 9.55 Important Connection to Your Medical Viewer

For the medical image viewer you are building, think of the pipeline as:

```text
                    DICOM
                      │
                      ▼
              Pixel Data / Metadata
                      │
                      ▼
             Rescale / Physical Units
                      │
                      ▼
                Image Statistics
                      │
          ┌───────────┴───────────┐
          ↓                       ↓
      Histogram              Min/P2/P98
          │                       │
          └───────────┬───────────┘
                      ↓
                 Display Range
                      ↓
                 Window/Level
                      ↓
              Optional Enhancement
                      │
             ┌────────┼─────────┐
             ↓        ↓         ↓
          Stretch   CLAHE     Gamma
             │        │         │
             └────────┼─────────┘
                      ↓
                     LUT
                      ↓
                 GPU / QML
                      ↓
                  Screen
```

This distinction between **original medical data** and **display processing** is one of the most important architectural concepts for your future enterprise DICOM viewer.

---

## Chapter 9 Complete

The core idea to remember is:

[
\boxed{
Histogram
\rightarrow
Distribution
\rightarrow
CDF
\rightarrow
Intensity Mapping
}
]

And the major methods are:

```text
Histogram
    ↓
Analyze
    ├── Statistics
    ├── Stretching
    ├── Equalization
    ├── CLAHE
    └── Matching / Specification
```

### Next — Chapter 10: Image Arithmetic and Logic

Strictly from your index:

* Image addition
* Image subtraction
* Image averaging
* Image masking
* AND
* OR
* XOR
* NOT
* Difference images
* Background subtraction 
