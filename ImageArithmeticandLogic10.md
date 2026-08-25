# Chapter 10 — Image Arithmetic and Logic

We continue **strictly according to your index**.

Chapter 10 topics:

1. Image addition
2. Image subtraction
3. Image averaging
4. Image masking
5. AND
6. OR
7. XOR
8. NOT
9. Difference images
10. Background subtraction 

The key difference from Chapter 7 is:

> **Chapter 7 operated mainly on individual pixel values. Chapter 10 performs operations between images or between an image and a mask.**

---

# 10.1 What Is Image Arithmetic?

Suppose we have two images:

[
A(x,y)
]

and:

[
B(x,y)
]

Image arithmetic calculates a new image:

[
C(x,y)=F(A(x,y),B(x,y))
]

For example:

[
C=A+B
]

or:

[
C=A-B
]

Conceptually:

```text
Image A ─────┐
             ├──→ Arithmetic Operation ──→ Image C
Image B ─────┘
```

The important requirement is usually that the images have compatible:

* dimensions
* pixel locations
* data types or convertible types

---

# 10.2 Why Image Arithmetic?

Image arithmetic is fundamental for:

* image comparison
* noise reduction
* averaging
* background removal
* image registration evaluation
* mask application
* temporal analysis
* subtraction imaging
* medical image processing

---

# 10.3 Image Addition

For two images:

[
\boxed{
C(x,y)=A(x,y)+B(x,y)
}
]

Each corresponding pair of pixels is added.

Example:

```text id="j5f2s6"
A:

10 20
30 40
```

and:

```text id="v5g1am"
B:

5  10
20 30
```

Then:

```text id="2v1d0q"
C:

15 30
50 70
```

---

# 10.4 Pixel Correspondence

Notice that:

```text id="g8c3y5"
A(0,0) + B(0,0)
A(0,1) + B(0,1)
A(1,0) + B(1,0)
A(1,1) + B(1,1)
```

We are not combining arbitrary pixels.

We combine corresponding coordinates.

Therefore:

[
C(x,y)=A(x,y)+B(x,y)
]

---

# 10.5 Overflow in Image Addition

Suppose:

```text id="s5krp8"
A = 200
B = 100
```

Then:

[
200+100=300
]

For an 8-bit output:

[
0\ldots255
]

300 is invalid.

So we need to decide whether to:

* clamp
* use a wider type
* normalize
* use floating-point intermediate values

For example:

[
300\rightarrow255
]

if clamped.

---

# 10.6 Image Subtraction

Image subtraction:

[
\boxed{
C(x,y)=A(x,y)-B(x,y)
}
]

Example:

```text id="7yrk4u"
A:

100 150
200 250
```

```text id="j0m2j5"
B:

20  50
80  100
```

Result:

```text id="jvry60"
C:

80  100
120 150
```

---

# 10.7 Why Image Subtraction?

Subtraction is particularly useful for finding differences.

For example:

```text id="9e4m4m"
Image A
   ↓
Image B
   ↓
A - B
   ↓
Differences become visible
```

Applications include:

* before/after comparison
* motion detection
* background subtraction
* temporal subtraction
* image registration evaluation

---

# 10.8 Absolute Difference

Sometimes we don't care whether the difference is positive or negative.

We want:

[
\boxed{
D(x,y)=|A(x,y)-B(x,y)|
}
]

Example:

```text id="qg1q4h"
A = 100
B = 130
```

Then:

[
D=|100-130|=30
]

Reverse them:

[
|130-100|=30
]

So absolute difference is symmetric.

---

# 10.9 Difference Image

A **difference image** represents how much two images differ at each coordinate.

```text id="g6d1cm"
Image A ─────┐
             ├──→ |A-B| ──→ Difference Image
Image B ─────┘
```

If two pixels are identical:

[
|A-B|=0
]

If they are very different:

[
|A-B|
]

is large.

---

# 10.10 Difference Image Example

Suppose:

```text id="d1g6v9"
A:

100 100
100 100
```

and:

```text id="p2dr67"
B:

100 120
100 150
```

Difference:

```text id="z6x1qk"
0  20
0  50
```

So the changed areas are immediately visible.

---

# 10.11 Medical Imaging — Difference Images

Difference images can be useful for:

* comparing repeated acquisitions
* image quality evaluation
* registration validation
* temporal subtraction
* detecting processing changes

But in medical applications, differences can arise from:

```text id="w4xv34"
actual anatomy
+
patient movement
+
registration error
+
noise
+
different acquisition conditions
```

Therefore a difference image must be interpreted carefully.

---

# 10.12 Image Averaging

Suppose we have multiple images of the same scene:

[
I_1,I_2,\ldots,I_N
]

We can calculate their average:

[
\boxed{
I_{avg}(x,y)=
\frac{1}{N}
\sum_{k=1}^{N}I_k(x,y)
}
]

This is useful for reducing random noise.

---

# 10.13 Why Averaging Reduces Noise

Suppose:

```text id="84cn1d"
Image
=
Signal
+
Random Noise
```

If the noise is random and approximately zero-mean, averaging multiple acquisitions can reduce the random component.

Conceptually:

```text id="1f47b2"
Image 1 = Signal + Noise 1
Image 2 = Signal + Noise 2
Image 3 = Signal + Noise 3
Image 4 = Signal + Noise 4
             ↓
          Average
             ↓
       Signal + less random noise
```

---

# 10.14 Noise Reduction Through Averaging

For independent zero-mean noise with standard deviation:

[
\sigma
]

averaging (N) images ideally gives approximately:

[
\boxed{
\sigma_{avg}=\frac{\sigma}{\sqrt{N}}
}
]

So:

### 1 image

[
\sigma
]

### 4 images

[
\frac{\sigma}{2}
]

### 9 images

[
\frac{\sigma}{3}
]

This is an idealized model; real imaging systems can have correlated or structured noise.

---

# 10.15 Example

Suppose noise standard deviation is:

[
\sigma=20
]

and we average:

[
N=4
]

images.

Then:

[
\sigma_{avg}
============

\frac{20}{\sqrt4}
]

[
=10
]

So the noise standard deviation is approximately halved.

---

# 10.16 Important Requirement for Averaging

The images should represent the same spatial content.

If the patient moves:

```text id="5kqbd3"
Image 1
   ↓
Image 2 shifted
   ↓
Image 3 shifted
```

then averaging may cause:

```text id="7cjqtk"
Blur
Ghosting
Loss of detail
```

Therefore:

> **Alignment/registration matters before averaging.**

---

# 10.17 Image Masking

A mask is usually an image containing values that determine where processing should occur.

For a binary mask:

```text id="n3uk2a"
0 = exclude
1 = include
```

Example:

```text id="x8z0ph"
Image:

100 120 140
160 180 200
220 240 250
```

Mask:

```text id="y6p7v5"
1 1 0
1 0 0
1 1 1
```

Apply mask:

```text id="z0h3x9"
100 120 0
160 0   0
220 240 250
```

---

# 10.18 Masking Formula

For a binary mask (M(x,y)):

[
\boxed{
C(x,y)=A(x,y)M(x,y)
}
]

If:

[
M=1
]

then:

[
C=A
]

If:

[
M=0
]

then:

[
C=0
]

---

# 10.19 Why Masking?

Masking allows us to select a region of interest.

```text id="h5r4we"
Full Image
     ↓
Mask
     ↓
Region of Interest
     ↓
Processing
```

Applications:

* ROI analysis
* segmentation
* organ extraction
* tumor analysis
* background removal
* statistics within selected regions

---

# 10.20 Medical Imaging Example

Suppose:

```text id="l9xj4m"
CT Image
   +
Organ Mask
   ↓
Masked CT
   ↓
Calculate statistics
```

Now mean, standard deviation, percentiles etc. can be calculated only inside the selected region.

This is much more useful than calculating statistics over the entire image.

---

# 10.21 Logical Image Operations

Logic operations generally operate on binary or bit-valued images.

The main operations are:

```text id="n8qg6r"
AND
OR
XOR
NOT
```

Think:

```text
0 = false
1 = true
```

---

# 10.22 AND

Truth table:

|  A |  B | A AND B |
| -: | -: | ------: |
|  0 |  0 |       0 |
|  0 |  1 |       0 |
|  1 |  0 |       0 |
|  1 |  1 |       1 |

Formula conceptually:

[
C=A\land B
]

AND selects pixels where **both** images are active.

---

# 10.23 AND for Image Masks

Suppose:

```text id="kysv55"
Mask A:

1 1 0
1 0 0
1 1 1
```

and:

```text id="xk2w9q"
Mask B:

1 0 1
1 1 0
0 1 1
```

Then:

```text id="j9v5iq"
A AND B:

1 0 0
1 0 0
0 1 1
```

This creates the intersection of two masks.

---

# 10.24 OR

Truth table:

|  A |  B | A OR B |
| -: | -: | -----: |
|  0 |  0 |      0 |
|  0 |  1 |      1 |
|  1 |  0 |      1 |
|  1 |  1 |      1 |

So:

[
C=A\lor B
]

OR selects a pixel if **either** mask contains it.

---

# 10.25 OR Example

```text id="78m2ka"
A:

1 1 0
1 0 0
```

```text id="h5k6jk"
B:

0 1 1
0 0 1
```

Result:

```text id="l0l9zx"
A OR B:

1 1 1
1 0 1
```

This is similar to taking the union of two binary regions.

---

# 10.26 XOR

XOR means:

> True when exactly one input is true.

Truth table:

|  A |  B | A XOR B |
| -: | -: | ------: |
|  0 |  0 |       0 |
|  0 |  1 |       1 |
|  1 |  0 |       1 |
|  1 |  1 |       0 |

So:

```text id="v05qv9"
Same
 ↓
0

Different
 ↓
1
```

---

# 10.27 XOR and Difference

XOR can identify locations where two binary masks differ.

Example:

```text id="4h3jhj"
Mask A:

1 1
0 0
```

Mask B:

```text id="u6d8xm"
1 0
0 1
```

XOR:

```text id="x5t5t0"
0 1
0 1
```

This identifies the regions where the masks disagree.

---

# 10.28 NOT

NOT reverses binary values:

[
\boxed{
C=\neg A
}
]

Truth table:

|  A | NOT A |
| -: | ----: |
|  0 |     1 |
|  1 |     0 |

Example:

```text id="n3l5wq"
A:

1 0
0 1
```

NOT:

```text id="3v8d8u"
0 1
1 0
```

This is useful for creating an inverse mask.

---

# 10.29 Mask Logic Mental Model

```text id="w1w1rj"
Mask A ──┐
         ├── AND ──→ Intersection
Mask B ──┘

Mask A ──┐
         ├── OR ───→ Union
Mask B ──┘

Mask A ──┐
         ├── XOR ──→ Difference
Mask B ──┘

Mask A ───── NOT ─→ Inverse
```

---

# 10.30 Image Arithmetic vs Logic

This distinction is important.

### Arithmetic

Uses numerical values:

```text id="m40f1x"
+
-
×
÷
```

### Logic

Uses binary/bit values:

```text id="yr1z7s"
AND
OR
XOR
NOT
```

Think:

```text id="3h5qu8"
Image Arithmetic
     ↓
Intensity calculations

Image Logic
     ↓
Mask / binary operations
```

---

# 10.31 Background Subtraction

Background subtraction attempts to remove a known or estimated background.

Suppose:

```text id="9k4n9f"
Observed image
=
Foreground
+
Background
```

If we have an estimate of the background:

[
B(x,y)
]

then:

[
\boxed{
F(x,y)=I(x,y)-B(x,y)
}
]

where:

* (I) = observed image
* (B) = background
* (F) = foreground/difference

---

# 10.32 Background Subtraction Example

Observed:

```text id="d75w8t"
100 120 140
160 180 200
```

Background:

```text id="6c9yjd"
80  80  80
80  80  80
```

Subtract:

```text id="v9u1hk"
20 40 60
80 100 120
```

The remaining image emphasizes what differs from the background.

---

# 10.33 Background Subtraction in Imaging

It can be used in:

* microscopy
* photography
* industrial inspection
* motion detection
* fluoroscopy-like temporal processing
* detector correction concepts

But medical images often contain more complicated acquisition effects than a simple static background.

---

# 10.34 Temporal Subtraction

Suppose:

```text id="j3v1t5"
Image at time T1
       ↓
Image at time T2
       ↓
Subtract
       ↓
Changes
```

This is called temporal subtraction in appropriate applications.

Conceptually:

[
D(x,y)=I_{T2}(x,y)-I_{T1}(x,y)
]

It can highlight changes between acquisitions.

---

# 10.35 Medical Image Registration Connection

If two images are not aligned:

```text id="o5h7d1"
Image A
  ↓
Image B shifted
  ↓
Difference
  ↓
Large differences everywhere
```

Even if anatomy is actually the same.

Therefore:

```text id="p1s6as"
Registration
    ↓
Alignment
    ↓
Difference image
```

This is one reason image registration is so important in medical imaging.

---

# 10.36 Image Averaging and SNR

If averaging reduces random noise:

[
\sigma_N\rightarrow\frac{\sigma_N}{\sqrt N}
]

then, under a simplified fixed-signal model:

[
SNR\propto\sqrt N
]

So:

```text id="4v2n5c"
4 images
 ↓
noise ≈ half
 ↓
SNR ≈ double
```

Again, this is an idealized result and assumes independent noise and consistent alignment.

---

# 10.37 Image Arithmetic and Data Types

This is one of the most important implementation topics.

Suppose two `uint8` images are added:

```text id="d04xgt"
200 + 100 = 300
```

If you perform the arithmetic directly in an 8-bit representation, you can lose information.

A safer pattern is:

```text id="3m9g8e"
uint8 input
   ↓
wider intermediate type
   ↓
arithmetic
   ↓
clamp/normalize
   ↓
output type
```

For example:

```cpp id="w4s8c7"
int value =
    static_cast<int>(a) +
    static_cast<int>(b);

value = std::clamp(value, 0, 255);
```

---

# 10.38 C++ Image Addition

```cpp id="47wq2n"
void addImages(
    const std::vector<std::uint8_t>& a,
    const std::vector<std::uint8_t>& b,
    std::vector<std::uint8_t>& output)
{
    if (a.size() != b.size())
        return;

    output.resize(a.size());

    for (std::size_t i = 0; i < a.size(); ++i)
    {
        int value =
            static_cast<int>(a[i]) +
            static_cast<int>(b[i]);

        value = std::clamp(value, 0, 255);

        output[i] =
            static_cast<std::uint8_t>(value);
    }
}
```

---

# 10.39 C++ Absolute Difference

```cpp id="d7p9ju"
void absoluteDifference(
    const std::vector<std::uint8_t>& a,
    const std::vector<std::uint8_t>& b,
    std::vector<std::uint8_t>& output)
{
    if (a.size() != b.size())
        return;

    output.resize(a.size());

    for (std::size_t i = 0; i < a.size(); ++i)
    {
        int diff =
            static_cast<int>(a[i]) -
            static_cast<int>(b[i]);

        diff = std::abs(diff);

        output[i] =
            static_cast<std::uint8_t>(
                std::min(diff, 255));
    }
}
```

The important detail is performing subtraction in a signed/wider type.

---

# 10.40 C++ Image Averaging

```cpp id="wzzdvi"
void averageImages(
    const std::vector<
        std::vector<std::uint8_t>>& images,
    std::vector<std::uint8_t>& output)
{
    if (images.empty())
        return;

    const std::size_t count =
        images.front().size();

    output.resize(count);

    for (std::size_t i = 0; i < count; ++i)
    {
        double sum = 0.0;

        for (const auto& image : images)
        {
            sum += image[i];
        }

        sum /= images.size();

        output[i] =
            static_cast<std::uint8_t>(
                std::clamp(sum, 0.0, 255.0));
    }
}
```

For a production implementation, you'd also validate:

* all images have equal dimensions
* same geometry
* same pixel type
* compatible acquisition conditions

---

# 10.41 C++ Binary Mask AND

If masks contain `0` and `1`:

```cpp id="c1h8w2"
std::uint8_t andMask(
    std::uint8_t a,
    std::uint8_t b)
{
    return a && b;
}
```

For `0/255` masks, bitwise/logical semantics need to be chosen deliberately.

For example, bitwise:

```cpp id="2t9n9b"
result = a & b;
```

is different from treating nonzero values as logical `true`.

Always know whether your mask representation is:

```text id="n8n6u8"
0 / 1
```

or:

```text id="c5y6fv"
0 / 255
```

---

# 10.42 Masking With a Medical ROI

Suppose we have:

```text id="u0r5x2"
CT Image
     +
Organ Mask
```

We can calculate:

[
\mu_{ROI}
=========

\frac{
\sum I(x,y)M(x,y)
}{
\sum M(x,y)
}
]

This calculates the mean only over selected pixels.

This is a powerful connection between:

```text id="y9r7b5"
Chapter 6: Statistics
+
Chapter 10: Masking
```

---

# 10.43 ROI Statistics

For a mask:

[
M(x,y)\in{0,1}
]

the selected pixels are:

[
M(x,y)=1
]

Then:

```text id="1up2jr"
Image
 ↓
Mask
 ↓
Selected pixels
 ↓
Mean
Median
StdDev
Min
Max
Percentiles
```

This becomes very important in:

* tumor measurements
* organ analysis
* radiomics
* quantitative image analysis

---

# 10.44 Difference Image Quality

A difference image should be interpreted carefully.

Large difference can mean:

```text id="xwzq73"
Anatomical change
OR
Patient motion
OR
Registration error
OR
Noise
OR
Different acquisition parameters
OR
Processing difference
```

Therefore:

[
\boxed{
Difference \neq Automatically Pathology
}
]

This is a critical medical-imaging principle.

---

# 10.45 Arithmetic Pipeline

```text id="y6n8ju"
Image A ───────────────┐
                       │
                       ├── Addition
                       ├── Subtraction
                       ├── Average
                       ├── Difference
                       │
Image B ───────────────┘

Image
  +
Mask
  ↓
ROI / Masked Image

Mask A
  +
Mask B
  ↓
AND / OR / XOR / NOT
```

---

# 10.46 Chapter 10 Mental Model

Memorize this:

```text id="cz6zli"
                 IMAGE ARITHMETIC
                        │
        ┌───────────────┼────────────────┐
        ↓               ↓                ↓
      Add            Subtract         Average
        │               │                │
        │               ↓                ↓
        │          Difference         Noise
        │               │             Reduction
        │               │
        └───────────────┼────────────────┘
                        ↓
                     Masking
                        │
                        ↓
                Logical Operations
                  ┌────┬────┬────┐
                  ↓    ↓    ↓    ↓
                 AND   OR   XOR  NOT
                        │
                        ↓
                Background /
                ROI Processing
```

---

# 10.47 Comparison Table

| Operation              | Formula               | Main Use                    |   |                          |
| ---------------------- | --------------------- | --------------------------- | - | ------------------------ |
| Addition               | (A+B)                 | Combining intensities       |   |                          |
| Subtraction            | (A-B)                 | Detecting changes           |   |                          |
| Absolute difference    | (                     | A-B                         | ) | Difference visualization |
| Averaging              | (\frac{1}{N}\sum I_i) | Noise reduction             |   |                          |
| Masking                | (I\times M)           | ROI selection               |   |                          |
| AND                    | (A\land B)            | Mask intersection           |   |                          |
| OR                     | (A\lor B)             | Mask union                  |   |                          |
| XOR                    | (A\oplus B)           | Mask difference             |   |                          |
| NOT                    | (\neg A)              | Inverse mask                |   |                          |
| Background subtraction | (I-B)                 | Remove estimated background |   |                          |

---

# 10.48 Key Formulas

### Addition

[
\boxed{C=A+B}
]

### Subtraction

[
\boxed{C=A-B}
]

### Absolute difference

[
\boxed{D=|A-B|}
]

### Average

[
\boxed{
I_{avg}=
\frac{1}{N}
\sum_{i=1}^{N}I_i
}
]

### Masking

[
\boxed{
I_{masked}=I\times M
}
]

### Background subtraction

[
\boxed{
F=I-B
}
]

### Noise reduction by averaging

[
\boxed{
\sigma_{avg}\approx
\frac{\sigma}{\sqrt N}
}
]

---

# 10.49 Knowledge Check

### Arithmetic

1. What is image addition?
2. Why can image addition overflow?
3. What is image subtraction used for?
4. Why is absolute difference useful?
5. What is image averaging?
6. Why can averaging reduce random noise?

### Masking

7. What is an image mask?
8. What does `0` mean in a binary mask?
9. What does `1` mean?
10. How does:

[
I\times M
]

create a masked image?

### Logic

11. What does AND do?
12. What does OR do?
13. What does XOR do?
14. What does NOT do?
15. What is the difference between AND and OR?

### Difference

16. What is a difference image?
17. Why might registration be required before subtraction?
18. Why doesn't a large difference automatically indicate pathology?

### Background

19. What is background subtraction?
20. What assumptions are made about the background?
21. Where can background subtraction be useful?

---

# 10.50 Practical Exercise

Given:

```text id="xwtx0d"
Image A:

100 120 140
160 180 200
220 240 250
```

and:

```text id="5h7j29"
Image B:

50  100 120
100 150 160
200 220 240
```

### Exercise 1

Calculate:

[
A+B
]

### Exercise 2

Calculate:

[
A-B
]

### Exercise 3

Calculate:

[
|A-B|
]

### Exercise 4

Calculate the average:

[
\frac{A+B}{2}
]

### Exercise 5

Use this mask:

```text id="axd0kz"
1 1 0
1 0 0
1 1 1
```

to mask Image A.

---

# 10.51 Logic Exercise

Given:

```text id="v7s1qz"
A:

1 1 0
1 0 0
1 1 1
```

```text id="2q8cv4"
B:

1 0 1
1 1 0
0 1 1
```

Calculate:

### AND

[
A\land B
]

### OR

[
A\lor B
]

### XOR

[
A\oplus B
]

### NOT A

[
\neg A
]

---

# 10.52 Medical Imaging Exercise

You have two registered CT images:

```text id="q0gphm"
CT_Pre
CT_Post
```

You want to investigate changes.

Design a pipeline:

```text id="pgd5c6"
CT_Pre
   │
   ├───────────────┐
   │               │
   ↓               ↓
Registration     CT_Post
   │               │
   └───────┬───────┘
           ↓
     Absolute Difference
           ↓
      Difference Image
           ↓
       ROI Mask
           ↓
      ROI Statistics
```

Explain why each step is required.

---

## Final Takeaway

Chapter 10 gives you the mathematical foundation for comparing and combining images:

[
\boxed{
\text{Image A}
+
\text{Image B}
\rightarrow
\text{New Image}
}
]

and:

[
\boxed{
\text{Image}
+
\text{Mask}
\rightarrow
\text{ROI}
}
]

and:

[
\boxed{
\text{Mask A}
+
\text{Mask B}
\rightarrow
AND/OR/XOR/NOT
}
]

For your medical-image software, the most important concepts are:

**absolute difference → averaging → masking → ROI statistics → image subtraction → background/temporal subtraction.**

**Chapter 10 complete.**

### Next, strictly according to your index:

# Chapter 11 — Neighborhood Processing

Topics:

* Neighborhood concept
* 4-connected
* 8-connected
* Local operations
* Mean filter
* Median filter
* Min / Max filter
* Rank filters
* Local statistics
* Sliding window 
