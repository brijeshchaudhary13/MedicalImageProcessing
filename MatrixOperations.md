# Module 2 → Chapter 16: Matrix Operations

Now that you understand **what matrices are**, we will learn how to perform operations on them.

Matrix operations are fundamental for:

* Image transformations
* Image filtering
* Computer vision
* Medical image registration
* CT/MRI processing
* Machine learning
* Deep learning
* 3D visualization

---

# 1. Main Matrix Operations

We will learn:

```text
1. Matrix Addition
2. Matrix Subtraction
3. Scalar Multiplication
4. Matrix Multiplication
5. Element-wise Multiplication
6. Matrix Transpose
7. Determinant
8. Matrix Inverse
```

---

# 2. Matrix Addition

Two matrices can be added only when they have the **same dimensions**.

Suppose:

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

and:

$$
B=
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
$$

Then:

$$
A+B
=
\begin{bmatrix}
1+5&2+6\\
3+7&4+8
\end{bmatrix}
$$

Therefore:

$$
A+B=
\begin{bmatrix}
6&8\\
10&12
\end{bmatrix}
$$

---

# 3. Image Addition

Suppose two image matrices are:

$$
I_1=
\begin{bmatrix}
10&20\\
30&40
\end{bmatrix}
$$

$$
I_2=
\begin{bmatrix}
5&10\\
15&20
\end{bmatrix}
$$

Then:

$$
I_1+I_2
=
\begin{bmatrix}
15&30\\
45&60
\end{bmatrix}
$$

Conceptually:

```text
Image 1
   +
Image 2
   ↓
Combined Image
```

In real image processing, pixel values may need **clamping** to the valid intensity range.

---

# 4. Matrix Subtraction

Again, matrices must have the same dimensions.

$$
A-B
$$

Example:

$$
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
-
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

Result:

$$
=
\begin{bmatrix}
4&4\\
4&4
\end{bmatrix}
$$

---

# 5. Image Difference

Image subtraction is useful for comparing images.

$$
D=I_1-I_2
$$

Often we use absolute difference:

$$
D=|I_1-I_2|
$$

Example:

$$
I_1=100
$$

$$
I_2=70
$$

Then:

$$
D=|100-70|=30
$$

Used in:

* Change detection
* Image registration
* Motion detection
* Image comparison

---

# 6. Scalar Multiplication

A matrix can be multiplied by a single number.

Suppose:

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

Multiply by:

$$
2
$$

Then:

$$
2A=
\begin{bmatrix}
2&4\\
6&8
\end{bmatrix}
$$

---

# 7. Image Intensity Scaling

Suppose:

$$
I_{new}=aI
$$

Where:

$$
a
$$

is a scalar.

Example:

$$
a=2
$$

$$
I=
\begin{bmatrix}
10&20\\
30&40
\end{bmatrix}
$$

Then:

$$
2I=
\begin{bmatrix}
20&40\\
60&80
\end{bmatrix}
$$

This concept is related to intensity scaling and contrast.

---

# 8. Matrix Multiplication

This is one of the most important operations.

Suppose:

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
$$

and:

$$
B=
\begin{bmatrix}
e&f\\
g&h
\end{bmatrix}
$$

Then:

$$
AB=
\begin{bmatrix}
ae+bg&af+bh\\
ce+dg&cf+dh
\end{bmatrix}
$$

---

# 9. Matrix Multiplication Example

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

$$
B=
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
$$

Calculate:

$$
AB
$$

### First element

$$
1(5)+2(7)=19
$$

### Second element

$$
1(6)+2(8)=22
$$

### Third element

$$
3(5)+4(7)=43
$$

### Fourth element

$$
3(6)+4(8)=50
$$

Therefore:

$$
AB=
\begin{bmatrix}
19&22\\
43&50
\end{bmatrix}
$$

---

# 10. Rule for Matrix Multiplication

Suppose:

$$
A
$$

has dimensions:

$$
m\times n
$$

and:

$$
B
$$

has dimensions:

$$
n\times p
$$

Then:

$$
AB
$$

is valid and has dimensions:

$$
m\times p
$$

The **inner dimensions must match**.

```text
(m × n)(n × p)
        ↑
     Must match
```

---

# 11. Example of Matrix Dimensions

Suppose:

$$
A=2\times3
$$

and:

$$
B=3\times4
$$

Then multiplication is valid:

$$
(2\times3)(3\times4)
$$

Result:

$$
2\times4
$$

---

# 12. Matrix Multiplication Is Not Commutative

Usually:

$$
AB\neq BA
$$

This is very important.

Example:

```text
A × B ≠ B × A
```

Order matters in:

* Rotation
* Scaling
* Translation
* Image transformations

---

# 13. Matrix × Vector

Matrices are commonly used to transform vectors.

Suppose:

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

Vector:

$$
v=
\begin{bmatrix}
1\\
2
\end{bmatrix}
$$

Then:

$$
Av
=
\begin{bmatrix}
2(1)+0(2)\\
0(1)+3(2)
\end{bmatrix}
$$

Result:

$$
Av=
\begin{bmatrix}
2\\
6
\end{bmatrix}
$$

This will become extremely important in **Chapter 17: Linear Transformations**.

---

# 14. Element-wise Multiplication

Element-wise multiplication is different from matrix multiplication.

Suppose:

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

$$
B=
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
$$

Element-wise multiplication:

$$
A\odot B
$$

Result:

$$
=
\begin{bmatrix}
1(5)&2(6)\\
3(7)&4(8)
\end{bmatrix}
$$

Therefore:

$$
A\odot B=
\begin{bmatrix}
5&12\\
21&32
\end{bmatrix}
$$

Do not confuse this with:

$$
AB=
\begin{bmatrix}
19&22\\
43&50
\end{bmatrix}
$$

They are completely different operations.

---

# 15. Matrix Transpose

For:

$$
A=
\begin{bmatrix}
1&2&3\\
4&5&6
\end{bmatrix}
$$

Transpose:

$$
A^T=
\begin{bmatrix}
1&4\\
2&5\\
3&6
\end{bmatrix}
$$

Rows become columns.

---

# 16. Important Transpose Rules

### Double transpose

$$
(A^T)^T=A
$$

### Addition

$$
(A+B)^T=A^T+B^T
$$

### Multiplication

$$
(AB)^T=B^TA^T
$$

Notice:

```text
Order reverses.
```

---

# 17. Determinant

The determinant is defined for **square matrices**.

For:

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
$$

The determinant is:

$$
\det(A)=ad-bc
$$

---

# 18. Determinant Example

$$
A=
\begin{bmatrix}
2&3\\
1&4
\end{bmatrix}
$$

Then:

$$
\det(A)=2(4)-3(1)
$$

$$
=8-3
$$

$$
=5
$$

---

# 19. Why Determinant Matters

The determinant helps determine whether a square matrix is invertible.

If:

$$
\det(A)=0
$$

then the matrix is **singular** and generally has no ordinary inverse.

If:

$$
\det(A)\neq0
$$

then the matrix is invertible.

Determinants also have geometric interpretations related to area and volume scaling.

---

# 20. Matrix Inverse

The inverse of matrix \(A\) is:

$$
A^{-1}
$$

If it exists:

$$
AA^{-1}=I
$$

Where \(I\) is the identity matrix.

---

# 21. Inverse of a 2×2 Matrix

For:

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
$$

The inverse is:

$$
A^{-1}
=
\frac{1}{ad-bc}
\begin{bmatrix}
d&-b\\
-c&a
\end{bmatrix}
$$

This is valid only if:

$$
ad-bc\neq0
$$

---

# 22. Inverse Example

Suppose:

$$
A=
\begin{bmatrix}
2&1\\
1&1
\end{bmatrix}
$$

Determinant:

$$
\det(A)=2(1)-1(1)=1
$$

Therefore:

$$
A^{-1}
=
\begin{bmatrix}
1&-1\\
-1&2
\end{bmatrix}
$$

---

# 23. Why Inverse Matters in Image Processing

Suppose an image transformation is:

$$
p'=Ap
$$

Where:

* \(p\) = original point
* \(A\) = transformation matrix
* \(p'\) = transformed point

To recover the original:

$$
p=A^{-1}p'
$$

Inverse transformations are important in:

* Image rotation
* Scaling
* Registration
* Coordinate mapping

---

# 24. Image Transformation Pipeline

Suppose:

```text
Original Point
      │
      ▼
Transformation A
      │
      ▼
Transformation B
      │
      ▼
Final Point
```

Mathematically:

$$
p'=BAp
$$

Order matters.

Applying \(A\) first and then \(B\):

$$
p'=B(Ap)
$$

---

# 25. Matrix Operations on Images

A simple image:

$$
I=
\begin{bmatrix}
10&20\\
30&40
\end{bmatrix}
$$

Brightness addition:

$$
I'=I+10
$$

Result:

$$
I'=
\begin{bmatrix}
20&30\\
40&50
\end{bmatrix}
$$

Contrast scaling:

$$
I'=2I
$$

Result:

$$
I'=
\begin{bmatrix}
20&40\\
60&80
\end{bmatrix}
$$

---

# 26. Important Warning: Image Matrix Operations

Not every mathematical matrix operation directly corresponds to the same image-processing operation.

For example:

```text
Matrix multiplication
≠
Convolution
≠
Element-wise multiplication
```

These are different concepts.

We will study **convolution** separately in image filtering.

---

# 27. C++ Example: Matrix Addition

```cpp
const int rows = 2;
const int cols = 2;

int A[rows][cols] =
{
    {1, 2},
    {3, 4}
};

int B[rows][cols] =
{
    {5, 6},
    {7, 8}
};

int result[rows][cols];

for (int i = 0; i < rows; ++i)
{
    for (int j = 0; j < cols; ++j)
    {
        result[i][j] =
            A[i][j] + B[i][j];
    }
}
```

Result:

```text
6   8
10  12
```

---

# 28. C++ Example: Matrix Multiplication

```cpp
const int rowsA = 2;
const int colsA = 2;

const int rowsB = 2;
const int colsB = 2;

int A[rowsA][colsA] =
{
    {1, 2},
    {3, 4}
};

int B[rowsB][colsB] =
{
    {5, 6},
    {7, 8}
};

int result[rowsA][colsB] = {0};

for (int i = 0; i < rowsA; ++i)
{
    for (int j = 0; j < colsB; ++j)
    {
        for (int k = 0; k < colsA; ++k)
        {
            result[i][j] +=
                A[i][k] *
                B[k][j];
        }
    }
}
```

Result:

```text
19  22
43  50
```

---

# 29. Matrix Multiplication Visualization

For:

$$
A\times B
$$

Each output element is:

```text
Row from A
    ×
Column from B
    ↓
Sum
    ↓
One output value
```

Example:

$$
C_{ij}
=
\sum_k A_{ik}B_{kj}
$$

This formula is fundamental.

---

# 30. Matrix Operations in Medical Imaging

Matrices are used for:

### Image Geometry

```text
Image Coordinates
      ↓
Transformation Matrix
      ↓
New Coordinates
```

### Image Registration

```text
Moving Image
      ↓
Transformation Matrix
      ↓
Fixed Image Space
```

### 3D Visualization

```text
Model Coordinates
      ↓
Rotation Matrix
      ↓
Screen / View Coordinates
```

### Machine Learning

```text
Input Matrix
      ↓
Weight Matrix
      ↓
Output Vector
```

---

# 31. Important Formulas

### Matrix Addition

$$
C=A+B
$$

### Matrix Subtraction

$$
C=A-B
$$

### Scalar Multiplication

$$
C=kA
$$

### Matrix Multiplication

$$
C=AB
$$

### Element-wise Multiplication

$$
C=A\odot B
$$

### Transpose

$$
C=A^T
$$

### Determinant

$$
\det
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
=
ad-bc
$$

### Inverse

$$
A^{-1}
=
\frac{1}{ad-bc}
\begin{bmatrix}
d&-b\\
-c&a
\end{bmatrix}
$$

---

# 32. Practice Questions

### Question 1: Addition

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

$$
B=
\begin{bmatrix}
5&6\\
7&8
\end{bmatrix}
$$

Find:

$$
A+B
$$

Answer:

$$
\begin{bmatrix}
6&8\\
10&12
\end{bmatrix}
$$

---

### Question 2: Scalar Multiplication

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

Find:

$$
3A
$$

Answer:

$$
\begin{bmatrix}
3&6\\
9&12
\end{bmatrix}
$$

---

### Question 3: Determinant

Find:

$$
\det
\begin{bmatrix}
3&2\\
1&4
\end{bmatrix}
$$

Answer:

$$
3(4)-2(1)
$$

$$
=10
$$

---

### Question 4: Matrix Multiplication

$$
A=
\begin{bmatrix}
1&2\\
3&4
\end{bmatrix}
$$

$$
B=
\begin{bmatrix}
2&0\\
1&2
\end{bmatrix}
$$

Calculate \(AB\):

$$
AB=
\begin{bmatrix}
4&4\\
10&8
\end{bmatrix}
$$

---

# 33. Chapter Summary

You learned:

* Matrix addition
* Matrix subtraction
* Image difference
* Scalar multiplication
* Matrix multiplication
* Matrix dimension rules
* Matrix-vector multiplication
* Element-wise multiplication
* Matrix transpose
* Determinant
* Matrix inverse
* Singular matrices
* Transformation pipelines
* Importance of multiplication order
* Image matrix operations
* C++ matrix operations

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
⬜ Chapter 17: Linear Transformations
⬜ Chapter 18: Eigenvalues and Eigenvectors
⬜ Chapter 19: Calculus Fundamentals
⬜ Chapter 20: Partial Derivatives
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 17 — Linear Transformations**
