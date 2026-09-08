# Level 0 → Module 1 → Chapter 4: Memory Management

## 1. Chapter Overview

Memory management is one of the most important topics in medical image processing because medical images can consume a large amount of RAM.

A medical imaging application may simultaneously hold:

```text
CT Volume
+ MRI Volume
+ PET Volume
+ Segmentation Masks
+ Temporary Processing Buffers
+ Dose Grid
+ 3D Rendering Data
```

If memory is managed poorly, the application may:

* Crash
* Become slow
* Leak memory
* Access invalid memory
* Produce corrupted images

For production medical software, correct memory management is essential.

---

# 2. Why Memory Management Is Important

Consider a CT volume:

$$
512 \times 512 \times 500
$$

Total voxels:

$$
512 \times 512 \times 500 = 131,072,000
$$

If every voxel uses 16 bits:

$$
16\ bits = 2\ bytes
$$

Memory required:

$$
131,072,000 \times 2
=
262,144,000\ bytes
$$

Approximately:

$$
250\ MB
$$

Now imagine multiple copies:

```text
Original CT Volume      250 MB
Filtered Volume         250 MB
Segmentation            250 MB
Temporary Buffer        250 MB
────────────────────────────
Total                 1000 MB
```

This is why memory management matters.

---

# 3. How Computer Memory Works

A simplified memory model:

```text
┌──────────────────────┐
│        Stack         │
│                      │
│ Local variables      │
│ Function calls       │
├──────────────────────┤
│        Heap          │
│                      │
│ Dynamic objects      │
│ Large buffers        │
├──────────────────────┤
│ Static / Global      │
│                      │
│ Global data          │
└──────────────────────┘
```

The most important areas for us are:

* Stack
* Heap

---

# 4. Stack Memory

Example:

```cpp
void process()
{
    int width = 512;
    int height = 512;
}
```

`width` and `height` are local variables.

Conceptually:

```text
process()
│
├── width
└── height
```

When the function ends:

```text
process() ends
      ↓
Local memory released automatically
```

### Advantages

* Fast allocation
* Automatic cleanup
* Simple lifetime management

### Limitation

The stack is limited in size.

Therefore, large medical image buffers generally should not be placed on the stack.

---

# 5. Heap Memory

Heap memory is generally used for dynamically sized objects.

Example:

```cpp
auto image =
    std::make_unique<Image>(512, 512);
```

The object can have dynamic lifetime and managed ownership.

Conceptually:

```text
Stack                    Heap

image pointer  ────────→ Image Object
```

When the `unique_ptr` is destroyed:

```text
unique_ptr destroyed
        ↓
Image automatically destroyed
```

---

# 6. Stack vs Heap

| Feature    | Stack               | Heap                    |
| ---------- | ------------------- | ----------------------- |
| Allocation | Automatic           | Dynamic                 |
| Lifetime   | Scope-based         | Controlled by ownership |
| Speed      | Generally fast      | Usually more expensive  |
| Size       | Limited             | Larger                  |
| Best for   | Small local objects | Large/dynamic objects   |

Example:

```text
Small variables
      ↓
Stack

Large image data
      ↓
Managed dynamic storage
```

---

# 7. Raw Dynamic Memory

Old C++:

```cpp
int* data = new int[1000];
```

Manual cleanup:

```cpp
delete[] data;
```

The danger:

```cpp
int* data = new int[1000];

// Exception happens here

delete[] data; // May never execute
```

Result:

```text
Memory Leak
```

Therefore, modern C++ prefers RAII.

---

# 8. Memory Leak

A memory leak happens when allocated memory is no longer properly released.

Example:

```cpp
void process()
{
    auto* image = new int[1000000];

    // Forgot delete[]
}
```

Every call allocates memory that remains allocated.

```text
Call 1 → Memory allocated
Call 2 → More memory allocated
Call 3 → More memory allocated
        ↓
Eventually application runs out of memory
```

---

# 9. Modern Solution: `std::vector`

For image buffers:

```cpp
#include <vector>
#include <cstdint>

std::vector<std::uint16_t> image(
    512 * 512
);
```

Automatic cleanup:

```text
vector created
     ↓
Memory allocated
     ↓
vector leaves scope
     ↓
Memory released automatically
```

This follows RAII.

---

# 10. Image Buffer Example

A simple image class:

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>

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

private:
    std::size_t m_width;
    std::size_t m_height;

    std::vector<std::uint16_t> m_data;
};
```

Memory ownership:

```text
Image
 │
 └── vector
       │
       └── Owns image buffer
```

When `Image` is destroyed:

```text
Image destroyed
      ↓
vector destroyed
      ↓
Buffer automatically released
```

---

# 11. Dangling Pointer

A dangling pointer points to memory that is no longer valid.

Example:

```cpp
int* createData()
{
    int value = 10;

    return &value;
}
```

Problem:

```text
Function ends
     ↓
value destroyed
     ↓
Pointer still exists
     ↓
Invalid memory access
```

Medical imaging example:

```text
Image Buffer Destroyed
        ↓
Viewer Still Uses Pointer
        ↓
Crash / Corrupted Image
```

---

# 12. Use-After-Free

Example:

```cpp
int* data = new int[100];

delete[] data;

data[0] = 10;
```

This is undefined behavior.

The memory has already been released.

Modern ownership tools reduce this risk significantly.

---

# 13. Double Free

Example:

```cpp
int* data = new int[100];

delete[] data;
delete[] data;
```

The same memory is released twice.

This can cause:

* Application crashes
* Heap corruption
* Security problems

RAII containers prevent many such errors.

---

# 14. Smart Pointers

## `std::unique_ptr`

```cpp
auto image =
    std::make_unique<Image>();
```

Ownership:

```text
unique_ptr
    │
    └── Image
```

Only one owner.

When the owner is destroyed:

```text
Image automatically deleted
```

---

## `std::shared_ptr`

```cpp
auto image =
    std::make_shared<Image>();
```

Multiple owners can share lifetime.

```text
Viewer ────┐
           ├──→ Image
Processor ─┘
```

But use carefully because reference counting adds complexity.

---

# 15. Object Lifetime

Understanding lifetime is critical.

Example:

```cpp
Image image(512, 512);

process(image);
```

Lifetime:

```text
Create Image
      ↓
Use Image
      ↓
Process Image
      ↓
Scope Ends
      ↓
Destroy Image
```

A pointer or reference must never outlive the object it refers to.

---

# 16. Copying Large Images

Suppose:

```cpp
Image image1 = loadImage();
Image image2 = image1;
```

This may create a complete copy.

For a 500 MB volume:

```text
image1 = 500 MB
image2 = 500 MB
```

Total:

```text
1 GB
```

Instead, use references when copying is unnecessary:

```cpp
void process(const Image& image);
```

This avoids copying.

---

# 17. Move Semantics

If ownership must be transferred:

```cpp
Image image1 = loadImage();

Image image2 =
    std::move(image1);
```

Concept:

```text
Before:

image1 → [Large Buffer]

After move:

image1 → valid but unspecified state

image2 → [Large Buffer]
```

Move semantics can avoid expensive copying.

---

# 18. Memory Alignment

CPUs work efficiently with properly aligned memory.

For image processing, alignment can matter for:

* SIMD
* AVX
* SSE
* GPU transfer

Concept:

```text
Memory Address
     ↓
Proper alignment
     ↓
More efficient CPU access
```

You usually should not manually optimize alignment until performance profiling shows it is necessary.

---

# 19. Cache Efficiency

Memory speed is not uniform from a CPU-performance perspective.

Simplified:

```text
CPU Registers
      ↓
L1 Cache
      ↓
L2 Cache
      ↓
L3 Cache
      ↓
RAM
```

Generally:

```text
Closer to CPU
    ↓
Faster
```

---

## Good Image Traversal

For row-major image storage:

```cpp
for (int y = 0; y < height; ++y)
{
    for (int x = 0; x < width; ++x)
    {
        process(image[y * width + x]);
    }
}
```

This accesses memory sequentially:

```text
0 → 1 → 2 → 3 → 4 → 5
```

This is generally cache-friendly.

---

## Poor Traversal

```cpp
for (int x = 0; x < width; ++x)
{
    for (int y = 0; y < height; ++y)
    {
        process(image[y * width + x]);
    }
}
```

This may jump through memory.

For row-major data, this is often less cache-friendly.

---

# 20. Buffer Size Calculation

For a 2D image:

$$
Memory =
Width \times Height \times BytesPerPixel
$$

Example:

```text
512 × 512
16-bit
```

$$
512 \times 512 \times 2
=
524,288\ bytes
$$

Approximately:

$$
512\ KB
$$

For a 3D volume:

$$
Memory =
Width \times Height \times Depth \times BytesPerVoxel
$$

Example:

```text
512 × 512 × 500
16-bit
```

$$
512 \times 512 \times 500 \times 2
$$

Approximately:

$$
250\ MB
$$

---

# 21. Integer Overflow in Size Calculation

Danger:

```cpp
int size =
    width * height * depth;
```

For large volumes, multiplication may overflow.

Better:

```cpp
const std::size_t size =
    static_cast<std::size_t>(width) *
    static_cast<std::size_t>(height) *
    static_cast<std::size_t>(depth);
```

Always think about:

```text
Large dimensions
      +
Bytes per voxel
      =
Potential overflow
```

---

# 22. Memory Management Algorithm

When loading a medical volume:

```text
Input Dimensions
      ↓
Validate Dimensions
      ↓
Calculate Total Voxels
      ↓
Check for Overflow
      ↓
Allocate Memory
      ↓
Load Data
      ↓
Process Data
      ↓
Automatically Release Memory
```

### Pseudocode

```text
VALIDATE width, height, depth

size =
    width × height × depth

CHECK overflow

ALLOCATE image buffer

LOAD voxel data

PROCESS image

AUTOMATIC CLEANUP
```

---

# 23. Safe 3D Volume Class

```cpp
#include <vector>
#include <cstdint>
#include <cstddef>
#include <stdexcept>

class Volume
{
public:
    Volume(std::size_t width,
           std::size_t height,
           std::size_t depth)
        : m_width(width),
          m_height(height),
          m_depth(depth),
          m_data(width * height * depth)
    {
        if (width == 0 ||
            height == 0 ||
            depth == 0)
        {
            throw std::invalid_argument(
                "Invalid volume dimensions"
            );
        }
    }

    std::uint16_t& voxel(
        std::size_t x,
        std::size_t y,
        std::size_t z)
    {
        return m_data.at(
            z * m_width * m_height +
            y * m_width +
            x
        );
    }

private:
    std::size_t m_width;
    std::size_t m_height;
    std::size_t m_depth;

    std::vector<std::uint16_t> m_data;
};
```

---

# 24. 3D Memory Layout

Formula:

$$
index =
z \times width \times height
+
y \times width
+
x
$$

Therefore:

```cpp
index =
    z * width * height
    + y * width
    + x;
```

Concept:

```text
Volume

Slice 0
─────────
Voxel Data

Slice 1
─────────
Voxel Data

Slice 2
─────────
Voxel Data
```

Each slice is stored sequentially.

---

# 25. In-Place vs Out-of-Place Processing

## In-Place

```text
Input Buffer
     ↓
Modify Same Buffer
```

Advantages:

* Less memory

Disadvantages:

* Original image is lost
* Some algorithms cannot work correctly in-place

---

## Out-of-Place

```text
Input Buffer
     ↓
Processing
     ↓
Output Buffer
```

Advantages:

* Original preserved
* Often easier algorithm design

Disadvantages:

* More memory required

---

# 26. Medical Imaging Example

Suppose:

```text
Original CT
     ↓
Gaussian Filter
     ↓
Filtered CT
```

You may need:

```text
Original Volume
      +
Output Volume
```

Memory requirement:

$$
2 \times VolumeMemory
$$

For a 500 MB CT:

```text
Original = 500 MB
Filtered = 500 MB

Total = 1 GB
```

This becomes important when designing processing pipelines.

---

# 27. Memory Pooling

Repeated allocation can be expensive:

```text
Allocate Buffer
      ↓
Process
      ↓
Free Buffer

Allocate Again
      ↓
Process
      ↓
Free Again
```

A memory pool can reuse buffers:

```text
Allocate Once
      ↓
Reuse Buffer
      ↓
Reuse Buffer
      ↓
Reuse Buffer
```

Useful in high-performance applications, but adds complexity.

Do not introduce custom memory pools without measuring a real allocation bottleneck.

---

# 28. Common Errors

### Memory Leak

```text
Allocated but never released
```

### Dangling Pointer

```text
Pointer refers to destroyed object
```

### Double Free

```text
Memory released twice
```

### Buffer Overflow

```text
Writing outside allocated buffer
```

### Integer Overflow

```text
Incorrect buffer-size calculation
```

### Excessive Copies

```text
Large volume copied unnecessarily
```

---

# 29. Debugging Memory Problems

Useful approaches include:

### Address Sanitizer

Can help detect:

* Use-after-free
* Buffer overflow
* Memory errors

### Memory Profiling

Monitor:

* RAM usage
* Allocations
* Memory growth

### Assertions

```cpp
assert(x < width);
assert(y < height);
assert(z < depth);
```

### Logging

Log:

```text
Volume dimensions
Voxel type
Expected memory
Actual allocated memory
```

---

# 30. Performance and Engineering Principles

For large medical images:

```text
1. Avoid unnecessary copies
2. Use RAII
3. Prefer contiguous storage
4. Validate dimensions
5. Check size calculations
6. Process cache-friendly memory layouts
7. Reuse buffers when justified
8. Profile before optimizing
```

---

# 31. Medical Imaging Connection

Memory management directly affects:

| Medical Application | Memory Challenge                  |
| ------------------- | --------------------------------- |
| CT Viewer           | Large 3D volumes                  |
| MRI                 | Multiple sequences                |
| PET-CT              | Multiple modalities               |
| MPR                 | Simultaneous slice access         |
| Volume Rendering    | CPU/GPU memory                    |
| Segmentation        | Input + masks + temporary buffers |
| TPS                 | CT + structures + dose grids      |

For your future TPS work, memory ownership and large-volume management are especially important.

---

# Interview Questions

## Basic

1. What is the difference between stack and heap memory?
2. What is a memory leak?
3. What is a dangling pointer?
4. What is RAII?
5. Why use `std::vector` for image data?

## Intermediate

1. How do you calculate memory required for a 3D volume?
2. Why can copying large images be dangerous?
3. What is cache-friendly traversal?
4. What is use-after-free?
5. What is the difference between in-place and out-of-place processing?

## Advanced

1. How would you manage several GB of medical image data?
2. How would you design ownership for a medical image viewer?
3. How would you avoid integer overflow during volume allocation?
4. When would buffer reuse be beneficial?
5. How would you investigate a memory leak in a medical imaging application?

---

# Exercises

### Beginner

1. Calculate memory for:

```text
512 × 512
8-bit
```

2. Calculate memory for:

```text
512 × 512 × 300
16-bit
```

3. Explain stack vs heap in your own words.

### Intermediate

1. Create a safe 2D `Image` class.
2. Create a safe 3D `Volume` class.
3. Implement voxel access.
4. Add bounds checking.

### Advanced

Design a memory strategy for:

```text
CT Volume
+
MRI Volume
+
PET Volume
+
Segmentation
+
Dose Grid
```

Explain:

* Ownership
* Buffer lifetime
* Processing buffers
* Copy avoidance
* GPU transfer strategy

---

# Key Takeaways

* Medical images can consume hundreds of MB or several GB.
* Prefer RAII and standard containers over manual memory management.
* Avoid unnecessary image copies.
* Understand object lifetime and ownership.
* Validate volume dimensions before allocating memory.
* Use `std::size_t` carefully for large buffer calculations.
* Row-major sequential traversal is generally cache-friendly.
* In-place processing saves memory but may destroy original data.
* Out-of-place processing uses more memory but preserves input.
* Memory correctness is essential for reliable medical software.

---

## Current Progress

* **Completed:** Chapter 3
* **Current:** **Level 0 → Module 1 → Chapter 4: Memory Management**
* **Next:** **Level 0 → Module 1 → Chapter 5: Pointers and Buffers**

Say **Next** when you are ready for Chapter 5.
