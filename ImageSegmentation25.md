# Chapter 25 — Image Segmentation

We continue **strictly according to your index**.

Image segmentation is one of the most important topics for your planned **CT/MRI/PET/X-ray medical image viewer**, because segmentation converts an image into meaningful regions such as:

```text
CT
 ↓
Liver
Kidney
Tumor
Bone
Lung
```

The central idea is:

[
\boxed{
Segmentation = partition\ an\ image\ into\ meaningful\ regions
}
]

---

# 25.1 What Is Image Segmentation?

Suppose an image contains:

```text
Background
Liver
Tumor
```

Segmentation attempts to assign each pixel/voxel to a region or label.

For a 2D image:

[
I(x,y)
]

we can create:

[
L(x,y)
]

where (L) is a label map.

Example:

```text
0 → Background
1 → Liver
2 → Tumor
3 → Kidney
```

Therefore:

[
\boxed{
Image \rightarrow Label\ Map
}
]

---

# 25.2 Segmentation Output

Original:

```text
┌──────────────────┐
│                  │
│      Liver       │
│        ○         │
│      Tumor       │
│                  │
└──────────────────┘
```

Segmentation:

```text
0 0 0 0 0 0 0
0 1 1 1 1 1 0
0 1 1 2 1 1 0
0 1 1 1 1 1 0
0 0 0 0 0 0 0
```

where:

```text
0 = background
1 = liver
2 = tumor
```

---

# 25.3 Pixel Segmentation vs Voxel Segmentation

For 2D:

[
\boxed{
Pixel\ segmentation
}
]

For 3D medical images:

[
\boxed{
Voxel\ segmentation
}
]

A CT volume can be represented as:

[
I(x,y,z)
]

and the segmentation becomes:

[
L(x,y,z)
]

---

# 25.4 2D Segmentation

Example:

```text
CT Slice
   ↓
2D Segmentation
   ↓
Mask
```

Each slice is processed independently.

Advantages:

* simpler
* lower memory requirement
* easier implementation

Disadvantages:

* may ignore information from neighboring slices
* segmentation can become inconsistent across slices

---

# 25.5 3D Segmentation

Instead:

```text
CT Volume
 ↓
3D Segmentation
 ↓
3D Label Volume
```

The algorithm considers:

[
x,y,z
]

together.

This can produce more spatially consistent results.

---

# 25.6 Segmentation vs Classification

These are different.

### Classification

Answers:

> What is in this image?

Example:

```text
Image → Pneumonia
```

### Segmentation

Answers:

> Which pixels/voxels belong to the object?

```text
Image
 ↓
Tumor mask
```

---

# 25.7 Segmentation vs Detection

### Detection

Finds:

```text
Object location
```

Usually through:

```text
Bounding box
```

Example:

```text
┌────────────────────┐
│                    │
│   ┌──────────┐     │
│   │  Tumor   │     │
│   └──────────┘     │
│                    │
└────────────────────┘
```

### Segmentation

Finds the actual shape:

```text
████████
████████
 ██████
  ████
```

---

# 25.8 Three Tasks

| Task           | Output                |
| -------------- | --------------------- |
| Classification | Class                 |
| Detection      | Location/bounding box |
| Segmentation   | Pixel/voxel mask      |

---

# 25.9 Binary Segmentation

Only two classes:

```text
0 → Background
1 → Object
```

Example:

```text
Background
     +
Tumor
```

Mathematically:

[
L(x,y)\in{0,1}
]

---

# 25.10 Multiclass Segmentation

Multiple classes:

[
L(x,y)\in
{0,1,2,3,\ldots,N}
]

Example:

```text
0 → Background
1 → Liver
2 → Kidney
3 → Tumor
4 → Bone
```

---

# 25.11 Multilabel Segmentation

In some systems, different binary masks can overlap.

For example:

```text
Mask 1 → Organ
Mask 2 → Tumor
Mask 3 → Vessel
```

Unlike a mutually exclusive multiclass label map, multilabel representations can allow multiple labels at a location depending on the task.

---

# 25.12 Thresholding

Thresholding is one of the simplest segmentation methods.

Given:

[
I(x,y)
]

choose threshold (T):

[
\boxed{
L(x,y)=
\begin{cases}
1 & I(x,y)>T\
0 & I(x,y)\le T
\end{cases}
}
]

---

# 25.13 Thresholding Example

Suppose:

```text
20  25  30
100 150 180
200 220 240
```

Take:

[
T=100
]

Then:

```text
0 0 0
0 1 1
1 1 1
```

---

# 25.14 Why Thresholding Works

It works well when:

```text
Object intensity
```

is sufficiently different from:

```text
Background intensity
```

Example:

```text
Background → 0–50
Object     → 150–255
```

A threshold around 100 can separate them easily.

---

# 25.15 Thresholding Failure

If intensity distributions overlap:

```text
Background → 50–150
Object     → 100–200
```

then one threshold cannot cleanly separate them.

This is common in medical images.

---

# 25.16 Global Thresholding

One threshold is applied to the entire image:

[
\boxed{
T=\text{constant}
}
]

Pipeline:

```text
Image
 ↓
One T
 ↓
Binary Mask
```

Simple and fast.

---

# 25.17 Adaptive Thresholding

Instead of one threshold, threshold depends on local image characteristics:

[
\boxed{
T=T(x,y)
}
]

Conceptually:

```text
Image
 ↓
Local neighborhoods
 ↓
Local thresholds
 ↓
Mask
```

Useful when illumination/intensity varies spatially.

In medical imaging, similar concepts can be useful where local intensity distributions vary, although modality-specific preprocessing is often important.

---

# 25.18 Otsu Thresholding

Otsu's method automatically chooses a threshold by maximizing separation between two classes, often described as maximizing between-class variance.

Conceptually:

```text
Histogram
    ↓
Evaluate candidate T
    ↓
Find best separation
    ↓
Optimal T
```

---

# 25.19 Otsu Intuition

Suppose histogram:

```text
Frequency
  ↑
  │    ███
  │   █████
  │  ██████        █████
  │ ███████       ███████
  └────────────────────────→ Intensity
             ↑
             T
```

Otsu tries to find a threshold between the two dominant groups.

---

# 25.20 Otsu Concept

For candidate threshold (t):

[
\sigma_B^2(t)
]

represents between-class variance.

Otsu chooses:

[
\boxed{
t^*=\arg\max_t \sigma_B^2(t)
}
]

This is useful when the histogram has a reasonably separable bimodal structure.

---

# 25.21 Region-Based Segmentation

Instead of focusing only on individual pixels, region-based methods group neighboring pixels with similar properties.

Examples:

```text
Region Growing
Region Splitting
Region Merging
```

---

# 25.22 Region Growing

Start with one or more seed points.

```text
Seed
 ↓
Check neighbors
 ↓
Similar?
 ↓
Add to region
 ↓
Continue
```

---

# 25.23 Region Growing Example

```text
       ●
      /|\
     / | \
    ●  ●  ●
       |
       ●
```

The center is the seed.

Neighboring pixels are added if they satisfy a similarity criterion.

---

# 25.24 Region Growing Algorithm

Conceptually:

```text
1. Choose seed
2. Initialize region
3. Examine neighboring pixels
4. Test similarity
5. Add suitable pixels
6. Continue until no suitable pixels remain
```

---

# 25.25 Similarity Criterion

Could be based on:

```text
Intensity
Color
Texture
Gradient
Distance
```

For example:

[
|I(p)-I(seed)|<T
]

---

# 25.26 Region Growing Problem

Results depend heavily on:

```text
Seed selection
Threshold
Connectivity
Noise
Intensity variation
```

Therefore robust medical segmentation may require preprocessing and carefully designed criteria.

---

# 25.27 Connectivity

For 2D images:

### 4-connectivity

Neighbors:

```text
  N
W P E
  S
```

### 8-connectivity

Includes diagonals:

```text
NW N NE
 W P E
SW S SE
```

---

# 25.28 3D Connectivity

In 3D, common neighborhoods include:

```text
6-connectivity
18-connectivity
26-connectivity
```

The choice affects connected-component and region-growing results.

---

# 25.29 Region Splitting

Start with a large region.

```text
Entire image
     ↓
Does region satisfy homogeneity?
     ↓
No
     ↓
Split
```

Continue recursively.

---

# 25.30 Region Merging

Start with smaller regions.

```text
Small regions
 ↓
Check similarity
 ↓
Merge compatible regions
```

---

# 25.31 Splitting and Merging

These can be combined:

```text
Image
 ↓
Split
 ↓
Small regions
 ↓
Merge similar regions
 ↓
Final segmentation
```

---

# 25.32 Edge-Based Segmentation

Edges represent strong intensity transitions.

Example:

```text
████████░░░░░░
```

The boundary:

```text
████|░░░░
    ↑
   Edge
```

can help identify object boundaries.

---

# 25.33 Gradient

The gradient measures intensity change:

[
\boxed{
\nabla I=
\left[
\frac{\partial I}{\partial x},
\frac{\partial I}{\partial y}
\right]
}
]

Magnitude:

[
\boxed{
|\nabla I|=
\sqrt{
I_x^2+I_y^2
}
}
]

Strong gradient:

```text
Strong edge
```

---

# 25.34 Edge Detection

Common operators:

```text
Sobel
Prewitt
Roberts
Canny
Laplacian
```

These can produce edge maps.

---

# 25.35 Edge Segmentation Problem

Edges do not always form closed boundaries.

Example:

```text
      ─────
     /
────
```

Gaps can make it difficult to identify complete regions.

Medical images can also contain:

* weak boundaries
* noise
* partial volume effects
* overlapping structures

---

# 25.36 Watershed

Watershed treats an image as a topographic surface.

Imagine:

```text
High
 /\        /\
/  \______/  \
      ↓
    valley
```

Conceptually:

```text
Bright → mountains
Dark   → valleys
```

Flooding the valleys produces watershed boundaries.

---

# 25.37 Watershed Intuition

```text
       /\        /\
      /  \______/  \
_____/              \____
        ↑
     watershed
```

The boundary between growing regions becomes a segmentation boundary.

---

# 25.38 Watershed Problem

A raw gradient image can produce:

[
\boxed{
Over-segmentation
}
]

Too many local minima can produce too many regions.

---

# 25.39 Marker-Controlled Watershed

A better approach:

```text
Image
 ↓
Markers
 ↓
Watershed
 ↓
Controlled regions
```

Markers tell the algorithm which regions are meaningful.

This is often much more useful than unrestricted watershed.

---

# 25.40 K-Means Segmentation

K-means clusters pixels based on feature similarity.

Suppose:

[
K=3
]

Then:

```text
Pixels
 ↓
3 clusters
 ↓
Cluster 1
Cluster 2
Cluster 3
```

---

# 25.41 K-Means Algorithm

Basic procedure:

```text
1. Choose K
2. Initialize centroids
3. Assign pixels to nearest centroid
4. Recalculate centroids
5. Repeat
```

Until convergence or a stopping condition.

---

# 25.42 K-Means Distance

For a scalar intensity:

[
d(x,c)=|x-c|
]

For a feature vector:

[
\boxed{
d(x,c)=\sqrt{\sum_i(x_i-c_i)^2}
}
]

---

# 25.43 K-Means Medical Example

Suppose intensity clusters roughly into:

```text
Low intensity
Medium intensity
High intensity
```

K-means may separate:

```text
Air
Soft tissue
Bone
```

in some CT contexts.

However, real CT segmentation often requires much more domain knowledge than simple clustering.

---

# 25.44 Active Contours

Active contours are also called snakes.

A contour evolves toward an object boundary.

```text
Initial contour
      ↓
Iterative evolution
      ↓
Object boundary
```

---

# 25.45 Active Contour Energy

Conceptually:

[
\boxed{
E=
E_{internal}
+
E_{external}
}
]

Internal energy controls the shape/smoothness.

External energy attracts the contour toward image features.

---

# 25.46 Internal Energy

Controls properties such as:

```text
Smoothness
Continuity
Curvature
```

Without constraints, a contour could become unstable or irregular.

---

# 25.47 External Energy

Can come from:

```text
Edges
Gradients
Intensity
Region statistics
```

The contour is attracted toward image structures.

---

# 25.48 Active Contour Example

```text
Initial:

   ______
 /        \
|          |
 \________/

       ↓

Final:

    ______
  /        \
 /  Tumor   \
 \          /
  \________/
```

---

# 25.49 Level Sets

Level-set methods represent a contour implicitly using a function:

[
\phi(x,y)
]

The contour is often represented by:

[
\boxed{
\phi(x,y)=0
}
]

This allows contours to naturally split or merge.

---

# 25.50 Why Level Sets Are Powerful

Explicit contour representation:

```text
List of boundary points
```

can be difficult when topology changes.

Level sets can naturally handle:

```text
One region
 ↓
splits into two
```

or:

```text
Two regions
 ↓
merge into one
```

---

# 25.51 Graph Cuts

Graph-cut segmentation represents the image as a graph.

```text
Pixels / voxels
      ↓
Graph nodes
      ↓
Edges represent relationships
```

Then segmentation can be formulated as a minimum-cut problem.

---

# 25.52 Graph Structure

Conceptually:

```text
     ●──●──●
     │  │  │
     ●──●──●
     │  │  │
     ●──●──●
```

Each node represents a pixel/voxel.

Edges encode relationships or costs.

---

# 25.53 Source and Sink

A common binary graph-cut formulation uses:

```text
Source
  ↓
Object

Sink
  ↓
Background
```

The algorithm finds a cut separating the two.

---

# 25.54 Graph Cuts Energy

A common formulation is:

[
\boxed{
E(L)=
E_{data}(L)+
\lambda E_{smooth}(L)
}
]

where:

* (E_{data}) = how well labels fit the image
* (E_{smooth}) = spatial consistency
* (\lambda) = tradeoff parameter

---

# 25.55 Morphological Segmentation

Mathematical morphology operates on shapes using a structuring element.

Important operations:

```text
Erosion
Dilation
Opening
Closing
```

These can be used to clean segmentation masks.

---

# 25.56 Binary Mask

Suppose segmentation produces:

```text
00000000
00111100
01111110
00111100
00010000
```

Morphological operations can remove:

* small noise
* holes
* isolated components

---

# 25.57 Erosion

Erosion generally shrinks foreground regions.

Conceptually:

```text
Before:

██████
██████
██████

After:

 ████
 ████
```

It can remove small structures.

---

# 25.58 Dilation

Dilation expands foreground regions.

```text
Before:

  ██
  ██

After:

 ████
██████
 ████
```

It can fill small gaps and connect nearby structures.

---

# 25.59 Opening

Opening:

[
\boxed{
Opening = Erosion \rightarrow Dilation
}
]

Useful for removing small foreground objects/noise while preserving larger structures.

---

# 25.60 Closing

Closing:

[
\boxed{
Closing = Dilation \rightarrow Erosion
}
]

Useful for:

* filling small holes
* connecting nearby regions
* closing small gaps

---

# 25.61 Connected Components

After segmentation, we may have:

```text
Object 1
Object 2
Object 3
```

Connected-component analysis assigns separate IDs.

Example:

```text
000000
011000
011000
000220
000220
```

where:

```text
1 = component 1
2 = component 2
```

---

# 25.62 Connected Components Pipeline

```text
Binary Mask
 ↓
Connected Components
 ↓
Component Labels
 ↓
Measure:
 ├── Area
 ├── Bounding box
 ├── Centroid
 └── Shape
```

This is extremely useful after segmentation.

---

# 25.63 Removing Small Components

Suppose:

```text
Component 1 → 50,000 pixels
Component 2 → 2 pixels
Component 3 → 3 pixels
```

If the task expects one large structure:

```text
Keep Component 1
Remove tiny components
```

This is often called area filtering or size filtering.

---

# 25.64 Segmentation Post-Processing

Typical pipeline:

```text
Raw segmentation
      ↓
Morphological cleanup
      ↓
Connected components
      ↓
Remove unwanted components
      ↓
Fill holes
      ↓
Smooth boundary
      ↓
Final mask
```

---

# 25.65 CT Segmentation

CT is particularly useful for intensity-based segmentation because CT values are related to attenuation and commonly expressed as Hounsfield Units after appropriate rescaling.

Example concept:

```text
HU
 ↓
Threshold
 ↓
Bone mask
```

Bone can often be separated from soft tissue using intensity ranges, but robust clinical segmentation usually requires additional processing.

---

# 25.66 CT Bone Segmentation

Simplified:

```text
CT
 ↓
HU conversion
 ↓
Bone threshold
 ↓
Binary mask
 ↓
Morphology
 ↓
Connected components
 ↓
Bone segmentation
```

---

# 25.67 Lung Segmentation

A simplified conceptual pipeline:

```text
CT
 ↓
Intensity-based thresholding
 ↓
Candidate lung regions
 ↓
Connected components
 ↓
Morphological cleanup
 ↓
Lung mask
```

Real clinical-grade lung segmentation is more complex.

---

# 25.68 Liver Segmentation

Liver boundaries can be difficult because liver and nearby soft tissues may have similar intensities.

Therefore:

```text
Thresholding alone
```

may be insufficient.

Possible methods:

```text
Region growing
Graph methods
Active contours
Atlas-based methods
Deep learning
```

---

# 25.69 Tumor Segmentation

Tumors are particularly difficult because they can have:

* variable intensity
* irregular boundaries
* heterogeneous texture
* low contrast
* partial-volume effects

Therefore tumor segmentation often requires advanced methods.

---

# 25.70 MRI Segmentation

MRI has multiple contrasts:

```text
T1
T2
FLAIR
DWI
ADC
```

A structure may appear very different across sequences.

Therefore segmentation can use:

```text
Single sequence
```

or:

```text
Multi-modal information
```

---

# 25.71 PET Segmentation

PET often contains functional information such as tracer uptake.

Segmentation may use:

```text
SUV
Thresholding
Region methods
CT/PET combination
AI
```

A fixed threshold is not universally appropriate for all lesions or clinical tasks.

---

# 25.72 Multimodal Segmentation

Example:

```text
CT
+
PET
 ↓
Segmentation
```

CT provides anatomical information.

PET provides functional/metabolic information.

Combined information can improve segmentation in appropriate applications.

---

# 25.73 Segmentation Mask

A segmentation mask should generally be represented separately from the original image:

```text
Image:
I(x,y,z)

Mask:
M(x,y,z)
```

Do not overwrite:

[
I
]

with:

[
M
]

---

# 25.74 Label Map

For multiclass segmentation:

[
M(x,y,z)\in
{0,1,\ldots,N}
]

Example:

```text
0 → Background
1 → Liver
2 → Kidney
3 → Tumor
```

---

# 25.75 Mask vs Label Map

### Binary mask

```text
0 / 1
```

### Label map

```text
0 / 1 / 2 / 3 / ...
```

### Probability map

```text
0.0 → 0%
1.0 → 100%
```

These are different representations.

---

# 25.76 Probability Map

AI segmentation often produces:

[
P(class|x)
]

Example:

```text
Voxel:
Tumor probability = 0.92
```

Then a threshold might produce:

```text
P > 0.5
 ↓
Tumor
```

But the threshold should be selected according to the task and validation.

---

# 25.77 AI Segmentation

Modern segmentation commonly uses deep learning.

Pipeline:

```text
Medical Image
      ↓
Preprocessing
      ↓
Neural Network
      ↓
Probability Map
      ↓
Post-processing
      ↓
Segmentation Mask
```

Examples of architectures include:

```text
U-Net
3D U-Net
V-Net
nnU-Net
Transformer-based models
```

---

# 25.78 U-Net

U-Net has an encoder-decoder structure.

```text
Input
  ↓
Encoder
  ↓
Bottleneck
  ↓
Decoder
  ↓
Segmentation
```

Skip connections help preserve spatial information.

---

# 25.79 AI Segmentation Is Still Segmentation

Traditional:

```text
Image
 ↓
Threshold
 ↓
Mask
```

AI:

```text
Image
 ↓
Neural Network
 ↓
Probability
 ↓
Mask
```

The final concept remains:

[
\boxed{
Image \rightarrow Region\ labels
}
]

---

# 25.80 Segmentation Evaluation

Important metrics:

```text
Dice
IoU
Sensitivity
Specificity
Precision
Recall
Hausdorff distance
```

---

# 25.81 Dice Similarity Coefficient

Dice:

[
\boxed{
Dice=
\frac{2|A\cap B|}
{|A|+|B|}
}
]

where:

* (A) = predicted segmentation
* (B) = reference segmentation

Range:

[
0\leq Dice\leq1
]

---

# 25.82 Dice Interpretation

[
Dice=1
]

means perfect overlap.

[
Dice=0
]

means no overlap.

Example:

```text
Prediction
██████

Reference
██████
```

→ high Dice.

---

# 25.83 IoU

Intersection over Union:

[
\boxed{
IoU=
\frac{|A\cap B|}
{|A\cup B|}
}
]

Also called the Jaccard index.

---

# 25.84 Dice vs IoU

Relationship:

[
\boxed{
Dice=\frac{2IoU}{1+IoU}
}
]

and:

[
\boxed{
IoU=\frac{Dice}{2-Dice}
}
]

Both measure overlap but use different formulas.

---

# 25.85 Boundary Metrics

Overlap alone may not fully describe boundary quality.

For medical structures, boundary distance can be important.

One metric is:

[
\boxed{
Hausdorff\ Distance
}
]

It measures the maximum distance between corresponding sets under a chosen definition.

---

# 25.86 Why Boundary Accuracy Matters

Suppose:

```text
Prediction:
  ███████
 █████████
  ███████

Reference:
 █████████
███████████
 █████████
```

The overlap might still be high, but boundary differences may matter for:

* radiotherapy
* surgical planning
* organ measurements

---

# 25.87 Segmentation in TPS

This is particularly relevant to your medical-domain work.

A TPS may use:

```text
CT
 ↓
Structure segmentation
 ↓
Contours
 ↓
3D structures
 ↓
Dose calculation
```

Examples:

```text
PTV
CTV
GTV
Spinal cord
Lung
Heart
Kidneys
```

Segmentation/contouring is therefore a foundational TPS operation.

---

# 25.88 Contours vs Masks

A contour is a boundary representation:

```text
     ______
   /        \
  |          |
   \________/
```

A mask represents the interior:

```text
  █████████
 ███████████
 ███████████
  █████████
```

Both can represent the same anatomical structure.

---

# 25.89 Contour → Mask

A contour can be rasterized into a mask:

```text
Contour
 ↓
Rasterization
 ↓
Binary mask
```

---

# 25.90 Mask → Contour

Conversely:

```text
Mask
 ↓
Boundary extraction
 ↓
Contour
```

Possible algorithms include:

```text
Marching Squares
Marching Cubes
```

for 2D and 3D representations respectively.

---

# 25.91 3D Segmentation to Surface

For a 3D mask:

```text
Voxel mask
 ↓
Surface extraction
 ↓
3D mesh
```

A common algorithm:

[
\boxed{
Marching\ Cubes
}
]

This is highly relevant to VTK-based visualization.

---

# 25.92 Segmentation Architecture for Your Application

```text
                    IMAGE
                      │
                      ↓
               Preprocessing
                      │
             ┌────────┴────────┐
             ↓                 ↓
       Traditional            AI
             │                 │
       ┌─────┼──────┐          │
       ↓     ↓      ↓          ↓
 Threshold Region  Edge      Model
          Growing Based        │
       │     │      │          │
       └─────┼──────┘          │
             ↓                 ↓
              └───────┬────────┘
                      ↓
                  Mask/Labels
                      ↓
                Post-processing
                      ↓
              Connected Components
                      ↓
                 Measurements
                      ↓
                 Visualization
```

---

# 25.93 Enterprise Segmentation Engine

A clean architecture:

```cpp
class ISegmentationAlgorithm
{
public:
    virtual ~ISegmentationAlgorithm() = default;

    virtual SegmentationResult segment(
        const Image& image) = 0;
};
```

Implementations:

```text
ThresholdSegmentation
RegionGrowingSegmentation
WatershedSegmentation
GraphCutSegmentation
ActiveContourSegmentation
AISegmentation
```

---

# 25.94 SegmentationResult

A useful result object could contain:

```cpp
struct SegmentationResult
{
    LabelMap labelMap;
    std::vector<Region> regions;
    std::vector<Contour> contours;
};
```

Potential metadata:

```text
Label
Name
Color
Opacity
Volume
Area
Centroid
Bounding box
```

---

# 25.95 QML Integration

Your QML viewer can expose:

```text
Segmentation
 ├── Create Mask
 ├── Threshold
 ├── Region Growing
 ├── AI
 ├── Show Contour
 ├── Fill
 ├── Color
 ├── Opacity
 ├── Visibility
 └── Statistics
```

Architecture:

```text
QML
 ↓
SegmentationController
 ↓
SegmentationEngine
 ↓
ITK / OpenCV / AI
 ↓
Mask / LabelMap
 ↓
Overlay
 ↓
QML / VTK
```

---

# 25.96 Segmentation + Color LUT

After segmentation:

```text
Label Map
 ↓
Label LUT
 ↓
RGBA
 ↓
Alpha Blend
 ↓
Viewer
```

Example:

```text
0 → transparent
1 → green
2 → red
3 → blue
```

This connects directly with **Chapter 22 — Image Color Processing**.

---

# 25.97 Segmentation + Restoration

A typical pipeline might be:

```text
DICOM
 ↓
Decode
 ↓
Optional validated preprocessing
 ↓
Restoration / denoising
 ↓
Segmentation
 ↓
Post-processing
 ↓
Visualization
```

But processing order must be chosen based on the segmentation algorithm and validated use case.

Do not assume that every segmentation pipeline should denoise first.

---

# 25.98 Segmentation + Registration

For multi-series medical imaging:

```text
CT
+
MRI
 ↓
Registration
 ↓
Aligned images
 ↓
Segmentation
```

or:

```text
Segmentation
 ↓
Registration
 ↓
Transfer structure
```

The correct ordering depends on the application.

---

# 25.99 Segmentation + Dose

In radiation therapy:

```text
CT
 ↓
Segmentation / Contouring
 ↓
Structures
 ↓
Dose Calculation
 ↓
Dose Distribution
 ↓
Dose-Volume Analysis
```

This is one reason segmentation is a fundamental component of your TPS learning path.

---

# 25.100 Chapter 25 Mental Model

```text
                    SEGMENTATION
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
   Thresholding     Region-based      Edge-based
        │                │                │
    ┌───┼───┐       ┌────┼────┐          │
    ↓   ↓   ↓       ↓    ↓    ↓          ↓
 Global Otsu Adaptive Growing Split/Merge Watershed
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
      K-Means        Active Contour     Graph Cut
        │                │                │
        └────────────────┼────────────────┘
                         ↓
                    Label / Mask
                         ↓
                  Post-processing
                         ↓
              Connected Components
                         ↓
                   Measurements
                         ↓
                 Contour / Surface
                         ↓
                Color + Visualization
                         ↓
                  Medical Viewer
```

---

# 25.101 Most Important Formulas

### Binary threshold

[
\boxed{
L(x,y)=
\begin{cases}
1,&I(x,y)>T\
0,&I(x,y)\leq T
\end{cases}
}
]

### Gradient magnitude

[
\boxed{
|\nabla I|=
\sqrt{I_x^2+I_y^2}
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

### Dice/IoU relationship

[
\boxed{
Dice=\frac{2IoU}{1+IoU}
}
]

---

# 25.102 Important Interview Questions

### Fundamentals

1. What is image segmentation?
2. What is the difference between segmentation and classification?
3. Segmentation vs detection?
4. What is binary segmentation?
5. What is multiclass segmentation?
6. What is a label map?
7. What is a segmentation mask?

### Traditional Segmentation

8. What is thresholding?
9. Global vs adaptive thresholding?
10. What is Otsu thresholding?
11. What is region growing?
12. What is region splitting?
13. What is region merging?
14. What is edge-based segmentation?
15. What is watershed?
16. Why does watershed over-segment?
17. What is marker-controlled watershed?
18. What is K-means segmentation?

### Advanced

19. What is an active contour?
20. What is a level set?
21. What is graph-cut segmentation?
22. What is an energy function?
23. Why is regularization useful?
24. What is connected-component analysis?
25. What is morphological post-processing?

### Medical Imaging

26. Why is CT suitable for some intensity-based segmentation?
27. Why can liver segmentation be difficult?
28. Why is tumor segmentation challenging?
29. Why can MRI segmentation use multiple sequences?
30. What is PET segmentation?
31. Why is 3D segmentation important?
32. What is the difference between a contour and a mask?
33. How can a mask be converted to a 3D surface?
34. What is Marching Cubes?
35. What is Dice?
36. What is IoU?
37. Why is boundary accuracy important?
38. How can segmentation be used in a TPS?

---

# 25.103 Practical Exercise — Thresholding

Given:

```text
10  20  30  40
50  60 100 120
150 170 180 200
220 230 240 250
```

Use:

[
T=100
]

Create the binary segmentation mask.

---

# 25.104 Practical Exercise — Dice

Prediction:

```text
A = {1,2,3,4,5}
```

Reference:

```text
B = {3,4,5,6,7}
```

Calculate:

[
|A\cap B|
]

[
|A|
]

[
|B|
]

and finally:

[
Dice
]

---

# 25.105 Practical Exercise — Medical Segmentation

Design a simplified **CT bone segmentation** pipeline:

```text
DICOM CT
 ↓
Decode
 ↓
HU conversion
 ↓
Bone threshold
 ↓
Binary mask
 ↓
Morphological cleanup
 ↓
Connected components
 ↓
3D surface extraction
 ↓
VTK visualization
```

Identify which component should handle each step:

```text
DCMTK
ITK
OpenCV
VTK
Qt/C++
QML
```

---

# 25.106 Chapter 25 Complete

The core concept is:

[
\boxed{
Image\rightarrow Regions\rightarrow Labels
}
]

Traditional methods:

[
\boxed{
Thresholding
}
]

[
\boxed{
Region\ Growing
}
]

[
\boxed{
Watershed
}
]

[
\boxed{
K\text{-}means
}
]

[
\boxed{
Active\ Contours
}
]

[
\boxed{
Graph\ Cuts
}
]

and modern AI:

[
\boxed{
Image\rightarrow Neural\ Network\rightarrow Probability\ Map\rightarrow Mask
}
]

For your medical imaging application, remember this architecture:

```text
DICOM
 ↓
Decoded Image
 ↓
Segmentation Engine
 ├── Threshold
 ├── Region Growing
 ├── Watershed
 ├── Graph Cut
 ├── Active Contour
 └── AI
 ↓
Mask / Label Map
 ↓
Post Processing
 ↓
Contours / 3D Surface
 ↓
Color LUT
 ↓
Alpha Overlay
 ↓
QML / VTK Viewer
```

**Chapter 25 complete.**
