# Chapter 6 — Image Intensity

We now move to **Chapter 6 exactly according to your index**. The topics are:

1. Intensity meaning
2. Brightness
3. Contrast
4. Intensity transformations
5. Linear transformations
6. Gamma correction
7. Log transform
8. Exponential transform
9. Windowing
10. Level
11. Contrast stretching
12. Histogram-based intensity operations



---

# 6.1 What Is Image Intensity?

**Intensity** is the numerical value associated with a pixel or voxel that represents the strength of the image signal at that location.

For a grayscale image:

[
I(x,y)
]

represents the intensity at position ((x,y)).

Example:

[
I(100,200)=150
]

means the pixel at that location has intensity `150`.

Conceptually:

```text
(x, y)
  ↓
Image
  ↓
Intensity
```

For a 3D medical image:

[
I(x,y,z)
]

represents the voxel intensity.

---

# 6.2 Intensity Is Data, Not Just Brightness

For an ordinary displayed grayscale image, we often interpret:

```text
Low intensity  → dark
High intensity → bright
```

But medical images are different.

For example, a CT voxel may represent a quantitative reconstructed value. The viewer then maps that value to a display intensity.

So:

```text
Medical value
      ↓
Intensity transformation
      ↓
Display intensity
      ↓
Screen brightness
```

This distinction is fundamental for CT window/level.

---

# 6.3 Brightness

**Brightness** describes how light or dark an image appears.

Suppose we have:

```text
10 20 30
40 50 60
70 80 90
```

If we add a positive value:

[
I'(x,y)=I(x,y)+30
]

we get:

```text
40 50 60
70 80 90
100 110 120
```

The image becomes brighter.

Conversely:

[
I'(x,y)=I(x,y)-30
]

makes it darker.

So:

```text
Increase intensity
        ↓
Image appears brighter

Decrease intensity
        ↓
Image appears darker
```

---

# 6.4 Contrast

**Contrast** describes the difference between intensity levels in an image.

Consider:

```text
Image A:

100 105 110
100 105 110
100 105 110
```

The intensity range is small.

Now:

```text
Image B:

20 100 180
20 100 180
20 100 180
```

The intensity range is much larger.

Image B generally has stronger contrast.

A simple conceptual measure is:

[
Contrast \propto I_{max}-I_{min}
]

although real contrast can be defined in several different ways.

---

# 6.5 Brightness vs Contrast

This distinction is important.

### Brightness

Moves intensity values up or down.

```text
100 → 130
120 → 150
140 → 170
```

### Contrast

Expands or compresses the difference between values.

```text
Original:
100 110 120

Higher contrast:
80 110 140
```

Think:

```text
Brightness
   ↓
Where is the intensity range located?

Contrast
   ↓
How spread out is the intensity range?
```

---

# 6.6 Intensity Transformation

An intensity transformation maps an input intensity to an output intensity.

Mathematically:

[
I'(x,y)=T(I(x,y))
]

where:

* (I) = original intensity
* (T) = transformation
* (I') = transformed intensity

The important point is:

> The transformation operates on the **intensity value**, not necessarily on the pixel's spatial position.

---

# 6.7 Basic Intensity Pipeline

```text
Original Pixel
     ↓
Input intensity
     ↓
Transformation T()
     ↓
Output intensity
     ↓
Display / Processing
```

For example:

[
T(x)=x+20
]

means every intensity is increased by 20.

---

# 6.8 Linear Transformations

A basic linear intensity transformation is:

[
I'=aI+b
]

where:

* (a) controls scaling/contrast
* (b) controls shifting/brightness

This is one of the most useful equations in image processing.

---

# 6.9 Effect of (b)

Consider:

[
I'=I+b
]

If:

[
b=50
]

then:

```text
Original → Output

20 → 70
50 → 100
100 → 150
150 → 200
```

The intensity values shift upward.

Therefore:

> (b) primarily changes brightness.

---

# 6.10 Effect of (a)

Now:

[
I'=aI
]

Suppose:

[
a=2
]

Then:

```text
20  → 40
50  → 100
100 → 200
```

The differences between values become larger.

Therefore:

> (a) primarily controls contrast.

---

# 6.11 Combined Transformation

Using:

[
I'=aI+b
]

suppose:

[
a=2,\quad b=10
]

and:

[
I=50
]

Then:

[
I'=2(50)+10
]

[
=110
]

So:

```text
50
 ↓
×2
 ↓
100
 ↓
+10
 ↓
110
```

---

# 6.12 What Happens Beyond the Data Range?

Suppose an 8-bit image has:

[
0\le I\le255
]

and:

[
I'=2I
]

For:

[
I=200
]

we get:

[
I'=400
]

But `UINT8` cannot represent 400.

Therefore we need a strategy such as:

### Clipping

[
I'=255
]

or:

### Conversion to a larger data type

such as `UINT16` or `FLOAT32`.

This is an important practical issue in intensity processing.

---

# 6.13 Gamma Correction

Gamma transformation is a nonlinear intensity transformation.

A common form is:

[
I'=cI^\gamma
]

where:

* (c) = scaling constant
* (\gamma) = gamma value

For normalized intensity:

[
0\le I\le1
]

we can study the effect of (\gamma).

---

# 6.14 Gamma Less Than 1

If:

[
0<\gamma<1
]

darker values are expanded relative to brighter values.

For example:

[
I'=I^{0.5}
]

which is equivalent to:

[
\sqrt I
]

For:

[
I=0.25
]

we get:

[
I'=0.5
]

So:

```text
0.25
 ↓
0.5
```

The darker region becomes relatively brighter.

---

# 6.15 Gamma Greater Than 1

If:

[
\gamma>1
]

lower intensities are compressed.

For example:

[
I'=I^2
]

If:

[
I=0.5
]

then:

[
I'=0.25
]

So:

```text
0.5
 ↓
0.25
```

The image becomes relatively darker.

---

# 6.16 Gamma Summary

```text
γ < 1
  ↓
Brightens darker regions

γ = 1
  ↓
No nonlinear change

γ > 1
  ↓
Darkens/compresses lower intensities
```

Gamma correction is useful when a simple linear transformation is not enough.

---

# 6.17 Log Transform

The logarithmic transformation is commonly written:

[
I'=c\log(1+I)
]

The `+1` prevents:

[
\log(0)
]

from occurring.

The log function expands lower values and compresses higher values.

Conceptually:

```text
Low intensities
      ↓
Expanded

High intensities
      ↓
Compressed
```

---

# 6.18 Why Use a Log Transform?

A log transform can be useful when an image contains a very large dynamic range.

For example:

```text
Original:
1
10
100
1000
10000
```

These values span several orders of magnitude.

A logarithmic transformation compresses this range.

This can make certain structures easier to visualize.

---

# 6.19 Exponential Transform

An exponential transformation has a form such as:

[
I'=c(e^{aI}-1)
]

depending on the desired normalization.

The exponential function behaves approximately opposite to the logarithm:

```text
Log:
large values → compressed

Exponential:
large values → expanded
```

It can therefore be used when we want a nonlinear mapping that emphasizes certain portions of the intensity range.

The exact form and normalization matter in practical implementation.

---

# 6.20 Linear vs Nonlinear Transformations

### Linear

[
I'=aI+b
]

Example:

```text
brightness
contrast
```

### Nonlinear

Examples:

[
I'=I^\gamma
]

[
I'=c\log(1+I)
]

[
I'=c(e^{aI}-1)
]

Conceptually:

```text
Intensity transformations
        │
        ├── Linear
        │    └── aI+b
        │
        └── Nonlinear
             ├── Gamma
             ├── Log
             └── Exponential
```

---

# 6.21 Windowing

Now we reach one of the **most important concepts for medical imaging**.

Windowing is especially important for CT visualization.

A CT image may contain a much wider intensity range than a monitor can display effectively.

We therefore select a useful intensity range.

This is called a **window**.

---

# 6.22 Window Level and Width

Two common parameters are:

* **Window Level (WL)**
* **Window Width (WW)**

### Window Level

The center of the displayed intensity range.

### Window Width

The size of the displayed intensity range.

Conceptually:

[
Lower = WL-\frac{WW}{2}
]

[
Upper = WL+\frac{WW}{2}
]

Therefore:

```text
                Window
        ┌──────────────────┐
        │                  │
--------┴------------------┴--------
      Lower              Upper
                 ↑
                 WL
```

---

# 6.23 Example of Windowing

Suppose:

[
WL=40
]

and:

[
WW=400
]

Then:

[
Lower=40-\frac{400}{2}
]

[
=40-200
]

[
=-160
]

and:

[
Upper=40+200
]

[
=240
]

So the display window is:

[
[-160,240]
]

---

# 6.24 What Happens Outside the Window?

Suppose the window is:

[
[-160,240]
]

Then values:

```text
< -160
```

are typically mapped to the darkest display value.

Values:

```text
> 240
```

are mapped to the brightest display value.

Values inside the window are mapped proportionally.

Conceptually:

```text
CT value

< -160      -160 ───────── 240      > 240
   ↓           ↓             ↓          ↓
 black       mapping       mapping    white
```

---

# 6.25 Windowing Formula

For a grayscale display with output range:

[
[0,255]
]

a simplified windowing operation is:

[
I_{display}
===========

255
\frac{I-L}{U-L}
]

where:

* (L) = lower window bound
* (U) = upper window bound

Then clamp:

[
I_{display}<0 \Rightarrow 0
]

[
I_{display}>255 \Rightarrow 255
]

---

# 6.26 Windowing Example

Suppose:

[
L=-160
]

[
U=240
]

and CT value:

[
I=40
]

Then:

[
I_{display}
===========

255
\frac{40-(-160)}
{240-(-160)}
]

# [

255\frac{200}{400}
]

[
=127.5
]

So approximately:

[
128
]

on the display.

---

# 6.27 Why Windowing Is Essential in CT

Suppose the CT contains:

```text
air
fat
soft tissue
bone
metal
```

Their values can occupy very different ranges.

If we display the entire range simultaneously:

```text
Huge CT range
       ↓
Poor visualization of specific tissues
```

Instead, we select a useful window.

For example, conceptually:

```text
Soft tissue window
      ↓
emphasize soft tissue

Bone window
      ↓
emphasize bone

Lung window
      ↓
emphasize lung structures
```

The underlying CT data hasn't changed.

Only the **display mapping** changes.

This distinction is extremely important.

---

# 6.28 Windowing Does NOT Change the Original CT

Suppose:

[
CT(x,y)=100
]

If we change the window:

```text
Window A
 ↓
display value = 150

Window B
 ↓
display value = 80
```

The original CT value is still:

[
100
]

So:

```text
Original medical data
        ↓
       SAME
        ↓
Different display windows
        ↓
Different visual appearance
```

This is why a radiologist can change window/level interactively without changing the underlying scan.

---

# 6.29 Level

The **level** is essentially the center of the intensity window.

For example:

[
WL=40
]

means the window is centered around 40.

If:

[
WW=400
]

then:

[
[-160,240]
]

is displayed.

Changing the level shifts the window:

```text
       Window A
----[===========]----

             Window B
--------[===========]--
```

---

# 6.30 Effect of Window Level

Increasing WL:

```text
Window
    ↓
moves toward higher intensities
```

Decreasing WL:

```text
Window
    ↓
moves toward lower intensities
```

So:

> **Window Level controls the center of the displayed intensity range.**

---

# 6.31 Effect of Window Width

Increasing WW:

```text
wider intensity range
      ↓
more values displayed
      ↓
lower contrast
```

Decreasing WW:

```text
narrower intensity range
      ↓
fewer values displayed
      ↓
higher contrast
```

Therefore:

```text
Window Width ↓
     ↓
Contrast ↑

Window Width ↑
     ↓
Contrast ↓
```

This is one of the most important practical relationships in CT viewing.

---

# 6.32 Window Level vs Width

| Parameter    | Main Effect                       |
| ------------ | --------------------------------- |
| Window Level | Shifts the center                 |
| Window Width | Controls displayed range/contrast |

Think:

```text
Level
  ↓
WHERE is the window?

Width
  ↓
HOW WIDE is the window?
```

---

# 6.33 Contrast Stretching

Contrast stretching expands a narrow intensity range to a larger output range.

Suppose the input image contains:

[
50\ldots100
]

but we want:

[
0\ldots255
]

We can map:

[
I'=255
\frac{I-50}{100-50}
]

For:

[
I=50
]

we get:

[
I'=0
]

For:

[
I=100
]

we get:

[
I'=255
]

---

# 6.34 Contrast Stretching Example

Suppose:

[
I=75
]

Then:

[
I'=255
\frac{75-50}{50}
]

[
=255\times0.5
]

[
=127.5
]

approximately:

[
128
]

So:

```text
50 → 0
75 → 128
100 → 255
```

The image's intensity differences are expanded.

---

# 6.35 Why Contrast Stretching Helps

Suppose an image uses only a small portion of the available intensity range:

```text
0 ─────────────────────────── 255
        █████████
        actual data
```

The image may appear low contrast.

Contrast stretching expands it:

```text
0 ─────────────────────────── 255
█████████████████████████████
```

This can make structures easier to distinguish visually.

---

# 6.36 Histogram

A histogram counts how frequently intensity values occur.

For an 8-bit grayscale image:

```text id="hist1"
Intensity →
0 1 2 3 ... 255

Count
 ↑
 │       ███
 │   ███ ████
 │ ███████████
 └────────────────→ intensity
```

Mathematically, a histogram can be represented as:

[
H(k)
]

where (H(k)) is the number of pixels having intensity (k).

---

# 6.37 Simple Histogram Example

Suppose:

```text id="hist2"
10, 10, 10,
20, 20,
30
```

Then:

[
H(10)=3
]

[
H(20)=2
]

[
H(30)=1
]

All other values have:

[
H(k)=0
]

---

# 6.38 Why Histograms Matter

A histogram tells us about the distribution of intensity values.

It can help identify:

* low contrast
* high contrast
* dark images
* bright images
* multiple intensity populations

For example:

```text
Histogram concentrated on left
       ↓
mostly dark values
```

while:

```text
Histogram concentrated on right
       ↓
mostly bright values
```

---

# 6.39 Histogram-Based Intensity Operations

Histograms can be used for operations such as:

* histogram equalization
* histogram matching
* intensity analysis

These techniques will become more important in later chapters, but the basic idea belongs here.

---

# 6.40 Histogram Equalization — Basic Idea

Suppose most pixel values occupy a narrow region:

```text
Intensity
     ↓
████████
```

Histogram equalization attempts to redistribute intensities to improve the use of the available range.

Conceptually:

```text
Narrow distribution
       ↓
Histogram transformation
       ↓
Broader intensity distribution
       ↓
Potentially improved contrast
```

The exact mathematical derivation will be covered when histogram processing is studied in greater depth.

---

# 6.41 Histogram Matching

Histogram matching attempts to transform one image's intensity distribution to resemble a target distribution.

Conceptually:

```text
Image A
  ↓
Histogram A

Target Image
  ↓
Histogram B

       ↓
Transformation

Image A'
  ↓
Histogram approximately like B
```

This can be useful when trying to normalize intensity characteristics across images.

---

# 6.42 Important: Histogram Is Not Spatial Information

A histogram tells us:

> **How many pixels have each intensity.**

It does **not** tell us:

> **Where those pixels are.**

For example:

```text
Image A:

100 100
200 200
```

and:

```text
Image B:

100 200
100 200
```

can have the same histogram.

But their spatial arrangements are different.

Therefore:

```text
Histogram
  ↓
Intensity distribution

Image
  ↓
Intensity + spatial arrangement
```

This distinction is fundamental.

---

# 6.43 Complete Intensity Transformation Map

We now have:

```text
                  INTENSITY
                      │
       ┌──────────────┼───────────────┐
       ↓              ↓               ↓
   Brightness      Contrast        Histogram
       │              │               │
       ↓              ↓               ↓
     + / -          × / stretch    Distribution
                      │
                      ↓
             Intensity transforms
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
     Linear         Gamma          Log/Exp
       │
       ↓
     Windowing
       │
       ├── Level
       └── Width
```

---

# 6.44 Medical Imaging Pipeline

For CT visualization:

```text
CT Voxel Value
      ↓
Window / Level
      ↓
Intensity Mapping
      ↓
0–255 Display Range
      ↓
Monitor
```

This is one of the most important pipelines in a CT viewer.

---

# 6.45 C++ Example — Linear Transformation

```cpp
#include <iostream>
#include <algorithm>

int main()
{
    double intensity = 100.0;

    double a = 1.5;
    double b = 20.0;

    double output = a * intensity + b;

    output = std::clamp(output, 0.0, 255.0);

    std::cout << output << '\n';

    return 0;
}
```

The transformation is:

[
I'=1.5I+20
]

The `clamp` protects the 8-bit display range.

---

# 6.46 C++ Example — Windowing

```cpp
#include <iostream>
#include <algorithm>

int main()
{
    double value = 40.0;

    double level = 40.0;
    double width = 400.0;

    double lower = level - width / 2.0;
    double upper = level + width / 2.0;

    double display =
        255.0 * (value - lower) / (upper - lower);

    display = std::clamp(display, 0.0, 255.0);

    std::cout << "Display value = "
              << display << '\n';

    return 0;
}
```

For:

[
WL=40
]

and:

[
WW=400
]

a CT value of 40 maps approximately to:

[
128
]

---

# 6.47 Python Example

```python
import numpy as np

image = np.array(
    [[10, 20, 30],
     [40, 50, 60],
     [70, 80, 90]],
    dtype=np.float32
)

a = 1.5
b = 20.0

output = a * image + b

output = np.clip(output, 0, 255)

print(output)
```

This applies:

[
I'=aI+b
]

to every pixel.

---

# 6.48 A Critical Medical-Software Principle

In a medical imaging viewer, distinguish between:

### Original image data

```text
CT voxel value
```

and:

### Display representation

```text
window/level transformed value
```

and:

### Screen pixel

```text
monitor RGB value
```

These are three different concepts:

```text
CT Value
   ↓
Display Transformation
   ↓
Display Intensity
   ↓
RGB/Screen
```

Confusing them can lead to incorrect image processing and incorrect measurements.

---

# 6.49 Chapter 6 Summary

You should now understand:

### Intensity

[
I(x,y)
]

represents the image value.

### Brightness

Controlled conceptually by shifting intensity:

[
I'=I+b
]

### Contrast

Controlled conceptually by scaling intensity:

[
I'=aI
]

### Linear transformation

[
I'=aI+b
]

### Gamma

[
I'=cI^\gamma
]

### Log

[
I'=c\log(1+I)
]

### Exponential

[
I'=c(e^{aI}-1)
]

### Windowing

[
L=WL-\frac{WW}{2}
]

[
U=WL+\frac{WW}{2}
]

### Contrast stretching

Maps:

[
[I_{min},I_{max}]
\rightarrow
[O_{min},O_{max}]
]

### Histogram

[
H(k)
]

counts how many pixels have intensity (k).

---

# Chapter 6 — Knowledge Check

Answer these before Chapter 7.

### Fundamentals

1. What is image intensity?
2. Why is medical image intensity not always the same as screen brightness?
3. What is brightness?
4. What is contrast?
5. What is an intensity transformation?

### Transformations

6. Explain:

[
I'=aI+b
]

7. What does (a) primarily control?
8. What does (b) primarily control?
9. What happens when (a>1)?
10. What happens when (0<a<1)?

### Nonlinear

11. What is gamma correction?
12. What happens when (\gamma<1)?
13. What happens when (\gamma>1)?
14. What is a log transform?
15. Why does a log transform compress large values?
16. What is an exponential transformation?

### CT Windowing

17. What is Window Level?
18. What is Window Width?
19. Calculate the lower and upper limits for:

[
WL=50,\quad WW=400
]

20. What happens to values below the lower window?
21. What happens to values above the upper window?
22. Why does reducing WW generally increase displayed contrast?
23. Why does changing WL change which tissue range is emphasized?
24. Does changing window/level modify the original CT voxel values?

### Histogram

25. What is an image histogram?
26. What information does a histogram contain?
27. What information does a histogram NOT contain?
28. What is histogram equalization?
29. What is histogram matching?

---

# Practical Exercise

Given:

[
I=
\begin{bmatrix}
10&50&100\
150&200&250\
30&80&120
\end{bmatrix}
]

### Task 1 — Brightness

Apply:

[
I'=I+20
]

### Task 2 — Contrast

Apply:

[
I'=1.5I
]

Then clip to:

[
[0,255]
]

### Task 3 — Linear transformation

Apply:

[
I'=1.2I+10
]

### Task 4 — CT windowing

Use:

[
WL=50
]

[
WW=100
]

Calculate:

[
L=WL-\frac{WW}{2}
]

[
U=WL+\frac{WW}{2}
]

Then determine the display value of:

[
I=75
]

using a 0–255 display range.

### Task 5 — Explain

In your own words, explain the difference between:

```text
Windowing
Contrast stretching
Gamma correction
Histogram equalization
```

---

## Important Medical-Imaging Concept

Remember this chain:

```text
              CT DATA
                 │
                 │  Original voxel values
                 ↓
          Window / Level
                 │
                 ↓
       Intensity Transformation
                 │
                 ↓
          Display 0–255
                 │
                 ↓
              SCREEN
```

**Window/level changes the visualization, not the underlying CT data.**

---

**Chapter 6 is complete.**

The next chapter in your exact index is:

# Chapter 7 — Image Histogram

Topics:

* Histogram definition
* Histogram computation
* Histogram interpretation
* Histogram equalization
* Histogram matching
* CDF
* PDF
* Histogram stretching
* Adaptive histogram equalization
* CLAHE.
