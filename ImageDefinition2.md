# Chapter 2 — What Is an Image?

We now move to **Chapter 2 exactly according to your index**. The Chapter 2 topics are:

1. Image definition
2. Analog image
3. Digital image
4. Pixel
5. Voxel
6. Image dimensions
7. Width / height / depth
8. Resolution
9. Spatial resolution
10. Intensity
11. Dynamic range
12. Bit depth

I will stay within these topics and build them from the fundamentals. 

---

# 2.1 Image Definition

An **image** is a representation of information that varies over spatial location.

For a 2D image, we can mathematically represent it as:

[
I(x,y)
]

where:

* (x) = horizontal position
* (y) = vertical position
* (I(x,y)) = value of the image at that position

Conceptually:

```text
             Image
               │
      ┌────────┴────────┐
      ↓                 ↓
   Position            Value
   (x, y)             I(x,y)
```

For a simple grayscale image:

```text
I(x,y)
  ↓
Intensity
```

For example:

[
I(10,20)=150
]

means:

> At position `(10,20)`, the image has an intensity value of 150.

---

# 2.2 An Image Is Data

This is one of the most important ideas in the entire course.

To a human:

```text
🩻 X-ray
```

looks like an image.

To a computer:

```text
Numbers + spatial organization
```

So:

```text
Visual Image
     ↓
Numerical Representation
     ↓
Computer Processing
```

For example:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

This matrix itself can represent a small grayscale image.

Therefore:

> **An image is not fundamentally a picture. It is structured data representing spatial information.**

---

# 2.3 Analog Image

An **analog image** is continuous rather than represented as discrete digital samples.

Imagine a photograph captured on traditional photographic film.

The physical image contains continuously varying information.

Conceptually:

```text
Real World
    ↓
Continuous physical signal
    ↓
Analog image
```

There is no requirement that the image be represented as:

```text
pixel 0
pixel 1
pixel 2
...
```

Instead, the information varies continuously across space.

---

# 2.4 Digital Image

A **digital image** represents an image using discrete numerical samples.

Conceptually:

```text
Continuous Image
       ↓
Sampling
       ↓
Quantization
       ↓
Digital Image
```

The digital image is represented using:

* discrete spatial locations
* numerical intensity values

For example:

```text
+----+----+----+
| 10 | 20 | 30 |
+----+----+----+
| 40 | 50 | 60 |
+----+----+----+
| 70 | 80 | 90 |
+----+----+----+
```

Each location contains a numerical value.

---

# 2.5 Analog vs Digital

| Feature             | Analog Image           | Digital Image          |
| ------------------- | ---------------------- | ---------------------- |
| Representation      | Continuous             | Discrete               |
| Spatial values      | Continuous             | Sampled                |
| Intensity           | Continuous             | Quantized              |
| Computer processing | Difficult directly     | Natural                |
| Storage             | Physical/analog medium | Digital memory/storage |

A digital image therefore gives us something computers can directly manipulate.

---

# 2.6 Pixel

A **pixel** is a basic element of a 2D digital image.

The word comes from:

> **Picture Element**

Consider:

```text
+-----+-----+-----+
|  10 |  20 |  30 |
+-----+-----+-----+
|  40 |  50 |  60 |
+-----+-----+-----+
|  70 |  80 |  90 |
+-----+-----+-----+
```

There are:

[
3\times3=9
]

pixels.

Each pixel has:

1. a location
2. one or more values

For a grayscale image:

```text
Pixel
 ├── Location
 │    └── (x,y)
 │
 └── Intensity
      └── e.g. 120
```

---

# 2.7 Pixel Location

Suppose:

[
I(x,y)
]

and:

[
I(2,3)=150
]

Then:

```text
x = 2
y = 3
intensity = 150
```

The important thing is to distinguish:

```text
WHERE?
 ↓
(x,y)

WHAT VALUE?
 ↓
I(x,y)
```

This distinction becomes critical when we later perform:

* filtering
* segmentation
* registration
* measurements

---

# 2.8 Voxel

A **voxel** is the 3D equivalent of a pixel.

The word comes from:

> **Volume Element**

A 2D image:

```text
Pixel
 ↓
(x,y)
```

A 3D image:

```text
Voxel
 ↓
(x,y,z)
```

Mathematically:

[
I(x,y,z)
]

For example:

[
I(100,200,50)
]

represents the value at a particular 3D location.

---

# 2.9 Pixel vs Voxel

| Concept     | Pixel        | Voxel          |
| ----------- | ------------ | -------------- |
| Dimension   | 2D           | 3D             |
| Coordinates | x, y         | x, y, z        |
| Represents  | Area element | Volume element |
| Typical use | 2D image     | CT/MRI volume  |

Conceptually:

```text
2D

+---+---+---+
| P | P | P |
+---+---+---+
| P | P | P |
+---+---+---+
```

versus:

```text
3D

+---------+
|         |
|  Voxel  |
|         |
+---------+
```

A CT scan is generally treated as a **3D volume**, so voxels become fundamental.

---

# 2.10 Image Dimensions

Image dimensions tell us the size of the image.

For a 2D image:

[
Width \times Height
]

For example:

[
512\times512
]

means:

```text
Width  = 512
Height = 512
```

Total pixels:

[
512\times512
============

262144
]

pixels.

---

# 2.11 Width

**Width** describes the number of columns in a digital image.

Example:

```text
Width = 5

+---+---+---+---+---+
|   |   |   |   |   |
+---+---+---+---+---+
```

There are 5 columns.

---

# 2.12 Height

**Height** describes the number of rows.

Example:

```text
Height = 4

+---+---+---+
|   |   |   |
+---+---+---+
|   |   |   |
+---+---+---+
|   |   |   |
+---+---+---+
|   |   |   |
+---+---+---+
```

There are 4 rows.

Therefore:

[
Width\times Height
==================

# 3\times4

12
]

pixels.

---

# 2.13 Depth

For a 3D volume we additionally have **depth**.

Example:

[
512\times512\times300
]

means:

```text
Width  = 512
Height = 512
Depth  = 300
```

Total voxels:

[
512\times512\times300
=====================

78,643,200
]

voxels.

This is why medical image datasets can become very large.

---

# 2.14 2D vs 3D

### 2D

[
I(x,y)
]

```text
        Width
   ─────────────→
   ┌─────────────┐
   │             │
   │    IMAGE    │ Height
   │             │
   └─────────────┘
```

### 3D

[
I(x,y,z)
]

```text
             z
             ↑
             │
             │
       ┌───────────┐
      /           /|
     /           / |
    └───────────┘  |
    |           |  |
    |           | /
    |           |/
    └───────────┘
```

The third dimension represents depth through the volume.

---

# 2.15 Resolution

Resolution describes the amount of spatial detail that an image can represent.

However, **resolution is not simply the same thing as image dimensions**.

For example:

```text
Image A
512 × 512

Image B
512 × 512
```

They have identical dimensions.

But they could represent different physical areas or have different physical pixel spacing.

Therefore:

> **Number of pixels alone does not completely describe spatial resolution.**

This is particularly important in medical imaging.

---

# 2.16 Pixel Spacing and Physical Size

Suppose an image has:

```text
512 × 512 pixels
```

and each pixel represents:

[
0.5\text{ mm}\times0.5\text{ mm}
]

Then the physical width is:

[
512\times0.5
============

256\text{ mm}
]

and the physical height is also:

[
256\text{ mm}
]

So:

```text
512 pixels
     ↓
0.5 mm/pixel
     ↓
256 mm physical size
```

This is why medical imaging cannot rely only on pixel counts.

---

# 2.17 Spatial Resolution

**Spatial resolution** describes the ability of an imaging system to distinguish small spatial details.

Imagine two tiny structures:

```text
Object A      Object B

   ██            ██
   ██            ██
```

If the imaging system has sufficient spatial resolution, the two objects can be distinguished.

If the resolution is poor:

```text
████████
```

they may appear as one structure.

Therefore:

> Higher spatial resolution generally means the system can distinguish finer spatial details.

But there are tradeoffs involving:

* noise
* acquisition time
* radiation dose
* computational cost

These will become important when we study actual medical modalities.

---

# 2.18 Resolution vs Dimensions

This distinction is critical.

Suppose:

### Image A

[
512\times512
]

with:

[
1\text{ mm/pixel}
]

Physical width:

[
512\text{ mm}
]

### Image B

[
512\times512
]

with:

[
0.5\text{ mm/pixel}
]

Physical width:

[
256\text{ mm}
]

Both have:

```text
512 × 512
```

but their physical sampling is different.

Therefore:

```text
Image dimensions
      ≠
Spatial resolution
```

---

# 2.19 Intensity

**Intensity** represents the numerical value associated with a pixel or voxel.

For a grayscale image:

```text
Low intensity
      ↓
Dark

High intensity
      ↓
Bright
```

For an 8-bit grayscale image:

[
0\rightarrow255
]

typically means:

```text
0   → black
255 → white
```

with intermediate values representing gray levels.

---

# 2.20 Intensity Is Not Always Brightness

This distinction becomes very important in medical imaging.

In an ordinary image:

```text
Pixel value
≈
display brightness
```

But in medical imaging:

```text
Pixel value
      ↓
Physical / reconstructed quantity
      ↓
Display transformation
      ↓
Screen brightness
```

For example, CT values are related to attenuation and are commonly converted into **Hounsfield Units**.

So:

> A medical image's stored value does not necessarily equal what your eye sees on the monitor.

---

# 2.21 Dynamic Range

Dynamic range describes the range of values an image representation can contain or distinguish.

For an 8-bit unsigned image:

[
0\leq I\leq255
]

Therefore:

[
Dynamic\ Range = 256\ levels
]

because:

[
2^8=256
]

For a 16-bit unsigned image:

[
2^{16}=65,536
]

possible values.

So:

```text
8-bit
 ↓
256 levels

16-bit
 ↓
65,536 levels
```

---

# 2.22 Bit Depth

Bit depth tells us how many bits are used to represent a pixel value.

For (n) bits:

[
Number\ of\ possible\ values=2^n
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

This can represent a binary image.

### 8-bit

[
2^8=256
]

values:

```text
0–255
```

### 12-bit

[
2^{12}=4096
]

values:

```text
0–4095
```

### 16-bit

[
2^{16}=65,536
]

values:

```text
0–65,535
```

---

# 2.23 Why Medical Images Use Higher Bit Depth

Consider an 8-bit image:

```text
0 ───────────────────── 255
```

There are only 256 possible levels.

Medical imaging often needs to preserve much finer intensity differences.

For example:

```text
Medical measurement
       ↓
Small intensity differences
       ↓
Important information
       ↓
Higher precision representation
```

Therefore medical images may use:

* 12-bit
* 16-bit
* signed 16-bit
* floating-point representations

depending on the modality and processing stage.

---

# 2.24 Signed vs Unsigned

This will become very important later.

### Unsigned 16-bit

Typically:

[
0\ldots65535
]

### Signed 16-bit

Typically:

[
-32768\ldots32767
]

Why might negative values matter?

Because some medical image representations can contain values below zero.

For example, CT data expressed in Hounsfield Units can include values below zero.

This is why **pixel data type** is an important concept.

---

# 2.25 Image Size in Memory

Suppose we have:

[
512\times512
]

pixels.

That is:

[
262,144
]

pixels.

If each pixel uses 8 bits:

[
262,144\times1
==============

262,144
]

bytes.

Approximately:

[
256\text{ KB}
]

If each pixel uses 16 bits:

[
262,144\times2
==============

524,288
]

bytes.

Approximately:

[
512\text{ KB}
]

So:

```text
Higher bit depth
      ↓
More memory per pixel
      ↓
Larger image
```

---

# 2.26 3D Medical Volume Memory

Consider:

[
512\times512\times300
]

voxels.

We calculated:

[
78,643,200
]

voxels.

With 16-bit data:

[
78,643,200\times2
=================

157,286,400
]

bytes.

Approximately:

[
150\text{ MiB}
]

for the voxel array alone.

Real medical software has additional memory requirements for:

* metadata
* buffers
* processed images
* segmentation masks
* rendering
* caches
* temporary calculations

This is one reason memory management becomes a major topic later in the course.

---

# 2.27 Pixel → Voxel → Volume

Keep this hierarchy in mind:

```text
2D

Pixel
  ↓
Image
```

and:

```text
3D

Voxel
  ↓
Volume
```

For example:

```text
        CT Volume
            │
     ┌──────┼──────┐
     ↓      ↓      ↓
  Slice 1 Slice 2 Slice 3 ...
     │
     ↓
   Pixels
```

So a CT volume can be thought of as a stack of image slices, while the complete dataset is a 3D voxel volume.

---

# 2.28 The Most Important Medical Imaging Distinction

A medical image has at least two different kinds of information:

```text
              Medical Image
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      Image Data          Spatial Information
          │                   │
       Intensity         Position/Geometry
          │                   │
       Pixel/Voxel       Spacing/Origin/etc.
```

For example:

```text
Voxel value
     +
Voxel location
     +
Voxel spacing
     +
Image orientation
```

together provide meaningful spatial information.

Later, your index will introduce coordinate systems, origin, spacing, direction, and orientation in detail.

---

# 2.29 Example: CT

Suppose a CT volume has:

```text
Dimensions:
512 × 512 × 300

Pixel spacing:
0.7 mm × 0.7 mm

Slice spacing:
1.0 mm
```

Then the volume has:

```text
512 × 512
     ↓
2D slice

300 slices
     ↓
3D volume
```

The physical dimensions are approximately:

[
512\times0.7=358.4\text{ mm}
]

in the first direction,

[
512\times0.7=358.4\text{ mm}
]

in the second direction,

and approximately:

[
300\times1.0=300\text{ mm}
]

through the slice direction.

Notice how:

```text
Digital dimensions
+
Physical spacing
=
Physical dimensions
```

This relationship is fundamental to medical image processing.

---

# 2.30 C++ Representation

A simple 2D image can be represented as:

```cpp
#include <vector>
#include <cstdint>

int main()
{
    const std::size_t width = 512;
    const std::size_t height = 512;

    std::vector<std::uint8_t> image(
        width * height
    );

    return 0;
}
```

Here:

```text
width  = 512
height = 512
```

and:

```text
total pixels = width × height
```

The image is stored as a one-dimensional memory block:

```text
[ p0 p1 p2 p3 p4 ... ]
```

while we logically interpret it as:

```text
row 0
row 1
row 2
...
```

This memory representation becomes extremely important when we later study performance.

---

# 2.31 Pixel Access

If the image is stored row-by-row, a common conceptual mapping is:

[
index=y\times width+x
]

For example:

```text
width = 512
x = 10
y = 20
```

then:

[
index=20\times512+10
]

[
=10250
]

So:

```text
(x,y)
 ↓
linear memory index
```

This simple equation becomes extremely important for image-processing implementation.

---

# 2.32 Python Representation

With NumPy:

```python
import numpy as np

image = np.zeros((512, 512), dtype=np.uint8)

print(image.shape)
print(image.dtype)
```

Conceptually:

```text
shape
 ↓
(512, 512)

dtype
 ↓
uint8
```

For a 3D volume:

```python
volume = np.zeros(
    (300, 512, 512),
    dtype=np.int16
)
```

Conceptually:

```text
300 slices
512 rows
512 columns
```

The exact axis convention depends on the library and application, which is why coordinate conventions must always be handled carefully.

---

# 2.33 Chapter 2 Mental Model

You should now have this structure:

```text
IMAGE
 │
 ├── 2D
 │    └── Pixel
 │         ├── Position
 │         └── Intensity
 │
 └── 3D
      └── Voxel
           ├── Position
           └── Intensity
```

Then:

```text
Image
 │
 ├── Dimensions
 │    ├── Width
 │    ├── Height
 │    └── Depth
 │
 ├── Resolution
 │
 ├── Spatial resolution
 │
 ├── Intensity
 │
 ├── Dynamic range
 │
 └── Bit depth
```

---

# 2.34 Critical Relationships

Memorize these relationships conceptually:

### Bit depth

[
n\ bits
\Rightarrow
2^n\ possible\ levels
]

### Image size

[
Width\times Height
==================

Number\ of\ pixels
]

### Volume size

[
Width\times Height\times Depth
==============================

Number\ of\ voxels
]

### Physical size

[
Number\ of\ pixels\times Pixel\ Spacing
=======================================

Physical\ length
]

### 2D image

[
I(x,y)
]

### 3D image

[
I(x,y,z)
]

---

# 2.35 Chapter 2 Knowledge Check

Answer these before moving to Chapter 3.

### Fundamental

1. What is an image?
2. What is an analog image?
3. What is a digital image?
4. What is a pixel?
5. What is a voxel?
6. What is the difference between a pixel and a voxel?

### Dimensions

7. What does `512 × 512` mean?
8. What does `512 × 512 × 300` mean?
9. How many pixels are in a `1024 × 768` image?
10. How many voxels are in a `256 × 256 × 100` volume?

### Resolution

11. What is resolution?
12. What is spatial resolution?
13. Why don't image dimensions alone completely describe spatial resolution?
14. Why is physical pixel spacing important in medical imaging?

### Intensity

15. What is image intensity?
16. Is image intensity always equivalent to screen brightness?
17. Why can medical image values have physical meaning?

### Bit depth

18. How many possible values can an 8-bit image represent?
19. How many values can a 12-bit image represent?
20. How many values can a 16-bit image represent?
21. What is the difference between signed and unsigned data?

### Practical calculation

A CT volume has:

```text
512 × 512 × 300
```

with:

```text
0.5 mm × 0.5 mm × 1 mm
```

spacing.

Calculate:

22. Total number of voxels.
23. Physical width.
24. Physical height.
25. Approximate physical depth.
26. Memory required if each voxel is `16-bit`.

---

## Chapter 2 Mini Exercise

Create a small image:

```text
[ 10  20  30  40
  50  60  70  80
  90 100 110 120 ]
```

Determine:

* width
* height
* number of pixels
* minimum intensity
* maximum intensity
* center-region values
* bit depth required if values are stored as `uint8`
* memory required for the pixel data

Then explain:

> **Why is a CT volume fundamentally different from a normal 2D photograph?**

---

**Chapter 2 is complete.**
The next chapter in your exact index is **Chapter 3 — Digital Image Representation**. 
