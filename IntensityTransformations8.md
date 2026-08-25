# Chapter 8 — Intensity Transformations

We continue **strictly according to your master index**.

Chapter 8 contains exactly:

1. Linear transformation
2. Contrast stretching
3. Log transformation
4. Power-law transformation
5. Gamma correction
6. Piecewise transformations
7. Lookup tables (LUT) 

We will follow your requested learning style:

**WHY → HOW → Mathematics → Algorithm → C++/Python → Medical application.** 

---

# 8.1 What Is an Intensity Transformation?

An intensity transformation changes the **value of a pixel** while normally leaving its spatial location unchanged.

We can write:

[
s=T(r)
]

where:

* (r) = input intensity
* (T) = transformation function
* (s) = output intensity

Conceptually:

```text
Input Image
     │
     ▼
Input intensity r
     │
     ▼
Transformation T(r)
     │
     ▼
Output intensity s
     │
     ▼
Output Image
```

The key idea:

> **Intensity transformation changes "what value is stored/displayed", not "where the pixel is".**

---

# 8.2 Why Do Intensity Transformations Exist?

A raw image may not have the intensity distribution that is most useful for visualization or analysis.

For example:

```text
Original intensity range

        50 ───────── 100
```

but our display supports:

```text
0 ───────────────────── 255
```

The image may look low contrast.

An intensity transformation can map:

```text
50 → 0
100 → 255
```

and values between them accordingly.

So:

```text
Raw intensity
      ↓
Transformation
      ↓
More useful representation
```

---

# 8.3 Intensity Transformation vs Pixel-Level Operations

You already learned pixel-level operations in Chapter 7.

For example:

[
I'=I+20
]

is a pixel-level operation.

Chapter 8 goes deeper into **functions that define how the entire intensity range is mapped**.

Think:

```text
Chapter 7
─────────
Basic pixel operations

Chapter 8
─────────
Intensity mapping functions
```

---

# 8.4 The Transformation Curve

Imagine plotting:

```text
Output s
   ↑
255│                 /
   │               /
   │             /
   │           /
   │         /
   │       /
   │     /
  0└──────────────────→ Input r
    0                255
```

This graph tells us:

> For every input intensity, what output intensity should be produced?

This curve is the heart of intensity transformation.

---

# 8.5 Linear Transformation

The simplest transformation is:

[
\boxed{s=ar+b}
]

where:

* (a) = scaling factor
* (b) = offset

This is the same fundamental relationship introduced in the previous chapter, but now we study it as a complete transformation function.

---

# 8.6 WHY Linear Transformation?

Suppose an image is:

```text
50
60
70
80
90
```

but we want to increase the separation between these values.

We can use:

[
s=2r
]

giving:

```text
100
120
140
160
180
```

The differences are expanded.

So linear transformation is useful for:

* brightness adjustment
* contrast adjustment
* intensity scaling
* range conversion

---

# 8.7 Effect of (a)

Consider:

[
s=ar
]

### (a>1)

Intensity differences expand.

```text
Contrast ↑
```

### (0<a<1)

Intensity differences compress.

```text
Contrast ↓
```

### (a=1)

No scaling.

```text
s=r
```

### (a<0)

The intensity relationship is reversed.

This can produce an inversion-like effect when combined with an offset.

---

# 8.8 Effect of (b)

Now consider:

[
s=r+b
]

If:

[
b>0
]

the intensity values shift upward.

```text
Brightness ↑
```

If:

[
b<0
]

they shift downward.

```text
Brightness ↓
```

So:

```text
a → scaling / contrast
b → offset / brightness
```

---

# 8.9 Linear Transformation Example

Suppose:

[
s=1.5r+20
]

For:

[
r=100
]

we get:

[
s=1.5(100)+20
]

[
s=170
]

Therefore:

```text
Input = 100
   ↓
× 1.5
   ↓
150
   ↓
+ 20
   ↓
Output = 170
```

---

# 8.10 Linear Transformation and Clamping

Suppose an 8-bit image uses:

[
0\le s\le255
]

and:

[
s=2r+20
]

For:

[
r=150
]

we get:

[
s=320
]

But 320 is outside the output range.

So:

[
s=255
]

after clamping.

Therefore:

```text
Transformation
      ↓
Possible out-of-range value
      ↓
Clamping
      ↓
Valid output
```

---

# 8.11 Contrast Stretching

Contrast stretching is a special intensity transformation that expands an image's useful intensity range.

Suppose the image uses:

[
r_{min}=50
]

and:

[
r_{max}=150
]

We want:

[
s_{min}=0
]

and:

[
s_{max}=255
]

Then:

[
\boxed{
s=
\frac{r-r_{min}}
{r_{max}-r_{min}}
(s_{max}-s_{min})
+s_{min}
}
]

---

# 8.12 Contrast Stretching Example

Given:

[
r_{min}=50
]

[
r_{max}=150
]

and:

[
r=100
]

Then:

[
s=
\frac{100-50}{150-50}
(255)
]

[
s=
\frac{50}{100}(255)
]

[
s=127.5
]

approximately:

[
128
]

So:

```text
50  → 0
100 → 128
150 → 255
```

---

# 8.13 Why Contrast Stretching Works

Before:

```text
Output
255 ┤
    │
    │
    │
  0 └─────████───────
          50-150
```

Only a small part of the available range is being used.

After stretching:

```text
Output
255 ┤              █
    │            /
    │          /
    │        /
  0 └──────/
       50 → 150
```

The useful range now occupies almost the full output range.

---

# 8.14 Contrast Stretching vs Histogram Equalization

These are different.

### Contrast stretching

Usually uses:

[
r_{min},r_{max}
]

and maps them to the desired output range.

### Histogram equalization

Uses the intensity distribution and CDF.

```text
Contrast Stretching
      ↓
Range-based mapping

Histogram Equalization
      ↓
Distribution-based mapping
```

So stretching is usually simpler and more predictable.

---

# 8.15 Log Transformation

The logarithmic transformation is:

[
\boxed{
s=c\log(1+r)
}
]

where:

* (r) = input
* (c) = scaling constant
* (s) = output

The `+1` prevents:

[
\log(0)
]

---

# 8.16 WHY Log Transformation?

A log transformation is useful when the input dynamic range is very large.

Suppose:

```text
1
10
100
1000
10000
```

The difference between 1 and 10 is much smaller than between 1000 and 10000.

A logarithm compresses large values.

Conceptually:

```text
Small values
     ↓
spread relatively more

Large values
     ↓
compressed
```

---

# 8.17 Log Transformation Curve

The curve looks approximately like:

```text
Output
  ↑
  │          ______
  │       __/
  │     _/
  │   _/
  │ _/
  └────────────────→ Input
```

It rises quickly initially and then becomes progressively flatter.

Therefore:

> **Log transformation expands low-intensity differences and compresses high-intensity differences.**

---

# 8.18 Example

Suppose:

[
c=1
]

and:

[
r=9
]

Then:

[
s=\log(1+9)
]

[
s=\log(10)
]

Using natural logarithm:

[
s\approx2.303
]

The exact numerical result depends on the logarithm base and scaling used.

---

# 8.19 Medical Imaging Connection

Logarithmic transformations can be useful when an imaging system contains a large dynamic range.

However, **do not automatically apply a log transform to quantitative medical data**.

For example, CT values have physical meaning.

A display transformation may be appropriate, but permanently replacing the original quantitative values with a log-transformed version can destroy their intended interpretation.

Always distinguish:

```text
Original medical data
        ↓
Display transformation
```

from:

```text
Original medical data
        ↓
Permanent data modification
```

---

# 8.20 Power-Law Transformation

Power-law transformation is:

[
\boxed{
s=cr^\gamma
}
]

where:

* (c) = scaling constant
* (\gamma) = exponent

This is one of the most important nonlinear transformations.

---

# 8.21 WHY Power-Law Transformation?

A linear transformation cannot always provide the desired intensity curve.

Power-law transformation allows us to control the shape of the mapping.

The parameter:

[
\gamma
]

controls the curve.

```text
γ
↓
Shape of intensity mapping
```

---

# 8.22 Gamma Correction

Gamma correction is a practical use of power-law transformations.

For normalized intensities:

[
0\le r\le1
]

we can use:

[
\boxed{s=r^\gamma}
]

---

# 8.23 Gamma Less Than 1

Suppose:

[
\gamma=0.5
]

Then:

[
s=\sqrt r
]

For:

[
r=0.25
]

we get:

[
s=\sqrt{0.25}
]

[
=0.5
]

So:

```text
0.25 → 0.50
```

Dark values become relatively brighter.

Conceptually:

```text
γ < 1
 ↓
brighten darker tones
```

---

# 8.24 Gamma Greater Than 1

Suppose:

[
\gamma=2
]

Then:

[
s=r^2
]

For:

[
r=0.5
]

we get:

[
s=0.25
]

So:

```text
0.50 → 0.25
```

Lower intensities become darker.

Conceptually:

```text
γ > 1
 ↓
darken lower tones
```

---

# 8.25 Gamma = 1

If:

[
\gamma=1
]

then:

[
s=r
]

No nonlinear transformation occurs.

```text
γ = 1
 ↓
identity mapping
```

---

# 8.26 Gamma Curve Summary

```text
                 Output
                   ↑
                   │     γ < 1
                   │   /
                   │  /
                   │ /
                   │/──────── γ = 1
                   │
                   │\
                   │ \
                   │  \ γ > 1
                   └────────────────→ Input
```

More accurately, the curves depend on normalization, but the key behavior is:

| Gamma      | Effect                      |
| ---------- | --------------------------- |
| (\gamma<1) | Brightens lower intensities |
| (\gamma=1) | Identity                    |
| (\gamma>1) | Darkens lower intensities   |

---

# 8.27 Gamma in Your Image-Processing Work

Your uploaded X-ray enhancement pipeline explicitly uses a nonlinear tone mapping parameter:

```text
TONE_GAMMA
```

and records a tuning change from `13.0` to `9.5` to make the tone mapping gentler and preserve abdominal/gas detail. 

This is a practical example of why gamma/power-law transformations need careful tuning: an overly aggressive nonlinear curve can crush or saturate useful image information.

---

# 8.28 Piecewise Transformations

Sometimes one mathematical function is not enough.

We can define different mappings for different intensity ranges.

For example:

[
s=
\begin{cases}
a_1r+b_1, & r<T_1\
a_2r+b_2, & T_1\le r<T_2\
a_3r+b_3, & r\ge T_2
\end{cases}
]

This is called a **piecewise transformation**.

---

# 8.29 WHY Piecewise Transformations?

Suppose we want:

```text
Dark region
   ↓
strong enhancement

Middle region
   ↓
moderate enhancement

Bright region
   ↓
compression
```

One global linear function cannot easily do all three.

So we divide the intensity range.

```text
0 ───── T1 ───────── T2 ───── 255
  Region 1   Region 2   Region 3
```

Each region gets its own transformation.

---

# 8.30 Piecewise Example

Suppose:

[
s=
\begin{cases}
2r, & 0\le r<50\
r+50, & 50\le r<150\
200+0.5(r-150), & r\ge150
\end{cases}
]

Then:

### Dark region

[
r=20
]

[
s=40
]

### Middle region

[
r=100
]

[
s=150
]

### Bright region

[
r=200
]

[
s=200+0.5(50)
]

[
s=225
]

So different intensity regions receive different treatment.

---

# 8.31 Piecewise Transformation Curve

Conceptually:

```text
Output
  ↑
  │                 /
  │               /
  │            __/
  │         __/
  │      __/
  │   __/
  └──────────────────→ Input
      T1        T2
```

The slope changes at the breakpoints.

The slope determines how strongly that intensity region is expanded or compressed.

---

# 8.32 Medical Imaging Use of Piecewise Mapping

A medical viewer might want to:

```text
Low values
 ↓
preserve detail

Middle values
 ↓
increase contrast

High values
 ↓
compress
```

A piecewise mapping can achieve this.

This can be useful for visualization pipelines, but quantitative medical values should be preserved separately.

---

# 8.33 Lookup Table — LUT

A **Lookup Table (LUT)** stores the output value for every possible input intensity.

Instead of calculating:

[
s=T(r)
]

every time, we can precompute:

```text
LUT[input] = output
```

Then:

```text
output = LUT[input]
```

---

# 8.34 WHY LUT?

Suppose we have an 8-bit image.

There are only:

[
256
]

possible input values.

Instead of repeatedly calculating a complex transformation:

```text
log()
pow()
piecewise function
```

we can calculate the mapping once:

```text
Input     Output
0         ...
1         ...
2         ...
...
255       ...
```

Then every pixel becomes a simple table lookup.

---

# 8.35 LUT Example

Suppose:

```text
LUT[0]   = 255
LUT[1]   = 254
LUT[2]   = 253
...
LUT[255] = 0
```

This LUT performs image inversion.

Instead of:

[
s=255-r
]

for every pixel, we can do:

```cpp
output = lut[input];
```

---

# 8.36 LUT for Gamma

Suppose we want:

[
s=r^\gamma
]

for 8-bit data.

We can precompute:

```cpp
for (int r = 0; r < 256; ++r)
{
    double normalized = r / 255.0;

    double transformed =
        std::pow(normalized, gamma);

    lut[r] =
        static_cast<std::uint8_t>(
            transformed * 255.0
        );
}
```

Then:

```cpp
outputPixel = lut[inputPixel];
```

---

# 8.37 LUT Pipeline

```text
Transformation Function
          ↓
     Pre-computation
          ↓
        LUT[ ]
          ↓
     Input Pixel
          ↓
      LUT[input]
          ↓
     Output Pixel
```

This can be much faster than recalculating an expensive nonlinear function for every pixel.

---

# 8.38 LUT and Medical Imaging

LUTs are especially useful for interactive viewers.

Suppose the user changes:

```text
Window Level
Window Width
Gamma
Inversion
Contrast
```

The viewer can generate a new mapping table and apply it efficiently.

Conceptually:

```text
User changes WL/WW
       ↓
Generate LUT
       ↓
Apply LUT
       ↓
Display image
```

This is one reason LUT-based display pipelines are common in imaging software.

---

# 8.39 LUT vs Original Data

Very important:

```text
Original CT
     │
     ├───────────────→ Stored safely
     │
     ↓
Display LUT
     ↓
Screen
```

The LUT should generally be considered a **display mapping** unless you intentionally create derived processed data.

Don't overwrite the original quantitative CT values simply because the LUT changes.

---

# 8.40 Linear Transformation Using LUT

For:

[
s=ar+b
]

we can precompute:

```cpp
for (int r = 0; r < 256; ++r)
{
    double value = a * r + b;

    value =
        std::max(0.0,
        std::min(value, 255.0));

    lut[r] =
        static_cast<std::uint8_t>(value);
}
```

Then:

```cpp
output[i] = lut[input[i]];
```

---

# 8.41 Piecewise Transformation Using LUT

This is particularly convenient.

Instead of:

```cpp
if (...)
    ...
else if (...)
    ...
else
    ...
```

for every pixel, calculate the piecewise mapping once.

```text
Piecewise Function
      ↓
Generate LUT
      ↓
Apply LUT to all pixels
```

This improves the separation between:

```text
Transformation definition
```

and:

```text
Image processing execution
```

---

# 8.42 C++ Generic LUT Application

```cpp
#include <array>
#include <cstdint>
#include <vector>

using Lut = std::array<std::uint8_t, 256>;

void applyLut(
    const std::vector<std::uint8_t>& input,
    std::vector<std::uint8_t>& output,
    const Lut& lut)
{
    output.resize(input.size());

    for (std::size_t i = 0; i < input.size(); ++i)
    {
        output[i] = lut[input[i]];
    }
}
```

The processing loop is extremely simple:

```text
read
 ↓
lookup
 ↓
write
```

---

# 8.43 Python LUT Example

```python
import numpy as np

gamma = 0.5

lut = np.zeros(256, dtype=np.uint8)

for r in range(256):
    normalized = r / 255.0

    transformed = normalized ** gamma

    lut[r] = np.clip(
        transformed * 255.0,
        0,
        255
    )

output = lut[image]
```

This is a practical gamma LUT pipeline.

---

# 8.44 Seven Concepts Together

Now connect the entire chapter:

```text
             INTENSITY TRANSFORMATION
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
    Linear         Nonlinear        Piecewise
       │               │                │
       │        ┌──────┼──────┐         │
       │        ↓      ↓      ↓         │
       │       Log   Power   Gamma      │
       │                                │
       └────────────────┬───────────────┘
                        ↓
                       LUT
```

LUT is not a separate mathematical transformation family in the same sense; it is an **efficient representation/application mechanism for an intensity mapping**.

---

# 8.45 Comparison Table

| Transformation      | Main Purpose                            | Type                     |
| ------------------- | --------------------------------------- | ------------------------ |
| Linear              | Brightness/contrast/range mapping       | Linear                   |
| Contrast stretching | Expand useful range                     | Usually linear           |
| Log                 | Expand low values, compress high values | Nonlinear                |
| Power-law           | Control nonlinear curve                 | Nonlinear                |
| Gamma               | Practical power-law mapping             | Nonlinear                |
| Piecewise           | Different mapping for different ranges  | Piecewise                |
| LUT                 | Fast application of a mapping           | Implementation technique |

---

# 8.46 Transformation Curves — Mental Model

Think of every transformation as a curve:

```text
                Output
                  ↑
                  │
                  │      /
                  │    /
                  │  /
                  │/
                  └────────────────→ Input
```

### Linear

Straight line.

### Gamma

Curved line.

### Log

Rapid initial rise, then flattening.

### Piecewise

Several connected line segments.

### LUT

A discrete table representing any of these mappings.

---

# 8.47 Important Difference: Transformation vs Filter

An intensity transformation generally works independently on each pixel:

[
s(x,y)=T(r(x,y))
]

A spatial filter uses neighboring pixels:

[
s(x,y)=F(
r(x-1,y-1),\ldots,r(x,y),\ldots)
]

Therefore:

```text
Intensity Transformation
        ↓
Pixel value itself

Spatial Filtering
        ↓
Pixel + neighbors
```

You will study neighborhood processing and convolution in Chapters 11–12.

---

# 8.48 Medical Imaging Example — Display Pipeline

A professional medical viewer can conceptually use:

```text
DICOM / RAW
     ↓
Original pixel data
     ↓
Rescale / physical conversion
     ↓
Window Level / Width
     ↓
Intensity mapping
     ↓
Gamma / LUT if required
     ↓
8/16-bit display representation
     ↓
GPU texture
     ↓
Screen
```

The original data should remain available independently of the display transformation.

---

# 8.49 Example: Chest X-Ray Enhancement

Your uploaded X-ray processing pipeline demonstrates a real multi-stage intensity pipeline:

```text
Original RAW
   ↓
Source normalization
   ↓
Radiographic polarity
   ↓
Nonlinear tone mapping
   ↓
CLAHE
   ↓
Local contrast
   ↓
Bone detail
   ↓
Fine detail
   ↓
uint16 RAW
```



The nonlinear tone mapping stage is specifically tuned using a gamma-like parameter, while later local enhancement stages are separate operations. This illustrates an important principle:

> **Do not assume one intensity transformation can perform every enhancement task.**

---

# 8.50 Common Mistakes

### Mistake 1 — Forgetting normalization for gamma

If using:

[
r^\gamma
]

on 8-bit values directly, the behavior may be inappropriate.

Usually normalize:

[
r_n=\frac{r}{255}
]

then:

[
s_n=r_n^\gamma
]

and convert back.

---

### Mistake 2 — No clamping

A transformation may produce:

```text
-20
300
```

for an 8-bit output.

Always handle the output range.

---

### Mistake 3 — Modifying original medical data

Do not replace quantitative medical values just to make the image look better.

---

### Mistake 4 — Confusing gamma with arbitrary brightness

Gamma is nonlinear.

```text
Brightness:
r + C

Gamma:
r^γ
```

They behave differently.

---

### Mistake 5 — Recalculating expensive functions per pixel

For a fixed 8-bit mapping:

```text
pow()
log()
complex piecewise calculation
```

can often be precomputed into a LUT.

---

# 8.51 Chapter 8 Core Mental Model

Memorize:

```text
                  INPUT INTENSITY
                         │
                         ▼
                  Transformation
                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
        Linear        Nonlinear      Piecewise
          │              │              │
          │         ┌────┼────┐         │
          │         ↓    ↓    ↓         │
          │        Log Power Gamma      │
          │                             │
          └──────────────┬──────────────┘
                         ↓
                        LUT
                         ↓
                  OUTPUT INTENSITY
```

---

# 8.52 Key Formulas

### Linear

[
\boxed{s=ar+b}
]

### Contrast stretching

[
\boxed{
s=
\frac{r-r_{min}}
{r_{max}-r_{min}}
(s_{max}-s_{min})
+s_{min}
}
]

### Log

[
\boxed{s=c\log(1+r)}
]

### Power-law

[
\boxed{s=cr^\gamma}
]

### Gamma

For normalized data:

[
\boxed{s=r^\gamma}
]

### Piecewise

[
\boxed{
s=
\begin{cases}
T_1(r),&r<R_1\
T_2(r),&R_1\le r<R_2\
T_3(r),&r\ge R_2
\end{cases}
}
]

### LUT

[
\boxed{s=LUT[r]}
]

---

# Chapter 8 — Knowledge Check

### Linear

1. What is an intensity transformation?
2. What does (a) control in:

[
s=ar+b
]

3. What does (b) control?
4. What happens when (a>1)?
5. What happens when (0<a<1)?

### Contrast stretching

6. What is contrast stretching?
7. Why is it useful?
8. Map `[50,150]` to `[0,255]`.
9. What happens to input value 100?

### Log

10. Write the log transformation formula.
11. Why do we use (1+r)?
12. What happens to high intensities under a log transformation?
13. Why can log transformation be useful for high dynamic-range data?

### Power/Gamma

14. Write the power-law formula.
15. What is gamma correction?
16. What happens when:

[
\gamma<1
]

17. What happens when:

[
\gamma>1
]

18. Calculate:

[
s=0.25^{0.5}
]

19. Calculate:

[
s=0.5^2
]

### Piecewise

20. What is a piecewise transformation?
21. Why would you use multiple intensity regions?
22. What does the slope of each region represent?

### LUT

23. What is a lookup table?
24. Why can LUTs make intensity processing faster?
25. How can a gamma transformation be implemented using a LUT?
26. Why are LUTs particularly useful for interactive image viewers?

---

# Practical Exercise

Given this 8-bit image:

```text
10   50   100
150  200  250
30   80   120
```

## Exercise 1 — Linear

Apply:

[
s=1.5r+10
]

and clamp to `[0,255]`.

---

## Exercise 2 — Contrast Stretch

Assume:

[
r_{min}=10
]

[
r_{max}=250
]

Map the image to:

[
[0,255]
]

---

## Exercise 3 — Gamma

Normalize each pixel:

[
r_n=\frac{r}{255}
]

then apply:

[
s_n=r_n^{0.5}
]

and convert back to 0–255.

---

## Exercise 4 — Piecewise

Create this transformation:

```text
0–50
 ↓
×2

51–150
 ↓
×1.2

151–255
 ↓
×0.8
```

Then apply it to the image.

---

## Exercise 5 — LUT

Build a 256-entry LUT for:

[
s=255-r
]

Then use the LUT to invert the entire image.

---

# Medical Imaging Exercise

Suppose you are designing your CT/X-ray viewer.

You have:

```text
Original image
      ↓
Window/Level
      ↓
Gamma
      ↓
LUT
      ↓
Display
```

Explain:

1. Why should the original pixel data be preserved?
2. Why might you use a LUT after calculating the display mapping?
3. Why can an aggressive gamma curve destroy useful details?
4. Why is contrast stretching different from gamma correction?
5. Why might a piecewise transformation be useful for medical image visualization?

---

## Final Takeaway

The central idea of Chapter 8 is:

[
\boxed{
\text{Input intensity}
\rightarrow
\text{Mapping function}
\rightarrow
\text{Output intensity}
}
]

The most important transformations are:

```text
Linear
   ↓
ar + b

Contrast Stretching
   ↓
expand useful range

Log
   ↓
compress high values

Power/Gamma
   ↓
nonlinear tone control

Piecewise
   ↓
different rules for different ranges

LUT
   ↓
fast implementation of the mapping
```

And for your medical-imaging work, keep this principle:

```text
Quantitative Medical Data
          │
          ├──────────────→ Preserve
          │
          ↓
     Display Mapping
          ↓
     Window / Gamma / LUT
          ↓
        Viewer
```

This separation will become very important when we reach **DICOM rescale slope/intercept, Hounsfield Units, CT window/level, and the enterprise DICOM viewer architecture**.

**Chapter 8 complete.**

Next, strictly according to your index:

# Chapter 9 — Histograms

* Histogram concept
* Histogram calculation
* Histogram visualization
* Histogram statistics
* Histogram stretching
* Histogram equalization
* CLAHE
* Histogram matching
* Histogram specification 
