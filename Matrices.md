# Module 2 → Chapter 15: Matrices

Matrices are **one of the most important mathematical concepts in image processing and medical imaging**.

A digital image itself can be represented as a matrix.

---

# 1. What Is a Matrix?

A matrix is a rectangular arrangement of numbers in **rows and columns**.

Example:

$$
A=
\begin{bmatrix}
1 & 2 & 3\\
4 & 5 & 6
\end{bmatrix}
$$

This matrix has:

* **2 rows**
* **3 columns**

Its size is:

$$
2\times3
$$

---

# 2. Rows and Columns

Consider:

$$
A=
\begin{bmatrix}
1 & 2 & 3\\
4 & 5 & 6
\end{bmatrix}
$$

```text
        Columns
       1   2   3
     ┌─────────────
Row 1│ 1   2   3
Row 2│ 4   5   6
```

---

# 3. Matrix Notation

A matrix is often written as:

$$
A=[a_{ij}]
$$

Where:

$$
i = \text{row index}
$$

$$
j = \text{column index}
$$

Example:

$$
A=
\begin{bmatrix}
1 & 2\\
3 & 4
\end{bmatrix}
$$

Then:

$$
a_{11}=1
$$

$$
a_{12}=2
$$

$$
a_{21}=3
$$

$$
a_{22}=4
$$

---

# 4. Matrix Dimensions

The dimensions of a matrix are:

$$
Rows\times Columns
$$

Examples:

$$
\begin{bmatrix}
1 & 2 & 3
\end{bmatrix}
$$

is:

$$
1\times3
$$

And:

$$
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
$$

is:

$$
3\times1
$$

---

# 5. Common Types of Matrices

## Row Matrix

Only one row:

$$
\begin{bmatrix}
1 & 2 & 3
\end{bmatrix}
$$

---

## Column Matrix

Only one column:

$$
\begin{bmatrix}
1\\
2\\
3
\end{bmatrix}
$$

A vector can be represented as a column matrix.

---

## Square Matrix

Same number of rows and columns.

Example:

$$
\begin{bmatrix}
1 & 2\\
3 & 4
\end{bmatrix}
$$

Size:

$$
2\times2
$$

---

# 6. Image as a Matrix

Consider this small grayscale image:

$$
I=
\begin{bmatrix}
10 & 20 & 30\\
40 & 50 & 60\\
70 & 80 & 90
\end{bmatrix}
$$

Each number represents a pixel intensity.

```text
10  20  30
40  50  60
70  80  90
```

This is a:

$$
3\times3
$$

image matrix.

---

# 7. Real Digital Image as a Matrix

For a grayscale image:

```text
Width × Height
```

Example:

$$
512\times512
$$

A matrix representation:

$$
I(x,y)
$$

Each matrix element stores pixel intensity.

For an 8-bit image:

$$
0\leq I(x,y)\leq255
$$

---

# 8. Color Image as Matrices

A color RGB image usually contains three channels:

```text
Red Matrix
Green Matrix
Blue Matrix
```

Conceptually:

$$
Image=
(R,G,B)
$$

Each channel is a separate matrix.

For example:

$$
R=
\begin{bmatrix}
255 & 100\\
50 & 0
\end{bmatrix}
$$

$$
G=
\begin{bmatrix}
0 & 100\\
200 & 255
\end{bmatrix}
$$

$$
B=
\begin{bmatrix}
50 & 100\\
150 & 255
\end{bmatrix}
$$

---

# 9. Medical Image as a Matrix

A CT slice can be represented as:

$$
I(x,y)
$$

Conceptually:

```text
CT Slice

┌──────────────────┐
│  Matrix of voxel │
│  intensity data  │
│                  │
└──────────────────┘
```

A complete CT volume can be represented as:

$$
I(x,y,z)
$$

This is effectively a 3D array of voxel values.

---

# 10. Matrix Element Access

Suppose:

$$
A=
\begin{bmatrix}
10 & 20 & 30\\
40 & 50 & 60\\
70 & 80 & 90
\end{bmatrix}
$$

Then:

$$
a_{11}=10
$$

$$
a_{23}=60
$$

$$
a_{32}=80
$$

Remember: mathematical indexing usually starts from **1**, while programming often starts from **0**.

---

# 11. C++ Matrix Representation

A fixed-size matrix:

```cpp
int matrix[3][3] =
{
    {10, 20, 30},
    {40, 50, 60},
    {70, 80, 90}
};
```

Access:

```cpp
int value = matrix[1][2];
```

Result:

```text
60
```

Because C++ indexing starts from zero.

---

# 12. Zero Matrix

A zero matrix contains only zeros.

$$
\begin{bmatrix}
0 & 0\\
0 & 0
\end{bmatrix}
$$

Used frequently in:

* Initialization
* Image buffers
* Mathematical operations

---

# 13. Identity Matrix

The identity matrix has:

* `1` on the main diagonal
* `0` everywhere else

Example:

$$
I=
\begin{bmatrix}
1 & 0\\
0 & 1
\end{bmatrix}
$$

For 3D:

$$
I=
\begin{bmatrix}
1&0&0\\
0&1&0\\
0&0&1
\end{bmatrix}
$$

Identity matrices are very important in transformations.

Applying an identity transformation means:

> No transformation.

---

# 14. Diagonal Matrix

A diagonal matrix has values only on the main diagonal.

Example:

$$
A=
\begin{bmatrix}
2&0&0\\
0&5&0\\
0&0&3
\end{bmatrix}
$$

---

# 15. Scalar Matrix

A scalar matrix is a diagonal matrix where all diagonal values are equal.

Example:

$$
A=
\begin{bmatrix}
5&0&0\\
0&5&0\\
0&0&5
\end{bmatrix}
$$

---

# 16. Symmetric Matrix

A matrix is symmetric when:

$$
A=A^T
$$

Example:

$$
A=
\begin{bmatrix}
1&2&3\\
2&4&5\\
3&5&6
\end{bmatrix}
$$

Values are mirrored across the main diagonal.

Symmetric matrices become important later in:

* Statistics
* Covariance matrices
* Eigenvalues
* Image analysis

---

# 17. Matrix Transpose

The transpose changes rows into columns.

Suppose:

$$
A=
\begin{bmatrix}
1&2&3\\
4&5&6
\end{bmatrix}
$$

Then:

$$
A^T=
\begin{bmatrix}
1&4\\
2&5\\
3&6
\end{bmatrix}
$$

---

# 18. Matrix Indexing and Image Coordinates

There is an important difference between mathematical matrices and image coordinates.

Matrix:

$$
A[row][column]
$$

Programming:

```cpp
image[row][column]
```

But image coordinates are often written as:

$$
I(x,y)
$$

Usually:

```text
x → column
y → row
```

Therefore conceptually:

$$
I(x,y)=image[y][x]
$$

This distinction is extremely important in image processing.

---

# 19. Image Coordinate System

A typical image coordinate system:

```text
(0,0) ───────────────→ X
  │
  │
  │
  ▼
  Y
```

Unlike normal mathematical graphs:

```text
Y
↑
│
│
└────────→ X
```

Images often have their origin at the **top-left corner**.

---

# 20. Matrix vs Image

| Matrix    | Image                       |
| --------- | --------------------------- |
| Rows      | Image height                |
| Columns   | Image width                 |
| Element   | Pixel                       |
| Value     | Pixel intensity             |
| 2D matrix | Grayscale image             |
| 3D array  | Volume / multi-channel data |

---

# 21. Matrix Representation of a Small Image

Suppose:

$$
I=
\begin{bmatrix}
0&0&0\\
0&255&0\\
0&0&0
\end{bmatrix}
$$

Visual representation:

```text
⬛ ⬛ ⬛
⬛ ⬜ ⬛
⬛ ⬛ ⬛
```

The center pixel is bright.

---

# 22. Matrix and Image Filters

A small matrix called a **kernel** can be applied to an image.

Example:

$$
K=
\begin{bmatrix}
0&-1&0\\
-1&4&-1\\
0&-1&0
\end{bmatrix}
$$

This type of kernel can be used for edge-related operations.

Later, you will study:

* Convolution
* Filtering
* Kernels
* Gaussian filters
* Edge detection

Matrices are the foundation of all these operations.

---

# 23. 3D Medical Image Volume

A CT volume contains multiple slices:

```text
Slice 0 → Matrix
Slice 1 → Matrix
Slice 2 → Matrix
Slice 3 → Matrix
```

Conceptually:

$$
Volume[x][y][z]
$$

or, depending on implementation:

```cpp
volume[z][y][x]
```

The exact storage order depends on the software and library.

Each element represents a **voxel**.

---

# 24. Pixel vs Voxel

### Pixel

A two-dimensional image element:

$$
(x,y)
$$

### Voxel

A three-dimensional volume element:

$$
(x,y,z)
$$

Examples:

```text
X-ray image → Pixels
CT volume   → Voxels
MRI volume  → Voxels
PET volume  → Voxels
```

---

# 25. Matrix Storage in Memory

A 2D matrix can be stored linearly in memory.

Example matrix:

```text
10 20 30
40 50 60
70 80 90
```

Possible row-major storage:

```text
10 20 30 40 50 60 70 80 90
```

For a matrix with:

```text
Rows = R
Columns = C
```

A common row-major offset is:

$$
index=row\times C+column
$$

Example:

```text
row = 1
column = 2
C = 3
```

$$
index=1\times3+2=5
$$

Value at linear index `5` is:

```text
60
```

This is important for high-performance image processing.

---

# 26. Matrix Storage and Performance

Large medical images can be huge.

Example:

```text
512 × 512 × 300
```

Number of voxels:

$$
512\times512\times300
$$

$$
=78,643,200
$$

So efficient memory access matters.

Conceptually:

```text
Poor memory access
       ↓
Slow processing

Cache-friendly access
       ↓
Faster processing
```

---

# 27. Matrix Representation in Medical Imaging

```text
Patient Scan
     │
     ▼
Medical Image Data
     │
     ├── Pixel Matrix
     │
     ├── Voxel Volume
     │
     ├── Orientation Matrix
     │
     └── Transformation Matrix
```

Matrices are used for both:

1. **Image data**
2. **Image geometry**

---

# 28. Matrix Representation in C++

Simple dynamic conceptual example:

```cpp
int rows = 3;
int columns = 3;

int matrix[3][3];
```

Initialization:

```cpp
int matrix[3][3] = {0};
```

Looping:

```cpp
for (int row = 0; row < rows; ++row)
{
    for (int column = 0;
         column < columns;
         ++column)
    {
        matrix[row][column] = 0;
    }
}
```

---

# 29. Example: Image Matrix Processing

Suppose we increase every pixel by `10`.

```cpp
for (int row = 0; row < 3; ++row)
{
    for (int column = 0;
         column < 3;
         ++column)
    {
        matrix[row][column] += 10;
    }
}
```

Conceptually:

$$
I_{new}=I_{old}+10
$$

This connects **matrix representation** with **pixel operations**.

---

# 30. Matrix Size in Real Medical Images

Common image sizes may include:

```text
256 × 256
512 × 512
1024 × 1024
```

A volume might conceptually be:

```text
512 × 512 × 300
```

But dimensions vary by modality, acquisition protocol, reconstruction, and dataset.

---

# 31. Important Matrix Concepts

At this stage, you should understand:

```text
Matrix
Rows
Columns
Dimensions
Matrix element
Square matrix
Row matrix
Column matrix
Zero matrix
Identity matrix
Diagonal matrix
Scalar matrix
Symmetric matrix
Transpose
Matrix indexing
Image matrix
Voxel volume
Memory layout
```

---

# 32. Practice Questions

### Question 1

Find the dimensions:

$$
A=
\begin{bmatrix}
1&2&3\\
4&5&6
\end{bmatrix}
$$

Answer:

$$
2\times3
$$

---

### Question 2

Find:

$$
a_{23}
$$

for:

$$
A=
\begin{bmatrix}
10&20&30\\
40&50&60
\end{bmatrix}
$$

Answer:

$$
60
$$

---

### Question 3

Find the transpose:

$$
A=
\begin{bmatrix}
1&2\\
3&4\\
5&6
\end{bmatrix}
$$

Answer:

$$
A^T=
\begin{bmatrix}
1&3&5\\
2&4&6
\end{bmatrix}
$$

---

### Question 4

What does this represent?

$$
I=
\begin{bmatrix}
0&255\\
255&0
\end{bmatrix}
$$

Answer:

A small grayscale image where:

```text
Dark  Bright
Bright Dark
```

---

# 33. Chapter Summary

You learned:

* What a matrix is
* Rows and columns
* Matrix dimensions
* Matrix notation
* Matrix elements
* Row and column matrices
* Square matrices
* Images as matrices
* RGB images as multiple matrices
* Medical images and voxel volumes
* Zero matrix
* Identity matrix
* Diagonal matrix
* Scalar matrix
* Symmetric matrix
* Matrix transpose
* Matrix indexing
* Image coordinate systems
* Pixel vs voxel
* Matrix memory storage
* Row-major indexing
* Matrix kernels
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

## Next: **Chapter 16 — Matrix Operations**
