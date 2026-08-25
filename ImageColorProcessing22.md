# Chapter 22 — Image Color Processing

We continue **strictly according to your index**.

This chapter is especially important for your **medical image viewer/editor**, because medical imaging frequently uses color not because the source image is naturally RGB, but to communicate quantitative information such as:

* tissue intensity
* CT window/level
* PET uptake
* dose
* segmentation labels
* overlays
* heatmaps
* measurement maps

---

# 22.1 Color Image Fundamentals

A color image represents each pixel using multiple components.

For a normal RGB image:

[
\boxed{
Pixel=(R,G,B)
}
]

For example:

```text
Pixel
  │
  ├── R = 255
  ├── G = 100
  └── B = 50
```

Unlike grayscale:

[
\boxed{
Pixel=I
}
]

color images contain multiple channels.

---

# 22.2 RGB

RGB means:

[
\boxed{
Red + Green + Blue
}
]

It is an **additive color model**.

Each channel contributes light.

For an 8-bit RGB image:

```text
R ∈ [0,255]
G ∈ [0,255]
B ∈ [0,255]
```

Therefore one pixel requires:

[
3\times8=24\text{ bits}
]

or:

[
\boxed{
24\text{-bit color}
}
]

---

# 22.3 RGB Examples

### Black

```text
R=0
G=0
B=0
```

[
\boxed{(0,0,0)}
]

### White

```text
R=255
G=255
B=255
```

[
\boxed{(255,255,255)}
]

### Red

[
\boxed{(255,0,0)}
]

### Green

[
\boxed{(0,255,0)}
]

### Blue

[
\boxed{(0,0,255)}
]

---

# 22.4 Additive Color Mixing

RGB combines light.

```text
Red + Green
      ↓
Yellow
```

```text
Green + Blue
      ↓
Cyan
```

```text
Red + Blue
      ↓
Magenta
```

```text
Red + Green + Blue
      ↓
White
```

Therefore:

[
\boxed{
RGB = additive
}
]

---

# 22.5 RGB in Computer Vision

RGB is convenient for:

* displaying images
* camera images
* photographs
* general computer vision

But it is not always convenient for:

* segmentation
* color-based classification
* perceptual analysis

because brightness and chromatic information are mixed across the three channels.

---

# 22.6 RGB Cube

Conceptually, RGB forms a 3D coordinate space:

```text
             B
             ↑
             │
             │
             └────────→ G
            /
           /
          R
```

Every point represents one color.

For example:

[
(255,0,0)
]

is red.

---

# 22.7 HSV

HSV means:

[
\boxed{
Hue + Saturation + Value
}
]

It represents color in a more intuitive way.

```text
HSV
 │
 ├── H → what color?
 ├── S → how saturated?
 └── V → how bright?
```

---

# 22.8 Hue

Hue represents the basic color.

Conceptually:

```text
Red
 ↓
Yellow
 ↓
Green
 ↓
Cyan
 ↓
Blue
 ↓
Magenta
 ↓
Red
```

Hue is typically represented as an angle:

[
\boxed{
H\in[0^\circ,360^\circ)
}
]

depending on the implementation.

---

# 22.9 Saturation

Saturation describes how pure/intense the color is.

High saturation:

```text
Strong pure color
```

Low saturation:

```text
Grayish / washed-out color
```

Conceptually:

```text
Saturation ↑
     ↓
More colorful
```

---

# 22.10 Value

Value represents brightness in HSV.

[
\boxed{
V=\max(R,G,B)
}
]

for normalized RGB channels.

For 8-bit channels:

```text
V = maximum of R,G,B
```

---

# 22.11 HSV Example

Suppose:

```text
RGB = (255,0,0)
```

Then approximately:

```text
H = 0°
S = 100%
V = 100%
```

For:

```text
RGB = (128,128,128)
```

we have:

```text
S ≈ 0
```

because there is no chromatic dominance.

---

# 22.12 Why HSV Is Useful

Suppose you want to segment a red object.

In RGB:

```text
R high
G low
B low
```

You can use those conditions.

But HSV allows:

```text
Hue → red
Saturation → sufficiently high
Value → sufficiently bright
```

which can be more intuitive.

---

# 22.13 HSL

HSL means:

[
\boxed{
Hue + Saturation + Lightness
}
]

It differs from HSV mainly in how the brightness/lightness dimension is defined.

```text
HSV
Value

HSL
Lightness
```

These are **not interchangeable**.

---

# 22.14 HSV vs HSL

| HSV                                 | HSL                                    |
| ----------------------------------- | -------------------------------------- |
| Hue                                 | Hue                                    |
| Saturation                          | Saturation                             |
| Value                               | Lightness                              |
| Often useful for color segmentation | Often useful for color selection/UI    |
| Brightness based on maximum channel | Lightness based on min/max combination |

---

# 22.15 YUV

YUV separates:

```text
Y → Luma
U → Chrominance
V → Chrominance
```

Conceptually:

[
\boxed{
Y + chroma
}
]

The important idea is:

```text
Brightness information
+
Color information
```

are separated.

YUV terminology is historically associated with analog/video systems, while related digital representations are commonly described using YCbCr.

---

# 22.16 Why Separate Luma and Chroma?

Human vision is generally more sensitive to spatial brightness detail than to fine chroma detail.

Therefore systems can process:

```text
Y
```

at higher spatial detail while reducing chroma information.

This is important in:

* video compression
* image transmission
* color processing

---

# 22.17 YCbCr

YCbCr is widely used in digital imaging/video.

Conceptually:

```text
Y
 ↓
Luma

Cb
 ↓
Blue-difference chroma

Cr
 ↓
Red-difference chroma
```

Therefore:

[
\boxed{
YCbCr = luminance/chroma\ representation
}
]

---

# 22.18 YCbCr in Imaging

It is commonly encountered in:

* JPEG
* video
* image codecs
* digital cameras

The exact numerical conversion depends on the color standard and range, so do not assume one universal RGB↔YCbCr equation without specifying the standard.

---

# 22.19 Lab

CIE Lab is a perceptually oriented color space.

Channels:

```text
L*
 ↓
Lightness

a*
 ↓
Green ↔ Red

b*
 ↓
Blue ↔ Yellow
```

So:

[
\boxed{
Lab=(L^*,a^*,b^*)
}
]

---

# 22.20 Why Lab Is Useful

Lab attempts to represent colors in a way that is more perceptually meaningful than raw RGB coordinates.

It is useful for:

* color difference
* color clustering
* segmentation
* color correction

---

# 22.21 Color Difference

A basic color-distance concept in Lab is:

[
\boxed{
\Delta E
}
]

which attempts to quantify perceptual color difference.

A simple Euclidean form is:

[
\Delta E_{ab}
=============

\sqrt{
(L_1-L_2)^2+
(a_1-a_2)^2+
(b_1-b_2)^2
}
]

More sophisticated ΔE standards also exist.

---

# 22.22 Color-Space Comparison

| Color Space | Main Idea                    | Common Use         |
| ----------- | ---------------------------- | ------------------ |
| RGB         | Additive channels            | Display            |
| HSV         | Hue + saturation + value     | Segmentation/UI    |
| HSL         | Hue + saturation + lightness | UI/color selection |
| YUV         | Luma + chroma                | Video              |
| YCbCr       | Digital luma + chroma        | JPEG/video         |
| Lab         | Perceptual representation    | Color analysis     |

---

# 22.23 Color Conversion

Conversion means transforming:

```text
RGB
 ↓
HSV
```

or:

```text
RGB
 ↓
Lab
```

The image data represents the same underlying color information, but coordinates in the color space change.

---

# 22.24 RGB → Grayscale

A simple average:

[
I=\frac{R+G+B}{3}
]

is possible, but generally does not match human luminance perception well.

A common luminance approximation is:

[
\boxed{
Y\approx0.299R+0.587G+0.114B
}
]

The exact coefficients depend on the color standard.

---

# 22.25 Why Green Gets More Weight

Human visual sensitivity is not equal across RGB channels.

Typically:

[
G > R > B
]

in terms of contribution to perceived luminance in common luma formulas.

Therefore:

```text
Green
 ↓
strong contribution

Blue
 ↓
smaller contribution
```

---

# 22.26 Color Enhancement

Color enhancement modifies an image to improve:

* visibility
* contrast
* interpretability
* visualization

Methods include:

```text
Color Enhancement
 │
 ├── Contrast adjustment
 ├── Saturation adjustment
 ├── Histogram processing
 ├── Color balancing
 └── Pseudocolor
```

---

# 22.27 Color Histogram

A histogram counts how frequently color values occur.

For grayscale:

```text
Intensity
   ↓
Histogram
```

For RGB:

```text
R histogram
G histogram
B histogram
```

You can maintain separate channel histograms.

---

# 22.28 RGB Histograms

Example:

```text
R → ███████
G → █████
B → ██
```

This tells you how channel intensities are distributed.

But separate channel histograms do not capture the full joint distribution of colors.

---

# 22.29 Color Histogram in HSV

You can analyze:

```text
Hue histogram
Saturation histogram
Value histogram
```

This can be more useful for certain applications.

For example:

```text
Hue histogram
 ↓
dominant colors
```

---

# 22.30 Color Segmentation

Color segmentation separates pixels according to color properties.

Pipeline:

```text
Image
 ↓
Convert RGB → HSV
 ↓
Define color range
 ↓
Threshold
 ↓
Mask
```

Example:

```text
Hue ∈ red range
AND
Saturation > threshold
```

---

# 22.31 Color Thresholding

General idea:

[
Mask(x,y)=
\begin{cases}
1,&color\ satisfies\ condition\
0,&otherwise
\end{cases}
]

Example:

```text
H ∈ [0,10]
S > 100
V > 80
```

The exact thresholds depend on the image and application.

---

# 22.32 Color Segmentation Example

```text
RGB Image
     ↓
HSV conversion
     ↓
Red threshold
     ↓
Binary mask
```

Output:

```text
██████
██  ██
██  ██
██████
```

---

# 22.33 Pseudocolor Processing

Medical grayscale images are often displayed using color even though the original data is grayscale.

This is called:

[
\boxed{
Pseudocolor
}
]

Example:

```text
Grayscale intensity
        ↓
Color LUT
        ↓
Blue → Green → Yellow → Red
```

---

# 22.34 Why Use Pseudocolor?

Human vision can distinguish many colors more easily than subtle grayscale changes in some visualization contexts.

Therefore pseudocolor can improve visual interpretation of:

* heatmaps
* dose distributions
* perfusion maps
* PET uptake
* parameter maps

---

# 22.35 Pseudocolor Is Not New Image Data

This is very important.

Suppose:

[
I=100
]

and the LUT maps it to:

```text
Yellow
```

The underlying measurement is still:

[
100
]

The yellow color is only a visualization mapping.

Therefore:

[
\boxed{
Pseudocolor \neq changing\ the\ physical\ measurement
}
]

---

# 22.36 LUT

LUT means:

[
\boxed{
Look-Up\ Table
}
]

It maps an input value to an output value.

For grayscale-to-color:

```text
Input intensity
      ↓
     LUT
      ↓
RGB color
```

Example:

```text
0   → Blue
50  → Cyan
100 → Green
150 → Yellow
200 → Orange
255 → Red
```

---

# 22.37 Medical LUT

A medical viewer can provide different color maps:

```text
LUT
 ├── Grayscale
 ├── Hot
 ├── Cool
 ├── Rainbow
 ├── PET
 ├── Dose
 └── Custom
```

The exact clinical appropriateness depends on the application.

---

# 22.38 Window/Level + LUT

For CT, a common pipeline is:

```text
Raw HU
 ↓
Window/Level
 ↓
Normalized display intensity
 ↓
LUT
 ↓
RGB
```

This is an important connection between your earlier chapters and this chapter.

---

# 22.39 Window/Level

Let:

[
W=\text{window width}
]

[
L=\text{window level}
]

Then:

[
Low=L-\frac W2
]

[
High=L+\frac W2
]

Values can then be mapped into the display range.

---

# 22.40 Window/Level Example

Suppose:

[
W=400
]

[
L=40
]

Then:

[
Low=40-200=-160
]

[
High=40+200=240
]

Therefore approximately:

```text
HU < -160
   ↓
display minimum

-160 to 240
   ↓
mapped to display range

HU > 240
   ↓
display maximum
```

---

# 22.41 Window/Level + Pseudocolor

For a colorized display:

```text
HU
 ↓
Window/Level
 ↓
0–255
 ↓
LUT
 ↓
RGB
```

This architecture is ideal for your medical viewer.

---

# 22.42 False Color

False color means assigning colors to data that isn't inherently represented as natural color.

Examples:

```text
CT intensity → color
Dose → color
Temperature → color
Perfusion → color
Probability → color
```

This is closely related to pseudocolor, although terminology can vary by application.

---

# 22.43 Medical Image Overlay

Suppose:

```text
CT
+
Tumor mask
```

We can overlay:

```text
CT → grayscale
Tumor → colored
```

Conceptually:

```text
CT
████████████

Tumor
    ███
```

Combined:

```text
CT + colored tumor
```

---

# 22.44 Alpha Blending

A common overlay formula:

[
\boxed{
C=
\alpha C_{overlay}
+
(1-\alpha)C_{base}
}
]

where:

[
0\leq\alpha\leq1
]

---

# 22.45 Alpha Example

If:

[
\alpha=0.5
]

then:

[
C=
0.5C_{overlay}
+
0.5C_{base}
]

So both images contribute equally.

---

# 22.46 Alpha Values

```text
α = 0
 ↓
Overlay invisible

α = 0.25
 ↓
Mostly base image

α = 0.5
 ↓
Balanced overlay

α = 1
 ↓
Overlay completely opaque
```

---

# 22.47 Medical Overlay Architecture

For your viewer:

```text
Base Image
    │
    ↓
Window/Level
    │
    ↓
Grayscale RGB
    │
    ├───────────────┐
    │               │
    │          Segmentation
    │               ↓
    │            LUT
    │               ↓
    │           Overlay RGB
    │               │
    └───────┬───────┘
            ↓
       Alpha Blend
            ↓
      Final Display
```

---

# 22.48 Multiple Overlays

An enterprise viewer may have:

```text
CT
 │
 ├── Tumor
 ├── Organ
 ├── Dose
 ├── Contours
 ├── Measurements
 └── AI result
```

Each can have:

* color
* opacity
* visibility
* priority

---

# 22.49 Overlay Compositing

For multiple layers:

```text
Base
 ↓
Layer 1
 ↓
Layer 2
 ↓
Layer 3
 ↓
Final
```

The compositing order matters.

For example:

```text
CT
 ↓
Dose
 ↓
Contour
 ↓
Annotation
```

may produce a different result from:

```text
CT
 ↓
Contour
 ↓
Dose
 ↓
Annotation
```

---

# 22.50 Segmentation Color Mapping

Suppose:

```text
Label 0 = background
Label 1 = liver
Label 2 = tumor
Label 3 = kidney
```

A label LUT might be:

```text
0 → transparent
1 → green
2 → red
3 → blue
```

Then:

```text
Label image
 ↓
Label LUT
 ↓
RGBA overlay
```

---

# 22.51 Why Labels Need a LUT

A segmentation mask is categorical:

```text
0
1
2
3
```

These numbers are not naturally colors.

The LUT provides:

[
\boxed{
Label \rightarrow RGBA
}
]

---

# 22.52 DICOM Palette Color Images

DICOM can contain palette color images.

Instead of storing direct RGB values for every pixel, a pixel value can index color lookup tables.

Conceptually:

```text
Pixel value
     ↓
Red LUT
Green LUT
Blue LUT
     ↓
RGB
```

DICOM provides specific attributes for these palette color lookup tables.

---

# 22.53 Palette LUT Architecture

```text
Pixel value
      │
      ├──→ Red LUT
      │
      ├──→ Green LUT
      │
      └──→ Blue LUT
             │
             ↓
           RGB
```

This is especially relevant when implementing a DICOM viewer.

---

# 22.54 DICOM Color Handling

Your DICOM loader should distinguish between different photometric interpretations and image representations.

Conceptually:

```text
DICOM
 ↓
Photometric Interpretation
 ↓
Decode pixels correctly
 ↓
Color conversion if necessary
 ↓
Display
```

Examples encountered in DICOM include:

```text
MONOCHROME1
MONOCHROME2
RGB
YBR_FULL
PALETTE COLOR
```

The exact decoding rules must follow the DICOM specification and the transfer syntax.

---

# 22.55 MONOCHROME1 vs MONOCHROME2

This is important for medical viewers.

The grayscale polarity can differ.

Conceptually:

```text
MONOCHROME2
low value → dark
high value → bright
```

while:

```text
MONOCHROME1
low value → bright
high value → dark
```

Therefore your DICOM pipeline cannot assume every grayscale image has the same polarity.

---

# 22.56 RGB vs Medical Pseudocolor

These are completely different concepts.

### Native RGB

```text
DICOM pixel
 ↓
RGB
```

The source image contains color information.

### Pseudocolor

```text
Grayscale measurement
 ↓
LUT
 ↓
RGB display
```

The source measurement may still be grayscale.

---

# 22.57 Color Management

An enterprise viewer should distinguish:

```text
Source data
      ↓
Processing
      ↓
Display representation
```

Do not overwrite original medical measurement values simply because the UI uses a color map.

This separation is critical:

[
\boxed{
Raw/measurement\ data
\neq
Display\ color
}
]

---

# 22.58 Medical Color Pipeline

A robust architecture:

```text
DICOM
 ↓
Pixel Decoder
 ↓
Physical/Stored Value
 ↓
Modality LUT / VOI processing
 ↓
Display normalization
 ↓
Color LUT
 ↓
RGBA
 ↓
GPU / QML rendering
```

For CT, this might involve:

```text
Stored values
 ↓
Rescale slope/intercept
 ↓
HU
 ↓
Window/Level
 ↓
LUT
 ↓
RGBA
```

---

# 22.59 Why Keep Raw Data?

Suppose the user selects:

```text
Lung window
```

then later:

```text
Bone window
```

You should not repeatedly transform an already colorized image.

Instead:

```text
Original data
    ↓
New W/L
    ↓
New display
```

This avoids cumulative display-processing errors.

---

# 22.60 GPU Color Mapping

For a high-performance viewer:

```text
CPU
 ↓
Image data
 ↓
GPU texture
 ↓
Shader
 ↓
Window/Level
 ↓
LUT
 ↓
RGBA
```

This can make interactive operations much faster.

For example:

```text
Mouse drag
 ↓
Change window/level
 ↓
Shader parameters update
 ↓
Immediate display
```

without recomputing the underlying image.

---

# 22.61 Qt/QML Architecture

For your Qt medical viewer:

```text
QML
 │
 ├── Window Level
 ├── LUT
 ├── Overlay Opacity
 ├── Color Map
 └── Invert
 │
 ↓
ImageController
 ↓
DisplayPipeline
 ↓
ImageProcessor
 ↓
RGBA / GPU Texture
 ↓
QML Image / Scene Graph
```

---

# 22.62 Color Map Object

A clean C++ representation:

```cpp
struct ColorPoint
{
    double position;
    float r;
    float g;
    float b;
    float a;
};
```

Then:

```cpp
class ColorMap
{
public:
    ColorMap();
    Color rgba(double normalizedValue) const;
};
```

This allows custom LUTs.

---

# 22.63 Medical LUT Interface

```cpp
class IColorMap
{
public:
    virtual ~IColorMap() = default;

    virtual Color map(double value) const = 0;
};
```

Implementations:

```text
GrayColorMap
HotColorMap
DoseColorMap
PetColorMap
CustomColorMap
```

---

# 22.64 Display Pipeline Interface

```cpp
class IDisplayMapper
{
public:
    virtual ~IDisplayMapper() = default;

    virtual Color map(
        double value,
        double window,
        double level) const = 0;
};
```

Conceptually:

```text
Value
 ↓
Window/Level
 ↓
Normalize
 ↓
LUT
 ↓
RGBA
```

---

# 22.65 Color Overlay Interface

```cpp
struct Overlay
{
    Image image;
    ColorMap colorMap;
    double opacity;
    bool visible;
};
```

Then:

```cpp
class OverlayComposer
{
public:
    Image compose(
        const Image& base,
        const std::vector<Overlay>& overlays);
};
```

---

# 22.66 Why This Architecture Is Important

It separates:

```text
Medical data
```

from:

```text
Display data
```

and:

```text
UI controls
```

So:

```text
QML
 ↓
Controller
 ↓
Display Pipeline
 ↓
Medical Image
```

instead of putting medical image processing directly into QML.

---

# 22.67 Color Processing and VTK

VTK is useful for:

* scalar-to-color mapping
* lookup tables
* volume rendering
* segmentation visualization
* overlays
* transfer functions

Conceptually:

```text
Scalar Volume
 ↓
VTK Color Mapping
 ↓
RGBA
 ↓
Rendering
```

---

# 22.68 Color Processing and OpenCV

OpenCV is useful for:

* RGB conversion
* HSV conversion
* Lab conversion
* color thresholding
* color segmentation
* histograms
* image processing

Conceptually:

```text
cv::Mat
 ↓
Color conversion
 ↓
Processing
 ↓
Feature / mask
```

---

# 22.69 ITK Color Processing

ITK is particularly useful when color/scalar processing is part of a larger medical-image processing pipeline.

For example:

```text
DICOM
 ↓
ITK Image
 ↓
Medical processing
 ↓
Segmentation
 ↓
Color/label representation
```

---

# 22.70 Enterprise Viewer Color Architecture

For your planned raw/DICOM/CT/MRI/X-ray viewer:

```text
                 IMAGE DATA
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      Grayscale               Color
          │                     │
          ↓                     ↓
   Physical values         RGB / YCbCr /
          │                 Palette Color
          ↓                     │
   Window/Level                │
          │                     │
          ↓                     │
      Normalize                 │
          │                     │
          ↓                     │
       Color LUT ───────────────┘
          │
          ↓
      Base RGBA
          │
     ┌────┼───────────┐
     ↓    ↓           ↓
   Dose  Segmentation Annotation
     │    │           │
     └────┴──────┬────┘
                  ↓
             Alpha Blend
                  ↓
             GPU Rendering
                  ↓
                QML
```

---

# 22.71 Important Medical Viewer Rule

Never confuse:

```text
Color
```

with:

```text
Measurement
```

For example:

```text
Red
```

might mean:

* high dose
* high uptake
* tumor
* high probability

depending on the active overlay.

Therefore the UI should clearly communicate:

```text
Color Map:
Dose
```

or:

```text
Color Map:
PET SUV
```

rather than relying on color alone.

---

# 22.72 Color Bar / Legend

Every quantitative color overlay should ideally have a legend.

Example:

```text
High
 🔴
 🟠
 🟡
 🟢
 🔵
Low
```

with numerical values:

```text
100 ┤ 🔴
 80 ┤ 🟠
 60 ┤ 🟡
 40 ┤ 🟢
 20 ┤ 🔵
```

This is essential for quantitative interpretation.

---

# 22.73 Accessibility

Do not depend solely on:

```text
red vs green
```

because color-vision deficiencies can make these distinctions difficult.

Use:

* labels
* contours
* symbols
* numerical legends
* opacity
* different line styles

when appropriate.

---

# 22.74 Chapter 22 Mental Model

```text
                    COLOR PROCESSING
                           │
        ┌──────────────────┼──────────────────┐
        ↓                  ↓                  ↓
       RGB                HSV                Lab
        │                  │                  │
    Display             Segmentation       Analysis
        │                  │                  │
        └──────────────────┼──────────────────┘
                           ↓
                    Color Conversion
                           ↓
                    Color Enhancement
                           ↓
                     Color Histogram
                           ↓
                    Color Segmentation
                           ↓
                     Pseudocolor
                           ↓
                         LUT
                           ↓
                  Medical Visualization
                           │
          ┌────────────────┼─────────────────┐
          ↓                ↓                 ↓
       Window/Level    Segmentation         Dose
          ↓                ↓                 ↓
         LUT              LUT               LUT
          └────────────────┼─────────────────┘
                           ↓
                      Alpha Blend
                           ↓
                       RGBA Image
                           ↓
                    GPU / QML Viewer
```

---

# 22.75 Key Formulas

### RGB → approximate luminance

[
\boxed{
Y\approx0.299R+0.587G+0.114B
}
]

### Alpha blending

[
\boxed{
C=\alpha C_{overlay}
+(1-\alpha)C_{base}
}
]

### Window limits

[
\boxed{
Low=L-\frac W2
}
]

[
\boxed{
High=L+\frac W2
}
]

### Lab color difference — basic ΔE

[
\boxed{
\Delta E_{ab}
=============

\sqrt{
\Delta L^2+\Delta a^2+\Delta b^2
}
}
]

---

# 22.76 Knowledge Check

### Color spaces

1. What is RGB?
2. Why is RGB called additive?
3. What are HSV's three components?
4. What is Hue?
5. What is Saturation?
6. What is Value?
7. What is HSL?
8. What is the difference between HSV and HSL?
9. What is YUV?
10. What is YCbCr?
11. What is Lab?
12. Why is Lab useful for color differences?

### Color processing

13. What is color conversion?
14. What is a color histogram?
15. What is color segmentation?
16. Why can HSV be useful for segmentation?
17. What is pseudocolor?
18. What is false color?
19. What is a LUT?
20. Why is a LUT important in medical visualization?

### Medical imaging

21. What is the difference between native RGB and pseudocolor?
22. How does Window/Level interact with a color LUT?
23. Why must original CT values be preserved?
24. What is alpha blending?
25. Why are segmentation masks mapped through a label LUT?
26. What is DICOM Palette Color?
27. What is MONOCHROME1?
28. What is MONOCHROME2?
29. Why is photometric interpretation important?
30. Why should quantitative color overlays have a color bar?
31. Why shouldn't a medical viewer rely only on color to communicate meaning?

---

# 22.77 Practical Exercise — Window/Level + LUT

Suppose:

[
W=400
]

[
L=40
]

Calculate:

[
Low
]

and:

[
High
]

Then determine what happens to:

```text
HU = -300
HU = -160
HU = 40
HU = 240
HU = 500
```

Finally design a LUT:

```text
0   → Blue
64  → Cyan
128 → Green
192 → Yellow
255 → Red
```

---

# 22.78 Practical Exercise — Alpha Blending

Suppose:

```text
Base = (100,100,100)
Overlay = (255,0,0)
α = 0.5
```

Calculate:

[
C=
0.5C_{overlay}
+
0.5C_{base}
]

You should obtain approximately:

[
\boxed{
(177.5,;50,;50)
}
]

before integer conversion/clamping.

---

# 22.79 Medical Viewer Exercise

Design this display pipeline:

```text
DICOM CT
 ↓
Rescale
 ↓
HU
 ↓
Window/Level
 ↓
Grayscale
 ↓
Tumor segmentation
 ↓
Red LUT
 ↓
50% opacity
 ↓
Alpha blend
 ↓
QML display
```

Then add:

```text
Dose overlay
 ↓
Dose LUT
 ↓
30% opacity
```

Explain how the final compositor should handle the two overlays.

---

# 22.80 Chapter 22 Complete

The most important concepts are:

[
\boxed{
RGB = display/color representation
}
]

[
\boxed{
HSV = intuitive color decomposition
}
]

[
\boxed{
YCbCr/YUV = luma + chroma
}
]

[
\boxed{
Lab = perceptually oriented color space
}
]

[
\boxed{
LUT = data\rightarrow color\ mapping
}
]

[
\boxed{
Pseudocolor = colorizing\ scalar/grayscale\ data
}
]

[
\boxed{
Alpha\ blending = combining\ image\ layers
}
]

And for your medical viewer:

```text
Raw / DICOM Data
       ↓
Physical Value
       ↓
Window / Level
       ↓
Normalize
       ↓
Color LUT
       ↓
Base RGBA
       ↓
Segmentation / Dose / Annotation overlays
       ↓
Alpha Composition
       ↓
GPU
       ↓
QML Viewer
```

**Chapter 22 complete.**

### Next, strictly according to your index:

# Chapter 23 — Image Compression

Topics:

* Why image compression is required
* Lossless vs lossy compression
* Redundancy
* Run-Length Encoding
* Huffman coding
* Arithmetic coding
* LZW
* JPEG
* JPEG 2000
* JPEG-LS
* PNG
* DICOM compression
* Transfer syntaxes
* Medical image compression
* Compression artifacts
* Compression ratio
* PSNR
* SSIM
* Compression and diagnostic quality
* Lossless medical-image workflows
