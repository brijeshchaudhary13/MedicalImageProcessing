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

# Chapter 1 — Mathematics for Image Processing

## Continued: Deepening the Foundations

We are continuing **Chapter 1**, not moving to Chapter 2 yet. The index requires Chapter 1 to cover numbers/functions, coordinates, vectors, matrices, transformations, eigenvalues/eigenvectors, complex numbers, calculus, probability, statistics, optimization, and numerical methods. 

The previous section introduced these concepts. Now let's make the mathematical connections stronger.

---

# 1.23 Functions — Deeper Understanding

A function can be viewed as:

[
f: X \rightarrow Y
]

This means:

> A function takes an input from (X) and produces an output in (Y).

For example:

[
f(x)=2x+1
]

If:

[
x=3
]

then:

[
f(3)=2(3)+1=7
]

So:

```text
x = 3
 ↓
f(x) = 2x + 1
 ↓
7
```

## Image function

For a grayscale image:

[
I(x,y)
]

we have two inputs:

```text
x
y
```

and one output:

```text
intensity
```

Therefore:

```text
(x, y)
   ↓
I(x,y)
   ↓
Intensity
```

Example:

[
I(100,200)=145
]

means the image intensity at that location is 145.

---

# 1.24 Scalar vs Vector vs Matrix

This distinction is essential.

## Scalar

One value:

[
5
]

```text
Scalar = one number
```

## Vector

Multiple values arranged as one mathematical object:

[
\mathbf{v}
==========

\begin{bmatrix}
5\
10\
15
\end{bmatrix}
]

```text
Vector = ordered collection of values
```

## Matrix

Values arranged in rows and columns:

[
A=
\begin{bmatrix}
1&2&3\
4&5&6
\end{bmatrix}
]

```text
Matrix = rectangular arrangement of values
```

Think:

```text
Scalar
   ↓
one value

Vector
   ↓
one-dimensional structure

Matrix
   ↓
two-dimensional structure
```

An image is commonly represented using a matrix, while an image coordinate can be represented using a vector.

---

# 1.25 Vector Operations

Consider:

[
\mathbf{a}
==========

\begin{bmatrix}
2\
3
\end{bmatrix}
]

and:

[
\mathbf{b}
==========

\begin{bmatrix}
4\
5
\end{bmatrix}
]

## Addition

[
\mathbf{a}+\mathbf{b}
=====================

\begin{bmatrix}
6\
8
\end{bmatrix}
]

## Subtraction

[
\mathbf{a}-\mathbf{b}
=====================

\begin{bmatrix}
-2\
-2
\end{bmatrix}
]

## Scalar multiplication

[
3\mathbf{a}
===========

\begin{bmatrix}
6\
9
\end{bmatrix}
]

---

# 1.26 Dot Product

The dot product is:

[
\mathbf{a}\cdot\mathbf{b}
=========================

a_1b_1+a_2b_2
]

For:

[
\mathbf{a}=
\begin{bmatrix}
2\3
\end{bmatrix}
]

and:

[
\mathbf{b}=
\begin{bmatrix}
4\5
\end{bmatrix}
]

we get:

[
2(4)+3(5)
]

[
=8+15
]

[
=23
]

Dot products are important later for:

* projections
* similarity
* geometry
* feature comparison
* machine learning
* image gradients

---

# 1.27 Vector Length

For:

[
\mathbf{v}
==========

\begin{bmatrix}
x\y
\end{bmatrix}
]

the Euclidean length is:

[
|\mathbf{v}|
============

\sqrt{x^2+y^2}
]

For:

[
\mathbf{v}
==========

\begin{bmatrix}
3\4
\end{bmatrix}
]

we get:

[
|\mathbf{v}|=5
]

In 3D:

[
\mathbf{v}
==========

\begin{bmatrix}
x\y\z
\end{bmatrix}
]

and:

[
|\mathbf{v}|
============

\sqrt{x^2+y^2+z^2}
]

This will become important when working with **3D medical volumes**.

---

# 1.28 Matrices — Dimensions Matter

Suppose:

[
A=
\begin{bmatrix}
1&2&3\
4&5&6
\end{bmatrix}
]

This is a:

[
2\times3
]

matrix.

That means:

```text
2 rows
3 columns
```

Now consider:

[
B=
\begin{bmatrix}
1&2\
3&4\
5&6
\end{bmatrix}
]

This is:

[
3\times2
]

Notice:

[
(2\times3)(3\times2)
]

is valid.

The inner dimensions match:

```text
2 × 3
    3 × 2
    ↑
    match
```

The result has the outer dimensions:

[
2\times2
]

This dimension rule becomes extremely important in transformations and machine learning.

---

# 1.29 Matrix Multiplication Intuition

Matrix multiplication isn't simply:

```text
element × corresponding element
```

Instead:

> Each output element is produced using a row from the first matrix and a column from the second.

For:

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

the first output element is:

[
A_{11}B_{11}+A_{12}B_{21}
]

[
=1(5)+2(7)
]

[
=19
]

Therefore:

[
AB=
\begin{bmatrix}
19&22\
43&50
\end{bmatrix}
]

---

# 1.30 Matrix as an Image

Suppose:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

We can interpret:

```text
10 20 30
40 50 60
70 80 90
```

as intensity values.

For example:

[
I(1,1)=50
]

depending on the indexing convention being used.

This is why image-processing programming requires us to be very careful about:

* row
* column
* x
* y
* width
* height

A common programming mistake is confusing:

```text
(x,y)
```

with:

```text
(row,column)
```

We will revisit this in detail in the image-coordinate chapters.

---

# 1.31 Linear Transformations

A transformation changes a mathematical object.

For example:

```text
Original point
     ↓
Transformation
     ↓
New point
```

Suppose:

[
\mathbf{p}
==========

\begin{bmatrix}
x\y
\end{bmatrix}
]

and:

[
A=
\begin{bmatrix}
2&0\
0&2
\end{bmatrix}
]

Then:

[
\mathbf{p'}=A\mathbf{p}
]

This scales the point by 2.

---

# 1.32 Rotation

A 2D rotation matrix is:

[
R(\theta)
=========

\begin{bmatrix}
\cos\theta&-\sin\theta\
\sin\theta&\cos\theta
\end{bmatrix}
]

This is extremely important.

For example, when:

[
\theta=90^\circ
]

we have:

[
\cos90^\circ=0
]

[
\sin90^\circ=1
]

so:

[
R=
\begin{bmatrix}
0&-1\
1&0
\end{bmatrix}
]

Applying it to:

[
\begin{bmatrix}
1\0
\end{bmatrix}
]

gives:

[
\begin{bmatrix}
0\1
\end{bmatrix}
]

So:

```text
(1,0)
 ↓
90° rotation
 ↓
(0,1)
```

Later this becomes fundamental to:

* image rotation
* registration
* 3D transformations
* medical coordinate systems

---

# 1.33 Eigenvalues and Eigenvectors — Intuition

Consider a transformation:

[
A\mathbf{v}
]

Most vectors change both:

* direction
* magnitude

But special vectors behave differently.

An eigenvector satisfies:

[
A\mathbf{v}=\lambda\mathbf{v}
]

Meaning:

```text
Original vector
      ↓
Transformation
      ↓
Same direction
      ↓
Scaled by λ
```

The number (\lambda) is the **eigenvalue**.

This is important in mathematical analysis of data and later applications such as PCA.

---

# 1.34 Complex Numbers

A complex number:

[
z=a+bi
]

has:

* real part (a)
* imaginary part (b)

where:

[
i=\sqrt{-1}
]

and therefore:

[
i^2=-1
]

Example:

[
z=3+4i
]

Its magnitude is:

[
|z|=\sqrt{3^2+4^2}=5
]

This looks surprisingly similar to vector magnitude.

That is not accidental.

Complex numbers can also be represented geometrically:

```text
             Imaginary
                 ↑
                 │
                 │      • (3,4)
                 │
─────────────────┼──────────→ Real
```

Later, Fourier transforms will use complex numbers to represent frequency information.

---

# 1.35 Derivative — Visual Meaning

Suppose an intensity profile looks like:

```text
Intensity

200 |              ________
    |             /
100 |            /
    |           /
  0 |__________/
    +----------------------→ position
```

The derivative tells us how steeply the intensity is changing.

A flat region:

```text
__________
```

has approximately:

[
\frac{dI}{dx}=0
]

A sharp transition:

```text
     /
    /
___/
```

has a large derivative.

Therefore:

```text
Intensity change
       ↓
Derivative
       ↓
Edge information
```

This is one of the key bridges between calculus and image processing.

---

# 1.36 Partial Derivatives in 2D

For:

[
I(x,y)
]

we have:

[
\frac{\partial I}{\partial x}
]

and:

[
\frac{\partial I}{\partial y}
]

Think of them as:

```text
∂I/∂x
  ↓
How intensity changes horizontally

∂I/∂y
  ↓
How intensity changes vertically
```

Together:

[
\nabla I
========

\begin{bmatrix}
I_x\
I_y
\end{bmatrix}
]

where:

[
I_x=\frac{\partial I}{\partial x}
]

and:

[
I_y=\frac{\partial I}{\partial y}
]

---

# 1.37 Gradient Magnitude

The gradient magnitude is:

[
|\nabla I|
==========

\sqrt{I_x^2+I_y^2}
]

Suppose:

[
I_x=3
]

and:

[
I_y=4
]

Then:

[
|\nabla I|=5
]

A larger gradient magnitude generally indicates a stronger intensity transition.

Later this becomes fundamental to edge detection.

---

# 1.38 Probability vs Statistics

These concepts are related but different.

### Probability

Starts with a model and asks:

> What is likely to happen?

### Statistics

Starts with observations/data and asks:

> What can we infer from the data?

Think:

```text
Probability
Model → Data

Statistics
Data → Information about model
```

In medical imaging:

```text
Image Data
    ↓
Statistics
    ↓
Intensity distribution
    ↓
Features
    ↓
Analysis
```

---

# 1.39 Mean and Variance

Suppose pixel values are:

[
10,20,30,40,50
]

Mean:

[
\mu=30
]

The deviations are:

[
-20,-10,0,10,20
]

Variance measures how spread out the values are.

For a population:

[
\sigma^2
========

\frac{1}{N}
\sum_{i=1}^{N}(x_i-\mu)^2
]

For our example:

[
\sigma^2
========

\frac{
400+100+0+100+400
}{5}
]

[
=200
]

Standard deviation:

[
\sigma=\sqrt{200}
]

approximately:

[
14.14
]

Later these concepts will be useful for understanding image noise and contrast.

---

# 1.40 Optimization — A Simple Example

Suppose:

[
f(x)=x^2
]

We want to find its minimum.

Obviously:

[
x=0
]

gives:

[
f(0)=0
]

So:

[
x^*=0
]

Graphically:

```text
f(x)

  ↑
  │ \       /
  │  \     /
  │   \___/
  │
  └────────────→ x
       0
```

The lowest point is the optimum.

---

# 1.41 Why Optimization Matters in Registration

Suppose we have:

```text
Fixed CT
   +
Moving MRI
```

We want to find the transformation that aligns them.

There are many possible transformations.

So we define an objective:

[
F(T)
]

where (T) represents the transformation.

Then:

[
T^*
===

\arg\min_T F(T)
]

or, depending on the metric:

[
T^*
===

\arg\max_T F(T)
]

The computer searches for the transformation that gives the best alignment.

So:

```text
Fixed image
      +
Moving image
      ↓
Similarity metric
      ↓
Optimization
      ↓
Best transformation
      ↓
Registered image
```

This is a very important future connection.

---

# 1.42 Numerical Methods

Imagine we want to solve:

[
f(x)=0
]

Sometimes an exact analytical solution isn't practical.

We can use an iterative method.

For example:

```text
Initial guess
     ↓
Calculate
     ↓
Improve estimate
     ↓
Calculate again
     ↓
Improve again
     ↓
...
     ↓
Approximate solution
```

This general idea appears throughout medical imaging.

Large images and complex models often require computational approximation rather than closed-form mathematical solutions.

---

# 1.43 How Chapter 1 Connects to Future Chapters

This is the key roadmap.

### Mathematics

```text
Functions
Vectors
Matrices
Calculus
Probability
Statistics
Optimization
```

↓

### Classical Image Processing

```text
Pixel operations
Convolution
Filtering
Gradients
Morphology
Fourier
```

↓

### Medical Image Processing

```text
CT
MRI
X-Ray
3D volumes
Segmentation
Registration
```

↓

### Advanced Medical Imaging

```text
Reconstruction
Radiomics
AI
Deep Learning
```

↓

### Software Engineering

```text
C++
Qt
ITK
VTK
DCMTK
CUDA
Architecture
```

This is why we are spending time on mathematics now.

---

# 1.44 Practical C++ Exercise

Create a small C++ program that performs vector operations:

```cpp
#include <iostream>
#include <cmath>

int main()
{
    double x1 = 3.0;
    double y1 = 4.0;

    double magnitude = std::sqrt(x1 * x1 + y1 * y1);

    std::cout << "Magnitude = "
              << magnitude
              << '\n';

    return 0;
}
```

Expected result:

```text
Magnitude = 5
```

Then modify it to calculate the magnitude of a 3D vector:

[
(x,y,z)
]

using:

[
\sqrt{x^2+y^2+z^2}
]

---

# 1.45 Practical Python Exercise

```python
import math

x = 3.0
y = 4.0

magnitude = math.sqrt(x*x + y*y)

print("Magnitude =", magnitude)
```

Then extend it to:

```text
x = 3
y = 4
z = 12
```

and calculate:

[
\sqrt{x^2+y^2+z^2}
]

You should get:

[
13
]

because:

[
3^2+4^2+12^2
============

# 9+16+144

169
]

and:

[
\sqrt{169}=13
]

---

# 1.46 Chapter 1 — Core Mental Model

Remember these relationships:

```text
IMAGE
  ↓
Function I(x,y)
  ↓
Matrix
  ↓
Pixels
  ↓
Numerical operations
```

For geometry:

```text
Coordinates
  ↓
Vectors
  ↓
Matrices
  ↓
Transformations
  ↓
Registration
```

For image boundaries:

```text
Intensity
  ↓
Derivative
  ↓
Gradient
  ↓
Edges
```

For uncertainty:

```text
Probability
  +
Statistics
  ↓
Image analysis
```

For finding the best solution:

```text
Objective
  ↓
Optimization
  ↓
Best solution
```

For computation:

```text
Mathematical problem
  ↓
Numerical method
  ↓
Computational solution
```

---

## Chapter 1 — Final Knowledge Check

Try these without looking back:

1. What is the difference between a scalar, vector, and matrix?
2. Why can an image be represented as a matrix?
3. What is a dot product?
4. How do you calculate vector magnitude?
5. What is matrix multiplication?
6. What is a linear transformation?
7. What does a rotation matrix do?
8. What is an eigenvector?
9. What is an eigenvalue?
10. Why are complex numbers needed for Fourier analysis?
11. What does a derivative represent?
12. Why are partial derivatives required for images?
13. What is an image gradient?
14. What does gradient magnitude represent?
15. What is the difference between probability and statistics?
16. What is variance?
17. What is optimization?
18. Why is optimization important for image registration?
19. What are numerical methods?
20. Explain the relationship:

```text
Mathematics
   ↓
Image Processing
   ↓
Medical Image Processing
```

### Practical challenge

Given:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

calculate:

1. Number of rows
2. Number of columns
3. Mean
4. Minimum
5. Maximum
6. Center value
7. Matrix × 2
8. Matrix + 10

**Chapter 2 — What Is an Image?** 

When you are ready, say **“continue Chapter 1”** and we will go deeper into the mathematics before moving to Chapter 2.
