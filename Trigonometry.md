# Module 2 → Chapter 13: Trigonometry

Trigonometry is the mathematics of **angles, triangles, rotation, and periodic motion**.

It is important in:

* Image rotation
* Image geometry
* CT reconstruction
* MRI
* Computer vision
* 3D medical imaging
* Image registration

---

# 1. What Is Trigonometry?

Trigonometry studies relationships between:

```text
Angles
Sides
Triangles
```

The most important starting point is the **right-angled triangle**.

```text
        /|
       / |
      /  | Opposite
     /   |
    /θ___|
   Adjacent

Hypotenuse
```

Where:

$$
\theta
$$

is an angle.

---

# 2. Parts of a Right Triangle

A right triangle has three important sides:

### Hypotenuse

The longest side.

It is opposite the:

$$
90^\circ
$$

angle.

### Opposite

The side opposite to the angle being considered.

### Adjacent

The side next to the angle being considered.

---

# 3. SOH-CAH-TOA

This is the most important beginner rule.

```text
SOH
Sin = Opposite / Hypotenuse

CAH
Cos = Adjacent / Hypotenuse

TOA
Tan = Opposite / Adjacent
```

Mathematically:

$$
\sin\theta=
\frac{Opposite}{Hypotenuse}
$$

$$
\cos\theta=
\frac{Adjacent}{Hypotenuse}
$$

$$
\tan\theta=
\frac{Opposite}{Adjacent}
$$

---

# 4. Example: Sine

Suppose:

```text
Opposite = 3
Hypotenuse = 5
```

Then:

$$
\sin\theta=
\frac{3}{5}
$$

$$
\sin\theta=0.6
$$

---

# 5. Example: Cosine

Suppose:

```text
Adjacent = 4
Hypotenuse = 5
```

Then:

$$
\cos\theta=
\frac{4}{5}
$$

$$
\cos\theta=0.8
$$

---

# 6. Example: Tangent

Suppose:

```text
Opposite = 3
Adjacent = 4
```

Then:

$$
\tan\theta=
\frac{3}{4}
$$

$$
\tan\theta=0.75
$$

---

# 7. Pythagorean Theorem

For a right triangle:

$$
a^2+b^2=c^2
$$

Where:

```text
a → Side
b → Side
c → Hypotenuse
```

Example:

$$
3^2+4^2=5^2
$$

$$
9+16=25
$$

Therefore:

$$
25=25
$$

---

# 8. Why Pythagoras Is Important in Image Processing

Distance between two pixels:

$$
(x_1,y_1)
$$

and:

$$
(x_2,y_2)
$$

is:

$$
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

Example:

```text
Point A = (0, 0)
Point B = (3, 4)
```

Distance:

$$
d=
\sqrt{3^2+4^2}
$$

$$
d=5
$$

Used in:

* Image registration
* Segmentation
* Object measurement
* Medical-image distance measurement

---

# 9. Degrees

Angles are often represented in degrees:

```text
0°
30°
45°
60°
90°
180°
270°
360°
```

A complete circle:

$$
360^\circ
$$

---

# 10. Radians

In mathematics and programming, angles are often represented in **radians**.

Important values:

| Degrees       | Radians   |
| ------------- | --------- |
| \(0^\circ\)   | \(0\)     |
| \(30^\circ\)  | \(\pi/6\) |
| \(45^\circ\)  | \(\pi/4\) |
| \(90^\circ\)  | \(\pi/2\) |
| \(180^\circ\) | \(\pi\)   |
| \(360^\circ\) | \(2\pi\)  |

---

# 11. Degrees to Radians

Formula:

$$
Radians=
Degrees\times
\frac{\pi}{180}
$$

Example:

$$
90^\circ
$$

$$
90\times\frac{\pi}{180}
$$

$$
=\frac{\pi}{2}
$$

---

# 12. Radians to Degrees

Formula:

$$
Degrees=
Radians\times
\frac{180}{\pi}
$$

Example:

$$
\frac{\pi}{2}
$$

$$
\frac{\pi}{2}
\times
\frac{180}{\pi}
$$

$$
=90^\circ
$$

---

# 13. Important Trigonometric Values

| Angle        | \(\sin\theta\) | \(\cos\theta\) | \(\tan\theta\) |
| ------------ | -------------: | -------------: | -------------: |
| \(0^\circ\)  |              0 |              1 |              0 |
| \(30^\circ\) |            0.5 |   \(\sqrt3/2\) |   \(1/\sqrt3\) |
| \(45^\circ\) |   \(\sqrt2/2\) |   \(\sqrt2/2\) |              1 |
| \(60^\circ\) |   \(\sqrt3/2\) |            0.5 |     \(\sqrt3\) |
| \(90^\circ\) |              1 |              0 |      Undefined |

You don't need to memorize everything immediately, but these values become useful later.

---

# 14. The Unit Circle

A unit circle has:

$$
Radius=1
$$

```text
             y
             ↑
             |
        (0,1)
             |
             |
(-1,0) ------+------ (1,0)
             |
             |
        (0,-1)
             |
             ↓
```

For an angle:

$$
\theta
$$

the point on the unit circle is:

$$
(\cos\theta,\sin\theta)
$$

This is extremely important for **rotation**.

---

# 15. Sine and Cosine Meaning

On the unit circle:

$$
x=\cos\theta
$$

$$
y=\sin\theta
$$

Therefore:

```text
Angle
  ↓
cos(θ) → X coordinate

sin(θ) → Y coordinate
```

---

# 16. Image Rotation

Suppose we have a point:

$$
(x,y)
$$

After rotation by angle:

$$
\theta
$$

the new point is:

$$
x'=x\cos\theta-y\sin\theta
$$

$$
y'=x\sin\theta+y\cos\theta
$$

This is one of the most important trigonometric applications in image processing.

---

# 17. Rotation Example

Suppose:

$$
(x,y)=(1,0)
$$

Rotate by:

$$
90^\circ
$$

We know:

$$
\cos90^\circ=0
$$

$$
\sin90^\circ=1
$$

Then:

$$
x'=1(0)-0(1)=0
$$

$$
y'=1(1)+0(0)=1
$$

Result:

$$
(1,0)
\rightarrow
(0,1)
$$

---

# 18. Rotation of Medical Images

```text
Original CT Slice
       │
       ▼
Rotation Angle θ
       │
       ▼
Rotated CT Slice
```

Trigonometry helps calculate the new pixel positions.

Used in:

* CT viewing
* Image registration
* Patient alignment
* 3D visualization
* Radiation therapy planning

---

# 19. Important Warning: Rotation Around the Origin

The rotation equations assume rotation around:

$$
(0,0)
$$

But images are often rotated around their center.

Image center:

$$
(c_x,c_y)
$$

General process:

```text
Original Point
      ↓
Translate to Center
      ↓
Rotate
      ↓
Translate Back
```

Mathematically:

### Step 1

$$
x=x-c_x
$$

$$
y=y-c_y
$$

### Step 2: Rotate

$$
x'=x\cos\theta-y\sin\theta
$$

$$
y'=x\sin\theta+y\cos\theta
$$

### Step 3: Translate Back

$$
x_{final}=x'+c_x
$$

$$
y_{final}=y'+c_y
$$

---

# 20. Inverse Trigonometric Functions

Sometimes we know the sides and need the angle.

Inverse functions:

$$
\sin^{-1}(x)
$$

$$
\cos^{-1}(x)
$$

$$
\tan^{-1}(x)
$$

Example:

$$
\tan\theta=
\frac{3}{4}
$$

Then:

$$
\theta=
\tan^{-1}
\left(
\frac{3}{4}
\right)
$$

Approximately:

$$
\theta\approx36.87^\circ
$$

---

# 21. `atan2`

In programming, for a point:

$$
(x,y)
$$

the angle is commonly calculated using:

$$
\theta=
atan2(y,x)
$$

This handles quadrants correctly.

Example C++:

```cpp
#include <cmath>

double angle =
    std::atan2(
        y,
        x
    );
```

---

# 22. Sine Wave

A sine wave can be represented as:

$$
y=\sin(x)
$$

Conceptually:

```text
      /\
     /  \
----/----\----
   /      \
  /        \
```

Sine waves are important in:

* Signal processing
* MRI
* CT reconstruction concepts
* Fourier transforms
* Frequency analysis

---

# 23. Periodicity

Sine and cosine repeat.

For example:

$$
\sin(\theta)
=
\sin(\theta+2\pi)
$$

The period is:

$$
2\pi
$$

or:

$$
360^\circ
$$

This becomes important in:

* Fourier analysis
* Signal processing
* Periodic signals

---

# 24. Trigonometric Identities

One of the most important identities:

$$
\sin^2\theta+
\cos^2\theta
=
1
$$

Example:

If:

$$
\sin\theta=0.6
$$

and:

$$
\cos\theta=0.8
$$

Then:

$$
0.6^2+0.8^2
$$

$$
0.36+0.64
$$

$$
=1
$$

---

# 25. Why This Identity Matters

The identity:

$$
\sin^2\theta+\cos^2\theta=1
$$

helps preserve geometric length during rotation.

This is one reason rotation using sine and cosine maintains geometric structure.

---

# 26. C++ Trigonometry

Include:

```cpp
#include <cmath>
```

Example:

```cpp
double angle =
    3.141592653589793 / 2.0;

double value =
    std::sin(angle);
```

Result is approximately:

```text
1
```

---

# 27. Degrees to Radians in C++

```cpp
#include <cmath>

double degrees = 90.0;

double radians =
    degrees *
    std::acos(-1.0) /
    180.0;
```

Then:

```cpp
double value =
    std::sin(radians);
```

---

# 28. C++ Example: Rotate a Point

```cpp
#include <cmath>

struct Point
{
    double x;
    double y;
};

Point rotatePoint(
    Point point,
    double angleRadians)
{
    Point result;

    result.x =
        point.x *
        std::cos(angleRadians)
        -
        point.y *
        std::sin(angleRadians);

    result.y =
        point.x *
        std::sin(angleRadians)
        +
        point.y *
        std::cos(angleRadians);

    return result;
}
```

---

# 29. Example Usage

```cpp
Point point;

point.x = 1.0;
point.y = 0.0;

double angle =
    std::acos(-1.0) / 2.0;

Point result =
    rotatePoint(
        point,
        angle
    );
```

Expected approximately:

```text
(0, 1)
```

Small floating-point errors are normal, so values may not be exactly zero.

---

# 30. Trigonometry in 3D

In 3D medical imaging, rotation can occur around:

```text
X-axis
Y-axis
Z-axis
```

```text
          Z
          ↑
          |
          |
          +------→ X
         /
        /
       Y
```

This becomes important in:

* 3D CT
* MRI visualization
* Volume rendering
* Surgical navigation
* Radiation therapy

---

# 31. Medical Imaging Applications

## Image Rotation

$$
\sin\theta,\cos\theta
$$

Used to rotate images.

## Distance and Geometry

$$
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

Used for measurement.

## Image Registration

```text
Image A
   ↓
Rotation
   ↓
Translation
   ↓
Image B
```

## CT Reconstruction

Trigonometry and geometry help model projections from different angles.

## 3D Visualization

Rotation around different axes uses trigonometric functions.

---

# 32. Important Concepts to Remember

```text
SOH-CAH-TOA
```

$$
\sin\theta=
\frac{Opposite}{Hypotenuse}
$$

$$
\cos\theta=
\frac{Adjacent}{Hypotenuse}
$$

$$
\tan\theta=
\frac{Opposite}{Adjacent}
$$

Pythagoras:

$$
a^2+b^2=c^2
$$

Degrees:

$$
360^\circ
$$

Radians:

$$
2\pi
$$

Rotation:

$$
x'=x\cos\theta-y\sin\theta
$$

$$
y'=x\sin\theta+y\cos\theta
$$

---

# 33. Practice Questions

### Question 1

Find:

$$
\sin30^\circ
$$

**Answer:**

$$
0.5
$$

---

### Question 2

Find the hypotenuse:

```text
a = 3
b = 4
```

$$
c=
\sqrt{3^2+4^2}
$$

$$
c=5
$$

---

### Question 3

Convert:

$$
180^\circ
$$

to radians.

$$
180\times
\frac{\pi}{180}
=
\pi
$$

---

### Question 4

Rotate:

$$
(1,0)
$$

by:

$$
90^\circ
$$

Answer:

$$
(0,1)
$$

---

# 34. Chapter Summary

You learned:

* Right triangles
* Hypotenuse
* Opposite and adjacent sides
* SOH-CAH-TOA
* Sine
* Cosine
* Tangent
* Pythagorean theorem
* Distance calculation
* Degrees
* Radians
* Degree/radian conversion
* Unit circle
* Sine and cosine coordinates
* Image rotation
* Rotation around image center
* Inverse trigonometric functions
* `atan2`
* Sine waves
* Periodicity
* Trigonometric identities
* C++ trigonometry
* 2D and 3D rotations

---

## Current Progress

```text
Module 2 — Mathematics Foundation

✅ Chapter 11: Algebra Fundamentals
✅ Chapter 12: Functions
✅ Chapter 13: Trigonometry
⬜ Chapter 14: Vectors
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

## Next: **Chapter 14 — Vectors**
