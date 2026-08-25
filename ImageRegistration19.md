# Chapter 19 — Image Registration

We continue **strictly according to your index**.

Image registration is one of the most important topics in medical imaging because the same anatomy can be acquired:

* at different times
* with different modalities
* in different positions
* with different scanners
* with different patient motion
* at different phases of treatment

The goal is to bring images into a common spatial coordinate system.

---

# 19.1 What Is Image Registration?

Image registration is the process of finding a spatial transformation that aligns one image with another.

Conceptually:

```text
Fixed Image                 Moving Image
     │                           │
     │                           ↓
     │                      Transformation
     │                           │
     └──────────────┬────────────┘
                    ↓
                 Aligned
```

We usually call:

* **Fixed image** → reference
* **Moving image** → image that is transformed

Mathematically:

[
\boxed{
I_F(x)=I_M(T(x))
}
]

Conceptually, we seek:

[
\boxed{
T^*=\arg\max_T S(I_F,I_M\circ T)
}
]

where:

* (I_F) = fixed image
* (I_M) = moving image
* (T) = transformation
* (S) = similarity measure

---

# 19.2 Why Registration Is Needed

Imagine two CT scans:

```text
Scan 1

     ❤️
    /  \
   /    \
```

and:

```text
Scan 2

       ❤️
      /  \
     /    \
```

The patient moved slightly.

Registration tries to calculate:

```text
Scan 2
  ↓
Transformation
  ↓
Aligned with Scan 1
```

---

# 19.3 Fixed vs Moving Image

This terminology is extremely important.

### Fixed

The reference coordinate system.

```text
Fixed
 ↓
Do not move
```

### Moving

The image being transformed.

```text
Moving
 ↓
Apply T
 ↓
Aligned
```

Example:

```text
CT 2025 → Fixed
CT 2026 → Moving
```

The algorithm finds the transformation required to align the 2026 image to the 2025 coordinate system.

---

# 19.4 Registration Pipeline

A typical registration pipeline:

```text
Fixed Image
      │
      │
Moving Image
      │
      ↓
Preprocessing
      ↓
Choose Transformation
      ↓
Choose Similarity Metric
      ↓
Optimization
      ↓
Resampling
      ↓
Registered Image
      ↓
Validation
```

There are four major components:

[
\boxed{
Transformation
+
Metric
+
Optimizer
+
Interpolator
}
]

---

# 19.5 Transformation

Transformation answers:

> How can the moving image be spatially changed?

Examples:

```text
Translation
Rotation
Scaling
Shearing
Deformation
```

---

# 19.6 Translation

Translation moves an image without changing its shape.

For 2D:

[
x'=x+t_x
]

[
y'=y+t_y
]

Matrix form using homogeneous coordinates:

[
\boxed{
\begin{bmatrix}
x'\
y'\
1
\end{bmatrix}
=============

\begin{bmatrix}
1&0&t_x\
0&1&t_y\
0&0&1
\end{bmatrix}
\begin{bmatrix}
x\
y\
1
\end{bmatrix}
}
]

---

# 19.7 Translation Example

Original:

```text
   ███
   ███
```

Move right:

```text
      ███
      ███
```

Move down:

```text
     
   ███
   ███
```

The shape remains unchanged.

---

# 19.8 Rotation

Rotation changes orientation.

For angle (\theta):

[
\boxed{
R=
\begin{bmatrix}
\cos\theta&-\sin\theta\
\sin\theta&\cos\theta
\end{bmatrix}
}
]

Therefore:

[
\begin{bmatrix}
x'\
y'
\end{bmatrix}
=============

R
\begin{bmatrix}
x\
y
\end{bmatrix}
]

---

# 19.9 Rotation Around a Point

Usually, an image should rotate around a center rather than the origin.

Conceptually:

```text
Translate center to origin
        ↓
Rotate
        ↓
Translate back
```

Mathematically:

[
\boxed{
T_{center}^{-1}RT_{center}
}
]

depending on coordinate-transform convention.

---

# 19.10 Scaling

Scaling changes size.

[
\boxed{
x'=s_xx
}
]

[
\boxed{
y'=s_yy
}
]

Matrix:

[
\begin{bmatrix}
s_x&0\
0&s_y
\end{bmatrix}
]

If:

[
s_x=s_y
]

we have uniform scaling.

If:

[
s_x\ne s_y
]

we have anisotropic scaling.

---

# 19.11 Shearing

Shearing changes the shape by sliding coordinates.

Example:

[
\boxed{
x'=x+k_xy
}
]

[
y'=y+k_yx
]

Matrix:

[
\begin{bmatrix}
1&k_x\
k_y&1
\end{bmatrix}
]

Shearing can model certain geometric distortions.

---

# 19.12 Transformation Categories

Your index divides registration into:

```text
Registration
     │
     ├── Rigid
     │
     ├── Affine
     │
     └── Deformable
```

This hierarchy is extremely important.

---

# 19.13 Rigid Registration

Rigid transformation preserves:

* distances
* angles
* shape

It usually consists of:

```text
Translation
+
Rotation
```

In 2D:

[
\boxed{
T(x)=Rx+t
}
]

---

# 19.14 Rigid Example

Before:

```text
   ███
   ███
```

After rotation + translation:

```text
      ███
       ██
```

The object's physical shape is unchanged.

Only its position/orientation changes.

---

# 19.15 When Use Rigid Registration?

Useful when anatomy behaves approximately like a rigid body.

Examples:

* bony structures
* head imaging under limited deformation
* certain intra-session scans
* image alignment where patient motion dominates

But soft tissue generally does not behave as a perfectly rigid body.

---

# 19.16 Affine Registration

Affine transformation includes:

* translation
* rotation
* scaling
* shearing

General 2D form:

[
\boxed{
\begin{bmatrix}
x'\
y'
\end{bmatrix}
=============

A
\begin{bmatrix}
x\
y
\end{bmatrix}
+t
}
]

where:

[
A=
\begin{bmatrix}
a&b\
c&d
\end{bmatrix}
]

---

# 19.17 Affine Matrix

Using homogeneous coordinates:

[
\boxed{
\begin{bmatrix}
x'\
y'\
1
\end{bmatrix}
=============

\begin{bmatrix}
a&b&t_x\
c&d&t_y\
0&0&1
\end{bmatrix}
\begin{bmatrix}
x\
y\
1
\end{bmatrix}
}
]

Affine transformations preserve straight lines and parallelism, but not necessarily distances or angles.

---

# 19.18 Rigid vs Affine

| Property         | Rigid | Affine     |
| ---------------- | ----- | ---------- |
| Translation      | Yes   | Yes        |
| Rotation         | Yes   | Yes        |
| Scaling          | No    | Yes        |
| Shearing         | No    | Yes        |
| Shape preserved  | Yes   | Can change |
| Parameters in 2D | 3     | 6          |

---

# 19.19 Deformable Registration

Rigid and affine assume a global transformation.

But anatomy can actually deform.

For example:

```text
Before:

████████
████████

After:

 ███████
█████████
```

The transformation varies spatially.

Therefore:

[
\boxed{
T=T(x,y)
}
]

rather than a single global transform.

---

# 19.20 Why Deformable Registration?

Useful when structures change due to:

* breathing
* organ motion
* soft-tissue deformation
* patient positioning
* growth
* treatment response
* anatomical changes

---

# 19.21 Deformable Registration Mental Model

```text
Fixed
████████

Moving
  ███████
 ████████

        ↓

Local deformation field

        ↓

Registered
████████
████████
```

Instead of moving the entire image uniformly, different regions move differently.

---

# 19.22 Deformation Field

A displacement field can be represented as:

[
\boxed{
u(x,y)=
\begin{bmatrix}
u_x(x,y)\
u_y(x,y)
\end{bmatrix}
}
]

Then:

[
\boxed{
T(x,y)=
\begin{bmatrix}
x+u_x(x,y)\
y+u_y(x,y)
\end{bmatrix}
}
]

For 3D:

[
u(x,y,z)=
\begin{bmatrix}
u_x\
u_y\
u_z
\end{bmatrix}
]

---

# 19.23 Transformation Hierarchy

Memorize:

```text
Rigid
 ↓
Global position/orientation

Affine
 ↓
Rigid + scale + shear

Deformable
 ↓
Local spatial deformation
```

Generally:

[
\boxed{
Rigid \subset Affine \subset Deformable
}
]

conceptually, though specific deformable models can have their own constraints and parameterizations.

---

# 19.24 Registration Is an Optimization Problem

This is one of the most important concepts.

We have:

```text
Fixed image
+
Moving image
+
Transformation parameters
```

We need to find the transformation that gives the best alignment.

So:

[
\boxed{
Registration
============

Optimization
}
]

---

# 19.25 Similarity Measure

A similarity measure tells us:

> How well are the two images aligned?

Examples:

* SSD
* NCC
* Mutual Information

---

# 19.26 SSD — Sum of Squared Differences

For images (F) and (M):

[
\boxed{
SSD=
\sum_x
[F(x)-M(T(x))]^2
}
]

Lower is better.

[
\boxed{
\min SSD
}
]

This works best when the image intensities have a similar meaning.

---

# 19.27 SSD Example

Fixed:

```text
100 120 150
```

Moving:

```text
105 118 152
```

Differences:

```text
-5  2  -2
```

Squares:

```text
25  4  4
```

Therefore:

[
SSD=33
]

A lower SSD means the intensity patterns are more similar under that transformation.

---

# 19.28 NCC — Normalized Cross-Correlation

Normalized cross-correlation measures correlation between intensity patterns.

A common form:

[
\boxed{
NCC=
\frac{
\sum(F-\bar F)(M-\bar M)
}{
\sqrt{
\sum(F-\bar F)^2
\sum(M-\bar M)^2
}
}
}
]

Higher is generally better.

[
\boxed{
\max NCC
}
]

---

# 19.29 Why NCC Helps

Suppose one image is approximately a scaled/offset version of another:

```text
Image A:
10 20 30

Image B:
100 200 300
```

SSD sees large intensity differences.

Correlation recognizes that the pattern is strongly related.

Therefore NCC can be more robust to certain intensity scaling/offset differences than raw SSD.

---

# 19.30 SSD vs NCC

| Metric | Optimization | Intensity relationship       |
| ------ | ------------ | ---------------------------- |
| SSD    | Minimize     | Similar absolute intensities |
| NCC    | Maximize     | Similar normalized patterns  |

---

# 19.31 Mutual Information

Mutual Information is particularly important in **multi-modal registration**.

It measures statistical dependence between image intensities.

Conceptually:

```text
CT intensity
      +
MRI intensity
      ↓
Joint histogram
      ↓
Mutual information
```

Higher mutual information means stronger statistical dependence.

---

# 19.32 Why Mutual Information?

CT and MRI intensities do not represent the same physical quantity.

For example:

```text
CT
 ↓
X-ray attenuation / HU-related values
```

while:

```text
MRI
 ↓
sequence-dependent signal intensity
```

Therefore:

[
\boxed{
SSD\ is\ generally\ not\ the\ natural\ choice\ for\ CT\text{-}MRI
}
]

Mutual information is commonly used because it does not require corresponding structures to have the same raw intensity.

---

# 19.33 Mutual Information Mental Model

Suppose:

```text
CT:

Bone → bright
Soft tissue → medium


MRI:

Bone → dark
Soft tissue → different contrast
```

The intensities differ.

But anatomical relationships remain correlated.

Mutual information attempts to capture those statistical relationships.

---

# 19.34 Feature-Based Registration

Feature-based registration does not necessarily compare every pixel directly.

Instead:

```text
Image
 ↓
Detect features
 ↓
Match features
 ↓
Estimate transformation
```

Features can include:

* points
* corners
* edges
* contours
* anatomical landmarks

---

# 19.35 Example Feature Registration

Suppose:

```text id="0m6r1c"
Fixed:
A •
B    •
C       •

Moving:
A' •
B'   •
C'      •
```

Match:

```text
A ↔ A'
B ↔ B'
C ↔ C'
```

Then estimate transformation from these correspondences.

---

# 19.36 Landmark-Based Registration

A medical example:

```text
Fixed CT:
landmarks
  ●
      ●
          ●

Moving CT:
  ●
        ●
             ●
```

Corresponding landmarks can be used to estimate transformation.

This can be:

* manual
* semi-automatic
* automatic

---

# 19.37 Intensity-Based Registration

Intensity-based registration uses image intensities directly.

Pipeline:

```text
Fixed
+
Moving
 ↓
Metric
 ↓
Optimizer
 ↓
Transformation
```

No explicit landmark matching is necessarily required.

---

# 19.38 Feature vs Intensity-Based

| Property                   | Feature-based       | Intensity-based |
| -------------------------- | ------------------- | --------------- |
| Uses pixels directly       | Usually no          | Yes             |
| Uses landmarks/features    | Yes                 | Not necessarily |
| Requires feature detection | Yes                 | No              |
| Can handle multi-modal     | Depends on features | MI often useful |
| Computational cost         | Can be lower        | Can be high     |

---

# 19.39 Optimization

Once we define:

```text
Transformation
+
Metric
```

we need to find the best parameters.

Example rigid 2D parameters:

[
\theta,t_x,t_y
]

We seek:

[
\boxed{
\theta^*,t_x^*,t_y^*
}
]

that optimize the similarity metric.

---

# 19.40 Optimizer

An optimizer searches the transformation parameter space.

Possible optimization strategies include:

* gradient descent
* regular-step gradient descent
* quasi-Newton methods
* evolutionary methods
* other numerical optimization techniques

The choice depends on the transformation and metric.

---

# 19.41 Registration Landscape

Imagine:

```text
Similarity
   ↑
   │         /\
   │        /  \
   │   /\  /    \
   │  /  \/      \
   └────────────────→ Parameters
```

The optimizer tries to find:

```text
Best peak
```

or:

```text
Minimum valley
```

depending on whether we maximize or minimize the chosen metric.

---

# 19.42 Local Minima

Registration can get stuck in a local optimum.

```text
Similarity
   │
   │     /\       /\
   │    /  \_____/  \
   │___/             \____
```

The optimizer may find:

```text
small peak
```

instead of:

```text
global best peak
```

This is one reason initialization and multi-resolution strategies are important.

---

# 19.43 Initialization

Good initialization can significantly improve registration.

For example:

```text
Fixed
+
Moving
 ↓
Initial translation estimate
 ↓
Optimization
```

Possible initialization:

* image centers
* metadata
* patient positioning
* landmarks
* known coordinate systems

---

# 19.44 Resampling

After finding the transformation, the moving image must be resampled into the fixed image grid.

Conceptually:

```text
Moving Image
      ↓
Transformation
      ↓
Interpolator
      ↓
Fixed Grid
```

This is a critical step.

---

# 19.45 Why Interpolation?

The transformed coordinates usually don't fall exactly on pixel centers.

Example:

```text
Original pixels:

●   ●   ●

Transformed point:

   ×
```

The value at (×) must be estimated.

This is interpolation.

---

# 19.46 Nearest Neighbor

Nearest-neighbor interpolation chooses the closest pixel.

Advantages:

* simple
* fast
* preserves labels

Disadvantage:

* blocky appearance

Very important for:

[
\boxed{
segmentation\ masks
}
]

because it does not create invalid intermediate labels.

---

# 19.47 Linear Interpolation

For 2D, bilinear interpolation estimates using neighboring pixels.

Advantages:

* smoother
* fast
* good for many intensity images

Disadvantage:

* can alter intensity values

Often suitable for continuous-valued image data.

---

# 19.48 Higher-Order Interpolation

Examples:

* cubic
* B-spline interpolation

Can produce smoother results.

But:

```text
More complexity
+
possible overshoot
+
higher computation
```

Choice depends on the application.

---

# 19.49 Critical Rule for Medical Registration

For an intensity image:

```text
CT / MRI
 ↓
Linear / higher-order interpolation
```

For a label map:

```text
Segmentation
 ↓
Nearest neighbor
```

Do **not** casually use linear interpolation on categorical labels.

Otherwise you can create meaningless values such as:

```text
Label 1
+
Label 2
 ↓
1.5
```

which is not a valid anatomical class.

---

# 19.50 Multi-Resolution Registration

Registration can be performed from coarse to fine.

```text
Full resolution
     ↑
Fine
     │
Medium
     │
Coarse
     ↓
Start
```

Typical pipeline:

```text
Coarse image
 ↓
Large structures aligned
 ↓
Medium resolution
 ↓
Refine
 ↓
Full resolution
 ↓
Fine alignment
```

---

# 19.51 Why Multi-Resolution?

At full resolution there may be many local structures.

Optimization can be difficult.

At lower resolution:

```text
Fine details disappear
 ↓
Large structures remain
 ↓
Simpler optimization
```

Then gradually refine.

---

# 19.52 Multi-Resolution Mental Model

```text
Level 3
Very coarse
   ↓
Global alignment

Level 2
Medium
   ↓
Refinement

Level 1
Fine
   ↓
Final alignment
```

---

# 19.53 CT–CT Registration

Same modality:

```text
CT A
+
CT B
```

Potential metrics:

* SSD
* NCC
* mutual information

depending on acquisition differences and preprocessing.

Possible transformation:

```text
Rigid
or
Affine
or
Deformable
```

---

# 19.54 CT–MRI Registration

Different modalities:

```text
CT
+
MRI
```

Intensity values have different meanings.

Therefore:

[
\boxed{
Mutual\ Information
}
]

is a common choice.

Potential pipeline:

```text
CT
 ↓
Fixed

MRI
 ↓
Moving

MI
 ↓
Optimizer
 ↓
Rigid/Affine
 ↓
Registered MRI
```

---

# 19.55 Intra-Patient Registration

Registration between images of the same patient.

Examples:

```text
CT day 1
+
CT day 30
```

or:

```text
CT
+
MRI
```

for the same patient.

Purpose:

* follow-up
* treatment planning
* image fusion
* longitudinal analysis

---

# 19.56 Inter-Patient Registration

Registration between different patients.

Example:

```text
Patient A
     +
Patient B
     ↓
Common anatomical space
```

This is much more challenging because anatomical variation is larger.

Applications include:

* population studies
* atlas construction
* statistical shape analysis
* group analysis

---

# 19.57 Intra vs Inter Patient

| Property             | Intra-patient           | Inter-patient          |
| -------------------- | ----------------------- | ---------------------- |
| Same person          | Yes                     | No                     |
| Anatomical variation | Lower                   | Higher                 |
| Typical transform    | Rigid/Affine/Deformable | Often deformable       |
| Challenge            | Motion/deformation      | Anatomical variability |

---

# 19.58 Registration in Radiation Therapy

This is particularly relevant to your TPS work.

Possible registration:

```text
Planning CT
     +
Follow-up CT
     ↓
Registration
     ↓
Compare anatomy
```

Also:

```text
Planning CT
     +
MRI
     ↓
Registration
     ↓
Better soft-tissue information
```

Registration can support:

* image fusion
* contour propagation
* adaptive planning workflows
* longitudinal comparison

But clinical use requires rigorous validation.

---

# 19.59 Deformable Registration in Radiation Therapy

Example:

```text
Planning CT
   ↓
Treatment
   ↓
New anatomy
```

Organs can move/deform.

Rigid registration may not adequately align them.

Deformable registration can estimate:

```text
Voxel A → Voxel A'
Voxel B → Voxel B'
Voxel C → Voxel C'
```

using a spatial deformation field.

---

# 19.60 Registration and Dose Mapping

In treatment planning, a deformable transformation may be used to map quantities between coordinate systems.

Conceptually:

```text
Dose A
 +
Deformation Field
 ↓
Mapped Dose
```

This is a highly sensitive application.

It is not simply an image-processing visualization feature.

Any clinical dose accumulation workflow requires carefully validated transformation, interpolation, coordinate conventions, and uncertainty handling.

---

# 19.61 Registration Validation

Never assume:

```text
Optimizer converged
=
Registration is correct
```

You should validate using:

### Visual overlay

```text
Fixed
+
Registered Moving
```

### Difference image

```text
Fixed - Registered
```

### Landmarks

Compare corresponding anatomical points.

### Metric

Check similarity measure.

### Clinical structures

Check contours/boundaries.

---

# 19.62 Checkerboard Visualization

A common visualization is a checkerboard:

```text id="q6xjz3"
Fixed | Moving
------+-------
Moving| Fixed
```

If alignment is correct:

```text
boundaries continue smoothly
```

If incorrect:

```text
edges appear discontinuous
```

This is a very useful registration debugging tool.

---

# 19.63 Alpha Overlay

Another method:

```text id="9b4m3s"
Fixed
  +
50% Moving
  ↓
Overlay
```

Misalignment can appear as double edges:

```text id="4q2zv8"
Fixed boundary  |
Moving boundary   |
```

Correct alignment:

```text id="0xv9u6"
Both boundaries overlap
        |
```

---

# 19.64 Difference Image

Calculate:

[
D(x)=F(x)-M(T(x))
]

If alignment is good:

```text
Difference
 ↓
small around corresponding structures
```

But intensity changes between acquisitions can produce differences even when spatial alignment is good.

Therefore difference images are useful but not sufficient by themselves, especially for multimodal registration.

---

# 19.65 Registration Architecture

For your medical imaging application:

```text id="4s7x5e"
RegistrationEngine
       │
       ├── Transform
       │     ├── Rigid
       │     ├── Affine
       │     └── Deformable
       │
       ├── Metric
       │     ├── SSD
       │     ├── NCC
       │     └── MutualInformation
       │
       ├── Optimizer
       │
       ├── Interpolator
       │     ├── Nearest
       │     ├── Linear
       │     └── BSpline
       │
       ├── MultiResolution
       │
       └── Validator
```

---

# 19.66 C++ Interface

A conceptual design:

```cpp
class IRegistrationAlgorithm
{
public:
    virtual ~IRegistrationAlgorithm() = default;

    virtual RegistrationResult registerImages(
        const Image& fixed,
        const Image& moving) = 0;
};
```

Result:

```cpp
struct RegistrationResult
{
    Transform transform;
    Image registeredImage;
    double metricValue;
    bool converged;
};
```

In a production medical system, you would likely include additional metadata, parameter provenance, and validation status.

---

# 19.67 Transformation Interface

```cpp
class ITransform
{
public:
    virtual ~ITransform() = default;

    virtual Point transform(
        const Point& p) const = 0;
};
```

Implementations:

```text
RigidTransform
AffineTransform
DeformableTransform
```

---

# 19.68 Metric Interface

```cpp
class IRegistrationMetric
{
public:
    virtual ~IRegistrationMetric() = default;

    virtual double evaluate(
        const Image& fixed,
        const Image& moving,
        const Transform& transform) const = 0;
};
```

Implementations:

```text
SSDMetric
NCCMetric
MutualInformationMetric
```

---

# 19.69 Optimizer Interface

```cpp
class IRegistrationOptimizer
{
public:
    virtual ~IRegistrationOptimizer() = default;

    virtual Transform optimize(
        const Image& fixed,
        const Image& moving,
        const IRegistrationMetric& metric,
        const Transform& initial) = 0;
};
```

This separation is important.

You should be able to change:

```text
Metric
```

without rewriting:

```text
Transformation
```

or:

```text
Optimizer
```

---

# 19.70 ITK Role

For your medical imaging architecture, **ITK** is particularly relevant to registration because it provides extensive image-processing and registration infrastructure.

Conceptually:

```text
DICOM
 ↓
ITK Image
 ↓
Registration
 ├── Transform
 ├── Metric
 ├── Optimizer
 └── Resampler
 ↓
Registered Image
```

This is preferable to implementing sophisticated medical registration infrastructure from scratch.

---

# 19.71 VTK Role

VTK becomes useful when registered images or structures need visualization.

For example:

```text
ITK
 ↓
Registered volume
 ↓
VTK
 ↓
3D visualization
```

or:

```text
Segmentation
 ↓
Surface extraction
 ↓
VTK
 ↓
3D overlay
```

So:

[
\boxed{
ITK \rightarrow processing/registration
}
]

[
\boxed{
VTK \rightarrow visualization
}
]

as a useful architectural division, though both ecosystems have overlapping capabilities.

---

# 19.72 Registration Workflow in QML Viewer

For your Qt/QML medical viewer:

```text
QML
 │
 ├── Fixed Series
 ├── Moving Series
 ├── Registration Type
 ├── Metric
 ├── Run
 └── Overlay
 │
 ▼
RegistrationController
 │
 ▼
RegistrationEngine
 │
 ▼
ITK / Algorithm
 │
 ▼
RegistrationResult
 │
 ├── Transform
 ├── Registered Image
 └── Metrics
 │
 ▼
QML Viewer
```

---

# 19.73 Important Coordinate-System Issue

Medical imaging registration is not simply:

```text
pixel x,y
```

A medical image has spatial information including:

* origin
* spacing
* direction/orientation
* dimensions

Therefore:

[
\boxed{
Voxel\ coordinates
\neq
physical\ coordinates
}
]

This is extremely important.

---

# 19.74 Voxel Space vs Physical Space

Suppose:

```text
Voxel:
(i,j,k)
```

with:

```text
Spacing:
(sx, sy, sz)
```

and origin:

```text
O
```

The physical location is obtained through the image's spatial geometry.

Conceptually:

[
\boxed{
P=O+D
\begin{bmatrix}
i,s_x\
j,s_y\
k,s_z
\end{bmatrix}
}
]

where (D) represents image orientation/direction.

Exact convention depends on the imaging toolkit and coordinate system.

---

# 19.75 Why This Matters

If you ignore physical coordinates:

```text
CT spacing:
0.5 × 0.5 × 5 mm
```

you may incorrectly assume:

```text
1 voxel = 1 mm
```

which is wrong.

Registration must account for physical geometry.

---

# 19.76 DICOM Registration

DICOM provides spatial metadata that helps establish image geometry and relationships.

A robust system should consider:

* Image Position
* Image Orientation
* Pixel Spacing
* Slice spacing/thickness information
* Frame of Reference UID
* series/study relationships

The **Frame of Reference UID** is particularly important when determining whether images share a common spatial reference frame.

---

# 19.77 Registration and Frame of Reference

Conceptually:

```text
Series A
FrameOfReference = X

Series B
FrameOfReference = X
```

This indicates they may already be in the same reference frame, although this does not automatically mean the images are perfectly aligned for every application.

If:

```text
Series B
FrameOfReference = Y
```

then an explicit spatial relationship/transformation may be needed.

---

# 19.78 Registration vs Reslicing

These are different.

### Registration

Find transformation:

[
T
]

### Resampling/reslicing

Apply transformation to generate an image on a chosen grid.

```text
Registration:
Find T

Resampling:
Apply T
```

Therefore:

[
\boxed{
Registration \neq Resampling
}
]

They are separate stages.

---

# 19.79 Registration vs Image Fusion

Registration:

```text
Align images
```

Fusion:

```text
Combine aligned images
```

Pipeline:

```text
CT
+
MRI
 ↓
Registration
 ↓
Aligned CT + MRI
 ↓
Fusion
 ↓
Visualization
```

So:

[
\boxed{
Registration
\rightarrow
Fusion
}
]

not the reverse.

---

# 19.80 Common Registration Failure Modes

### Poor initialization

```text
Moving too far away
 ↓
Optimizer fails
```

### Wrong transformation model

```text
Deforming anatomy
+
Rigid transform
 ↓
Residual misalignment
```

### Wrong metric

```text
CT + MRI
+
SSD
 ↓
Poor alignment
```

### Poor interpolation

```text
Wrong interpolation
 ↓
Artifacts
```

### Incorrect coordinate system

```text
Wrong orientation
 ↓
Registration appears impossible
```

### Overfitting deformable transform

```text
Noise
 ↓
Deformation
 ↓
Artificial alignment
```

---

# 19.81 Registration Validation Checklist

For an enterprise medical application:

```text
☐ Correct fixed image
☐ Correct moving image
☐ Correct modality
☐ Correct coordinate system
☐ Correct spacing
☐ Correct orientation
☐ Correct transform
☐ Appropriate metric
☐ Appropriate optimizer
☐ Appropriate interpolation
☐ Visual validation
☐ Quantitative validation
☐ Record parameters
```

---

# 19.82 Chapter 19 Mental Model

Memorize this:

```text id="8a1r0j"
                    IMAGE REGISTRATION
                            │
              ┌─────────────┴─────────────┐
              ↓                           ↓
           FIXED                       MOVING
        Reference                    To transform
              │                           │
              └─────────────┬─────────────┘
                            ↓
                      TRANSFORMATION
                            │
             ┌──────────────┼──────────────┐
             ↓              ↓              ↓
           RIGID          AFFINE       DEFORMABLE
             │              │              │
          R + T        R+T+S+Shear    Local deformation
             │              │              │
             └──────────────┴──────────────┘
                            ↓
                          METRIC
             ┌──────────────┼──────────────┐
             ↓              ↓              ↓
            SSD            NCC             MI
             │              │              │
             └──────────────┼──────────────┘
                            ↓
                        OPTIMIZER
                            ↓
                        RESAMPLING
                            ↓
                    REGISTERED IMAGE
                            ↓
                         VALIDATE
```

---

# 19.83 Key Formulas

### Translation

[
\boxed{
x'=x+t_x,\quad y'=y+t_y
}
]

### Rotation

[
\boxed{
R=
\begin{bmatrix}
\cos\theta&-\sin\theta\
\sin\theta&\cos\theta
\end{bmatrix}
}
]

### Affine

[
\boxed{
x'=Ax+t
}
]

### Deformation

[
\boxed{
T(x)=x+u(x)
}
]

### SSD

[
\boxed{
SSD=
\sum_x[F(x)-M(T(x))]^2
}
]

### NCC

[
\boxed{
NCC=
\frac{
\sum(F-\bar F)(M-\bar M)
}{
\sqrt{
\sum(F-\bar F)^2
\sum(M-\bar M)^2
}
}
}
]

### Registration Optimization

[
\boxed{
T^*=
\arg\max_T S(F,M\circ T)
}
]

or, for a cost such as SSD:

[
\boxed{
T^*=
\arg\min_T C(F,M\circ T)
}
]

---

# 19.84 Knowledge Check

### Fundamentals

1. What is image registration?
2. What is a fixed image?
3. What is a moving image?
4. Why is registration an optimization problem?
5. What is the difference between registration and resampling?

### Transformations

6. What is translation?
7. What is rotation?
8. What is scaling?
9. What is shearing?
10. What is a rigid transformation?
11. What is an affine transformation?
12. What is deformable registration?
13. What is a deformation field?

### Metrics

14. What is SSD?
15. Why is lower SSD better?
16. What is NCC?
17. Why can NCC handle some intensity scaling differences?
18. What is mutual information?
19. Why is mutual information useful for CT–MRI registration?

### Algorithms

20. What is feature-based registration?
21. What is intensity-based registration?
22. What is an optimizer?
23. What is local minimum?
24. Why is initialization important?
25. What is multi-resolution registration?

### Resampling

26. Why is interpolation required?
27. When should nearest-neighbor interpolation be used?
28. Why should you not use linear interpolation for segmentation labels?

### Medical imaging

29. Why are physical coordinates important?
30. What is Frame of Reference UID?
31. What is CT–MRI registration?
32. What is intra-patient registration?
33. What is inter-patient registration?
34. Why is deformable registration useful in radiation therapy?
35. Why must registration be validated?
36. Why can an optimizer converging still produce a clinically incorrect registration?

---

# 19.85 Practical Exercise — Rigid Registration

Suppose the moving image is shifted:

[
t_x=10
]

[
t_y=-5
]

and rotated:

[
\theta=10^\circ
]

Explain the complete rigid transformation pipeline.

Draw:

```text
Moving
  ↓
Rotation
  ↓
Translation
  ↓
Registered
```

Also explain why the order of transformations matters when represented as matrix products.

---

# 19.86 Practical Exercise — CT–MRI

You have:

```text
Fixed  = CT
Moving = MRI
```

Choose:

1. Transformation
2. Similarity metric
3. Optimizer
4. Interpolation
5. Validation method

A strong classical answer would consider:

```text
Transformation → rigid/affine initially
Metric         → mutual information
Optimizer      → numerical optimizer
Interpolation  → appropriate continuous-image interpolator
Validation     → overlay + landmarks + visual/anatomical checks
```

Then consider whether deformable registration is necessary.

---

# 19.87 Practical Exercise — Segmentation Registration

You have:

```text
CT
 ↓
Tumor segmentation
```

Then register CT to another CT.

Question:

> Should the tumor mask be resampled using linear interpolation?

**No.**

For a categorical label map, use nearest-neighbor interpolation or another label-preserving strategy.

Why?

Because:

```text
Tumor = 1
Background = 0
```

must remain categorical.

Linear interpolation could create:

```text
0.2
0.5
0.8
```

which are not valid class labels.

---

# 19.88 Medical Imaging Exercise — Registration QA

Design a QA workflow:

```text
Fixed CT
    +
Moving CT
    ↓
Registration
    ↓
Checkerboard
    +
Alpha overlay
    +
Landmark comparison
    ↓
Accept / Reject
```

Explain why a single similarity score is insufficient.

---

# 19.89 Chapter 19 Complete

The central idea is:

[
\boxed{
Registration
============

Find\ a\ spatial\ transformation\ that\ aligns\ images
}
]

The complete mental model:

```text
Fixed + Moving
      ↓
Transformation
      ↓
Similarity Metric
      ↓
Optimization
      ↓
Resampling
      ↓
Registered Image
      ↓
Validation
```

The transformation hierarchy:

[
\boxed{
Rigid
\rightarrow
Affine
\rightarrow
Deformable
}
]

The metric hierarchy:

```text
SSD
 ↓
similar intensity

NCC
 ↓
correlated intensity patterns

Mutual Information
 ↓
statistical relationship
especially useful for multimodal images
```

And the most important medical-imaging principle:

[
\boxed{
\text{Correct registration requires correct spatial geometry, not just visually similar images.}
}
]

**Chapter 19 complete.**

### Next, strictly according to your index:

# Chapter 20 — Image Feature Extraction

Topics:

* What is a feature?
* Low-level vs high-level features
* Point features
* Edge features
* Corner detection
* Harris corner detector
* Shi-Tomasi
* FAST
* Blob detection
* LoG blobs
* DoG
* SIFT
* SURF
* ORB
* BRIEF
* Feature descriptors
* Feature matching
* Distance metrics
* RANSAC
* Feature-based registration
* Medical-image feature extraction
