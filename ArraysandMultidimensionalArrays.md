# Level 0 → Module 1 → Chapter 6: Arrays and Multidimensional Arrays

## 1. Chapter Overview

Arrays are fundamental to digital and medical image processing because an image is essentially a structured collection of pixel or voxel values.

```text
2D Image
   ↓
2D Pixel Coordinates
   ↓
Usually stored in
   ↓
1D Continuous Memory
```

For medical imaging:

```text
X-ray → 2D
CT    → 3D volume
MRI   → 3D / 4D data
PET   → 3D / 4D data
```

Understanding arrays is essential before learning:

* Image representation
* Pixel access
* Voxel access
* Image filtering
* Convolution
* CT/MRI volume processing

---

# 2. What Is an Array?

An array stores multiple values of the same type.

```cpp
int values[5] =
{
    10,
    20,
    30,
    40,
    50
};
```

Memory conceptually:

```text
Index:   0    1    2    3    4
Value:  10   20   30   40   50
```

Access:

```cpp
int value = values[2];
```

Result:

```text
30
```

---

# 3. Array Indexing

C++ arrays start at index:

```text
0
```

For:

```cpp
int image[5];
```

Valid indexes:

```text
0
1
2
3
4
```

Invalid:

```text
5
```

Important:

```cpp
image[5]
```

causes undefined behavior because it accesses memory outside the array.

---

# 4. Arrays and Image Pixels

A grayscale image:

```text
10   20   30
40   50   60
70   80   90
```

Can conceptually be represented as:

```cpp
std::uint8_t image[3][3] =
{
    {10, 20, 30},
    {40, 50, 60},
    {70, 80, 90}
};
```

Access pixel:

```cpp
int pixel = image[1][2];
```

Result:

```text
60
```

Because:

```text
Row = 1
Column = 2
```

---

# 5. One-Dimensional Array

Example:

```cpp
int pixels[10];
```

Memory:

```text
pixels
│
├── pixels[0]
├── pixels[1]
├── pixels[2]
├── pixels[3]
└── ...
```

Image processing libraries often use one-dimensional buffers internally because memory is linear.

Example:

```cpp
std::vector<std::uint16_t> pixels(
    width * height
);
```

---

# 6. Two-Dimensional Arrays

A 2D array:

```cpp
int image[3][4];
```

Means:

```text
3 rows
4 columns
```

Visualization:

```text
Column

        0   1   2   3
      ┌────────────────
Row 0 │
Row 1 │
Row 2 │
```

Access:

```cpp
image[row][column];
```

Example:

```cpp
image[2][3] = 100;
```

---

# 7. Row-Major Memory Layout

C++ multidimensional arrays use **row-major order**.

Example:

```cpp
int image[2][3] =
{
    {10, 20, 30},
    {40, 50, 60}
};
```

Conceptually:

```text
Row 0:

10 20 30

Row 1:

40 50 60
```

Actual linear memory order:

```text
10 → 20 → 30 → 40 → 50 → 60
```

Formula:

$$
\boxed{index = y \times width + x}
$$

---

# 8. Why Row-Major Order Matters

Consider processing an image.

Efficient:

```cpp
for (int y = 0; y < height; ++y)
{
    for (int x = 0; x < width; ++x)
    {
        process(image[y][x]);
    }
}
```

This accesses memory sequentially.

Usually better for cache performance.

Less efficient for row-major memory:

```cpp
for (int x = 0; x < width; ++x)
{
    for (int y = 0; y < height; ++y)
    {
        process(image[y][x]);
    }
}
```

Because memory access jumps between rows.

---

# 9. Flattening a 2D Array

Suppose:

```text
Width = W
Height = H
```

Coordinates:

```text
(x, y)
```

Convert to a 1D index:

$$
\boxed{index = yW + x}
$$

Example:

```text
Width = 5

x = 2
y = 3
```

$$
index = 3 \times 5 + 2
$$

$$
index = 17
$$

---

# 10. 2D Image Using `std::vector`

A practical approach:

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>

class Image2D
{
public:
    Image2D(
        std::size_t width,
        std::size_t height)
        : m_width(width),
          m_height(height),
          m_data(width * height)
    {
    }

    std::uint16_t& pixel(
        std::size_t x,
        std::size_t y)
    {
        return m_data[
            y * m_width + x
        ];
    }

private:
    std::size_t m_width;
    std::size_t m_height;

    std::vector<std::uint16_t> m_data;
};
```

Memory:

```text
Image2D
   │
   ├── Width
   ├── Height
   │
   └── Continuous Pixel Buffer
```

---

# 11. Three-Dimensional Arrays

Medical imaging frequently uses 3D data.

Example CT:

```text
            Z
            │
            │
         ┌──┘
        /
       /
      Y
     /
    /
   X
```

Coordinates:

$$
(x, y, z)
$$

A 3D array:

```cpp
int volume[depth][height][width];
```

Access:

```cpp
volume[z][y][x];
```

---

# 12. 3D Volume Flattening

For:

```text
Width  = W
Height = H
Depth  = D
```

The linear index is:

$$
\boxed{
index =
z \times H \times W
+
y \times W
+
x
}
$$

Or:

$$
\boxed{
index = zHW + yW + x
}
$$

---

# 13. Example 3D Volume

Suppose:

```text
Width = 4
Height = 3
Depth = 2
```

Total voxels:

$$
4 \times 3 \times 2 = 24
$$

Memory:

```text
Slice 0
────────────

0  1  2  3
4  5  6  7
8  9 10 11


Slice 1
────────────

12 13 14 15
16 17 18 19
20 21 22 23
```

The slices are stored sequentially.

---

# 14. 3D Volume Class

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>

class Volume3D
{
public:
    Volume3D(
        std::size_t width,
        std::size_t height,
        std::size_t depth)
        : m_width(width),
          m_height(height),
          m_depth(depth),
          m_data(width * height * depth)
    {
    }

    std::uint16_t& voxel(
        std::size_t x,
        std::size_t y,
        std::size_t z)
    {
        return m_data[
            z * m_width * m_height +
            y * m_width +
            x
        ];
    }

private:
    std::size_t m_width;
    std::size_t m_height;
    std::size_t m_depth;

    std::vector<std::uint16_t> m_data;
};
```

---

# 15. Array Traversal

## One-Dimensional

```cpp
for (std::size_t i = 0;
     i < size;
     ++i)
{
    process(data[i]);
}
```

---

## Two-Dimensional

```cpp
for (std::size_t y = 0;
     y < height;
     ++y)
{
    for (std::size_t x = 0;
         x < width;
         ++x)
    {
        process(
            image[y * width + x]
        );
    }
}
```

---

## Three-Dimensional

```cpp
for (std::size_t z = 0;
     z < depth;
     ++z)
{
    for (std::size_t y = 0;
         y < height;
         ++y)
    {
        for (std::size_t x = 0;
             x < width;
             ++x)
        {
            const std::size_t index =
                z * width * height +
                y * width +
                x;

            process(volume[index]);
        }
    }
}
```

---

# 16. Static vs Dynamic Arrays

## Static Array

```cpp
int image[512][512];
```

Size is fixed.

Problems:

* Size must generally be known at compile time.
* Large stack allocations can be dangerous.
* Not flexible for medical images of different sizes.

---

## Dynamic Container

```cpp
std::vector<std::uint16_t> image(
    width * height
);
```

Advantages:

* Runtime size
* Automatic memory management
* Contiguous memory
* RAII

For medical imaging, dynamic containers are generally more practical.

---

# 17. `std::array`

Modern C++ also provides:

```cpp
#include <array>

std::array<int, 5> values =
{
    10,
    20,
    30,
    40,
    50
};
```

Useful when the size is known at compile time.

Example:

```cpp
std::array<float, 3> spacing =
{
    1.0f,
    1.0f,
    2.5f
};
```

Possible medical meaning:

```text
X spacing = 1.0 mm
Y spacing = 1.0 mm
Z spacing = 2.5 mm
```

---

# 18. Array vs Vector

| Feature | C Array | `std::array` | `std::vector` |
|---|---|---|
| Size | Fixed | Fixed | Dynamic |
| RAII | Limited abstraction | Yes | Yes |
| Runtime resizing | No | No | Yes |
| Contiguous memory | Yes | Yes | Yes |
| Best use | Low-level/legacy | Fixed-size data | Image buffers |

For most dynamically sized images:

```text
std::vector
```

is usually the best starting choice.

---

# 19. Multidimensional Arrays vs Flattened Buffers

## Multidimensional Syntax

```cpp
image[y][x];
```

Easy to read.

## Flattened Buffer

```cpp
image[y * width + x];
```

Advantages:

* Dynamic dimensions
* Easy integration with libraries
* Contiguous memory
* Flexible allocation

This is why flattened buffers are common in image processing.

---

# 20. 4D Medical Image Data

Some medical datasets have a fourth dimension.

Example:

```text
MRI Time Series
```

Dimensions:

```text
X
Y
Z
T
```

Where:

```text
X → Width
Y → Height
Z → Slice
T → Time
```

Index:

$$
index =
tDHW
+
zHW
+
yW
+
x
$$

This is useful for:

* Dynamic MRI
* Functional MRI
* Cardiac imaging
* Time-dependent PET

---

# 21. Generic N-Dimensional Concept

A medical image can be represented as:

```text
Dimensions
+
Pixel Type
+
Memory Buffer
+
Metadata
```

For example:

```text
CT

Dimensions:
512 × 512 × 300

Pixel Type:
16-bit signed integer

Buffer:
Continuous voxel memory

Metadata:
Spacing
Origin
Orientation
Patient information
```

Later, libraries like ITK will provide abstractions for these concepts.

---

# 22. Bounds Checking

Unsafe:

```cpp
data[index];
```

Safe checked access:

```cpp
data.at(index);
```

Example:

```cpp
std::vector<int> data(10);

data.at(20);
```

This detects an invalid index by throwing an exception.

For medical software, validation of dimensions and coordinates is important.

---

# 23. Array Copying

Suppose:

```cpp
std::vector<std::uint16_t> image1;

std::vector<std::uint16_t> image2 =
    image1;
```

This copies the complete buffer.

For large CT volumes, this may consume significant memory.

Prefer references for read-only processing:

```cpp
void process(
    const std::vector<std::uint16_t>& image
);
```

---

# 24. In-Place Array Processing

Example:

```cpp
void invert(
    std::vector<std::uint8_t>& image)
{
    for (auto& pixel : image)
    {
        pixel = 255 - pixel;
    }
}
```

Concept:

```text
Input Image
     ↓
Modify same buffer
     ↓
Output
```

Memory efficient.

---

# 25. Out-of-Place Processing

```cpp
void copyImage(
    const std::vector<std::uint8_t>& input,
    std::vector<std::uint8_t>& output)
{
    output.resize(input.size());

    for (std::size_t i = 0;
         i < input.size();
         ++i)
    {
        output[i] = input[i];
    }
}
```

Concept:

```text
Input Buffer
      ↓
Processing
      ↓
Output Buffer
```

Uses additional memory but preserves the original.

---

# 26. Common Array Mistakes

## Mistake 1: Off-by-One Error

Wrong:

```cpp
for (int i = 0;
     i <= size;
     ++i)
```

Correct:

```cpp
for (int i = 0;
     i < size;
     ++i)
```

---

## Mistake 2: Wrong Dimension Order

For:

```cpp
volume[z][y][x];
```

Do not accidentally assume:

```cpp
volume[x][y][z];
```

Always define the dimension convention clearly.

---

## Mistake 3: Integer Overflow

Danger:

```cpp
int total =
    width * height * depth;
```

Prefer careful `std::size_t` calculations and overflow validation for real production allocations.

---

## Mistake 4: Large Stack Arrays

Danger:

```cpp
std::uint16_t image[
    512 * 512 * 500
];
```

Do not place huge medical volumes on the stack.

Use managed dynamic storage.

---

# 27. Medical Imaging Coordinate vs Array Index

This distinction is important.

Array index:

```text
x = 100
y = 200
z = 50
```

These are discrete voxel locations.

Physical location may be:

```text
X = 100 mm
Y = 200 mm
Z = 125 mm
```

The relationship depends on:

* Spacing
* Origin
* Orientation

This becomes extremely important in:

* DICOM
* Image registration
* MPR
* Radiation therapy planning

We will study this deeply later.

---

# 28. Real Medical Imaging Example

Suppose CT data:

```text
Dimensions:

512 × 512 × 300
```

Voxel type:

```text
int16_t
```

Storage:

```cpp
std::vector<std::int16_t> ctVolume;
```

Total elements:

$$
512 \times 512 \times 300
=
78,643,200
$$

Approximate memory:

$$
78,643,200 \times 2
$$

$$
= 157,286,400\ bytes
$$

Approximately:

```text
150 MB
```

A processing pipeline might contain:

```text
CT Volume
    ↓
Noise Reduction
    ↓
Segmentation
    ↓
Registration
    ↓
Visualization
```

Each stage must carefully manage additional buffers.

---

# 29. Interview Questions

### Basic

1. What is an array?
2. Why do arrays start at index 0?
3. What is row-major order?
4. What is a multidimensional array?
5. What is the difference between `std::array` and `std::vector`?

### Intermediate

1. How do you flatten a 2D array?
2. How do you flatten a 3D volume?
3. Why are contiguous buffers useful?
4. Why are large arrays dangerous on the stack?
5. What is an off-by-one error?

### Advanced

1. How would you design an N-dimensional image container?
2. How would you prevent overflow during multidimensional allocation?
3. How would you represent 4D MRI data?
4. What is the difference between image index coordinates and physical coordinates?
5. How would you optimize traversal of a large CT volume?

---

# 30. Exercises

## Beginner

1. Create an array containing 10 integers.
2. Print all elements.
3. Create a `3 × 3` image array.
4. Access pixel `(x=2, y=1)`.

---

## Intermediate

Create an `Image2D` class supporting:

```text
Width
Height
Pixel Buffer
getPixel()
setPixel()
```

Use:

$$
index = yW + x
$$

---

## Advanced

Create a generic:

```cpp
Volume3D<T>
```

that supports:

```text
Different voxel types
Width
Height
Depth
Contiguous memory
Voxel access
Read-only access
Writable access
```

Example:

```cpp
Volume3D<std::int16_t> ct;

Volume3D<float> dose;

Volume3D<std::uint8_t> mask;
```

---

# Key Takeaways

* Images and volumes are fundamentally multidimensional arrays.
* C++ uses row-major memory layout.
* A 2D image is commonly flattened using:

$$
\boxed{index = yW + x}
$$

* A 3D volume is commonly flattened using:

$$
\boxed{index = zHW + yW + x}
$$

* `std::vector` is useful for dynamically sized image buffers.
* `std::array` is useful for fixed-size data.
* Large medical volumes should not be allocated on the stack.
* Sequential memory traversal is generally cache-friendly.
* 4D imaging introduces a time or additional dimension.
* Array coordinates and physical patient coordinates are not the same.

---

## Current Progress

* **Completed:** Chapter 5
* **Current:** **Level 0 → Module 1 → Chapter 6: Arrays and Multidimensional Arrays**
* **Next:** **Level 0 → Module 1 → Chapter 7: Data Types and Pixel Representation**

Say **Next** to continue.
