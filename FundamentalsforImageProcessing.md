# Level 0 → Module 1 → Chapter 2: C++ Fundamentals for Image Processing

## 1. Chapter Overview

Before processing medical images, you need strong C++ fundamentals because an image is ultimately stored and manipulated as **data in memory**.

In this chapter, we focus on how C++ represents image data and the fundamental concepts needed for image-processing algorithms.

---

# 2. Why C++ Fundamentals Matter for Image Processing

Consider a grayscale image:

```text
Width = 512 pixels
Height = 512 pixels
```

Total pixels:

$$
512 \times 512 = 262,144
$$

Each pixel must be stored in memory.

Conceptually:

```text
Image
 │
 ├── Width
 ├── Height
 ├── Pixel Data
 └── Metadata
```

An image-processing program constantly performs operations like:

```text
Read Pixel
   ↓
Modify Pixel
   ↓
Store Result
```

For example:

```text
Original Pixel = 100
Brightness +20
Result = 120
```

---

# 3. Core C++ Concepts Required

For image processing, you must understand:

1. Variables and data types
2. Arrays
3. Loops
4. Functions
5. Pointers
6. References
7. Dynamic memory
8. Structures and classes
9. `const`
10. Basic performance considerations

---

# 4. Data Types and Medical Images

Different medical images use different data types.

## 8-bit Unsigned Integer

```cpp
unsigned char pixel;
```

Range:

$$
0 \text{ to } 255
$$

Example:

```text
0     = Black
255   = White
128   = Gray
```

---

## 16-bit Unsigned Integer

```cpp
unsigned short pixel;
```

Range:

$$
0 \text{ to } 65535
$$

Medical images often require more precision than ordinary photographs.

---

## Signed Integer

Some medical image data can contain negative values.

For example, CT processing involves intensity values where negative values are meaningful.

```cpp
short value;
```

---

## Floating Point

Used in:

* Image processing calculations
* Registration
* Interpolation
* AI preprocessing
* Dose calculation

```cpp
float value;
double preciseValue;
```

---

# 5. Simple Image Representation

A simple 2D grayscale image:

```text
10   20   30
40   50   60
70   80   90
```

We can represent it using a 2D array:

```cpp
int image[3][3] =
{
    {10, 20, 30},
    {40, 50, 60},
    {70, 80, 90}
};
```

Access:

```cpp
image[row][column];
```

Example:

```cpp
int pixel = image[1][2];
```

Result:

```text
60
```

---

# 6. Image Processing Requires Loops

Suppose we want to increase brightness.

Formula:

$$
Output = Input + Brightness
$$

Example:

```text
Input = 100
Brightness = 20

Output = 120
```

## C++ Implementation

```cpp
#include <iostream>

int main()
{
    const int width = 3;
    const int height = 3;

    int image[height][width] =
    {
        {10, 20, 30},
        {40, 50, 60},
        {70, 80, 90}
    };

    const int brightness = 20;

    for (int y = 0; y < height; ++y)
    {
        for (int x = 0; x < width; ++x)
        {
            image[y][x] += brightness;
        }
    }

    return 0;
}
```

---

# 7. Understanding the Image Loop

This is one of the most important patterns:

```cpp
for (int y = 0; y < height; ++y)
{
    for (int x = 0; x < width; ++x)
    {
        // Process pixel
    }
}
```

Conceptually:

```text
y = Row

x = Column
```

Traversal:

```text
(0,0) → (1,0) → (2,0)
  ↓
(0,1) → (1,1) → (2,1)
  ↓
(0,2) → (1,2) → (2,2)
```

The processing complexity is:

$$
O(width \times height)
$$

For a 3D image:

$$
O(width \times height \times depth)
$$

---

# 8. Functions for Image Processing

Instead of writing everything inside `main()`, create reusable functions.

```cpp
void increaseBrightness(
    int image[][3],
    int width,
    int height,
    int brightness)
{
    for (int y = 0; y < height; ++y)
    {
        for (int x = 0; x < width; ++x)
        {
            image[y][x] += brightness;
        }
    }
}
```

Benefits:

* Reusability
* Better organization
* Easier testing
* Easier debugging

---

# 9. Image Processing Pipeline in C++

A simple architecture:

```text
Load Image
    ↓
Process Image
    ↓
Display Image
```

C++ representation:

```cpp
Image image = loadImage();

applyFilter(image);

displayImage(image);
```

Later, this concept becomes much more advanced with:

* OpenCV
* ITK pipelines
* VTK pipelines
* Multithreading

---

# 10. Pointers and Image Memory

An image is often stored in contiguous memory.

Example:

```cpp
unsigned char* imageData;
```

Memory:

```text
Address

1000 → Pixel 0
1001 → Pixel 1
1002 → Pixel 2
1003 → Pixel 3
```

Access:

```cpp
imageData[index];
```

For a 2D image:

$$
index = y \times width + x
$$

---

## Example

For:

```text
Width = 512
x = 10
y = 20
```

$$
index = 20 \times 512 + 10
$$

$$
index = 10250
$$

Therefore:

```cpp
pixel = imageData[y * width + x];
```

This formula is extremely important.

---

# 11. 2D Image Stored as 1D Memory

Even though an image looks like:

```text
10  20  30
40  50  60
70  80  90
```

Memory may actually look like:

```text
10 | 20 | 30 | 40 | 50 | 60 | 70 | 80 | 90
```

For:

```text
width = 3
```

The mapping is:

```text
(0,0) → 0
(1,0) → 1
(2,0) → 2

(0,1) → 3
(1,1) → 4
(2,1) → 5

(0,2) → 6
(1,2) → 7
(2,2) → 8
```

Formula:

$$
index = y \times width + x
$$

---

# 12. Dynamic Memory

For images whose size is known only at runtime:

```cpp
int width;
int height;
```

Older style:

```cpp
unsigned char* image =
    new unsigned char[width * height];
```

Cleanup:

```cpp
delete[] image;
```

But modern C++ prefers RAII containers such as:

```cpp
#include <vector>

std::vector<unsigned char> image(
    width * height
);
```

Benefits:

* Automatic cleanup
* Safer memory management
* Fewer memory leaks

Access:

```cpp
image[y * width + x];
```

---

# 13. Simple Image Class

A basic image object:

```cpp
#include <vector>
#include <cstdint>

class Image
{
public:
    Image(int width, int height)
        : m_width(width),
          m_height(height),
          m_data(width * height)
    {
    }

    std::uint8_t& pixel(int x, int y)
    {
        return m_data[y * m_width + x];
    }

    const std::uint8_t& pixel(int x, int y) const
    {
        return m_data[y * m_width + x];
    }

    int width() const
    {
        return m_width;
    }

    int height() const
    {
        return m_height;
    }

private:
    int m_width;
    int m_height;

    std::vector<std::uint8_t> m_data;
};
```

Usage:

```cpp
Image image(512, 512);

image.pixel(10, 20) = 150;
```

---

# 14. Why `const` Is Important

Suppose a function should only read an image:

```cpp
void analyzeImage(const Image& image);
```

This means:

```text
const  → Do not modify image
&      → Do not copy image
```

For large medical volumes, this is important.

Bad:

```cpp
void process(Image image);
```

This may copy a huge image.

Better:

```cpp
void process(const Image& image);
```

---

# 15. Common Image Processing Data Flow

```text
Input Buffer
     ↓
Read Pixel
     ↓
Mathematical Operation
     ↓
Output Buffer
```

Example:

$$
output(x,y) = input(x,y) + brightness
$$

C++:

```cpp
output[index] =
    input[index] + brightness;
```

---

# 16. Important Problem: Overflow

Suppose:

```text
Pixel = 250
Brightness = 20
```

Mathematically:

$$
250 + 20 = 270
$$

But an 8-bit unsigned value cannot represent 270.

Maximum:

$$
255
$$

Therefore we use **clamping**:

$$
Output =
\min(255, Input + Brightness)
$$

Example:

```cpp
int value = input + brightness;

if (value > 255)
{
    value = 255;
}
```

Modern C++:

```cpp
#include <algorithm>

value = std::clamp(value, 0, 255);
```

Result:

```text
250 + 20 = 270

Clamped = 255
```

---

# 17. Algorithm: Brightness Adjustment

## Input

```text
Image
Brightness Value
```

## Processing

For every pixel:

$$
output = input + brightness
$$

Then:

$$
output = clamp(output, min, max)
$$

## Output

Brightness-adjusted image.

### Pseudocode

```text
FOR every pixel
    value = pixel + brightness

    IF value > maximum
        value = maximum

    IF value < minimum
        value = minimum

    outputPixel = value
END FOR
```

### Complexity

For an image of width \(W\) and height \(H\):

**Time:**

$$
O(W \times H)
$$

**Extra memory:**

* In-place processing: \(O(1)\)
* Separate output image: \(O(W \times H)\)

---

# 18. C++ Implementation From Scratch

```cpp
#include <vector>
#include <algorithm>
#include <cstdint>

void adjustBrightness(
    const std::vector<std::uint8_t>& input,
    std::vector<std::uint8_t>& output,
    int brightness)
{
    output.resize(input.size());

    for (std::size_t i = 0; i < input.size(); ++i)
    {
        const int value =
            static_cast<int>(input[i]) + brightness;

        output[i] =
            static_cast<std::uint8_t>(
                std::clamp(value, 0, 255)
            );
    }
}
```

### Why use `int` temporarily?

Because:

```cpp
uint8_t + int
```

must be calculated safely before converting back to an 8-bit value.

---

# 19. Medical Imaging Consideration

In medical imaging, blindly clamping to:

```text
0 → 255
```

is often incorrect.

For example, CT data may have:

* Higher bit depth
* Signed values
* Rescaling information

Therefore:

```text
Normal Image Processing
        ≠
Always Medical Image Processing
```

Medical image processing must consider:

* Original data type
* Bit depth
* Signed/unsigned representation
* Physical meaning of intensity
* DICOM metadata

We will study this deeply in later chapters.

---

# 20. Performance Considerations

For a:

```text
512 × 512 image
```

pixels:

$$
262,144
$$

For a volume:

```text
512 × 512 × 500
```

voxels:

$$
131,072,000
$$

Every unnecessary copy can consume significant memory.

Therefore prefer:

```cpp
const Image&
```

when reading large images.

Also consider:

* Contiguous memory
* Cache-friendly traversal
* Avoiding unnecessary allocations
* Multithreading for large volumes

---

# 21. Common Mistakes

## Mistake 1: Wrong Index Calculation

Wrong:

```cpp
index = x * width + y;
```

Usually correct for row-major storage:

```cpp
index = y * width + x;
```

---

## Mistake 2: Buffer Overflow

Wrong:

```cpp
for (int x = 0; x <= width; ++x)
```

Correct:

```cpp
for (int x = 0; x < width; ++x)
```

---

## Mistake 3: Ignoring Pixel Type

An 8-bit image and a 16-bit medical image cannot always use the same assumptions.

---

## Mistake 4: Copying Large Images

Avoid unnecessary:

```cpp
Image image2 = image1;
```

when processing large medical volumes.

---

# 22. Debugging Tips

When an image looks corrupted, check:

### Dimensions

```text
Width
Height
Depth
```

### Indexing

```text
index = y × width + x
```

### Data Type

```text
uint8_t?
int16_t?
float?
```

### Buffer Size

Expected:

$$
width \times height \times bytesPerPixel
$$

For 3D:

$$
width \times height \times depth \times bytesPerVoxel
$$

---

# 23. C++ Fundamentals → Medical Imaging Connection

```text
C++ Arrays
    ↓
Image Buffers

Pointers
    ↓
Pixel/Voxel Memory

Loops
    ↓
Image Algorithms

Functions
    ↓
Processing Operations

Classes
    ↓
Image Objects

Multithreading
    ↓
Large Volume Processing

Memory Management
    ↓
High-Performance Medical Software
```

---

# Interview Questions

## Basic

1. How is a 2D image stored in memory?
2. What is the formula for converting `(x, y)` into a linear index?
3. Why are loops heavily used in image processing?
4. Why is `std::vector` safer than raw `new[]`?
5. What is pixel overflow?

## Intermediate

1. Why should large images usually be passed using `const&`?
2. What is row-major memory layout?
3. How would you calculate image buffer size?
4. What happens if image indexing is incorrect?
5. What is the difference between in-place and out-of-place processing?

## Advanced

1. How would you design a generic 2D/3D image container?
2. How would you optimize traversal for CPU cache efficiency?
3. How would you process a 500 MB volume without excessive copying?
4. How would you parallelize pixel processing safely?
5. How would you support multiple pixel data types?

---

# Exercises

### Beginner

1. Create a `3 × 3` image using an array.
2. Print every pixel.
3. Increase every pixel by 10.
4. Implement brightness clamping.

### Intermediate

1. Create an `Image` class using `std::vector`.
2. Implement `getPixel(x, y)`.
3. Implement `setPixel(x, y, value)`.
4. Implement brightness adjustment.

### Advanced

1. Create a templated image class:

```cpp
Image<std::uint8_t>
Image<std::int16_t>
Image<float>
```

2. Extend it for 3D volume data.

---

# Key Takeaways

* An image is fundamentally a structured buffer in memory.
* The basic 2D index formula is:

$$
\boxed{index = y \times width + x}
$$

* Medical image processing requires careful data-type handling.
* `std::vector` provides safer memory management than raw arrays.
* Avoid unnecessary copies of large medical images.
* `const` references are important for performance.
* Most image algorithms involve traversing every pixel or voxel.
* C++ fundamentals are the foundation for high-performance medical imaging software.

---

## Current Progress

* **Completed:** Chapter 1
* **Current:** **Level 0 → Module 1 → Chapter 2: C++ Fundamentals for Image Processing**
* **Next:** **Chapter 3: Modern C++ for Medical Software**

When you're ready, say **Next** to continue to Chapter 3.
