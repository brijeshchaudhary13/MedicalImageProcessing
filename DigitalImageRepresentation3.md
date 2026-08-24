# Chapter 3 — Digital Image Representation

We now move to **Chapter 3 exactly according to your index**. The chapter contains:

1. Binary images
2. Grayscale images
3. RGB images
4. RGBA
5. Multichannel images
6. Image matrices
7. Memory representation
8. Pixel data types
9. UINT8
10. INT8
11. UINT16
12. INT16
13. FLOAT32
14. FLOAT64

I will stay within these topics and build them from the fundamentals. 

---

# 3.1 What Does "Image Representation" Mean?

In Chapter 2, we learned:

> A digital image is numerical data organized spatially.

Now we need to answer:

> **How exactly are those numbers represented inside a computer?**

For example, a human sees:

```text id="w0s9b4"
████████
██      ██
██      ██
████████
```

The computer needs something like:

```text id="l9j3x2"
10  20  30  40
50  60  70  80
90 100 110 120
```

So image representation describes:

```text id="m2q7z1"
Image
 │
 ├── Shape
 │
 ├── Pixel arrangement
 │
 ├── Number of channels
 │
 ├── Data type
 │
 └── Memory layout
```

This becomes extremely important when you work with **C++, OpenCV, ITK, VTK and medical images**.

---

# 3.2 Binary Images

A binary image contains only **two possible values**.

Usually:

[
0
]

and:

[
1
]

Conceptually:

```text id="4wq0sa"
0 → Black
1 → White
```

Example:

```text id="4j1n6c"
0 0 0 0 0
0 1 1 1 0
0 1 1 1 0
0 0 0 0 0
```

This can represent a simple object mask.

---

# 3.3 Why Binary Images Are Useful

Binary images are extremely important for **segmentation**.

Suppose we have a CT image:

```text id="8yr6qk"
CT
 ↓
Segmentation
 ↓
Binary mask
```

The mask might contain:

```text id="h2qz4f"
0 → background
1 → target structure
```

For example:

```text id="d5m1g8"
Original CT

██████████████
████  ████  ██
███   ████   ██
██████████████


Binary Mask

00000000000000
00001111000000
00001111000000
00000000000000
```

This means:

```text id="k2f4cw"
0 → not part of object
1 → part of object
```

Later, when we study segmentation, this concept will become fundamental.

---

# 3.4 Binary Image in C++

Conceptually:

```cpp
bool pixel = true;
```

or:

```cpp
std::uint8_t pixel = 0;
```

and:

```cpp
pixel = 1;
```

However, actual image libraries may use different storage conventions.

For example, OpenCV often represents a binary mask using `CV_8U`, where:

```text id="zq1n0c"
0   → background
255 → foreground
```

rather than:

```text
0 / 1
```

This is an important practical distinction.

---

# 3.5 Grayscale Images

A grayscale image represents intensity using a single channel.

For an 8-bit grayscale image:

[
0\leq I(x,y)\leq255
]

Typical interpretation:

```text id="h3n9yq"
0
↓
Black

128
↓
Gray

255
↓
White
```

Example:

```text id="w1r5k7"
10   50  100
80  150  200
30  120  255
```

Each pixel has **one value**.

---

# 3.6 Grayscale Representation

A grayscale image can be represented as:

[
I(x,y)
]

For example:

[
I(10,20)=120
]

There is only one value associated with that pixel.

Conceptually:

```text id="j4n2v7"
Pixel
  │
  └── Intensity
```

This is different from RGB.

---

# 3.7 RGB Images

RGB means:

* **R** = Red
* **G** = Green
* **B** = Blue

Each pixel contains three values.

For example:

```text id="x7n2qc"
Pixel
 ├── R
 ├── G
 └── B
```

For an 8-bit RGB image:

[
R,G,B\in[0,255]
]

Therefore one pixel might be:

[
(255,0,0)
]

which represents pure red.

Another:

[
(0,255,0)
]

represents green.

Another:

[
(0,0,255)
]

represents blue.

---

# 3.8 RGB Image as Three Matrices

An RGB image can conceptually be represented using three matrices:

```text id="7r7gqa"
R channel

R R R
R R R
R R R


G channel

G G G
G G G
G G G


B channel

B B B
B B B
B B B
```

Together:

```text id="1l5dsv"
RGB Image
   │
   ├── Red matrix
   ├── Green matrix
   └── Blue matrix
```

Mathematically:

[
I(x,y)=
\begin{bmatrix}
R(x,y)\
G(x,y)\
B(x,y)
\end{bmatrix}
]

So a pixel is no longer one number.

It is a vector of three numbers.

---

# 3.9 RGB Pixel Example

Suppose:

[
I(10,20)=
\begin{bmatrix}
255\
100\
50
\end{bmatrix}
]

This means:

```text id="l0u7b4"
R = 255
G = 100
B = 50
```

So:

```text id="51f0tb"
Grayscale pixel
      ↓
one value

RGB pixel
      ↓
three values
```

---

# 3.10 RGBA Images

RGBA adds an additional channel:

* R = Red
* G = Green
* B = Blue
* A = Alpha

Therefore:

[
I(x,y)=
\begin{bmatrix}
R\G\B\A
\end{bmatrix}
]

The alpha channel usually represents:

> **Opacity/transparency information.**

---

# 3.11 Alpha Example

Suppose:

```text id="2cbm7d"
R = 255
G = 0
B = 0
A = 255
```

This is:

```text id="ydt5l9"
Opaque red
```

If:

```text
A = 0
```

it is fully transparent.

Conceptually:

```text id="75g3jp"
Alpha = 255
   ↓
Fully visible

Alpha = 0
   ↓
Fully transparent
```

---

# 3.12 Why Alpha Is Useful

Alpha is useful for:

* overlays
* transparent graphics
* annotations
* UI elements
* image compositing

In medical imaging, transparency is particularly useful when displaying:

```text id="a3g4x9"
CT
 +
Segmentation Mask
 +
Dose Overlay
```

For example:

```text id="2mt7h6"
CT Image
   ↓
Semi-transparent segmentation
   ↓
User sees both anatomy and segmentation
```

---

# 3.13 Multichannel Images

A **channel** is an independent set of values associated with every spatial location.

Examples:

### Grayscale

```text id="n6c1f0"
1 channel
```

### RGB

```text id="w7s4q8"
3 channels
```

### RGBA

```text id="6w8x3d"
4 channels
```

A general image can be represented as:

[
I(x,y,c)
]

where:

* (x) = horizontal position
* (y) = vertical position
* (c) = channel

---

# 3.14 Channel Dimension

Suppose an RGB image is:

[
512\times512\times3
]

The dimensions can be interpreted as:

```text id="o8t0kh"
512 → width
512 → height
3   → channels
```

Total stored values:

[
512\times512\times3
]

[
=786,432
]

values.

If each value is one byte:

[
786,432\text{ bytes}
]

approximately:

[
768\text{ KiB}
]

---

# 3.15 Medical Images and Channels

Medical images can be more complicated.

For example:

```text id="u5b3xz"
CT
 ↓
1 intensity value per voxel
```

But a medical dataset may also have:

```text id="h7v9qm"
CT
+
MRI
+
Segmentation
+
Dose
```

These can be treated as separate image volumes or channels depending on the application.

For AI, we might eventually represent an input as:

```text id="7n6g4j"
Channel 1 → CT
Channel 2 → MRI
Channel 3 → another modality
```

But this is an application-level representation; we must not confuse it with the underlying DICOM organization.

---

# 3.16 Image Matrices

A grayscale image:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

is naturally represented as a matrix.

Rows correspond to one spatial dimension.

Columns correspond to another.

Conceptually:

```text id="5x7k5k"
       x →
     0   1   2

y 0 10  20  30
↓ 1 40  50  60
  2 70  80  90
```

---

# 3.17 Matrix Representation in Memory

Although we think of an image as a matrix:

```text id="k8h5nq"
10 20 30
40 50 60
70 80 90
```

computer memory is generally linear.

It may actually be stored as:

```text id="f9o1qg"
10 → 20 → 30 → 40 → 50 → 60 → 70 → 80 → 90
```

This is called a **linear memory representation**.

---

# 3.18 Row-Major Representation

A common representation is row-major:

```text id="xj8c0e"
Row 0:
10 20 30

Row 1:
40 50 60

Row 2:
70 80 90
```

Memory:

```text id="d2j7cq"
10 20 30 40 50 60 70 80 90
```

The index formula is:

[
index=y\times width+x
]

For:

[
width=3
]

and:

[
(x,y)=(2,1)
]

we get:

[
index=1\times3+2
]

[
=5
]

Therefore:

```text id="xj5yyn"
memory[5] = 60
```

---

# 3.19 Why Memory Representation Matters

At first this seems like a programming detail.

It isn't.

Medical images can contain:

```text id="v4j6hf"
millions of pixels
or
millions of voxels
```

Therefore memory access can significantly affect:

* speed
* cache utilization
* memory usage
* multithreading
* GPU transfers

Later in your course, this connects directly to **performance engineering**.

---

# 3.20 Pixel Data Types

The image data type determines:

> **What numerical values can each pixel represent and how much memory each value requires.**

Examples from your index:

```text id="s8m4b1"
UINT8
INT8
UINT16
INT16
FLOAT32
FLOAT64
```

Let's understand each.

---

# 3.21 UINT8

`UINT8` means:

> Unsigned 8-bit integer.

Break it down:

```text id="n4e1sh"
U
↓
Unsigned

INT
↓
Integer

8
↓
8 bits
```

Range:

[
0\ldots255
]

because:

[
2^8=256
]

possible values.

---

# 3.22 UINT8 Example

```cpp id="6j6l1f"
#include <cstdint>

std::uint8_t pixel = 200;
```

Possible values:

```text id="cnm3xa"
0
1
2
...
255
```

Common uses:

* grayscale images
* RGB channels
* masks
* display images

---

# 3.23 INT8

`INT8` means:

> Signed 8-bit integer.

Typical range:

[
-128\ldots127
]

There are still:

[
256
]

possible values.

But the range is divided between negative and positive numbers.

```text id="5u0h8j"
-128 ───── 0 ───── 127
```

It is less common as the primary representation for ordinary grayscale images.

---

# 3.24 UINT16

`UINT16` means:

> Unsigned 16-bit integer.

Range:

[
0\ldots65535
]

because:

[
2^{16}=65536
]

possible values.

This is useful when we need more intensity levels than 8-bit representation provides.

---

# 3.25 INT16

`INT16` means:

> Signed 16-bit integer.

Typical range:

[
-32768\ldots32767
]

This is particularly important in medical imaging because signed values can represent both negative and positive quantities.

For example, medical image processing pipelines may use signed 16-bit representations.

---

# 3.26 FLOAT32

`FLOAT32` is a 32-bit floating-point representation.

Unlike integer types, it can represent fractional values.

Examples:

```text id="q5v5em"
0.5
1.25
-3.75
12.345
```

This is useful when image processing produces non-integer results.

For example:

```text id="5r8u4b"
Original pixel
      ↓
Filtering
      ↓
12.37
```

If the result is stored as an integer, fractional information may be lost.

---

# 3.27 FLOAT64

`FLOAT64` is a 64-bit floating-point representation.

It generally provides greater precision than `FLOAT32`.

Examples:

```text id="9k2nmg"
0.123456789
3.1415926535
-0.000001
```

It requires more memory than `FLOAT32`.

So:

```text id="5m1t8r"
FLOAT32
 ↓
less memory
 ↓
lower precision

FLOAT64
 ↓
more memory
 ↓
higher precision
```

The appropriate choice depends on the algorithm and required accuracy.

---

# 3.28 Data Type Comparison

| Type    | Bits |      Typical Range | Fractional? |
| ------- | ---: | -----------------: | ----------- |
| UINT8   |    8 |            0 → 255 | No          |
| INT8    |    8 |         -128 → 127 | No          |
| UINT16  |   16 |         0 → 65,535 | No          |
| INT16   |   16 |   -32,768 → 32,767 | No          |
| FLOAT32 |   32 |  Approx. ±3.4×10³⁸ | Yes         |
| FLOAT64 |   64 | Approx. ±1.8×10³⁰⁸ | Yes         |

The floating-point ranges are approximate; precision and range are separate concepts.

---

# 3.29 Memory Per Pixel

The number of bits determines how much memory one pixel requires.

### UINT8

[
8\text{ bits}=1\text{ byte}
]

### UINT16

[
16\text{ bits}=2\text{ bytes}
]

### FLOAT32

[
32\text{ bits}=4\text{ bytes}
]

### FLOAT64

[
64\text{ bits}=8\text{ bytes}
]

Therefore:

```text id="a3k7vx"
Data Type       Bytes/pixel

UINT8              1
INT8               1
UINT16             2
INT16              2
FLOAT32            4
FLOAT64            8
```

---

# 3.30 Example — 512×512 Image

Number of pixels:

[
512\times512=262,144
]

### UINT8

[
262,144\times1
==============

262,144
]

bytes.

### UINT16

[
262,144\times2
==============

524,288
]

bytes.

### FLOAT32

[
262,144\times4
==============

1,048,576
]

bytes.

### FLOAT64

[
262,144\times8
==============

2,097,152
]

bytes.

So:

```text id="g1m7e0"
Same image dimensions
        +
Different data type
        ↓
Different memory requirements
```

---

# 3.31 Medical Example

Suppose a CT volume contains:

[
512\times512\times300
]

voxels.

We calculated:

[
78,643,200
]

voxels.

If stored as `INT16`:

[
78,643,200\times2
]

[
=157,286,400
]

bytes.

Approximately:

[
150\text{ MiB}
]

Now suppose processing creates a `FLOAT32` version.

That requires:

[
78,643,200\times4
]

[
=314,572,800
]

bytes.

Approximately:

[
300\text{ MiB}
]

So simply converting:

```text id="6tw7re"
INT16
 ↓
FLOAT32
```

approximately doubles the memory requirement.

In a real medical viewer, multiple such buffers can exist simultaneously.

---

# 3.32 Why Processing Often Uses Floating Point

Suppose we have:

```text id="h9k8b5"
Pixel A = 100
Pixel B = 101
```

Their average is:

[
\frac{100+101}{2}=100.5
]

If we store this immediately as an integer, we may have to round/truncate it.

With floating point:

[
100.5
]

can be preserved.

Therefore a processing pipeline might be:

```text id="h3w0h1"
INT16 Medical Image
        ↓
Convert to FLOAT32
        ↓
Processing
        ↓
FLOAT32 result
        ↓
Convert if necessary
        ↓
Display / storage
```

The exact conversion strategy depends on the application.

---

# 3.33 Image Representation Summary

A digital image can be described by several properties:

```text id="u5o6y3"
                  IMAGE
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
     Shape       Channels      Data Type
       │            │            │
   Width/Height   Gray/RGB    UINT8/INT16
   /Depth         /RGBA       FLOAT32...
       │
       ↓
   Memory Layout
```

This is the mental model you should retain.

---

# 3.34 Grayscale vs RGB vs RGBA

### Grayscale

```text id="qg2p5u"
Pixel
 ↓
1 value
```

### RGB

```text id="a9j2t1"
Pixel
 ↓
R + G + B
 ↓
3 values
```

### RGBA

```text id="m3f7s8"
Pixel
 ↓
R + G + B + A
 ↓
4 values
```

---

# 3.35 Medical Image vs RGB Photograph

This distinction is very important.

### Photograph

Often:

```text id="0l6c8f"
Width × Height × 3
UINT8
RGB
```

### CT

Often conceptually:

```text id="c4b7k9"
Width × Height × Depth
INT16
Single intensity channel
```

But don't treat this as a universal rule. Actual medical datasets can use different representations.

The important point is:

> **Medical images frequently represent quantitative physical/reconstructed information rather than simply display colors.**

---

# 3.36 C++ Example — Different Data Types

```cpp id="z9z1uk"
#include <cstdint>

int main()
{
    std::uint8_t  gray8  = 200;
    std::int16_t  ct     = -500;
    std::uint16_t value  = 50000;
    float         result = 100.5f;
    double        precise = 100.123456789;

    return 0;
}
```

Conceptually:

```text id="t4z7q0"
gray8
 ↓
8-bit unsigned

ct
 ↓
16-bit signed

value
 ↓
16-bit unsigned

result
 ↓
32-bit floating point

precise
 ↓
64-bit floating point
```

---

# 3.37 Python / NumPy Example

NumPy allows explicit image data types:

```python id="p7k4ac"
import numpy as np

image8 = np.zeros((512, 512), dtype=np.uint8)

image16 = np.zeros((512, 512), dtype=np.int16)

image_float = np.zeros((512, 512), dtype=np.float32)

print(image8.dtype)
print(image16.dtype)
print(image_float.dtype)
```

This is extremely useful when working with medical imaging.

---

# 3.38 A Critical Programming Problem: Overflow

Suppose a `UINT8` pixel is:

[
250
]

and we add:

[
20
]

Mathematically:

[
250+20=270
]

But `UINT8` cannot represent 270.

Its maximum is:

[
255
]

So depending on the operation and language/library behavior, the result can overflow or require explicit saturation/clamping.

This is one reason image-processing code must carefully consider data types.

Later, when we study **pixel-level operations**, we will examine this in detail.

---

# 3.39 Chapter 3 Mental Model

Keep this structure:

```text id="0f6t4k"
                 DIGITAL IMAGE
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      Shape         Channels       Data Type
        │              │              │
    W × H × D      Gray/RGB/RGBA    UINT8
                                    INT16
                                    FLOAT32
                                    ...
                       │
                       ↓
                 Memory Layout
                       │
                       ↓
                 Image Processing
```

---

# Chapter 3 — Knowledge Check

Answer these before we move to Chapter 4.

### Fundamentals

1. What is binary image representation?
2. What are the typical values in a binary image?
3. What is a grayscale image?
4. How many values does one grayscale pixel contain?
5. What does RGB mean?
6. How many values does one RGB pixel contain?
7. What is RGBA?
8. What does the alpha channel represent?
9. What is a multichannel image?

### Representation

10. How can a grayscale image be represented as a matrix?
11. How can an RGB image be represented mathematically?
12. What does `512 × 512 × 3` mean for an RGB image?
13. Why is image data stored linearly in memory?
14. What is row-major representation?
15. What is the formula for converting `(x,y)` into a linear array index?

### Data types

16. What does `UINT8` mean?
17. What is the range of `UINT8`?
18. What is the range of `INT8`?
19. What is the range of `UINT16`?
20. What is the range of `INT16`?
21. What is `FLOAT32`?
22. What is `FLOAT64`?
23. How many bytes does each require?

### Medical imaging

24. Why might a CT image use `INT16`?
25. Why might image processing convert an integer image to `FLOAT32`?
26. Why does data type affect memory consumption?
27. Why is memory representation important for large CT/MRI volumes?
28. Why is a CT volume fundamentally different from an RGB photograph?

### Calculation

For a:

[
1024\times1024
]

grayscale image:

29. How many pixels are there?
30. How much memory does `UINT8` require?
31. How much memory does `UINT16` require?
32. How much memory does `FLOAT32` require?
33. How much memory does `FLOAT64` require?

For an RGB `512 × 512` image using `UINT8`:

34. How many individual channel values are stored?
35. How many bytes are required?

---

## Mini Practical Exercise

Create these five arrays in C++ or Python:

```text id="7f0m4r"
1. UINT8 grayscale
2. INT16 medical image
3. UINT16 image
4. FLOAT32 processing image
5. FLOAT64 processing image
```

For each one, determine:

```text
Data type
Range
Bytes per value
Memory for 512 × 512
```

Then answer:

> **Why shouldn't we blindly use FLOAT64 for every medical image?**

---

**Chapter 3 is complete.**

The next chapter in your exact index is:

> **Chapter 4 — Sampling and Quantization**

with:

* Sampling
* Spatial sampling
* Temporal sampling
* Quantization
* Aliasing
* Nyquist theorem
* Undersampling
* Oversampling
* Bit-depth effects. 
