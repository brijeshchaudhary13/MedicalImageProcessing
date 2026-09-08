# Module 2 → Chapter 20: Partial Derivatives

Partial derivatives are extremely important for:

* Image processing
* Computer vision
* Medical image analysis
* Image gradients
* Edge detection
* Image registration
* Machine learning
* Deep learning
* Optimization

In the previous chapter, we studied derivatives of functions with **one variable**:

$$
y=f(x)
$$

Now we study functions with **multiple variables**:

$$
z=f(x,y)
$$

---

# 1. What Is a Partial Derivative?

Suppose:

$$
f(x,y)
$$

depends on two variables:

```text
x → Horizontal direction
y → Vertical direction
```

A partial derivative measures how the function changes with respect to **one variable while keeping the other variable constant**.

---

# 2. Main Notation

For:

$$
f(x,y)
$$

Partial derivative with respect to \(x\):

$$
\frac{\partial f}{\partial x}
$$

Partial derivative with respect to \(y\):

$$
\frac{\partial f}{\partial y}
$$

The symbol:

$$
\partial
$$

means **partial derivative**.

---

# 3. Simple Example

Suppose:

$$
f(x,y)=x^2+3y
$$

## Partial Derivative with Respect to \(x\)

Treat \(y\) as a constant:

$$
\frac{\partial f}{\partial x}
=
2x
$$

because:

$$
\frac{\partial}{\partial x}(3y)=0
$$

Therefore:

$$
\boxed{
\frac{\partial f}{\partial x}=2x
}
$$

---

## Partial Derivative with Respect to \(y\)

Now treat \(x\) as constant:

$$
\frac{\partial f}{\partial y}
=
3
$$

Therefore:

$$
\boxed{
\frac{\partial f}{\partial y}=3
}
$$

---

# 4. Important Rule

When differentiating with respect to one variable:

```text
Differentiate one variable
        +
Treat all other variables as constants
```

Example:

$$
f(x,y)=5x^2y
$$

### With respect to \(x\):

$$
\frac{\partial f}{\partial x}
=
10xy
$$

because \(y\) is constant.

### With respect to \(y\):

$$
\frac{\partial f}{\partial y}
=
5x^2
$$

because \(x\) is constant.

---

# 5. More Examples

Suppose:

$$
f(x,y)=x^3+2xy+y^2
$$

## With Respect to \(x\)

Treat \(y\) as constant:

$$
\frac{\partial f}{\partial x}
=
3x^2+2y
$$

---

## With Respect to \(y\)

Treat \(x\) as constant:

$$
\frac{\partial f}{\partial y}
=
2x+2y
$$

---

# 6. Geometric Meaning

A function:

$$
z=f(x,y)
$$

can be imagined as a surface.

```text
           z
           ↑
           │
        Surface
         /\
        /  \
       /____\
      x      y
```

The partial derivative:

$$
\frac{\partial f}{\partial x}
$$

measures the slope when moving in the \(x\)-direction.

The partial derivative:

$$
\frac{\partial f}{\partial y}
$$

measures the slope when moving in the \(y\)-direction.

---

# 7. Image Interpretation

A grayscale image can be represented as:

$$
I(x,y)
$$

Where:

* \(x\) = horizontal position
* \(y\) = vertical position
* \(I(x,y)\) = pixel intensity

The partial derivatives are:

$$
\frac{\partial I}{\partial x}
$$

and:

$$
\frac{\partial I}{\partial y}
$$

These measure intensity changes in horizontal and vertical directions.

---

# 8. Horizontal Intensity Change

$$
\frac{\partial I}{\partial x}
$$

Conceptually:

```text
Dark      Bright
██████│░░░░░░
      ↑
Strong change
```

A large value can indicate a strong intensity transition along the \(x\)-direction.

---

# 9. Vertical Intensity Change

$$
\frac{\partial I}{\partial y}
$$

Conceptually:

```text
████████
████████
────────  ← intensity change
░░░░░░░░
░░░░░░░░
```

A large value indicates strong intensity change along the \(y\)-direction.

---

# 10. First-Order Partial Derivatives

For an image:

$$
I(x,y)
$$

First-order derivatives are:

$$
I_x=
\frac{\partial I}{\partial x}
$$

and:

$$
I_y=
\frac{\partial I}{\partial y}
$$

These are fundamental for:

* Edge detection
* Gradient calculation
* Image registration
* Feature detection

---

# 11. Discrete Partial Derivatives

Digital images are discrete.

Therefore, continuous derivatives are approximated.

For the \(x\)-direction:

$$
\frac{\partial I}{\partial x}
\approx
I(x+1,y)-I(x,y)
$$

For the \(y\)-direction:

$$
\frac{\partial I}{\partial y}
\approx
I(x,y+1)-I(x,y)
$$

---

# 12. Forward Difference

A forward difference approximation:

$$
I_x(x,y)
\approx
I(x+1,y)-I(x,y)
$$

Example:

```text
Pixel values:

10 → 20

Difference:

20 - 10 = 10
```

---

# 13. Backward Difference

$$
I_x(x,y)
\approx
I(x,y)-I(x-1,y)
$$

Example:

```text
10 ← 20

Difference:

20 - 10 = 10
```

---

# 14. Central Difference

A commonly useful approximation is:

$$
\frac{\partial I}{\partial x}
\approx
\frac{I(x+1,y)-I(x-1,y)}{2}
$$

Similarly:

$$
\frac{\partial I}{\partial y}
\approx
\frac{I(x,y+1)-I(x,y-1)}{2}
$$

Central differences often provide a more balanced approximation than using only one neighboring side.

---

# 15. Example Using a Small Image

Suppose:

$$
I=
\begin{bmatrix}
10&20&30\\
15&25&35\\
20&30&40
\end{bmatrix}
$$

At the center:

$$
I(2,2)=25
$$

Using central difference for \(x\):

$$
I_x
\approx
\frac{35-15}{2}
$$

$$
=10
$$

Using central difference for \(y\):

$$
I_y
\approx
\frac{30-20}{2}
$$

$$
=5
$$

Therefore:

```text
Horizontal change = 10
Vertical change   = 5
```

---

# 16. Second-Order Partial Derivatives

We can differentiate partial derivatives again.

For \(x\):

$$
\frac{\partial^2f}{\partial x^2}
$$

For \(y\):

$$
\frac{\partial^2f}{\partial y^2}
$$

These measure how the first derivative changes.

---

# 17. Example of Second Partial Derivatives

Suppose:

$$
f(x,y)=x^3+xy^2
$$

First derivative with respect to \(x\):

$$
f_x=3x^2+y^2
$$

Differentiate again:

$$
f_{xx}=6x
$$

---

With respect to \(y\):

$$
f_y=2xy
$$

Differentiate again:

$$
f_{yy}=2x
$$

---

# 18. Mixed Partial Derivatives

We can differentiate with respect to different variables.

First:

$$
\frac{\partial f}{\partial x}
$$

Then differentiate with respect to \(y\):

$$
\frac{\partial^2f}{\partial y\partial x}
$$

This is called a **mixed partial derivative**.

---

# 19. Example of Mixed Partial Derivative

Suppose:

$$
f(x,y)=x^2y+3xy^2
$$

First differentiate with respect to \(x\):

$$
f_x=2xy+3y^2
$$

Then with respect to \(y\):

$$
f_{xy}=2x+6y
$$

Now reverse the order.

First:

$$
f_y=x^2+6xy
$$

Then:

$$
f_{yx}=2x+6y
$$

Therefore:

$$
f_{xy}=f_{yx}
$$

for this smooth function.

---

# 20. Hessian Matrix

Second-order partial derivatives can be organized into the **Hessian matrix**.

For:

$$
f(x,y)
$$

the Hessian is:

$$
H=
\begin{bmatrix}
\frac{\partial^2f}{\partial x^2}
&
\frac{\partial^2f}{\partial x\partial y}
\\
\frac{\partial^2f}{\partial y\partial x}
&
\frac{\partial^2f}{\partial y^2}
\end{bmatrix}
$$

The Hessian is important in:

* Optimization
* Feature detection
* Image analysis
* Machine learning

---

# 21. Example: Hessian

Suppose:

$$
f(x,y)=x^2+3xy+y^2
$$

First derivatives:

$$
f_x=2x+3y
$$

$$
f_y=3x+2y
$$

Second derivatives:

$$
f_{xx}=2
$$

$$
f_{yy}=2
$$

$$
f_{xy}=3
$$

$$
f_{yx}=3
$$

Therefore:

$$
H=
\begin{bmatrix}
2&3\\
3&2
\end{bmatrix}
$$

---

# 22. Connection Between Partial Derivatives and Gradient

The gradient combines all first-order partial derivatives.

For:

$$
f(x,y)
$$

the gradient is:

$$
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x}\\
\frac{\partial f}{\partial y}
\end{bmatrix}
$$

Example:

$$
f(x,y)=x^2+y^2
$$

Then:

$$
\frac{\partial f}{\partial x}=2x
$$

$$
\frac{\partial f}{\partial y}=2y
$$

Therefore:

$$
\nabla f=
\begin{bmatrix}
2x\\
2y
\end{bmatrix}
$$

We will study gradients deeply in the next chapter.

---

# 23. Partial Derivatives in Image Processing

For an image:

$$
I(x,y)
$$

calculate:

$$
I_x
$$

and:

$$
I_y
$$

Then:

```text
Image
  ↓
Partial Derivatives
  ↓
Gradient
  ↓
Edge Detection
```

---

# 24. Sobel Operator

The Sobel operator estimates image intensity derivatives.

A common \(x\)-direction Sobel kernel is:

$$
G_x=
\begin{bmatrix}
-1&0&1\\
-2&0&2\\
-1&0&1
\end{bmatrix}
$$

A common \(y\)-direction Sobel kernel is:

$$
G_y=
\begin{bmatrix}
-1&-2&-1\\
0&0&0\\
1&2&1
\end{bmatrix}
$$

These estimate intensity changes in two perpendicular directions.

---

# 25. Partial Derivatives in Medical Imaging

Partial derivatives are used in:

### Edge Detection

```text
CT / MRI Image
      ↓
Partial Derivatives
      ↓
Strong Intensity Changes
      ↓
Possible Boundaries
```

### Image Registration

```text
Moving Image
      ↓
Calculate Similarity Error
      ↓
Partial Derivatives
      ↓
Update Transformation
```

### Image Segmentation

Partial derivatives can help analyze:

* Boundaries
* Local intensity changes
* Image structures

---

# 26. Optimization Connection

Suppose:

$$
f(x,y)
$$

is an error function.

To minimize it, calculate:

$$
\frac{\partial f}{\partial x}
$$

and:

$$
\frac{\partial f}{\partial y}
$$

Together:

$$
\nabla f
$$

The gradient provides local information about how the function changes.

Optimization methods can use this information to update parameters.

---

# 27. Finding Critical Points

For:

$$
f(x,y)
$$

a critical point may occur when:

$$
\frac{\partial f}{\partial x}=0
$$

and:

$$
\frac{\partial f}{\partial y}=0
$$

Example:

$$
f(x,y)=x^2+y^2
$$

Then:

$$
f_x=2x
$$

$$
f_y=2y
$$

Set both equal to zero:

$$
x=0
$$

$$
y=0
$$

Therefore:

$$
(0,0)
$$

is a critical point.

---

# 28. Partial Derivative Rules

The rules are similar to ordinary differentiation.

### Constant Rule

$$
\frac{\partial}{\partial x}(c)=0
$$

### Power Rule

$$
\frac{\partial}{\partial x}(x^n)=nx^{n-1}
$$

### Product Rule

$$
\frac{\partial}{\partial x}(uv)
=
u_xv+uv_x
$$

### Chain Rule

If:

$$
z=f(g(x,y))
$$

then partial derivatives use the chain rule according to the variables involved.

---

# 29. C++ Example: Discrete Image Derivative

```cpp
int derivativeX(
    const int leftPixel,
    const int rightPixel)
{
    return (rightPixel - leftPixel) / 2;
}
```

Example:

```cpp
int left = 15;
int right = 35;

int gradientX =
    derivativeX(left, right);
```

Result:

```text
(35 - 15) / 2 = 10
```

---

# 30. Important Formulas

### First Partial Derivative

$$
f_x=
\frac{\partial f}{\partial x}
$$

$$
f_y=
\frac{\partial f}{\partial y}
$$

### Second Partial Derivative

$$
f_{xx}
=
\frac{\partial^2f}{\partial x^2}
$$

$$
f_{yy}
=
\frac{\partial^2f}{\partial y^2}
$$

### Mixed Partial Derivative

$$
f_{xy}
=
\frac{\partial^2f}{\partial y\partial x}
$$

### Gradient

$$
\nabla f=
\begin{bmatrix}
f_x\\
f_y
\end{bmatrix}
$$

### Hessian

$$
H=
\begin{bmatrix}
f_{xx}&f_{xy}\\
f_{yx}&f_{yy}
\end{bmatrix}
$$

---

# 31. Practice Questions

### Question 1

Given:

$$
f(x,y)=x^2+4y
$$

Find:

$$
\frac{\partial f}{\partial x}
$$

**Answer:**

$$
2x
$$

---

### Question 2

Given:

$$
f(x,y)=x^2+4y
$$

Find:

$$
\frac{\partial f}{\partial y}
$$

**Answer:**

$$
4
$$

---

### Question 3

Given:

$$
f(x,y)=x^2y
$$

Find:

$$
\frac{\partial f}{\partial x}
$$

**Answer:**

$$
2xy
$$

---

### Question 4

Given:

$$
f(x,y)=x^2y
$$

Find:

$$
\frac{\partial f}{\partial y}
$$

**Answer:**

$$
x^2
$$

---

### Question 5

What do \(I_x\) and \(I_y\) represent in an image?

**Answer:**

$$
I_x
$$

represents the approximate intensity change with respect to the \(x\)-direction, while:

$$
I_y
$$

represents the approximate intensity change with respect to the \(y\)-direction.

---

# 32. Common Mistakes

### Mistake 1: Differentiating Every Variable

For:

$$
\frac{\partial}{\partial x}
$$

all other variables must be treated as constants.

---

### Mistake 2: Confusing Partial and Ordinary Derivatives

```text
One variable:
dy/dx

Multiple variables:
∂f/∂x
```

---

### Mistake 3: Ignoring Mixed Derivatives

Mixed derivatives are important for:

* Hessian matrices
* Optimization
* Image feature analysis

---

### Mistake 4: Forgetting Digital Images Are Discrete

Continuous partial derivatives are approximated using neighboring pixels in digital image processing.

---

# 33. Chapter Summary

You learned:

* Functions of multiple variables
* Partial derivatives
* Partial derivative notation
* Differentiating one variable while keeping others constant
* Geometric interpretation
* First-order partial derivatives
* Discrete derivatives
* Forward difference
* Backward difference
* Central difference
* Second-order partial derivatives
* Mixed partial derivatives
* Hessian matrix
* Connection between partial derivatives and gradients
* Sobel derivative estimation
* Critical points
* Partial derivatives in image processing
* Partial derivatives in medical imaging
* Partial derivatives in optimization

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
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 21 — Gradient**
