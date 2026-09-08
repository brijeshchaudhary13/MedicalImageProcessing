# Level 0 → Module 1 → Chapter 5: Pointers and Buffers

## 1. Chapter Overview

Pointers and buffers are fundamental to high-performance image processing.

A medical image is often stored as a large block of memory:

```text
CT / MRI / PET Image
        ↓
Continuous Memory Buffer
        ↓
Pixels or Voxels
```

Understanding pointers helps you understand:

* How image libraries access pixel data
* How DICOM pixel buffers are stored
* How OpenCV accesses image memory
* How ITK represents image buffers internally
* How to process large CT/MRI volumes efficiently
* How to avoid memory corruption

---

# 2. What Is a Pointer?

A pointer stores the **memory address** of another object.

Example:

```cpp
int value = 100;

int* pointer = &value;
```

Conceptually:

```text
value
Address: 1000
Value:   100

pointer
Value: 1000
         │
         ▼
       value
       100
```

`&value` means:

> Give me the memory address of `value`.

---

# 3. Dereferencing a Pointer

To access the value through a pointer:

```cpp
int value = 100;

int* pointer = &value;

int result = *pointer;
```

Here:

```text
pointer  → memory address
*pointer → value stored at that address
```

Example:

```cpp
*pointer = 200;
```

Now:

```text
value = 200
```

---

# 4. Why Pointers Matter in Image Processing

Suppose we have a small image:

```text
10   20   30
40   50   60
70   80   90
```

It can be stored as one contiguous buffer:

```text
Address     Value

1000          10
1001          20
1002          30
1003          40
1004          50
1005          60
1006          70
1007          80
1008          90
```

A pointer can point to the first pixel:

```cpp
std::uint8_t* data;
```

Then pixels can be accessed using an index.

---

# 5. Pointer Arithmetic

Suppose:

```cpp
std::uint8_t image[5] =
{
    10,
    20,
    30,
    40,
    50
};

std::uint8_t* pointer = image;
```

Then:

```cpp
pointer[0]
```

is equivalent to:

```cpp
*(pointer + 0)
```

Similarly:

```cpp
pointer[2]
```

is equivalent to:

```cpp
*(pointer + 2)
```

Value:

```text
30
```

---

# 6. Important: Pointer Arithmetic Depends on Type

Consider:

```cpp
std::uint8_t* pointer;
```

Moving:

```cpp
pointer + 1
```

moves by:

```text
1 byte
```

But:

```cpp
std::uint16_t* pointer;
```

Moving:

```cpp
pointer + 1
```

moves by:

```text
2 bytes
```

And:

```cpp
float* pointer;
```

usually moves by:

```text
sizeof(float)
```

Therefore pointer arithmetic automatically considers the data type.

---

# 7. What Is a Buffer?

A **buffer** is a block of memory used to store data.

For an image:

```text
Image Buffer

Pixel 0
Pixel 1
Pixel 2
Pixel 3
...
Pixel N
```

Example:

```cpp
std::vector<std::uint16_t> buffer(
    width * height
);
```

The buffer stores all pixels.

---

# 8. Image Buffer Memory Layout

For a 2D image:

```text
Width = 4
Height = 3
```

Conceptual image:

```text
 0   1   2   3
 4   5   6   7
 8   9  10  11
```

Memory:

```text
Index

0 → 0
1 → 1
2 → 2
3 → 3
4 → 4
5 → 5
6 → 6
7 → 7
8 → 8
9 → 9
10 → 10
11 → 11
```

Formula:

$$
\boxed{index = y \times width + x}
$$

---

# 9. Accessing Pixels Through a Pointer

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    constexpr int width = 3;
    constexpr int height = 3;

    std::uint8_t image[width * height] =
    {
        10, 20, 30,
        40, 50, 60,
        70, 80, 90
    };

    std::uint8_t* data = image;

    int x = 1;
    int y = 2;

    int index = y * width + x;

    std::cout
        << static_cast<int>(data[index]);

    return 0;
}
```

Result:

```text
80
```

Because:

```text
index = 2 × 3 + 1
      = 7
```

---

# 10. Buffer Access vs Pointer Traversal

## Index-Based Access

```cpp
for (std::size_t i = 0;
     i < size;
     ++i)
{
    buffer[i] = 0;
}
```

## Pointer-Based Access

```cpp
std::uint16_t* pointer = buffer.data();

for (std::size_t i = 0;
     i < buffer.size();
     ++i)
{
    pointer[i] = 0;
}
```

Both can work efficiently.

In modern C++, prefer the clearer abstraction unless profiling shows a specific need for lower-level pointer handling.

---

# 11. `std::vector::data()`

A very important function:

```cpp
std::vector<std::uint16_t> image;
```

Get pointer to its internal buffer:

```cpp
std::uint16_t* data =
    image.data();
```

Concept:

```text
std::vector
    │
    ├── size
    ├── capacity
    │
    └── contiguous buffer
            │
            ▼
          data()
```

This is useful when integrating with:

* C APIs
* Image libraries
* GPU APIs
* Legacy C++ code

---

# 12. `const` Pointers

There are important differences.

## Pointer to Constant Data

```cpp
const int* pointer;
```

You cannot modify the data through this pointer:

```cpp
*pointer = 10; // Not allowed
```

But the pointer can point somewhere else.

---

## Constant Pointer

```cpp
int* const pointer = &value;
```

The pointer cannot point somewhere else.

But the value can be modified.

---

## Constant Pointer to Constant Data

```cpp
const int* const pointer;
```

Neither:

* The pointer location
* Nor the pointed data

can be modified through that pointer.

---

# 13. Medical Imaging Example: Read-Only Input

A filter should not modify the original image:

```cpp
void process(
    const std::uint16_t* input,
    std::uint16_t* output,
    std::size_t size)
{
    for (std::size_t i = 0;
         i < size;
         ++i)
    {
        output[i] = input[i];
    }
}
```

Concept:

```text
Input Buffer
(Read Only)
      ↓
Processing
      ↓
Output Buffer
(Modified)
```

This makes ownership and modification intent clearer.

---

# 14. Buffer Ownership vs Buffer Access

This is extremely important.

## Ownership

Who is responsible for releasing memory?

Example:

```cpp
std::vector<std::uint16_t> buffer;
```

The vector owns the memory.

## Access

Who can access the memory?

```cpp
std::uint16_t* data =
    buffer.data();
```

The pointer accesses memory but does **not** own it.

Concept:

```text
std::vector
   │
   │ OWNS
   ▼
Memory Buffer
   ▲
   │ ACCESSES
   │
Raw Pointer
```

A raw pointer should usually not automatically mean ownership.

---

# 15. Dangerous Example: Returning Pointer to Local Data

Wrong:

```cpp
std::uint16_t* createBuffer()
{
    std::uint16_t data[100];

    return data;
}
```

After the function ends:

```text
data is destroyed
```

The returned pointer is invalid.

This creates a dangling pointer.

Better:

```cpp
std::vector<std::uint16_t>
createBuffer()
{
    return std::vector<std::uint16_t>(100);
}
```

---

# 16. 3D Volume Buffer

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
z \times W \times H
+
y \times W
+
x
}
$$

C++:

```cpp
std::size_t index =
    z * width * height +
    y * width +
    x;
```

---

# 17. Example: 3D Voxel Access

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>

std::uint16_t getVoxel(
    const std::vector<std::uint16_t>& volume,
    std::size_t width,
    std::size_t height,
    std::size_t x,
    std::size_t y,
    std::size_t z)
{
    const std::size_t index =
        z * width * height +
        y * width +
        x;

    return volume[index];
}
```

---

# 18. Pointer-Based Image Traversal

Example:

```cpp
void increaseBrightness(
    std::uint16_t* data,
    std::size_t size,
    int brightness)
{
    for (std::size_t i = 0;
         i < size;
         ++i)
    {
        int value =
            static_cast<int>(data[i]) +
            brightness;

        if (value > 65535)
        {
            value = 65535;
        }

        if (value < 0)
        {
            value = 0;
        }

        data[i] =
            static_cast<std::uint16_t>(value);
    }
}
```

This processes the image in-place.

---

# 19. Buffer Boundaries

Suppose:

```text
Buffer Size = 10
```

Valid indexes:

```text
0 1 2 3 4 5 6 7 8 9
```

Invalid:

```text
10
```

Danger:

```cpp
buffer[10] = 100;
```

This can write outside the allocated memory.

Result:

```text
Memory Corruption
      ↓
Unexpected Crash
      ↓
Incorrect Image
```

---

# 20. Safe Buffer Access

During development:

```cpp
buffer.at(index);
```

performs bounds checking.

Example:

```cpp
std::uint16_t value =
    buffer.at(index);
```

For performance-critical code:

```cpp
buffer[index];
```

may be used after dimensions and indexes are already validated.

Important principle:

> Validate boundaries at appropriate API boundaries; avoid relying on undefined behavior for performance.

---

# 21. Stride

So far, we assumed every row is stored immediately after the previous row.

Sometimes an image has **stride** or **row pitch**.

Example:

```text
Image Width = 4 pixels
Bytes Per Pixel = 2
```

Expected row data:

$$
4 \times 2 = 8\ bytes
$$

But actual stride might be:

```text
16 bytes
```

Memory:

```text
Row 0 data + padding
Row 1 data + padding
Row 2 data + padding
```

Formula:

$$
Address =
base +
y \times stride +
x \times bytesPerPixel
$$

This is important when working with:

* OpenCV
* Camera buffers
* GPU textures
* External image memory

---

# 22. Generic Image Buffer Concept

```text
┌──────────────────────────┐
│ Image Metadata           │
│                          │
│ Width                    │
│ Height                   │
│ Depth                    │
│ Pixel Type               │
│ Spacing                  │
│                          │
├──────────────────────────┤
│ Image Buffer             │
│                          │
│ Pixel/Voxel Data         │
│                          │
└──────────────────────────┘
```

A medical image is therefore more than:

```text
Pointer + Width + Height
```

It may also require:

* Pixel type
* Physical spacing
* Origin
* Orientation
* DICOM metadata

---

# 23. Safe Buffer API Design

A better design separates:

```text
Ownership
```

from:

```text
Access
```

Example:

```cpp
class Image
{
public:
    std::uint16_t* data()
    {
        return m_data.data();
    }

    const std::uint16_t* data() const
    {
        return m_data.data();
    }

    std::size_t size() const
    {
        return m_data.size();
    }

private:
    std::vector<std::uint16_t> m_data;
};
```

Usage:

```cpp
Image image;

const auto* readOnlyData =
    image.data();
```

The caller gets access but does not own the buffer.

---

# 24. Algorithm Complexity

For sequential processing:

```text
Input Buffer
     ↓
Visit Every Pixel
     ↓
Output
```

For \(N\) pixels:

### Time Complexity

$$
O(N)
$$

### Extra Memory

In-place:

$$
O(1)
$$

Separate output buffer:

$$
O(N)
$$

For a 3D volume:

$$
N = W \times H \times D
$$

---

# 25. Common Mistakes

## Mistake 1: Confusing Ownership and Access

```cpp
std::uint16_t* data;
```

This does not automatically tell you who owns the memory.

---

## Mistake 2: Returning Invalid Pointers

```cpp
std::uint16_t* getData()
{
    std::uint16_t local[100];
    return local;
}
```

Invalid after the function returns.

---

## Mistake 3: Wrong Index Formula

2D:

$$
y \times width + x
$$

3D:

$$
zWH + yW + x
$$

---

## Mistake 4: Ignoring Stride

Do not assume:

```text
row size = width × bytes per pixel
```

for every external image buffer.

---

## Mistake 5: Accessing After Reallocation

Example:

```cpp
auto* pointer = buffer.data();

buffer.push_back(100);
```

A vector reallocation may invalidate `pointer`.

This is a very important real-world bug.

---

# 26. Debugging Pointers and Buffers

When an image is corrupted, check:

### Dimensions

```text
Width
Height
Depth
```

### Buffer Size

Expected:

$$
W \times H \times D
$$

### Data Type

```text
uint8_t?
uint16_t?
int16_t?
float?
```

### Index Formula

```text
2D → yW + x

3D → zWH + yW + x
```

### Stride

Check whether rows contain padding.

### Pointer Lifetime

Ask:

> Does this pointer still point to valid memory?

---

# 27. Medical Imaging Connection

Pointers and buffers are used throughout medical imaging:

```text
DICOM Pixel Data
       ↓
Memory Buffer
       ↓
Image Processing
       ↓
ITK Image
       ↓
Visualization
       ↓
VTK
       ↓
Qt Viewer
```

For a high-performance medical image viewer, understanding this data path is essential.

---

# Interview Questions

## Basic

1. What is a pointer?
2. What is a buffer?
3. What is pointer dereferencing?
4. What is a dangling pointer?
5. What is the difference between pointer ownership and pointer access?

## Intermediate

1. How is a 2D image stored in a 1D buffer?
2. What is the formula for a 3D voxel index?
3. What is stride?
4. What does `vector.data()` return?
5. Why can vector reallocation invalidate pointers?

## Advanced

1. How would you design a safe image-buffer API?
2. How would you process external memory with stride?
3. How would you safely expose an image buffer to a legacy C API?
4. How would you prevent pointer lifetime bugs?
5. How would you design a zero-copy medical imaging pipeline?

---

# Exercises

### Beginner

1. Create an array of 10 pixels.
2. Create a pointer to the first pixel.
3. Print every pixel using pointer indexing.
4. Calculate the index for `(x=3, y=2)` in an image of width `10`.

### Intermediate

1. Implement a 2D image buffer using `std::vector`.
2. Implement safe `getPixel()` and `setPixel()`.
3. Implement 3D voxel indexing.
4. Write a function that receives a read-only input pointer and an output pointer.

### Advanced

Design a buffer abstraction that supports:

```text
2D and 3D
Different pixel types
Stride
Read-only access
Writable access
Ownership
External memory
```

---

# Key Takeaways

* A pointer stores a memory address.
* A buffer stores image pixel or voxel data.
* A raw pointer does not automatically imply ownership.
* `std::vector::data()` exposes contiguous buffer memory.
* 2D indexing:

$$
\boxed{index = yW + x}
$$

* 3D indexing:

$$
\boxed{index = zWH + yW + x}
$$

* Stride must be considered when processing external image memory.
* Pointer lifetime is critical.
* Vector reallocations can invalidate raw pointers.
* Safe ownership and clear buffer access are essential in medical imaging software.

---

## Current Progress

* **Completed:** Chapter 4
* **Current:** **Level 0 → Module 1 → Chapter 5: Pointers and Buffers**
* **Next:** **Level 0 → Module 1 → Chapter 6: Arrays and Multidimensional Arrays**

Say **Next** when you are ready for Chapter 6.
