# Chapter 21 — Image Texture Analysis

We continue **strictly according to your index**.

Chapter 21 covers:

1. What is texture?
2. Texture vs intensity
3. Statistical texture
4. First-order statistics
5. Second-order statistics
6. GLCM
7. Contrast
8. Dissimilarity
9. Homogeneity
10. Energy
11. ASM
12. Correlation
13. Entropy
14. Local Binary Patterns
15. Gabor filters
16. Wavelet-based texture
17. Run-Length Matrix
18. Texture segmentation
19. Medical texture analysis
20. Radiomics fundamentals

---

# 21.1 What Is Texture?

Texture describes the **spatial arrangement and statistical behavior of intensity values** in an image.

Intensity tells you:

> How bright/dark is this pixel?

Texture tells you:

> **How are neighboring pixels arranged?**

For example:

```text id="x8w4c9"
Image A:

100 100 100
100 100 100
100 100 100
```

Very uniform texture.

Image B:

```text id="3k6m5e"
100 200 100
200 100 200
100 200 100
```

Much more variation.

Even if both images have similar average intensity, their textures are very different.

---

# 21.2 Intensity vs Texture

Consider:

```text id="j8q9c3"
Image A:

100 100 100
100 100 100
100 100 100
```

Mean:

[
100
]

Now:

```text id="s8j3pd"
Image B:

50 150 50
150 50 150
50 150 50
```

Mean is also:

[
100
]

But:

```text id="8bq1je"
A → smooth
B → highly varying
```

Therefore:

[
\boxed{
Intensity \neq Texture
}
]

---

# 21.3 Why Texture Matters

Texture can help distinguish structures with similar average intensity.

For example:

```text id="2xqk7v"
Tissue A
Mean ≈ 100
Texture = smooth

Tissue B
Mean ≈ 100
Texture = heterogeneous
```

A simple intensity threshold may not separate them.

Texture features can provide additional information.

---

# 21.4 Texture in Medical Imaging

Texture analysis can be useful for:

* lesion characterization
* tissue analysis
* tumor heterogeneity
* fibrosis-related analysis
* organ characterization
* radiomics
* classification
* segmentation

But texture features should not automatically be interpreted as clinically meaningful biomarkers without appropriate validation.

---

# 21.5 Texture Categories

A useful hierarchy:

```text id="u4xj1b"
Texture
  │
  ├── Statistical
  │     ├── First-order
  │     └── Second-order
  │
  ├── Structural
  │
  ├── Model-based
  │
  └── Transform-based
        ├── Gabor
        └── Wavelet
```

This chapter focuses heavily on statistical and transform-based approaches.

---

# 21.6 First-Order Statistics

First-order statistics use individual pixel intensity distributions.

They do **not** consider the spatial relationship between neighboring pixels.

Given intensities:

```text id="1u2gcy"
10 20 30
20 30 40
30 40 50
```

we can calculate:

* mean
* variance
* standard deviation
* minimum
* maximum
* median
* percentiles
* skewness
* kurtosis
* entropy

---

# 21.7 Mean

The mean is:

[
\boxed{
\mu=\frac{1}{N}\sum_{i=1}^{N}x_i
}
]

It represents average intensity.

Example:

```text id="yd0wup"
10 20 30
```

[
\mu=\frac{10+20+30}{3}=20
]

---

# 21.8 Variance

Variance measures intensity spread:

[
\boxed{
\sigma^2=
\frac{1}{N}
\sum_i(x_i-\mu)^2
}
]

Higher variance:

```text id="5vqm3w"
More intensity variation
```

Lower variance:

```text id="pqz6y1"
More uniform region
```

---

# 21.9 Standard Deviation

Standard deviation:

[
\boxed{
\sigma=\sqrt{\sigma^2}
}
]

It is in the same intensity units as the original data.

---

# 21.10 Range

Range:

[
\boxed{
Range=Max-Min
}
]

Example:

```text id="jzj2i9"
Min = 20
Max = 100
```

Then:

[
Range=80
]

---

# 21.11 Median

The median is the middle value after sorting.

Example:

```text id="lskw2d"
10, 20, 30, 40, 50
```

Median:

[
30
]

The median is often more robust to extreme outliers than the mean.

---

# 21.12 Skewness

Skewness describes asymmetry in a distribution.

Conceptually:

```text id="mm3k6v"
Symmetric
      /\

Positive skew
      /\
     /  \____

Negative skew
____/\
   /  \
```

It can describe whether intensity distributions have longer tails on one side.

---

# 21.13 Kurtosis

Kurtosis describes aspects of the distribution's tail/peakedness.

It can help distinguish:

```text id="6fjg8x"
Different intensity distributions
```

but its exact interpretation depends on the definition used by the software.

---

# 21.14 First-Order Limitation

First-order statistics ignore spatial arrangement.

Compare:

```text id="6y0q8e"
A:

100 100
200 200
```

and:

```text id="y1e1y6"
B:

100 200
100 200
```

They can have identical intensity histograms.

But spatial organization differs.

First-order statistics cannot capture that difference.

---

# 21.15 Second-Order Statistics

Second-order statistics consider relationships between pairs of pixels.

This is where:

[
\boxed{
GLCM
}
]

becomes important.

---

# 21.16 GLCM

GLCM means:

[
\boxed{
Gray-Level\ Co-occurrence\ Matrix
}
]

It describes how frequently pairs of gray levels occur with a specified spatial relationship.

Parameters include:

* distance
* direction

---

# 21.17 GLCM Example

Consider:

```text id="l9q5cn"
1 1 2
1 2 2
2 2 3
```

Suppose we examine horizontal neighbors with distance:

[
d=1
]

We count pairs such as:

```text id="5p5g1h"
(1,1)
(1,2)
(2,2)
(2,3)
```

These counts form the GLCM.

---

# 21.18 GLCM Matrix

For gray levels:

```text id="6h7xg0"
1
2
3
```

we might obtain:

[
P=
\begin{bmatrix}
2&3&0\
1&4&2\
0&1&1
\end{bmatrix}
]

The exact values depend on:

* image
* direction
* distance
* whether the matrix is symmetric
* normalization

---

# 21.19 GLCM Normalization

The raw matrix contains counts.

We can normalize:

[
\boxed{
p(i,j)=
\frac{P(i,j)}
{\sum_{i,j}P(i,j)}
}
]

Then:

[
\sum_{i,j}p(i,j)=1
]

Now the GLCM can be treated like a joint probability distribution.

---

# 21.20 GLCM Direction

Common directions:

```text id="cr7z3n"
0°
→

45°
↗

90°
↑

135°
↖
```

Texture can be directional.

For example:

```text id="2f3kqp"
Horizontal fibers
```

may produce a different GLCM at:

[
0^\circ
]

than at:

[
90^\circ
]

---

# 21.21 GLCM Distance

You can calculate GLCM using:

[
d=1
]

or:

[
d=2,3,\ldots
]

Small distance:

```text id="qk7f0v"
Fine texture
```

Larger distance:

```text id="8m7jps"
Coarser spatial relationships
```

---

# 21.22 GLCM Contrast

GLCM contrast measures intensity difference between neighboring pairs.

[
\boxed{
Contrast=
\sum_{i,j}(i-j)^2p(i,j)
}
]

Large differences contribute strongly.

Therefore:

```text id="v4p7yh"
High contrast
 ↓
strong local intensity variation
```

---

# 21.23 GLCM Dissimilarity

Dissimilarity:

[
\boxed{
Dissimilarity=
\sum_{i,j}|i-j|p(i,j)
}
]

Compared with contrast:

```text id="h8c4rn"
Contrast:
(i-j)²

Dissimilarity:
|i-j|
```

Large differences have stronger emphasis in contrast.

---

# 21.24 Contrast vs Dissimilarity

| Metric        | Formula   | Behavior                             |   |                |
| ------------- | --------- | ------------------------------------ | - | -------------- |
| Contrast      | ((i-j)^2) | Strongly penalizes large differences |   |                |
| Dissimilarity | (         | i-j                                  | ) | Linear penalty |

---

# 21.25 GLCM Homogeneity

Homogeneity measures how concentrated the GLCM is around the diagonal.

A common definition:

[
\boxed{
Homogeneity=
\sum_{i,j}
\frac{p(i,j)}
{1+|i-j|}
}
]

If neighboring pixels have similar intensities:

[
i\approx j
]

then homogeneity is high.

---

# 21.26 Homogeneity Intuition

Smooth region:

```text id="p3kh2h"
100 101 100
101 100 102
100 101 100
```

Pairs are similar.

Therefore:

[
\boxed{
High\ homogeneity
}
]

Heterogeneous region:

```text id="9s0m5p"
50 200 50
200 50 200
50 200 50
```

contains strong differences.

Therefore:

```text id="5x1kz2"
Lower homogeneity
```

---

# 21.27 GLCM Energy

Energy is related to uniformity.

[
\boxed{
Energy=
\sum_{i,j}p(i,j)^2
}
]

A common equivalent is the square root of ASM:

[
Energy=\sqrt{ASM}
]

depending on the feature convention.

---

# 21.28 ASM

ASM means:

[
\boxed{
Angular\ Second\ Moment
}
]

[
\boxed{
ASM=
\sum_{i,j}p(i,j)^2
}
]

High ASM means the GLCM probability distribution is concentrated in fewer entries.

---

# 21.29 Energy vs ASM

They are related:

[
\boxed{
Energy=\sqrt{ASM}
}
]

when using the standard definitions above.

Therefore they contain related information.

---

# 21.30 GLCM Correlation

Correlation measures relationships between the gray levels of neighboring pixels.

A common formulation:

[
\boxed{
Correlation=
\frac{
\sum_{i,j}
(i-\mu_i)(j-\mu_j)p(i,j)
}{
\sigma_i\sigma_j
}
}
]

where:

* (\mu_i,\mu_j) = marginal means
* (\sigma_i,\sigma_j) = marginal standard deviations

It measures how strongly paired gray levels are related.

---

# 21.31 GLCM Entropy

Entropy measures randomness/complexity.

[
\boxed{
Entropy=
-\sum_{i,j}p(i,j)\log p(i,j)
}
]

with the convention:

[
0\log0=0
]

High entropy:

```text id="1un7yd"
More diverse/random distribution
```

Low entropy:

```text id="y9u9jv"
More uniform/predictable distribution
```

---

# 21.32 Entropy Example

Suppose:

```text id="7m8ndh"
Distribution A:

p = [1,0,0,0]
```

Entropy:

[
0
]

Very predictable.

Now:

```text id="a1jtdp"
Distribution B:

p=[0.25,0.25,0.25,0.25]
```

Entropy is higher.

More possible outcomes:

[
\boxed{
Higher\ uncertainty
}
]

---

# 21.33 GLCM Feature Summary

| Feature       | General Interpretation         |
| ------------- | ------------------------------ |
| Contrast      | Intensity differences          |
| Dissimilarity | Absolute intensity differences |
| Homogeneity   | Local similarity               |
| ASM           | Uniformity                     |
| Energy        | Uniformity                     |
| Correlation   | Pairwise linear relationship   |
| Entropy       | Randomness/complexity          |

---

# 21.34 GLCM Directional Analysis

Instead of using only one direction:

```text id="bq3z4u"
0°
45°
90°
135°
```

you can calculate GLCMs for multiple directions.

Then:

```text id="x8f2kd"
Feature0
Feature45
Feature90
Feature135
```

can be combined.

---

# 21.35 Averaging GLCMs

A common approach is:

```text id="l84t0a"
GLCM0
GLCM45
GLCM90
GLCM135
   ↓
Average / aggregate
   ↓
Texture features
```

But the exact method should be standardized because different implementations can produce different feature values.

---

# 21.36 Texture Anisotropy

If texture changes strongly with direction:

```text id="6k0p8f"
0° → strong
90° → weak
```

the texture is directional.

This can be important for:

* fibers
* muscle
* trabecular structures
* vessels
* oriented tissues

---

# 21.37 Local Binary Pattern — LBP

LBP is a local texture descriptor.

It compares neighboring pixels with a center pixel.

Basic example with 8 neighbors:

```text id="m6jv4q"
p0 p1 p2
p3 pc p4
p5 p6 p7
```

For each neighbor:

```text id="ydl9js"
neighbor >= center → 1
neighbor < center  → 0
```

This produces a binary pattern.

---

# 21.38 LBP Example

Suppose center:

[
p_c=100
]

Neighbors:

```text id="fmyw0o"
110  90 120
 95 100 105
 80 100 130
```

Compare each neighbor to 100:

```text id="y4g1fu"
1 0 1
0 X 1
0 1 1
```

This gives an 8-bit pattern.

---

# 21.39 LBP Binary Code

The bits can be ordered around the center:

```text id="ylv42b"
1 0 1 1 1 1 0 0
```

which corresponds to a binary number.

A histogram of LBP patterns over a region becomes the texture descriptor.

---

# 21.40 Why LBP Works

LBP captures local micro-patterns such as:

* edges
* corners
* spots
* flat areas

It is relatively simple and computationally efficient.

---

# 21.41 LBP Strengths

* simple
* fast
* local
* relatively robust to monotonic intensity changes
* useful for texture classification

---

# 21.42 LBP Limitations

Basic LBP can be sensitive to:

* noise
* sampling
* scale
* rotation depending on the variant

Therefore many LBP variants exist.

Examples:

```text id="yytmqi"
Rotation-invariant LBP
Uniform LBP
Multi-scale LBP
```

---

# 21.43 Gabor Filters

Gabor filters are useful for analyzing texture at different:

* orientations
* frequencies

A Gabor filter can be represented conceptually as:

[
\boxed{
G(x,y)=
\text{Gaussian envelope}
\times
\text{sinusoidal carrier}
}
]

---

# 21.44 Gabor Filter Intuition

Think of a Gabor filter as asking:

> Does this region contain a pattern with this frequency and orientation?

For example:

```text id="x7kzj4"
Horizontal texture
────────────
────────────
────────────
```

A horizontal Gabor filter may respond strongly.

---

# 21.45 Gabor Orientation

You can create filters at:

```text id="y1b6gc"
0°
45°
90°
135°
```

Then:

```text id="q9r9e0"
Image
 ↓
Gabor bank
 ↓
Multiple responses
```

---

# 21.46 Gabor Frequency

Different frequencies detect different texture scales.

```text id="x7ap0e"
High frequency
 ↓
Fine texture

Low frequency
 ↓
Coarse texture
```

Therefore Gabor filtering provides multi-scale and multi-orientation texture information.

---

# 21.47 Gabor Filter Bank

A practical system might use:

```text id="9ozr5j"
4 orientations
×
5 frequencies
```

giving:

[
20
]

filter responses.

Features can then be calculated from those responses.

---

# 21.48 Wavelet Texture Analysis

Wavelets analyze an image at multiple scales.

Conceptually:

```text id="9qj8as"
Image
 ↓
Wavelet transform
 ↓
Low-frequency component
+
High-frequency components
```

---

# 21.49 Wavelet Decomposition

A 2D wavelet transform typically produces:

```text id="o5wz9s"
        Image
          ↓
      Wavelet Transform
          ↓
 ┌────────┼────────┐
 ↓        ↓        ↓
LL       LH       HL       HH
```

where:

* LL = low-low
* LH = low-high
* HL = high-low
* HH = high-high

Exact naming depends on convention.

---

# 21.50 Wavelet Components

### LL

Mostly low-frequency structure.

```text id="4lkwj5"
Smooth information
```

### LH

Captures detail in one orientation.

### HL

Captures another directional detail.

### HH

Captures high-frequency details.

---

# 21.51 Why Wavelets Help Texture Analysis

Texture exists at different scales.

Wavelets allow:

```text id="r1qf8x"
Coarse structure
+
Fine structure
```

to be analyzed separately.

This can be useful for:

* medical image texture
* denoising
* feature extraction
* compression
* radiomics

---

# 21.52 Run-Length Matrix

The Gray-Level Run Length Matrix (GLRLM) captures consecutive runs of similar intensity.

Example:

```text id="7q4h0p"
111122233
```

Runs:

```text id="6p9y0j"
1111 → length 4
222  → length 3
33   → length 2
```

The matrix records:

```text id="l0y6v1"
Gray level
+
Run length
```

---

# 21.53 Why Run Length Matters

It captures whether texture contains:

```text id="2wzj8q"
Short runs
```

or:

```text id="8b6mzt"
Long runs
```

A homogeneous region may have longer runs.

A highly heterogeneous texture may contain many short runs.

---

# 21.54 GLRLM Features

Common features include:

* Short Run Emphasis
* Long Run Emphasis
* Gray-Level Non-Uniformity
* Run-Length Non-Uniformity
* Low Gray-Level Run Emphasis
* High Gray-Level Run Emphasis

These quantify texture structure.

---

# 21.55 Texture Segmentation

Texture can itself be used to divide an image into regions.

Pipeline:

```text id="8bqj4a"
Image
 ↓
Texture feature extraction
 ↓
Texture feature map
 ↓
Clustering / thresholding
 ↓
Texture regions
```

This is useful when intensity alone cannot separate regions.

---

# 21.56 Example

Suppose two regions have:

```text id="v0y5h5"
Same mean intensity
```

but:

```text id="wzj1z6"
Region A → smooth
Region B → heterogeneous
```

A texture feature such as:

[
Contrast
]

could distinguish them.

---

# 21.57 Texture Feature Map

Instead of calculating one texture value for the entire image, calculate it locally.

```text id="g9tq2p"
Image
 ↓
Sliding window
 ↓
Texture feature
 ↓
Feature map
```

Example:

```text id="f1q0xu"
Contrast map
```

Each pixel/region receives a texture value.

---

# 21.58 Sliding Window

For each location:

```text id="m3t6j4"
┌───────┐
│       │
│ window│
│       │
└───────┘
```

calculate:

```text id="8n1j6z"
Mean
Variance
GLCM contrast
Entropy
LBP
```

Then move the window.

---

# 21.59 Window Size

This is critical.

Small window:

```text id="wq8x8n"
Fine texture
```

Large window:

```text id="s5c8kh"
Coarse texture
```

Therefore:

[
\boxed{
Window\ size
\rightarrow
texture\ scale
}
]

---

# 21.60 Medical Texture Analysis

Medical image texture analysis attempts to quantify patterns that may not be obvious visually.

Example:

```text id="3v0h3k"
Tumor
 ↓
ROI
 ↓
Texture features
 ↓
Feature vector
```

Example feature vector:

```text id="9k0t1n"
[
 mean,
 variance,
 entropy,
 contrast,
 homogeneity,
 energy,
 correlation,
 LBP,
 GLRLM features
]
```

---

# 21.61 Radiomics

Radiomics is a broader framework for extracting large numbers of quantitative features from medical images.

Conceptual pipeline:

```text id="k31x3d"
Medical Image
      ↓
Segmentation
      ↓
ROI
      ↓
Preprocessing
      ↓
Feature Extraction
      ↓
Feature Vector
      ↓
Statistical / ML Analysis
```

---

# 21.62 Radiomics Feature Categories

Common categories include:

```text id="9yl2kv"
Radiomics
 │
 ├── First-order statistics
 │
 ├── Shape
 │
 ├── GLCM
 │
 ├── GLRLM
 │
 ├── GLSZM
 │
 ├── GLDM
 │
 └── Transform-based
```

The exact feature set depends on the radiomics specification and software implementation.

---

# 21.63 Shape Features

Examples:

```text id="2u6f4b"
Volume
Surface area
Maximum diameter
Compactness
Sphericity
```

These are derived from the segmentation geometry rather than intensity texture.

---

# 21.64 First-Order Radiomics

Examples:

[
\boxed{
Mean
}
]

[
\boxed{
Median
}
]

[
\boxed{
Variance
}
]

[
\boxed{
Skewness
}
]

[
\boxed{
Kurtosis
}
]

[
\boxed{
Entropy
}
]

These summarize the intensity distribution inside the ROI.

---

# 21.65 Second-Order Radiomics

Examples:

```text id="5s4a1p"
GLCM
```

which provides:

* contrast
* correlation
* homogeneity
* energy
* entropy

These incorporate spatial relationships.

---

# 21.66 Why Segmentation Is Critical for Radiomics

Suppose the ROI is:

```text id="6zhm2k"
Correct tumor
```

then:

```text id="d9t1x7"
Texture features
```

represent the intended region.

But if the ROI contains:

```text id="4z1p1q"
Tumor
+
healthy tissue
```

the features may change substantially.

Therefore:

[
\boxed{
Segmentation\ quality
\rightarrow
Radiomics\ feature\ quality
}
]

---

# 21.67 Preprocessing Matters

Radiomics can be sensitive to:

* voxel spacing
* intensity discretization
* normalization
* reconstruction parameters
* acquisition differences
* scanner differences

Therefore a robust pipeline needs standardized preprocessing.

---

# 21.68 Intensity Discretization

GLCM and related texture methods often use a finite number of gray levels.

Suppose original CT has many intensity values.

You may discretize:

```text id="6t9rj3"
HU
 ↓
Gray-level bins
 ↓
Texture matrix
```

Example:

```text id="c8g2y6"
0–49   → 1
50–99  → 2
100–149 → 3
150–199 → 4
```

The binning strategy can significantly influence texture features.

---

# 21.69 Why Discretization Matters

Suppose:

```text id="rb0qvj"
Method A
→ 32 bins

Method B
→ 128 bins
```

The resulting GLCM can be different.

Therefore:

[
\boxed{
Texture\ features
depend\ on\ feature\ extraction\ settings
}
]

This is one reason reproducibility is important in radiomics.

---

# 21.70 Voxel Spacing

Suppose:

```text id="6a4x7m"
Dataset A:
0.5 × 0.5 × 1.0 mm

Dataset B:
1.0 × 1.0 × 3.0 mm
```

The same physical structure can produce very different pixel/voxel patterns.

Therefore texture comparisons should account for spatial resolution and acquisition characteristics.

---

# 21.71 2D vs 3D Texture

Texture can be calculated on:

### 2D

Each slice separately.

```text id="lq0y71"
Slice 1 → features
Slice 2 → features
Slice 3 → features
```

### 3D

Across the full volume.

```text id="6ydx8f"
3D ROI
 ↓
3D texture features
```

These approaches are not equivalent.

---

# 21.72 Medical Imaging Example

Suppose:

```text id="1c7s3j"
CT Volume
 ↓
Tumor segmentation
 ↓
3D ROI
 ↓
Resampling / preprocessing
 ↓
Discretization
 ↓
GLCM / GLRLM / first-order
 ↓
Feature vector
```

This is a simplified radiomics workflow.

---

# 21.73 Texture and Tumor Heterogeneity

A tumor can appear:

```text id="q1s5c8"
Homogeneous
```

or:

```text id="k7f4vz"
Heterogeneous
```

Texture metrics attempt to quantify such patterns.

For example:

```text id="u0h1d9"
Higher variation
 ↓
potentially higher heterogeneity
```

But:

[
\boxed{
Texture\ heterogeneity
\neq
specific\ diagnosis
}
]

Clinical interpretation requires validated evidence.

---

# 21.74 Texture and Classification

A machine-learning pipeline could be:

```text id="t9r4zq"
ROI
 ↓
Texture features
 ↓
Feature normalization
 ↓
Feature selection
 ↓
Classifier
 ↓
Prediction
```

Possible models:

* logistic regression
* SVM
* random forest
* gradient boosting
* neural networks

---

# 21.75 Feature Selection

If we extract:

```text id="p9q4h0"
500 features
```

we may not want to use all of them.

Some can be:

* redundant
* unstable
* noisy
* irrelevant

Therefore:

```text id="8s1f7k"
500 features
 ↓
Feature selection
 ↓
20 useful features
```

---

# 21.76 Feature Normalization

Features can have very different scales:

```text id="m7x3p2"
Volume = 12000
Entropy = 2.5
Circularity = 0.8
```

A model may benefit from normalization.

For example:

[
z=
\frac{x-\mu}{\sigma}
]

This is standardization.

---

# 21.77 Texture Feature Stability

A medical texture feature is useful only if it is sufficiently reproducible.

Changes can come from:

```text id="5l5w9h"
Scanner
Reconstruction
Slice thickness
Noise
Segmentation
Discretization
Preprocessing
```

Therefore:

[
\boxed{
Reproducibility
}
]

is a major consideration.

---

# 21.78 Texture Pipeline Architecture

For your medical imaging software:

```text id="qf4m0p"
TextureEngine
      │
      ├── FirstOrder
      │     ├── Mean
      │     ├── Variance
      │     ├── Skewness
      │     └── Entropy
      │
      ├── GLCM
      │     ├── Contrast
      │     ├── Dissimilarity
      │     ├── Homogeneity
      │     ├── Energy
      │     ├── Correlation
      │     └── Entropy
      │
      ├── LBP
      │
      ├── Gabor
      │
      ├── Wavelet
      │
      └── GLRLM
```

---

# 21.79 C++ Interface

A clean interface:

```cpp id="b8s6qz"
struct TextureFeature
{
    std::string name;
    double value;
};
```

Extractor:

```cpp id="b4d9g4"
class ITextureExtractor
{
public:
    virtual ~ITextureExtractor() = default;

    virtual std::vector<TextureFeature> extract(
        const Image& image,
        const Mask& roi) = 0;
};
```

---

# 21.80 GLCM API

Conceptually:

```cpp id="2q1a1n"
struct GLCMParameters
{
    int distance;
    std::vector<int> angles;
    int grayLevels;
    bool symmetric;
};
```

Then:

```cpp id="9y2x1m"
class GLCMExtractor : public ITextureExtractor
{
public:
    std::vector<TextureFeature> extract(
        const Image& image,
        const Mask& roi) override;
};
```

---

# 21.81 QML Integration

Your medical viewer could expose:

```text id="4b5f2y"
Texture Analysis
 ├── ROI
 ├── Feature Family
 │    ├── First Order
 │    ├── GLCM
 │    ├── LBP
 │    ├── Gabor
 │    └── GLRLM
 │
 ├── Distance
 ├── Direction
 ├── Gray Levels
 └── Calculate
```

Then:

```text id="y4j9k2"
QML
 ↓
TextureController
 ↓
TextureEngine
 ↓
Feature Results
 ↓
Table / Graph / Report
```

---

# 21.82 Texture Heatmap

Instead of returning one value, you can generate a texture map.

Example:

```text id="6r9y1p"
CT
 ↓
Local GLCM
 ↓
Contrast map
 ↓
Heatmap
```

This can show where texture varies spatially.

---

# 21.83 Texture Segmentation Pipeline

```text id="s4w2p9"
Image
 ↓
Local texture features
 ↓
Feature maps
 ↓
Feature normalization
 ↓
Clustering
 ↓
Texture regions
 ↓
Morphological cleanup
```

This connects:

```text id="x6t1y0"
Chapter 20 → Features
Chapter 21 → Texture
Chapter 18 → Segmentation
Chapter 17 → Morphology
```

---

# 21.84 Texture + Segmentation

A powerful classical pipeline:

```text id="5d0c3j"
Medical Image
      ↓
Preprocessing
      ↓
Initial Segmentation
      ↓
ROI
      ↓
Texture Analysis
      ↓
Feature Map
      ↓
Refined Segmentation / Classification
```

Texture can therefore support segmentation rather than only classification.

---

# 21.85 Texture + Registration

Texture can also help registration.

For example:

```text id="7xq6rm"
Fixed
 ↓
Texture features

Moving
 ↓
Texture features

       ↓

Feature matching
       ↓
Registration
```

However, intensity-based registration remains a major approach in medical imaging.

---

# 21.86 Texture + AI

Modern systems may combine:

```text id="5a3p8g"
Raw image
+
Hand-crafted texture features
+
Deep learned features
```

However, deep-learning systems can often learn their own representations, so explicit handcrafted texture features are not always necessary.

---

# 21.87 Handcrafted vs Learned Features

### Handcrafted

```text id="t9x8j0"
Engineer defines:
GLCM
LBP
Gabor
etc.
```

### Learned

```text id="z1k6pm"
Neural network
 ↓
Learns features automatically
```

This distinction is important for later AI chapters.

---

# 21.88 Enterprise Radiomics Architecture

A robust architecture could be:

```text id="5w0x4d"
DICOM
 ↓
Image Volume
 ↓
Segmentation / ROI
 ↓
Preprocessing
 ├── Resampling
 ├── Intensity normalization
 └── Discretization
 ↓
Feature Extraction
 ├── First Order
 ├── Shape
 ├── GLCM
 ├── GLRLM
 ├── LBP
 ├── Wavelet
 └── Other
 ↓
Feature Validation
 ↓
Feature Vector
 ↓
ML / Statistical Analysis
 ↓
Report
```

---

# 21.89 Medical Device Consideration

For enterprise medical software, texture-analysis results should be traceable.

Record:

```text id="x7f3t8"
Patient/Study context
ROI
Algorithm version
Parameters
Voxel spacing
Preprocessing
Discretization
Feature definitions
Results
```

This makes results reproducible.

---

# 21.90 Important Reproducibility Principle

Suppose two systems calculate:

```text id="0p2h4g"
GLCM Contrast
```

but use:

```text id="e8y4j1"
different
distance
angle
gray-level bins
normalization
```

they may produce different values.

Therefore:

[
\boxed{
Feature\ name\ alone
\neq
complete\ feature\ definition
}
]

---

# 21.91 Chapter 21 Mental Model

```text id="3z2m8x"
                     TEXTURE
                        │
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
   FIRST ORDER      SECOND ORDER      TRANSFORM
        │               │                │
        ↓               ↓          ┌─────┴─────┐
 Mean/Variance        GLCM        Gabor      Wavelet
 Entropy              │
 Skewness       ┌─────┼─────┐
 Kurtosis       ↓     ↓     ↓
              Contrast Homogeneity
              Energy   Correlation
              Entropy  Dissimilarity
                   │
                   ↓
                  LBP
                   │
                   ↓
                 GLRLM
                   │
                   ↓
             Feature Vector
                   │
          ┌────────┴────────┐
          ↓                 ↓
      Classification      Radiomics
```

---

# 21.92 Key Formulas

### Mean

[
\boxed{
\mu=\frac{1}{N}\sum_i x_i
}
]

### Variance

[
\boxed{
\sigma^2=
\frac{1}{N}
\sum_i(x_i-\mu)^2
}
]

### GLCM Contrast

[
\boxed{
\sum_{i,j}(i-j)^2p(i,j)
}
]

### Dissimilarity

[
\boxed{
\sum_{i,j}|i-j|p(i,j)
}
]

### Homogeneity

[
\boxed{
\sum_{i,j}
\frac{p(i,j)}
{1+|i-j|}
}
]

### ASM

[
\boxed{
ASM=\sum_{i,j}p(i,j)^2
}
]

### Energy

[
\boxed{
Energy=\sqrt{ASM}
}
]

### GLCM Entropy

[
\boxed{
-\sum_{i,j}p(i,j)\log p(i,j)
}
]

### Circularity

[
\boxed{
C=\frac{4\pi A}{P^2}
}
]

---

# 21.93 Knowledge Check

### Fundamentals

1. What is texture?
2. What is the difference between intensity and texture?
3. What is a first-order statistic?
4. What is a second-order statistic?
5. Why can't first-order statistics describe spatial arrangement?

### First-order

6. What is mean?
7. What is variance?
8. What is standard deviation?
9. What is skewness?
10. What is kurtosis?
11. What is entropy?

### GLCM

12. What is GLCM?
13. Why are distance and direction important?
14. What is GLCM normalization?
15. What is contrast?
16. What is dissimilarity?
17. What is homogeneity?
18. What is ASM?
19. What is energy?
20. What is correlation?
21. What is GLCM entropy?

### Other methods

22. What is LBP?
23. How is an LBP binary pattern generated?
24. What are Gabor filters?
25. Why use multiple Gabor orientations?
26. Why use multiple Gabor frequencies?
27. What is a wavelet transform?
28. What do LL, LH, HL, HH represent?
29. What is GLRLM?
30. What is Short Run Emphasis?
31. What is Long Run Emphasis?

### Medical imaging

32. Why is texture useful for medical images?
33. Why is segmentation important for radiomics?
34. Why does voxel spacing matter?
35. Why does gray-level discretization matter?
36. What is the difference between 2D and 3D texture?
37. What is radiomics?
38. Why is reproducibility important?
39. What acquisition factors can affect texture features?
40. Why should texture features not automatically be treated as diagnostic biomarkers?

---

# 21.94 Practical Exercise — First Order

Given:

```text id="6m5d2z"
10 20 30
20 30 40
30 40 50
```

Calculate:

1. Mean
2. Minimum
3. Maximum
4. Range
5. Variance
6. Standard deviation

Then explain whether the region appears homogeneous or heterogeneous.

---

# 21.95 Practical Exercise — GLCM

Given:

```text id="v0x5t9"
1 1 2
1 2 2
2 2 3
```

Calculate the horizontal distance-1 co-occurrence counts.

Then calculate:

[
p(i,j)
]

by normalization.

From the normalized matrix, calculate:

* contrast
* dissimilarity
* homogeneity
* ASM

This is an excellent exercise because it forces you to understand what the GLCM actually represents instead of simply calling a library function.

---

# 21.96 Practical Exercise — LBP

For:

```text id="5b8m3y"
110 90 120
 95 100 105
 80 100 130
```

with center:

[
100
]

calculate the 8-bit LBP pattern.

Then explain what happens if every intensity is increased by 50:

```text id="9r2f7q"
160 140 170
145 150 155
130 150 180
```

The relative comparison with the center remains the same.

---

# 21.97 Medical Exercise — Radiomics

Design a radiomics pipeline for a tumor:

```text id="j5k1b4"
CT
 ↓
DICOM conversion
 ↓
Tumor segmentation
 ↓
ROI
 ↓
Resampling
 ↓
Intensity preprocessing
 ↓
Gray-level discretization
 ↓
First-order features
 ↓
Shape features
 ↓
GLCM
 ↓
GLRLM
 ↓
Feature vector
 ↓
Feature selection
 ↓
ML analysis
```

For every stage identify:

* input
* output
* parameters
* possible source of variability

---

# 21.98 Chapter 21 Complete

The central idea is:

[
\boxed{
Texture
=======

Spatial\ pattern\ of\ image\ intensities
}
]

The major progression is:

```text id="7w2q5z"
Individual pixels
      ↓
First-order statistics
      ↓
Pixel relationships
      ↓
GLCM / GLRLM
      ↓
Local patterns
      ↓
LBP
      ↓
Frequency/orientation
      ↓
Gabor / Wavelets
      ↓
Quantitative feature vector
      ↓
Radiomics / ML
```

The most important distinction:

[
\boxed{
First\text{-}order
\rightarrow
What\ intensities exist?
}
]

[
\boxed{
Second\text{-}order
\rightarrow
How\ do\ neighboring\ intensities\ relate?
}
]

And for your medical imaging architecture:

```text id="0k9c6q"
DICOM
 ↓
Image Volume
 ↓
Segmentation / ROI
 ↓
Preprocessing
 ↓
TextureEngine
 ├── First Order
 ├── GLCM
 ├── LBP
 ├── Gabor
 ├── Wavelet
 └── GLRLM
 ↓
Feature Vector
 ↓
Radiomics / ML
 ↓
Report / Visualization
```

**Chapter 21 complete.**

### Next, strictly according to your index:

# Chapter 22 — Image Color Processing

Topics:

* Color image fundamentals
* RGB
* HSV
* HSL
* YUV
* YCbCr
* Lab
* Color spaces
* Color conversion
* Color enhancement
* Color segmentation
* Color histograms
* Pseudocolor processing
* False-color medical visualization
* Window/level color mapping
* LUTs
* DICOM palette color images
* Medical image overlays
* Alpha blending
* Color management for enterprise viewers
