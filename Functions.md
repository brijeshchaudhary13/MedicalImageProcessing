# Module 2 → Chapter 12: Functions

Functions are one of the most important mathematical concepts for **image processing and medical imaging**.

A digital image itself can be represented as a mathematical function:

$$
I(x,y)
$$

For a 3D medical image:

$$
I(x,y,z)
$$

---

# 1. What Is a Function?

A function takes an **input** and produces an **output**.

```text
Input
  ↓
Function
  ↓
Output
```

Mathematically:

$$
y=f(x)
$$

Where:

* \(x\) = input
* \(f\) = function
* \(y\) = output

Example:

$$
f(x)=2x+3
$$

If:

$$
x=5
$$

Then:

$$
f(5)=2(5)+3=13
$$

Therefore:

$$
y=13
$$

---

# 2. Function Notation

General notation:

$$
f(x)
$$

Read as:

> “f of x”

Example:

$$
f(x)=x^2
$$

If:

$$
x=4
$$

Then:

$$
f(4)=4^2=16
$$

---

# 3. Domain and Range

Every function has:

### Domain

The possible input values.

### Range

The possible output values.

Example:

$$
f(x)=x^2
$$

If the domain is:

$$
x\in\{-2,-1,0,1,2\}
$$

Then:

| Input \(x\) | Output \(f(x)\) |
| ----------- | --------------: |
| -2          |               4 |
| -1          |               1 |
| 0           |               0 |
| 1           |               1 |
| 2           |               4 |

Range:

$$
\{0,1,4\}
$$

---

# 4. Image as a Function

A grayscale image can be represented as:

$$
I(x,y)
$$

Where:

```text
x → Horizontal position
y → Vertical position
I → Pixel intensity
```

Example:

$$
I(10,20)=150
$$

Means:

```text
Pixel position = (10, 20)
Pixel intensity = 150
```

For an 8-bit grayscale image:

$$
0\leq I(x,y)\leq255
$$

---

# 5. 3D Medical Image Function

A CT or MRI volume can be represented as:

$$
I(x,y,z)
$$

Where:

```text
x → Width
y → Height
z → Depth / Slice
```

Example:

$$
I(100,200,50)=800
$$

This means the voxel at:

```text
x = 100
y = 200
z = 50
```

has intensity:

```text
800
```

---

# 6. Independent and Dependent Variables

Example:

$$
y=f(x)
$$

Here:

```text
x → Independent variable
y → Dependent variable
```

Because \(y\) depends on \(x\).

For an image:

$$
I(x,y)
$$

Intensity depends on spatial location.

---

# 7. Linear Functions

A linear function:

$$
f(x)=mx+c
$$

Where:

```text
m → Slope
c → Intercept
```

Example:

$$
f(x)=2x+5
$$

If:

$$
x=10
$$

Then:

$$
f(10)=25
$$

---

# 8. Linear Function in Image Processing

Brightness and contrast can be represented as:

$$
I_{new}=aI_{old}+b
$$

Where:

```text
a → Contrast
b → Brightness
```

Example:

$$
I_{new}=1.5(100)+20
$$

$$
I_{new}=170
$$

This is a linear intensity transformation.

---

# 9. Identity Function

The identity function:

$$
f(x)=x
$$

Example:

$$
f(10)=10
$$

In image processing:

$$
I_{output}=I_{input}
$$

No change occurs.

---

# 10. Constant Function

A constant function:

$$
f(x)=c
$$

Example:

$$
f(x)=100
$$

No matter what \(x\) is:

$$
f(x)=100
$$

Image example:

```text
Every output pixel = 100
```

This creates a uniform grayscale image.

---

# 11. Quadratic Function

A quadratic function:

$$
f(x)=ax^2+bx+c
$$

Example:

$$
f(x)=x^2
$$

Values:

| \(x\) | \(f(x)\) |
| ----: | -------: |
|     0 |        0 |
|     1 |        1 |
|     2 |        4 |
|     3 |        9 |

Quadratic functions are useful in mathematical modeling and later optimization topics.

---

# 12. Exponential Function

Example:

$$
f(x)=a^x
$$

For example:

$$
f(x)=2^x
$$

| \(x\) | \(2^x\) |
| ----: | ------: |
|     0 |       1 |
|     1 |       2 |
|     2 |       4 |
|     3 |       8 |

Exponential functions appear in:

* Signal decay
* Imaging physics
* Probability
* Machine learning

---

# 13. Logarithmic Function

A logarithmic function is:

$$
f(x)=\log(x)
$$

Example:

$$
\log_{10}(100)=2
$$

Because:

$$
10^2=100
$$

Logarithmic transformations can be used for intensity enhancement.

---

# 14. Intensity Transformation as a Function

Suppose:

$$
s=T(r)
$$

Where:

```text
r → Input intensity
T → Transformation function
s → Output intensity
```

General concept:

```text
Input Pixel
    ↓
Transformation Function T
    ↓
Output Pixel
```

---

# 15. Negative Image Transformation

For an 8-bit image:

$$
s=255-r
$$

Example:

$$
r=100
$$

Then:

$$
s=255-100
$$

$$
s=155
$$

Thus dark and bright values are inverted.

---

# 16. Threshold Function

A threshold transformation:

$$
T(r)=
\begin{cases}
0 & r<T\\
255 & r\geq T
\end{cases}
$$

Example:

```text
Threshold = 128
```

Input:

```text
100 → 0
200 → 255
```

Used in:

* Segmentation
* Object separation
* Binary masks

---

# 17. Piecewise Functions

A piecewise function uses different formulas for different conditions.

Example:

$$
f(x)=
\begin{cases}
x+10 & x<100\\
2x & x\geq100
\end{cases}
$$

If:

$$
x=50
$$

Then:

$$
f(x)=50+10=60
$$

If:

$$
x=150
$$

Then:

$$
f(x)=2(150)=300
$$

Thresholding is also a piecewise function.

---

# 18. Function Composition

Suppose:

$$
f(x)=x+10
$$

and:

$$
g(x)=2x
$$

Then:

$$
f(g(x))
$$

First:

$$
g(x)=2x
$$

Then:

$$
f(g(x))=2x+10
$$

Example with:

$$
x=5
$$

$$
g(5)=10
$$

Then:

$$
f(10)=20
$$

---

# 19. Image Processing Pipeline as Function Composition

Suppose:

```text
Original Image
      ↓
Brightness Function
      ↓
Contrast Function
      ↓
Threshold Function
      ↓
Output Image
```

Mathematically:

$$
I_{output}
=
T_3(T_2(T_1(I)))
$$

This is **function composition**.

Very important for image-processing pipelines.

---

# 20. Inverse Functions

An inverse function reverses another function.

Suppose:

$$
f(x)=2x+10
$$

Find inverse.

Start:

$$
y=2x+10
$$

Subtract 10:

$$
y-10=2x
$$

Divide by 2:

$$
x=\frac{y-10}{2}
$$

Therefore:

$$
f^{-1}(y)=\frac{y-10}{2}
$$

---

# 21. Image Transformation and Inverse Transformation

Suppose:

$$
I_{new}=2I_{old}
$$

Then approximately reversing:

$$
I_{old}=\frac{I_{new}}{2}
$$

Inverse transformations are important in:

* Geometry
* Image registration
* Coordinate transformation
* Reconstruction

---

# 22. One-to-One Functions

A one-to-one function gives a unique output for each unique input.

Example:

$$
f(x)=2x
$$

```text
1 → 2
2 → 4
3 → 6
```

Different inputs produce different outputs.

---

# 23. Many-to-One Functions

Example:

$$
f(x)=x^2
$$

```text
2 → 4
-2 → 4
```

Different inputs can produce the same output.

This matters when discussing whether an inverse function exists over a chosen domain.

---

# 24. Discrete Functions

Digital images are discrete.

Instead of all real positions:

$$
x,y\in\mathbb{R}
$$

we usually have integer pixel coordinates:

$$
x,y\in\mathbb{Z}
$$

Example:

```text
(0,0)
(0,1)
(0,2)
...
```

So a digital image is often modeled as:

$$
I:\mathbb{Z}^2\rightarrow\mathbb{R}
$$

Conceptually:

```text
Pixel Coordinates
       ↓
Intensity Value
```

---

# 25. Continuous vs Discrete Functions

### Continuous

$$
f(x)
$$

Input can vary continuously.

Example:

```text
x = 1.1
x = 1.25
x = 1.251
```

### Discrete

Input values occur at specific positions.

Example:

```text
Pixel 0
Pixel 1
Pixel 2
Pixel 3
```

Medical image acquisition often begins with physical continuous signals that are sampled into discrete digital data.

---

# 26. Function Mapping

A function maps:

$$
Input\rightarrow Output
$$

For image processing:

$$
(x,y)\rightarrow I(x,y)
$$

Example:

```text
(0,0) → 50
(1,0) → 120
(2,0) → 200
```

---

# 27. Lookup Table (LUT)

A function can be implemented using a lookup table.

Example:

```text
Input → Output

0   → 0
1   → 10
2   → 20
3   → 30
```

In image processing:

```text
Input Intensity
      ↓
LUT
      ↓
Output Intensity
```

Useful for:

* Windowing
* Contrast adjustment
* Gamma transformation
* Color mapping

---

# 28. Gamma Function

A common intensity transformation is:

$$
s=c r^\gamma
$$

Where:

```text
r → Input intensity
s → Output intensity
γ → Gamma
c → Scaling constant
```

Example concept:

```text
Input Intensity
      ↓
Gamma Function
      ↓
Modified Intensity
```

Gamma transformations are nonlinear.

---

# 29. Medical Image Windowing

A simplified concept:

```text
Raw Intensity
      ↓
Window Function
      ↓
Display Intensity
```

Mathematically:

$$
I_{display}=T(I_{medical})
$$

Different transformation functions can highlight different tissue or intensity ranges.

---

# 30. C++ Function Example

Mathematical function:

$$
f(x)=2x+3
$$

C++:

```cpp
int function(int x)
{
    return 2 * x + 3;
}
```

Usage:

```cpp
int result =
    function(5);
```

Result:

```text
13
```

---

# 31. Image Intensity Function in C++

```cpp
float adjustIntensity(
    float pixel,
    float contrast,
    float brightness)
{
    return
        contrast * pixel +
        brightness;
}
```

Usage:

```cpp
float result =
    adjustIntensity(
        100.0f,
        1.2f,
        20.0f
    );
```

Result:

```text
140
```

---

# 32. Important Function Types

You should understand:

```text
Linear Function
Constant Function
Identity Function
Quadratic Function
Exponential Function
Logarithmic Function
Piecewise Function
Inverse Function
Discrete Function
Continuous Function
```

---

# 33. Practice Questions

### Question 1

Given:

$$
f(x)=3x+2
$$

Find:

$$
f(5)
$$

Answer:

$$
f(5)=3(5)+2=17
$$

---

### Question 2

Given:

$$
f(x)=x^2
$$

Find:

$$
f(4)
$$

Answer:

$$
16
$$

---

### Question 3

For an 8-bit negative transformation:

$$
s=255-r
$$

If:

$$
r=80
$$

Find \(s\).

Answer:

$$
s=175
$$

---

### Question 4

Threshold:

$$
T=100
$$

Function:

$$
s=
\begin{cases}
0 & r<100\\
255 & r\geq100
\end{cases}
$$

Find output for:

```text
r = 70
r = 150
```

Answer:

```text
70  → 0
150 → 255
```

---

# 34. Function Connection to Medical Image Processing

```text
Medical Image
     │
     ▼
I(x, y)
     │
     ├── Intensity Transformation
     │
     ├── Threshold Function
     │
     ├── Gamma Function
     │
     ├── Window Function
     │
     ├── Filter Function
     │
     └── Geometric Transformation
```

For 3D medical imaging:

$$
I(x,y,z)
$$

This mathematical representation becomes essential for:

* CT processing
* MRI processing
* Image filtering
* Segmentation
* Registration
* Reconstruction
* Computer vision
* Deep learning

---

# 35. Chapter 12 Summary

You learned:

* What a function is
* Function notation
* Domain and range
* Independent and dependent variables
* Linear functions
* Identity functions
* Constant functions
* Quadratic functions
* Exponential functions
* Logarithmic functions
* Intensity transformation functions
* Negative transformation
* Threshold functions
* Piecewise functions
* Function composition
* Inverse functions
* Discrete and continuous functions
* Function mapping
* Lookup tables
* Gamma transformation
* Medical image windowing

---

## Current Progress


## Next: Chapter 13 — Trigonometry
