# Chapter 15 — Edge Detection

We continue **strictly according to your index**.

Chapter 15 topics:

1. Edge concept
2. Edge types
3. Gradient
4. First derivative
5. Second derivative
6. Sobel
7. Prewitt
8. Roberts
9. Laplacian
10. Laplacian of Gaussian
11. Canny edge detector
12. Non-maximum suppression
13. Hysteresis thresholding

---

# 15.1 What Is an Edge?

An **edge** is a location in an image where intensity changes significantly over a relatively small spatial distance.

Example:

```text
100 100 100 100 250 250 250 250
                  ↑
                EDGE
```

The intensity changes from:

[
100\rightarrow250
]

So there is a strong edge.

---

# 15.2 Why Are Edges Important?

Edges often correspond to meaningful boundaries:

```text
Bone
  │
  └── Soft tissue boundary

Organ
  │
  └── Surrounding tissue

Object
  │
  └── Background
```

Edges are therefore useful for:

* segmentation
* object detection
* shape analysis
* registration
* measurement
* feature extraction
* contour detection

In medical imaging, edges can correspond to anatomical boundaries, but not every anatomical boundary is equally strong or clinically meaningful.

---

# 15.3 Edge vs Region

Consider:

```text
100 100 100 100
100 100 100 100
100 100 100 100
```

This is a uniform region.

There is little local change.

Now:

```text
100 100 100 250
100 100 100 250
100 100 100 250
```

There is a boundary between:

```text
100 | 250
    ↑
   edge
```

So:

```text
Region
 ↓
slow variation

Edge
 ↓
rapid variation
```

---

# 15.4 Edge Types

Common idealized edge profiles include:

1. Step edge
2. Ramp edge
3. Roof edge
4. Line edge

---

# 15.5 Step Edge

A step edge changes abruptly.

```text
100 100 100 100 | 250 250 250 250
                 ↑
                edge
```

Mathematically:

```text
Low ─────────┐
             │
             └──────── High
```

This is the simplest edge model.

---

# 15.6 Ramp Edge

A ramp changes gradually.

```text
100 100 130 160 190 220 250 250
```

Instead of:

```text
100 100 100 250 250 250
```

The transition is spread over several pixels.

This is common in real images because of:

* blur
* acquisition physics
* finite resolution
* partial volume
* motion

---

# 15.7 Roof Edge

A roof edge forms a peak:

```text
100 100 150 200 150 100 100
```

Conceptually:

```text
       /\
      /  \
_____/    \_____
```

---

# 15.8 Line Edge

A narrow line:

```text
100 100 100 200 100 100 100
```

contains a bright narrow structure.

Depending on the image, a line may represent:

* vessel
* wire
* fine structure
* boundary artifact

---

# 15.9 Edge Detection as Differentiation

The key idea is:

[
\boxed{
\text{Edge}
\approx
\text{rapid intensity change}
}
]

Derivatives measure intensity change.

Therefore:

[
\boxed{
\text{Edge detection}
\rightarrow
\text{Derivative calculation}
}
]

---

# 15.10 First Derivative

For a 1D signal:

[
f(x)
]

the first derivative is:

[
\boxed{
\frac{df}{dx}
}
]

For a constant region:

[
f(x)=100
]

there is no change:

[
\frac{df}{dx}=0
]

At an edge:

```text
100 100 100 250 250 250
```

the derivative becomes large near the transition.

---

# 15.11 First Derivative Edge Concept

```text
Intensity:

      _________
     /
____/
   ↑
 edge


First derivative:

      /\
     /  \
____/    \____
      ↑
 strong response
```

So the first derivative produces a strong response around the edge.

---

# 15.12 Second Derivative

The second derivative is:

[
\boxed{
\frac{d^2f}{dx^2}
}
]

It measures how the gradient itself changes.

The second derivative often produces a positive/negative response around an edge.

Conceptually:

```text
First derivative:

       /\
      /  \
_____/    \_____


Second derivative:

       /\
      /  \
_____/    \_____
      ↑
 zero crossing
```

The exact polarity depends on edge orientation and sign convention.

---

# 15.13 First vs Second Derivative

| Property          | First derivative | Second derivative   |
| ----------------- | ---------------- | ------------------- |
| Measures          | Intensity change | Change of gradient  |
| Main concept      | Gradient         | Curvature           |
| Edge response     | Strong magnitude | Often zero crossing |
| Noise sensitivity | High             | Higher              |
| Common operators  | Sobel, Prewitt   | Laplacian           |

---

# 15.14 Gradient

For a 2D image:

[
f(x,y)
]

the gradient is:

[
\boxed{
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x}\
\frac{\partial f}{\partial y}
\end{bmatrix}
}
]

Define:

[
G_x=\frac{\partial f}{\partial x}
]

and:

[
G_y=\frac{\partial f}{\partial y}
]

Then the gradient contains:

* magnitude
* direction

---

# 15.15 Gradient Magnitude

The gradient magnitude is:

[
\boxed{
|\nabla f|=
\sqrt{G_x^2+G_y^2}
}
]

A computationally cheaper approximation is sometimes:

[
|G_x|+|G_y|
]

The Euclidean magnitude is more mathematically exact.

---

# 15.16 Gradient Direction

The gradient direction is:

[
\boxed{
\theta=
\operatorname{atan2}(G_y,G_x)
}
]

Important:

> The gradient points in the direction of greatest intensity increase.

So:

```text
Dark → Bright
       ↑
gradient direction
```

---

# 15.17 Why Calculate Both (G_x) and (G_y)?

An edge can have any orientation.

Horizontal change:

```text
100 100 250 250
```

Vertical change:

```text
100
100
250
250
```

Diagonal change:

```text
100 100 250
100 250 250
250 250 250
```

Therefore we need both directions:

[
G_x
]

and:

[
G_y
]

---

# 15.18 Sobel Operator

The Sobel operator is one of the most commonly used gradient operators.

Sobel (x):

[
\boxed{
G_x=
\begin{bmatrix}
-1&0&1\
-2&0&2\
-1&0&1
\end{bmatrix}
}
]

Sobel (y):

[
\boxed{
G_y=
\begin{bmatrix}
-1&-2&-1\
0&0&0\
1&2&1
\end{bmatrix}
}
]

---

# 15.19 Why Does Sobel Have 2s?

The center row/column gets more weight.

For (G_x):

```text
-1  0  1
-2  0  2
-1  0  1
```

This gives some smoothing perpendicular to the derivative direction.

So Sobel isn't simply a basic difference operator.

---

# 15.20 Sobel Processing

Pipeline:

```text
Image
  │
  ├──────→ Sobel X → Gx
  │
  └──────→ Sobel Y → Gy
                    │
              ┌─────┴─────┐
              ↓           ↓
          Magnitude     Direction
```

Magnitude:

[
M=
\sqrt{G_x^2+G_y^2}
]

Direction:

[
\theta=
atan2(G_y,G_x)
]

---

# 15.21 Example of Sobel

Consider:

```text
100 100 100
100 100 100
250 250 250
```

There is a horizontal intensity transition.

Sobel (G_y) responds strongly because the intensity changes vertically.

Therefore:

```text
Gx → small
Gy → strong
```

and:

[
M\approx |G_y|
]

---

# 15.22 Prewitt Operator

Prewitt is similar to Sobel but uses equal weights in its smoothing direction.

Prewitt (x):

[
\boxed{
\begin{bmatrix}
-1&0&1\
-1&0&1\
-1&0&1
\end{bmatrix}
}
]

Prewitt (y):

[
\boxed{
\begin{bmatrix}
-1&-1&-1\
0&0&0\
1&1&1
\end{bmatrix}
}
]

---

# 15.23 Sobel vs Prewitt

| Property         | Sobel                   | Prewitt |
| ---------------- | ----------------------- | ------- |
| Derivative       | Yes                     | Yes     |
| Kernel           | 3×3                     | 3×3     |
| Smoothing        | Weighted                | Uniform |
| Center weighting | 2                       | 1       |
| Noise behavior   | Usually somewhat better | Simpler |
| Computation      | Similar                 | Similar |

Sobel is commonly preferred when slightly stronger smoothing is desirable.

---

# 15.24 Roberts Cross Operator

Roberts uses very small (2\times2) kernels.

One form:

[
\boxed{
G_x=
\begin{bmatrix}
1&0\
0&-1
\end{bmatrix}
}
]

and:

[
\boxed{
G_y=
\begin{bmatrix}
0&1\
-1&0
\end{bmatrix}
}
]

It detects diagonal gradients.

---

# 15.25 Roberts Advantages and Disadvantages

Advantages:

* very small
* computationally simple
* good localization

Disadvantages:

* highly sensitive to noise
* poor smoothing
* less robust than Sobel for many practical images

So:

```text
Roberts
 ↓
simple + fast
but
 ↓
noise sensitive
```

---

# 15.26 Laplacian

The Laplacian is:

[
\boxed{
\nabla^2 f
==========

\frac{\partial^2f}{\partial x^2}
+
\frac{\partial^2f}{\partial y^2}
}
]

A common discrete kernel:

[
\boxed{
\begin{bmatrix}
0&1&0\
1&-4&1\
0&1&0
\end{bmatrix}
}
]

Another sign convention is:

[
\begin{bmatrix}
0&-1&0\
-1&4&-1\
0&-1&0
\end{bmatrix}
]

---

# 15.27 Laplacian Properties

Unlike Sobel and Prewitt:

```text
Sobel / Prewitt
 ↓
First derivative
 ↓
Directional
```

Laplacian:

```text
Second derivative
 ↓
No single preferred direction
```

It responds to changes in all directions.

---

# 15.28 Laplacian and Zero Crossing

The second derivative often changes sign around an edge.

For example:

```text
Positive
   ↓
   /\ 
  /  \
 /    \
--------→ zero
       \
        \
         Negative
```

Therefore zero crossings can be used to locate edges.

---

# 15.29 Why Laplacian Is Sensitive to Noise

Remember:

```text
Noise
 ↓
High-frequency variation
```

The Laplacian is a second derivative.

Differentiation emphasizes high-frequency changes.

Therefore:

[
\boxed{
\text{Laplacian can strongly amplify noise}
}
]

A common solution is to smooth first.

---

# 15.30 Laplacian of Gaussian — LoG

The idea:

```text
Image
 ↓
Gaussian smoothing
 ↓
Laplacian
 ↓
Edges
```

This is:

[
\boxed{
LoG
===

\nabla^2(G*f)
}
]

Because differentiation and convolution can be related:

[
\nabla^2(G*f)
=============

(\nabla^2G)*f
]

So we can create a **Laplacian-of-Gaussian kernel** and convolve directly.

---

# 15.31 LoG Mental Model

```text
Noise
 ↓
Gaussian
 ↓
Reduced noise
 ↓
Laplacian
 ↓
Edge response
```

This is better than applying a Laplacian directly to a very noisy image.

---

# 15.32 Canny Edge Detector

Canny is a more complete edge-detection pipeline.

The classical Canny algorithm consists of several stages:

```text
Image
 ↓
Gaussian smoothing
 ↓
Gradient calculation
 ↓
Gradient magnitude + direction
 ↓
Non-maximum suppression
 ↓
Double threshold
 ↓
Hysteresis
 ↓
Final edges
```

This sequence is extremely important.

---

# 15.33 Why Canny?

Simple gradient filters can produce:

* thick edges
* noisy edges
* multiple responses around one edge
* weak unwanted edges

Canny attempts to produce:

```text
Thin
+
well-localized
+
connected
edges
```

under its design assumptions.

---

# 15.34 Canny Step 1 — Gaussian Smoothing

First:

[
I_s=G_\sigma*I
]

Why?

Because derivatives are sensitive to noise.

So:

```text
Noisy image
 ↓
Gaussian
 ↓
Cleaner image
```

The value of (\sigma) controls the scale of smoothing.

---

# 15.35 Canny Step 2 — Gradient

Calculate:

[
G_x
]

and:

[
G_y
]

Then:

[
M=
\sqrt{G_x^2+G_y^2}
]

and:

[
\theta=
atan2(G_y,G_x)
]

Now every pixel has:

```text
Magnitude
+
Direction
```

---

# 15.36 Canny Step 3 — Non-Maximum Suppression

Raw gradient magnitude often produces thick edges.

Example:

```text
  ███
 █████
  ███
```

We want:

```text
  █
  █
  █
```

Non-maximum suppression keeps only local maxima along the gradient direction.

---

# 15.37 Why "Non-Maximum"?

Suppose along the gradient direction:

```text
10  50  30
```

The center value:

[
50
]

is the maximum.

Keep it.

But:

```text
10  20  30
```

the center is not the maximum.

Suppress it.

So:

```text
Local maximum → keep
Otherwise      → suppress
```

---

# 15.38 Gradient Direction Quantization

For practical Canny implementations, gradient direction is often approximated into categories such as:

```text
0°
45°
90°
135°
```

Then non-maximum suppression compares the pixel with the two neighbors along the closest gradient direction.

Conceptually:

```text
Gradient direction
      ↓
Choose comparison direction
      ↓
Compare neighboring pixels
      ↓
Keep only local maximum
```

---

# 15.39 Canny Step 4 — Double Threshold

After non-maximum suppression, we still have:

* strong edges
* weak edges
* noise

Use two thresholds:

[
T_{high}
]

and:

[
T_{low}
]

Classify:

```text
Magnitude ≥ Thigh
       ↓
   Strong edge


Tlow ≤ Magnitude < Thigh
       ↓
   Weak edge


Magnitude < Tlow
       ↓
   Suppress
```

---

# 15.40 Why Two Thresholds?

A single threshold can be problematic.

If too high:

```text
Weak real edges disappear
```

If too low:

```text
Noise becomes edges
```

Two thresholds allow:

```text
Strong edge
+
potential weak continuation
```

to be handled differently.

---

# 15.41 Canny Step 5 — Hysteresis

This is the key connection mechanism.

A weak edge is kept **if it is connected to a strong edge** according to the algorithm's connectivity.

Conceptually:

```text
Strong ── Weak ── Weak
  │
  └── connected
       ↓
      KEEP
```

But:

```text
Weak ── Weak
```

with no connection to a strong edge may be discarded.

---

# 15.42 Hysteresis Mental Model

```text
Strong edge
     │
     ├── weak neighbor → KEEP
     │
     └── weak neighbor → KEEP
              │
              └── continuation
```

But isolated weak responses:

```text
Weak
 ↓
No strong connection
 ↓
REMOVE
```

This helps reduce fragmented/noisy edges.

---

# 15.43 Canny Complete Pipeline

Memorize:

[
\boxed{
\text{Gaussian}
\rightarrow
\text{Gradient}
\rightarrow
\text{NMS}
\rightarrow
\text{Double Threshold}
\rightarrow
\text{Hysteresis}
}
]

Expanded:

```text
Image
  ↓
Gaussian smoothing
  ↓
Sobel / gradient
  ↓
Magnitude + direction
  ↓
Non-maximum suppression
  ↓
High / Low threshold
  ↓
Hysteresis
  ↓
Final thin edges
```

---

# 15.44 Sobel vs Canny

Sobel:

```text
Image
 ↓
Gradient
 ↓
Edge strength
```

Canny:

```text
Image
 ↓
Smooth
 ↓
Gradient
 ↓
Thin
 ↓
Threshold
 ↓
Connect
 ↓
Edges
```

Therefore:

[
\boxed{
Canny
=====

\text{multi-stage edge detector}
}
]

while Sobel is primarily a gradient operator.

---

# 15.45 Edge Detector Comparison

| Detector  | Derivative              | Noise Handling         | Edge Thickness | Complexity |
| --------- | ----------------------- | ---------------------- | -------------- | ---------- |
| Roberts   | First                   | Poor                   | Often thin     | Low        |
| Prewitt   | First                   | Moderate               | Moderate       | Low        |
| Sobel     | First                   | Better                 | Moderate       | Low        |
| Laplacian | Second                  | Poor without smoothing | Variable       | Low        |
| LoG       | Second + smoothing      | Better                 | Good           | Moderate   |
| Canny     | First + multiple stages | Good                   | Thin           | Higher     |

---

# 15.46 Why Canny Is Often Better

Canny addresses several problems at once:

### Noise

Gaussian smoothing.

### Thick edges

Non-maximum suppression.

### Weak edges

Double threshold.

### Broken edges

Hysteresis.

So:

```text
Simple derivative
      ↓
Raw edge response

Canny
      ↓
Processed edge map
```

---

# 15.47 Edge Localization

An important edge-detector goal is localization.

Suppose the true boundary is here:

```text
100 100 | 250 250
        ↑
      true edge
```

A good detector should place the detected edge close to that location.

Excessive smoothing can shift or weaken the edge.

Therefore:

[
\boxed{
Noise reduction
\leftrightarrow
Edge localization
}
]

is a trade-off.

---

# 15.48 Edge Detection and Scale

Consider a tiny structure:

```text
100 100 200 100 100
```

and a broad transition:

```text
100 120 140 160 180 200
```

The filter scale affects what is considered an edge.

Small Gaussian (\sigma):

```text
Fine structures preserved
```

Large (\sigma):

```text
Small structures may disappear
```

This leads to the concept of **scale-space**, which becomes important later.

---

# 15.49 Medical Imaging — Edge Detection

In CT:

```text
Bone ↔ Soft tissue
```

may produce strong edges.

In MRI:

```text
Tissue boundaries
```

may have varying contrast.

In X-ray:

```text
Bone boundaries
```

can be strong.

But:

> Not every edge represents an anatomical boundary.

Edges can also result from:

* noise
* acquisition artifacts
* metal
* reconstruction effects
* motion
* partial-volume effects

---

# 15.50 Partial Volume Effect

Suppose a voxel contains multiple tissues.

Instead of:

```text
100 | 200
```

you may observe:

```text
100 120 145 170 200
```

The boundary becomes a gradual ramp.

Therefore:

```text
Physical boundary
      ↓
Imaging system
      ↓
Blurred/ramp transition
      ↓
Edge detector
```

This is why real medical-image edges are not always ideal step edges.

---

# 15.51 Edge Detection Should Not Modify Original Data

For a medical viewer:

```text
DICOM pixel data
       │
       ├──────────────→ preserve
       │
       ↓
Edge detection
       ↓
Overlay / derived image
```

A good architecture treats edge detection as a derived visualization or analysis result.

For example:

```text
Original CT
     +
Edge Overlay
     ↓
Viewer
```

rather than permanently changing the original pixels.

---

# 15.52 Edge Overlay

A useful medical viewer workflow:

```text
Original image
      │
      ├── Window/Level
      │
      └── Edge detector
               ↓
          Edge mask
               ↓
          Transparent overlay
```

This lets the user compare:

```text
Original anatomy
+
Detected boundaries
```

---

# 15.53 C++ Conceptual Interface

```cpp
class IEdgeDetector
{
public:
    virtual ~IEdgeDetector() = default;

    virtual Image detect(
        const Image& input) = 0;
};
```

Implementations:

```text
IEdgeDetector
      │
      ├── SobelEdgeDetector
      ├── PrewittEdgeDetector
      ├── RobertsEdgeDetector
      ├── LaplacianEdgeDetector
      ├── LoGEdgeDetector
      └── CannyEdgeDetector
```

---

# 15.54 Canny Parameters

A production implementation should expose parameters explicitly.

For example:

```cpp
struct CannyParameters
{
    double sigma;
    double lowThreshold;
    double highThreshold;
};
```

Potential additional parameters:

```text
gradient operator
border mode
kernel size
L2 gradient flag
connectivity
```

The exact options depend on the implementation/library.

---

# 15.55 Threshold Selection

Threshold selection is important.

If:

[
T_{high}
]

is too high:

```text
Weak anatomical boundaries
 ↓
Lost
```

If:

[
T_{low}
]

is too low:

```text
Noise
 ↓
Potential edge candidates
```

So thresholds must be chosen relative to:

* image intensity characteristics
* noise level
* modality
* preprocessing
* intended task

---

# 15.56 Sobel Example

Consider:

```text
100 100 100
100 100 100
200 200 200
```

There is a horizontal boundary.

Sobel (G_x):

[
\begin{bmatrix}
-1&0&1\
-2&0&2\
-1&0&1
\end{bmatrix}
]

produces approximately zero because there is no left-right variation.

Sobel (G_y):

[
\begin{bmatrix}
-1&-2&-1\
0&0&0\
1&2&1
\end{bmatrix}
]

produces a strong response.

Therefore:

```text
Gx ≈ 0
Gy = strong
```

---

# 15.57 Direction Interpretation

If:

[
G_x
]

is strong:

```text
Intensity changes horizontally
```

If:

[
G_y
]

is strong:

```text
Intensity changes vertically
```

The **edge orientation itself is perpendicular to the gradient direction**.

This is an important distinction.

Example:

```text
Gradient →
────────────
edge orientation
```

The gradient points across the edge, not along it.

---

# 15.58 Edge Orientation vs Gradient Direction

Suppose the edge is vertical:

```text
100 | 250
    ↑
 vertical edge
```

The intensity changes horizontally.

Therefore:

```text
Gradient → horizontal
Edge     → vertical
```

So:

[
\boxed{
\text{Gradient direction}
\perp
\text{Edge direction}
}
]

---

# 15.59 First Derivative vs Canny

A first derivative operator can identify where intensity changes.

But it doesn't automatically solve:

```text
Noise
Thickness
Weak edges
Connectivity
```

Canny adds processing stages for those problems.

Therefore:

```text
Derivative
 ↓
basic edge information

Canny
 ↓
structured edge map
```

---

# 15.60 Edge Detection and Segmentation

Edge detection can be an input to segmentation:

```text
Image
 ↓
Edge detection
 ↓
Boundary information
 ↓
Segmentation
```

But edge detection alone is not segmentation.

A segmentation algorithm determines regions/classes.

So:

[
\boxed{
Edge\ Detection
\neq
Segmentation
}
]

---

# 15.61 Edge Detection and Registration

Edges can also be useful for registration.

Conceptually:

```text
CT image A
 ↓
Edges

CT image B
 ↓
Edges

     ↓

Compare structures
     ↓
Estimate alignment
```

This can reduce dependence on absolute intensity in some registration strategies.

---

# 15.62 Edge Detection in 3D

For volumetric images, edge detection can operate in 3D.

Gradient becomes:

[
\boxed{
\nabla f=
\begin{bmatrix}
f_x\
f_y\
f_z
\end{bmatrix}
}
]

Magnitude:

[
\boxed{
|\nabla f|=
\sqrt{
f_x^2+f_y^2+f_z^2
}
}
]

This is useful for:

* CT volumes
* MRI
* 3D surface extraction
* volumetric segmentation

Again, voxel spacing must be considered.

---

# 15.63 3D Medical Imaging Consideration

Suppose:

```text
Spacing:
0.5 × 0.5 × 5 mm
```

Then raw derivatives along (x,y,z) cannot simply be treated as physically equivalent.

Physical gradients should account for spacing:

[
\frac{\partial f}{\partial x}
]

versus:

[
\frac{\partial f}{\partial z}
]

with different physical distances.

Therefore:

[
\boxed{
\text{Voxel spacing matters for quantitative gradients.}
}
]

---

# 15.64 Performance

Edge detection is highly parallelizable.

For Sobel:

```text
Each pixel
 ↓
Local neighborhood
 ↓
Gx + Gy
```

Many pixels can be processed independently.

Therefore:

```text
CPU
 ├── SIMD
 └── multithreading

GPU
 └── massively parallel convolution
```

can accelerate large medical volumes.

---

# 15.65 Chapter 15 Mental Model

Memorize this:

```text
                         EDGE DETECTION
                               │
                ┌──────────────┴──────────────┐
                ↓                             ↓
          FIRST DERIVATIVE              SECOND DERIVATIVE
                │                             │
       ┌────────┼────────┐              ┌─────┴─────┐
       ↓        ↓        ↓              ↓           ↓
    Sobel    Prewitt  Roberts       Laplacian      LoG
       │
       ↓
    Gradient
       │
   ┌───┴────┐
   ↓        ↓
Magnitude Direction
   │
   ↓
  CANNY
   │
   ├── Gaussian
   ├── Gradient
   ├── NMS
   ├── Double Threshold
   └── Hysteresis
```

---

# 15.66 Key Formulas

### Gradient

[
\boxed{
\nabla f=
\begin{bmatrix}
f_x\
f_y
\end{bmatrix}
}
]

### Magnitude

[
\boxed{
M=
\sqrt{G_x^2+G_y^2}
}
]

### Direction

[
\boxed{
\theta=
atan2(G_y,G_x)
}
]

### Laplacian

[
\boxed{
\nabla^2f=f_{xx}+f_{yy}
}
]

### LoG

[
\boxed{
LoG=
\nabla^2(G*f)
}
]

### Canny

[
\boxed{
Gaussian
\rightarrow
Gradient
\rightarrow
NMS
\rightarrow
Double\ Threshold
\rightarrow
Hysteresis
}
]

---

# 15.67 Knowledge Check

### Fundamentals

1. What is an edge?
2. Why are edges important in image processing?
3. What is a step edge?
4. What is a ramp edge?
5. What is a line edge?

### Gradient

6. What is a first derivative?
7. What is a gradient?
8. What are (G_x) and (G_y)?
9. How do you calculate gradient magnitude?
10. How do you calculate gradient direction?
11. Why is the gradient perpendicular to the edge?

### Operators

12. What is Sobel?
13. Why does Sobel use weights of 2?
14. What is Prewitt?
15. What is Roberts?
16. Compare Sobel, Prewitt, and Roberts.
17. What is the Laplacian?
18. Why is the Laplacian sensitive to noise?

### LoG

19. What is Laplacian of Gaussian?
20. Why smooth before calculating a second derivative?

### Canny

21. List every stage of Canny.
22. Why does Canny use Gaussian smoothing?
23. What is non-maximum suppression?
24. Why are two thresholds used?
25. What is hysteresis?
26. Why does hysteresis help connect edges?

### Medical imaging

27. Why can an anatomical boundary appear as a ramp rather than a step?
28. Why does voxel spacing matter for 3D gradients?
29. Why should detected edges normally be treated as derived information?
30. Why can edge enhancement create misleading structures?

---

# 15.68 Practical Exercise — Sobel

Given:

```text
100 100 100
100 100 100
200 200 200
```

Calculate:

[
G_x
]

using:

[
\begin{bmatrix}
-1&0&1\
-2&0&2\
-1&0&1
\end{bmatrix}
]

Then calculate:

[
G_y
]

using:

[
\begin{bmatrix}
-1&-2&-1\
0&0&0\
1&2&1
\end{bmatrix}
]

Finally calculate:

[
M=\sqrt{G_x^2+G_y^2}
]

and determine the gradient direction.

---

# 15.69 Practical Exercise — Canny

Take this conceptual pipeline:

```text
Noisy CT
   ↓
Gaussian σ = 1
   ↓
Sobel
   ↓
Magnitude
   ↓
Non-maximum suppression
   ↓
Low threshold
High threshold
   ↓
Hysteresis
   ↓
Edge map
```

For each stage, answer:

**What problem is this stage solving?**

That question is more important than memorizing the names.

---

# 15.70 Medical Imaging Exercise

Imagine a CT image containing:

```text
Large bone boundary
+
small vessel
+
noise
```

Compare:

### Sobel

### Laplacian

### LoG

### Canny

For each, explain:

* noise sensitivity
* edge thickness
* localization
* small-structure behavior
* computational cost
* whether you would use it for visualization or quantitative analysis

---

# 15.71 Chapter 15 Complete

The most important relationship is:

[
\boxed{
\text{Edge}
\rightarrow
\text{Intensity Change}
\rightarrow
\text{Derivative}
\rightarrow
\text{Gradient}
}
]

Then:

```text
Gradient
   ↓
Magnitude + Direction
   ↓
Sobel / Prewitt
```

For more sophisticated detection:

```text
Image
 ↓
Gaussian
 ↓
Gradient
 ↓
Non-Maximum Suppression
 ↓
Double Threshold
 ↓
Hysteresis
 ↓
Canny Edge Map
```

And remember:

[
\boxed{
\text{Gradient direction}
\perp
\text{edge direction}
}
]

**Chapter 15 complete.**

### Next, strictly according to your index:

# Chapter 16 — Image Restoration & Noise

Topics:

* Image degradation model
* Noise model
* Gaussian noise
* Salt-and-pepper noise
* Poisson noise
* Speckle noise
* Uniform noise
* Rayleigh noise
* Periodic noise
* Restoration vs enhancement
* Mean filters
* Median filters
* Adaptive filtering
* Wiener filtering
* Inverse filtering
* Deconvolution
* PSF
* MSE / PSNR
* SSIM
