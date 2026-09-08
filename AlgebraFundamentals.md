# Module 2 → Chapter 11: Algebra Fundamentals

Algebra is the foundation for many formulas used in **image processing, medical imaging, computer vision, AI, and machine learning**.

---

# 1. What Is Algebra?

Algebra uses:

* Numbers
* Variables
* Constants
* Operators
* Equations
* Expressions

Example:

$$
x + 5 = 10
$$

Here:

```text
x = Variable
5 = Constant
10 = Constant
```

We solve:

$$
x = 5
$$

---

# 2. Variables

A variable represents a value that can change.

Example:

$$
x = 10
$$

In image processing:

```text
I(x, y)
```

means:

```text
Image intensity
at position
(x, y)
```

Example:

```text
I(100, 200)
```

could represent the pixel intensity at:

```text
x = 100
y = 200
```

---

# 3. Constants

A constant has a fixed value.

Example:

$$
a = 10
$$

In image processing:

```text
Maximum 8-bit intensity = 255
```

So:

$$
I_{max} = 255
$$

For normalized images:

$$
I_{max} = 1
$$

---

# 4. Algebraic Expressions

Example:

$$
2x + 5
$$

This is an expression because there is no equality sign.

If:

$$
x = 10
$$

then:

$$
2(10)+5=25
$$

---

# 5. Arithmetic Operators

| Operator | Meaning        |
| -------- | -------------- |
| `+`      | Addition       |
| `-`      | Subtraction    |
| `×`      | Multiplication |
| `/`      | Division       |
| `^`      | Power          |

Examples:

$$
10+5=15
$$

$$
10-5=5
$$

$$
10\times5=50
$$

$$
10/5=2
$$

$$
2^3=8
$$

---

# 6. Equations

An equation contains:

$$
=
$$

Example:

$$
2x+5=15
$$

Solve:

$$
2x=10
$$

Therefore:

$$
x=5
$$

---

# 7. Solving Linear Equations

Example:

$$
3x+4=19
$$

### Step 1: Move constant

$$
3x=19-4
$$

$$
3x=15
$$

### Step 2: Divide

$$
x=5
$$

---

# 8. Image Processing Example: Brightness

Suppose:

$$
I_{new}=I_{old}+B
$$

Where:

```text
Iold = Original pixel intensity
B    = Brightness value
Inew = New pixel intensity
```

Example:

$$
I_{old}=100
$$

$$
B=50
$$

Therefore:

$$
I_{new}=100+50=150
$$

This is a basic algebraic image transformation.

---

# 9. Contrast Formula

A simplified contrast transformation:

$$
I_{new}=aI_{old}
$$

Where:

```text
a = Contrast factor
```

Example:

$$
I_{old}=100
$$

$$
a=1.5
$$

Then:

$$
I_{new}=1.5\times100=150
$$

---

# 10. Combined Brightness and Contrast

A common mathematical form is:

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
I_{new}=1.2(100)+20
$$

$$
I_{new}=120+20
$$

$$
I_{new}=140
$$

This equation will appear repeatedly in image processing.

---

# 11. Rearranging Equations

Suppose:

$$
y=ax+b
$$

Find \(x\).

### Step 1

$$
y-b=ax
$$

### Step 2

$$
x=\frac{y-b}{a}
$$

This skill is important when deriving imaging formulas.

---

# 12. Fractions

Example:

$$
\frac{x}{2}=10
$$

Multiply both sides by 2:

$$
x=20
$$

Image normalization example:

$$
I_{normalized}
=
\frac{I-I_{min}}
{I_{max}-I_{min}}
$$

This converts intensity into a normalized range.

---

# 13. Example: Image Normalization

Suppose:

$$
I=150
$$

$$
I_{min}=50
$$

$$
I_{max}=250
$$

Then:

$$
I_{normalized}
=
\frac{150-50}
{250-50}
$$

$$
=
\frac{100}{200}
$$

$$
=0.5
$$

Therefore:

```text
Original intensity = 150
Normalized intensity = 0.5
```

---

# 14. Powers

Example:

$$
x^2
$$

means:

$$
x\times x
$$

If:

$$
x=5
$$

Then:

$$
x^2=25
$$

Powers are important in:

* Distance calculation
* Variance
* Standard deviation
* Gaussian functions
* Image filtering

---

# 15. Square Root

Example:

$$
\sqrt{25}=5
$$

Important image-processing example:

$$
distance=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

This calculates Euclidean distance.

Used in:

* Image registration
* Segmentation
* Object detection
* Medical image geometry

---

# 16. Order of Operations

Remember:

```text
B → Brackets
P → Powers
M → Multiplication
D → Division
A → Addition
S → Subtraction
```

Example:

$$
2+3\times4
$$

Correct:

$$
2+12=14
$$

Not:

$$
(2+3)\times4=20
$$

unless brackets are explicitly present.

---

# 17. Negative Numbers

Example:

$$
10-15=-5
$$

Negative values are very important in medical imaging.

For example, some image representations use signed pixel values:

```text
-1000
-500
0
500
1000
```

Negative values can represent meaningful physical intensity ranges depending on the modality and representation.

---

# 18. Absolute Value

Absolute value:

$$
|x|
$$

Example:

$$
|-10|=10
$$

Image difference:

$$
Difference=
|I_1-I_2|
$$

Example:

$$
|100-80|=20
$$

Used in:

* Image comparison
* Error measurement
* Registration
* Change detection

---

# 19. Ratios

A ratio compares values.

Example:

$$
\frac{a}{b}
$$

Suppose:

$$
a=100
$$

$$
b=200
$$

Then:

$$
\frac{100}{200}=0.5
$$

Ratios are used in:

* Image scaling
* Normalization
* Signal comparison
* Feature extraction

---

# 20. Proportions

Example:

$$
\frac{a}{b}
=
\frac{c}{d}
$$

Suppose:

$$
\frac{x}{10}
=
\frac{20}{40}
$$

Then:

$$
\frac{x}{10}
=
0.5
$$

Therefore:

$$
x=5
$$

---

# 21. Linear Relationships

A linear equation:

$$
y=mx+c
$$

Where:

```text
m = Slope
c = Intercept
```

Example:

$$
y=2x+3
$$

If:

$$
x=5
$$

Then:

$$
y=13
$$

Linear transformations are widely used in image intensity processing.

---

# 22. Polynomial Expressions

Example:

$$
ax^2+bx+c
$$

Example:

$$
x^2+5x+6
$$

Polynomial equations can appear in:

* Curve fitting
* Calibration
* Image models
* Approximation algorithms

---

# 23. Quadratic Equations

General form:

$$
ax^2+bx+c=0
$$

Example:

$$
x^2-5x+6=0
$$

Factor:

$$
(x-2)(x-3)=0
$$

Therefore:

$$
x=2
$$

or:

$$
x=3
$$

---

# 24. Scientific Notation

Large medical-image calculations may involve large or very small values.

Example:

$$
1,000,000=10^6
$$

Small value:

$$
0.000001=10^{-6}
$$

Examples:

```text
10⁶ → One million
10⁻³ → 0.001
10⁻⁶ → 0.000001
```

---

# 25. Logarithms

A logarithm answers:

> To what power should the base be raised?

Example:

$$
\log_{10}(100)=2
$$

Because:

$$
10^2=100
$$

Natural logarithm:

$$
\ln(x)
$$

Logarithmic operations appear in:

* Signal processing
* Image enhancement
* Information theory
* Medical imaging physics

---

# 26. Important Image Algebra Formulas

## Brightness

$$
I'=I+b
$$

## Contrast

$$
I'=aI
$$

## Brightness + Contrast

$$
I'=aI+b
$$

## Normalization

$$
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
$$

## Absolute Difference

$$
D=|I_1-I_2|
$$

## Euclidean Distance

$$
d=
\sqrt{
(x_2-x_1)^2+
(y_2-y_1)^2
}
$$

These formulas are fundamental for later chapters.

---

# 27. C++ Example: Brightness

```cpp
int pixel = 100;
int brightness = 50;

int newPixel =
    pixel + brightness;
```

Result:

```text
150
```

---

# 28. C++ Example: Contrast

```cpp
float pixel = 100.0f;
float contrast = 1.5f;

float newPixel =
    pixel * contrast;
```

Result:

```text
150
```

---

# 29. C++ Example: Brightness + Contrast

```cpp
float pixel = 100.0f;

float contrast = 1.2f;
float brightness = 20.0f;

float newPixel =
    contrast * pixel +
    brightness;
```

Result:

```text
140
```

---

# 30. Important Algebra Skills for Image Processing

You should be comfortable with:

```text
Variables
Constants
Expressions
Equations
Fractions
Powers
Square roots
Negative numbers
Absolute values
Ratios
Proportions
Linear equations
Quadratic equations
Scientific notation
Logarithms
```

---

# 31. Practice Questions

### Question 1

Solve:

$$
3x+7=22
$$

**Answer:**

$$
x=5
$$

---

### Question 2

If:

$$
I'=2I+10
$$

and:

$$
I=50
$$

Find \(I'\).

$$
I'=2(50)+10
$$

$$
I'=110
$$

---

### Question 3

Normalize:

$$
I=150
$$

$$
I_{min}=50
$$

$$
I_{max}=250
$$

Answer:

$$
0.5
$$

---

### Question 4

Find:

$$
|50-120|
$$

Answer:

$$
70
$$

---

# 32. Medical Image Connection

A medical image can be represented mathematically as:

$$
I(x,y)
$$

For a 3D volume:

$$
I(x,y,z)
$$

Every later operation is based on mathematics:

```text
Medical Image
      │
      ▼
Mathematical Representation
      │
      ├── Algebra
      ├── Vectors
      ├── Matrices
      ├── Calculus
      ├── Probability
      └── Statistics
```

---

# 33. Chapter 11 Summary

You learned:

* Variables
* Constants
* Expressions
* Equations
* Linear equations
* Rearranging equations
* Fractions
* Powers
* Square roots
* Negative numbers
* Absolute values
* Ratios and proportions
* Linear relationships
* Polynomials
* Quadratic equations
* Scientific notation
* Logarithms
* Image normalization
* Brightness and contrast equations

---

## Current Progress



**Next: Chapter 12 — Functions**
