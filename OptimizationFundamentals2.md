# Chapter 24: Optimization Fundamentals

This is **one complete chapter** containing all fundamental optimization concepts required before advanced topics in:

* Digital Image Processing
* Medical Image Processing
* Computer Vision
* Machine Learning
* Deep Learning
* Image Registration
* Image Reconstruction
* Image Segmentation
* Treatment Planning Systems (TPS)

---

# 1. Introduction to Optimization

Optimization means finding the **best possible solution** to a problem.

Mathematically:

$$
\boxed{\text{Find the best value of one or more variables}}
$$

There are two major goals:

$$
\boxed{\text{Minimization}}
$$

and

$$
\boxed{\text{Maximization}}
$$

---

# 2. Why Do We Need Optimization?

Many real-world problems have multiple possible solutions.

Example:

```text
Many Possible Solutions
        ↓
Evaluate Each Solution
        ↓
Find Best Solution
        ↓
Optimization
```

Examples in medical imaging:

* Best image alignment
* Best image reconstruction
* Best segmentation
* Minimum image error
* Best machine-learning model parameters
* Best radiation treatment plan

---

# 3. Basic Optimization Problem

A general optimization problem can be written as:

$$
\boxed{
\min_x f(x)
}
$$

Where:

* \(x\) = variable or parameter
* \(f(x)\) = objective function

The goal is to find:

$$
x^*
$$

such that:

$$
f(x^*) \leq f(x)
$$

for all feasible \(x\).

---

# 4. Minimization

Minimization means finding the smallest value.

Example:

$$
f(x)=x^2
$$

We want:

$$
\min_x x^2
$$

Since:

$$
x^2\geq0
$$

the minimum occurs at:

$$
\boxed{x=0}
$$

and:

$$
\boxed{f(0)=0}
$$

---

# 5. Maximization

Maximization means finding the largest value.

Mathematically:

$$
\boxed{
\max_x f(x)
}
$$

Example:

Find the maximum value of:

$$
f(x)=10-(x-3)^2
$$

The maximum occurs at:

$$
\boxed{x=3}
$$

because:

$$
f(3)=10
$$

---

# 6. Objective Function

The function we want to optimize is called an **objective function**.

Other names include:

* Cost function
* Loss function
* Energy function
* Error function

Example:

$$
E(x)
$$

Goal:

$$
\boxed{
\min_x E(x)
}
$$

---

# 7. Optimization Variables

The values that optimization changes are called:

$$
\boxed{\text{Optimization Variables}}
$$

Example:

$$
f(x,y)
$$

The variables are:

$$
x,\ y
$$

For image registration:

$$
\theta=
\begin{bmatrix}
t_x\\
t_y\\
r
\end{bmatrix}
$$

Where:

* \(t_x\) = translation in x-direction
* \(t_y\) = translation in y-direction
* \(r\) = rotation parameter

The optimizer tries to find the best values of:

$$
\theta
$$

---

# 8. Optimization Problem Components

A general optimization problem contains:

```text
Optimization Problem
│
├── Variables
├── Objective Function
├── Constraints
├── Initial Solution
├── Optimization Algorithm
└── Stopping Condition
```

---

# 9. Unconstrained Optimization

In unconstrained optimization, variables do not have additional restrictions.

Example:

$$
\boxed{
\min_x x^2
}
$$

There are no constraints.

---

# 10. Constrained Optimization

Constrained optimization includes restrictions.

General form:

$$
\min_x f(x)
$$

subject to:

$$
g(x)\leq0
$$

and/or:

$$
h(x)=0
$$

Where:

* \(g(x)\) = inequality constraint
* \(h(x)\) = equality constraint

---

# 11. Constraint Example

Suppose:

$$
\min_x x^2
$$

subject to:

$$
x\geq2
$$

Without the constraint:

$$
x=0
$$

would be the minimum.

But because:

$$
x\geq2
$$

the constrained minimum is:

$$
\boxed{x=2}
$$

---

# 12. Optimization in Radiation Therapy

Optimization is very important in a **Treatment Planning System (TPS)**.

Conceptually:

```text
Deliver Required Tumor Dose
            +
Protect Healthy Organs
            +
Respect Machine Constraints
            ↓
       Optimization
            ↓
      Treatment Plan
```

A simplified formulation can be:

$$
\boxed{
\min \text{Dose Penalty}
}
$$

subject to clinical and physical constraints.

---

# 13. Search Space

The **search space** contains all possible candidate solutions.

Example:

If:

$$
x\in[-10,10]
$$

then every possible value of \(x\) in this range belongs to the search space.

Optimization searches this space for the best solution.

---

# 14. Feasible Region

The feasible region contains all solutions satisfying the constraints.

Example:

$$
x\geq2
$$

Then:

```text
<───────|════════════════════>
        2
        ↑
  Feasible Region
```

Only feasible solutions can be accepted.

---

# 15. Local Minimum

A local minimum is lower than nearby points.

```text
        /\        /\
       /  \______/  \
             ↑
        Local Minimum
```

It is the best solution in a nearby region.

But it may not be the best solution overall.

---

# 16. Global Minimum

A global minimum is the lowest point across the entire search space.

```text
        /\              /\
       /  \____    ____/  \
            \____/
              ↑
        Global Minimum
```

For minimization:

$$
\boxed{
f(x^*)\leq f(x)
}
$$

for every feasible \(x\).

---

# 17. Local vs Global Minimum

| Local Minimum                        | Global Minimum                  |
| ------------------------------------ | ------------------------------- |
| Best nearby solution                 | Best overall solution           |
| May have a better solution elsewhere | No feasible solution is better  |
| Common in non-convex problems        | Desired optimum when attainable |

---

# 18. Critical Points

For a differentiable one-dimensional function:

$$
f(x)
$$

a critical point may occur when:

$$
\boxed{
f'(x)=0
}
$$

For a multivariable function:

$$
f(x_1,x_2,\dots,x_n)
$$

we use:

$$
\boxed{
\nabla f=0
}
$$

---

# 19. First Derivative Example

Consider:

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

This is a critical point.

---

# 20. Second Derivative Test

For a critical point:

$$
f'(x)=0
$$

we examine:

$$
f''(x)
$$

### Local Minimum

$$
\boxed{
f''(x)>0
}
$$

### Local Maximum

$$
\boxed{
f''(x)<0
}
$$

### Inconclusive

$$
\boxed{
f''(x)=0
}
$$

Further analysis is required.

---

# 21. Gradient

For a multivariable function:

$$
f(x,y)
$$

the gradient is:

$$
\boxed{
\nabla f=
\begin{bmatrix}
\frac{\partial f}{\partial x}\\
\frac{\partial f}{\partial y}
\end{bmatrix}
}
$$

The gradient points in the direction of maximum local increase.

Therefore:

$$
-\nabla f
$$

points in a direction of local decrease.

---

# 22. Gradient-Based Optimization

Gradient-based optimization uses derivatives to improve the solution.

General idea:

```text
Current Solution
      ↓
Calculate Gradient
      ↓
Find Direction
      ↓
Update Parameters
      ↓
Repeat
```

The most fundamental algorithm is:

$$
\boxed{\text{Gradient Descent}}
$$

---

# 23. Gradient Descent

The basic formula is:

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

* \(\alpha\) = learning rate
* \(\nabla f\) = gradient

---

# 24. Gradient Descent Example

Consider:

$$
f(x)=x^2
$$

Derivative:

$$
f'(x)=2x
$$

Start:

$$
x=4
$$

Learning rate:

$$
\alpha=0.1
$$

Gradient:

$$
f'(4)=8
$$

Update:

$$
x_{new}
=
4-0.1(8)
$$

$$
\boxed{x_{new}=3.2}
$$

Next iteration:

$$
f'(3.2)=6.4
$$

Therefore:

$$
x_{new}
=
3.2-0.1(6.4)
$$

$$
\boxed{x_{new}=2.56}
$$

The process continues toward:

$$
\boxed{x=0}
$$

---

# 25. Gradient Descent Workflow

```text
Initialize Parameters
        ↓
Calculate Objective Function
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

# 26. Learning Rate

The learning rate is:

$$
\boxed{\alpha}
$$

It determines how large each optimization step is.

### Small Learning Rate

```text
Small Steps
    ↓
More Iterations
    ↓
Slow Convergence
```

### Large Learning Rate

```text
Large Steps
    ↓
May Overshoot
    ↓
May Oscillate or Diverge
```

---

# 27. Learning Rate Problems

### Learning Rate Too Small

$$
\alpha \rightarrow \text{very small}
$$

Result:

* Slow optimization
* Many iterations

### Learning Rate Too Large

Result:

* Overshooting
* Oscillation
* Divergence

A suitable step size is important.

---

# 28. Convergence

Optimization converges when the algorithm satisfies a stopping condition.

Examples:

### Small Gradient

$$
\boxed{
\|\nabla f\|<\epsilon
}
$$

### Small Objective Change

$$
\boxed{
|f_{new}-f_{old}|<\epsilon
}
$$

### Small Parameter Change

$$
\boxed{
\|x_{new}-x_{old}\|<\epsilon
}
$$

---

# 29. Common Stopping Conditions

```text
Stop When:

✓ Maximum iterations reached

✓ Gradient is sufficiently small

✓ Objective improvement is sufficiently small

✓ Parameter changes are sufficiently small
```

---

# 30. Convex Optimization

A convex function has a bowl-like shape.

```text
       \        /
        \      /
         \    /
          \  /
           \/
```

Example:

$$
f(x)=x^2
$$

For a convex optimization problem, under appropriate conditions:

$$
\boxed{
\text{Every local minimum is a global minimum}
}
$$

---

# 31. Non-Convex Optimization

A non-convex function can have multiple valleys.

```text
       /\       /\
      /  \_____/  \___
```

Possible challenges:

* Multiple local minima
* Saddle points
* Complex search spaces

Many practical image-processing and machine-learning problems are non-convex.

---

# 32. Saddle Point

Consider:

$$
f(x,y)=x^2-y^2
$$

At:

$$
(0,0)
$$

we have:

$$
\nabla f=0
$$

However, the point is neither a normal local minimum nor maximum.

It is a:

$$
\boxed{\text{Saddle Point}}
$$

---

# 33. Hessian Matrix

The Hessian contains second-order derivatives.

For:

$$
f(x,y)
$$

$$
\boxed{
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
}
$$

The Hessian helps analyze:

* Curvature
* Minima
* Maxima
* Saddle points

---

# 34. Hessian Classification

For a two-variable function, define:

$$
D=f_{xx}f_{yy}-(f_{xy})^2
$$

At a critical point:

### Local Minimum

$$
D>0
$$

and:

$$
f_{xx}>0
$$

### Local Maximum

$$
D>0
$$

and:

$$
f_{xx}<0
$$

### Saddle Point

$$
D<0
$$

---

# 35. Newton's Method

Newton's method uses first- and second-order derivative information.

For one variable:

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

---

# 36. Gradient Descent vs Newton's Method

| Feature                  | Gradient Descent | Newton's Method        |
| ------------------------ | ---------------- | ---------------------- |
| Derivatives              | First-order      | First and second-order |
| Hessian required         | No               | Yes                    |
| Per-iteration complexity | Usually lower    | Usually higher         |
| Memory requirement       | Usually lower    | Can be higher          |
| Step control             | Learning rate    | Curvature-based step   |

---

# 37. Other Fundamental Optimization Algorithms

Important algorithms include:

```text
Optimization Algorithms
│
├── Gradient Descent
├── Stochastic Gradient Descent
├── Newton's Method
├── Quasi-Newton Methods
├── Conjugate Gradient
├── Coordinate Descent
└── Population-Based Methods
```

These will become more important in advanced topics.

---

# 38. Regularization

Regularization adds additional preferences or penalties to an optimization problem.

General form:

$$
\boxed{
\text{Objective}
=
\text{Data Term}
+
\lambda
\text{Regularization Term}
}
$$

Example:

$$
\boxed{
\min_x
\|Ax-y\|^2
+
\lambda R(x)
}
$$

Where:

* \(A\) = system model
* \(x\) = unknown solution
* \(y\) = measured data
* \(R(x)\) = regularization
* \(\lambda\) = regularization strength

---

# 39. Why Regularization?

Without regularization:

```text
Data
 ↓
Fit Measurements
 ↓
Possible Noise Amplification
```

With regularization:

```text
Data Fitting
      +
Prior / Constraint
      ↓
More Controlled Solution
```

Regularization can help with:

* Noise
* Ill-posed problems
* Overfitting
* Unstable solutions

---

# 40. Optimization in Image Registration

Image registration aligns two or more images.

```text
Fixed Image
      +
Moving Image
      ↓
Transformation
      ↓
Similarity Measurement
      ↓
Optimization
      ↓
Best Alignment
```

We define:

$$
T(\theta)
$$

and optimize:

$$
\boxed{
\min_\theta E(\theta)
}
$$

Parameters may include:

* Translation
* Rotation
* Scaling
* Deformation

---

# 41. Optimization in Image Reconstruction

A general reconstruction problem is:

$$
\boxed{
\min_x
\|Ax-y\|^2
+
\lambda R(x)
}
$$

Conceptually:

```text
Measured Data
      +
Physical System Model
      +
Regularization
      ↓
Optimization
      ↓
Reconstructed Image
```

---

# 42. Optimization in Image Segmentation

Segmentation can be formulated as an optimization problem.

Example:

$$
\boxed{
E
=
E_{data}
+
\lambda E_{smoothness}
}
$$

Goal:

$$
\boxed{
\min E
}
$$

The optimizer balances:

* Agreement with image data
* Smooth boundaries
* Other constraints or priors

---

# 43. Optimization in Machine Learning

Machine learning models have parameters:

$$
\theta
$$

A loss function is defined:

$$
L(\theta)
$$

Goal:

$$
\boxed{
\min_\theta L(\theta)
}
$$

Typical process:

```text
Training Data
      ↓
Model Prediction
      ↓
Calculate Loss
      ↓
Calculate Gradient
      ↓
Update Parameters
      ↓
Repeat
```

---

# 44. Optimization in Medical Image Processing

Optimization is used in:

```text
Medical Image Processing
│
├── Image Registration
├── Image Reconstruction
├── Image Segmentation
├── Image Enhancement
├── Machine Learning
├── Deep Learning
└── Treatment Planning
```

---

# 45. Optimization in Treatment Planning Systems

For radiation therapy:

```text
Tumor
 ↓
Required Dose

Healthy Organs
 ↓
Dose Limits

Machine
 ↓
Physical Constraints

        ↓

   Optimization

        ↓

Optimal Treatment Plan
```

A treatment plan may balance competing objectives such as:

$$
\boxed{
\text{Tumor Coverage}
+
\text{Organ Protection}
+
\text{Deliverability}
}
$$

The exact objective formulation depends on the TPS and clinical planning requirements.

---

# 46. General Optimization Workflow

```text
1. Define Variables
        ↓
2. Define Objective Function
        ↓
3. Define Constraints
        ↓
4. Select Algorithm
        ↓
5. Initialize Parameters
        ↓
6. Optimize
        ↓
7. Check Convergence
        ↓
8. Validate Result
```

---

# 47. C++ Example — Simple Gradient Descent

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
        x = x - learningRate * gradient(x);

        std::cout
            << "Iteration: "
            << iteration + 1
            << "  x = "
            << x
            << "  f(x) = "
            << objective(x)
            << '\n';
    }

    return 0;
}
```

The solution gradually approaches:

$$
\boxed{x=0}
$$

---

# 48. C++ Example — Two-Variable Optimization

Consider:

$$
f(x,y)=x^2+y^2
$$

Gradient:

$$
\nabla f=
\begin{bmatrix}
2x\\
2y
\end{bmatrix}
$$

```cpp
#include <iostream>

int main()
{
    double x = 5.0;
    double y = 3.0;

    double learningRate = 0.1;

    for (int i = 0; i < 20; ++i)
    {
        double gradientX = 2.0 * x;
        double gradientY = 2.0 * y;

        x = x - learningRate * gradientX;
        y = y - learningRate * gradientY;

        std::cout
            << "x = " << x
            << ", y = " << y
            << '\n';
    }

    return 0;
}
```

The algorithm approaches:

$$
\boxed{
(x,y)=(0,0)
}
$$

---

# 49. Common Optimization Problems

| Problem              | Objective                                   |
| -------------------- | ------------------------------------------- |
| Image registration   | Best image alignment                        |
| Image reconstruction | Minimum reconstruction error                |
| Image segmentation   | Best region separation                      |
| Machine learning     | Minimum loss                                |
| Deep learning        | Minimum training loss                       |
| TPS                  | Balance clinical objectives and constraints |

---

# 50. Important Formulas

### General Optimization

$$
\boxed{
\min_x f(x)
}
$$

### Constrained Optimization

$$
\boxed{
\min_x f(x)
}
$$

subject to:

$$
g(x)\leq0
$$

$$
h(x)=0
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
x-
\frac{f'(x)}
{f''(x)}
}
$$

### Regularized Optimization

$$
\boxed{
\min_x
\|Ax-y\|^2
+
\lambda R(x)
}
$$

---

# 51. Practice Questions

### Question 1

What is optimization?

**Answer:** Finding the best solution according to an objective function.

---

### Question 2

What is the difference between minimization and maximization?

**Answer:**

* Minimization finds the smallest objective value.
* Maximization finds the largest objective value.

---

### Question 3

What is an objective function?

**Answer:** The mathematical function that optimization tries to minimize or maximize.

---

### Question 4

Why does gradient descent move in the negative gradient direction?

**Answer:** Because the negative gradient gives a local direction of decreasing objective value.

---

### Question 5

What is a local minimum?

**Answer:** A solution that has a lower objective value than nearby solutions.

---

### Question 6

What is a global minimum?

**Answer:** The lowest objective value across the entire feasible search space.

---

### Question 7

What happens when the learning rate is too large?

**Answer:** The optimizer may overshoot, oscillate, or diverge.

---

### Question 8

What is regularization?

**Answer:** Adding a penalty or preference term to control the optimization solution.

---

### Question 9

Where is optimization used in medical image processing?

**Answer:** Image registration, reconstruction, segmentation, machine learning, and treatment planning.

---

# 52. Common Mistakes

### Mistake 1: Assuming Every Critical Point Is a Minimum

A critical point may be:

* Minimum
* Maximum
* Saddle point

---

### Mistake 2: Using an Incorrect Learning Rate

Too small:

```text
Very Slow
```

Too large:

```text
Unstable
```

---

### Mistake 3: Ignoring Constraints

A mathematically optimal solution may be invalid if it violates practical or physical constraints.

---

### Mistake 4: Confusing Local and Global Optimum

A local optimum is not necessarily the best solution overall.

---

### Mistake 5: Assuming Convergence Means Correctness

An algorithm can converge to:

* A local minimum
* A suboptimal solution
* An unsuitable solution due to incorrect modeling

Therefore, results must be validated.

---

# 53. Chapter 24 Summary

In **Optimization Fundamentals**, you learned:

* Meaning of optimization
* Minimization
* Maximization
* Objective functions
* Cost, loss, and energy functions
* Optimization variables
* Search space
* Feasible region
* Unconstrained optimization
* Constrained optimization
* Local minimum
* Global minimum
* Critical points
* First derivative
* Second derivative test
* Gradient
* Gradient-based optimization
* Gradient descent
* Learning rate
* Convergence
* Stopping conditions
* Convex optimization
* Non-convex optimization
* Saddle points
* Hessian matrix
* Newton's method
* Other optimization algorithms
* Regularization
* Image registration optimization
* Image reconstruction optimization
* Image segmentation optimization
* Machine-learning optimization
* Medical image processing optimization
* Treatment planning optimization

---

# 🎉 Module 2 — Mathematics Foundation Completed

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

**Chapter 24 is now covered as one complete, comprehensive chapter containing all its fundamental optimization topics.**
