# Chapter 20 — Image Feature Extraction

We continue **strictly according to your index**.

Chapter 20 covers:

1. What is a feature?
2. Low-level vs high-level features
3. Point features
4. Edge features
5. Corner detection
6. Harris corner detector
7. Shi-Tomasi
8. FAST
9. Blob detection
10. LoG blobs
11. DoG
12. SIFT
13. SURF
14. ORB
15. BRIEF
16. Feature descriptors
17. Feature matching
18. Distance metrics
19. RANSAC
20. Feature-based registration
21. Medical-image feature extraction

---

# 20.1 What Is an Image Feature?

A **feature** is an identifiable characteristic of an image that can be used for analysis.

Examples:

```text
Image
 │
 ├── Edge
 ├── Corner
 ├── Blob
 ├── Texture
 ├── Contour
 └── Keypoint
```

A feature should ideally provide useful information about the structure or content of an image.

---

# 20.2 Why Do We Need Features?

Consider two images:

```text
Image A                 Image B

   ●                       ●
      ●                       ●
          ●                       ●
```

Instead of comparing every pixel, we can detect important points:

```text
A → {P1, P2, P3}

B → {Q1, Q2, Q3}
```

Then match:

```text
P1 ↔ Q1
P2 ↔ Q2
P3 ↔ Q3
```

This is particularly useful for:

* registration
* object recognition
* image matching
* tracking
* stitching
* motion estimation

---

# 20.3 Feature Extraction Pipeline

The basic pipeline is:

```text
Image
  ↓
Feature Detection
  ↓
Keypoints
  ↓
Feature Description
  ↓
Descriptors
  ↓
Feature Matching
  ↓
Geometric Verification
  ↓
Application
```

This distinction is very important:

[
\boxed{
Detection \neq Description \neq Matching
}
]

---

# 20.4 Feature Detector

A detector answers:

> **Where are the interesting points?**

Example:

```text
Image
 ↓
Harris
 ↓
Keypoint locations
```

Output:

```text
P1 = (100,80)
P2 = (220,140)
P3 = (310,200)
```

---

# 20.5 Feature Descriptor

A descriptor answers:

> **What does this feature look like?**

For each keypoint:

```text
Keypoint
   ↓
Local neighborhood
   ↓
Descriptor vector
```

Example:

```text
P1 → [0.12, 0.52, 0.81, ...]
```

or for binary descriptors:

```text
P1 → 101100101001...
```

---

# 20.6 Feature Matching

Matching asks:

> **Which feature in image A corresponds to which feature in image B?**

Example:

```text
A:
P1 P2 P3 P4

B:
Q1 Q2 Q3 Q4
```

Result:

```text
P1 → Q3
P2 → Q1
P3 → Q4
```

---

# 20.7 Low-Level Features

Low-level features are usually derived directly from image appearance.

Examples:

* intensity
* gradient
* edges
* corners
* blobs
* local texture

Pipeline:

```text
Pixels
 ↓
Low-level features
```

---

# 20.8 High-Level Features

High-level features represent more semantic concepts.

Examples:

```text
Organ
Tumor
Bone
Vessel
Lesion
```

These generally require more complex processing.

Modern AI systems can learn high-level representations automatically.

---

# 20.9 Low-Level vs High-Level

| Low-Level | High-Level           |
| --------- | -------------------- |
| Edge      | Organ                |
| Corner    | Tumor                |
| Blob      | Lesion               |
| Gradient  | Anatomical structure |
| Texture   | Object identity      |

Chapter 20 primarily focuses on **low-level/local features**.

---

# 20.10 Point Features

A point feature is a location with distinctive local image information.

Examples:

```text
Corner
Blob
Keypoint
Interest point
```

A point is generally represented as:

[
\boxed{
p=(x,y)
}
]

For 3D:

[
\boxed{
p=(x,y,z)
}
]

---

# 20.11 Why Corners Are Useful

A flat region:

```text
████████
████████
████████
```

doesn't provide much positional information.

An edge:

```text
████
████
----
----
```

has strong variation in one direction.

A corner:

```text
████
████
```

has strong variation in two directions.

Therefore corners can be highly distinctive.

---

# 20.12 Corner Detection

A corner is approximately a point where intensity changes significantly in multiple directions.

Conceptually:

```text
Flat:

→ little change
↑ little change


Edge:

→ strong change
↑ little change


Corner:

→ strong change
↑ strong change
```

---

# 20.13 Harris Corner Detector

The Harris detector is one of the classical corner detectors.

It examines intensity variation around a point.

The key mathematical object is the **structure tensor** / second-moment matrix:

[
\boxed{
M=
\begin{bmatrix}
\sum I_x^2 & \sum I_xI_y\
\sum I_xI_y & \sum I_y^2
\end{bmatrix}
}
]

where the sums are calculated over a local window, often with weighting.

---

# 20.14 Harris Intuition

The matrix:

[
M
]

captures how much intensity changes in different directions.

Its eigenvalues are:

[
\lambda_1,\lambda_2
]

Interpretation:

### Flat region

[
\lambda_1\approx0,\quad\lambda_2\approx0
]

### Edge

One large, one small:

[
\lambda_1\gg\lambda_2
]

### Corner

Both large:

[
\lambda_1\gg0,\quad\lambda_2\gg0
]

This is the fundamental intuition behind Harris detection.

---

# 20.15 Harris Response

The classical Harris response is:

[
\boxed{
R=\det(M)-k(\operatorname{trace}(M))^2
}
]

where (k) is a parameter.

Conceptually:

```text
R large positive
      ↓
   Corner

R negative
      ↓
     Edge

R near zero
      ↓
Flat region
```

Exact thresholding and sign interpretation depend on implementation details, but this is the standard conceptual model.

---

# 20.16 Shi-Tomasi

Shi-Tomasi is closely related to Harris.

Instead of using:

[
R=\det(M)-k(\operatorname{trace}(M))^2
]

it considers:

[
\boxed{
R=\min(\lambda_1,\lambda_2)
}
]

A point is a strong corner if the smaller eigenvalue is large.

---

# 20.17 Harris vs Shi-Tomasi

| Harris                          | Shi-Tomasi                                  |
| ------------------------------- | ------------------------------------------- |
| Uses determinant/trace response | Uses minimum eigenvalue                     |
| Classical corner detector       | Classical corner detector                   |
| Good corner detection           | Often strong for selecting trackable points |
| More parameterized response     | Direct eigenvalue interpretation            |

---

# 20.18 FAST

FAST means:

[
\boxed{
Features\ from\ Accelerated\ Segment\ Test
}
]

It was designed to detect corners very efficiently.

It examines pixels around a candidate pixel on a circular pattern.

Conceptually:

```text
      ● ● ●
    ●       ●
    ●   X   ●
    ●       ●
      ● ● ●
```

where (X) is the candidate.

---

# 20.19 FAST Intuition

If a sufficiently long contiguous segment of surrounding pixels is significantly brighter or darker than the center:

```text
Bright segment
      OR
Dark segment
```

the point can be classified as a corner.

FAST is popular when computational speed matters.

---

# 20.20 Harris vs Shi-Tomasi vs FAST

| Detector   | Main Idea        | Speed     |
| ---------- | ---------------- | --------- |
| Harris     | Structure tensor | Moderate  |
| Shi-Tomasi | Eigenvalues      | Moderate  |
| FAST       | Segment test     | Very fast |

---

# 20.21 Blob Detection

A blob is a region that differs from its surroundings.

Example:

```text
Background:

............

Blob:

....████....
...██████...
....████....
```

Blobs can represent:

* bright regions
* dark regions
* approximately circular structures
* local extrema

---

# 20.22 Why Blob Detection?

Blob detection can identify structures such as:

```text
Spot
Lesion-like region
Circular object
Local intensity structure
```

In medical imaging, blobs can sometimes be useful for detecting candidate structures, but a blob detector by itself does not establish clinical meaning.

---

# 20.23 LoG — Laplacian of Gaussian

LoG combines:

1. Gaussian smoothing
2. Laplacian

Conceptually:

```text
Image
 ↓
Gaussian blur
 ↓
Laplacian
 ↓
Blob response
```

Mathematically:

[
\boxed{
LoG=\nabla^2(G_\sigma * I)
}
]

where:

* (G_\sigma) = Gaussian
* (I) = image
* (\nabla^2) = Laplacian

---

# 20.24 Why Gaussian Before Laplacian?

The Laplacian is sensitive to noise.

Gaussian smoothing reduces high-frequency noise before applying the second derivative.

Therefore:

[
\boxed{
Smooth \rightarrow Differentiate
}
]

rather than directly differentiating a noisy image.

---

# 20.25 Multi-Scale Blob Detection

A blob can have different sizes.

Therefore LoG can be applied at multiple scales:

```text
σ small
 ↓
small blobs

σ medium
 ↓
medium blobs

σ large
 ↓
large blobs
```

This gives:

[
\boxed{
Scale-space\ blob\ detection
}
]

---

# 20.26 DoG — Difference of Gaussians

DoG approximates the Laplacian-of-Gaussian response.

Calculate:

[
\boxed{
DoG=
G_{\sigma_2}*I-G_{\sigma_1}*I
}
]

with:

[
\sigma_2>\sigma_1
]

Conceptually:

```text
Image
 ↓
Gaussian σ1
 ↓
Gaussian σ2
 ↓
Subtract
 ↓
DoG
```

---

# 20.27 Why DoG Is Important

DoG is computationally efficient compared with directly computing LoG at many scales.

It is particularly famous because it forms the scale-space detection stage of SIFT.

---

# 20.28 SIFT

SIFT means:

[
\boxed{
Scale\text{-}Invariant\ Feature\ Transform
}
]

SIFT is a classic feature detection and description method.

Its key goals include robustness to:

* scale changes
* rotation
* moderate illumination changes
* moderate viewpoint changes

---

# 20.29 SIFT Pipeline

Simplified:

```text
Image
 ↓
Scale-space
 ↓
DoG
 ↓
Keypoint detection
 ↓
Keypoint localization
 ↓
Orientation assignment
 ↓
Descriptor generation
```

---

# 20.30 SIFT Scale Space

SIFT creates multiple blurred versions:

```text
Original
 ↓
σ1
 ↓
σ2
 ↓
σ3
 ↓
...
```

Then computes differences between neighboring scales:

```text
Gσ1 - Gσ2
Gσ2 - Gσ3
...
```

These are DoG images.

---

# 20.31 SIFT Keypoint Detection

A candidate keypoint is detected as a local extremum in scale-space.

It is compared with neighboring points in:

* spatial dimensions
* scale dimension

This gives SIFT scale-aware keypoints.

---

# 20.32 SIFT Orientation

For each keypoint, a dominant local gradient orientation is estimated.

This allows the descriptor to be normalized relative to orientation.

Therefore:

```text
Image rotated
      ↓
Keypoint orientation changes
      ↓
Descriptor normalized
      ↓
More robust matching
```

---

# 20.33 SIFT Descriptor

SIFT commonly produces a descriptor with:

[
\boxed{
128
}
]

values.

It summarizes local gradient information around the keypoint.

Conceptually:

```text
Keypoint
   ↓
Local patch
   ↓
Gradient directions
   ↓
Orientation histograms
   ↓
128-dimensional descriptor
```

---

# 20.34 SIFT Descriptor Intuition

Suppose a local patch contains:

```text
Edge →
```

The descriptor captures:

```text
gradient magnitude
+
gradient direction
+
spatial distribution
```

rather than simply storing raw pixels.

---

# 20.35 SIFT Strengths

SIFT is well known for robustness to:

* scale changes
* rotation
* moderate illumination variation
* moderate viewpoint changes

It is computationally heavier than some newer binary descriptors.

---

# 20.36 SURF

SURF means:

[
\boxed{
Speeded\ Up\ Robust\ Features
}
]

It was designed as a faster alternative to SIFT using approximations and efficient filters.

SURF uses ideas involving:

* Hessian-based detection
* integral images
* local descriptors

---

# 20.37 SURF Concept

Simplified pipeline:

```text
Image
 ↓
Hessian-based keypoint detection
 ↓
Scale selection
 ↓
Orientation
 ↓
Descriptor
```

It aims to retain robustness while improving efficiency.

---

# 20.38 SIFT vs SURF

| SIFT                            | SURF                                |
| ------------------------------- | ----------------------------------- |
| DoG-based scale-space detection | Hessian-based detection             |
| 128-D descriptor                | Typically 64-D or extended variants |
| Robust                          | Robust                              |
| Computationally heavier         | Designed for speed                  |

In modern systems, licensing, implementation availability, performance, and project requirements all matter when choosing between classical methods.

---

# 20.39 BRIEF

BRIEF means:

[
\boxed{
Binary\ Robust\ Independent\ Elementary\ Features
}
]

It creates a binary descriptor by comparing pairs of pixels inside a local patch.

For example:

```text
Pixel A > Pixel B ?

Yes → 1
No  → 0
```

Repeat many times:

```text
101100101011001...
```

---

# 20.40 BRIEF Descriptor

Suppose we perform 256 binary comparisons:

[
\boxed{
descriptor=256\ bits
}
]

Each comparison is:

[
\tau(p,q)=
\begin{cases}
1,&I(p)<I(q)\
0,&otherwise
\end{cases}
]

depending on the chosen convention.

---

# 20.41 BRIEF Advantage

Binary descriptors are compact and fast.

Instead of:

```text
128 floating-point values
```

you may have:

```text
256 bits
```

This makes matching very efficient.

---

# 20.42 BRIEF Limitation

Basic BRIEF is not inherently rotation invariant.

If the image rotates:

```text
Original patch
      ↓
Rotated patch
      ↓
Binary tests change
```

and matching can degrade.

This motivates ORB.

---

# 20.43 ORB

ORB means:

[
\boxed{
Oriented\ FAST\ and\ Rotated\ BRIEF
}
]

ORB combines:

```text
FAST
+
orientation estimation
+
rotated BRIEF
```

It is designed to be:

* fast
* compact
* rotation-aware
* suitable for real-time applications

---

# 20.44 ORB Pipeline

```text
Image
 ↓
FAST keypoints
 ↓
Orientation estimation
 ↓
Rotated BRIEF
 ↓
Binary descriptors
```

---

# 20.45 ORB Descriptor

ORB typically uses a binary descriptor.

Therefore matching can use:

[
\boxed{
Hamming\ distance
}
]

which is very efficient.

---

# 20.46 SIFT vs ORB

| SIFT                           | ORB                       |
| ------------------------------ | ------------------------- |
| Floating-point descriptor      | Binary descriptor         |
| Larger descriptor              | Compact descriptor        |
| More computationally expensive | Fast                      |
| Strong robustness              | Good practical robustness |
| Euclidean-type matching        | Hamming matching          |

For a real-time application, ORB can be attractive.

---

# 20.47 BRIEF vs ORB

| BRIEF                      | ORB                       |
| -------------------------- | ------------------------- |
| Binary                     | Binary                    |
| Fast                       | Fast                      |
| Basic rotation sensitivity | Adds orientation handling |
| Simple                     | More sophisticated        |

---

# 20.48 Feature Descriptor

A descriptor is a numerical representation of a local image region.

Conceptually:

```text
Keypoint
   ↓
Patch
   ↓
Descriptor
```

Example:

```text
P1
 ↓
[0.12, 0.41, 0.88, ...]
```

or:

```text
P1
 ↓
101101001001...
```

---

# 20.49 Why Descriptors Are Needed

Coordinates alone aren't enough.

Suppose:

```text
Image A:

P1 = (100,100)

Image B:

Q1 = (250,180)
```

How do we know:

[
P1\leftrightarrow Q1?
]

We compare descriptors.

```text
Descriptor(P1)
      vs
Descriptor(Q1)
```

---

# 20.50 Descriptor Matching

Given:

```text
A:
D1 D2 D3

B:
E1 E2 E3 E4
```

Calculate distances:

```text
D1-E1
D1-E2
D1-E3
D1-E4
```

Choose the best candidate.

---

# 20.51 Distance Metrics

Different descriptors require different distance metrics.

### Floating-point descriptors

Often:

[
\boxed{
Euclidean\ distance
}
]

### Binary descriptors

Usually:

[
\boxed{
Hamming\ distance
}
]

---

# 20.52 Euclidean Distance

For vectors:

[
a=(a_1,a_2,\ldots,a_n)
]

and:

[
b=(b_1,b_2,\ldots,b_n)
]

Euclidean distance:

[
\boxed{
d(a,b)=
\sqrt{
\sum_i(a_i-b_i)^2
}
}
]

Smaller means more similar.

---

# 20.53 Hamming Distance

For binary strings:

```text
A = 10110100
B = 10011100
```

Count differing bits.

```text
10110100
10011100
  ↑ ↑
```

If two positions differ:

[
\boxed{
Hamming=2
}
]

Smaller means more similar.

---

# 20.54 Descriptor Matching Pipeline

```text
Image A
 ↓
Keypoints
 ↓
Descriptors
       \
        \
         → Matcher
        /
       /
Image B
 ↓
Keypoints
 ↓
Descriptors
```

Output:

```text
Correspondence pairs
```

---

# 20.55 Nearest-Neighbor Matching

For each descriptor in image A:

```text
Find closest descriptor in B
```

Example:

```text
D1 → E3
D2 → E1
D3 → E4
```

Simple, but it can produce false matches.

---

# 20.56 Ratio Test

A common approach for floating-point descriptors is to compare:

* best match distance
* second-best match distance

Suppose:

[
d_1=\text{best}
]

[
d_2=\text{second best}
]

Accept if:

[
\boxed{
\frac{d_1}{d_2}<r
}
]

for a chosen threshold (r).

The exact threshold is application-dependent.

---

# 20.57 Why Ratio Test Works

Suppose:

```text
Best match = very good
Second best = much worse
```

Then:

[
d_1\ll d_2
]

so:

[
\frac{d_1}{d_2}
]

is small.

This suggests a distinctive match.

But if:

```text
Best = 10
Second = 11
```

then the feature is ambiguous.

---

# 20.58 Cross-Check Matching

Another technique:

```text
A → B
```

and:

```text
B → A
```

Accept only if they agree.

This is called:

[
\boxed{
Mutual\ /\ cross\ checking
}
]

It can reduce asymmetric false matches.

---

# 20.59 RANSAC

RANSAC means:

[
\boxed{
Random\ Sample\ Consensus
}
]

It is extremely important for feature-based registration.

Why?

Because feature matching can contain:

```text
Correct matches
+
Wrong matches
```

RANSAC attempts to estimate a geometric model while rejecting outliers.

---

# 20.60 RANSAC Example

Suppose:

```text
100 matches
```

but:

```text
70 correct
30 incorrect
```

A direct transformation fit may be badly affected.

RANSAC tries to find:

```text
Transformation
 ↓
maximum consistent subset
```

---

# 20.61 RANSAC Algorithm

Simplified:

```text
1. Randomly select minimal sample
2. Estimate model
3. Calculate errors
4. Count inliers
5. Repeat
6. Keep model with strongest consensus
```

---

# 20.62 RANSAC Example — Affine

Suppose we want an affine transformation.

We randomly choose enough point correspondences to estimate it.

Then:

```text
Estimate affine T
      ↓
Transform all points
      ↓
Measure error
      ↓
Inlier / Outlier
```

Example:

```text
● ● ● ● ● ● ●
✓ ✓ ✓ ✓ ✓ ✗ ✗
```

RANSAC attempts to find the transformation explaining the majority of the consistent points.

---

# 20.63 Inlier and Outlier

### Inlier

A correspondence consistent with the estimated transformation.

```text
A → B
 ↓
fits model
```

### Outlier

A correspondence that does not fit.

```text
A → wrong B
 ↓
large geometric error
```

---

# 20.64 RANSAC Importance in Registration

Feature matching:

```text
Keypoints
 ↓
Descriptors
 ↓
Matches
 ↓
Many possible outliers
 ↓
RANSAC
 ↓
Reliable transformation
```

This is the classic feature-based registration pipeline.

---

# 20.65 Feature-Based Registration

Now combine everything:

```text
Fixed Image
     ↓
Feature Detection
     ↓
Keypoints
     ↓
Descriptors
     │
     │
     │ Matching
     │
     ↓
Moving Image
     ↑
Feature Detection
     ↑
Keypoints
     ↑
Descriptors
     ↓
Matches
     ↓
RANSAC
     ↓
Transformation
     ↓
Registration
```

---

# 20.66 Feature-Based vs Intensity-Based Registration

| Feature-Based                        | Intensity-Based                       |
| ------------------------------------ | ------------------------------------- |
| Uses keypoints/features              | Uses image intensities                |
| Matching required                    | No explicit feature matching required |
| Can be fast after feature extraction | Can be computationally expensive      |
| RANSAC often useful                  | Optimizer/metric central              |
| Sensitive to feature quality         | Sensitive to metric/initialization    |

---

# 20.67 When Feature-Based Registration Works Well

Good when images contain:

* distinctive structures
* corners
* edges
* local texture
* repeated recognizable landmarks

For example:

```text
Image
 ↓
Distinct anatomical landmarks
 ↓
Feature matching
```

---

# 20.68 When Feature-Based Registration Can Fail

Medical images can be difficult because:

```text
Low texture
+
Smooth anatomy
+
Low contrast
+
Repetitive structures
```

may provide few distinctive local features.

For example, a relatively homogeneous organ may not contain enough stable corners for reliable feature-based registration.

---

# 20.69 Medical Image Feature Extraction

Medical images can have very different characteristics from natural images.

Examples:

### CT

```text
Bone
Edges
Fracture
Lesion
```

### MRI

```text
Texture
Tissue boundaries
Lesions
```

### X-ray

```text
Bone contours
Anatomical landmarks
```

### Ultrasound

```text
Texture
Speckle
Boundaries
```

Therefore feature selection must consider modality.

---

# 20.70 Medical Feature Types

Useful feature categories:

```text
Geometric
 ├── Points
 ├── Corners
 ├── Contours
 └── Landmarks

Intensity
 ├── Mean
 ├── Variance
 └── Histogram

Texture
 ├── GLCM
 ├── Local Binary Pattern
 └── Wavelets

Shape
 ├── Area
 ├── Perimeter
 ├── Circularity
 └── Curvature
```

Not all of these are local keypoint features, but they are important feature-engineering concepts in medical imaging.

---

# 20.71 Medical Feature Example — Lesion

Suppose a lesion candidate is detected.

Extract:

```text
Shape
 ├── Area
 ├── Perimeter
 ├── Circularity

Intensity
 ├── Mean
 ├── Min
 ├── Max
 └── Variance

Texture
 ├── Contrast
 ├── Homogeneity
 └── Entropy
```

These can be used for downstream analysis or classification.

---

# 20.72 Shape Features

For a segmented object:

### Area

[
\boxed{
A=\text{number of pixels}
}
]

or physical area after accounting for pixel spacing.

### Perimeter

Length of the boundary.

### Circularity

[
\boxed{
C=\frac{4\pi A}{P^2}
}
]

where:

* (A) = area
* (P) = perimeter

A perfect circle has:

[
C=1
]

approximately, depending on discretization.

---

# 20.73 Why Physical Units Matter

Suppose:

```text
Pixel area = 100 pixels
```

This isn't necessarily:

```text
100 mm²
```

If pixel spacing is:

[
0.5\times0.5\text{ mm}
]

then:

[
1\text{ pixel}=0.25\text{ mm}^2
]

Therefore:

[
100\text{ pixels}=25\text{ mm}^2
]

This is extremely important for medical measurements.

---

# 20.74 Texture Features

Texture describes spatial intensity patterns.

A common method is the **Gray-Level Co-occurrence Matrix (GLCM)**.

It captures how often pairs of gray levels occur with a specified:

* distance
* direction

---

# 20.75 GLCM Concept

Suppose:

```text
Pixel A = 50
Neighbor = 60
```

Count:

```text
50 → 60
```

for all specified neighboring pairs.

This creates:

[
\boxed{
GLCM
}
]

From it, features can be calculated.

---

# 20.76 Common GLCM Features

Examples:

### Contrast

Measures local intensity variation.

### Homogeneity

Measures closeness of neighboring values.

### Energy

Measures uniformity.

### Correlation

Measures relationships between neighboring intensities.

These can be useful in radiomics-style analysis.

---

# 20.77 Feature Extraction vs Radiomics

Feature extraction is broader.

Radiomics generally refers to systematic extraction of quantitative image features from medical images.

Pipeline:

```text
Medical Image
 ↓
ROI / Segmentation
 ↓
Preprocessing
 ↓
Feature Extraction
 ↓
Quantitative Features
 ↓
Analysis / Model
```

---

# 20.78 Feature Extraction Architecture

For your medical imaging application:

```text
FeatureEngine
     │
     ├── Keypoints
     │    ├── Harris
     │    ├── ShiTomasi
     │    ├── FAST
     │    └── ORB/SIFT
     │
     ├── Descriptors
     │    ├── SIFT
     │    ├── BRIEF
     │    └── ORB
     │
     ├── Shape
     │
     ├── Intensity
     │
     ├── Texture
     │
     └── Statistical
```

---

# 20.79 C++ Interface

A clean architecture could use:

```cpp
class IFeatureDetector
{
public:
    virtual ~IFeatureDetector() = default;

    virtual std::vector<KeyPoint> detect(
        const Image& image) = 0;
};
```

Descriptor:

```cpp
class IFeatureDescriptor
{
public:
    virtual ~IFeatureDescriptor() = default;

    virtual DescriptorSet compute(
        const Image& image,
        const std::vector<KeyPoint>& keypoints) = 0;
};
```

Matcher:

```cpp
class IFeatureMatcher
{
public:
    virtual ~IFeatureMatcher() = default;

    virtual std::vector<Match> match(
        const DescriptorSet& a,
        const DescriptorSet& b) = 0;
};
```

---

# 20.80 RANSAC Interface

```cpp
class IGeometricEstimator
{
public:
    virtual ~IGeometricEstimator() = default;

    virtual Transform estimate(
        const std::vector<Match>& matches) = 0;
};
```

Implementation could use:

```text
AffineEstimator
RigidEstimator
HomographyEstimator
```

with RANSAC as the robust estimation strategy.

---

# 20.81 OpenCV Role

For your architecture, **OpenCV** is particularly useful for classical 2D feature processing.

Conceptually:

```text
Image
 ↓
OpenCV
 ├── Harris
 ├── FAST
 ├── ORB
 ├── SIFT
 ├── descriptors
 ├── matching
 └── geometric estimation
```

For medical volumetric processing and registration, ITK may be more appropriate depending on the operation and data dimensionality.

---

# 20.82 ITK vs OpenCV for Features

A practical division can be:

```text
OpenCV
 ↓
2D computer vision
Feature detection
Feature matching
```

```text
ITK
 ↓
Medical image processing
3D volumes
Spatial metadata
Registration
```

But this is not an absolute rule. Choose based on the specific algorithm and data model.

---

# 20.83 QML Integration

Your QML viewer could expose:

```text
Feature Tool
 ├── Detector
 │    ├── Harris
 │    ├── FAST
 │    └── ORB
 │
 ├── Descriptor
 │    ├── ORB
 │    └── SIFT
 │
 ├── Match
 │
 └── Show Keypoints
```

Architecture:

```text
QML
 ↓
FeatureController
 ↓
FeatureEngine
 ↓
OpenCV / ITK
 ↓
FeatureResult
 ↓
QML Overlay
```

---

# 20.84 Feature Overlay

For debugging:

```text
Original Image
      +
Keypoints
      ↓
Visualization
```

Example:

```text
       ×
  ×
          ×
     ×
```

This is extremely useful when validating detector behavior.

---

# 20.85 Feature Matching Visualization

Two images:

```text
Fixed                    Moving

  ×───────×                ×
     \                     │
      \                    │
       ×───────────────────×
```

Lines indicate matched features.

Then RANSAC can classify:

```text
Correct matches → keep
Outliers         → reject
```

---

# 20.86 Feature Pipeline for Registration

A production-oriented pipeline:

```text
Fixed Image
   ↓
Preprocessing
   ↓
Feature Detection
   ↓
Feature Description
   ↓
        ┌───────────────┐
        │               │
        ↓               ↓
     Keypoints       Descriptors
        │               │
        └───────┬───────┘
                ↓
             Matching
                ↓
             Filtering
                ↓
             RANSAC
                ↓
        Transformation
                ↓
          Registration
                ↓
           Validation
```

---

# 20.87 Important Difference: Keypoint vs Descriptor

This is an interview favorite.

### Keypoint

```text
WHERE?
```

Example:

[
(x,y,\sigma,\theta)
]

### Descriptor

```text
WHAT DOES IT LOOK LIKE?
```

Example:

```text
[0.12, 0.52, ...]
```

Therefore:

[
\boxed{
Keypoint = location + properties
}
]

[
\boxed{
Descriptor = local appearance representation
}
]

---

# 20.88 Important Difference: Detector vs Descriptor

For example:

```text
FAST
```

is primarily a detector.

```text
BRIEF
```

is primarily a descriptor.

```text
ORB
```

combines an oriented FAST detector with a rotated BRIEF-style descriptor.

This distinction is very important.

---

# 20.89 SIFT Pipeline Memory Trick

Remember:

```text
SIFT

Scale
 ↓
DoG
 ↓
Keypoint
 ↓
Orientation
 ↓
Descriptor
```

---

# 20.90 ORB Memory Trick

Remember:

```text
ORB

O = Oriented
R = FAST
B = BRIEF
```

Conceptually:

[
\boxed{
ORB = Oriented\ FAST + Rotated\ BRIEF
}
]

---

# 20.91 RANSAC Memory Trick

Remember:

```text
Matches
 ↓
Random sample
 ↓
Model
 ↓
Count inliers
 ↓
Repeat
 ↓
Best consensus
```

Therefore:

[
\boxed{
RANSAC = robust\ model\ estimation
}
]

---

# 20.92 Medical Imaging Caution

Natural-image feature methods do not automatically transfer perfectly to medical images.

For example:

```text
Natural image:
many corners + textures

Medical image:
large homogeneous regions
+
low contrast
+
modality-specific intensity
```

Therefore:

[
\boxed{
Algorithm\ selection
must\ consider\ modality,\ anatomy,\ resolution,\ and\ task.
}
]

---

# 20.93 Chapter 20 Mental Model

```text
                     FEATURE EXTRACTION
                            │
            ┌───────────────┼────────────────┐
            ↓               ↓                ↓
         POINTS           EDGES            BLOBS
            │               │                │
      ┌─────┼─────┐         │          ┌─────┴─────┐
      ↓     ↓     ↓         │          ↓           ↓
    Harris Shi  FAST       Edge       LoG         DoG
   Tomasi                   │
      │                     │
      └──────────┬──────────┘
                 ↓
             KEYPOINTS
                 ↓
            DESCRIPTORS
          ┌──────┼──────┐
          ↓      ↓      ↓
        SIFT    BRIEF   ORB
          │      │      │
          └──────┼──────┘
                 ↓
              MATCHING
                 ↓
          Distance Metric
          ┌──────┴──────┐
          ↓             ↓
       Euclidean      Hamming
          │             │
          └──────┬──────┘
                 ↓
              RANSAC
                 ↓
        Feature Registration
```

---

# 20.94 Key Concepts to Memorize

[
\boxed{
Feature = useful image characteristic
}
]

[
\boxed{
Keypoint = interesting location
}
]

[
\boxed{
Descriptor = representation of local appearance
}
]

[
\boxed{
Matcher = finds corresponding descriptors
}
]

[
\boxed{
RANSAC = rejects geometric outliers
}
]

---

# 20.95 Knowledge Check

### Fundamentals

1. What is an image feature?
2. What is a low-level feature?
3. What is a high-level feature?
4. What is a keypoint?
5. What is a descriptor?
6. What is the difference between detection and description?

### Corners

7. What is a corner?
8. What does Harris detect?
9. What is the Harris structure tensor?
10. What do its eigenvalues represent?
11. What is the Harris response equation?
12. How does Shi-Tomasi differ from Harris?
13. What is FAST?

### Blobs

14. What is a blob?
15. What is LoG?
16. Why is Gaussian smoothing used before the Laplacian?
17. What is scale-space?
18. What is DoG?
19. Why is DoG related to SIFT?

### Descriptors

20. What is SIFT?
21. Why is SIFT relatively robust to scale and rotation?
22. What is SURF?
23. What is BRIEF?
24. Why is BRIEF fast?
25. What is ORB?
26. Why is ORB suitable for fast applications?

### Matching

27. What is feature matching?
28. What is Euclidean distance?
29. What is Hamming distance?
30. Which distance is appropriate for binary descriptors?
31. What is the ratio test?
32. What is cross-check matching?

### RANSAC

33. What is RANSAC?
34. What is an inlier?
35. What is an outlier?
36. Why is RANSAC useful for registration?
37. What happens if most feature matches are incorrect?

### Medical imaging

38. Why can natural-image features perform poorly on medical images?
39. Why is modality important?
40. Why should physical spacing be considered for medical measurements?
41. How can features be used for lesion analysis?
42. How can features be used for registration?
43. What is the difference between feature-based and intensity-based registration?

---

# 20.96 Practical Exercise — Harris

Consider three regions:

```text
A:
████████
████████
████████

B:
████
████
----
----

C:
████
████
```

Classify each approximately as:

```text
Flat
Edge
Corner
```

Then explain using:

[
\lambda_1,\lambda_2
]

---

# 20.97 Practical Exercise — Descriptor Matching

Given:

```text
A = 10110100
B = 10100100
C = 00110110
```

Calculate the Hamming distance:

```text
A ↔ B
A ↔ C
```

Which is the better binary match?

---

# 20.98 Practical Exercise — RANSAC

Suppose feature matching gives:

```text
50 matches
```

and:

```text
35 are correct
15 are incorrect
```

Explain how RANSAC can estimate the transformation despite the 15 incorrect correspondences.

Then explain what happens if:

```text
10 correct
40 incorrect
```

and why RANSAC becomes much less reliable.

---

# 20.99 Medical Feature Exercise

You have a segmented lesion.

Build a feature vector containing:

```text
Shape
 ├── Area
 ├── Perimeter
 └── Circularity

Intensity
 ├── Mean
 ├── Standard deviation
 └── Maximum

Texture
 ├── Contrast
 ├── Homogeneity
 └── Energy
```

Then explain how these features could be used by a downstream classification model.

---

# 20.100 Chapter 20 Complete

The complete feature-processing pipeline is:

[
\boxed{
Image
\rightarrow
Detection
\rightarrow
Keypoints
\rightarrow
Description
\rightarrow
Matching
\rightarrow
RANSAC
\rightarrow
Geometric\ Model
}
]

The most important distinction:

[
\boxed{
Keypoint = WHERE
}
]

[
\boxed{
Descriptor = WHAT
}
]

[
\boxed{
Matcher = WHICH\ ONE
}
]

[
\boxed{
RANSAC = WHICH\ MATCHES\ ARE\ GEOMETRICALLY\ CONSISTENT
}
]

And for your medical imaging architecture:

```text
DICOM
 ↓
ITK / Image representation
 ↓
Preprocessing
 ↓
FeatureEngine
 ├── Detection
 ├── Description
 └── Matching
 ↓
RANSAC
 ↓
Registration / Analysis
 ↓
VTK / QML Visualization
```

**Chapter 20 complete.**

### Next, strictly according to your index:

# Chapter 21 — Image Texture Analysis

Topics:

* What is texture?
* Texture vs intensity
* Statistical texture
* First-order statistics
* Second-order statistics
* GLCM
* Contrast
* Dissimilarity
* Homogeneity
* Energy
* ASM
* Correlation
* Entropy
* Local Binary Patterns
* Gabor filters
* Wavelet-based texture
* Run-Length Matrix
* Texture segmentation
* Medical texture analysis
* Radiomics fundamentals
