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

**We are NOT moving to Chapter 2 yet.**

First answer the **10 quiz questions** and, if possible, submit your C++ `Image` class. I will review your answers like a professor, correct misconceptions, give you a score, and then continue **Chapter 1** with the next lesson rather than jumping ahead.
