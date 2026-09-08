# Module 2 → Chapter 18: Eigenvalues and Eigenvectors

Eigenvalues and eigenvectors are important in:

* Image processing
* Computer vision
* Medical imaging
* PCA (Principal Component Analysis)
* Image registration
* Feature detection
* Machine learning
* Data compression

This chapter connects **matrices** with special vectors whose direction remains unchanged after a transformation.

---

# 1. What Is an Eigenvector?

Suppose a matrix transformation changes a vector:

$$
A\vec{v}
$$

Usually, both the **length and direction** of the vector may change.

But some special vectors keep the same direction after transformation.

These are called **eigenvectors**.

Mathematically:

$$
A\vec{v}=\lambda\vec{v}
$$

Where:

* \(A\) = matrix
* \(\vec{v}\) = eigenvector
* \(\lambda\) = eigenvalue

---

# 2. Simple Meaning

An eigenvector is a vector that, after a matrix transformation:

```text
Original Vector
      ↓
Matrix Transformation
      ↓
Same Direction
      +
Different Length
```

The amount by which its length changes is represented by the **eigenvalue**.

---

# 3. Main Eigenvalue Equation

The fundamental equation is:

$$
A\vec{v}=\lambda\vec{v}
$$

Example:

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

Take:

$$
\vec{v}=
\begin{bmatrix}
1\\
0
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
1\\
0
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
2\\
0
\end{bmatrix}
$$

This can be written as:

$$
A\vec{v}
=
2
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

Therefore:

$$
\lambda=2
$$

and:

$$
\vec{v}=
\begin{bmatrix}
1\\
0
\end{bmatrix}
$$

is an eigenvector.

---

# 4. Another Eigenvector

Using the same matrix:

$$
A=
\begin{bmatrix}
2&0\\
0&3
\end{bmatrix}
$$

Take:

$$
\vec{v}=
\begin{bmatrix}
0\\
1
\end{bmatrix}
$$

Then:

$$
A\vec{v}
=
\begin{bmatrix}
0\\
3
\end{bmatrix}
$$

Therefore:

$$
A\vec{v}
=
3\vec{v}
$$

So:

$$
\lambda=3
$$

---

# 5. How to Find Eigenvalues

Start with:

$$
A\vec{v}=\lambda\vec{v}
$$

Move everything to one side:

$$
A\vec{v}-\lambda\vec{v}=0
$$

Factor:

$$
(A-\lambda I)\vec{v}=0
$$

For a non-zero eigenvector:

$$
\det(A-\lambda I)=0
$$

This is called the **characteristic equation**.

---

# 6. Finding Eigenvalues: Example

Suppose:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

We need:

$$
\det(A-\lambda I)=0
$$

Identity matrix:

$$
I=
\begin{bmatrix}
1&0\\
0&1
\end{bmatrix}
$$

Therefore:

$$
A-\lambda I=
\begin{bmatrix}
2-\lambda&1\\
1&2-\lambda
\end{bmatrix}
$$

---

# 7. Calculate the Determinant

For:

$$
\begin{bmatrix}
2-\lambda&1\\
1&2-\lambda
\end{bmatrix}
$$

The determinant is:

$$
(2-\lambda)^2-1=0
$$

Expand:

$$
4-4\lambda+\lambda^2-1=0
$$

$$
\lambda^2-4\lambda+3=0
$$

Factor:

$$
(\lambda-1)(\lambda-3)=0
$$

Therefore:

$$
\lambda_1=1
$$

$$
\lambda_2=3
$$

These are the eigenvalues.

---

# 8. How to Find Eigenvectors

After finding an eigenvalue, solve:

$$
(A-\lambda I)\vec{v}=0
$$

---

# 9. Eigenvector for \(\lambda=3\)

Matrix:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

For:

$$
\lambda=3
$$

Calculate:

$$
A-3I
$$

$$
=
\begin{bmatrix}
-1&1\\
1&-1
\end{bmatrix}
$$

Now solve:

$$
\begin{bmatrix}
-1&1\\
1&-1
\end{bmatrix}
\begin{bmatrix}
x\\
y
\end{bmatrix}
=
\begin{bmatrix}
0\\
0
\end{bmatrix}
$$

From:

$$
-x+y=0
$$

Therefore:

$$
y=x
$$

One possible eigenvector is:

$$
\vec{v}_1=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

---

# 10. Eigenvector for \(\lambda=1\)

Calculate:

$$
A-I
$$

$$
=
\begin{bmatrix}
1&1\\
1&1
\end{bmatrix}
$$

Solve:

$$
x+y=0
$$

Therefore:

$$
y=-x
$$

One possible eigenvector:

$$
\vec{v}_2=
\begin{bmatrix}
1\\
-1
\end{bmatrix}
$$

---

# 11. Important Verification

For:

$$
\lambda=3
$$

and:

$$
\vec{v}=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

Calculate:

$$
A\vec{v}
=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
3\\
3
\end{bmatrix}
$$

And:

$$
3\vec{v}
=
3
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

$$
=
\begin{bmatrix}
3\\
3
\end{bmatrix}
$$

Therefore:

$$
A\vec{v}=\lambda\vec{v}
$$

Correct.

---

# 12. Eigenvectors Are Not Unique

If:

$$
\vec{v}
$$

is an eigenvector, then any non-zero scalar multiple is also an eigenvector.

For example:

$$
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

and:

$$
\begin{bmatrix}
2\\
2
\end{bmatrix}
$$

represent the same eigenvector direction.

Usually, we normalize eigenvectors:

$$
|\vec{v}|=1
$$

---

# 13. Normalized Eigenvector

For:

$$
\vec{v}=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

Magnitude:

$$
|\vec{v}|=
\sqrt{1^2+1^2}
$$

$$
=\sqrt{2}
$$

Normalized vector:

$$
\hat{v}
=
\frac{1}{\sqrt{2}}
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

---

# 14. Geometric Meaning

Imagine a transformation that stretches space.

Most vectors:

```text
Change Direction
+
Change Length
```

Eigenvectors:

```text
Same Direction
+
Length Changes
```

Eigenvalue:

```text
Amount of Scaling
```

---

# 15. Eigenvalue Interpretation

### Positive Eigenvalue

The vector keeps its direction.

### Negative Eigenvalue

The vector reverses direction.

### Eigenvalue Greater Than 1

The vector stretches.

### Eigenvalue Between 0 and 1

The vector shrinks.

### Eigenvalue = 0

The vector is mapped to zero.

---

# 16. Eigenvalue = 1

If:

$$
\lambda=1
$$

Then:

$$
A\vec{v}=\vec{v}
$$

The eigenvector remains unchanged.

---

# 17. Eigenvalue = 0

If:

$$
\lambda=0
$$

Then:

$$
A\vec{v}=0
$$

The eigenvector is mapped to the zero vector.

This indicates a singular matrix:

$$
\det(A)=0
$$

for a square matrix having zero as an eigenvalue.

---

# 18. Trace and Eigenvalues

For a square matrix:

$$
A=
\begin{bmatrix}
a&b\\
c&d
\end{bmatrix}
$$

The trace is:

$$
trace(A)=a+d
$$

For a \(2\times2\) matrix:

$$
\lambda_1+\lambda_2=trace(A)
$$

Example:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

Trace:

$$
2+2=4
$$

Eigenvalues:

$$
1+3=4
$$

Correct.

---

# 19. Determinant and Eigenvalues

For a \(2\times2\) matrix:

$$
\lambda_1\lambda_2=\det(A)
$$

Example:

$$
\det(A)
=
2(2)-1(1)
$$

$$
=3
$$

Eigenvalues:

$$
1\times3=3
$$

Correct.

---

# 20. Symmetric Matrices

A symmetric matrix satisfies:

$$
A=A^T
$$

Example:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

Important properties of real symmetric matrices:

* Eigenvalues are real.
* Eigenvectors corresponding to distinct eigenvalues are orthogonal.
* Eigenvectors can be chosen to form an orthonormal basis.

This is extremely useful in:

* PCA
* Covariance analysis
* Image processing
* Medical image analysis

---

# 21. Orthogonal Eigenvectors

For the example:

$$
\vec{v}_1=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

and:

$$
\vec{v}_2=
\begin{bmatrix}
1\\
-1
\end{bmatrix}
$$

Dot product:

$$
\vec{v}_1\cdot\vec{v}_2
$$

$$
=1(1)+1(-1)
$$

$$
=0
$$

Therefore, they are orthogonal.

---

# 22. Eigen Decomposition

For some matrices, we can write:

$$
A=Q\Lambda Q^{-1}
$$

Where:

* \(Q\) contains eigenvectors.
* \(\Lambda\) is a diagonal matrix containing eigenvalues.

For symmetric matrices:

$$
A=Q\Lambda Q^T
$$

because orthonormal eigenvectors satisfy:

$$
Q^{-1}=Q^T
$$

---

# 23. Eigenvalue Matrix

Suppose eigenvalues are:

$$
\lambda_1=3
$$

$$
\lambda_2=1
$$

Then:

$$
\Lambda=
\begin{bmatrix}
3&0\\
0&1
\end{bmatrix}
$$

---

# 24. Why Eigen Decomposition Matters

A complex transformation can sometimes be understood as:

```text
Original Space
      ↓
Eigenvector Coordinates
      ↓
Independent Scaling
      ↓
Transform Back
```

Eigenvectors provide important directions.

Eigenvalues describe scaling along those directions.

---

# 25. PCA Connection

PCA means:

**Principal Component Analysis**

A simplified process:

```text
Data
 ↓
Mean Centering
 ↓
Covariance Matrix
 ↓
Eigenvalues + Eigenvectors
 ↓
Principal Components
```

Eigenvectors represent important directions of variation.

Eigenvalues indicate how much variance is associated with those directions.

---

# 26. PCA Example

Suppose medical image features have variation mainly in one direction.

PCA can find:

```text
Highest Variation Direction
          ↓
First Principal Component
```

The eigenvector with the largest eigenvalue is associated with the direction of maximum variance.

---

# 27. Image Compression

PCA can reduce data dimensions.

Conceptually:

```text
High-Dimensional Data
        ↓
Find Important Eigenvectors
        ↓
Keep Major Components
        ↓
Reduced Data
```

Applications can include:

* Feature compression
* Data reduction
* Statistical image analysis

---

# 28. Computer Vision Applications

Eigenvalues and eigenvectors appear in methods involving:

### PCA

```text
Feature Data
     ↓
Covariance Matrix
     ↓
Eigen Decomposition
```

### Feature Detection

Some corner and structure analysis methods use eigenvalues of local matrices.

### Shape Analysis

Eigenvectors can describe important modes of variation.

---

# 29. Medical Imaging Applications

### Image Registration

Eigen-related matrix methods can be part of:

* Statistical shape analysis
* Transformation analysis
* Optimization methods

### PCA-Based Analysis

```text
Medical Dataset
      ↓
Feature Extraction
      ↓
Covariance Matrix
      ↓
Eigenvectors
      ↓
Principal Components
```

### Statistical Shape Models

Eigenvectors can represent important modes of anatomical shape variation.

---

# 30. Covariance Matrix

Suppose we have two variables:

$$
X
$$

and:

$$
Y
$$

A covariance matrix can be:

$$
C=
\begin{bmatrix}
Var(X)&Cov(X,Y)\\
Cov(Y,X)&Var(Y)
\end{bmatrix}
$$

For real-valued data, covariance matrices are typically symmetric.

Therefore eigenvalue decomposition is particularly useful.

---

# 31. Step-by-Step Method

To solve an eigenvalue problem:

### Step 1

Start with:

$$
A\vec{v}=\lambda\vec{v}
$$

### Step 2

Write:

$$
(A-\lambda I)\vec{v}=0
$$

### Step 3

Find eigenvalues:

$$
\det(A-\lambda I)=0
$$

### Step 4

Solve for:

$$
\lambda
$$

### Step 5

For each eigenvalue, solve:

$$
(A-\lambda I)\vec{v}=0
$$

### Step 6

Find eigenvectors.

### Step 7

Normalize eigenvectors if needed.

---

# 32. C++ Example: Verify an Eigenvector

```cpp
struct Vector2D
{
    double x;
    double y;
};

struct Matrix2x2
{
    double a;
    double b;
    double c;
    double d;
};

Vector2D multiply(
    const Matrix2x2& matrix,
    const Vector2D& vector)
{
    return
    {
        matrix.a * vector.x +
        matrix.b * vector.y,

        matrix.c * vector.x +
        matrix.d * vector.y
    };
}
```

Example:

```cpp
Matrix2x2 matrix =
{
    2, 1,
    1, 2
};

Vector2D eigenvector =
{
    1,
    1
};

Vector2D result =
    multiply(matrix, eigenvector);
```

Result:

```text
(3, 3)
```

Since:

```text
Eigenvalue = 3
Eigenvector = (1, 1)

3 × (1, 1) = (3, 3)
```

---

# 33. Important Formulas

### Eigenvalue Equation

$$
A\vec{v}=\lambda\vec{v}
$$

### Characteristic Equation

$$
\det(A-\lambda I)=0
$$

### Eigenvector Equation

$$
(A-\lambda I)\vec{v}=0
$$

### Eigen Decomposition

$$
A=Q\Lambda Q^{-1}
$$

For symmetric matrices:

$$
A=Q\Lambda Q^T
$$

---

# 34. Practice Questions

### Question 1

Find the eigenvalues of:

$$
A=
\begin{bmatrix}
4&0\\
0&2
\end{bmatrix}
$$

Answer:

$$
\lambda_1=4
$$

$$
\lambda_2=2
$$

---

### Question 2

For:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

What are the eigenvalues?

Answer:

$$
\lambda_1=3
$$

$$
\lambda_2=1
$$

---

### Question 3

For:

$$
A=
\begin{bmatrix}
2&1\\
1&2
\end{bmatrix}
$$

Find an eigenvector corresponding to:

$$
\lambda=3
$$

Answer:

$$
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

Any non-zero scalar multiple also represents the same eigenvector direction.

---

### Question 4

What does a large eigenvalue represent in PCA?

**Answer:** It indicates a direction associated with a larger amount of variance in the data.

---

# 35. Common Mistakes

### Mistake 1: Forgetting the Identity Matrix

Incorrect:

$$
\det(A-\lambda)=0
$$

Correct:

$$
\det(A-\lambda I)=0
$$

---

### Mistake 2: Thinking Every Vector Is an Eigenvector

Only special vectors satisfy:

$$
A\vec{v}=\lambda\vec{v}
$$

---

### Mistake 3: Assuming Eigenvectors Are Unique

If:

$$
\vec{v}
$$

is an eigenvector, then:

$$
k\vec{v}
$$

is also an eigenvector for any:

$$
k\neq0
$$

---

### Mistake 4: Confusing Eigenvalue and Eigenvector

```text
Eigenvector → Direction

Eigenvalue → Scaling factor
```

---

# 36. Chapter Summary

You learned:

* Eigenvectors
* Eigenvalues
* The eigenvalue equation
* Characteristic equation
* How to calculate eigenvalues
* How to calculate eigenvectors
* Eigenvector normalization
* Geometric interpretation
* Positive and negative eigenvalues
* Zero eigenvalues
* Trace
* Determinant
* Symmetric matrices
* Orthogonal eigenvectors
* Eigen decomposition
* PCA
* Covariance matrices
* Image compression
* Computer vision applications
* Medical imaging applications

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
⬜ Chapter 19: Calculus Fundamentals
⬜ Chapter 20: Partial Derivatives
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 19 — Calculus Fundamentals**
