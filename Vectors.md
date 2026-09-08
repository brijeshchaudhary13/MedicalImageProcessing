# Module 2 → Chapter 14: Vectors

Vectors are fundamental to **computer vision, medical image processing, image registration, 3D visualization, CT/MRI geometry, and machine learning**.

---

# 1. What Is a Vector?

A vector has:

* **Magnitude** (length)
* **Direction**

Example:

```text
A ───────────→ B
```

A vector can represent movement from point A to point B.

Mathematically:

$$
\vec{v}=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

Example:

$$
\vec{v}=
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

---

# 2. Scalar vs Vector

## Scalar

A scalar has only magnitude.

Examples:

```text
Temperature = 25
Pixel Intensity = 150
Distance = 10
```

## Vector

A vector has magnitude and direction.

Example:

```text
Movement:
Right = 3
Up = 4
```

$$
\vec{v}=
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

---

# 3. 2D Vectors

A two-dimensional vector:

$$
\vec{v}=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

Example:

$$
\vec{v}=
\begin{bmatrix}
5\\
2
\end{bmatrix}
$$

Meaning:

```text
Move 5 units horizontally
Move 2 units vertically
```

---

# 4. 3D Vectors

For 3D:

$$
\vec{v}=
\begin{bmatrix}
x\\
y\\
z
\end{bmatrix}
$$

Example:

$$
\vec{v}=
\begin{bmatrix}
3\\
4\\
5
\end{bmatrix}
$$

Important in medical imaging:

```text
CT Volume
MRI Volume
3D Reconstruction
Patient Coordinates
Image Orientation
```

---

# 5. Point vs Vector

A point represents a **location**:

$$
P=(x,y)
$$

A vector represents a **direction/displacement**:

$$
\vec{v}=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

Example:

```text
Point A = (2, 3)
Point B = (7, 8)
```

The vector from A to B:

$$
\vec{AB}=B-A
$$

$$
=
\begin{bmatrix}
7-2\\
8-3
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
5\\
5
\end{bmatrix}
$$

---

# 6. Vector Addition

Suppose:

$$
\vec{a}=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

and:

$$
\vec{b}=
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

Then:

$$
\vec{a}+\vec{b}
=
\begin{bmatrix}
2+4\\
3+5
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
6\\
8
\end{bmatrix}
$$

---

# 7. Vector Subtraction

$$
\vec{a}-
\vec{b}
$$

Example:

$$
\begin{bmatrix}
5\\
8
\end{bmatrix}
-
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

Result:

$$
=
\begin{bmatrix}
3\\
5
\end{bmatrix}
$$

Used to calculate:

```text
Direction
Displacement
Position Difference
```

---

# 8. Scalar Multiplication

Suppose:

$$
\vec{v}=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

Multiply by:

$$
2
$$

Then:

$$
2\vec{v}
=
\begin{bmatrix}
4\\
6
\end{bmatrix}
$$

The direction remains the same, while the magnitude changes.

---

# 9. Vector Magnitude

The magnitude or length of a vector:

$$
\vec{v}=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

is:

$$
|\vec{v}|
=
\sqrt{x^2+y^2}
$$

Example:

$$
\vec{v}=
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

Then:

$$
|\vec{v}|
=
\sqrt{3^2+4^2}
$$

$$
=
5
$$

---

# 10. 3D Vector Magnitude

For:

$$
\vec{v}=
\begin{bmatrix}
x\\
y\\
z
\end{bmatrix}
$$

Magnitude:

$$
|\vec{v}|
=
\sqrt{x^2+y^2+z^2}
$$

Example:

$$
\vec{v}=
\begin{bmatrix}
1\\
2\\
2
\end{bmatrix}
$$

Then:

$$
|\vec{v}|
=
\sqrt{1+4+4}
$$

$$
=3
$$

---

# 11. Unit Vector

A unit vector has magnitude:

$$
1
$$

To normalize a vector:

$$
\hat{v}
=
\frac{\vec{v}}
{|\vec{v}|}
$$

Example:

$$
\vec{v}
=
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

Magnitude:

$$
|\vec{v}|=5
$$

Therefore:

$$
\hat{v}
=
\begin{bmatrix}
3/5\\
4/5
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
0.6\\
0.8
\end{bmatrix}
$$

---

# 12. Why Normalization Is Important

Normalization keeps direction but changes magnitude to:

$$
1
$$

Used in:

* Image gradients
* Surface normals
* Direction vectors
* Machine learning
* Image registration

---

# 13. Dot Product

For:

$$
\vec{a}=
\begin{bmatrix}
a_1\\
a_2
\end{bmatrix}
$$

and:

$$
\vec{b}=
\begin{bmatrix}
b_1\\
b_2
\end{bmatrix}
$$

The dot product is:

$$
\vec{a}\cdot\vec{b}
=
a_1b_1+a_2b_2
$$

Example:

$$
\vec{a}
=
\begin{bmatrix}
2\\
3
\end{bmatrix}
$$

$$
\vec{b}
=
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

Then:

$$
\vec{a}\cdot\vec{b}
=
2(4)+3(5)
$$

$$
=8+15
$$

$$
=23
$$

---

# 14. Dot Product and Angle

Another important formula:

$$
\vec{a}\cdot\vec{b}
=
|\vec{a}|
|\vec{b}|
\cos\theta
$$

Therefore:

$$
\cos\theta
=
\frac{
\vec{a}\cdot\vec{b}
}{
|\vec{a}|
|\vec{b}|
}
$$

This helps calculate the angle between vectors.

---

# 15. Orthogonal Vectors

Two vectors are perpendicular if:

$$
\vec{a}\cdot\vec{b}=0
$$

Example:

$$
\begin{bmatrix}
1\\
0
\end{bmatrix}
\cdot
\begin{bmatrix}
0\\
1
\end{bmatrix}
=0
$$

Therefore, they are perpendicular.

---

# 16. Medical Imaging Connection: Image Orientation

In 3D medical imaging, vectors can represent:

```text
Patient Right Direction
Patient Up Direction
Slice Direction
```

For example:

$$
\vec{r}
$$

may represent one orientation direction, while:

$$
\vec{c}
$$

represents another.

These orientation vectors help define how image pixels map into physical 3D space.

---

# 17. Cross Product

The cross product applies to **3D vectors**.

For:

$$
\vec{a}
$$

and:

$$
\vec{b}
$$

the cross product:

$$
\vec{a}\times\vec{b}
$$

produces a vector perpendicular to both.

Example:

$$
\begin{bmatrix}
1\\
0\\
0
\end{bmatrix}
\times
\begin{bmatrix}
0\\
1\\
0
\end{bmatrix}
=
\begin{bmatrix}
0\\
0\\
1
\end{bmatrix}
$$

---

# 18. Right-Hand Rule

The cross product direction follows the right-hand rule.

```text
X × Y = Z
```

This is important in:

* 3D graphics
* Surface normals
* Medical visualization
* Geometry

---

# 19. Vector Projection

Projection tells us how much one vector points in the direction of another.

Projection of:

$$
\vec{a}
$$

onto:

$$
\vec{b}
$$

is:

$$
proj_{\vec{b}}(\vec{a})
=
\frac{
\vec{a}\cdot\vec{b}
}{
|\vec{b}|^2
}
\vec{b}
$$

Used in:

* Geometry
* Motion analysis
* Image registration
* Feature analysis

---

# 20. Vector Distance

For two points:

$$
A=(x_1,y_1)
$$

$$
B=(x_2,y_2)
$$

Vector:

$$
\vec{AB}
=
B-A
$$

Distance:

$$
d=
|\vec{AB}|
$$

Therefore:

$$
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

---

# 21. Image Gradient as a Vector

Later, when studying gradients, we will use:

$$
\nabla I
=
\begin{bmatrix}
\frac{\partial I}{\partial x}\\
\frac{\partial I}{\partial y}
\end{bmatrix}
$$

This is a vector.

It describes:

```text
How intensity changes
        +
Direction of maximum change
```

This is fundamental for:

* Edge detection
* Image segmentation
* Feature detection

---

# 22. 3D Image Gradient

For a 3D image:

$$
I(x,y,z)
$$

The gradient is:

$$
\nabla I
=
\begin{bmatrix}
\frac{\partial I}{\partial x}\\
\frac{\partial I}{\partial y}\\
\frac{\partial I}{\partial z}
\end{bmatrix}
$$

Used in:

* Volume analysis
* Surface extraction
* 3D segmentation

---

# 23. Vector Representation in C++

```cpp
struct Vector2D
{
    double x;
    double y;
};
```

Example:

```cpp
Vector2D position;

position.x = 3.0;
position.y = 4.0;
```

---

# 24. Vector Addition in C++

```cpp
Vector2D add(
    const Vector2D& a,
    const Vector2D& b)
{
    Vector2D result;

    result.x = a.x + b.x;
    result.y = a.y + b.y;

    return result;
}
```

---

# 25. Vector Magnitude in C++

```cpp
#include <cmath>

double magnitude(
    const Vector2D& vector)
{
    return std::sqrt(
        vector.x * vector.x +
        vector.y * vector.y
    );
}
```

---

# 26. Vector Normalization in C++

```cpp
Vector2D normalize(
    const Vector2D& vector)
{
    double length =
        magnitude(vector);

    if (length == 0.0)
    {
        return {0.0, 0.0};
    }

    return
    {
        vector.x / length,
        vector.y / length
    };
}
```

Always handle the zero vector before division.

---

# 27. Dot Product in C++

```cpp
double dot(
    const Vector2D& a,
    const Vector2D& b)
{
    return
        a.x * b.x +
        a.y * b.y;
}
```

---

# 28. Cross Product in C++

For 3D:

```cpp
struct Vector3D
{
    double x;
    double y;
    double z;
};
```

Cross product:

```cpp
Vector3D cross(
    const Vector3D& a,
    const Vector3D& b)
{
    return
    {
        a.y * b.z - a.z * b.y,

        a.z * b.x - a.x * b.z,

        a.x * b.y - a.y * b.x
    };
}
```

---

# 29. Practical Example: Direction Between Two Pixels

Suppose:

```text
Point A = (10, 20)
Point B = (30, 50)
```

Direction vector:

$$
\vec{AB}
=
\begin{bmatrix}
30-10\\
50-20
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
20\\
30
\end{bmatrix}
$$

Magnitude:

$$
|\vec{AB}|
=
\sqrt{20^2+30^2}
$$

$$
=
\sqrt{1300}
$$

$$
\approx36.06
$$

Unit direction:

$$
\hat{AB}
=
\frac{
\vec{AB}
}{
|\vec{AB}|
}
$$

---

# 30. Medical Imaging Applications

Vectors are used in:

### Image Coordinates

```text
Pixel Position
     ↓
Vector
```

### Image Registration

```text
Image A
   ↓
Translation Vector
   ↓
Image B
```

### Gradient

```text
Intensity Change
      ↓
Gradient Vector
```

### 3D Geometry

```text
X Direction Vector
Y Direction Vector
Z Direction Vector
```

### Surface Normals

```text
Surface
   ↓
Normal Vector
```

### Patient Orientation

Vectors help describe how image rows, columns, and slices are oriented in physical space.

---

# 31. Important Formulas

### Magnitude

$$
|\vec{v}|
=
\sqrt{x^2+y^2}
$$

### 3D Magnitude

$$
|\vec{v}|
=
\sqrt{x^2+y^2+z^2}
$$

### Unit Vector

$$
\hat{v}
=
\frac{\vec{v}}
{|\vec{v}|}
$$

### Dot Product

$$
\vec{a}\cdot\vec{b}
=
a_1b_1+a_2b_2
$$

### Angle

$$
\cos\theta
=
\frac{
\vec{a}\cdot\vec{b}
}{
|\vec{a}|
|\vec{b}|
}
$$

### Cross Product

$$
\vec{a}\times\vec{b}
$$

### Distance

$$
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

---

# 32. Practice Questions

### Question 1

Find the magnitude:

$$
\vec{v}
=
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

Answer:

$$
5
$$

---

### Question 2

Add:

$$
\begin{bmatrix}
2\\
3
\end{bmatrix}
+
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

Answer:

$$
\begin{bmatrix}
6\\
8
\end{bmatrix}
$$

---

### Question 3

Find the dot product:

$$
\begin{bmatrix}
2\\
3
\end{bmatrix}
\cdot
\begin{bmatrix}
4\\
5
\end{bmatrix}
$$

Answer:

$$
23
$$

---

### Question 4

Normalize:

$$
\begin{bmatrix}
3\\
4
\end{bmatrix}
$$

Answer:

$$
\begin{bmatrix}
0.6\\
0.8
\end{bmatrix}
$$

---

# 33. Chapter Summary

You learned:

* Scalars vs vectors
* 2D vectors
* 3D vectors
* Points vs vectors
* Vector addition
* Vector subtraction
* Scalar multiplication
* Vector magnitude
* Unit vectors
* Normalization
* Dot product
* Angle between vectors
* Orthogonal vectors
* Cross product
* Right-hand rule
* Vector projection
* Distance
* Image gradients as vectors
* 3D vectors
* Vector applications in medical imaging

---

## Current Progress

```text
Module 2 — Mathematics Foundation

✅ Chapter 11: Algebra Fundamentals
✅ Chapter 12: Functions
✅ Chapter 13: Trigonometry
✅ Chapter 14: Vectors
⬜ Chapter 15: Matrices
⬜ Chapter 16: Matrix Operations
⬜ Chapter 17: Linear Transformations
⬜ Chapter 18: Eigenvalues and Eigenvectors
⬜ Chapter 19: Calculus Fundamentals
⬜ Chapter 20: Partial Derivatives
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 15 — Matrices**
