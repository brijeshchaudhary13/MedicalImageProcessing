# 🎓 Image Processing Master Course

## Chapter 1 — What Is Image Processing?

**Level:** Beginner
**Lesson:** 1.1
**Prerequisite:** None
**Goal:** Build the correct mental model of an image before writing a single line of code.

---

# 1. What is Image Processing?

Let's begin with the simplest possible definition:

> **Image Processing is the process of using computational methods to manipulate, transform, improve, analyze, or extract information from an image.**

But this definition is too broad.

Let's understand it from first principles.

---

# 2. First: What is an Image?

You see this:

```text
        👁️
        │
        ▼
   ┌───────────┐
   │   IMAGE   │
   └───────────┘
```

Your brain interprets the image as objects, shapes, colors, edges, etc.

A computer doesn't naturally see:

> "This is a CT scan of a patient's chest."

Instead, a computer sees **numbers**.

For example, a tiny grayscale image might be:

```text
10   20   30   40
20   30   40   50
30   40   50   60
40   50   60   70
```

Those numbers represent **pixel intensities**.

So one of the most important ideas in this entire course is:

> **A digital image is numerical data arranged in a spatial structure.**

Remember this.

---

# 3. The Digital Image Pipeline

A real-world scene might look like:

```text
         REAL WORLD
             │
             ▼
       Light / Energy
             │
             ▼
          SENSOR
             │
             ▼
      Analog Signal
             │
             ▼
       Sampling
             │
             ▼
      Quantization
             │
             ▼
      DIGITAL IMAGE
             │
             ▼
     IMAGE PROCESSING
             │
             ▼
       INFORMATION
```

This pipeline is fundamental.

For a normal camera:

```text
Object
  ↓
Light
  ↓
Lens
  ↓
CMOS Sensor
  ↓
Electrical Signal
  ↓
ADC
  ↓
Digital Image
```

For CT:

```text
X-ray source
     ↓
Patient
     ↓
X-ray attenuation
     ↓
Detector
     ↓
Projection data
     ↓
Reconstruction
     ↓
CT volume
     ↓
Image Processing
```

For MRI:

```text
Magnetic field
       ↓
RF excitation
       ↓
Patient
       ↓
MR signal
       ↓
Acquisition
       ↓
Reconstruction
       ↓
MRI image
       ↓
Image Processing
```

Already you can see why medical image processing is much more complicated than simply loading a JPEG.

---

# 4. Why Do We Need Image Processing?

Suppose you have an X-ray image:

```text
        X-RAY
┌──────────────────────┐
│                      │
│       LUNG           │
│      ______          │
│     /      \         │
│    |        |        │
│     \______/         │
│                      │
└──────────────────────┘
```

The raw image may have:

* poor contrast
* noise
* artifacts
* uneven illumination
* low signal
* blur

We can process it.

For example:

```text
Raw Image
    │
    ├── Denoising
    │
    ├── Contrast Enhancement
    │
    ├── Sharpening
    │
    ├── Edge Detection
    │
    └── Segmentation
            │
            ▼
       Useful Information
```

---

# 5. Main Categories of Image Processing

Image processing isn't one operation.

We can divide it into several major categories.

### A. Image Enhancement

Make an image easier to see.

Examples:

* brightness adjustment
* contrast enhancement
* gamma correction
* histogram equalization
* sharpening
* denoising

Example:

```text
Dark Image
    ↓
Contrast Enhancement
    ↓
Better Visibility
```

---

### B. Image Restoration

Try to recover an image degraded by a known/estimated process.

For example:

```text
Original
   ↓
Blur
   ↓
Observed Image
   ↓
Restoration
   ↓
Estimated Original
```

Applications:

* motion blur correction
* defocus correction
* noise reduction
* deconvolution

---

### C. Image Segmentation

Separate an image into meaningful regions.

Example:

```text
CT Image

┌─────────────────────┐
│                     │
│      ┌───────┐      │
│      │ TUMOR │      │
│      └───────┘      │
│                     │
└─────────────────────┘
```

The computer might produce:

```text
Background = 0
Tumor      = 1
```

This is extremely important in medical imaging.

---

### D. Feature Extraction

Extract useful characteristics.

For example:

```text
Image
 ↓
Edges
 ↓
Corners
 ↓
Contours
 ↓
Shape
 ↓
Features
```

Features can include:

* edges
* corners
* texture
* shape
* intensity
* local descriptors

---

### E. Image Analysis

Use an image to obtain measurements.

Example:

```text
Tumor Segmentation
       ↓
Area
       ↓
Perimeter
       ↓
Volume
       ↓
Diameter
       ↓
Growth rate
```

This is where image processing begins becoming **image analysis**.

---

# 6. Image Processing vs Computer Vision

This distinction is extremely important for interviews.

| Image Processing   | Computer Vision     |
| ------------------ | ------------------- |
| Manipulates images | Understands images  |
| Enhancement        | Recognition         |
| Filtering          | Detection           |
| Denoising          | Classification      |
| Contrast           | Object detection    |
| Segmentation       | Tracking            |
| Restoration        | Scene understanding |

A simplified view:

```text
IMAGE PROCESSING
      │
      ▼
Improve / Transform / Extract
      │
      ▼
COMPUTER VISION
      │
      ▼
Understand / Recognize / Decide
```

But they overlap heavily.

For example:

```text
Camera
  ↓
Denoising          ← Image Processing
  ↓
Contrast
  ↓
Edge Detection     ← Image Processing
  ↓
Feature Extraction ← Both
  ↓
Object Detection   ← Computer Vision
  ↓
Decision
```

So don't think of them as completely separate fields.

---

# 7. Image Processing vs Computer Graphics

Another common interview question.

### Image Processing

Starts with an image/data and processes it.

```text
IMAGE
 ↓
PROCESS
 ↓
IMAGE / INFORMATION
```

### Computer Graphics

Starts with mathematical descriptions and generates images.

```text
OBJECT / MODEL
      ↓
RENDERING
      ↓
IMAGE
```

Example:

A CT scan:

```text
Patient
 ↓
Scanner
 ↓
CT Image
```

This is imaging.

A 3D rendering of a patient's skull:

```text
3D Model
 ↓
Renderer
 ↓
2D Display
```

This is computer graphics/visualization.

---

# 8. Image Processing vs Image Analysis

Consider a CT image.

First:

```text
CT
 ↓
Denoising
 ↓
Windowing
 ↓
Contrast enhancement
```

This is primarily **image processing**.

Then:

```text
CT
 ↓
Tumor segmentation
 ↓
Tumor area
 ↓
Tumor volume
 ↓
Measurements
```

This is **image analysis**.

---

# 9. Medical Imaging Connection

Now let's connect today's concept directly to your TPS work.

Suppose a CT scanner produces a CT study.

You may receive:

```text
Patient
   │
   ▼
CT Scanner
   │
   ▼
DICOM
   │
   ▼
CT Slices
   │
   ▼
3D Volume
   │
   ├── Window/Level
   ├── Filtering
   ├── Registration
   ├── Segmentation
   ├── MPR
   ├── 3D Rendering
   └── Dose Visualization
```

Every one of these involves image-processing or image-analysis concepts.

Eventually your software might look like:

```text
                 ┌──────────────┐
                 │ DICOM Reader │
                 └──────┬───────┘
                        ↓
                 ┌──────────────┐
                 │ Image Model  │
                 └──────┬───────┘
                        ↓
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Processing     Analysis      Rendering
          ↓             ↓             ↓
       ITK/OpenCV    Algorithms       VTK
          │             │             │
          └─────────────┼─────────────┘
                        ↓
                       Qt
                        ↓
                   User Interface
```

This architecture will become very important later.

---

# 10. What Does a Computer Actually Store?

Suppose we have a grayscale image:

```text
4 × 4
```

It could be represented as:

```text
10   20   30   40
20   30   40   50
30   40   50   60
40   50   60   70
```

Mathematically:

[
I(x,y)
]

where:

* (x) = horizontal coordinate
* (y) = vertical coordinate
* (I(x,y)) = intensity

For example:

[
I(2,3)=50
]

depending on the coordinate convention being used.

The key idea:

```text
Image
  =
2D function
  =
matrix of values
  =
numerical data
```

---

# 11. Grayscale Image

A grayscale image usually has one intensity value per pixel.

For an 8-bit image:

[
0 \le I(x,y) \le 255
]

Typically:

```text
0     → black
255   → white
```

and:

```text
0
│
├── dark gray
│
├── gray
│
├── light gray
│
255
```

So:

```text
Pixel = intensity
```

for a basic grayscale image.

---

# 12. Color Image

A typical RGB image has three channels:

```text
Pixel
 ├── R
 ├── G
 └── B
```

For example:

```text
R = 255
G = 0
B = 0
```

represents red.

A color image can therefore be viewed as three matrices:

```text
       RGB Image

       ┌─────┐
       │     │
       │     │
       └─────┘
          │
     ┌────┼────┐
     ↓    ↓    ↓
     R    G    B
```

This becomes important when we study:

* RGB
* HSV
* LAB
* YCbCr
* color segmentation

---

# 13. A Very Important Medical Difference

A CT image is **not simply an ordinary grayscale photograph**.

For example, CT pixels can represent reconstructed attenuation values.

After appropriate DICOM rescaling:

[
HU = PixelValue \times RescaleSlope + RescaleIntercept
]

This produces **Hounsfield Units**.

Typical conceptual values:

| Material    |           Approximate HU |
| ----------- | -----------------------: |
| Air         |                    -1000 |
| Fat         |              around -100 |
| Water       |                        0 |
| Soft tissue |     positive, near water |
| Bone        | several hundred to >1000 |
| Dense metal |                very high |

We'll study this properly in the medical-imaging section.

This is why medical image processing requires understanding both:

**image processing + imaging physics + metadata.**

---

# 14. What Can We Do to an Image?

Think of an image as data.

Then we can perform mathematical operations on it.

For example:

### Brightness

[
I'(x,y)=I(x,y)+C
]

### Contrast

[
I'(x,y)=\alpha I(x,y)
]

### Thresholding

[
I'(x,y)=
\begin{cases}
255 & I(x,y)>T\
0 & I(x,y)\leq T
\end{cases}
]

### Convolution

[
g(x,y)=
\sum_i\sum_j
f(x-i,y-j)h(i,j)
]

These formulas will become fundamental tools throughout this course.

---

# 15. First C++ Mental Model

Before using OpenCV, let's understand what we would build ourselves.

A very simple grayscale image class:

```cpp
#include <vector>
#include <cstddef>

class Image
{
public:
    Image(std::size_t width, std::size_t height)
        : m_width(width),
          m_height(height),
          m_data(width * height, 0)
    {
    }

    unsigned char& pixel(std::size_t x, std::size_t y)
    {
        return m_data[y * m_width + x];
    }

    unsigned char pixel(std::size_t x, std::size_t y) const
    {
        return m_data[y * m_width + x];
    }

    std::size_t width() const noexcept
    {
        return m_width;
    }

    std::size_t height() const noexcept
    {
        return m_height;
    }

private:
    std::size_t m_width;
    std::size_t m_height;
    std::vector<unsigned char> m_data;
};
```

The most important line is:

```cpp
m_data[y * m_width + x]
```

A 2D image is often stored in **contiguous 1D memory**.

For:

```text
width = 4
height = 3
```

memory can look like:

```text
Row 0:
[0] [1] [2] [3]

Row 1:
[4] [5] [6] [7]

Row 2:
[8] [9] [10] [11]
```

The address/index formula is:

[
index = y \times width + x
]

This simple concept becomes extremely important when we later discuss:

* cache locality
* SIMD
* multithreading
* CUDA
* image performance
* ITK memory layout
* OpenCV `Mat`

---

# 16. Why Memory Layout Matters

Suppose we process an image row-by-row:

```text
→ → → →
→ → → →
→ → → →
```

The CPU can efficiently read nearby memory.

But if we access:

```text
↓
↓
↓
↓
```

depending on the layout and algorithm, memory access can become less cache-friendly.

Later we'll study:

```text
CPU
 │
 ├── Registers
 ├── L1 Cache
 ├── L2 Cache
 ├── L3 Cache
 └── RAM
```

and learn how an image-processing algorithm can be made **2×, 5×, or even much faster** through memory-aware design.

---

# 17. First From-Scratch Operation

Let's implement brightness adjustment.

Mathematically:

[
I'(x,y)=I(x,y)+B
]

C++:

```cpp
void adjustBrightness(Image& image, int brightness)
{
    for (std::size_t y = 0; y < image.height(); ++y)
    {
        for (std::size_t x = 0; x < image.width(); ++x)
        {
            int value = image.pixel(x, y) + brightness;

            if (value < 0)
                value = 0;

            if (value > 255)
                value = 255;

            image.pixel(x, y) =
                static_cast<unsigned char>(value);
        }
    }
}
```

Notice something important.

We don't blindly do:

```cpp
image.pixel(x, y) += brightness;
```

because values must remain inside:

[
[0,255]
]

This is called **clamping**.

---

# 18. Complexity

For an image of width (W) and height (H):

```cpp
for y
    for x
```

we process:

[
W \times H
]

pixels.

Therefore:

### Time

[
O(W H)
]

### Space

[
O(1)
]

additional memory if processing in-place.

This basic complexity analysis will become standard for every algorithm we learn.

---

# 19. OpenCV Version

Only after understanding the algorithm ourselves should we use OpenCV.

Conceptually:

```cpp
cv::Mat image;

image += brightness;
```

or a more controlled implementation using OpenCV operations.

But the important principle for this course is:

> **First understand the algorithm. Then use the library.**

We will not treat OpenCV as a black box.

---

# 20. ITK Perspective

For medical imaging, ITK introduces a richer image model.

Conceptually:

```text
ITK Image
│
├── Pixel Data
├── Dimension
├── Region
├── Index
├── Size
├── Spacing
├── Origin
└── Direction
```

That last group is extremely important.

An ordinary image might only need:

```text
width
height
pixel
```

A medical image needs to know:

```text
What does this pixel physically represent?
Where is it located?
What is its physical spacing?
What orientation does it have?
```

For example:

```text
Pixel spacing:
0.7 mm × 0.7 mm
```

means a pixel is not merely:

```text
1 pixel
```

It represents a physical area.

---

# 21. The Three Levels You Must Learn

Throughout this course, always think at three levels:

### Level 1 — Mathematical

```text
I(x,y)
```

### Level 2 — Algorithm

```text
for every pixel
    calculate new value
```

### Level 3 — Software

```text
C++17
 ↓
Image class
 ↓
Memory
 ↓
Algorithm
 ↓
Optimization
 ↓
OpenCV / ITK
 ↓
Qt application
```

An expert must understand all three.

---

# 22. Real Industrial Example

Consider an industrial X-ray inspection system.

```text
X-ray Detector
      ↓
Raw Image
      ↓
Calibration
      ↓
Noise Reduction
      ↓
Contrast Enhancement
      ↓
Defect Detection
      ↓
Segmentation
      ↓
Feature Extraction
      ↓
Classification
      ↓
PASS / FAIL
```

Image processing is therefore not merely:

> "make image beautiful."

It can be part of a **decision-making pipeline**.

---

# 23. Medical Example

Consider a radiotherapy TPS:

```text
DICOM CT
   ↓
DICOM Parsing
   ↓
HU Conversion
   ↓
3D Volume
   ↓
Window/Level
   ↓
Registration
   ↓
Structure Segmentation
   ↓
RTSTRUCT
   ↓
Dose Calculation
   ↓
RTDOSE
   ↓
Dose Visualization
   ↓
DVH
```

Eventually, your knowledge of image processing will connect almost every component in this pipeline.

---

# 24. Common Beginner Mistakes

### Mistake 1

Thinking:

> Image = picture.

Correct:

> Image = structured numerical data representing measurements or visual information.

---

### Mistake 2

Thinking every image is 8-bit.

Wrong.

Images can be:

```text
1-bit
8-bit
10-bit
12-bit
16-bit
32-bit
64-bit
float
double
```

Medical imaging frequently requires higher precision.

---

### Mistake 3

Thinking CT is just grayscale.

Wrong.

CT pixel values have physical meaning and depend on acquisition/reconstruction and DICOM metadata.

---

### Mistake 4

Learning OpenCV functions without understanding mathematics.

We will avoid this.

---

### Mistake 5

Ignoring memory layout.

For small images it doesn't matter much.

For:

```text
4096 × 4096
```

or a:

```text
512 × 512 × 1000
```

medical volume, it matters enormously.

---

# 25. Interview Questions

### Beginner

**Q1. What is digital image processing?**

**Q2. What is a pixel?**

**Q3. What is the difference between grayscale and RGB images?**

**Q4. What is image enhancement?**

**Q5. What is image segmentation?**

---

### Intermediate

**Q6. Why is a digital image represented as a matrix?**

**Q7. What is the difference between image processing and computer vision?**

**Q8. What is the difference between image processing and image restoration?**

**Q9. Why is image memory often represented as a 1D contiguous array?**

**Q10. What is the time complexity of processing every pixel once?**

---

### Advanced

**Q11. Why is medical image processing different from ordinary photographic image processing?**

**Q12. Why can't you safely assume that every medical image uses 8-bit unsigned pixels?**

**Q13. Why are physical spacing and image orientation important in medical imaging?**

**Q14. How can cache locality affect image-processing performance?**

---

# 🧠 Lesson 1 Quiz — Don't Look Up the Answers

Answer these yourself.

### Q1

What is an image from a computer's perspective?

A. A photograph
B. A collection of numerical data
C. A GUI object
D. A text file

---

### Q2

For an 8-bit grayscale image, the usual intensity range is:

A. 0–1
B. 0–100
C. 0–255
D. -255–255

---

### Q3

If an image has width (1920) and height (1080), how many pixels does it contain?

---

### Q4

What does this represent?

[
I(x,y)
]

---

### Q5

What is the time complexity of visiting every pixel once?

---

### Q6

Why do we clamp a brightness-adjusted pixel to `[0,255]`?

---

### Q7

Explain the difference between:

```text
Image Processing
Computer Vision
Computer Graphics
```

in your own words.

---

### Q8 — Medical Imaging

Why is this:

```text
512 × 512
```

not enough information to completely describe a CT image?

---

### Q9 — C++

Given:

```text
width = 5
x = 3
y = 2
```

calculate:

[
index = y \times width + x
]

---

### Q10 — Expert Thinking

Why might this:

```cpp
for (y)
    for (x)
```

be faster than an algorithm that accesses pixels in a scattered/random pattern?

---

# 🏠 Homework

## Assignment 1 — Conceptual

Write your own definition of:

1. Image
2. Digital image
3. Pixel
4. Image processing
5. Computer vision
6. Image analysis
7. Image enhancement
8. Image segmentation

Don't copy my definitions.

---

## Assignment 2 — C++17

Implement your own:

```cpp
class Image
```

with:

```text
width
height
pixel access
fill
brightness adjustment
```

Do **not** use OpenCV.

---

## Assignment 3 — Mathematical

For this image:

```text
10  20  30
40  50  60
70  80  90
```

calculate the result after:

[
I'(x,y)=I(x,y)+20
]

Then calculate the result after:

[
I'(x,y)=I(x,y)-40
]

---

# 🔬 Implementation Challenge

Create a small C++17 program that:

```text
Creates a 5 × 5 image
        ↓
Fills it with intensity 100
        ↓
Increases brightness by 50
        ↓
Prints the matrix
```

Expected conceptual output:

```text
150 150 150 150 150
150 150 150 150 150
150 150 150 150 150
150 150 150 150 150
150 150 150 150 150
```

Then try brightness:

```text
+200
```

and observe why the values must be clamped to 255.

---

# 📌 Your Current Progress

```text
Chapter 1 — What Is Image Processing?

Lesson 1.1 — Fundamentals
██████████░░░░░░░░░░  ~50%

✓ What is an image
✓ Digital image
✓ Pixel
✓ Image as numerical data
✓ Image processing
✓ Image enhancement
✓ Image restoration
✓ Image segmentation
✓ Image analysis
✓ Computer vision
✓ Computer graphics
✓ Basic C++ image representation
✓ Basic complexity
✓ Medical imaging connection

⬜ Image formation in depth
⬜ Image representation in depth
⬜ Image acquisition
⬜ Sensor fundamentals
⬜ Sampling
⬜ Quantization
⬜ Image coordinate systems
```

# 🎓 Image Processing Master Course

## Chapter 1 — What Is Image Processing?

### Lesson 1.2 — Image Formation & Acquisition

We continue **Chapter 1**. We are not moving to Chapter 2 yet.

Today we answer a fundamental question:

> **Where does an image actually come from?**

If you understand this properly, later topics such as CT, MRI, X-ray, DICOM, noise, sampling, reconstruction, and image enhancement will make much more sense.

---

# 1. The Big Picture

An image does not magically exist.

Something in the physical world interacts with some form of energy, a sensor measures that interaction, and the measurement is converted into digital numbers.

The general model is:

```text
             REAL WORLD
                  │
                  ▼
        Physical phenomenon
                  │
       ┌──────────┼──────────┐
       │          │          │
      Light      X-ray      RF
       │          │          │
       ▼          ▼          ▼
     Sensor     Detector    Coil
       │          │          │
       ▼          ▼          ▼
    Signal      Signal      Signal
       │          │          │
       └──────────┼──────────┘
                  ▼
             Acquisition
                  │
                  ▼
              Sampling
                  │
                  ▼
             Quantization
                  │
                  ▼
           DIGITAL IMAGE
```

This pipeline is one of the most important mental models in image processing.

---

# 2. What Is Image Formation?

**Image formation** is the process by which information from the physical world becomes an image.

For a camera:

```text
Object
  ↓
Light reflected from object
  ↓
Lens
  ↓
Sensor
  ↓
Electrical signal
  ↓
Digital values
  ↓
Image
```

For CT:

```text
X-ray source
     ↓
X-rays
     ↓
Patient
     ↓
Attenuation
     ↓
Detector
     ↓
Projection measurements
     ↓
Reconstruction
     ↓
CT image
```

For MRI:

```text
Magnetic field
      ↓
RF excitation
      ↓
Nuclear response
      ↓
RF signal
      ↓
Acquisition
      ↓
Mathematical reconstruction
      ↓
MRI image
```

Notice something important:

**The camera, CT scanner, and MRI scanner do not produce images in the same physical way.**

That is why medical image processing requires some understanding of imaging physics.

---

# 3. Camera Example

Let's start with something familiar.

Suppose you photograph a person.

There is an object:

```text
       PERSON
          │
          │ reflected light
          ▼
        Lens
          │
          ▼
     ┌─────────┐
     │  CMOS   │
     │ Sensor  │
     └─────────┘
          │
          ▼
   Electrical signal
          │
          ▼
        ADC
          │
          ▼
     Digital image
```

The sensor contains millions of tiny sensing elements.

Each location produces a measurement.

Conceptually:

```text
Sensor

● ● ● ● ● ●
● ● ● ● ● ●
● ● ● ● ● ●
● ● ● ● ● ●
```

Each sensor location eventually corresponds to an image pixel.

---

# 4. What Is a Pixel?

A pixel is the smallest addressable element of a digital image.

For a basic grayscale image:

```text
Pixel
  │
  └── intensity
```

For RGB:

```text
Pixel
 ├── Red
 ├── Green
 └── Blue
```

But don't make the mistake of thinking:

> Pixel = physical point.

A pixel represents a measurement over some finite spatial region.

That distinction becomes extremely important in medical imaging.

---

# 5. Pixel vs Voxel

For a 2D image:

```text
Pixel
```

For a 3D medical volume:

```text
Voxel = Volume Element
```

Conceptually:

```text
2D

┌───┬───┬───┐
│ P │ P │ P │
├───┼───┼───┤
│ P │ P │ P │
├───┼───┼───┤
│ P │ P │ P │
└───┴───┴───┘
```

3D:

```text
       ┌───────────────┐
      /│              /│
     / │             / │
    ┌───────────────┐  │
    │  │            │  │
    │  └────────────│──┘
    │ /             │ /
    │/              │/
    └───────────────┘

             VOXELS
```

A CT volume may have:

```text
512 × 512 × 300
```

voxels.

That's:

[
512 \times 512 \times 300
= 78,643,200
]

voxels.

That is almost **79 million measurements**.

This is why medical-image memory management becomes important later.

---

# 6. The Pinhole Camera Model

Now we introduce an important mathematical model.

Imagine a tiny hole between an object and an image plane:

```text
Object                Camera              Image

   P
   ●
    \
     \
      \
       ●
      pinhole
         \
          \
           ●
          P'
```

This is the **pinhole camera model**.

The 3D world is projected onto a 2D plane.

---

# 7. Perspective Projection

Suppose:

```text
3D point:

P = (X, Y, Z)
```

Its image coordinate can be approximately expressed as:

[
x = f\frac{X}{Z}
]

[
y = f\frac{Y}{Z}
]

where:

* (X,Y,Z) = 3D coordinates
* (x,y) = image coordinates
* (f) = focal length

This equation is incredibly important.

---

# 8. Why Does Perspective Matter?

Suppose two identical objects exist:

```text
Object A       Object B

  █               █
  █               █
  █               █
```

If Object A is close:

```text
Camera
  │
  ▼
 █
```

and Object B is far:

```text
Camera
  │
  │
  │
  ▼
 █
```

the far object appears smaller.

Why?

Because:

[
x = f\frac{X}{Z}
]

As (Z) increases:

[
x \rightarrow smaller
]

This is the mathematical foundation of perspective.

---

# 9. Why This Matters for Computer Vision

Suppose a self-driving car sees:

```text
       CAR
        ↓
      Camera
```

The camera only receives a 2D image.

But the world is 3D.

So computer vision has to infer:

```text
2D image
   ↓
features
   ↓
geometry
   ↓
depth
   ↓
3D understanding
```

This eventually leads us to:

* camera calibration
* stereo vision
* epipolar geometry
* depth estimation
* 3D reconstruction
* SLAM

These are much later chapters.

---

# 10. Image Formation Is Not Perfect

Real sensors introduce imperfections.

Suppose the ideal image is:

```text
I(x,y)
```

The sensor may produce:

```text
Observed image
=
Ideal image
+
Noise
+
Blur
+
Distortion
+
Sampling effects
+
Quantization effects
```

A simplified model is:

[
g(x,y)=h(x,y)*f(x,y)+n(x,y)
]

where:

* (f(x,y)) = original image
* (h(x,y)) = system blur / point spread function
* (*) = convolution
* (n(x,y)) = noise
* (g(x,y)) = observed image

Don't worry if this equation isn't completely intuitive yet.

We will derive it properly when we study convolution and image degradation.

But remember the concept:

> **The image we receive is often an imperfect measurement of reality.**

---

# 11. Point Spread Function

An imaging system does not always reproduce a perfect point as a perfect point.

Suppose the real object contains:

```text
•
```

An ideal system would produce:

```text
•
```

A real system might produce:

```text
  ░
 ░░░
░░░░░
 ░░░
  ░
```

This response is related to the **Point Spread Function (PSF)**.

PSF becomes important in:

* microscopy
* CT
* MRI
* astronomy
* photography
* deconvolution
* image restoration

---

# 12. Image Acquisition

Now let's distinguish **image formation** from **image acquisition**.

### Image formation

Physical process:

```text
World
 ↓
Energy interaction
 ↓
Sensor response
```

### Image acquisition

Technical process of collecting and digitizing those measurements.

```text
Sensor
 ↓
Analog signal
 ↓
ADC
 ↓
Digital data
```

So:

```text
IMAGE FORMATION
       +
IMAGE ACQUISITION
       ↓
DIGITAL IMAGE
```

---

# 13. Analog to Digital Conversion

Sensors often produce physical/electrical signals.

Computers need numbers.

Therefore:

```text
Physical signal
      ↓
Analog signal
      ↓
ADC
      ↓
Digital value
```

ADC = **Analog-to-Digital Converter**.

Two fundamental processes occur:

### Sampling

Discretization in space/time.

### Quantization

Discretization in amplitude.

This distinction is extremely important.

---

# 14. Sampling vs Quantization

Suppose a continuous image is:

```text
continuous image
~~~~~~~~~~~~~~~~~~~~
~~~~~~~~~~~~~~~~~~~~
~~~~~~~~~~~~~~~~~~~~
```

Sampling chooses spatial locations:

```text
●   ●   ●   ●
            
●   ●   ●   ●
            
●   ●   ●   ●
```

Quantization chooses allowed intensity levels.

For example:

```text
Continuous intensity:

0.00
0.13
0.27
0.43
0.58
0.71
0.86
1.00
```

With 3-bit quantization:

[
2^3=8
]

possible levels.

So values are mapped to one of:

```text
0
1
2
3
4
5
6
7
```

We'll study sampling and quantization mathematically in the next lesson.

---

# 15. Medical Imaging: X-Ray

Let's apply the same model to X-ray.

X-rays pass through the body.

Different tissues attenuate X-rays differently.

Conceptually:

```text
X-ray source
     │
     ▼
   X-rays
     │
     ▼
 ┌─────────┐
 │ Patient │
 └─────────┘
     │
     ▼
   Detector
     │
     ▼
 Measurements
```

The detector doesn't directly measure:

> "bone"

It measures radiation intensity after passing through the body.

The system then processes those measurements to generate an image.

---

# 16. Medical Imaging: CT

CT is even more interesting.

One projection is not enough.

The scanner collects measurements from many angles.

Conceptually:

```text
             X-ray
               ↓
        \      │      /
         \     │     /
          \    │    /
           \   │   /
        ┌───────────┐
        │  PATIENT  │
        └───────────┘
           /   │   \
          /    │    \
         /     │     \
```

Many projections are collected.

Then:

```text
Projection Data
      ↓
Reconstruction Algorithm
      ↓
CT Slice
```

This is fundamentally different from simply taking a photograph.

---

# 17. CT Image Formation

A simplified CT pipeline:

```text
X-ray Tube
    ↓
X-ray Beam
    ↓
Patient
    ↓
Attenuation
    ↓
Detector Measurements
    ↓
Projection Data
    ↓
Sinogram
    ↓
Reconstruction
    ↓
CT Image
    ↓
HU Conversion
    ↓
Medical Image Processing
```

Later we'll study:

* Beer-Lambert law
* attenuation coefficient
* Radon transform
* sinogram
* filtered back projection
* iterative reconstruction
* Hounsfield Units

These are critical for your TPS/medical imaging path.

---

# 18. MRI Is Different

MRI doesn't primarily use X-ray attenuation.

Instead, it uses magnetic fields and radio-frequency excitation.

Very simplified:

```text
Strong Magnetic Field
         ↓
Hydrogen nuclei
         ↓
RF excitation
         ↓
Magnetization response
         ↓
RF signal
         ↓
k-space
         ↓
Fourier reconstruction
         ↓
MRI image
```

Notice:

```text
CT → projection/reconstruction

MRI → k-space/Fourier reconstruction
```

This is why Fourier analysis becomes especially important in medical imaging.

---

# 19. Image Formation Models

You should start thinking of images as measurements generated by a system.

A general model:

[
\boxed{
Image = Imaging\ System(World)
}
]

More realistically:

[
g = H(f) + n
]

where:

```text
f = underlying object
H = imaging system
n = noise
g = observed image
```

This is a powerful abstraction.

---

# 20. Why This Model Matters

Suppose you want to remove blur.

You have:

```text
f → H → g
```

You observe `g`.

You want to estimate `f`.

That's an inverse problem:

```text
Observed image
      ↓
   Inversion
      ↓
Estimated original
```

This leads to:

* restoration
* deconvolution
* CT reconstruction
* MRI reconstruction
* image registration
* compressed sensing

A huge part of advanced medical imaging is essentially solving **inverse problems**.

---

# 21. Camera vs CT vs MRI

| Property       | Camera                  | CT                            | MRI                              |
| -------------- | ----------------------- | ----------------------------- | -------------------------------- |
| Energy         | Visible light           | X-rays                        | RF + magnetic field              |
| Sensor         | CMOS/CCD                | X-ray detector                | RF coils                         |
| Output         | 2D image                | 2D/3D volume                  | 2D/3D volume                     |
| Acquisition    | Direct imaging          | Projections                   | k-space                          |
| Reconstruction | Relatively simple       | Essential                     | Essential                        |
| Typical data   | RGB                     | HU                            | MR intensity                     |
| Main artifacts | Blur, noise, distortion | Motion, metal, beam hardening | Motion, susceptibility, aliasing |

This table is worth remembering.

---

# 22. C++ Representation

Let's connect the physical process to software.

Suppose the acquisition system produces:

```text
Width  = 512
Height = 512
Pixel type = int16
```

Memory requirement:

[
512 \times 512 \times 2
]

bytes.

That is:

[
524,288\text{ bytes}
]

≈ **512 KB** for one slice.

Now consider 500 slices:

[
512 \times 512 \times 500 \times 2
]

≈ **250 MB**.

Suddenly image-processing architecture matters.

---

# 23. Why `int16_t` Matters in CT

If CT values or stored pixel values require signed 16-bit storage:

```cpp
std::int16_t
```

has a range:

[
-32768 \leq x \leq 32767
]

Compare that with:

```cpp
std::uint8_t
```

which is:

[
0 \leq x \leq 255
]

Therefore, using:

```cpp
uint8_t
```

for every medical image would be a serious design mistake.

---

# 24. A Better Image Class

Eventually we want something more generic:

```cpp
template<typename PixelType>
class Image
{
public:
    Image(std::size_t width,
          std::size_t height);

    PixelType& pixel(std::size_t x,
                     std::size_t y);

private:
    std::size_t m_width{};
    std::size_t m_height{};

    std::vector<PixelType> m_data;
};
```

Then:

```cpp
Image<std::uint8_t> image8;
Image<std::uint16_t> image16;
Image<std::int16_t> ctImage;
Image<float> floatImage;
```

This is the beginning of **generic image processing architecture**.

Later we'll build a much more professional version supporting:

* dimensions
* stride
* spacing
* origin
* direction
* channels
* ownership
* views
* ROI
* type conversion

---

# 25. Complexity

Image acquisition itself is hardware-dependent.

But after acquisition, many simple image operations process each pixel once.

For an image:

[
W\times H
]

a single-pass operation generally has:

### Time

[
O(W H)
]

### Extra Space

Often:

[
O(1)
]

for in-place processing.

Or:

[
O(W H)
]

if producing a separate output image.

For a 3D volume:

[
W\times H\times D
]

the corresponding complexity becomes:

[
O(WHD)
]

This difference becomes enormous for medical volumes.

---

# 26. Common Mistakes

### ❌ Mistake 1

Thinking the camera directly creates a digital image.

Actually:

```text
Physical world
→ optics
→ sensor
→ electrical signal
→ ADC
→ digital data
```

---

### ❌ Mistake 2

Thinking pixels are infinitely small points.

They represent spatial measurements over finite regions.

---

### ❌ Mistake 3

Thinking CT acquisition is equivalent to photography.

It isn't.

CT involves projection measurements and reconstruction.

---

### ❌ Mistake 4

Confusing sampling and quantization.

Remember:

```text
Sampling   → WHERE
Quantization → WHAT VALUE
```

---

### ❌ Mistake 5

Ignoring physical dimensions.

In medical imaging:

```text
512 × 512
```

doesn't tell you whether pixels represent:

```text
0.5 mm × 0.5 mm
```

or:

```text
1.0 mm × 1.0 mm
```

That changes physical measurements.

---

# 🧠 Knowledge Check

Don't look up the answers.

### Q1

What is image formation?

### Q2

Explain the pipeline:

```text
Physical world
→ sensor
→ ADC
→ digital image
```

### Q3

What is the difference between:

**sampling** and **quantization**?

### Q4

Why does a distant object appear smaller in a camera image?

Use:

[
x=fX/Z
]

in your explanation.

### Q5

What is a voxel?

### Q6

Why is CT image formation different from camera image formation?

### Q7

What is the difference between:

```text
Pixel
Voxel
```

### Q8

Why might CT data require `int16_t` instead of `uint8_t`?

### Q9

What is the meaning of:

[
g = H(f)+n
]

?

### Q10 — Important

Why is medical image processing often considered an **inverse problem**?

---

# 🧪 Mini Coding Exercise

Without OpenCV, create:

```cpp
template<typename PixelType>
class Image
```

and support:

```cpp
width()
height()

pixel(x, y)

fill(value)
```

Test it with:

```cpp
Image<std::uint8_t>
Image<std::int16_t>
Image<float>
```

Print a small `4 × 4` image.

---

# 📌 Current Progress

```text
CHAPTER 1 — WHAT IS IMAGE PROCESSING?

Lesson 1.1
████████████████████  COMPLETE

Lesson 1.2
Image Formation & Acquisition
████████████████░░░░  ~80%

Covered:
✓ Physical → sensor → digital image
✓ Image formation
✓ Image acquisition
✓ Pixels
✓ Voxels
✓ Camera model
✓ Perspective projection
✓ Sensor
✓ ADC
✓ Sampling introduction
✓ Quantization introduction
✓ PSF introduction
✓ Image degradation model
✓ CT acquisition concept
✓ MRI acquisition concept
✓ Medical image representation
✓ C++ image representation
✓ Memory calculation
✓ Complexity
```

# 🎓 Image Processing Master Course

## Chapter 1 — What Is Image Processing?

### Lesson 1.3 — Digital Image Representation

**Level:** Beginner
**Focus:** How a computer actually represents an image in memory.

Today we'll go deeper into:

* Pixel
* Coordinates
* Width/height
* Resolution
* Aspect ratio
* Bit depth
* Channels
* Grayscale
* RGB
* Dynamic range
* Pixel type
* Stride
* Memory layout
* Physical spacing
* Medical-image representation

This lesson is foundational. Almost every OpenCV/ITK/VTK operation you eventually write depends on these concepts.

---

# 1. The Core Mental Model

Forget the visual picture for a moment.

A computer sees an image approximately as:

```text
             IMAGE
               │
       ┌───────┴────────┐
       │                │
    Metadata          Pixel Data
       │                │
       │                └── Numerical values
       │
       ├── Width
       ├── Height
       ├── Pixel type
       ├── Spacing
       ├── Origin
       └── Direction
```

This distinction is **extremely important in medical imaging**.

For example:

```text
Pixel Data:
[23, 45, 67, 89, ...]

Metadata:
Width      = 512
Height     = 512
Pixel Type = int16
Spacing    = 0.7 × 0.7 mm
```

The numbers alone aren't enough to completely describe the image.

---

# 2. What Is a Pixel?

Suppose:

```text
10  20  30
40  50  60
70  80  90
```

Each number represents one pixel value.

We can label coordinates:

```text
        x →
        0    1    2

y=0    10   20   30
y=1    40   50   60
y=2    70   80   90
↓
```

So:

[
I(0,0)=10
]

[
I(1,0)=20
]

[
I(2,0)=30
]

and:

[
I(1,2)=80
]

---

# 3. Coordinate Convention

Most image-processing software uses:

```text
Origin
(0,0)
  ─────────→ X
  │
  │
  │
  ↓
  Y
```

So:

```text
(0,0) → top-left
```

Unlike the traditional Cartesian coordinate system:

```text
      Y
      ↑
      │
      │
──────┼──────→ X
      │
```

image coordinates commonly increase downward.

This difference causes many bugs when people first work with computer vision.

---

# 4. Width and Height

Suppose:

```text
Width  = 5
Height = 4
```

Then:

```text
5 columns
        ↓
┌───┬───┬───┬───┬───┐
│   │   │   │   │   │
├───┼───┼───┼───┼───┤
│   │   │   │   │   │
├───┼───┼───┼───┼───┤
│   │   │   │   │   │
├───┼───┼───┼───┼───┤
│   │   │   │   │   │
└───┴───┴───┴───┴───┘
↑
4 rows
```

Number of pixels:

[
N = W \times H
]

Therefore:

[
5\times4=20
]

pixels.

---

# 5. Resolution

This word causes a lot of confusion.

Suppose an image is:

```text
1920 × 1080
```

This describes its **pixel dimensions**.

It contains:

[
1920\times1080=2,073,600
]

pixels.

Approximately:

**2.07 megapixels.**

But resolution can also refer to **spatial resolution**.

These are not the same thing.

---

# 6. Pixel Dimensions vs Spatial Resolution

Consider two CT images:

```text
CT A:
512 × 512
pixel spacing = 0.5 mm

CT B:
512 × 512
pixel spacing = 1.0 mm
```

Both have:

```text
512 × 512 pixels
```

But they represent different physical sizes.

CT A:

[
512\times0.5=256\text{ mm}
]

CT B:

[
512\times1.0=512\text{ mm}
]

So the same number of pixels can represent very different physical dimensions.

---

# 7. Aspect Ratio

Aspect ratio describes the proportional relationship between width and height.

For:

```text
1920 × 1080
```

aspect ratio:

[
1920:1080
]

Divide by 120:

[
16:9
]

So:

```text
16 : 9
```

is a common display aspect ratio.

---

# 8. Pixel Aspect Ratio

There is another concept:

**Pixel aspect ratio.**

Suppose pixels physically measure:

```text
1.0 mm × 0.5 mm
```

Then they are not physically square.

```text
┌──────────────┐
│              │
└──────────────┘
```

versus:

```text
┌─────┐
│     │
└─────┘
```

This matters enormously in medical imaging.

You must distinguish:

```text
Image aspect ratio
```

from:

```text
Pixel spacing / physical aspect ratio
```

---

# 9. Bit Depth

Now let's answer:

> How many different values can a pixel store?

If we have (N) bits:

[
\text{Number of levels}=2^N
]

### 1-bit

[
2^1=2
]

values:

```text
0
1
```

### 8-bit

[
2^8=256
]

values:

```text
0–255
```

### 10-bit

[
2^{10}=1024
]

values:

```text
0–1023
```

### 12-bit

[
2^{12}=4096
]

### 16-bit

[
2^{16}=65536
]

---

# 10. Why Bit Depth Matters

Suppose you have:

```text
Dark
│
│
│
Bright
```

If there are only a few intensity levels, transitions may become coarse.

With more levels:

```text
0 ─────────────────────── 255
```

you can represent intensity differences more precisely.

But higher bit depth also means:

```text
More precision
      +
More memory
      +
Potentially more processing cost
```

So higher bit depth isn't automatically "better" in every context.

---

# 11. Unsigned vs Signed

This is very important for C++ and medical imaging.

### `uint8_t`

```text
0 → 255
```

### `int8_t`

```text
-128 → 127
```

### `uint16_t`

```text
0 → 65535
```

### `int16_t`

```text
-32768 → 32767
```

For example:

```cpp
std::uint8_t
```

is appropriate for many ordinary grayscale images.

But:

```cpp
std::int16_t
```

may be used for medical data where signed values matter.

---

# 12. Grayscale Image

A grayscale image normally has one channel.

Example:

```text
┌────┬────┬────┐
│ 20 │ 40 │ 60 │
├────┼────┼────┤
│ 30 │ 50 │ 70 │
├────┼────┼────┤
│ 40 │ 60 │ 80 │
└────┴────┴────┘
```

Conceptually:

[
I(x,y)
]

One pixel:

```text
Pixel = one value
```

---

# 13. Binary Image

A binary image has only two values.

For example:

```text
0 = background
1 = foreground
```

or:

```text
0 = black
255 = white
```

Example:

```text
0 0 0 1 1
0 0 1 1 1
0 1 1 1 1
```

Binary images become very important for:

* segmentation
* morphology
* connected components
* object detection

---

# 14. RGB Image

RGB has three channels:

```text
R
G
B
```

A pixel might be:

```text
(255, 0, 0)
```

meaning pure red.

Another:

```text
(0, 255, 0)
```

green.

Another:

```text
(0, 0, 255)
```

blue.

A color image can therefore be represented as:

```text
             RGB
              │
       ┌──────┼──────┐
       ↓      ↓      ↓
       R      G      B
       │      │      │
    Matrix  Matrix  Matrix
```

---

# 15. Channel Representation

Suppose we have a 2 × 2 RGB image.

```text
Pixel layout:

P00  P01
P10  P11
```

Each pixel contains:

```text
(R,G,B)
```

For example:

```text
(255,0,0)     (0,255,0)
(0,0,255)     (255,255,255)
```

Internally there are multiple possible memory arrangements.

---

# 16. Interleaved vs Planar

### Interleaved

```text
RGB RGB RGB RGB
```

Memory:

```text
R G B | R G B | R G B | R G B
```

### Planar

```text
RRRR...
GGGG...
BBBB...
```

Memory:

```text
RRRR | GGGG | BBBB
```

Both represent the same image.

But they have different performance characteristics.

---

# 17. Why Memory Layout Matters

Suppose an algorithm processes only the red channel.

With planar storage:

```text
RRRRRRRRRR
```

the data is contiguous.

With interleaved storage:

```text
RGBRGBRGBRGB
```

the CPU has to skip over G and B.

Therefore memory layout can affect:

* cache utilization
* SIMD
* vectorization
* GPU transfers
* performance

This will become important in our optimization chapters.

---

# 18. Stride

This is one of the most important concepts for image programming.

You might think:

```text
row size = width × pixel size
```

Often yes.

But not always.

An image may contain **padding**.

Suppose:

```text
Width = 5 pixels
Pixel = 1 byte
```

Raw row data:

```text
[0][1][2][3][4]
```

could theoretically be stored with:

```text
stride = 5 bytes
```

But it could also have:

```text
stride = 8 bytes
```

with padding:

```text
[0][1][2][3][4][P][P][P]
```

Therefore:

> **Stride = number of bytes from the beginning of one row to the beginning of the next row.**

This is critical.

---

# 19. Correct Memory Address Calculation

If:

```text
base = beginning of image
stride = bytes per row
pixelSize = bytes per pixel
```

then:

[
address(x,y)
============

base+y\times stride+x\times pixelSize
]

This is more general than:

[
y\times width+x
]

because real image buffers may have padding.

---

# 20. C++ Example

```cpp
std::uint8_t* row =
    buffer + y * stride;

std::uint8_t value =
    row[x];
```

For a 3-byte RGB image:

```cpp
std::uint8_t* pixel =
    buffer + y * stride + x * 3;
```

Then:

```cpp
auto r = pixel[0];
auto g = pixel[1];
auto b = pixel[2];
```

assuming that specific channel ordering.

---

# 21. Important OpenCV Detail

When you later use:

```cpp
cv::Mat
```

do not assume:

```cpp
step == width
```

Instead, OpenCV provides a row step/stride.

Conceptually:

```text
cv::Mat
│
├── rows
├── cols
├── type
├── data
└── step
```

Understanding this will prevent many bugs when integrating OpenCV with custom C++ buffers.

---

# 22. Memory Calculation

Suppose:

```text
Width  = 1920
Height = 1080
Channels = 3
Bytes/channel = 1
```

Memory:

[
1920\times1080\times3
]

[
=6,220,800
]

bytes.

Approximately:

[
5.93\text{ MiB}
]

ignoring any row padding.

---

# 23. 16-bit Medical Image

Suppose:

```text
512 × 512
1 channel
2 bytes/pixel
```

Memory:

[
512\times512\times2
]

[
=524,288
]

bytes.

Approximately:

```text
512 KiB
```

For 1000 slices:

[
524,288\times1000
]

≈ **500 MiB**.

So a CT study can easily occupy hundreds of MB in memory.

---

# 24. Dynamic Range

Dynamic range describes the range between the smallest and largest representable/useful intensity.

For 8-bit:

```text
0 ───────────────────────── 255
```

For unsigned 16-bit:

```text
0 ───────────────────────── 65535
```

For signed 16-bit:

```text
-32768 ─────────────── 32767
```

But be careful:

> **Representable range and actual image dynamic range are not necessarily the same.**

An image might be stored in 16 bits but only use a small portion of that range.

---

# 25. CT Dynamic Range

This becomes especially interesting in CT.

Suppose the reconstructed image contains values covering a wide range.

Displaying every value simultaneously on a normal monitor is not useful.

Therefore we use:

**Window Width + Window Center**

For example:

```text
        Full CT Range

-1000 ---------------------- +3000
             │
             │ display window
             ▼
          [--------]
```

Windowing maps a selected range to display intensity.

We will study the mathematics of window/level later.

---

# 26. Physical Spacing

Now comes one of the most important medical concepts.

Suppose a CT image has:

```text
512 × 512
spacing = 0.7 × 0.7 mm
```

The physical width is:

[
512\times0.7=358.4\text{ mm}
]

≈ 35.84 cm.

The image height is also approximately:

[
358.4\text{ mm}
]

So:

```text
Pixels:
512 × 512

Physical size:
358.4 mm × 358.4 mm
```

These are different concepts.

---

# 27. Slice Thickness

A CT volume also has a Z dimension.

Suppose:

```text
Pixel spacing:
0.7 × 0.7 mm

Slice spacing:
1.0 mm
```

Then voxel dimensions are approximately:

[
0.7\times0.7\times1.0\text{ mm}
]

So:

```text
Voxel
┌───────────────┐
│               │ 1.0 mm
│               │
└───────────────┘
   0.7 × 0.7
```

This is why a CT volume isn't simply:

```text
512 × 512 × 300
```

It also has **physical geometry**.

---

# 28. Medical Image = Data + Geometry

This is a concept I want you to remember permanently:

[
\boxed{
Medical\ Image = Pixel\ Data + Spatial\ Geometry + Metadata
}
]

For example:

```text
Pixel data
   +
Spacing
   +
Origin
   +
Orientation
   +
Metadata
```

Without geometry, you cannot reliably answer:

> "How far apart are these two structures in the patient?"

---

# 29. ITK's Image Model

This is why ITK has concepts such as:

```text
Image
│
├── PixelContainer
├── Region
├── Index
├── Size
├── Spacing
├── Origin
└── Direction
```

Conceptually:

```text
Index coordinates
      ↓
Physical coordinates
      ↓
Patient/world space
```

This becomes critical for:

* registration
* MPR
* segmentation
* measurement
* dose calculation
* RTSTRUCT
* RTDOSE

---

# 30. Index Space vs Physical Space

Suppose:

```text
Pixel index:
(100, 100)
```

This doesn't necessarily mean:

```text
100 mm, 100 mm
```

Instead, physical position depends on:

* origin
* spacing
* direction

A simplified 2D case:

[
P_x=O_x+xS_x
]

[
P_y=O_y+yS_y
]

where:

* (x,y) = image index
* (S) = spacing
* (O) = origin

With orientation, the relationship becomes a matrix transformation.

We'll derive that later.

---

# 31. Why This Matters in Your TPS

Suppose you have:

```text
CT
+
RTSTRUCT
```

The structure contour coordinates must correspond to the CT's physical coordinate system.

If you incorrectly treat:

```text
pixel index = physical coordinate
```

your contour can appear shifted or completely misplaced.

For example:

```text
Correct:

       CT
┌───────────────────┐
│        ███        │
│       █████       │
│        ███        │
└───────────────────┘

Incorrect coordinate handling:

┌───────────────────┐
│                   │
│ ███               │
│█████              │
│ ███               │
└───────────────────┘
```

This is not merely a visualization bug.

In medical software, coordinate errors can become **serious safety issues**.

---

# 32. C++17 Image Class

Let's improve our previous class.

```cpp
#include <cstddef>
#include <vector>
#include <stdexcept>

template<typename PixelType>
class Image
{
public:

    Image(std::size_t width,
          std::size_t height)
        : m_width(width),
          m_height(height),
          m_data(width * height)
    {
    }

    PixelType& at(std::size_t x,
                  std::size_t y)
    {
        if (x >= m_width || y >= m_height)
            throw std::out_of_range("Pixel coordinate");

        return m_data[y * m_width + x];
    }

    const PixelType& at(std::size_t x,
                        std::size_t y) const
    {
        if (x >= m_width || y >= m_height)
            throw std::out_of_range("Pixel coordinate");

        return m_data[y * m_width + x];
    }

    std::size_t width() const noexcept
    {
        return m_width;
    }

    std::size_t height() const noexcept
    {
        return m_height;
    }

private:
    std::size_t m_width{};
    std::size_t m_height{};

    std::vector<PixelType> m_data;
};
```

Usage:

```cpp
Image<std::uint8_t> image(512, 512);

image.at(100, 100) = 255;
```

---

# 33. But This Class Is Still Not Production-Ready

Why?

Because it doesn't support:

```text
stride
spacing
origin
direction
channels
ROI
image views
external memory
alignment
multidimensional images
```

Eventually we'll build a much more sophisticated abstraction.

The important thing is that you're now beginning to see how libraries such as ITK are designed.

---

# 34. Complexity

Accessing a pixel:

[
O(1)
]

For processing the entire image:

[
O(W H)
]

For a volume:

[
O(W H D)
]

Memory:

[
Memory = W\times H\times Channels\times BytesPerPixel
]

For a 3D volume:

[
Memory =
W\times H\times D\times Channels\times BytesPerPixel
]

This formula will appear repeatedly throughout the course.

---

# 35. Common Mistakes

### Mistake 1

Confusing:

```text
width × height
```

with physical dimensions.

---

### Mistake 2

Assuming every image starts at `(0,0)` in physical space.

---

### Mistake 3

Ignoring stride.

---

### Mistake 4

Assuming every pixel is one byte.

---

### Mistake 5

Assuming every image is grayscale.

---

### Mistake 6

Assuming:

```text
pixel index = patient coordinate
```

Wrong.

---

### Mistake 7

Assuming more bits automatically means better image quality.

Bit depth is only one component of image quality.

---

# 🎤 Interview Questions

### Beginner

1. What is a pixel?
2. What is image resolution?
3. What is bit depth?
4. What is a grayscale image?
5. What is an RGB image?

### Intermediate

6. What is stride?
7. What is the difference between interleaved and planar image storage?
8. What is dynamic range?
9. What is pixel spacing?
10. What is the difference between pixel and voxel?

### Advanced

11. Why can two `512 × 512` CT images have different physical dimensions?
12. Why is image stride important for performance?
13. Why can't you use `uint8_t` for all medical images?
14. Why are origin, spacing, and direction important in ITK?
15. Explain the difference between index space and physical space.

---

# 🧪 Quiz

Answer these without searching.

### Q1

How many intensity levels does a 12-bit image have?

### Q2

How much raw memory does a:

```text
1024 × 1024
8-bit
grayscale
```

image require?

### Q3

How much memory does:

```text
512 × 512
16-bit
1 channel
```

require?

### Q4

What is the difference between:

```text
pixel
pixel spacing
image resolution
```

?

### Q5

Explain:

```text
interleaved RGB
```

versus:

```text
planar RGB
```

### Q6

If:

```text
width = 100
pixel size = 2 bytes
stride = 208 bytes
```

why isn't the stride 200 bytes?

### Q7

A CT has:

```text
512 × 512
spacing = 0.8 × 0.8 mm
```

What is the physical width?

### Q8

A CT volume has:

```text
512 × 512 × 400
```

with 16-bit pixels.

Approximately how much raw memory is required?

### Q9

Why does a medical image need geometry in addition to pixel values?

### Q10 — Important

Explain this statement:

> **A medical image is not merely a matrix of numbers.**

---

# 🏠 Homework

Implement a C++17 class:

```cpp
template<typename PixelType>
class Image
```

with:

```text
✓ width
✓ height
✓ pixel access
✓ fill()
✓ memory size()
✓ stride()
✓ raw data access
```

Then test:

```cpp
Image<std::uint8_t>
Image<std::uint16_t>
Image<std::int16_t>
Image<float>
```

Also write a function:

```cpp
std::size_t calculateMemory(
    width,
    height,
    channels,
    bytesPerPixel);
```

---

# 📍 Chapter Progress

```text
CHAPTER 1 — WHAT IS IMAGE PROCESSING?

1.1 What Is Image Processing?
████████████████████ COMPLETE

1.2 Image Formation & Acquisition
████████████████████ COMPLETE

1.3 Digital Image Representation
██████████████████░░ COMPLETE

Covered:
✓ Pixel
✓ Coordinate system
✓ Width / Height
✓ Resolution
✓ Aspect ratio
✓ Bit depth
✓ Pixel types
✓ Grayscale
✓ Binary
✓ RGB
✓ Channels
✓ Interleaved / Planar
✓ Stride
✓ Memory layout
✓ Dynamic range
✓ Pixel spacing
✓ Voxel
✓ Physical space
✓ Index space
✓ ITK image model
✓ Medical image geometry
```

# 🎓 Image Processing Master Course

## Chapter 1 — What Is Image Processing?

### Lesson 1.4 — Intensity, Brightness, Contrast, Dynamic Range & Gamma

Today we move from **“what an image is”** to **“what the numbers inside an image mean and how we transform them.”**

This lesson is especially important for the medical-image editor/viewer you want to build because **brightness, contrast, gamma, and window/level are fundamental display transformations.**

---

# 1. Image Intensity

For a grayscale image we write:

[
I(x,y)
]

where (I) represents the intensity at position ((x,y)).

Example:

```text
10   20   30
40   50   60
70   80   90
```

The pixel at:

[
(x=1,y=1)
]

has:

[
I(1,1)=50
]

So intensity is simply the numerical value associated with a pixel.

---

# 2. Intensity Is Not Always "Brightness"

This distinction is important.

For a normal photograph:

```text
pixel value
    ↓
roughly related to recorded light
```

But in medical imaging:

```text
pixel value
    ↓
may represent a physical measurement
```

For CT, after DICOM rescaling:

[
HU = StoredValue \times Slope + Intercept
]

Therefore:

> **The stored pixel value and the displayed brightness are not necessarily the same thing.**

This is one of the most important concepts for your future DICOM viewer.

---

# 3. Brightness

Brightness adjustment is one of the simplest point operations.

Suppose:

```text
Original:
20 40 60
30 50 70
40 60 80
```

Add 30:

[
I'(x,y)=I(x,y)+30
]

Result:

```text
50 70 90
60 80 100
70 90 110
```

Every pixel moves upward by the same amount.

---

# 4. Brightness Transformation Graph

We can visualize the transformation:

```text
Output
255 |             /
    |            /
    |           /
    |          /
    |         /
    |        /
  0 +---------------- Input
```

For:

[
y=x+C
]

the line shifts vertically.

For positive (C):

```text
C > 0 → brighter
```

For negative (C):

```text
C < 0 → darker
```

---

# 5. Clamping

An 8-bit pixel cannot normally exceed:

[
255
]

Suppose:

```text
pixel = 240
brightness = 30
```

Mathematically:

[
240+30=270
]

But 270 cannot be represented by `uint8_t`.

So:

[
I'=\min(255,\max(0,I+C))
]

Result:

```text
255
```

This is **clamping**.

---

# 6. C++ Brightness

```cpp
#include <algorithm>
#include <cstdint>

std::uint8_t adjustBrightness(
    std::uint8_t pixel,
    int amount)
{
    int value =
        static_cast<int>(pixel) + amount;

    value = std::clamp(value, 0, 255);

    return static_cast<std::uint8_t>(value);
}
```

Notice that we perform the calculation in `int`.

That's deliberate.

---

# 7. Why Not Calculate Directly in `uint8_t`?

Because arithmetic involving small unsigned types can create unexpected results through integer promotions or overflow/conversion behavior.

For robust image processing:

```text
Read pixel
   ↓
Convert to appropriate calculation type
   ↓
Perform arithmetic
   ↓
Clamp
   ↓
Convert back
```

This pattern appears constantly in production image-processing code.

---

# 8. Contrast

Brightness moves intensity values.

Contrast changes the **spread** of intensity values.

A simple linear contrast transformation is:

[
I'(x,y)=\alpha I(x,y)
]

where:

* (\alpha>1) → increase contrast
* (0<\alpha<1) → decrease contrast

---

# 9. Example

Original:

```text
40 50 60
70 80 90
```

Use:

[
\alpha=2
]

Then:

```text
80 100 120
140 160 180
```

The differences between pixels become larger.

Therefore the image has greater contrast.

---

# 10. Brightness vs Contrast

This distinction must become automatic for you.

### Brightness

```text
20 40 60
30 50 70
```

Add 20:

```text
40 60 80
50 70 90
```

**Everything shifts.**

### Contrast

Multiply by 2:

```text
40 80 120
60 100 140
```

**The separation between intensities changes.**

---

# 11. Combined Brightness + Contrast

A common linear transformation is:

[
I'=\alpha I+\beta
]

where:

* (\alpha) controls contrast
* (\beta) controls brightness

This equation is extremely important.

Think:

```text
α → slope
β → vertical shift
```

Graphically:

```text
Output
255 |          /
    |         /
    |        /
    |       /
    |      /
  0 +---------------- Input
```

Change (\alpha):

```text
steeper / flatter
```

Change (\beta):

```text
move line up/down
```

---

# 12. Linear Transformation

General form:

[
y=ax+b
]

In image processing:

[
I'= \alpha I+\beta
]

This is a **point operation** because the output pixel depends only on the corresponding input pixel.

There is no neighborhood involved.

Compare this with convolution later:

[
g(x,y)=\sum_i\sum_j f(x-i,y-j)h(i,j)
]

where surrounding pixels matter.

This distinction is fundamental.

---

# 13. Point Operation

A point operation:

```text
Input pixel
     │
     ▼
Transformation
     │
     ▼
Output pixel
```

Example:

[
I'=I+50
]

Every pixel can be processed independently.

That means:

```text
Pixel 1 ──→ Output 1
Pixel 2 ──→ Output 2
Pixel 3 ──→ Output 3
Pixel 4 ──→ Output 4
```

This makes point operations naturally parallelizable.

---

# 14. Dynamic Range

Suppose an image uses values:

```text
20 ─────────────── 80
```

Its actual used dynamic range is:

[
80-20=60
]

But an 8-bit image can represent:

```text
0 ─────────────────────── 255
```

So much of the available range is unused.

This can make the image look low contrast.

---

# 15. Contrast Stretching

We can map the existing range:

[
[a,b]
]

to:

[
[0,255]
]

using:

[
I'=
\frac{I-a}{b-a}\times255
]

This is called **contrast stretching**.

---

# 16. Example

Suppose:

```text
minimum = 50
maximum = 150
```

and:

```text
I = 100
```

Then:

[
I'=
\frac{100-50}{150-50}\times255
]

[
=\frac{50}{100}\times255
]

[
=127.5
]

approximately:

```text
128
```

So:

```text
50 → 0
100 → 128
150 → 255
```

---

# 17. Why Contrast Stretching Is Useful

Imagine an image whose values are concentrated:

```text
50 52 55 60 62
55 58 60 65 68
```

The full 8-bit range isn't being used.

Contrast stretching expands it:

```text
0   15  30  45  55
30  40  50  65  75
```

The visual difference becomes more apparent.

---

# 18. Dynamic Range vs Bit Depth

Do not confuse these.

### Bit depth

What the storage system can represent.

For 8-bit:

[
0\ldots255
]

### Dynamic range

The useful/actual intensity range represented by the image or system.

An image can be:

```text
16-bit storage
```

but only use:

```text
1000–3000
```

of that range.

---

# 19. Gamma Correction

Now we reach one of the most important nonlinear transformations.

Gamma transformation:

[
I' = cI^\gamma
]

Usually we normalize the intensity first.

Let:

[
r=\frac{I}{I_{max}}
]

Then:

[
s=cr^\gamma
]

and convert back to the desired range.

For a normalized image:

[
0\leq r\leq1
]

---

# 20. Why Gamma?

Our visual system and display devices don't behave like simple linear systems.

Gamma transformations allow us to redistribute intensity levels.

Consider:

[
\gamma<1
]

This tends to brighten darker values.

For:

[
\gamma>1
]

dark regions are compressed and the image tends to become darker.

---

# 21. Gamma Curve

For:

[
s=r^\gamma
]

### Gamma < 1

```text
Output
1 |       ______
  |     /
  |   /
  | /
0 +---------------- Input
```

Low input values are lifted.

### Gamma = 1

```text
Output
1 |       /
  |     /
  |   /
  | /
0 +---------------- Input
```

Identity.

### Gamma > 1

```text
Output
1 |          /
  |        /
  |      /
  |   __/
0 +---------------- Input
```

Dark values are compressed.

---

# 22. Example

Suppose:

[
r=0.25
]

and:

[
\gamma=0.5
]

Then:

[
s=0.25^{0.5}
]

[
s=0.5
]

So:

```text
25% → 50%
```

The dark intensity becomes brighter.

---

# 23. C++ Gamma Correction

```cpp
#include <algorithm>
#include <cmath>
#include <cstdint>

std::uint8_t gammaCorrect(
    std::uint8_t pixel,
    double gamma)
{
    const double normalized =
        static_cast<double>(pixel) / 255.0;

    const double corrected =
        std::pow(normalized, gamma);

    const double output =
        corrected * 255.0;

    return static_cast<std::uint8_t>(
        std::clamp(
            std::lround(output),
            0L,
            255L));
}
```

This is a basic implementation.

For an entire image, we can process every pixel independently.

---

# 24. Gamma and Lookup Tables

There is an important optimization.

Gamma correction repeatedly computes:

[
pow(x,\gamma)
]

For an 8-bit image there are only:

[
256
]

possible input values.

So we can precompute:

```text
LUT[0]
LUT[1]
...
LUT[255]
```

Then:

```cpp
output = LUT[input];
```

Instead of:

```cpp
output = pow(input, gamma);
```

for every pixel.

---

# 25. Lookup Table

Conceptually:

```text
Input
  │
  ▼
┌─────────────┐
│ LUT         │
│             │
│ 0 → 0       │
│ 1 → 1       │
│ 2 → 3       │
│ ...         │
│ 255 → 255   │
└─────────────┘
  │
  ▼
Output
```

This can be dramatically faster for large images.

We will revisit LUT optimization later.

---

# 26. Medical Imaging: Why Gamma Is Different

Here's an important distinction.

In a medical viewer, you generally shouldn't confuse:

```text
Gamma
```

with:

```text
CT Window/Level
```

They are different transformations.

Gamma is nonlinear:

[
s=r^\gamma
]

Window/Level is essentially a selected intensity mapping based on:

* Window Center
* Window Width

Medical viewers often rely heavily on windowing because CT intensity values have physical meaning.

---

# 27. Window/Level

Suppose:

```text
Window Center = C
Window Width  = W
```

The display window is approximately:

[
L=C-\frac{W}{2}
]

[
U=C+\frac{W}{2}
]

where:

* (L) = lower boundary
* (U) = upper boundary

Then:

```text
Below L
   ↓
black

L → U
   ↓
mapped to display range

Above U
   ↓
white
```

---

# 28. Window/Level Graph

```text
Display
255 |              ______
    |             /
    |            /
    |           /
    |__________/
  0 +------------------------ CT value
             L      U
```

Values outside the window are clipped.

Values inside are mapped approximately linearly.

---

# 29. Window/Level Formula

For an 8-bit display:

[
D =
\begin{cases}
0 & HU \leq L\
255 & HU \geq U\
\frac{HU-L}{W}\times255 & L<HU<U
\end{cases}
]

This is extremely important.

For example:

```text
Window Center = 40
Window Width  = 400
```

Then:

[
L=40-200=-160
]

[
U=40+200=240
]

Therefore approximately:

```text
HU <= -160  → 0
HU >= 240   → 255
-160 < HU < 240 → linear mapping
```

---

# 30. Why Window/Level Is Powerful

Consider the full CT range:

```text
-1000 ───────────────────────── +3000
```

You cannot see all tissues with equal contrast simultaneously.

Instead:

### Lung window

```text
Center ≈ negative
Width   = wide
```

### Bone window

```text
Center = high
Width  = high
```

### Brain/soft tissue window

```text
Center = moderate
Width  = moderate
```

The same CT volume can therefore appear dramatically different depending on the display window.

---

# 31. This Is Critical for Your DICOM Viewer

Your eventual architecture might contain:

```text
DICOM Pixel Data
       ↓
Rescale Slope/Intercept
       ↓
Physical CT Values
       ↓
Window/Level
       ↓
8-bit Display Image
       ↓
Qt / VTK Rendering
```

Notice:

**The original CT data should not simply be overwritten when the user changes window/level.**

Instead:

```text
Original volume
      │
      ├── Window A → Display A
      │
      ├── Window B → Display B
      │
      └── Window C → Display C
```

This is a very important software architecture principle.

---

# 32. Original Data vs Display Data

Think of two layers:

```text
                    ORIGINAL DATA
                         │
              ┌──────────┼──────────┐
              ↓          ↓          ↓
           Window 1   Window 2   Window 3
              ↓          ↓          ↓
           Display    Display    Display
```

Do not destroy the original data just to change the visualization.

This concept will later extend to:

* non-destructive editing
* image pipelines
* undo/redo
* GPU rendering
* medical viewer architecture

---

# 33. C++ Window/Level Function

A basic implementation:

```cpp
#include <algorithm>
#include <cstdint>

std::uint8_t windowLevel(
    double value,
    double center,
    double width)
{
    const double lower =
        center - width / 2.0;

    const double upper =
        center + width / 2.0;

    if (value <= lower)
        return 0;

    if (value >= upper)
        return 255;

    const double normalized =
        (value - lower) / width;

    const double output =
        normalized * 255.0;

    return static_cast<std::uint8_t>(
        output);
}
```

Later we'll make this more robust and discuss:

* DICOM VOI LUT
* signed pixel representation
* rescale slope/intercept
* modality LUT
* presentation LUT
* 10/12/16-bit displays
* GPU implementation

---

# 34. One Important Architectural Rule

For medical imaging:

```text
DICOM stored value
       ↓
Modality transformation
       ↓
Physical value
       ↓
VOI / Windowing
       ↓
Presentation
```

Don't collapse these stages into one random function.

A professional architecture should keep them conceptually separate.

---

# 35. Point Operations and Parallelism

Brightness:

[
I'=I+\beta
]

Gamma:

[
I'=I^\gamma
]

Windowing:

[
I'=f(I)
]

All are point-wise.

Therefore:

```text
Pixel 0 ──→ independent
Pixel 1 ──→ independent
Pixel 2 ──→ independent
Pixel 3 ──→ independent
```

This means they are excellent candidates for:

* multithreading
* SIMD
* GPU
* CUDA

Later we'll optimize the same algorithms using those techniques.

---

# 36. Complexity

For an image containing (N) pixels:

### Brightness

[
O(N)
]

### Contrast

[
O(N)
]

### Gamma

[
O(N)
]

### Window/Level

[
O(N)
]

Extra memory if processing in-place:

[
O(1)
]

If producing another image:

[
O(N)
]

---

# 37. Memory and Cache

A simple pixel loop:

```cpp
for (std::size_t y = 0; y < height; ++y)
{
    for (std::size_t x = 0; x < width; ++x)
    {
        // process pixel
    }
}
```

usually provides good spatial locality for row-major data.

Why?

```text
Memory:

[pixel][pixel][pixel][pixel][pixel]...
   ↑
   └── CPU reads nearby values
```

This allows the CPU cache to work efficiently.

We will later compare:

```text
row-major traversal
```

against:

```text
column-major traversal
```

and benchmark them.

---

# 38. Common Mistakes

### ❌ Mistake 1

Thinking brightness and contrast are the same.

They aren't.

---

### ❌ Mistake 2

Applying gamma directly to integer values without normalization.

Correct conceptual pipeline:

```text
integer
 ↓
normalize
 ↓
gamma
 ↓
denormalize
 ↓
clamp
```

---

### ❌ Mistake 3

Destroying CT pixel data when changing window/level.

Window/level should normally be a display transformation.

---

### ❌ Mistake 4

Confusing:

```text
Gamma
```

with:

```text
Window/Level
```

---

### ❌ Mistake 5

Ignoring overflow.

For example:

```cpp
uint8_t value = 250;
value += 20;
```

should not be treated as mathematically equivalent to 270.

---

# 🎤 Interview Questions

### Beginner

1. What is image intensity?
2. What is brightness?
3. What is contrast?
4. What is dynamic range?
5. What is gamma correction?

### Intermediate

6. Explain:

[
I'=\alpha I+\beta
]

7. Why do we normalize intensity before gamma correction?
8. What is contrast stretching?
9. What is a point operation?
10. Why are point operations easy to parallelize?

### Advanced

11. Explain CT window/level mathematically.
12. Why should window/level not modify the original CT data?
13. What is the difference between gamma correction and CT windowing?
14. Why can a LUT accelerate gamma correction?
15. What is the difference between stored CT pixel value and displayed intensity?

---

# 🧪 Quiz

### Q1

For:

[
I'=I+30
]

what happens to the image?

---

### Q2

For:

[
I'=2I
]

what happens to contrast?

---

### Q3

What is the purpose of clamping?

---

### Q4

Calculate:

[
I' = 2I+10
]

for:

[
I=50
]

---

### Q5

An 8-bit image contains a pixel value of 240.

Apply brightness:

[
+30
]

What should the final value be?

---

### Q6

For:

[
r=0.25,\quad\gamma=0.5
]

calculate:

[
r^\gamma
]

---

### Q7

Given:

```text
Window Center = 50
Window Width  = 400
```

calculate:

[
L
]

and:

[
U
]

---

### Q8

What happens to a CT value below (L)?

---

### Q9

Why is this transformation:

[
I'=f(I)
]

called a point operation?

---

### Q10 — Important

Explain the difference between:

```text
Original CT data
        ↓
Window/Level
        ↓
Displayed image
```

and:

```text
Original CT data
        ↓
Modify pixels permanently
```

Why is the first architecture preferable?

---

# 🏠 Homework

Implement these **from scratch in C++17**, without OpenCV:

```text
1. adjustBrightness()
2. adjustContrast()
3. contrastStretch()
4. gammaCorrection()
5. windowLevel()
```

Then create a test image:

```text
0
25
50
75
100
125
150
175
200
225
250
```

and print the transformed values.

---

# 🔬 Implementation Challenge

Build a small command-line image-processing pipeline:

```text
Input image
    ↓
Brightness
    ↓
Contrast
    ↓
Gamma
    ↓
Output image
```

Architecture:

```text
Image
  │
  ▼
BrightnessFilter
  │
  ▼
ContrastFilter
  │
  ▼
GammaFilter
  │
  ▼
Output
```

Don't worry about file loading yet.

Use your own `Image<T>` class and synthetic image data.

---

# 📍 Progress

```text
CHAPTER 1 — WHAT IS IMAGE PROCESSING?

1.1 Fundamentals
████████████████████ COMPLETE

1.2 Image Formation & Acquisition
████████████████████ COMPLETE

1.3 Digital Image Representation
████████████████████ COMPLETE

1.4 Intensity / Brightness / Contrast / Gamma
██████████████████░░ ~90%

✓ Intensity
✓ Brightness
✓ Contrast
✓ Linear transformation
✓ Dynamic range
✓ Contrast stretching
✓ Gamma
✓ LUT concept
✓ Window/Level
✓ CT display pipeline
✓ Point operations
✓ Complexity
✓ Cache considerations
```

## Next

**Lesson 1.5 — Image Histograms & Statistical Properties**

We'll derive:

[
H(k)
]

and learn:

* histogram
* normalized histogram
* cumulative histogram
* mean
* variance
* standard deviation
* median
* percentile
* histogram interpretation
* histogram-based enhancement
* why histogram equalization works
* the mathematical foundation of **CLAHE**

That will prepare us for **Chapter 3: Core Image Processing** later.

