# Module 2 → Chapter 19: Calculus Fundamentals

Calculus is essential for advanced:

* Image processing
* Computer vision
* Medical image analysis
* Image registration
* Machine learning
* Deep learning
* Optimization
* Image segmentation

The two major parts of calculus are:

```text
Calculus
│
├── Differential Calculus
│   └── Rate of change / Derivatives
│
└── Integral Calculus
    └── Accumulation / Area
```

---

# 1. What Is Calculus?

Calculus studies:

* Change
* Rates of change
* Motion
* Accumulation
* Curves
* Optimization

For image processing, calculus helps us understand how **image intensity changes across space**.

For example:

```text
Smooth Image Area
      ↓
Small intensity change
```

But at an edge:

```text
Dark Region → Bright Region
        ↑
 Large intensity change
```

Derivatives help detect these changes.

---

# 2. Functions and Change

Suppose:

$$
y=f(x)
$$

Example:

$$
f(x)=x^2
$$

If \(x\) changes, then \(y\) changes.

```text
x → Input
↓
Function f
↓
y → Output
```

Calculus studies **how fast the output changes when the input changes**.

---

# 3. Limits

The foundation of calculus is the concept of a **limit**.

We write:

$$
\lim_{x\to a}f(x)
$$

This means:

> What value does \(f(x)\) approach as \(x\) approaches \(a\)?

---

# 4. Simple Limit Example

Suppose:

$$
f(x)=x+2
$$

Find:

$$
\lim_{x\to3}(x+2)
$$

Substitute:

$$
3+2=5
$$

Therefore:

$$
\boxed{5}
$$

---

# 5. Limit of a Quadratic Function

Suppose:

$$
f(x)=x^2
$$

Find:

$$
\lim_{x\to2}x^2
$$

Therefore:

$$
2^2=4
$$

So:

$$
\boxed{4}
$$

---

# 6. Why Limits Matter

Limits allow us to study extremely small changes.

Imagine:

```text
Large Change
    ↓
Smaller Change
    ↓
Very Small Change
    ↓
Approaches Zero
```

This leads to the concept of a derivative.

---

# 7. What Is a Derivative?

A derivative measures the **rate of change** of a function.

If:

$$
y=f(x)
$$

The derivative is written as:

$$
\frac{dy}{dx}
$$

or:

$$
f'(x)
$$

It tells us:

> How much does \(y\) change when \(x\) changes?

---

# 8. Derivative Using Limits

The derivative of:

$$
f(x)
$$

is defined as:

$$
f'(x)
=
\lim_{h\to0}
\frac{f(x+h)-f(x)}{h}
$$

This is one of the most important formulas in calculus.

---

# 9. Example: Derivative of \(x^2\)

Let:

$$
f(x)=x^2
$$

Using the definition:

$$
f'(x)
=
\lim_{h\to0}
\frac{(x+h)^2-x^2}{h}
$$

Expand:

$$
(x+h)^2=x^2+2xh+h^2
$$

Therefore:

$$
f'(x)
=
\lim_{h\to0}
\frac{2xh+h^2}{h}
$$

Factor:

$$
=
\lim_{h\to0}(2x+h)
$$

Therefore:

$$
\boxed{f'(x)=2x}
$$

---

# 10. Geometric Meaning of Derivative

The derivative represents the **slope of a curve**.

```text
          Curve
           /
         /
       /
-----●---------
     Tangent
```

At a specific point:

$$
f'(x)
$$

represents the slope of the tangent line.

---

# 11. Basic Derivative Rules

## Constant Rule

$$
\frac{d}{dx}(c)=0
$$

Example:

$$
\frac{d}{dx}(5)=0
$$

---

## Power Rule

$$
\frac{d}{dx}(x^n)=nx^{n-1}
$$

Example:

$$
\frac{d}{dx}(x^3)=3x^2
$$

---

## Constant Multiple Rule

$$
\frac{d}{dx}(cf(x))
=
cf'(x)
$$

Example:

$$
\frac{d}{dx}(5x^2)
=
10x
$$

---

# 12. Sum Rule

$$
\frac{d}{dx}
[f(x)+g(x)]
=
f'(x)+g'(x)
$$

Example:

$$
f(x)=x^2+3x
$$

Then:

$$
f'(x)=2x+3
$$

---

# 13. Product Rule

Suppose:

$$
y=u(x)v(x)
$$

Then:

$$
\frac{dy}{dx}
=
u'v+uv'
$$

Example:

$$
y=x^2(x+1)
$$

Let:

$$
u=x^2
$$

$$
v=x+1
$$

Then:

$$
u'=2x
$$

$$
v'=1
$$

Therefore:

$$
y'
=
2x(x+1)+x^2(1)
$$

$$
=3x^2+2x
$$

---

# 14. Quotient Rule

If:

$$
y=
\frac{u}{v}
$$

Then:

$$
y'
=
\frac{u'v-uv'}{v^2}
$$

Example:

$$
y=
\frac{x}{x+1}
$$

Then:

$$
u=x
$$

$$
v=x+1
$$

Therefore:

$$
y'
=
\frac{1(x+1)-x(1)}
{(x+1)^2}
$$

$$
=
\frac{1}{(x+1)^2}
$$

---

# 15. Chain Rule

The chain rule is used for a function inside another function.

Suppose:

$$
y=f(g(x))
$$

Then:

$$
\frac{dy}{dx}
=
f'(g(x))g'(x)
$$

Example:

$$
y=(x^2+1)^3
$$

Let:

$$
u=x^2+1
$$

Then:

$$
y=u^3
$$

Therefore:

$$
\frac{dy}{du}=3u^2
$$

and:

$$
\frac{du}{dx}=2x
$$

Thus:

$$
\frac{dy}{dx}
=
3(x^2+1)^2(2x)
$$

$$
\boxed{
6x(x^2+1)^2
}
$$

---

# 16. Important Trigonometric Derivatives

$$
\frac{d}{dx}\sin x=\cos x
$$

$$
\frac{d}{dx}\cos x=-\sin x
$$

$$
\frac{d}{dx}\tan x=\sec^2x
$$

---

# 17. Exponential and Logarithmic Derivatives

$$
\frac{d}{dx}e^x=e^x
$$

$$
\frac{d}{dx}\ln x=\frac{1}{x}
$$

---

# 18. Second Derivative

The derivative of a derivative is called the **second derivative**.

$$
f''(x)
=
\frac{d^2y}{dx^2}
$$

Example:

$$
f(x)=x^3
$$

First derivative:

$$
f'(x)=3x^2
$$

Second derivative:

$$
f''(x)=6x
$$

---

# 19. Why Second Derivatives Matter?

The first derivative measures:

```text
Rate of Change
```

The second derivative measures:

```text
Change of Rate of Change
```

In image processing, second derivatives are useful for:

* Edge detection
* Blob detection
* Laplacian filtering

---

# 20. Critical Points

Critical points occur where:

$$
f'(x)=0
$$

or where the derivative does not exist.

These points are important for:

* Maximum values
* Minimum values
* Optimization

Example:

$$
f(x)=x^2
$$

Derivative:

$$
f'(x)=2x
$$

Set equal to zero:

$$
2x=0
$$

Therefore:

$$
x=0
$$

At this point:

$$
f(0)=0
$$

which is a minimum.

---

# 21. Maximum and Minimum

Suppose:

$$
f'(x)=0
$$

We can use the second derivative.

### If:

$$
f''(x)>0
$$

Then:

```text
Minimum
```

### If:

$$
f''(x)<0
$$

Then:

```text
Maximum
```

---

# 22. Example of Maximum

Suppose:

$$
f(x)=-x^2+4x
$$

First derivative:

$$
f'(x)=-2x+4
$$

Set:

$$
-2x+4=0
$$

Therefore:

$$
x=2
$$

Second derivative:

$$
f''(x)=-2
$$

Since:

$$
f''(x)<0
$$

there is a maximum at:

$$
x=2
$$

---

# 23. Introduction to Integration

Integration is another major part of calculus.

While derivatives study change:

```text
Derivative
↓
Rate of Change
```

Integration studies accumulation:

```text
Integration
↓
Accumulation
```

---

# 24. Indefinite Integral

The integral of:

$$
f(x)
$$

is written:

$$
\int f(x)dx
$$

Example:

$$
\int x^2dx
$$

Using the integration power rule:

$$
\int x^n dx
=
\frac{x^{n+1}}{n+1}+C
$$

Therefore:

$$
\int x^2dx
=
\frac{x^3}{3}+C
$$

---

# 25. Why \(+C\)?

The derivative of a constant is zero.

For example:

$$
\frac{d}{dx}
\left(
\frac{x^3}{3}+5
\right)
=x^2
$$

Also:

$$
\frac{d}{dx}
\left(
\frac{x^3}{3}+10
\right)
=x^2
$$

Therefore we include:

$$
+C
$$

---

# 26. Definite Integral

A definite integral has limits:

$$
\int_a^b f(x)dx
$$

It represents accumulated quantity over an interval.

Example:

$$
\int_0^2x\,dx
$$

Antiderivative:

$$
\frac{x^2}{2}
$$

Apply limits:

$$
\left[
\frac{x^2}{2}
\right]_0^2
$$

$$
=
\frac{4}{2}-0
$$

$$
=2
$$

---

# 27. Fundamental Theorem of Calculus

Differentiation and integration are closely connected.

If:

$$
F'(x)=f(x)
$$

Then:

$$
\int_a^b f(x)dx
=
F(b)-F(a)
$$

---

# 28. Derivatives in Image Processing

An image can be represented as:

$$
I(x,y)
$$

Where:

* \(x\) = horizontal position
* \(y\) = vertical position
* \(I\) = intensity

The rate of intensity change can help identify important structures.

For example:

```text
Dark Region      Bright Region
██████████│░░░░░░░░░░
          ↑
        Edge
```

At an edge, intensity changes rapidly.

---

# 29. First Derivative in Images

For a one-dimensional signal:

$$
I(x)
$$

The derivative is:

$$
\frac{dI}{dx}
$$

A large magnitude can indicate a strong intensity change.

Conceptually:

```text
Intensity
   │       ┌────
   │       │
───┘───────
        ↑
       Edge

Derivative
   │      ↑
───┼──────│────
          Strong Change
```

---

# 30. Discrete Derivative

Digital images are discrete.

Therefore, instead of:

$$
\frac{dI}{dx}
$$

we approximate:

$$
\frac{\partial I}{\partial x}
\approx
I(x+1)-I(x)
$$

Similarly:

$$
\frac{\partial I}{\partial y}
\approx
I(y+1)-I(y)
$$

---

# 31. Image Gradient Preview

For a 2D image:

$$
I(x,y)
$$

the gradient is:

$$
\nabla I
=
\begin{bmatrix}
\frac{\partial I}{\partial x}\\
\frac{\partial I}{\partial y}
\end{bmatrix}
$$

We will study this deeply in **Chapter 21: Gradient**.

---

# 32. Second Derivative in Images

The second derivative detects changes in the first derivative.

A common second-order operator is the Laplacian:

$$
\nabla^2I
=
\frac{\partial^2I}{\partial x^2}
+
\frac{\partial^2I}{\partial y^2}
$$

This is used in:

* Edge detection
* Image enhancement
* Blob detection

---

# 33. Optimization Connection

Suppose we want to minimize an error function:

$$
E(x)
$$

We calculate:

$$
\frac{dE}{dx}
$$

Then optimization algorithms use derivatives to find better solutions.

Conceptually:

```text
Current Solution
       ↓
Calculate Derivative
       ↓
Determine Direction
       ↓
Update Solution
       ↓
Repeat
```

This is the foundation of many optimization and machine-learning algorithms.

---

# 34. Medical Imaging Applications

Calculus is used in:

### Image Registration

```text
Moving Image
      ↓
Transformation Parameters
      ↓
Calculate Error
      ↓
Derivative
      ↓
Update Parameters
```

### Image Segmentation

Derivatives can help identify:

* Boundaries
* Intensity transitions
* Image structures

### Image Filtering

Derivatives are used in:

* Sobel operators
* Laplacian operators
* Gradient-based filters

### Machine Learning

Training often involves:

$$
Loss Function
$$

and its derivatives:

$$
\frac{\partial Loss}{\partial Parameters}
$$

---

# 35. Important Formulas

### Derivative Definition

$$
f'(x)
=
\lim_{h\to0}
\frac{f(x+h)-f(x)}{h}
$$

### Power Rule

$$
\frac{d}{dx}x^n
=
nx^{n-1}
$$

### Product Rule

$$
(uv)'
=
u'v+uv'
$$

### Quotient Rule

$$
\left(
\frac{u}{v}
\right)'
=
\frac{u'v-uv'}
{v^2}
$$

### Chain Rule

$$
\frac{dy}{dx}
=
\frac{dy}{du}
\frac{du}{dx}
$$

### Integration Power Rule

$$
\int x^n dx
=
\frac{x^{n+1}}{n+1}+C
$$

for:

$$
n\neq-1
$$

---

# 36. Practice Questions

### Question 1

Find:

$$
\frac{d}{dx}(x^4)
$$

Answer:

$$
4x^3
$$

---

### Question 2

Find:

$$
\frac{d}{dx}(3x^2+5x)
$$

Answer:

$$
6x+5
$$

---

### Question 3

Find:

$$
\frac{d}{dx}(x^2+1)^3
$$

Answer:

$$
6x(x^2+1)^2
$$

---

### Question 4

Find:

$$
\int 2x\,dx
$$

Answer:

$$
x^2+C
$$

---

### Question 5

What does a large image derivative usually indicate?

**Answer:** A rapid intensity change, often associated with an edge or boundary.

---

# 37. Chapter Summary

You learned:

* What calculus is
* Limits
* Derivatives
* Derivative definition
* Slope and geometric meaning
* Power rule
* Sum rule
* Product rule
* Quotient rule
* Chain rule
* Trigonometric derivatives
* Exponential derivatives
* Logarithmic derivatives
* Second derivatives
* Critical points
* Maximum and minimum
* Integration
* Indefinite integrals
* Definite integrals
* Fundamental theorem of calculus
* Discrete derivatives
* Image derivatives
* Introduction to gradients
* Introduction to optimization

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
⬜ Chapter 20: Partial Derivatives
⬜ Chapter 21: Gradient
⬜ Chapter 22: Probability
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 20 — Partial Derivatives**
