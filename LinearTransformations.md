# Module 2 → Chapter 17: Linear Transformations

Linear transformations are extremely important in:

* Image processing
* Computer vision
* Medical image registration
* CT/MRI image geometry
* 2D and 3D graphics
* Image rotation and scaling
* Machine learning

They explain how **vectors and coordinates can be transformed using matrices**.

---

# 1. What Is a Transformation?

A transformation changes something.

For example, a point:

$$
P=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

can be:

```text
Scaled
Rotated
Reflected
Sheared
Translated
```

Example:

```text
Original Image
      ↓
Transformation
      ↓
New Image
```

---

# 2. What Is a Linear Transformation?

A linear transformation maps one vector to another while preserving two important properties.

If:

$$
T(\vec{u})
$$

is a transformation, then:

### Property 1: Addition

$$
T(\vec{u}+\vec{v})
=
T(\vec{u})+T(\vec{v})
$$

### Property 2: Scalar Multiplication

$$
T(c\vec{v})
=
cT(\vec{v})
$$

These properties define linearity.

---

# 3. Matrix Representation

A linear transformation can usually be represented as:

$$
\vec{v'}=A\vec{v}
$$

Where:

* \(A\) = transformation matrix
* \(\vec{v}\) = original vector
* \(\vec{v'}\) = transformed vector

---

# 4. Example

Suppose:

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

and:

$$
\vec{v}=
\begin{bmatrix}
2\\
1
\end{bmatrix}
$$

Then:

$$
A\vec{v}
=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
\begin{bmatrix}
2\\
1
\end{bmatrix}
$$

Calculate:

$$
=
\begin{bmatrix}
2(2)+0(1)\\
0(2)+3(1)
\end{bmatrix}
$$

Therefore:

$$
\vec{v'}=
\begin{bmatrix}
4\\
3
\end{bmatrix}
$$

---

# 5. Identity Transformation

The identity matrix:

$$
I=
\begin{bmatrix}
1&0\\
0&1
\end{bmatrix}
$$

Applying it:

$$
\vec{v'}=I\vec{v}
$$

gives:

$$
\vec{v'}=\vec{v}
$$

So:

```text
Identity Transformation
        ↓
No Change
```

---

# 6. Scaling Transformation

Scaling changes the size of an object.

General scaling matrix:

$$
S=
\begin{bmatrix}
S_x&0\\
0&S_y
\end{bmatrix}
$$

Example:

$$
S=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

For:

$$
P=
\begin{bmatrix}
2\\
1
\end{bmatrix}
$$

Result:

$$
P'=SP
$$

$$
=
\begin{bmatrix}
4\\
3
\end{bmatrix}
$$

This means:

```text
X direction → ×2
Y direction → ×3
```

---

# 7. Uniform vs Non-Uniform Scaling

## Uniform Scaling

$$
S_x=S_y
$$

Example:

$$
\begin{bmatrix}
2&0\\
0&2
\end{bmatrix}
$$

The object keeps its proportions.

## Non-Uniform Scaling

$$
S_x\neq S_y
$$

Example:

$$
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

The object stretches differently in each direction.

---

# 8. Rotation Transformation

Rotation changes direction around the origin.

For angle \(\theta\):

$$
R=
\begin{bmatrix}
\cos\theta&-\sin\theta\\
\sin\theta&\cos\theta
\end{bmatrix}
$$

This is one of the most important transformation matrices.

---

# 9. Rotation by 90°

For:

$$
\theta=90^\circ
$$

We know:

$$
\cos90^\circ=0
$$

$$
\sin90^\circ=1
$$

Therefore:

$$
R=
\begin{bmatrix}
0&-1\\
1&0
\end{bmatrix}
$$

Suppose:

$$
P=
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

Then:

$$
P'=RP
$$

$$
=
\begin{bmatrix}
0&-1\\
1&0
\end{bmatrix}
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

Result:

$$
P'=
\begin{bmatrix}
0\\
1
\end{bmatrix}
$$

So the point rotates 90° counterclockwise.

---

# 10. Reflection Transformation

Reflection flips an object.

## Reflection Across X-axis

$$
R_x=
\begin{bmatrix}
1&0\\
0&-1
\end{bmatrix}
$$

Transformation:

$$
(x,y)\rightarrow(x,-y)
$$

---

## Reflection Across Y-axis

$$
R_y=
\begin{bmatrix}
-1&0\\
0&1
\end{bmatrix}
$$

Transformation:

$$
(x,y)\rightarrow(-x,y)
$$

---

# 11. Shear Transformation

Shearing changes the shape by shifting coordinates.

## X-direction Shear

$$
H_x=
\begin{bmatrix}
1&k\\
0&1
\end{bmatrix}
$$

Transformation:

$$
x'=x+ky
$$

$$
y'=y
$$

---

## Y-direction Shear

$$
H_y=
\begin{bmatrix}
1&0\\
k&1
\end{bmatrix}
$$

Transformation:

$$
x'=x
$$

$$
y'=kx+y
$$

---

# 12. Translation

Translation moves an object.

Example:

$$
(x,y)
$$

moves by:

$$
(t_x,t_y)
$$

Then:

$$
x'=x+t_x
$$

$$
y'=y+t_y
$$

Important:

> Translation alone is not a linear transformation in ordinary Cartesian coordinates because it does not generally preserve the origin.

For example:

$$
T(0)\neq0
$$

Instead, translation is commonly handled using **homogeneous coordinates**.

---

# 13. Homogeneous Coordinates

A 2D point:

$$
(x,y)
$$

can be represented as:

$$
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$

Then translation becomes a matrix operation.

$$
T=
\begin{bmatrix}
1&0&t_x\\
0&1&t_y\\
0&0&1
\end{bmatrix}
$$

Then:

$$
P'=TP
$$

---

# 14. Translation Example

Suppose:

$$
P=
\begin{bmatrix}
2\\
3\\
1
\end{bmatrix}
$$

Translation:

$$
t_x=5
$$

$$
t_y=4
$$

Matrix:

$$
T=
\begin{bmatrix}
1&0&5\\
0&1&4\\
0&0&1
\end{bmatrix}
$$

Result:

$$
P'=TP
$$

$$
=
\begin{bmatrix}
7\\
7\\
1
\end{bmatrix}
$$

Therefore:

```text
(2,3)
  ↓
Translate by (5,4)
  ↓
(7,7)
```

---

# 15. Affine Transformation

An affine transformation can combine:

* Scaling
* Rotation
* Shearing
* Translation

A general 2D affine transformation:

$$
\begin{bmatrix}
x'\\
y'
\end{bmatrix}
=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
\begin{bmatrix}
x\\
y
\end{bmatrix}
+
\begin{bmatrix}
t_x\\
t_y
\end{bmatrix}
$$

Using homogeneous coordinates:

$$
\begin{bmatrix}
x'\\
y'\\
1
\end{bmatrix}
=
\begin{bmatrix}
a&b&t_x\\
c&d&t_y\\
0&0&1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$

---

# 16. Combining Transformations

Suppose we want:

```text
Scale
  ↓
Rotate
  ↓
Translate
```

The combined transformation can be written as:

$$
P'=TRSP
$$

The rightmost transformation happens first.

Therefore:

```text
P
↓
S
↓
R
↓
T
↓
P'
```

Matrix order is extremely important.

---

# 17. Order Matters

Generally:

$$
AB\neq BA
$$

Therefore:

```text
Rotate → Translate
```

can produce a different result from:

```text
Translate → Rotate
```

This is critical in:

* Computer graphics
* Image registration
* Medical image geometry
* 3D visualization

---

# 18. Image Transformation

Suppose an image coordinate:

$$
(x,y)
$$

is transformed into:

$$
(x',y')
$$

Using:

$$
P'=AP
$$

Conceptually:

```text
Original Image
       ↓
Transformation Matrix
       ↓
New Coordinate
       ↓
Transformed Image
```

---

# 19. Forward Mapping

Forward mapping:

$$
P'=T(P)
$$

Each source pixel maps to a destination location.

Problem:

```text
Source Pixels
     ↓
Transformation
     ↓
Destination
```

Some destination pixels may not receive a value.

This can create:

```text
Holes
```

---

# 20. Inverse Mapping

Instead:

$$
P=T^{-1}(P')
$$

For every destination pixel, we find the corresponding source location.

Conceptually:

```text
Destination Pixel
       ↓
Inverse Transform
       ↓
Source Position
```

Inverse mapping is widely used in image resampling.

---

# 21. Interpolation

After transformation, coordinates may become non-integer.

Example:

$$
(10.4,20.7)
$$

But image pixels exist at discrete positions.

Therefore interpolation is needed.

Common methods:

* Nearest Neighbor
* Bilinear Interpolation
* Bicubic Interpolation

---

# 22. Nearest Neighbor

Suppose transformed coordinate:

$$
(10.4,20.7)
$$

Nearest pixel approximately:

$$
(10,21)
$$

This method is:

```text
Fast
Simple
```

But can produce blocky results.

---

# 23. Bilinear Interpolation

Bilinear interpolation uses nearby pixels to estimate a new value.

Conceptually:

```text
P1 ───── P2
│         │
│    ●    │
│         │
P3 ───── P4
```

The new pixel value is estimated from surrounding pixels.

This usually provides smoother results than nearest-neighbor interpolation.

---

# 24. Medical Image Transformation

Transformations are heavily used in medical imaging.

Example:

```text
Patient CT
    ↓
Transformation
    ↓
Patient MRI
```

The goal may be to align images into a common coordinate system.

This is related to **image registration**.

---

# 25. Image Registration

Conceptually:

```text
Moving Image
     ↓
Transformation Matrix
     ↓
Alignment
     ↓
Fixed Image
```

Possible transformation types:

```text
Rigid
Affine
Deformable
```

### Rigid Transformation

Usually includes:

* Translation
* Rotation

### Affine Transformation

Can additionally include:

* Scaling
* Shearing

### Deformable Transformation

Allows local, non-linear deformation.

---

# 26. 3D Transformations

In 3D, a point is:

$$
P=
\begin{bmatrix}
x\\
y\\
z
\end{bmatrix}
$$

Homogeneous representation:

$$
P=
\begin{bmatrix}
x\\
y\\
z\\
1
\end{bmatrix}
$$

A 3D transformation uses a:

$$
4\times4
$$

matrix.

---

# 27. 3D Translation Matrix

$$
T=
\begin{bmatrix}
1&0&0&t_x\\
0&1&0&t_y\\
0&0&1&t_z\\
0&0&0&1
\end{bmatrix}
$$

This is widely used in:

* CT visualization
* MRI visualization
* 3D reconstruction
* Medical navigation

---

# 28. 3D Scaling Matrix

$$
S=
\begin{bmatrix}
S_x&0&0\\
0&S_y&0\\
0&0&S_z
\end{bmatrix}
$$

---

# 29. 3D Rotation

3D rotations can occur around:

```text
X-axis
Y-axis
Z-axis
```

### Rotation Around X-axis

$$
R_x=
\begin{bmatrix}
1&0&0\\
0&\cos\theta&-\sin\theta\\
0&\sin\theta&\cos\theta
\end{bmatrix}
$$

### Rotation Around Y-axis

$$
R_y=
\begin{bmatrix}
\cos\theta&0&\sin\theta\\
0&1&0\\
-\sin\theta&0&\cos\theta
\end{bmatrix}
$$

### Rotation Around Z-axis

$$
R_z=
\begin{bmatrix}
\cos\theta&-\sin\theta&0\\
\sin\theta&\cos\theta&0\\
0&0&1
\end{bmatrix}
$$

---

# 30. DICOM and Medical Image Geometry

Medical images have both:

```text
Pixel/Voxel Data
        +
Physical Geometry
```

The transformation between image indices and physical patient coordinates can involve:

* Pixel spacing
* Image orientation
* Image position
* Direction vectors

Conceptually:

```text
Voxel Index
    ↓
Geometry Transformation
    ↓
Physical Patient Position
```

This becomes extremely important when working with:

* DICOM
* ITK
* VTK
* Image registration
* Treatment Planning Systems

---

# 31. C++ Example: 2D Scaling

Without using external libraries:

```cpp
struct Point
{
    double x;
    double y;
};

Point scalePoint(
    const Point& point,
    double sx,
    double sy)
{
    Point result;

    result.x = point.x * sx;
    result.y = point.y * sy;

    return result;
}
```

---

# 32. C++ Example: 2D Translation

```cpp
Point translatePoint(
    const Point& point,
    double tx,
    double ty)
{
    Point result;

    result.x = point.x + tx;
    result.y = point.y + ty;

    return result;
}
```

---

# 33. C++ Example: 2D Rotation

```cpp
#include <cmath>

Point rotatePoint(
    const Point& point,
    double angleRadians)
{
    Point result;

    double cosine = std::cos(angleRadians);
    double sine = std::sin(angleRadians);

    result.x =
        point.x * cosine -
        point.y * sine;

    result.y =
        point.x * sine +
        point.y * cosine;

    return result;
}
```

For degrees:

```cpp
double degreesToRadians(double degrees)
{
    const double PI = 3.141592653589793;

    return degrees * PI / 180.0;
}
```

---

# 34. C++ Example: 2×2 Matrix Transformation

```cpp
struct Matrix2x2
{
    double m11;
    double m12;
    double m21;
    double m22;
};

Point transformPoint(
    const Matrix2x2& matrix,
    const Point& point)
{
    Point result;

    result.x =
        matrix.m11 * point.x +
        matrix.m12 * point.y;

    result.y =
        matrix.m21 * point.x +
        matrix.m22 * point.y;

    return result;
}
```

---

# 35. Practical Example

Original point:

$$
P=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

Scale by:

$$
S_x=2
$$

$$
S_y=3
$$

Result:

$$
P'=
\begin{bmatrix}
4\\
9
\end{bmatrix}
$$

Then rotate or translate the new point to create more complex transformations.

---

# 36. Important Concepts to Remember

| Concept                 | Meaning                                          |
| ----------------------- | ------------------------------------------------ |
| Linear Transformation   | Matrix-based transformation preserving linearity |
| Scaling                 | Changes size                                     |
| Rotation                | Changes orientation                              |
| Reflection              | Flips object                                     |
| Shear                   | Slants object                                    |
| Translation             | Moves position                                   |
| Homogeneous Coordinates | Allow translation using matrices                 |
| Affine Transformation   | Linear part + translation                        |
| Inverse Transformation  | Maps output back to source                       |
| Interpolation           | Estimates non-integer pixel values               |

---

# 37. Practice Questions

### Question 1

Apply:

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

to:

$$
P=
\begin{bmatrix}
2\\
1
\end{bmatrix}
$$

Answer:

$$
P'=
\begin{bmatrix}
4\\
3
\end{bmatrix}
$$

---

### Question 2

What transformation is:

$$
\begin{bmatrix}
0&-1\\
1&0
\end{bmatrix}
$$

Answer:

A \(90^\circ\) counterclockwise rotation.

---

### Question 3

What is the main purpose of homogeneous coordinates?

**Answer:** They allow affine transformations such as translation to be represented using matrix multiplication.

---

### Question 4

Which is generally preferred for image resampling?

```text
Forward Mapping
or
Inverse Mapping
```

**Answer:** Inverse mapping is commonly preferred because every destination pixel can be mapped back to a source position, helping avoid unmapped holes.

---

# 38. Chapter Summary

You learned:

* Linear transformation definition
* Matrix transformation
* Identity transformation
* Scaling
* Uniform scaling
* Non-uniform scaling
* Rotation
* Reflection
* Shearing
* Translation
* Homogeneous coordinates
* Affine transformations
* Combining transformations
* Transformation order
* Forward mapping
* Inverse mapping
* Interpolation
* 2D transformations
* 3D transformations
* Medical image registration basics
* Medical image geometry

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
⬜ Chapter 18: Eigenvalues and Eigenvectors
⬜ Chapter 19: Calculus Fundamentals
⬜ Chapter 20: Partial Derivatives
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 18 — Eigenvalues and Eigenvectors**
