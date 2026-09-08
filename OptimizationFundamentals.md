# Module 2 → Chapter 24: Optimization Fundamentals

Optimization is the process of finding the **best solution** according to a mathematical objective.

It is extremely important in:

* Image processing
* Medical image processing
* Computer vision
* Image registration
* Image reconstruction
* Image segmentation
* Machine learning
* Deep learning
* Treatment planning systems

---

# 1. What Is Optimization?

Suppose we have a function:

$$
f(x)
$$

Optimization means finding the value of \(x\) that gives:

* the **minimum** value, or
* the **maximum** value.

```text
Optimization
│
├── Minimization
│
└── Maximization
```

---

# 2. Simple Example

Consider:

$$
f(x)=x^2
$$

We want to minimize:

$$
f(x)
$$

The minimum occurs at:

$$
\boxed{x=0}
$$

because:

$$
f(0)=0
$$

and:

$$
x^2\geq0
$$

for every real \(x\).

---

# 3. Minimization and Maximization

## Minimization

$$
\boxed{
\min_x f(x)
}
$$

Example:

```text
Find the smallest error.
```

Common applications:

* Image registration error
* Reconstruction error
* Machine learning loss

---

## Maximization

$$
\boxed{
\max_x f(x)
}
$$

Example:

```text
Find the highest similarity.
```

Common applications:

* Image similarity
* Feature matching
* Classification confidence objectives

---

# 4. Objective Function

The function we optimize is called the:

* Objective function
* Cost function
* Loss function
* Energy function

Depending on the application.

For example:

$$
E(\theta)
$$

Where:

* \(E\) = error
* \(\theta\) = parameters

Goal:

$$
\boxed{
\min_\theta E(\theta)
}
$$

---

# 5. Optimization Variables

Suppose:

$$
f(x,y)
$$

We want to find the best:

$$
x
$$

and:

$$
y
$$

Then:

```text
x, y
 ↓
Optimization Variables
```

Example in image registration:

$$
\theta=
\begin{bmatrix}
t_x\\
t_y\\
\theta_r
\end{bmatrix}
$$

Where:

* \(t_x\) = horizontal translation
* \(t_y\) = vertical translation
* \(\theta_r\) = rotation

---

# 6. Unconstrained Optimization

In unconstrained optimization, variables are free within their mathematical domain.

Example:

$$
\min_x x^2
$$

There are no additional constraints.

---

# 7. Constrained Optimization

Constrained optimization has restrictions.

Example:

$$
\min_x f(x)
$$

subject to:

$$
g(x)\leq0
$$

or:

$$
h(x)=0
$$

Example:

```text
Minimize radiation dose

Subject to:

Tumor dose ≥ required dose
Normal organ dose ≤ allowed limit
```

This type of optimization is very important in radiation therapy.

---

# 8. Local Minimum

A local minimum is lower than nearby values.

```text
        /\        /\
       /  \______/  \
             ↑
        Local Minimum
```

Mathematically, it may not be the lowest value over the entire search space.

---

# 9. Global Minimum

The global minimum is the lowest value across the entire domain.

```text
        /\              /\
       /  \____    ____/  \
            \____/
              ↑
        Global Minimum
```

---

# 10. Local vs Global Minimum

```text
Local Minimum
    ↓
Best solution nearby

Global Minimum
    ↓
Best solution overall
```

This distinction is important because many practical optimization problems are **non-convex** and may contain multiple local minima.

---

# 11. Critical Points

For a differentiable function:

$$
f(x)
$$

critical points can occur where:

$$
\boxed{
f'(x)=0
}
$$

For multiple variables:

$$
\boxed{
\nabla f=0
}
$$

Example:

$$
f(x)=x^2
$$

Derivative:

$$
f'(x)=2x
$$

Set:

$$
2x=0
$$

Therefore:

$$
\boxed{x=0}
$$

---

# 12. Second Derivative Test

For a one-dimensional function:

### If:

$$
f'(x)=0
$$

and:

$$
f''(x)>0
$$

then the point is a local minimum.

### If:

$$
f''(x)<0
$$

then the point is a local maximum.

### If:

$$
f''(x)=0
$$

the test is inconclusive and additional analysis may be needed.

---

# 13. Example

Suppose:

$$
f(x)=x^2
$$

First derivative:

$$
f'(x)=2x
$$

Critical point:

$$
x=0
$$

Second derivative:

$$
f''(x)=2
$$

Since:

$$
2>0
$$

there is a minimum at:

$$
\boxed{x=0}
$$

---

# 14. Gradient-Based Optimization

For a multi-variable function:

$$
f(x,y)
$$

we calculate:

$$
\nabla f
$$

The gradient points toward the direction of maximum increase.

Therefore, to minimize a function:

$$
\boxed{
\text{Move opposite to the gradient}
}
$$

This leads to **gradient descent**.

---

# 15. Gradient Descent

The basic update rule is:

$$
\boxed{
x_{new}
=
x_{old}
-
\alpha
\frac{df}{dx}
}
$$

For multiple variables:

$$
\boxed{
\mathbf{x}_{new}
=
\mathbf{x}_{old}
-
\alpha\nabla f(\mathbf{x})
}
$$

Where:

* \(\alpha\) = learning rate or step size
* \(\nabla f\) = gradient

---

# 16. Why Subtract the Gradient?

The gradient points toward maximum increase:

```text
Gradient
    →
Increasing Function
```

To minimize:

```text
Move ←
Opposite Direction
```

Therefore:

$$
-\nabla f
$$

points toward local decrease.

---

# 17. Gradient Descent Example

Suppose:

$$
f(x)=x^2
$$

Then:

$$
f'(x)=2x
$$

Start:

$$
x=4
$$

Choose:

$$
\alpha=0.1
$$

Update:

$$
x_{new}
=
4-0.1(8)
$$

$$
=
4-0.8
$$

$$
=
\boxed{3.2}
$$

Next iteration:

$$
x_{new}
=
3.2-0.1(6.4)
$$

$$
=
\boxed{2.56}
$$

The value gradually moves toward:

$$
x=0
$$

---

# 18. Gradient Descent Process

```text
Initial Parameters
       ↓
Calculate Objective
       ↓
Calculate Gradient
       ↓
Update Parameters
       ↓
Check Convergence
       ↓
Repeat
```

---

# 19. Learning Rate

The learning rate is:

$$
\alpha
$$

It controls the update size.

### Small Learning Rate

```text
Slow
↓
Small Steps
↓
More Iterations
```

### Large Learning Rate

```text
Large Steps
↓
May Overshoot
↓
May Become Unstable
```

---

# 20. Learning Rate Example

Suppose:

$$
x_{new}
=
x-\alpha\nabla f
$$

If:

$$
\alpha=0.001
$$

updates may be very small.

If:

$$
\alpha=10
$$

updates may be too large.

Therefore, choosing a suitable step size is important.

---

# 21. Convergence

Optimization converges when updates become sufficiently small or another stopping criterion is met.

Examples:

$$
|f_{new}-f_{old}|<\epsilon
$$

or:

$$
\|\nabla f\|<\epsilon
$$

Where:

$$
\epsilon
$$

is a small threshold.

---

# 22. Common Stopping Conditions

```text
Stop Optimization When:

✓ Maximum iterations reached
✓ Gradient is sufficiently small
✓ Objective improvement is sufficiently small
✓ Parameter change is sufficiently small
```

Practical algorithms often use multiple stopping conditions.

---

# 23. Convex Functions

A convex function has a shape similar to:

```text
       \      /
        \    /
         \  /
          \/
```

For a convex optimization problem under appropriate conditions, every local minimum is also a global minimum.

Example:

$$
f(x)=x^2
$$

---

# 24. Non-Convex Functions

A non-convex function may contain multiple valleys.

```text
       /\      /\
      /  \____/  \__
```

This may contain:

* Multiple local minima
* Saddle points
* Complex optimization landscapes

Medical imaging and machine learning often involve non-convex optimization problems.

---

# 25. Saddle Points

A saddle point is neither a simple local minimum nor a simple local maximum.

Example function:

$$
f(x,y)=x^2-y^2
$$

At:

$$
(0,0)
$$

the gradient is zero:

$$
\nabla f=0
$$

but the point is not a minimum or maximum.

---

# 26. Hessian Matrix in Optimization

For:

$$
f(x,y)
$$

the Hessian is:

$$
H=
\begin{bmatrix}
f_{xx}&f_{xy}\\
f_{yx}&f_{yy}
\end{bmatrix}
$$

The Hessian contains second-order derivative information.

It can help analyze:

* Curvature
* Minima
* Maxima
* Saddle points

---

# 27. Hessian Classification

For a two-variable function, at a critical point define:

$$
D=f_{xx}f_{yy}-(f_{xy})^2
$$

Then:

### Local Minimum

$$
D>0
$$

and:

$$
f_{xx}>0
$$

---

### Local Maximum

$$
D>0
$$

and:

$$
f_{xx}<0
$$

---

### Saddle Point

$$
D<0
$$

---

# 28. Newton's Method

Newton's method uses second-order information.

For one variable:

$$
\boxed{
x_{new}
=
x-
\frac{f'(x)}
{f''(x)}
}
$$

For multiple variables:

$$
\boxed{
\mathbf{x}_{new}
=
\mathbf{x}_{old}
-
H^{-1}\nabla f
}
$$

Where:

* \(H\) = Hessian matrix
* \(\nabla f\) = gradient

---

# 29. Gradient Descent vs Newton's Method

| Feature               | Gradient Descent        | Newton's Method               |
| --------------------- | ----------------------- | ----------------------------- |
| Uses                  | First derivative        | First + second derivatives    |
| Memory cost           | Usually lower           | Can be higher                 |
| Computation           | Simpler per iteration   | More expensive per iteration  |
| Curvature information | No explicit Hessian     | Uses Hessian                  |
| Common challenge      | Learning-rate selection | Hessian computation/inversion |

---

# 30. Optimization in Image Registration

Suppose:

```text
Fixed Image
     +
Moving Image
```

We define a transformation:

$$
T(\theta)
$$

and an objective function:

$$
E(\theta)
$$

Goal:

$$
\boxed{
\min_\theta E(\theta)
}
$$

Process:

```text
Initialize Transformation
        ↓
Transform Moving Image
        ↓
Calculate Similarity / Error
        ↓
Optimize Parameters
        ↓
Repeat
```

Parameters may include:

* Translation
* Rotation
* Scaling
* Deformation parameters

---

# 31. Optimization in Image Reconstruction

Suppose we want to reconstruct an image:

$$
x
$$

from measured data:

$$
y
$$

A general optimization model may be:

$$
\boxed{
\min_x
\left[
\text{Data Fidelity}
+
\lambda
\text{Regularization}
\right]
}
$$

For example:

$$
\min_x
\|Ax-y\|^2
+
\lambda R(x)
$$

Where:

* \(A\) = system model
* \(y\) = measured data
* \(R(x)\) = regularization term
* \(\lambda\) = regularization weight

---

# 32. What Is Regularization?

Regularization adds preferences or constraints to the optimization problem.

Example:

```text
Data Fitting
    +
Regularization
    ↓
Balanced Solution
```

It can help reduce:

* Overfitting
* Noise amplification
* Unrealistic solutions

---

# 33. Optimization in Image Segmentation

A segmentation algorithm may define an energy:

$$
E
$$

For example:

$$
E=
E_{data}
+
\lambda E_{smoothness}
$$

Goal:

$$
\min E
$$

The optimizer searches for a segmentation that balances:

* Agreement with image data
* Smoothness or other prior assumptions

---

# 34. Optimization in Medical Image Processing

Optimization is used for:

### Image Registration

$$
\min_\theta E(\theta)
$$

Find the best alignment.

---

### Image Reconstruction

$$
\min_x E(x)
$$

Find the best reconstructed image.

---

### Image Segmentation

Find the segmentation that minimizes an energy or maximizes an objective.

---

### Machine Learning

Minimize:

$$
Loss(\theta)
$$

Examples include training models using gradient-based methods.

---

### Radiation Therapy Treatment Planning

A simplified formulation can be written as:

$$
\min
\text{Dose Penalty}
$$

subject to clinical and physical constraints.

Conceptually:

```text
Desired Tumor Dose
       +
Normal Tissue Protection
       +
Machine Constraints
       ↓
Optimization
       ↓
Treatment Plan
```

---

# 35. Optimization Workflow

```text
1. Define Variables
        ↓
2. Define Objective Function
        ↓
3. Define Constraints
        ↓
4. Select Optimization Algorithm
        ↓
5. Initialize Parameters
        ↓
6. Optimize
        ↓
7. Check Convergence
        ↓
8. Validate Solution
```

---

# 36. C++ Example: Gradient Descent

```cpp
#include <iostream>
#include <cmath>

double gradient(double x)
{
    return 2.0 * x;
}

int main()
{
    double x = 4.0;
    double learningRate = 0.1;

    for (int i = 0; i < 20; ++i)
    {
        x = x - learningRate * gradient(x);

        std::cout
            << "Iteration "
            << i + 1
            << ": x = "
            << x
            << '\n';
    }

    return 0;
}
```

This gradually approaches:

$$
x=0
$$

for:

$$
f(x)=x^2
$$

---

# 37. C++ Example: Objective Function

```cpp
#include <iostream>

double objective(double x)
{
    return x * x;
}

double gradient(double x)
{
    return 2.0 * x;
}

int main()
{
    double x = 5.0;
    double learningRate = 0.1;

    for (int iteration = 0;
         iteration < 20;
         ++iteration)
    {
        double grad = gradient(x);

        x = x - learningRate * grad;

        std::cout
            << "x = " << x
            << ", f(x) = "
            << objective(x)
            << '\n';
    }

    return 0;
}
```

---

# 38. Important Optimization Algorithms

At a beginner-to-intermediate level, know these concepts:

```text
Optimization Algorithms
│
├── Gradient Descent
├── Stochastic Gradient Descent
├── Newton's Method
├── Quasi-Newton Methods
├── Conjugate Gradient
├── Coordinate Descent
└── Evolutionary / Population-Based Methods
```

You will encounter more specialized algorithms later in:

* Machine learning
* Computer vision
* Medical image registration
* Image reconstruction
* Treatment planning

---

# 39. Important Formulas

### General Optimization

$$
\min_x f(x)
$$

### Gradient Descent

$$
\boxed{
x_{new}
=
x_{old}
-
\alpha\nabla f(x)
}
$$

### Critical Point

$$
\boxed{
\nabla f=0
}
$$

### Newton's Method

$$
\boxed{
x_{new}
=
x_{old}
-
\frac{f'(x)}
{f''(x)}
}
$$

### Image Reconstruction Example

$$
\boxed{
\min_x
\|Ax-y\|^2+\lambda R(x)
}
$$

---

# 40. Practice Questions

### Question 1

What is the purpose of optimization?

**Answer:** To find parameter values that minimize or maximize an objective function.

---

### Question 2

What direction does the gradient point?

**Answer:** The direction of maximum local increase of a differentiable function.

---

### Question 3

Why does gradient descent subtract the gradient?

**Answer:** Because the negative gradient gives a local direction of decrease.

---

### Question 4

What happens if the learning rate is too large?

**Answer:** The algorithm may overshoot the minimum or become unstable.

---

### Question 5

What happens if the learning rate is too small?

**Answer:** Convergence may become very slow.

---

### Question 6

What is the difference between local and global minimum?

**Answer:**

* Local minimum → lowest relative to nearby points.
* Global minimum → lowest over the entire search domain.

---

### Question 7

Where is optimization used in medical image processing?

**Answer:** Common applications include image registration, reconstruction, segmentation, machine learning, and treatment planning.

---

# 41. Common Mistakes

### Mistake 1: Assuming Every Critical Point Is a Minimum

A point where:

$$
\nabla f=0
$$

can be:

* Minimum
* Maximum
* Saddle point

---

### Mistake 2: Using a Very Large Learning Rate

This can cause:

* Overshooting
* Oscillation
* Divergence

---

### Mistake 3: Ignoring Constraints

Many real medical and engineering problems require constrained optimization.

---

### Mistake 4: Confusing Local and Global Minimum

A good nearby solution is not always the best overall solution.

---

### Mistake 5: Trusting Convergence Without Validation

Optimization convergence does not automatically guarantee that the solution is clinically, physically, or practically valid.

---

# 42. Chapter Summary

You learned:

* Optimization fundamentals
* Minimization
* Maximization
* Objective functions
* Cost functions
* Loss functions
* Optimization variables
* Constraints
* Unconstrained optimization
* Local minima
* Global minima
* Critical points
* Second derivative test
* Gradient-based optimization
* Gradient descent
* Learning rate
* Convergence
* Convex optimization
* Non-convex optimization
* Saddle points
* Hessian matrix
* Newton's method
* Regularization
* Image registration optimization
* Image reconstruction optimization
* Image segmentation optimization
* Medical image optimization
* Radiation therapy optimization

---

# 🎉 Module 2 Completed

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
✅ Chapter 21: Gradient
✅ Chapter 22: Probability
✅ Chapter 23: Statistics
✅ Chapter 24: Optimization Fundamentals
```

## Next: **Module 3 — Digital Image Fundamentals** 🚀
