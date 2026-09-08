# Module 2 → Chapter 21: Gradient

The **gradient** is one of the most important mathematical concepts in:

* Image processing
* Computer vision
* Medical imaging
* Edge detection
* Image registration
* Image segmentation
* Optimization
* Machine learning

---

# 1. What Is a Gradient?

For a function with multiple variables:

$$
f(x,y)
$$

the gradient combines its partial derivatives into a vector:

$$
\boxed{
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x}\\
\frac{\partial f}{\partial y}
\end{bmatrix}
}
$$

The symbol:

$$
\nabla
$$

is called **nabla** or **del operator**.

---

# 2. Simple Meaning

The gradient tells us:

* The direction of maximum increase of a function.
* How rapidly the function increases in that direction.

```text
Function Surface
       ↑
       │ Maximum increase
       │
       → Gradient direction
```

---

# 3. Gradient of a Simple Function

Suppose:

$$
f(x,y)=x^2+y^2
$$

First calculate partial derivatives:

$$
\frac{\partial f}{\partial x}=2x
$$

$$
\frac{\partial f}{\partial y}=2y
$$

Therefore:

$$
\boxed{
\nabla f=
\begin{bmatrix}
2x\\
2y
\end{bmatrix}
}
$$

---

# 4. Gradient at a Point

Suppose:

$$
f(x,y)=x^2+y^2
$$

Find the gradient at:

$$
(2,3)
$$

We have:

$$
\nabla f=
\begin{bmatrix}
2x\\
2y
\end{bmatrix}
$$

Substitute:

$$
\nabla f(2,3)=
\begin{bmatrix}
4\\
6
\end{bmatrix}
$$

Therefore:

$$
\boxed{
\nabla f(2,3)=
\begin{bmatrix}
4\\
6
\end{bmatrix}
}
$$

---

# 5. Gradient Magnitude

The magnitude of a gradient is:

$$
|\nabla f|
=
\sqrt{
\left(
\frac{\partial f}{\partial x}
\right)^2
+
\left(
\frac{\partial f}{\partial y}
\right)^2
}
$$

This represents the strength of the rate of change.

---

# 6. Gradient Direction

The direction is:

$$
\theta=
\tan^{-1}
\left(
\frac{f_y}{f_x}
\right)
$$

More robustly in computation:

$$
\theta=\operatorname{atan2}(f_y,f_x)
$$

Where:

$$
f_x=\frac{\partial f}{\partial x}
$$

and:

$$
f_y=\frac{\partial f}{\partial y}
$$

---

# 7. Gradient in an Image

A grayscale image can be represented as:

$$
I(x,y)
$$

The image gradient is:

$$
\boxed{
\nabla I=
\begin{bmatrix}
I_x\\
I_y
\end{bmatrix}
=
\begin{bmatrix}
\frac{\partial I}{\partial x}\\
\frac{\partial I}{\partial y}
\end{bmatrix}
}
$$

Where:

* \(I_x\) = intensity change in the \(x\)-direction
* \(I_y\) = intensity change in the \(y\)-direction

---

# 8. Why Is Gradient Important in Images?

Consider:

```text
Dark Region        Bright Region

██████████│░░░░░░░░░░
          ↑
         Edge
```

At a smooth region:

$$
|\nabla I|
$$

is generally small.

At a strong intensity transition:

$$
|\nabla I|
$$

is generally large.

Therefore:

```text
Large Gradient
      ↓
Strong Intensity Change
      ↓
Possible Edge
```

---

# 9. Gradient Magnitude in Images

The standard magnitude is:

$$
\boxed{
|\nabla I|
=
\sqrt{I_x^2+I_y^2}
}
$$

A faster approximation sometimes used is:

$$
|\nabla I|
\approx
|I_x|+|I_y|
$$

---

# 10. Gradient Direction in Images

The gradient direction is:

$$
\boxed{
\theta=
\operatorname{atan2}(I_y,I_x)
}
$$

This tells us the direction of maximum intensity increase.

**Important:** The gradient direction is perpendicular to the local edge direction.

---

# 11. Example

Suppose:

$$
I_x=6
$$

and:

$$
I_y=8
$$

Magnitude:

$$
|\nabla I|
=
\sqrt{6^2+8^2}
$$

$$
=
\sqrt{36+64}
$$

$$
=
\boxed{10}
$$

Direction:

$$
\theta=
\operatorname{atan2}(8,6)
$$

Approximately:

$$
\boxed{53.13^\circ}
$$

---

# 12. Discrete Image Gradient

Digital images consist of pixels, so derivatives are approximated.

A simple horizontal derivative:

$$
I_x(x,y)
\approx
I(x+1,y)-I(x,y)
$$

A simple vertical derivative:

$$
I_y(x,y)
\approx
I(x,y+1)-I(x,y)
$$

---

# 13. Central Difference Gradient

A more balanced approximation is:

$$
I_x
\approx
\frac{I(x+1,y)-I(x-1,y)}{2}
$$

$$
I_y
\approx
\frac{I(x,y+1)-I(x,y-1)}{2}
$$

---

# 14. Example Using Pixels

Suppose the horizontal neighborhood is:

```text
15   25   35
```

At the center:

$$
I_x=
\frac{35-15}{2}
$$

$$
\boxed{I_x=10}
$$

Suppose the vertical neighborhood is:

```text
20
25
30
```

Then:

$$
I_y=
\frac{30-20}{2}
$$

$$
\boxed{I_y=5}
$$

Gradient magnitude:

$$
|\nabla I|
=
\sqrt{10^2+5^2}
$$

$$
=
\sqrt{125}
$$

$$
\approx
\boxed{11.18}
$$

---

# 15. Gradient Operators

Different filters estimate image derivatives.

Important operators include:

```text
Gradient Operators
│
├── Roberts
├── Prewitt
├── Sobel
└── Scharr
```

---

# 16. Roberts Operator

The Roberts operator uses small \(2\times2\) kernels.

One common form is:

$$
G_x=
\begin{bmatrix}
1&0\\
0&-1
\end{bmatrix}
$$

$$
G_y=
\begin{bmatrix}
0&1\\
-1&0
\end{bmatrix}
$$

It is simple and computationally small, but can be sensitive to noise.

---

# 17. Prewitt Operator

The Prewitt operator commonly uses:

$$
G_x=
\begin{bmatrix}
-1&0&1\\
-1&0&1\\
-1&0&1
\end{bmatrix}
$$

$$
G_y=
\begin{bmatrix}
-1&-1&-1\\
0&0&0\\
1&1&1
\end{bmatrix}
$$

---

# 18. Sobel Operator

The Sobel operator gives greater weight to central neighboring pixels.

A common \(x\)-direction kernel:

$$
G_x=
\begin{bmatrix}
-1&0&1\\
-2&0&2\\
-1&0&1
\end{bmatrix}
$$

A common \(y\)-direction kernel:

$$
G_y=
\begin{bmatrix}
-1&-2&-1\\
0&0&0\\
1&2&1
\end{bmatrix}
$$

---

# 19. Why Sobel Is Useful?

The Sobel operator:

* Estimates intensity changes.
* Provides directional information.
* Includes some local smoothing effect due to weighted neighboring pixels.
* Is widely used for basic edge detection.

Process:

```text
Image
  ↓
Sobel Gx
  ↓
Horizontal Derivative

Image
  ↓
Sobel Gy
  ↓
Vertical Derivative

Gx + Gy
  ↓
Gradient Magnitude + Direction
```

---

# 20. Scharr Operator

The Scharr operator is another derivative filter designed to provide improved rotational symmetry compared with simple Sobel filters.

A common \(x\)-kernel is:

$$
G_x=
\begin{bmatrix}
-3&0&3\\
-10&0&10\\
-3&0&3
\end{bmatrix}
$$

A corresponding \(y\)-kernel is:

$$
G_y=
\begin{bmatrix}
-3&-10&-3\\
0&0&0\\
3&10&3
\end{bmatrix}
$$

---

# 21. Comparing Gradient Operators

| Operator |  Kernel Size | Main Characteristic             |
| -------- | -----------: | ------------------------------- |
| Roberts  | \(2\times2\) | Simple and small                |
| Prewitt  | \(3\times3\) | Uniform directional derivative  |
| Sobel    | \(3\times3\) | Weighted directional derivative |
| Scharr   | \(3\times3\) | Improved rotational behavior    |

---

# 22. Gradient and Edge Detection

The basic idea:

```text
Image
  ↓
Calculate Ix
  ↓
Calculate Iy
  ↓
Calculate Magnitude
  ↓
Find Large Values
  ↓
Possible Edges
```

Mathematically:

$$
G=
\sqrt{I_x^2+I_y^2}
$$

Then a threshold may be applied:

$$
G>T
$$

Where \(T\) is an edge threshold.

---

# 23. Threshold Example

Suppose:

$$
T=50
$$

Then:

```text
Gradient = 20 → No strong edge
Gradient = 80 → Strong edge candidate
```

Threshold selection depends on:

* Image contrast
* Noise
* Modality
* Desired sensitivity

---

# 24. Gradient Direction and Edge Orientation

Remember:

$$
\text{Gradient Direction}
\perp
\text{Edge Direction}
$$

Example:

```text
Vertical Edge
     │
     │
     │

Gradient →
```

The intensity changes across the edge, not along it.

---

# 25. Gradient in CT Images

A CT image may contain:

* Air
* Soft tissue
* Bone

Intensity transitions between structures can produce gradients.

```text
CT Image
   ↓
Compute Gradient
   ↓
Strong Intensity Transitions
   ↓
Potential Anatomical Boundaries
```

---

# 26. Gradient in MRI Images

MRI often contains soft-tissue structures with intensity differences.

Gradient information can support:

* Boundary analysis
* Segmentation
* Feature extraction
* Registration

However, gradients can also be affected by:

* Noise
* Intensity non-uniformity
* Acquisition differences

---

# 27. Gradient in Image Segmentation

Gradient information can help identify boundaries:

```text
Image
 ↓
Gradient Calculation
 ↓
Boundary Strength
 ↓
Segmentation Algorithm
```

Examples of approaches that use boundary or gradient information include:

* Active contours
* Watershed methods
* Edge-based segmentation

---

# 28. Gradient in Image Registration

Suppose we have:

```text
Fixed Image
      +
Moving Image
```

An optimization algorithm may minimize a similarity function:

$$
E(\theta)
$$

where \(\theta\) represents transformation parameters.

The gradient:

$$
\nabla E
$$

can help determine how to update parameters.

```text
Current Parameters
       ↓
Calculate Error
       ↓
Calculate Gradient
       ↓
Update Parameters
       ↓
Repeat
```

---

# 29. Gradient Descent Connection

For a function:

$$
f(x)
$$

the gradient points toward the direction of maximum increase.

Therefore, to minimize a function, gradient descent moves in the opposite direction:

$$
\boxed{
x_{new}
=
x_{old}
-
\alpha\nabla f(x)
}
$$

Where:

* \(\alpha\) = learning rate
* \(\nabla f(x)\) = gradient

---

# 30. 2D Gradient Descent

For:

$$
f(x,y)
$$

we have:

$$
\nabla f=
\begin{bmatrix}
f_x\\
f_y
\end{bmatrix}
$$

Update:

$$
x_{new}=x-\alpha f_x
$$

$$
y_{new}=y-\alpha f_y
$$

This concept is fundamental in:

* Optimization
* Machine learning
* Deep learning
* Image registration

---

# 31. Gradient Vector Field

For every point:

$$
(x,y)
$$

we can calculate a gradient vector.

```text
↑   ↗   →
↖   ↑   ↗
←   ↖   ↑
```

This collection is called a **gradient vector field**.

It describes how a function changes across space.

---

# 32. 3D Image Gradient

Medical images are often volumetric:

$$
I(x,y,z)
$$

The gradient becomes:

$$
\boxed{
\nabla I=
\begin{bmatrix}
\frac{\partial I}{\partial x}\\
\frac{\partial I}{\partial y}\\
\frac{\partial I}{\partial z}
\end{bmatrix}
}
$$

Gradient magnitude:

$$
|\nabla I|
=
\sqrt{
I_x^2+
I_y^2+
I_z^2
}
$$

This is important for:

* CT volumes
* MRI volumes
* PET volumes
* Radiation therapy imaging

---

# 33. Gradient in Medical Image Processing

Applications include:

### Edge Detection

Finding strong intensity transitions.

### Segmentation

Supporting boundary identification.

### Image Registration

Optimizing transformation parameters.

### Feature Extraction

Detecting local image structure.

### 3D Visualization

Gradient information can help estimate:

* Surface orientation
* Normals
* Local structure

---

# 34. Gradient and Noise

Derivatives amplify rapid intensity changes.

Unfortunately, noise also creates rapid intensity changes.

Therefore:

```text
Noisy Image
     ↓
Derivative
     ↓
Noise May Become Strong
```

A common approach is:

```text
Image
 ↓
Smoothing
 ↓
Gradient Calculation
 ↓
Edge Analysis
```

Gaussian smoothing is frequently used before derivative-based processing.

---

# 35. C++ Example: Gradient Magnitude

```cpp
#include <cmath>

double gradientMagnitude(double gx, double gy)
{
    return std::sqrt(gx * gx + gy * gy);
}
```

Example:

```cpp
double gx = 6.0;
double gy = 8.0;

double magnitude =
    gradientMagnitude(gx, gy);
```

Result:

$$
\sqrt{6^2+8^2}=10
$$

---

# 36. C++ Example: Gradient Direction

```cpp
#include <cmath>

double gradientDirection(double gx, double gy)
{
    return std::atan2(gy, gx);
}
```

The result is usually in radians.

Convert to degrees:

```cpp
double radiansToDegrees(double radians)
{
    const double pi = 3.141592653589793;

    return radians * 180.0 / pi;
}
```

---

# 37. Important Formulas

### Gradient

$$
\nabla f=
\begin{bmatrix}
f_x\\
f_y
\end{bmatrix}
$$

### Image Gradient

$$
\nabla I=
\begin{bmatrix}
I_x\\
I_y
\end{bmatrix}
$$

### Gradient Magnitude

$$
|\nabla I|
=
\sqrt{I_x^2+I_y^2}
$$

### Approximate Magnitude

$$
|\nabla I|
\approx
|I_x|+|I_y|
$$

### Gradient Direction

$$
\theta=
\operatorname{atan2}(I_y,I_x)
$$

### Gradient Descent

$$
x_{new}
=
x_{old}
-
\alpha\nabla f(x)
$$

---

# 38. Practice Questions

### Question 1

Given:

$$
f(x,y)=x^2+y^2
$$

Find:

$$
\nabla f
$$

**Answer:**

$$
\boxed{
\nabla f=
\begin{bmatrix}
2x\\
2y
\end{bmatrix}
}
$$

---

### Question 2

Given:

$$
I_x=3
$$

and:

$$
I_y=4
$$

Find gradient magnitude.

$$
\sqrt{3^2+4^2}
$$

$$
\boxed{5}
$$

---

### Question 3

What does a large gradient magnitude in an image usually indicate?

**Answer:** A strong local intensity change, which may correspond to an edge or boundary.

---

### Question 4

What is the relationship between gradient direction and local edge direction?

**Answer:** The gradient direction is perpendicular to the local edge direction.

---

### Question 5

Why is smoothing often applied before gradient calculation?

**Answer:** Because derivatives can amplify rapid changes caused by noise.

---

# 39. Common Mistakes

### Mistake 1: Confusing Gradient Direction with Edge Direction

The gradient points in the direction of maximum intensity increase.

The local edge direction is perpendicular to it.

---

### Mistake 2: Ignoring Both Components

A 2D gradient requires:

$$
I_x
$$

and:

$$
I_y
$$

---

### Mistake 3: Forgetting Noise

Gradient operators can respond strongly to noise.

---

### Mistake 4: Using \(\tan^{-1}(I_y/I_x)\) Directly in Code

This can lose quadrant information and can have issues when \(I_x=0\).

Prefer:

$$
\operatorname{atan2}(I_y,I_x)
$$

---

# 40. Chapter Summary

You learned:

* What a gradient is
* Gradient notation
* Gradient direction
* Gradient magnitude
* Image gradients
* Discrete gradients
* Forward and central differences
* Roberts operator
* Prewitt operator
* Sobel operator
* Scharr operator
* Edge detection using gradients
* Gradient direction vs edge direction
* Gradient thresholding
* CT and MRI applications
* Segmentation applications
* Image registration applications
* Gradient descent connection
* Gradient vector fields
* 3D image gradients
* Gradient and noise
* Gradient magnitude calculation
* Gradient direction calculation

---

## Current Progress

```text
Module 2 — Mathematics Foundation

✅ Chapter 11: Algebra Fundamentals
✅ Chapter 12: Functions
✅ Chapter 13: Trigonometry
✅ Chapter 14: Vectors
✅ Chapter 15: Matrices
✅ Chapter 16: Matrix Operations
✅ Chapter 17: Linear Transformations
✅ Chapter 18: Eigenvalues and Eigenvectors
✅ Chapter 19: Calculus Fundamentals
✅ Chapter 20: Partial Derivatives
✅ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 22 — Probability**
