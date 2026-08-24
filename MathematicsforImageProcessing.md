# Chapter 1 — Mathematics for Image Processing

We now move to **Chapter 1 exactly as defined in your index**. We will cover only the topics listed under this chapter, in order. 

## Chapter 1 Index

1. Numbers and functions
2. Coordinate systems
3. Vectors
4. Matrices
5. Matrix operations
6. Linear transformations
7. Eigenvalues / eigenvectors
8. Complex numbers
9. Calculus basics
10. Derivatives
11. Partial derivatives
12. Integrals
13. Probability
14. Statistics
15. Optimization basics
16. Numerical methods

---

# 1.1 Why Mathematics Is Important

Image processing is essentially **mathematics applied to image data**.

An image may look like:

```text
      IMAGE
        ↓
   pixels / voxels
        ↓
     numbers
        ↓
 mathematical operations
        ↓
 processed image
```

For example, a grayscale image can be represented as:

[
I(x,y)
]

where:

* (x) = position in the horizontal direction
* (y) = position in the vertical direction
* (I(x,y)) = intensity at that location

Later, when we study:

* convolution
* filtering
* Fourier transforms
* segmentation
* registration
* reconstruction
* machine learning

we will repeatedly use mathematics.

But we will **not assume you are a mathematics expert**.

The goal is:

> Learn the mathematics necessary to understand and implement image-processing algorithms.

---

# 1.2 Numbers and Functions

Let's begin with the most basic concept.

## 1.2.1 Numbers

Computers represent image information using numbers.

Examples:

```text
0
1
10
128
255
-100
3.14159
0.001
```

Different types of numbers are useful for different purposes.

### Natural numbers

[
0,1,2,3,\ldots
]

Useful for:

* pixel indices
* image dimensions
* array positions

Example:

```text
512 × 512
```

### Integers

[
..., -2,-1,0,1,2,...
]

Useful for:

* signed image values
* coordinate calculations
* differences

### Real numbers

Examples:

[
0.5,\quad 1.25,\quad -3.7,\quad \pi
]

Important for:

* interpolation
* filtering
* physical measurements
* optimization
* probability

---

# 1.2.2 Functions

A function maps an input to an output.

For example:

[
y=f(x)
]

Suppose:

[
f(x)=2x
]

Then:

[
f(1)=2
]

[
f(5)=10
]

[
f(10)=20
]

Conceptually:

```text
Input
  │
  ▼
Function
  │
  ▼
Output
```

---

# 1.2.3 Why Functions Matter in Image Processing

An image itself can be considered a function.

For a grayscale image:

[
I(x,y)
]

The function takes:

```text
(x, y)
```

and returns:

```text
intensity
```

For example:

[
I(10,20)=125
]

means:

> At coordinate `(10,20)`, the image intensity is `125`.

This is the foundation of almost everything we will learn.

---

# 1.2.4 Image as a Function

Think of:

[
I:\mathbb{R}^2\rightarrow\mathbb{R}
]

as:

```text
(x, y)
   ↓
 Image Function
   ↓
Intensity
```

For a color image, we might have:

[
I(x,y) =
\begin{bmatrix}
R(x,y)\
G(x,y)\
B(x,y)
\end{bmatrix}
]

So one coordinate produces three values:

```text
(x,y)
 ↓
 ┌───────────┐
 │ R value   │
 │ G value   │
 │ B value   │
 └───────────┘
```

---

# 1.3 Coordinate Systems

Images contain spatial information.

We therefore need a way to describe **where** something is.

That is the job of a coordinate system.

---

## 1.3.1 2D Coordinate System

A basic Cartesian coordinate system has:

```text
          y
          ↑
          │
          │
          │
──────────┼──────────→ x
          │
          │
          │
```

A point can be represented as:

[
(x,y)
]

For example:

[
(3,2)
]

means:

```text
x = 3
y = 2
```

---

# 1.3.2 Image Coordinates

Computer images commonly use a coordinate convention where the origin is at the **top-left**.

Conceptually:

```text
(0,0) ───────────────→ x
  │
  │
  │
  │
  ↓
  y
```

Example:

```text
        x →
      0   1   2   3
    ┌───┬───┬───┬───┐
  0 │   │   │   │   │
    ├───┼───┼───┼───┤
  1 │   │ X │   │   │
    ├───┼───┼───┼───┤
  2 │   │   │   │   │
    └───┴───┴───┴───┘
      ↑
      y
```

The `X` is located at:

[
(x,y)=(1,1)
]

This coordinate convention becomes extremely important when we work with:

* OpenCV
* ITK
* VTK
* DICOM
* CT
* MRI
* MPR

---

# 1.4 Vectors

A vector is a mathematical object that can represent:

* direction
* magnitude
* position-related quantities
* multiple values together

A 2D vector can be written:

[
\mathbf{v}
==========

\begin{bmatrix}
x\
y
\end{bmatrix}
]

For example:

[
\mathbf{v}
==========

\begin{bmatrix}
3\
4
\end{bmatrix}
]

---

# 1.4.1 Vector Magnitude

The magnitude of:

[
\mathbf{v}=
\begin{bmatrix}
3\
4
\end{bmatrix}
]

is:

[
|\mathbf{v}|=\sqrt{3^2+4^2}
]

[
=\sqrt{9+16}
]

[
=\sqrt{25}=5
]

This is simply the Pythagorean theorem.

---

# 1.4.2 Why Vectors Matter

Vectors appear everywhere in image processing.

For example:

### Pixel coordinate

[
\mathbf{p}
==========

\begin{bmatrix}
x\
y
\end{bmatrix}
]

### Image gradient

[
\nabla I
========

\begin{bmatrix}
\frac{\partial I}{\partial x}\
\frac{\partial I}{\partial y}
\end{bmatrix}
]

### 3D voxel coordinate

[
\mathbf{p}
==========

\begin{bmatrix}
x\
y\
z
\end{bmatrix}
]

Later, registration and geometric transformations will heavily use vectors.

---

# 1.5 Matrices

A matrix is a rectangular arrangement of numbers.

Example:

[
A=
\begin{bmatrix}
1&2&3\
4&5&6\
7&8&9
\end{bmatrix}
]

This matrix has:

* 3 rows
* 3 columns

Therefore its size is:

[
3\times3
]

---

# 1.5.1 Image as a Matrix

A grayscale image can be represented as a matrix.

Example:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

Therefore:

```text
Image
  ↓
Matrix
  ↓
Numbers
```

This is why matrices are so important in image processing.

---

# 1.6 Matrix Operations

We need to understand several operations.

## Matrix Addition

Given:

[
A=
\begin{bmatrix}
1&2\
3&4
\end{bmatrix}
]

and:

[
B=
\begin{bmatrix}
5&6\
7&8
\end{bmatrix}
]

Then:

[
A+B=
\begin{bmatrix}
6&8\
10&12
\end{bmatrix}
]

We add corresponding elements.

---

# 1.6.1 Scalar Multiplication

Suppose:

[
A=
\begin{bmatrix}
1&2\
3&4
\end{bmatrix}
]

Multiply by 2:

[
2A=
\begin{bmatrix}
2&4\
6&8
\end{bmatrix}
]

This is useful conceptually for image intensity scaling.

---

# 1.6.2 Matrix Multiplication

This is different from element-by-element multiplication.

Suppose:

[
A=
\begin{bmatrix}
1&2\
3&4
\end{bmatrix}
]

and:

[
B=
\begin{bmatrix}
5&6\
7&8
\end{bmatrix}
]

Then:

[
AB=
\begin{bmatrix}
1(5)+2(7)&1(6)+2(8)\
3(5)+4(7)&3(6)+4(8)
\end{bmatrix}
]

Therefore:

[
AB=
\begin{bmatrix}
19&22\
43&50
\end{bmatrix}
]

We will later use matrix multiplication for transformations.

---

# 1.7 Linear Transformations

A transformation changes something mathematically.

For example:

```text
Original Image
      ↓
Transformation
      ↓
Modified Image
```

A matrix can be used to perform transformations.

For a 2D point:

[
\mathbf{p}
==========

\begin{bmatrix}
x\
y
\end{bmatrix}
]

we can apply:

[
\mathbf{p'}=A\mathbf{p}
]

where (A) is a transformation matrix.

---

## Example — Scaling

Consider:

[
A=
\begin{bmatrix}
2&0\
0&2
\end{bmatrix}
]

and:

[
p=
\begin{bmatrix}
3\
4
\end{bmatrix}
]

Then:

[
p'=Ap
]

# [

\begin{bmatrix}
2&0\
0&2
\end{bmatrix}
\begin{bmatrix}
3\
4
\end{bmatrix}
]

# [

\begin{bmatrix}
6\
8
\end{bmatrix}
]

So:

```text
(3,4)
  ↓
Scaling ×2
  ↓
(6,8)
```

Later this becomes:

* image resizing
* rotation
* registration
* geometric transformation
* 3D transformation

---

# 1.8 Eigenvalues and Eigenvectors

This is an important mathematical concept, but **do not worry if it seems abstract initially**.

For a matrix (A), an eigenvector satisfies:

[
A\mathbf{v}=\lambda\mathbf{v}
]

where:

* (A) = matrix
* (\mathbf{v}) = eigenvector
* (\lambda) = eigenvalue

The important intuition is:

> An eigenvector is a special direction that a transformation does not change in direction; it only scales it.

Conceptually:

```text
Vector
  ↓
Matrix Transformation
  ↓
Same direction
  ↓
Different magnitude
```

Eigenvalues/eigenvectors become useful later in areas such as:

* image analysis
* dimensionality reduction
* PCA
* covariance analysis
* feature extraction

We will revisit them with much more detail when required.

---

# 1.9 Complex Numbers

A complex number has the form:

[
z=a+bi
]

where:

[
i^2=-1
]

For example:

[
z=3+4i
]

Complex numbers are extremely important in **Fourier analysis**.

Eventually, when we study:

```text
Image
 ↓
Fourier Transform
 ↓
Complex Frequency Representation
```

complex numbers will become unavoidable.

For now, remember:

```text
Complex Number
      │
      ├── Real part
      │
      └── Imaginary part
```

---

# 1.10 Calculus Basics

Calculus studies how quantities:

* change
* accumulate
* vary

The two major concepts we need are:

```text
Calculus
 ├── Derivatives
 └── Integrals
```

---

# 1.11 Derivatives

A derivative measures **how quickly something changes**.

For:

[
f(x)=x^2
]

the derivative is:

[
f'(x)=2x
]

At:

[
x=3
]

the derivative is:

[
f'(3)=6
]

So around (x=3), the function is changing at a rate of 6.

---

# 1.11.1 Why Derivatives Matter in Images

Suppose image intensity changes rapidly from one pixel to another.

That can indicate an **edge**.

For example:

```text
Dark pixels       Bright pixels

10 10 10 10 | 200 200 200 200
             ↑
            EDGE
```

Mathematically, we can detect changes using derivatives.

Later:

[
\frac{\partial I}{\partial x}
]

and:

[
\frac{\partial I}{\partial y}
]

will form the image gradient.

This leads directly to:

* Sobel
* Prewitt
* Scharr
* Canny
* gradient-based segmentation

---

# 1.12 Partial Derivatives

An image has at least two spatial variables:

[
I(x,y)
]

Therefore we need to understand how intensity changes in each direction.

### Horizontal change

[
\frac{\partial I}{\partial x}
]

### Vertical change

[
\frac{\partial I}{\partial y}
]

Together:

[
\nabla I=
\begin{bmatrix}
\frac{\partial I}{\partial x}\
\frac{\partial I}{\partial y}
\end{bmatrix}
]

This is the **gradient**.

The gradient is one of the most important concepts in image processing.

---

# 1.13 Integrals

An integral is related to **accumulation**.

For example:

[
\int_a^b f(x),dx
]

can represent the area under a curve.

Why do we care?

Integrals appear in:

* continuous image models
* probability
* Fourier transforms
* image formation
* physical imaging models
* reconstruction

You don't need to become an integration expert before continuing, but you need to understand what integration represents.

Think:

```text
Derivative
→ rate of change

Integral
→ accumulation
```

---

# 1.14 Probability

Image processing often deals with uncertainty.

For example:

```text
Is this pixel noise?
Is this pixel part of a tumor?
Is this edge real?
Is this measurement reliable?
```

Probability gives us a mathematical language for uncertainty.

Basic notation:

[
P(A)
]

means:

> Probability of event (A).

For example:

[
0\leq P(A)\leq1
]

where:

* `0` = impossible
* `1` = certain

---

# 1.15 Statistics

Statistics helps us understand image data.

Suppose pixel intensities are:

```text
10, 20, 30, 40, 50
```

The mean is:

[
\mu=\frac{10+20+30+40+50}{5}
]

[
\mu=30
]

Statistics will later be used for:

* image intensity analysis
* noise estimation
* segmentation
* radiomics
* machine learning
* medical AI evaluation

Important concepts include:

* mean
* median
* variance
* standard deviation
* distributions
* covariance

---

# 1.16 Optimization Basics

Optimization means:

> Find the best solution according to some objective.

For example:

```text
Find x that minimizes:
f(x)
```

We might write:

[
x^*=\arg\min_x f(x)
]

The (x^*) means:

> the value of (x) that gives the minimum value of the function.

Optimization becomes extremely important later in:

* image registration
* reconstruction
* machine learning
* deep learning
* segmentation

For example:

```text
Fixed Image
     +
Moving Image
     ↓
Registration
     ↓
Optimization
     ↓
Best transformation
```

---

# 1.17 Numerical Methods

Computers generally cannot solve every mathematical problem exactly.

Therefore, we often use **numerical methods** to obtain approximate solutions.

Examples:

* numerical differentiation
* numerical integration
* iterative optimization
* interpolation
* numerical linear algebra

For example:

```text
Mathematical problem
       ↓
Numerical algorithm
       ↓
Approximate solution
```

This becomes especially important for large medical images because we may be processing millions of voxels.

---

# 1.18 How All the Mathematics Connects

This is the most important part of today's chapter.

```text
Numbers
   ↓
Functions
   ↓
Coordinates
   ↓
Vectors
   ↓
Matrices
   ↓
Transformations
   ↓
Derivatives
   ↓
Gradients
   ↓
Integrals
   ↓
Probability + Statistics
   ↓
Optimization
   ↓
Numerical Methods
   ↓
IMAGE PROCESSING
```

And eventually:

```text
IMAGE PROCESSING
       ↓
MEDICAL IMAGE PROCESSING
       ↓
CT / MRI / X-RAY
       ↓
SEGMENTATION
       ↓
REGISTRATION
       ↓
RECONSTRUCTION
       ↓
AI
       ↓
MEDICAL IMAGING SOFTWARE
```

---

# 1.19 First C++ Mathematical Example

Let's connect mathematics with C++.

```cpp
#include <iostream>
#include <cmath>

int main()
{
    double x = 3.0;

    double y = x * x;

    double derivative = 2.0 * x;

    std::cout << "f(x) = " << y << '\n';
    std::cout << "f'(x) = " << derivative << '\n';

    return 0;
}
```

Here:

[
f(x)=x^2
]

and:

[
f'(x)=2x
]

At `x = 3`:

```text
f(x)  = 9
f'(x) = 6
```

This is a very simple example, but later the same mathematical ideas become part of real image-processing algorithms.

---

# 1.20 Python Example

```python
x = 3.0

y = x * x
derivative = 2.0 * x

print("f(x) =", y)
print("f'(x) =", derivative)
```

Output:

```text
f(x) = 9.0
f'(x) = 6.0
```

---

# 1.21 Medical Imaging Connection

Consider a CT image:

[
I(x,y)
]

Suppose intensity changes sharply around a bone boundary.

Then:

[
\frac{\partial I}{\partial x}
]

or:

[
\frac{\partial I}{\partial y}
]

may become large.

Therefore:

```text
CT
 ↓
Intensity changes
 ↓
Derivative
 ↓
Gradient
 ↓
Edge
 ↓
Bone boundary
```

This simple chain connects **mathematics → image processing → medical imaging**.

---

# 1.22 Chapter 1 — What You Should Understand So Far

At this stage, you should understand the role of:

| Concept                  | Why we need it                       |
| ------------------------ | ------------------------------------ |
| Numbers                  | Represent image data                 |
| Functions                | Represent image intensity            |
| Coordinates              | Locate pixels/voxels                 |
| Vectors                  | Represent positions/directions       |
| Matrices                 | Represent images and transformations |
| Matrix operations        | Process and transform data           |
| Linear transformations   | Change geometry                      |
| Eigenvalues/eigenvectors | Analyze mathematical structures      |
| Complex numbers          | Fourier analysis                     |
| Derivatives              | Detect changes/edges                 |
| Partial derivatives      | Analyze image changes in x/y         |
| Integrals                | Accumulation and continuous models   |
| Probability              | Uncertainty                          |
| Statistics               | Analyze image data                   |
| Optimization             | Find best solutions                  |
| Numerical methods        | Solve problems computationally       |

---

# Chapter 1 — Knowledge Check

Before going deeper, answer these yourself:

### Basic

1. What is a function?
2. How can an image be represented as a function?
3. What does (I(x,y)) mean?
4. What is a coordinate system?
5. What is a vector?
6. What is a matrix?

### Intermediate

7. What is the difference between scalar multiplication and matrix multiplication?
8. Why can an image be represented as a matrix?
9. What is a linear transformation?
10. What is an eigenvector?
11. What is an eigenvalue?
12. Why are complex numbers important in image processing?

### Image-processing connection

13. Why are derivatives useful for detecting image edges?
14. What is the difference between (\frac{\partial I}{\partial x}) and (\frac{\partial I}{\partial y})?
15. What is a gradient?
16. Why do we need probability in image processing?
17. Why do we need statistics?
18. What does optimization mean?
19. Why are numerical methods necessary?

### Practical

For this image:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

answer:

20. What are its dimensions?
21. What is the center pixel?
22. What is the mean intensity?
23. Multiply the complete matrix by 2.
24. Add 10 to every element.
25. What would the matrix represent if it were a grayscale image?

---

## Important

**Do not move to Chapter 2 yet.**

Chapter 1 is intentionally foundational. We need to make sure these mathematical concepts are comfortable before moving to:

> **Chapter 2 — What Is an Image?**

When you are ready, say **“continue Chapter 1”** and we will go deeper into the mathematics before moving to Chapter 2.
