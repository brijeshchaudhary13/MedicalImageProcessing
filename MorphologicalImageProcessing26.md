# Chapter 26 — Morphological Image Processing

We continue **strictly according to your index**.

Morphological processing is extremely important for your medical imaging software because segmentation rarely produces a perfect mask directly.

Typical pipeline:

```text
DICOM
 ↓
Segmentation
 ↓
Raw Mask
 ↓
Morphological Processing
 ↓
Clean Mask
 ↓
Measurement / Contour / 3D Surface
```

The key idea is:

[
\boxed{
Morphology = shape\text{-}based\ image\ processing
}
]

It is especially powerful for **binary images, masks, labels, and anatomical structures**.

---

# 26.1 What Is Mathematical Morphology?

Mathematical morphology analyzes and modifies image structures according to their **shape and spatial relationships**.

It uses a small shape called a:

[
\boxed{
Structuring\ Element
}
]

or:

[
\boxed{
SE
}
]

Example:

```text id="v8v1py"
3 × 3 structuring element

1 1 1
1 1 1
1 1 1
```

The structuring element moves across the image and determines how pixels are modified.

---

# 26.2 Where Morphology Is Used

Morphological operations are useful for:

* removing small noise
* filling small holes
* connecting broken structures
* separating objects
* extracting boundaries
* cleaning segmentation masks
* skeletonization
* shape analysis

Medical applications include:

```text id="j6n0so"
CT
 ├── Bone mask cleanup
 ├── Lung mask cleanup
 └── Organ segmentation

MRI
 ├── Brain mask cleanup
 └── Tumor mask cleanup

X-Ray
 ├── Bone extraction
 └── Structure cleanup
```

---

# 26.3 Binary Morphology

For binary morphology:

```text id="lppd7k"
0 → Background
1 → Foreground
```

Example:

```text id="n5xx1f"
000000
001100
011110
001100
000000
```

The foreground represents the object.

---

# 26.4 Grayscale Morphology

Morphology is not limited to binary images.

It can also operate on grayscale images.

```text id="z2v6l0"
Binary morphology
     ↓
0 / 1

Grayscale morphology
     ↓
Intensity values
```

The underlying operations use local minimum/maximum behavior.

---

# 26.5 Structuring Element

A structuring element defines the neighborhood used by morphology.

Examples:

### Square

```text id="lqf7xr"
1 1 1
1 1 1
1 1 1
```

### Cross

```text id="1z8x8a"
0 1 0
1 1 1
0 1 0
```

### Horizontal line

```text id="n0f7ls"
1 1 1 1 1
```

### Vertical line

```text id="v7qsm1"
1
1
1
1
1
```

Different structuring elements produce different geometric effects.

---

# 26.6 Why Structuring Element Shape Matters

Suppose you want to preserve horizontal structures.

Use:

```text id="x6x9pj"
1 1 1 1 1
```

For vertical structures:

```text id="o4n4z3"
1
1
1
1
1
```

For approximately isotropic processing:

```text id="8m7n1x"
disk / circle
```

The structuring element therefore encodes a shape assumption.

---

# 26.7 Basic Morphological Operations

The four fundamental operations are:

[
\boxed{
Erosion
}
]

[
\boxed{
Dilation
}
]

[
\boxed{
Opening
}
]

[
\boxed{
Closing
}
]

---

# 26.8 Erosion

Erosion generally **shrinks foreground regions**.

Conceptually:

```text id="wq5gip"
Before:

██████
██████
██████
██████

After:

 ████
 ████
 ████
```

Small structures may disappear.

---

# 26.9 Erosion Intuition

Think of erosion as:

> Removing pixels from the boundary of foreground objects.

Therefore:

```text id="6g0a0h"
Object
 ↓
Boundary removed
 ↓
Smaller object
```

---

# 26.10 Mathematical Definition of Erosion

For sets (A) and (B):

[
\boxed{
A\ominus B
==========

{x\mid B_x\subseteq A}
}
]

where:

* (A) = image/object
* (B) = structuring element

The structuring element must fit completely inside the foreground for the output location to remain foreground.

---

# 26.11 Erosion Example

Suppose:

```text id="0z1d3m"
11111
11111
11111
11111
11111
```

Using a 3×3 structuring element:

```text id="h48c0y"
111
111
111
```

the result becomes approximately:

```text id="2f6puk"
00000
01110
01110
01110
00000
```

The object shrinks.

---

# 26.12 Dilation

Dilation generally **expands foreground regions**.

```text id="2nq7y7"
Before:

  ██
  ██

After:

 ████
██████
 ████
```

---

# 26.13 Dilation Intuition

Think of dilation as:

> Adding pixels around the boundary of foreground objects.

Therefore:

```text id="l7j0y6"
Object
 ↓
Boundary expanded
 ↓
Larger object
```

---

# 26.14 Mathematical Definition of Dilation

For sets (A) and (B):

[
\boxed{
A\oplus B
=========

{x\mid B_x\cap A\neq\emptyset}
}
]

Intuitively, the structuring element adds its shape around foreground locations.

---

# 26.15 Erosion vs Dilation

| Erosion                  | Dilation                   |
| ------------------------ | -------------------------- |
| Shrinks objects          | Expands objects            |
| Removes boundary pixels  | Adds boundary pixels       |
| Can remove small objects | Can fill small gaps        |
| Separates nearby objects | Can connect nearby objects |

---

# 26.16 Opening

Opening is:

[
\boxed{
A\circ B=(A\ominus B)\oplus B
}
]

In words:

```text id="1z3v4b"
Erosion
 ↓
Dilation
```

---

# 26.17 Why Opening?

Opening is useful for:

* removing small foreground noise
* breaking thin connections
* smoothing boundaries
* removing small protrusions

Example:

```text id="3c1s1f"
Large object + tiny dots

██████     ●
██████
██████       ●
```

After opening:

```text id="q5fjf9"
██████
██████
██████
```

Small isolated structures can disappear.

---

# 26.18 Opening Preserves Large Objects

Suppose:

```text id="sk9y9d"
Large object → remains
Tiny object  → removed
```

This is why opening is useful for cleaning segmentation masks.

---

# 26.19 Closing

Closing is:

[
\boxed{
A\bullet B=(A\oplus B)\ominus B
}
]

In words:

```text id="m1p3gr"
Dilation
 ↓
Erosion
```

---

# 26.20 Why Closing?

Closing is useful for:

* filling small holes
* closing small gaps
* connecting nearby structures
* smoothing boundaries

Example:

```text id="8i1z2f"
████████
███  ███
███  ███
████████
```

The hole may be filled by closing.

---

# 26.21 Opening vs Closing

| Opening                          | Closing                      |
| -------------------------------- | ---------------------------- |
| Erosion → Dilation               | Dilation → Erosion           |
| Removes small foreground objects | Fills small background holes |
| Breaks thin connections          | Connects small gaps          |
| Smooths outward protrusions      | Smooths inward gaps          |

Remember:

[
\boxed{
Opening = remove
}
]

[
\boxed{
Closing = fill/connect
}
]

---

# 26.22 Morphological Pipeline

A typical segmentation cleanup:

```text id="8m0p7c"
Raw Mask
   ↓
Opening
   ↓
Remove small noise
   ↓
Closing
   ↓
Fill small holes
   ↓
Clean Mask
```

This is extremely common in practical image-processing pipelines.

---

# 26.23 Example — Tumor Mask

Suppose AI segmentation generates:

```text id="h9r2l3"
0000000000
0011111000
0011111000
0011101000
0011111000
0000100000
0000000010
0000000000
```

There may be:

* small isolated pixels
* holes
* irregular boundaries

Morphology can clean the mask before visualization or measurement.

---

# 26.24 Morphological Gradient

Morphological gradient can be defined as:

[
\boxed{
G=(A\oplus B)-(A\ominus B)
}
]

It highlights object boundaries.

Conceptually:

```text id="2m9z8m"
Object
 ↓
Dilation - Erosion
 ↓
Boundary
```

---

# 26.25 Boundary Extraction

Another simple boundary representation is:

[
\boxed{
Boundary(A)=A-(A\ominus B)
}
]

This extracts the pixels removed by erosion.

Useful for:

* contour visualization
* boundary analysis
* overlay display

---

# 26.26 Morphological Top-Hat

White top-hat:

[
\boxed{
T_{white}=A-(A\circ B)
}
]

It highlights small bright structures that are removed by opening.

Conceptually:

```text id="t5u6k8"
Original
 -
Opened image
 =
Small bright structures
```

---

# 26.27 Black-Hat

Black-hat:

[
\boxed{
T_{black}=(A\bullet B)-A
}
]

It highlights small dark structures that are filled by closing.

---

# 26.28 Top-Hat Applications

Can be useful for:

* local feature enhancement
* uneven background correction
* extracting small bright structures

Medical imaging may use morphology as part of preprocessing, but the clinical interpretation must be validated.

---

# 26.29 Skeletonization

Skeletonization reduces an object to a thin representation while attempting to preserve its topology.

Example:

```text id="wq2v4n"
Thick object:

██████
██████
██████
██████

Skeleton:

  ██
  ██
  ██
  ██
```

The exact result depends on the algorithm.

---

# 26.30 Why Skeletonization?

Useful for structures such as:

* vessels
* roads
* lines
* branching structures

In medical imaging:

```text id="f4jv0s"
Vessel segmentation
 ↓
Skeletonization
 ↓
Centerline
```

---

# 26.31 Vessel Centerline

A vessel mask:

```text id="0q1t0c"
████████
  ██████
    ████
```

can be transformed into a centerline:

```text id="3jv5xx"
   •
   •
    •
     •
```

This can support:

* vessel length
* branching analysis
* centerline measurements

---

# 26.32 Skeleton vs Contour

Do not confuse:

```text id="s9y8n0"
Skeleton
```

with:

```text id="5q8j9n"
Boundary
```

### Boundary

Describes the outer edge.

### Skeleton

Describes an internal medial structure.

```text id="8cnp2j"
Boundary:
████████
██    ██
████████

Skeleton:
   ██
   ██
```

---

# 26.33 Morphological Reconstruction

Morphological reconstruction uses:

```text id="f0v6tk"
Marker
+
Mask
```

to reconstruct connected structures under constraints.

Conceptually:

```text id="h0p4f8"
Marker
  +
Mask
  ↓
Reconstruction
```

This is more selective than simple dilation.

---

# 26.34 Why Reconstruction Is Useful

It can preserve connected structures while removing unwanted features.

Applications include:

* object extraction
* hole filling
* marker-based segmentation
* connected component filtering

---

# 26.35 Hole Filling

Suppose:

```text id="9v6m8d"
████████
██    ██
██    ██
████████
```

The center is a hole.

Hole filling attempts:

```text id="v6p3bz"
████████
████████
████████
████████
```

---

# 26.36 Connected Components + Morphology

A powerful combination:

```text id="o3o5p4"
Segmentation
 ↓
Connected Components
 ↓
Calculate area
 ↓
Remove tiny components
 ↓
Morphological Closing
 ↓
Final mask
```

---

# 26.37 Area Opening

Instead of using a fixed geometric structuring element, area-based filtering can remove connected components smaller than a chosen size.

Example:

```text id="4j4y1p"
Component A → 50,000 voxels
Component B → 100 voxels
Component C → 5 voxels
```

Threshold:

[
A_{min}=1000
]

Keep:

```text id="d6n0ab"
A
```

Remove:

```text id="i4z2cz"
B
C
```

---

# 26.38 Morphology in CT

Example bone mask:

```text id="tv0f2c"
CT
 ↓
HU threshold
 ↓
Bone mask
 ↓
Opening
 ↓
Remove small noise
 ↓
Closing
 ↓
Fill small gaps
 ↓
Connected components
 ↓
Final bone mask
```

---

# 26.39 Morphology in MRI

Example brain mask:

```text id="m1e3fo"
MRI
 ↓
Initial segmentation
 ↓
Opening
 ↓
Remove small regions
 ↓
Closing
 ↓
Fill holes
 ↓
Brain mask
```

The exact workflow depends on sequence and segmentation method.

---

# 26.40 Morphology in Ultrasound

Speckle can create many small regions.

Morphological operations can help with:

```text id="1cyk5g"
Candidate segmentation
 ↓
Remove small regions
 ↓
Fill gaps
```

However, morphology should not be used as a substitute for proper speckle modeling.

---

# 26.41 Morphology in X-Ray

After thresholding or edge-based processing:

```text id="7ktwqk"
X-Ray
 ↓
Candidate mask
 ↓
Morphological cleanup
 ↓
Connected components
 ↓
Structure extraction
```

---

# 26.42 2D Morphology

For 2D:

```text id="h3z1v4"
Image(x,y)
```

structuring element operates over:

```text id="j2e7vv"
x,y neighborhood
```

Example:

```text id="8x4z5m"
3×3
5×5
disk
cross
```

---

# 26.43 3D Morphology

For volumetric data:

[
I(x,y,z)
]

we can use a 3D structuring element.

Example:

```text id="x5g3xk"
3 × 3 × 3
```

Now morphology operates across adjacent slices.

---

# 26.44 Why 3D Morphology Matters

Suppose an anatomical structure is continuous across slices.

2D processing:

```text id="8n9h4g"
Slice 1 → process
Slice 2 → process
Slice 3 → process
```

may create inconsistent results.

3D morphology:

```text id="kq1j1o"
Volume
 ↓
3D structuring element
 ↓
Spatially consistent processing
```

can preserve volumetric continuity.

---

# 26.45 Isotropic vs Anisotropic Voxels

Medical volumes often have different spacing:

```text id="q9a5ef"
X spacing = 0.7 mm
Y spacing = 0.7 mm
Z spacing = 5.0 mm
```

A naive 3×3×3 structuring element treats voxel indices equally, but physical dimensions are not equal.

Therefore:

[
\boxed{
Voxel\ spacing\ matters
}
]

---

# 26.46 Why This Is Important

A 3-voxel operation along Z may correspond to:

[
3\times5=15\text{ mm}
]

while 3 voxels in X correspond to:

[
3\times0.7=2.1\text{ mm}
]

That is a huge physical difference.

Therefore medical morphology should consider image spacing when appropriate.

---

# 26.47 Physical-Size Morphology

Instead of saying:

```text id="wq5a2b"
kernel = 5 voxels
```

it can be better to reason in physical dimensions:

```text id="l2f4j8"
kernel ≈ 3 mm
```

and convert that to voxel dimensions based on spacing.

This is especially important for CT/MRI volumes.

---

# 26.48 Morphology and Anisotropic Volumes

Example:

```text id="bq3e3r"
Voxel spacing:

0.8 × 0.8 × 4.0 mm
```

A spherical physical structuring element should not necessarily be:

```text id="z0x0u6"
5 × 5 × 5 voxels
```

because that corresponds to:

[
4\times4\times20\text{ mm}
]

which is highly elongated physically.

---

# 26.49 ITK Morphology

ITK is particularly suitable for medical morphological processing because it supports:

```text id="a1h2h6"
2D images
3D images
Physical spacing
Medical image pipelines
```

It provides various binary and grayscale morphological filters.

---

# 26.50 OpenCV Morphology

OpenCV provides practical operations such as:

```cpp id="5hqu4a"
cv::erode()
cv::dilate()
cv::morphologyEx()
```

and operations for:

```text id="19b2hj"
Opening
Closing
Gradient
Top-hat
Black-hat
```

---

# 26.51 OpenCV Example

Conceptually:

```cpp id="2a3g75"
cv::Mat kernel =
    cv::getStructuringElement(
        cv::MORPH_ELLIPSE,
        cv::Size(5, 5)
    );

cv::morphologyEx(
    mask,
    result,
    cv::MORPH_CLOSE,
    kernel
);
```

This is useful for 2D mask processing.

---

# 26.52 ITK vs OpenCV

| Requirement                 | Better fit |
| --------------------------- | ---------- |
| 2D general image morphology | OpenCV     |
| 3D medical volume           | ITK        |
| Physical voxel spacing      | ITK        |
| Quick image processing      | OpenCV     |
| Medical image pipeline      | ITK        |
| Visualization               | VTK        |

Again, you do not need to force one library to do everything.

---

# 26.53 VTK Role

VTK is useful after morphology when you want to visualize the resulting segmentation:

```text id="u3yyi9"
3D Mask
 ↓
Surface extraction
 ↓
VTK
 ↓
3D structure
```

For example:

```text id="2f8t8p"
Binary volume
 ↓
Marching Cubes
 ↓
vtkPolyData
 ↓
3D rendering
```

---

# 26.54 Qt/QML Role

Qt/QML should control the workflow:

```text id="m4o4m6"
QML
 ↓
MorphologyController
 ↓
ProcessingEngine
 ↓
ITK/OpenCV
 ↓
Mask
 ↓
Viewer
```

UI controls might include:

```text id="b4c7pn"
Morphology
 ├── Operation
 ├── Shape
 ├── Size
 ├── Iterations
 ├── 2D / 3D
 ├── Apply
 └── Reset
```

---

# 26.55 Morphology Engine Design

A clean interface:

```cpp id="a6qk83"
enum class MorphologyOperation
{
    Erode,
    Dilate,
    Open,
    Close,
    Gradient,
    TopHat,
    BlackHat
};
```

Parameters:

```cpp id="6s4tx6"
struct MorphologyParameters
{
    MorphologyOperation operation;
    int radius;
    int iterations;
};
```

---

# 26.56 Morphology Strategy

You can create:

```cpp id="j8klpv"
class IMorphologyOperation
{
public:
    virtual ~IMorphologyOperation() = default;

    virtual Image apply(
        const Image& input,
        const MorphologyParameters& params) = 0;
};
```

Implementations:

```text id="6t2e0c"
ErosionOperation
DilationOperation
OpeningOperation
ClosingOperation
```

---

# 26.57 Non-Destructive Medical Workflow

For your viewer:

```text id="4l2y0g"
Original DICOM
      │
      ├────────→ Original Display
      │
      └────────→ Derived Mask
                       ↓
                  Morphology
                       ↓
                 Clean Mask
                       ↓
                 Visualization
```

Never silently overwrite the original DICOM pixel data.

---

# 26.58 Morphology + Segmentation Architecture

This is the complete connection between Chapters 25 and 26:

```text id="u9qj6x"
DICOM
 ↓
Decoded Image
 ↓
Segmentation
 ↓
Raw Mask
 ↓
Morphological Processing
 ├── Opening
 ├── Closing
 ├── Hole Filling
 ├── Component Filtering
 └── Boundary Processing
 ↓
Final Mask
 ↓
Contour / Surface
 ↓
LUT
 ↓
Alpha Overlay
 ↓
QML / VTK
```

---

# 26.59 Morphology + AI

AI segmentation often produces imperfect masks:

```text id="h1i9te"
AI
 ↓
Probability Map
 ↓
Threshold
 ↓
Raw Mask
 ↓
Morphological Post-processing
 ↓
Final Mask
```

Possible operations:

```text id="x2r0wb"
Remove isolated components
Fill small holes
Smooth boundaries
```

But post-processing should be validated because it can also remove clinically meaningful structures.

---

# 26.60 Morphology + Measurements

After cleaning:

```text id="0qf3pi"
Mask
 ↓
Connected Components
 ↓
Area / Volume
 ↓
Centroid
 ↓
Shape
 ↓
Clinical measurement
```

A poor mask can produce incorrect measurements.

Therefore:

[
\boxed{
Segmentation\ quality
\rightarrow
Measurement\ quality
}
]

---

# 26.61 Morphology + Contouring

Pipeline:

```text id="h3k7x4"
Mask
 ↓
Morphological cleanup
 ↓
Boundary extraction
 ↓
Contour
 ↓
Display
```

This is particularly relevant to radiation therapy structures.

---

# 26.62 Morphology + 3D Surface

Pipeline:

```text id="1f5v4x"
3D Mask
 ↓
Morphological cleanup
 ↓
Marching Cubes
 ↓
3D Mesh
 ↓
VTK
 ↓
3D Viewer
```

---

# 26.63 Common Mistake

Do not think:

> Erosion = always bad because it removes pixels.

or:

> Dilation = always good because it fills gaps.

The correct operation depends on the problem.

For example:

```text id="7jhl8r"
Small false-positive islands
 ↓
Opening
```

while:

```text id="k8bq84"
Small holes inside correct object
 ↓
Closing
```

---

# 26.64 Another Common Mistake

Do not use an arbitrary kernel size without considering:

```text id="x8n2sv"
Image resolution
Voxel spacing
Anatomical scale
Clinical task
```

A 10-pixel operation may mean:

```text id="cz6k8g"
2 mm
```

in one image and:

```text id="4g5r8k"
10 mm
```

in another.

---

# 26.65 Enterprise-Level Morphology Pipeline

```text id="9p4m6n"
                   MORPHOLOGY ENGINE
                          │
          ┌───────────────┼────────────────┐
          ↓               ↓                ↓
       Binary          Grayscale         3D
          │               │                │
     ┌────┼────┐      ┌───┼───┐       ┌───┴────┐
     ↓    ↓    ↓      ↓   ↓   ↓       ↓        ↓
 Erosion Dilation Open Close TopHat  Volume  Spacing
     │    │       │      │             │
     └────┴───────┴──────┴─────────────┘
                      ↓
               Processed Image/Mask
                      ↓
               Segmentation Output
                      ↓
                Measurements
                      ↓
             Contour / 3D Surface
```

---

# 26.66 Key Formulas

### Erosion

[
\boxed{
A\ominus B
}
]

### Dilation

[
\boxed{
A\oplus B
}
]

### Opening

[
\boxed{
A\circ B=(A\ominus B)\oplus B
}
]

### Closing

[
\boxed{
A\bullet B=(A\oplus B)\ominus B
}
]

### Morphological gradient

[
\boxed{
G=(A\oplus B)-(A\ominus B)
}
]

### White top-hat

[
\boxed{
T_{white}=A-(A\circ B)
}
]

### Black-hat

[
\boxed{
T_{black}=(A\bullet B)-A
}
]

---

# 26.67 Most Important Concepts

Remember:

[
\boxed{
Erosion = Shrink
}
]

[
\boxed{
Dilation = Expand
}
]

[
\boxed{
Opening = Erode \rightarrow Dilate
}
]

[
\boxed{
Closing = Dilate \rightarrow Erode
}
]

And:

[
\boxed{
Structuring\ Element
====================

shape\ that\ controls\ morphological\ processing
}
]

---

# 26.68 Medical Imaging Mental Model

```text id="r3z4a7"
                 SEGMENTATION MASK
                        │
                        ↓
                 Is it noisy?
                        │
                       YES
                        ↓
                    Opening
                        │
                        ↓
                  Small noise ↓
                        │
                        ↓
                  Are there holes?
                        │
                       YES
                        ↓
                    Closing
                        │
                        ↓
                  Holes ↓ / gaps ↓
                        │
                        ↓
               Connected Components
                        │
                        ↓
                Remove tiny regions
                        │
                        ↓
                   Clean Mask
                        │
            ┌───────────┴───────────┐
            ↓                       ↓
        Contours                3D Surface
            ↓                       ↓
           QML                     VTK
```

---

# 26.69 Interview Questions

### Fundamentals

1. What is mathematical morphology?
2. What is a structuring element?
3. What is binary morphology?
4. What is grayscale morphology?
5. What is erosion?
6. What is dilation?
7. What is opening?
8. What is closing?

### Advanced

9. What is morphological gradient?
10. What is top-hat transformation?
11. What is black-hat transformation?
12. What is skeletonization?
13. What is morphological reconstruction?
14. What is hole filling?
15. What is connected-component filtering?
16. Why does structuring-element shape matter?

### Medical Imaging

17. Why is morphology useful after segmentation?
18. How can opening remove segmentation noise?
19. How can closing fill holes?
20. Why is 3D morphology different from 2D morphology?
21. Why does voxel spacing matter?
22. Why can a 5×5×5 kernel be dangerous on anisotropic CT data?
23. How can morphology help vessel segmentation?
24. How can morphology affect quantitative measurements?
25. Why should morphological processing not overwrite original DICOM data?

---

# 26.70 Practical Exercise — Opening

Given:

```text id="q8n8e2"
000000000
001110000
001110010
001110000
000000000
```

Assume the isolated pixel on the right is noise.

Explain how **opening** can remove it while preserving the main object.

---

# 26.71 Practical Exercise — Closing

Given:

```text id="v5k9qk"
000000
011110
011010
011110
000000
```

There is a small hole in the object.

Explain why **closing** is more appropriate than opening.

---

# 26.72 Practical Exercise — Medical 3D

Suppose CT spacing is:

[
0.7\times0.7\times5.0\text{ mm}
]

Explain why a:

[
5\times5\times5
]

voxel structuring element is not physically isotropic.

Calculate its approximate physical dimensions.

---

# 26.73 Chapter 26 Complete

The core workflow is:

[
\boxed{
Segmentation
\rightarrow
Morphological\ Cleanup
\rightarrow
Final\ Mask
}
]

The most important operations are:

```text id="d1q6pq"
Erosion
Dilation
Opening
Closing
```

For your enterprise medical imaging system:

```text id="z8v1k2"
DICOM
 ↓
DCMTK
 ↓
Decoded Image
 ↓
ITK / OpenCV
 ↓
Segmentation
 ↓
Morphology
 ↓
Clean Mask
 ↓
Contour / Measurement / 3D Surface
 ↓
VTK / QML
```

**Chapter 26 complete.**
