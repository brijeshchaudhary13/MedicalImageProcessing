# Chapter 18 — Image Segmentation

We continue **strictly according to your index**.

Chapter 18 topics:

1. What is segmentation?
2. Semantic vs instance segmentation
3. Thresholding
4. Global thresholding
5. Adaptive thresholding
6. Otsu
7. Region growing
8. Region splitting
9. Region merging
10. Watershed
11. Connected components
12. K-means segmentation
13. Active contours
14. Level sets
15. Graph-based segmentation
16. Medical-image segmentation
17. CT/MRI/X-ray segmentation considerations

This is one of the most important chapters for your **medical imaging / DICOM viewer / future segmentation system**.

---

# 18.1 What Is Image Segmentation?

Image segmentation divides an image into meaningful regions.

Conceptually:

```text
Input Image
     ↓
Segmentation
     ↓
Regions / Objects
```

For example:

```text
CT Image
   ↓
Segment
   ↓
Lung
Bone
Tumor
Background
```

The result is usually a **mask** or a labeled image.

---

# 18.2 Simple Binary Segmentation

Suppose:

```text
Intensity:

50  50  50  200 200 200
50  50  50  200 200 200
```

Choose threshold:

[
T=100
]

Then:

[
g(x,y)=
\begin{cases}
1,&f(x,y)>T\
0,&f(x,y)\le T
\end{cases}
]

Result:

```text
0 0 0 1 1 1
0 0 0 1 1 1
```

This is the simplest form of segmentation.

---

# 18.3 Segmentation vs Classification

These are different.

### Classification

Answers:

> What is in the image?

Example:

```text
Image → CT
```

### Segmentation

Answers:

> Which pixels/voxels belong to the object?

Example:

```text
Image
 ↓
Tumor mask
```

Therefore:

[
\boxed{
Classification = label
}
]

[
\boxed{
Segmentation = label + location
}
]

---

# 18.4 Semantic Segmentation

Semantic segmentation assigns a class to every pixel/voxel.

Example:

```text
Background → 0
Lung       → 1
Tumor      → 2
Bone       → 3
```

Conceptually:

```text id="1wz8tw"
Input
 ↓
Pixel-wise classification
 ↓
Semantic mask
```

All objects belonging to the same class receive the same label.

---

# 18.5 Instance Segmentation

Instance segmentation distinguishes individual objects.

Example:

```text
Tumor 1
Tumor 2
Tumor 3
```

even though all are the same class.

Output:

```text
Instance 1 → Label 1
Instance 2 → Label 2
Instance 3 → Label 3
```

---

# 18.6 Semantic vs Instance

| Feature                          | Semantic               | Instance               |
| -------------------------------- | ---------------------- | ---------------------- |
| Class labels                     | Yes                    | Yes                    |
| Pixel-level                      | Yes                    | Yes                    |
| Distinguishes individual objects | No                     | Yes                    |
| Example                          | All lungs = same class | Each lesion separately |

---

# 18.7 Segmentation Output Types

Common representations:

### Binary mask

```text
0 = background
1 = object
```

### Multi-label mask

```text
0 = background
1 = liver
2 = kidney
3 = tumor
```

### Probability map

```text
0.00 → unlikely
0.50 → uncertain
1.00 → highly likely
```

### Contour

```text
Object boundary only
```

### Mesh

```text
3D surface
```

---

# 18.8 Thresholding

Thresholding separates pixels according to intensity.

The basic equation:

[
\boxed{
g(x,y)=
\begin{cases}
1,&f(x,y)\ge T\
0,&f(x,y)<T
\end{cases}
}
]

where (T) is the threshold.

---

# 18.9 Global Thresholding

A single threshold is applied to the entire image.

```text
Image
 ↓
One T
 ↓
Foreground / Background
```

Example:

[
T=128
]

Everything above 128:

```text
Foreground
```

Everything below:

```text
Background
```

---

# 18.10 When Global Thresholding Works Well

Global thresholding works well when:

```text
Object intensity
        ↓
clearly separated from
        ↓
background intensity
```

Example:

```text
Background = 20
Object = 200
```

Histogram:

```text
frequency
   │
   │ ███
   │ ███        ███
   │ ███        ███
   └──────────────────
     20         200
```

A threshold between the two peaks can work well.

---

# 18.11 When Global Thresholding Fails

Suppose illumination/intensity varies:

```text
Left side:
object = 120

Right side:
object = 180
```

A single threshold may classify one side incorrectly.

Therefore:

[
\boxed{
Non-uniform intensity
\rightarrow
global threshold may fail
}
]

---

# 18.12 Adaptive Thresholding

Adaptive thresholding calculates the threshold locally.

Conceptually:

```text
Image
 ↓
Local neighborhood
 ↓
Local threshold
 ↓
Pixel classification
```

So instead of:

[
T=\text{constant}
]

we have:

[
T=T(x,y)
]

---

# 18.13 Adaptive Threshold Example

Suppose:

```text
Left region → darker
Right region → brighter
```

Adaptive thresholding can use:

```text
Left → lower local threshold
Right → higher local threshold
```

This handles nonuniform illumination/intensity better.

---

# 18.14 Adaptive Threshold Types

Common approaches include:

### Mean-based

[
T(x,y)=\text{local mean}-C
]

### Gaussian-weighted

[
T(x,y)=\text{weighted local mean}-C
]

where (C) controls the offset.

---

# 18.15 Otsu Thresholding

Otsu's method automatically chooses a global threshold from the image histogram.

The fundamental idea:

> Choose the threshold that best separates foreground and background classes.

It is particularly useful when the histogram is approximately bimodal.

---

# 18.16 Otsu Intuition

Histogram:

```text
frequency
   │
   │ ███                 ███
   │ ███                 ███
   │ ███      valley     ███
   └──────────────────────────
       Background   Object
```

Otsu tries to find a threshold near the valley between the two classes.

---

# 18.17 Otsu Mathematical Idea

For every possible threshold (t), divide pixels into:

[
C_0
]

and:

[
C_1
]

Calculate within-class variance:

[
\sigma_w^2(t)
]

Otsu chooses:

[
\boxed{
t^*=
\arg\min_t \sigma_w^2(t)
}
]

Equivalent formulations maximize between-class variance:

[
\boxed{
t^*=
\arg\max_t \sigma_b^2(t)
}
]

---

# 18.18 Otsu Limitations

Otsu is not magic.

It may perform poorly when:

* histogram isn't bimodal
* object/background intensities overlap
* illumination is nonuniform
* image contains multiple tissues
* noise is strong

For medical images, simple Otsu thresholding can be useful in specific cases but is rarely a universal segmentation solution.

---

# 18.19 Region-Based Segmentation

Instead of looking only at individual pixel intensity, region-based methods consider neighboring pixels and region similarity.

Basic idea:

```text
Seed
 ↓
Find similar neighboring pixels
 ↓
Grow region
```

---

# 18.20 Region Growing

Start with a seed point.

Example:

```text
        Seed
          ↓
        █
      ███
    █████
```

Then examine neighboring pixels.

If they satisfy the similarity criterion:

```text
Add to region
```

Continue until no more suitable pixels are found.

---

# 18.21 Region Growing Algorithm

Conceptually:

```text
1. Select seed
2. Add seed to region
3. Examine neighbors
4. Test similarity
5. Add suitable neighbors
6. Repeat
7. Stop when no more pixels qualify
```

---

# 18.22 Similarity Criterion

Could be based on:

* intensity difference
* mean intensity
* variance
* texture
* distance
* multiple features

Simple example:

[
|I(p)-I(seed)|<T
]

If true:

```text
Add pixel
```

---

# 18.23 Region Growing Advantages

* intuitive
* connected regions
* incorporates spatial information
* can work well for homogeneous structures

---

# 18.24 Region Growing Problems

It depends heavily on:

```text
Seed selection
+
Similarity threshold
```

Bad seed:

```text
Wrong region
```

Threshold too small:

```text
Region incomplete
```

Threshold too large:

```text
Region leaks
```

---

# 18.25 Region Leakage

Suppose:

```text
Organ
   ↓
similar neighboring tissue
```

If the threshold is too permissive:

```text
Organ
 ↓
neighboring tissue
 ↓
background
```

The region grows beyond the intended structure.

This is called:

[
\boxed{
Region\ leakage
}
]

---

# 18.26 Region Splitting

Region splitting starts with a large region and divides it when it is not homogeneous.

Conceptually:

```text
Whole Image
    ↓
Is region homogeneous?
    │
   No
    ↓
Split
 ┌──┴──┐
 ↓     ↓
R1    R2
```

Continue recursively.

---

# 18.27 Region Splitting and Quadtree

A common implementation uses a quadtree.

Each region is divided into four:

```text
┌─────┬─────┐
│  1  │  2  │
├─────┼─────┤
│  3  │  4  │
└─────┴─────┘
```

If a region is homogeneous:

```text
STOP
```

Otherwise:

```text
Split again
```

---

# 18.28 Region Merging

Region merging does the opposite.

Start with multiple regions:

```text
R1 R2 R3 R4
```

If neighboring regions are sufficiently similar:

```text
R1 + R2 → R12
```

Then:

```text
R12 + R3 → R123
```

---

# 18.29 Split-and-Merge

These can be combined:

```text
Image
 ↓
Split
 ↓
Small regions
 ↓
Merge similar neighboring regions
 ↓
Final segmentation
```

This uses both:

```text
Homogeneity
+
Spatial connectivity
```

---

# 18.30 Connected Components

Connected-component labeling identifies separate connected objects in a binary image.

Example:

```text
███       ██
███       ██


    ███
```

Output:

```text
Component 1
Component 2
Component 3
```

Each connected region gets a unique label.

---

# 18.31 4-Connectivity

Neighbors:

```text
  N
W X E
  S
```

Only horizontal and vertical neighbors are considered.

---

# 18.32 8-Connectivity

Includes diagonals:

```text
NW N NE
 W X E
SW S SE
```

This can join diagonally touching pixels.

---

# 18.33 Why Connected Components Matter

After segmentation:

```text
Binary mask
 ↓
Connected components
 ↓
Remove tiny components
 ↓
Keep relevant component
```

For example:

```text
Tumor segmentation
       ↓
Connected components
       ↓
Largest / selected lesion
```

But "largest component = tumor" is only valid when that assumption is justified by the task.

---

# 18.34 Watershed Segmentation

Watershed is based on a topographic interpretation of an image.

Imagine the image as a landscape:

```text
High intensity → mountains
Low intensity  → valleys
```

Now imagine flooding the landscape.

Water fills valleys.

Where two flooded regions would meet:

```text
Watershed boundary
```

is created.

---

# 18.35 Watershed Mental Model

```text
Image
 ↓
Topographic surface
 ↓
Flood valleys
 ↓
Catchment basins
 ↓
Watershed lines
 ↓
Segmentation
```

---

# 18.36 Catchment Basins

Each local minimum can act as a basin.

During flooding:

```text
Basin A
     ↓
~~~~~~~

Basin B
     ↓
~~~~~~~
```

When water from different basins approaches:

```text
A | B
  ↑
watershed
```

a boundary is created.

---

# 18.37 Watershed Over-Segmentation

A major problem is:

[
\boxed{
Over-segmentation
}
]

Noise creates many local minima.

Therefore:

```text
Noise
 ↓
Many minima
 ↓
Many basins
 ↓
Too many regions
```

---

# 18.38 Marker-Controlled Watershed

A common solution is to use markers.

```text
Image
 +
Markers
 ↓
Controlled watershed
 ↓
Meaningful regions
```

Markers tell the algorithm where important regions should originate.

This can dramatically reduce over-segmentation.

---

# 18.39 K-Means Segmentation

K-means is an unsupervised clustering algorithm.

Suppose we choose:

[
K=3
]

The algorithm tries to divide pixels into 3 clusters.

Conceptually:

```text
Pixel intensities
       ↓
K-means
       ↓
Cluster 1
Cluster 2
Cluster 3
```

---

# 18.40 K-Means Algorithm

### Step 1

Choose (K) cluster centers.

### Step 2

Assign each pixel to nearest center.

### Step 3

Recalculate cluster centers.

### Step 4

Repeat.

Until:

```text
Cluster assignments stabilize
```

---

# 18.41 K-Means Objective

K-means minimizes:

[
\boxed{
J=
\sum_{k=1}^{K}
\sum_{x_i\in C_k}
|x_i-\mu_k|^2
}
]

where:

* (C_k) = cluster
* (\mu_k) = cluster center

---

# 18.42 K-Means for Images

You don't have to use only intensity.

Features could include:

```text
Pixel
 ├── intensity
 ├── x
 ├── y
 ├── texture
 └── other features
```

This is important because purely intensity-based clustering ignores spatial relationships.

---

# 18.43 K-Means Limitations

* must choose (K)
* sensitive to initialization
* assumes cluster structure
* can ignore spatial connectivity
* may produce anatomically meaningless regions

Therefore:

[
\boxed{
K-means
\neq
medical\ semantic\ segmentation
}
]

---

# 18.44 Active Contours

Active contours, often called **snakes**, represent a deformable curve that moves toward object boundaries.

Conceptually:

```text
Initial contour
      ↓
   (      )
      ↓
Deformation
      ↓
Object boundary
```

---

# 18.45 Active Contour Energy

A simplified energy formulation:

[
\boxed{
E=
E_{internal}
+
E_{image}
+
E_{external}
}
]

The contour moves to minimize energy.

---

# 18.46 Internal Energy

Controls contour smoothness.

It discourages:

```text
Very sharp bends
Unstable shapes
```

So the contour remains reasonably smooth.

---

# 18.47 Image Energy

Encourages the contour toward image features such as edges.

```text
Strong edge
     ↓
Attract contour
```

---

# 18.48 External Constraints

Additional forces can guide the contour.

For example:

```text
User initialization
Shape prior
Region constraints
```

This makes active contours more flexible.

---

# 18.49 Level Sets

Level-set methods represent a contour implicitly using a higher-dimensional function.

Instead of storing the contour directly:

```text
x(t), y(t)
```

we use a function:

[
\phi(x,y)
]

The contour is represented by:

[
\boxed{
\phi(x,y)=0
}
]

---

# 18.50 Why Level Sets?

One advantage is that topology can change naturally.

For example:

```text
One object
     ↓
splits into two
```

or:

```text
Two objects
     ↓
merge into one
```

without explicitly rebuilding the contour representation.

---

# 18.51 Active Contour vs Level Set

| Feature          | Active Contour | Level Set         |
| ---------------- | -------------- | ----------------- |
| Representation   | Explicit curve | Implicit function |
| Topology changes | More difficult | Natural           |
| Deformation      | Yes            | Yes               |
| 3D extension     | Possible       | Natural framework |
| Complexity       | Moderate       | Higher            |

---

# 18.52 Graph-Based Segmentation

Graph-based methods represent an image as a graph.

```text
Pixels / voxels
      ↓
Graph nodes
      ↓
Neighbor relationships
      ↓
Graph edges
```

Each edge has a weight representing similarity/difference.

---

# 18.53 Graph Representation

Example:

```text
A ---- B
|      |
|      |
C ---- D
```

Each node is a pixel/voxel.

Edge weight could represent:

[
w(i,j)=|I_i-I_j|
]

or a more sophisticated similarity measure.

---

# 18.54 Graph Cut

Graph-cut methods formulate segmentation as an optimization problem.

Conceptually:

```text
Graph
 ↓
Define source/background
Define sink/foreground
 ↓
Find optimal cut
 ↓
Segmentation
```

The goal is to separate the graph into meaningful regions while minimizing an energy function.

---

# 18.55 Why Graph Methods Are Powerful

They can combine:

* intensity
* spatial relationships
* smoothness
* boundary information
* user constraints

This makes them useful for interactive and structured segmentation.

---

# 18.56 Medical Segmentation Hierarchy

For medical imaging, segmentation approaches can be viewed broadly as:

```text
Segmentation
     │
     ├── Classical
     │    ├── Threshold
     │    ├── Region
     │    ├── Watershed
     │    └── Morphology
     │
     ├── Optimization
     │    ├── Active contours
     │    ├── Level sets
     │    └── Graph cuts
     │
     └── AI / Deep Learning
          ├── U-Net
          ├── nnU-Net
          ├── 3D networks
          └── Transformer-based methods
```

Your current Chapter 18 is focusing on the classical foundation.

AI segmentation belongs to later advanced chapters.

---

# 18.57 CT Segmentation

CT provides quantitative attenuation information, commonly represented in Hounsfield Units after appropriate DICOM rescaling.

Example concept:

```text
Air       → very low HU
Fat       → low HU
Soft tissue → intermediate
Bone      → high HU
```

This makes threshold-based segmentation useful for certain structures.

But tissue overlap means a single threshold does not universally segment every organ.

---

# 18.58 CT Example — Bone

A simple conceptual approach:

```text
CT
 ↓
HU conversion
 ↓
Threshold high HU
 ↓
Bone mask
 ↓
Morphological cleanup
```

Then:

```text
Connected components
 ↓
Remove irrelevant structures
```

This is a classical pipeline.

---

# 18.59 MRI Segmentation

MRI is different.

MRI intensity is not generally standardized in the same way as CT HU.

Therefore:

```text
MRI
 ↓
Intensity normalization
 ↓
Segmentation
```

may be necessary.

Different sequences also produce different tissue contrast:

* T1
* T2
* FLAIR
* diffusion-related images

Therefore segmentation strategy depends strongly on sequence and acquisition.

---

# 18.60 X-Ray Segmentation

X-ray is a 2D projection.

Unlike CT:

```text
CT:
3D anatomy
```

X-ray:

```text
3D anatomy projected into 2D
```

Different anatomical structures overlap.

Therefore simple segmentation can be significantly harder.

---

# 18.61 Medical Segmentation Pipeline

A robust classical pipeline might look like:

```text
DICOM
  ↓
Pixel conversion
  ↓
Preprocessing
  ↓
Noise reduction
  ↓
Intensity normalization
  ↓
Segmentation
  ↓
Morphological cleanup
  ↓
Connected components
  ↓
Boundary refinement
  ↓
Validation
  ↓
Mask
```

---

# 18.62 Segmentation and Morphology

Chapter 17 now becomes directly useful.

Example:

```text
Threshold
   ↓
Raw mask
   ↓
Opening
   ↓
Remove small components
   ↓
Closing
   ↓
Fill gaps
   ↓
Final mask
```

This is a common classical image-processing pattern.

---

# 18.63 Segmentation and Edge Detection

Another pipeline:

```text
Image
 ↓
Edge detection
 ↓
Boundary information
 ↓
Segmentation
```

For example, active contours can use image edges to guide the contour.

So:

```text
Chapter 15
Edge Detection
       ↓
Chapter 18
Segmentation
```

are directly connected.

---

# 18.64 Segmentation and Restoration

Another connection:

```text
Chapter 16
Restoration
      ↓
Cleaner image
      ↓
Chapter 18
Segmentation
```

Poor preprocessing can cause:

```text
Noise
 ↓
Bad segmentation
```

Therefore preprocessing and segmentation should be considered together.

---

# 18.65 Segmentation Quality Metrics

Once you have a predicted segmentation, you need to compare it with a reference annotation.

Important metrics include:

### Dice coefficient

[
\boxed{
Dice=
\frac{2|A\cap B|}
{|A|+|B|}
}
]

### Jaccard / IoU

[
\boxed{
IoU=
\frac{|A\cap B|}
{|A\cup B|}
}
]

These are extremely important in medical segmentation.

---

# 18.66 Dice Interpretation

Suppose:

```text
Ground truth
      +
Predicted mask
```

Perfect overlap:

[
Dice=1
]

No overlap:

[
Dice=0
]

Therefore:

[
\boxed{
0\le Dice\le1
}
]

with larger values generally indicating better overlap.

---

# 18.67 Jaccard / IoU

[
IoU=
\frac{TP}{TP+FP+FN}
]

where:

* TP = true positive
* FP = false positive
* FN = false negative

Perfect segmentation:

[
IoU=1
]

No overlap:

[
IoU=0
]

---

# 18.68 Dice vs IoU

They are related.

For sets:

[
Dice=
\frac{2|A\cap B|}
{|A|+|B|}
]

and:

[
IoU=
\frac{|A\cap B|}
{|A\cup B|}
]

Relationship:

[
\boxed{
Dice=
\frac{2IoU}{1+IoU}
}
]

and:

[
\boxed{
IoU=
\frac{Dice}{2-Dice}
}
]

---

# 18.69 Why Pixel Accuracy Can Mislead

Suppose:

```text
Image:
99% background
1% tumor
```

A model predicts:

```text
Everything = background
```

Accuracy could be:

[
99%
]

but tumor segmentation is:

[
0%
]

So:

[
\boxed{
Accuracy\ alone
\neq
good\ segmentation
}
]

This is particularly important for small lesions.

---

# 18.70 Sensitivity and Specificity

For medical segmentation/detection tasks, also consider:

[
Sensitivity=
\frac{TP}{TP+FN}
]

[
Specificity=
\frac{TN}{TN+FP}
]

Sensitivity asks:

> How much of the target did we detect?

Specificity asks:

> How much background did we correctly reject?

---

# 18.71 False Positive vs False Negative

### False Positive

Algorithm marks something as target when it isn't.

```text
Healthy tissue
 ↓
incorrectly labeled tumor
```

### False Negative

Algorithm misses actual target.

```text
Actual tumor
 ↓
not segmented
```

Clinical consequences can differ significantly.

---

# 18.72 Segmentation Uncertainty

A segmentation does not always have a clear answer.

Example:

```text
Boundary
  ↓
low contrast
  ↓
uncertain
```

Instead of only:

```text
0 / 1
```

a system may maintain:

```text
Probability
0.0 → 1.0
```

For example:

```text
0.95 → likely foreground
0.52 → uncertain
0.05 → likely background
```

This becomes particularly important in AI-based segmentation.

---

# 18.73 Interactive Segmentation

For a professional medical viewer, segmentation may be interactive.

Example:

```text
User clicks seed
      ↓
Region growing
      ↓
Preview
      ↓
User adjusts threshold
      ↓
Update mask
      ↓
Accept
```

This is often more practical than expecting a single automatic algorithm to work perfectly on every case.

---

# 18.74 Segmentation Controller Architecture

For your Qt/QML application:

```text id="n9t2q7"
QML
 │
 ├── Segmentation Tool
 │
 ├── Threshold Slider
 │
 ├── Seed Point
 │
 └── Mask Overlay
 │
 ▼
SegmentationController
 │
 ▼
SegmentationEngine
 │
 ├── Threshold
 ├── RegionGrowing
 ├── Watershed
 ├── KMeans
 ├── ActiveContour
 └── GraphCut
 │
 ▼
SegmentationResult
 │
 ├── Mask
 ├── LabelMap
 ├── Contours
 └── Metrics
```

---

# 18.75 Segmentation Result Object

A good architecture could represent:

```cpp
struct SegmentationResult
{
    Image mask;
    Image labelMap;

    std::vector<Contour> contours;

    double dice = 0.0;
    double iou = 0.0;
};
```

In production, you would likely separate computed metrics from the core image result rather than always embedding them directly.

---

# 18.76 Why Label Maps Matter

Suppose:

```text
0 = background
1 = liver
2 = kidney
3 = tumor
```

This is much more useful than a simple RGB visualization.

The label map is machine-readable.

Then visualization can convert:

```text
Label
 ↓
Color overlay
```

This keeps data separate from presentation.

---

# 18.77 Segmentation Data Flow

```text
DICOM
 ↓
Image Volume
 ↓
Segmentation Algorithm
 ↓
Label Map
 ↓
Post-processing
 ↓
Contour / Surface
 ↓
Visualization
```

For 3D segmentation:

```text
Label Map
 ↓
Marching Cubes / Surface Extraction
 ↓
3D Mesh
 ↓
VTK
 ↓
3D Visualization
```

This is where your ITK + VTK architecture becomes especially useful.

---

# 18.78 Classical vs AI Segmentation

| Feature        | Classical        | AI                         |
| -------------- | ---------------- | -------------------------- |
| Threshold      | Yes              | Usually learned indirectly |
| Region growing | Yes              | Not typical                |
| Watershed      | Yes              | Not typical                |
| Active contour | Yes              | Can be combined            |
| Graph methods  | Yes              | Sometimes                  |
| Training data  | Not usually      | Required                   |
| Generalization | Limited by rules | Depends on training        |
| Explainability | Often easier     | More complex               |
| Compute        | Often lower      | Can be high                |

---

# 18.79 Enterprise Medical Segmentation

A mature system shouldn't be:

```text
One segmentation algorithm
```

Instead:

```text
Segmentation Framework
        │
 ┌──────┼───────────────┐
 ↓      ↓               ↓
Classical Interactive    AI
 │        │              │
Threshold Region       U-Net
Watershed  Growing      nnU-Net
Graph      Contour      etc.
```

The user/task determines which method is appropriate.

---

# 18.80 Important Medical Imaging Principle

A segmentation algorithm should not simply produce a mask.

An enterprise system should track:

```text
Input series
Algorithm
Parameters
Version
Timestamp
Result
Validation
```

This is important for reproducibility and medical-device software workflows.

---

# 18.81 Chapter 18 Mental Model

Memorize:

```text
                     SEGMENTATION
                           │
          ┌────────────────┼─────────────────┐
          ↓                ↓                 ↓
      THRESHOLDING       REGIONS          BOUNDARIES
          │                │                 │
      ┌───┼───┐       ┌────┼────┐      ┌─────┴─────┐
      ↓   ↓   ↓       ↓    ↓    ↓      ↓           ↓
 Global Adaptive Otsu Growing Split/Merge Watershed Active
                                               Contour
          │
          ↓
    Connected Components
          │
          ↓
      Morphology
          │
          ↓
       Final Mask
```

---

# 18.82 Key Formulas

### Thresholding

[
\boxed{
g(x,y)=
\begin{cases}
1,&f(x,y)\ge T\
0,&f(x,y)<T
\end{cases}
}
]

### Otsu

[
\boxed{
t^*=\arg\min_t\sigma_w^2(t)
}
]

or:

[
\boxed{
t^*=\arg\max_t\sigma_b^2(t)
}
]

### K-means

[
\boxed{
J=
\sum_{k=1}^{K}
\sum_{x_i\in C_k}
|x_i-\mu_k|^2
}
]

### Dice

[
\boxed{
Dice=
\frac{2|A\cap B|}
{|A|+|B|}
}
]

### IoU

[
\boxed{
IoU=
\frac{|A\cap B|}
{|A\cup B|}
}
]

### Sensitivity

[
\boxed{
Sensitivity=
\frac{TP}{TP+FN}
}
]

### Specificity

[
\boxed{
Specificity=
\frac{TN}{TN+FP}
}
]

---

# 18.83 Knowledge Check

### Fundamentals

1. What is image segmentation?
2. What is the difference between segmentation and classification?
3. What is semantic segmentation?
4. What is instance segmentation?
5. What is a label map?
6. What is a binary mask?

### Thresholding

7. What is thresholding?
8. What is global thresholding?
9. What is adaptive thresholding?
10. When does global thresholding work well?
11. When does global thresholding fail?
12. What is Otsu's method?
13. What assumption often makes Otsu useful?

### Region methods

14. What is region growing?
15. What is a seed?
16. What is region leakage?
17. What is region splitting?
18. What is region merging?
19. What is split-and-merge segmentation?

### Watershed

20. What is watershed segmentation?
21. What is a catchment basin?
22. Why does watershed suffer from over-segmentation?
23. How do markers help?

### Other algorithms

24. What is connected-component labeling?
25. Difference between 4- and 8-connectivity?
26. What is K-means segmentation?
27. Why must (K) be chosen?
28. What are active contours?
29. What is a level set?
30. What is graph-cut segmentation?

### Medical imaging

31. Why is thresholding useful for some CT structures?
32. Why is MRI more difficult to threshold universally?
33. Why is X-ray segmentation complicated by projection?
34. Why is morphology useful after segmentation?
35. Why is pixel accuracy insufficient for small lesions?
36. What are Dice and IoU?
37. What are false positives and false negatives?
38. Why is segmentation uncertainty important?
39. Why should segmentation parameters and algorithm versions be recorded?

---

# 18.84 Practical Exercise — Thresholding

Given:

```text
50  55  60  200 210
45  50  65  205 215
40  55  70  195 220
```

Use:

[
T=100
]

Create the binary mask.

Then answer:

> Would thresholding work well if the right side gradually changed from 100 to 180?

---

# 18.85 Practical Exercise — Otsu

Consider a histogram with two strong peaks:

```text
Background       Object
   ███             ████
   ███             ████
   ███             ████
─────── valley ─────────
```

Explain why Otsu is likely to work well.

Then explain why it may fail if the histogram becomes:

```text
████████████████
```

with no clear separation.

---

# 18.86 Practical Exercise — Region Growing

Start with seed:

```text
100
```

and threshold:

[
|I-seed|<20
]

For:

```text
100 105 110 125
102 108 115 130
100 104 109 140
```

determine which pixels can be reached from the seed assuming 4-connectivity.

Pay attention to the difference between:

```text
pixel is similar
```

and:

```text
pixel is reachable through similar pixels
```

---

# 18.87 Medical Imaging Exercise

Design a classical CT bone segmentation pipeline:

```text
CT
 ↓
DICOM rescale
 ↓
HU image
 ↓
Threshold
 ↓
Binary mask
 ↓
Morphological cleanup
 ↓
Connected components
 ↓
Bone mask
```

For each stage explain:

1. Input
2. Output
3. Why it is needed
4. Possible failure

---

# 18.88 Medical Imaging Exercise — Tumor

Suppose you have:

```text
MRI
+
low-contrast tumor
+
intensity variation
+
noise
```

Would you immediately choose:

> Global thresholding?

Explain why or why not.

A stronger answer should consider:

```text
Preprocessing
 ↓
Intensity normalization
 ↓
Region/edge information
 ↓
Interactive or AI-assisted segmentation
 ↓
Morphological refinement
 ↓
Validation
```

rather than assuming one threshold is sufficient.

---

# 18.89 Chapter 18 Complete

The most important concept is:

[
\boxed{
Segmentation
============

Assign\ pixels/voxels\ to\ meaningful\ regions
}
]

The classical segmentation toolbox is:

```text
Thresholding
      ↓
Region methods
      ↓
Watershed
      ↓
Connected components
      ↓
Clustering
      ↓
Active contours
      ↓
Level sets
      ↓
Graph methods
      ↓
Morphological refinement
```

For medical imaging:

```text
Image
 ↓
Preprocessing
 ↓
Segmentation
 ↓
Post-processing
 ↓
Validation
 ↓
Mask / Label Map
 ↓
Visualization / Measurement
```

And remember:

[
\boxed{
A\ visually\ clean\ segmentation
\neq
a\ clinically\ correct\ segmentation
}
]

**Chapter 18 complete.**

### Next, strictly according to your index:

# Chapter 19 — Image Registration

Topics:

* What is image registration?
* Fixed vs moving image
* Registration pipeline
* Rigid registration
* Affine registration
* Deformable registration
* Translation
* Rotation
* Scaling
* Shearing
* Transformation matrices
* Similarity measures
* SSD
* NCC
* Mutual information
* Feature-based registration
* Intensity-based registration
* Multi-resolution registration
* Medical-image registration
* CT–MRI registration
* Intra-patient vs inter-patient registration
