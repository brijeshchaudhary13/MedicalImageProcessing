# Chapter 7 — Pixel-Level Operations

We continue **strictly according to your master index**. Chapter 7 contains exactly these topics:

1. Pixel addition
2. Pixel subtraction
3. Multiplication
4. Division
5. Clamping
6. Normalization
7. Scaling
8. Thresholding
9. Binary conversion
10. Image inversion



This chapter is the foundation for later **image arithmetic, windowing, enhancement, segmentation, and medical-image processing**.

---

# 7.1 What Are Pixel-Level Operations?

A pixel-level operation calculates the new value of a pixel using its existing value.

The simplest model is:

[
I'(x,y)=T(I(x,y))
]

where:

* (I(x,y)) = original pixel
* (T) = operation
* (I'(x,y)) = new pixel

Conceptually:

```text
Original Pixel
      ↓
Pixel Operation
      ↓
New Pixel
```

For example:

[
I'=I+20
]

Every pixel becomes 20 brighter.

---

# 7.2 Why Are Pixel Operations Important?

They are the basic building blocks of many image-processing operations.

```text
Pixel Operations
      │
      ├── Brightness
      ├── Contrast
      ├── Normalization
      ├── Thresholding
      ├── Binary images
      ├── Inversion
      └── Medical windowing
```

They are also very easy to implement efficiently.

---

# 7.3 Pixel Addition

The simplest operation is:

[
I'(x,y)=I(x,y)+C
]

where (C) is a constant.

Example:

```text
Original:
10 20 30
40 50 60
```

Add 10:

```text
20 30 40
50 60 70
```

So:

[
I'=I+10
]

---

# 7.4 Pixel Addition for Brightness

Adding a positive constant generally increases brightness.

```text
I
↓
+ C
↓
I'
```

For example:

```text
50 → 70
100 → 120
150 → 170
```

Therefore:

[
C>0
\Rightarrow
\text{brighter}
]

while:

[
C<0
\Rightarrow
\text{darker}
]

---

# 7.5 Overflow Problem

Suppose we have an 8-bit image:

[
0\le I\le255
]

Consider:

[
I=250
]

and add:

[
20
]

Mathematically:

[
250+20=270
]

But 8-bit unsigned data cannot represent 270.

Therefore we need **clamping** or another appropriate conversion strategy.

---

# 7.6 Pixel Subtraction

The basic operation is:

[
I'(x,y)=I(x,y)-C
]

Example:

```text
Original:
100 150 200
```

Subtract 30:

```text
70 120 170
```

This can be used for:

* brightness adjustment
* background correction
* difference calculations

---

# 7.7 Negative Values

Suppose:

[
I=10
]

and:

[
C=30
]

Then:

[
I'=10-30=-20
]

If the destination is `UINT8`, `-20` cannot be represented.

Again, we need to consider:

```text
signed type
        OR
clamping
        OR
floating-point processing
```

---

# 7.8 Multiplication

Pixel values can be multiplied by a constant:

[
I'=aI
]

Example:

```text
Original:
10 20 30
```

with:

[
a=2
]

gives:

```text
20 40 60
```

This is commonly used for intensity scaling.

---

# 7.9 Multiplication and Contrast

Consider:

[
I'=aI
]

If:

[
a>1
]

the intensity differences become larger.

If:

[
0<a<1
]

the intensity differences become smaller.

So conceptually:

```text
a > 1
 ↓
expand intensity differences
 ↓
higher contrast
```

and:

```text
0 < a < 1
 ↓
compress intensity differences
 ↓
lower contrast
```

---

# 7.10 Pixel Division

Division is:

[
I'=\frac{I}{C}
]

Example:

```text
Original:
100 200 240
```

Divide by 2:

```text
50 100 120
```

This reduces intensity values.

---

# 7.11 Division by Zero

This is an important programming issue.

Never blindly perform:

```cpp
result = pixel / divisor;
```

without checking:

```cpp
divisor != 0
```

Otherwise:

[
\frac{x}{0}
]

is undefined.

For floating-point code, you may get infinity or NaN depending on the operation, while integer division by zero is undefined behavior.

---

# 7.12 Pixel Addition, Subtraction, Multiplication, Division

The four basic operations are:

[
\boxed{I'=I+C}
]

[
\boxed{I'=I-C}
]

[
\boxed{I'=I\times C}
]

[
\boxed{I'=I/C}
]

Think:

```text
          Pixel
            │
     ┌──────┼──────┐
     ↓      ↓      ↓
     +      -      ×
                    \
                     ÷
```

---

# 7.13 Clamping

**Clamping** restricts a value to a specified range.

For an 8-bit image:

[
[0,255]
]

A value below 0 becomes:

[
0
]

A value above 255 becomes:

[
255
]

Mathematically:

[
I'=\min(\max(I,L),U)
]

where:

* (L) = lower bound
* (U) = upper bound

---

# 7.14 Clamping Examples

For range:

[
[0,255]
]

```text
-50  → 0
20   → 20
150  → 150
300  → 255
```

Conceptually:

```text
             Valid Range
          ┌───────────────┐
──────────┼───────────────┼──────────
         0               255
          ↑               ↑
        clamp           clamp
```

---

# 7.15 Why Clamping Is Important

Suppose:

[
I'=1.5I+50
]

For:

[
I=200
]

we get:

[
I'=350
]

If the output is 8-bit:

```text
350
 ↓
clamp
 ↓
255
```

Without correct handling, conversion to an unsigned 8-bit type can produce incorrect results due to narrowing/wrapping behavior.

---

# 7.16 C++ Clamping

Modern C++:

```cpp
#include <algorithm>

double value = 350.0;

value = std::clamp(value, 0.0, 255.0);
```

Now:

```text
value = 255
```

For Qt 5.14/C++11-era code, if `std::clamp` is unavailable, you can use:

```cpp
value = std::max(0.0, std::min(value, 255.0));
```

This is useful when working with older Qt/C++ environments.

---

# 7.17 Normalization

Normalization transforms values from one range into another standardized range.

Suppose the original values are:

[
[I_{min},I_{max}]
]

and we want:

[
[0,1]
]

Then:

[
\boxed{
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
}
]

---

# 7.18 Normalization Example

Suppose:

[
I_{min}=50
]

[
I_{max}=150
]

and:

[
I=100
]

Then:

[
I'=
\frac{100-50}{150-50}
]

[
=\frac{50}{100}
]

[
=0.5
]

So:

```text
100
 ↓
normalize
 ↓
0.5
```

---

# 7.19 Normalization to 0–255

We can normalize directly into another range.

General formula:

[
\boxed{
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
(O_{max}-O_{min})
+O_{min}
}
]

For:

[
O_{min}=0
]

[
O_{max}=255
]

we get:

[
I'=255
\frac{I-I_{min}}
{I_{max}-I_{min}}
]

---

# 7.20 Normalization Example

Suppose:

[
I_{min}=100
]

[
I_{max}=500
]

and:

[
I=300
]

Then:

[
I'=255
\frac{300-100}{500-100}
]

[
=255\frac{200}{400}
]

[
=127.5
]

Approximately:

[
128
]

So:

```text
100 → 0
300 → 128
500 → 255
```

---

# 7.21 Scaling

Scaling changes the magnitude of pixel values.

For example:

[
I'=aI
]

This is technically a simple intensity transformation, but the concept is important enough to distinguish:

```text
Scaling
 ↓
multiply by factor
```

Example:

[
a=2
]

```text
20 → 40
40 → 80
80 → 160
```

---

# 7.22 Normalization vs Scaling

These are often confused.

### Scaling

Usually:

[
I'=aI
]

It changes magnitude according to a factor.

### Normalization

Uses the observed or specified range:

[
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
]

It makes values fit a standardized range.

Think:

```text
Scaling
 ↓
"multiply by this factor"

Normalization
 ↓
"map this range into another range"
```

---

# 7.23 Thresholding

Thresholding converts pixels according to a threshold value.

The simplest binary threshold:

[
I'(x,y)=
\begin{cases}
1 & I(x,y)\ge T\
0 & I(x,y)<T
\end{cases}
]

where (T) is the threshold.

---

# 7.24 Threshold Example

Suppose:

[
T=100
]

Input:

```text
50  80  100  120  200
```

Output:

```text
0   0   1    1    1
```

because:

```text
< 100 → 0
≥ 100 → 1
```

---

# 7.25 Thresholding Concept

```text
Input intensity
      ↓
Compare with T
      │
 ┌────┴────┐
 ↓         ↓
< T       ≥ T
 ↓         ↓
 0         1
```

This is one of the foundations of image segmentation.

---

# 7.26 Thresholding in Medical Imaging

Thresholding can be used for tasks such as:

* extracting high-density structures
* preliminary bone segmentation
* isolating certain intensity ranges
* creating masks
* separating foreground/background

But a single threshold is often not enough for complex medical anatomy.

For example:

```text
CT
 ↓
Threshold
 ↓
Initial mask
 ↓
Further processing
```

More advanced segmentation methods come later.

---

# 7.27 Binary Conversion

A binary image has only two values.

Usually:

[
0
]

and:

[
1
]

or for display:

[
0
]

and:

[
255
]

For example:

```text
Original:

20  50  150
80  200 100
```

Threshold (100):

```text
Binary:

0   0   255
0   255 0
```

---

# 7.28 0/1 vs 0/255 Binary Images

Both are commonly used.

### Logical binary representation

```text
0 / 1
```

Useful for:

* masks
* mathematical operations
* segmentation labels

### Display binary representation

```text
0 / 255
```

Useful for:

* grayscale display
* PNG/JPEG-style visualization

Don't confuse the two.

---

# 7.29 Image Inversion

For an 8-bit grayscale image:

[
\boxed{
I'=255-I
}
]

This reverses the intensity.

Example:

```text
0   → 255
50  → 205
100 → 155
200 → 55
255 → 0
```

Conceptually:

```text
Black ↔ White
Dark  ↔ Bright
```

---

# 7.30 General Image Inversion

For a maximum value (M):

[
I'=M-I
]

For 8-bit:

[
M=255
]

For a 16-bit unsigned image:

[
M=65535
]

Therefore:

```text
8-bit:
I' = 255 - I

16-bit:
I' = 65535 - I
```

---

# 7.31 Medical Image Inversion

Inversion can be useful for visualization, but it must be used carefully.

A medical image viewer may allow:

```text
Normal display
      ↕
Inverted display
```

This changes visualization, not the underlying anatomy.

For quantitative medical data, remember to distinguish:

```text
Original voxel
      ≠
Inverted display value
```

---

# 7.32 Pixel-Level Operations Pipeline

The entire chapter can be represented as:

```text
                    PIXEL
                      │
        ┌─────────────┼─────────────┐
        ↓             ↓             ↓
     Addition     Subtraction   Multiplication
        │             │             │
        └─────────────┼─────────────┘
                      ↓
                   Division
                      ↓
                 Clamping
                      ↓
                 Scaling
                      ↓
                Normalization
                      ↓
                 Thresholding
                      ↓
               Binary Conversion
                      ↓
                 Inversion
```

---

# 7.33 Important Difference: Clamp vs Normalize

Suppose:

```text
Input:
-100, 0, 100, 200, 400
```

### Clamp to 0–255

```text
0, 0, 100, 200, 255
```

Values outside the range are simply clipped.

### Normalize based on min/max

If:

[
Min=-100
]

[
Max=400
]

then:

```text
-100 → 0
0    → 51
100  → 102
200  → 153
400  → 255
```

Normalization preserves relative position within the selected range.

So:

```text
Clamp
 ↓
cut off values outside range

Normalize
 ↓
map entire range to target range
```

---

# 7.34 Pixel Operation on Every Pixel

Suppose:

```text
I =
10 20
30 40
```

and:

[
I'=I+10
]

we perform:

```text
10 → 20
20 → 30
30 → 40
40 → 50
```

Result:

```text
20 30
40 50
```

The coordinates do not change.

This is an important property:

> **Pixel-level intensity operations modify values, not image geometry.**

---

# 7.35 C++ Generic Pixel Operation

You can represent an operation using a function:

```cpp
double transformPixel(double value)
{
    return value + 20.0;
}
```

Then:

```cpp
for (double& pixel : image)
{
    pixel = transformPixel(pixel);
}
```

This basic idea later becomes useful for building reusable image-processing pipelines.

---

# 7.36 C++ Thresholding

```cpp
std::uint8_t threshold(
    std::uint8_t pixel,
    std::uint8_t thresholdValue)
{
    return pixel >= thresholdValue ? 255 : 0;
}
```

For:

```text
threshold = 100
```

we get:

```text
50  → 0
100 → 255
150 → 255
```

---

# 7.37 C++ Inversion

```cpp
std::uint8_t invert(std::uint8_t pixel)
{
    return 255 - pixel;
}
```

Example:

```text
0   → 255
100 → 155
255 → 0
```

---

# 7.38 C++ Normalization

```cpp
double normalize(
    double value,
    double minValue,
    double maxValue)
{
    if (maxValue == minValue)
        return 0.0;

    return (value - minValue)
           / (maxValue - minValue);
}
```

The special case:

```cpp
maxValue == minValue
```

is important because otherwise we divide by zero.

---

# 7.39 Medical Imaging: CT Example

Suppose CT values are:

```text
-1000 → 3000 HU
```

but the display is:

```text
0 → 255
```

A simplified mapping can be:

```text
CT value
   ↓
Window/selected range
   ↓
Normalize
   ↓
Clamp
   ↓
0–255
   ↓
Display
```

This connects Chapter 7 directly to CT visualization.

---

# 7.40 Thresholding and Segmentation

Thresholding is one of the simplest segmentation techniques.

For example:

```text
CT
 ↓
HU values
 ↓
Threshold
 ↓
Binary mask
```

Example:

[
I(x,y)>300
]

could produce a mask:

```text
0 = background
1 = selected structure
```

Later, you'll learn much more advanced methods such as:

* Otsu
* region growing
* watershed
* level sets
* graph cuts
* U-Net

But thresholding is the foundation.

---

# 7.41 Common Programming Problems

When implementing pixel operations, watch for:

### 1. Integer overflow

```text
250 + 20
```

### 2. Integer underflow

```text
10 - 30
```

### 3. Division by zero

```text
pixel / 0
```

### 4. Floating-point precision

```text
float vs double
```

### 5. Type conversion

```text
double → uint8
```

### 6. Clamping

Always consider the valid output range.

### 7. Constant image normalization

If:

[
Min=Max
]

normalization denominator becomes zero.

---

# 7.42 Enterprise Image-Processing Design

Instead of putting everything into one huge function, you can separate operations:

```text
PixelOperations
 ├── add()
 ├── subtract()
 ├── multiply()
 ├── divide()
 ├── clamp()
 ├── normalize()
 ├── scale()
 ├── threshold()
 ├── binarize()
 └── invert()
```

This separation makes later integration with:

* Qt/QML
* OpenCV
* ITK
* VTK
* DICOM
* GPU processing

much easier.

---

# 7.43 Chapter 7 Mental Model

Memorize this:

```text
              ORIGINAL PIXEL
                     │
                     ↓
          ┌─────────────────────┐
          │  Pixel Operation    │
          └─────────────────────┘
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
     + / -          × / ÷       Threshold
       │             │             │
       └─────────────┼─────────────┘
                     ↓
                Clamp / Scale
                     ↓
                Normalize
                     ↓
             Binary / Invert
                     ↓
              OUTPUT PIXEL
```

---

# 7.44 Key Formulas

### Addition

[
\boxed{I'=I+C}
]

### Subtraction

[
\boxed{I'=I-C}
]

### Multiplication

[
\boxed{I'=aI}
]

### Division

[
\boxed{I'=\frac{I}{a}}
]

### Clamping

[
\boxed{
I'=\min(\max(I,L),U)
}
]

### Normalization

[
\boxed{
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
}
]

### General range mapping

[
\boxed{
I'=
\frac{I-I_{min}}
{I_{max}-I_{min}}
(O_{max}-O_{min})
+O_{min}
}
]

### Binary threshold

[
\boxed{
I'=
\begin{cases}
1,&I\ge T\
0,&I<T
\end{cases}
}
]

### 8-bit inversion

[
\boxed{
I'=255-I
}
]

---

# Chapter 7 — Knowledge Check

### Basic operations

1. What is a pixel-level operation?
2. Write the formula for pixel addition.
3. How does subtraction affect brightness?
4. How does multiplication affect contrast?
5. What problem can occur when adding values to an 8-bit image?
6. Why must division check for zero?

### Clamping

7. What is clamping?
8. Clamp these values to `[0,255]`:

```text
-20, 50, 260, 500
```

### Normalization

9. What is normalization?
10. Normalize `50` when:

[
Min=0,\quad Max=100
]

11. Normalize `75` from `[50,150]` to `[0,255]`.

### Thresholding

12. What is thresholding?
13. If (T=100), convert:

```text
20 80 100 150 200
```

to binary `0/1`.

14. Why is thresholding useful for segmentation?

### Binary conversion

15. What is the difference between binary `0/1` and binary `0/255`?
16. Where would you typically use a binary mask?

### Inversion

17. Invert these 8-bit values:

```text
0, 50, 100, 200, 255
```

18. What is the inversion formula for a 16-bit unsigned image?

---

# Practical Exercise

Given:

```text
Image:

10   50   100
150  200  250
30   80   120
```

### Exercise 1 — Addition

Apply:

[
I'=I+20
]

### Exercise 2 — Subtraction

Apply:

[
I'=I-40
]

Clamp to:

[
[0,255]
]

### Exercise 3 — Multiplication

Apply:

[
I'=1.5I
]

Clamp to `[0,255]`.

### Exercise 4 — Normalization

Find:

[
Min
]

and:

[
Max
]

Then normalize the entire image to:

[
[0,1]
]

### Exercise 5 — Thresholding

Use:

[
T=100
]

Create a binary `0/1` image.

### Exercise 6 — Inversion

Create the inverted 8-bit image:

[
I'=255-I
]

---

## Medical Imaging Exercise

Suppose a CT slice has values:

```text
-1000
-500
-100
0
100
500
1000
2000
```

Explain what happens when you apply:

1. **Clamping** to `[-500,1000]`
2. **Normalization** to `[0,1]`
3. **Scaling** by `0.5`
4. **Thresholding** at `300`
5. **Inversion**

For each operation, state whether it primarily changes:

* intensity values
* dynamic range
* binary representation
* visualization

---

## One Very Important Rule

For medical imaging, always distinguish:

```text
RAW / ORIGINAL DATA
        ↓
Processing operation
        ↓
Derived data
        ↓
Display mapping
        ↓
Screen
```

Do **not** overwrite the original quantitative medical data merely because you want a different display.

That distinction will become increasingly important when we reach **DICOM, CT Hounsfield Units, window/level, segmentation, and medical image viewers**.

---

**Chapter 7 is complete.**

### Next, strictly according to your index:

# Chapter 8 — Intensity Transformations

Topics:

* Linear transformation
* Contrast stretching
* Log transformation
* Power-law transformation
* Gamma correction
* Piecewise transformations
* Lookup tables (LUT) 
