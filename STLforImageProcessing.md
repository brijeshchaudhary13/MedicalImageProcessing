# Level 0 → Module 1 → Chapter 7: STL for Image Processing

## 1. Chapter Overview

**STL (Standard Template Library)** is extremely useful in image processing because images involve:

* Large pixel buffers
* Dynamic memory
* Searching
* Sorting
* Statistical calculations
* Iteration
* Algorithms
* Data structures

In medical image processing, STL is commonly used for managing image data efficiently.

```text
Medical Image
      ↓
Pixel / Voxel Buffer
      ↓
STL Containers
      ↓
STL Algorithms
      ↓
Processing Results
```

---

# 2. Main Parts of STL

STL can be understood through four major components:

```text
STL
│
├── Containers
│
├── Iterators
│
├── Algorithms
│
└── Function Objects / Utilities
```

For image processing, the most important are:

* `std::vector`
* `std::array`
* Iterators
* `<algorithm>`
* `<numeric>`
* `std::unordered_map`
* `std::map`

---

# 3. `std::vector` — Most Important Container

For image processing, `std::vector` is one of the most useful STL containers.

```cpp
#include <vector>
#include <cstdint>

std::vector<std::uint16_t> image;
```

A vector stores elements dynamically.

Example:

```cpp
std::vector<std::uint8_t> pixels =
{
    10,
    20,
    30,
    40
};
```

Conceptually:

```text
pixels

Index:   0   1   2   3
Value:  10  20  30  40
```

---

# 4. Why `std::vector` Is Useful for Images

Image dimensions may not be known until runtime.

For example:

```text
Image A → 512 × 512

Image B → 1024 × 1024

CT Volume → 512 × 512 × 300
```

Therefore:

```cpp
std::vector<std::uint16_t> pixels(
    width * height
);
```

For a 3D volume:

```cpp
std::vector<std::int16_t> volume(
    width * height * depth
);
```

---

# 5. Continuous Memory

A vector stores its elements contiguously.

```text
Vector:

[10][20][30][40][50]
```

This is useful for image processing because:

* Cache-friendly access
* Easy raw buffer access
* Compatible with many libraries
* Efficient sequential traversal

Get the underlying buffer:

```cpp
std::uint16_t* buffer =
    image.data();
```

---

# 6. Basic Vector Operations

## Create

```cpp
std::vector<int> values;
```

## Add element

```cpp
values.push_back(10);
```

## Size

```cpp
std::size_t count =
    values.size();
```

## Access

```cpp
int value = values[0];
```

## Checked access

```cpp
int value = values.at(0);
```

## Remove all elements

```cpp
values.clear();
```

---

# 7. Image Buffer Using `std::vector`

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>

class Image
{
public:
    Image(
        std::size_t width,
        std::size_t height)
        : m_width(width),
          m_height(height),
          m_pixels(width * height)
    {
    }

    std::uint16_t& pixel(
        std::size_t x,
        std::size_t y)
    {
        return m_pixels[
            y * m_width + x
        ];
    }

private:
    std::size_t m_width;
    std::size_t m_height;

    std::vector<std::uint16_t> m_pixels;
};
```

---

# 8. `std::array`

Use `std::array` when the size is fixed.

```cpp
#include <array>

std::array<float, 3> spacing =
{
    1.0f,
    1.0f,
    2.5f
};
```

Medical example:

```text
Voxel Spacing

X = 1.0 mm
Y = 1.0 mm
Z = 2.5 mm
```

Another example:

```cpp
std::array<float, 3> position;
```

Useful for:

* Coordinates
* Spacing
* RGB values
* Fixed-size matrices
* Direction vectors

---

# 9. `std::vector` vs `std::array`

| Feature           | `std::vector` | `std::array`                                   |
| ----------------- | ------------- | ---------------------------------------------- |
| Size              | Dynamic       | Fixed                                          |
| Heap allocation   | Usually       | No extra dynamic allocation by itself          |
| Resize            | Yes           | No                                             |
| Contiguous memory | Yes           | Yes                                            |
| Image buffer      | Excellent     | Usually not practical for runtime-sized images |
| Coordinates       | Good          | Excellent                                      |

Example:

```text
Image pixels
     ↓
std::vector
```

```text
3D coordinate
     ↓
std::array
```

---

# 10. Iterators

An iterator allows us to traverse a container.

Example:

```cpp
std::vector<int> values =
{
    10,
    20,
    30
};

for (
    auto it = values.begin();
    it != values.end();
    ++it)
{
    std::cout << *it << '\n';
}
```

Concept:

```text
begin()

10 → 20 → 30

               end()
```

---

# 11. Range-Based Loop

Modern C++ provides a simpler approach:

```cpp
for (const auto& pixel : image)
{
    std::cout << pixel << '\n';
}
```

For modifying pixels:

```cpp
for (auto& pixel : image)
{
    pixel = 100;
}
```

For image processing, range-based loops are often clean and readable.

---

# 12. STL Algorithms

Include:

```cpp
#include <algorithm>
```

Common algorithms:

```text
std::min
std::max
std::min_element
std::max_element
std::find
std::fill
std::transform
std::count
std::sort
std::clamp
```

These are extremely useful for image processing.

---

# 13. Finding Minimum Pixel Value

```cpp
#include <algorithm>

auto minimum =
    std::min_element(
        image.begin(),
        image.end()
    );
```

Access value:

```cpp
std::uint16_t minValue =
    *minimum;
```

Example:

```text
Image:

100  200  50  300

Minimum = 50
```

Medical use:

```text
Image intensity analysis
```

---

# 14. Finding Maximum Pixel Value

```cpp
auto maximum =
    std::max_element(
        image.begin(),
        image.end()
    );

std::uint16_t maxValue =
    *maximum;
```

Example:

```text
Image:

100  200  50  300

Maximum = 300
```

---

# 15. Finding Both Minimum and Maximum

```cpp
auto result =
    std::minmax_element(
        image.begin(),
        image.end()
    );

auto minValue =
    *result.first;

auto maxValue =
    *result.second;
```

Useful for:

```text
Automatic intensity range detection
```

---

# 16. Filling an Image

Set all pixels to zero:

```cpp
std::fill(
    image.begin(),
    image.end(),
    0
);
```

Concept:

```text
Before:

50  100  200  80

After:

0  0  0  0
```

Useful for:

* Initialize masks
* Clear image buffers
* Reset segmentation data

---

# 17. Searching Pixel Values

```cpp
auto result =
    std::find(
        image.begin(),
        image.end(),
        255
    );
```

Check:

```cpp
if (result != image.end())
{
    // Found
}
```

---

# 18. Counting Pixels

Example:

```cpp
#include <algorithm>

std::size_t count =
    std::count(
        image.begin(),
        image.end(),
        255
    );
```

Medical example:

```text
Segmentation Mask

0 → Background
1 → Organ
```

Count foreground pixels:

```cpp
std::size_t foregroundCount =
    std::count(
        mask.begin(),
        mask.end(),
        1
    );
```

---

# 19. `std::count_if`

Suppose we want pixels greater than `1000`.

```cpp
std::size_t count =
    std::count_if(
        image.begin(),
        image.end(),
        [](std::int16_t value)
        {
            return value > 1000;
        }
    );
```

Useful for:

* Thresholding analysis
* Counting high-intensity pixels
* Quality checks

---

# 20. `std::transform`

One of the most important STL algorithms for image processing.

Example: increase brightness.

```cpp
std::transform(
    image.begin(),
    image.end(),
    image.begin(),
    [](std::uint8_t pixel)
    {
        int value =
            static_cast<int>(pixel) + 50;

        value =
            std::clamp(
                value,
                0,
                255
            );

        return static_cast<std::uint8_t>(
            value
        );
    }
);
```

Concept:

```text
Input Pixel
     ↓
Transformation
     ↓
Output Pixel
```

---

# 21. `std::transform` With Two Images

Example:

```cpp
std::transform(
    image1.begin(),
    image1.end(),
    image2.begin(),
    output.begin(),
    [](int a, int b)
    {
        return a + b;
    }
);
```

Concept:

```text
Image A ──┐
          ├── Operation ──→ Output
Image B ──┘
```

Useful for:

* Image arithmetic
* Difference images
* Pixel-wise operations

---

# 22. Image Inversion

For an 8-bit image:

$$
Output = 255 - Input
$$

Using STL:

```cpp
std::transform(
    image.begin(),
    image.end(),
    image.begin(),
    [](std::uint8_t pixel)
    {
        return static_cast<std::uint8_t>(
            255 - pixel
        );
    }
);
```

---

# 23. Thresholding Using STL

Suppose:

```text
Threshold = 128
```

Rule:

```text
Pixel >= 128 → 255

Pixel < 128 → 0
```

Code:

```cpp
std::transform(
    image.begin(),
    image.end(),
    image.begin(),
    [](std::uint8_t pixel)
    {
        return
            pixel >= 128
            ? 255
            : 0;
    }
);
```

This creates a binary image.

---

# 24. `<numeric>` Library

Include:

```cpp
#include <numeric>
```

Important algorithms:

```text
std::accumulate
std::iota
std::inner_product
std::reduce
```

---

# 25. Calculate Sum of Pixels

```cpp
std::uint64_t sum =
    std::accumulate(
        image.begin(),
        image.end(),
        std::uint64_t{0}
    );
```

Why use `uint64_t`?

Because a large image may overflow smaller integer types.

---

# 26. Calculate Mean Intensity

Formula:

$$
Mean =
\frac{\sum Pixels}{Number\ of\ Pixels}
$$

Code:

```cpp
double sum =
    std::accumulate(
        image.begin(),
        image.end(),
        0.0
    );

double mean =
    sum /
    image.size();
```

Example:

```text
Pixels:

10 20 30 40
```

$$
Mean =
\frac{10+20+30+40}{4}
=
25
$$

---

# 27. `std::iota`

Generate sequential values:

```cpp
std::vector<int> values(10);

std::iota(
    values.begin(),
    values.end(),
    0
);
```

Result:

```text
0 1 2 3 4 5 6 7 8 9
```

Useful for:

* Index arrays
* Test images
* Lookup tables

---

# 28. Sorting

```cpp
std::sort(
    values.begin(),
    values.end()
);
```

Example:

```text
Before:

50 10 40 20

After:

10 20 40 50
```

Useful in image analysis for:

* Percentiles
* Median calculation
* Intensity statistics

---

# 29. Median Calculation

```cpp
std::vector<int> values =
{
    50,
    10,
    30,
    20,
    40
};

std::sort(
    values.begin(),
    values.end()
);

int median =
    values[
        values.size() / 2
    ];
```

Result:

```text
10 20 30 40 50

Median = 30
```

---

# 30. Important Warning About Sorting Images

Do **not** normally sort an image buffer if you want to preserve spatial information.

Original:

```text
Pixel Position:

10  200  30
50  80   150
```

After sorting:

```text
10 30 50 80 150 200
```

Spatial structure is lost.

Instead:

```cpp
std::vector<int> copy = image;

std::sort(
    copy.begin(),
    copy.end()
);
```

Use the copy for statistics.

---

# 31. `std::map`

A map stores key-value pairs.

Example:

```cpp
#include <map>

std::map<int, std::string> labels;

labels[0] = "Background";
labels[1] = "Liver";
labels[2] = "Kidney";
labels[3] = "Tumor";
```

Useful for:

```text
Segmentation Labels
```

---

# 32. `std::unordered_map`

```cpp
#include <unordered_map>

std::unordered_map<int, int>
histogram;
```

Example:

```cpp
for (auto pixel : image)
{
    ++histogram[pixel];
}
```

Concept:

```text
Pixel Value → Count

0   → 100
1   → 200
2   → 150
...
```

Useful for histogram generation.

---

# 33. Histogram Using STL

For an 8-bit image:

```cpp
std::array<
    std::size_t,
    256
> histogram = {};

for (std::uint8_t pixel : image)
{
    ++histogram[pixel];
}
```

Concept:

```text
Intensity

0     ███
50    ███████
100   ██████████
150   █████
255   ██
```

This is usually better than `unordered_map` when the range is known and fixed.

---

# 34. `std::deque`

A deque supports efficient insertion/removal at both ends.

```cpp
#include <deque>

std::deque<int> values;
```

Potential image-processing use:

```text
Sliding Window Algorithms
```

However, for normal image buffers:

```text
std::vector
```

is usually preferred because of contiguous memory.

---

# 35. `std::queue`

```cpp
#include <queue>

std::queue<Point> pixels;
```

Useful for:

* Flood fill
* Region growing
* Breadth-first search

Example:

```text
Seed Pixel
    ↓
Find Neighbors
    ↓
Add to Queue
    ↓
Process
    ↓
Repeat
```

This becomes important in image segmentation.

---

# 36. `std::priority_queue`

Useful when processing items by priority.

Possible applications:

* Graph algorithms
* Image pathfinding
* Watershed-related implementations
* Shortest-path algorithms

---

# 37. Container Selection for Image Processing

| Requirement           | Recommended STL Tool                   |
| --------------------- | -------------------------------------- |
| Image buffer          | `std::vector`                          |
| Fixed-size coordinate | `std::array`                           |
| Pixel traversal       | Iterators                              |
| Minimum/Maximum       | `std::min_element`, `std::max_element` |
| Pixel transformation  | `std::transform`                       |
| Mean                  | `std::accumulate`                      |
| Histogram             | `std::array` / `std::vector`           |
| Label lookup          | `std::map`                             |
| Fast key lookup       | `std::unordered_map`                   |
| Region growing        | `std::queue`                           |
| Sliding operations    | `std::deque`                           |

---

# 38. Real Medical Image Example

Suppose we have CT data:

```text
512 × 512 × 300
```

Store:

```cpp
std::vector<std::int16_t>
ctVolume;
```

Calculate minimum and maximum:

```cpp
auto result =
    std::minmax_element(
        ctVolume.begin(),
        ctVolume.end()
    );
```

Convert values for processing:

```cpp
std::vector<float> processed(
    ctVolume.size()
);

std::transform(
    ctVolume.begin(),
    ctVolume.end(),
    processed.begin(),
    [](std::int16_t value)
    {
        return
            static_cast<float>(value);
    }
);
```

---

# 39. STL Pipeline Example

```text
DICOM Pixel Data
       ↓
std::vector<int16_t>
       ↓
std::minmax_element
       ↓
Intensity Analysis
       ↓
std::transform
       ↓
Image Processing
       ↓
std::accumulate
       ↓
Statistics
```

---

# 40. Common Mistakes

## Mistake 1: Copying Large Images Unnecessarily

Bad:

```cpp
void process(
    std::vector<std::int16_t> image
);
```

This may copy the complete image.

Better:

```cpp
void process(
    const std::vector<std::int16_t>& image
);
```

---

## Mistake 2: Using `std::map` for Pixel Buffers

Avoid:

```text
std::map<int, Pixel>
```

for a normal dense image buffer.

Use:

```text
std::vector
```

Dense image data needs efficient contiguous memory.

---

## Mistake 3: Forgetting `reserve()`

When adding many elements:

```cpp
std::vector<int> values;

values.reserve(1000000);
```

Then:

```cpp
values.push_back(...);
```

This can reduce reallocations.

---

## Mistake 4: Invalid Iterators

Example:

```cpp
auto it = image.begin();

image.push_back(100);
```

A vector reallocation may invalidate iterators.

Be careful when modifying container size during iteration.

---

## Mistake 5: Wrong Accumulator Type

Potential problem:

```cpp
int sum =
    std::accumulate(
        image.begin(),
        image.end(),
        0
    );
```

Large images can overflow.

Better:

```cpp
std::uint64_t sum =
    std::accumulate(
        image.begin(),
        image.end(),
        std::uint64_t{0}
    );
```

---

# 41. Performance Considerations

For image buffers:

### Prefer contiguous memory

```cpp
std::vector
```

### Avoid unnecessary copies

```cpp
const std::vector<T>&
```

### Reserve memory when needed

```cpp
vector.reserve(size);
```

### Prefer STL algorithms where appropriate

```cpp
std::transform
std::fill
std::minmax_element
```

### Be careful with memory allocation

Large medical volumes can consume hundreds of MB or multiple GB.

---

# 42. Practical Exercise

Create an image buffer:

```cpp
std::vector<std::uint8_t> image(
    512 * 512
);
```

Perform:

### Step 1

Fill with:

```text
100
```

### Step 2

Increase brightness by:

```text
50
```

### Step 3

Find:

```text
Minimum
Maximum
Mean
```

### Step 4

Create a histogram.

---

# 43. Mini Project

## Medical Image Statistics Engine

Create:

```text
ImageStatistics
```

Features:

```text
Input Image
    ↓
Minimum
Maximum
Mean
Median
Histogram
```

Suggested class:

```cpp
class ImageStatistics
{
public:

    int minimum();

    int maximum();

    double mean();

    double median();

    void histogram();
};
```

Later, we can improve this using:

* Templates
* Multithreading
* Parallel algorithms
* SIMD
* GPU processing

---

# 44. Interview Questions

### Beginner

1. What is STL?
2. What is `std::vector`?
3. Why is `std::vector` useful for image buffers?
4. What is an iterator?
5. What is the difference between `std::array` and `std::vector`?

### Intermediate

1. How would you calculate minimum and maximum pixel values?
2. How would you calculate image mean?
3. Why should large image vectors be passed by reference?
4. What is iterator invalidation?
5. Why should you avoid sorting the original image buffer?

### Advanced

1. Which STL containers would you use for region growing?
2. How would you implement an image histogram efficiently?
3. How do unnecessary copies affect large medical images?
4. Why is contiguous memory important for image processing?
5. How would you design a generic STL-based image container?

---

# 45. Chapter Summary

You learned:

* STL basics for image processing
* `std::vector` for image and volume buffers
* `std::array` for fixed-size medical data
* Iterators
* Range-based loops
* `std::transform`
* `std::fill`
* `std::find`
* `std::count`
* `std::count_if`
* `std::min_element`
* `std::max_element`
* `std::minmax_element`
* `std::accumulate`
* `std::sort`
* Histogram generation
* `std::map`
* `std::unordered_map`
* `std::queue`
* STL performance considerations

---

## Current Progress

* **Completed:** Chapter 6 — Arrays and Multidimensional Arrays
* **Completed:** Chapter 7 — STL for Image Processing
* **Next:** **Chapter 8 — Templates**

Templates are especially important because they allow us to build reusable medical image classes such as:

```cpp
Image<std::uint8_t>
Image<std::int16_t>
Image<float>
```

Say **Next** to continue with **Chapter 8: Templates**.
