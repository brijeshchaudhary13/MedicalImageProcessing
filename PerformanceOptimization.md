# Level 0 → Module 1 → Chapter 10: Performance Optimization

Performance optimization is extremely important in **medical image processing** because images can be very large.

Examples:

```text
X-ray       → 2048 × 2048
CT Slice    → 512 × 512
CT Volume   → 512 × 512 × 300
MRI Volume  → Large 3D datasets
```

The goal is not simply to make code “fast.” The goal is:

```text
Correct
   ↓
Measured
   ↓
Optimized
```

> **First make it correct. Then measure it. Then optimize the real bottleneck.**

---

# 1. What Is Performance Optimization?

Performance optimization means improving:

* Execution speed
* Memory usage
* CPU utilization
* Cache efficiency
* Responsiveness
* Scalability

For medical imaging:

```text
Large Image
     ↓
Load Faster
     ↓
Process Faster
     ↓
Use Less Memory
     ↓
Keep UI Responsive
```

---

# 2. The Golden Rule: Measure First

Do not optimize based only on assumptions.

Bad approach:

```text
"I think this function is slow."
        ↓
Optimize it
```

Better:

```text
Measure
   ↓
Find Bottleneck
   ↓
Optimize
   ↓
Measure Again
```

---

# 3. What Is a Bottleneck?

A bottleneck is the part of a program consuming the most important resource, often execution time.

Example:

```text
Program Execution Time = 10 seconds

Image Loading       = 1 second
Image Filtering     = 8 seconds
UI Update           = 1 second
```

The bottleneck is:

```text
Image Filtering
```

Optimizing image loading from:

```text
1 second → 0.5 second
```

will not solve the main problem.

---

# 4. Algorithm Complexity

Before low-level optimization, understand algorithm complexity.

Common complexities:

| Complexity   | Meaning            |
| ------------ | ------------------ |
| `O(1)`       | Constant           |
| `O(log n)`   | Logarithmic        |
| `O(n)`       | Linear             |
| `O(n log n)` | Linear-logarithmic |
| `O(n²)`      | Quadratic          |

---

# 5. Example: Linear Processing

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    image[i] *= 2;
}
```

Complexity:

```text
O(n)
```

Every pixel is processed once.

For:

```text
1,000,000 pixels
```

approximately:

```text
1,000,000 operations
```

---

# 6. Example: Quadratic Complexity

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    for (std::size_t j = 0;
         j < image.size();
         ++j)
    {
        // Work
    }
}
```

Complexity:

```text
O(n²)
```

For large images, this can become extremely expensive.

---

# 7. Algorithm Optimization Comes First

Consider searching for a pixel value.

Bad:

```text
Nested unnecessary loops
```

Better:

```text
Use appropriate algorithm
```

General rule:

```text
Better Algorithm
       >
Micro Optimization
```

A poor algorithm cannot usually be fixed by tiny syntax-level optimizations.

---

# 8. Avoid Unnecessary Image Copies

Very important for medical imaging.

Bad:

```cpp
void process(
    std::vector<std::int16_t> image)
{
}
```

This can copy the entire image.

Better:

```cpp
void process(
    const std::vector<std::int16_t>& image)
{
}
```

Concept:

```text
Bad:

Original Image
     ↓ Copy
Function


Better:

Original Image
     ↓ Reference
Function
```

---

# 9. How Large Can Copies Become?

Example CT volume:

```text
512 × 512 × 300
```

Total voxels:

```text
78,643,200
```

For `int16_t`:

```text
2 bytes per voxel
```

Memory:

```text
78,643,200 × 2
≈ 150 MB
```

One unnecessary copy can consume approximately another:

```text
150 MB
```

This can significantly affect performance.

---

# 10. Move Semantics

Move semantics can avoid unnecessary copies.

Example:

```cpp
std::vector<int> createImage()
{
    std::vector<int> image(1000000);

    return image;
}
```

Modern C++ can often move or elide copies.

Explicit move example:

```cpp
std::vector<int> image2 =
    std::move(image1);
```

Conceptually:

```text
Before:

image1 → Large Memory Buffer

After move:

image2 → Large Memory Buffer
```

Ownership/resources are transferred instead of performing a full element-by-element copy.

Do not use `std::move()` blindly; the moved-from object remains valid but its previous contents should not be assumed.

---

# 11. Cache Efficiency

Modern CPUs are much faster when data access follows memory locality.

Example:

```text
Contiguous Memory

[Pixel][Pixel][Pixel][Pixel]
```

This is cache-friendly.

`std::vector` is useful because elements are contiguous.

---

# 12. Sequential Access

Better:

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    process(image[i]);
}
```

Memory access:

```text
0 → 1 → 2 → 3 → 4
```

This generally has good locality.

---

# 13. Random Access

Potentially less cache-friendly:

```text
0 → 10000 → 25 → 90000 → 5
```

The CPU may need to load data from different cache locations repeatedly.

---

# 14. Row-Major Image Processing

Suppose image data is stored as:

```text
Row 0 → Pixel 0, Pixel 1, Pixel 2...
Row 1 → Pixel 0, Pixel 1, Pixel 2...
```

Usually process memory in storage order.

For a typical row-major buffer:

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

# 15. Why Loop Order Matters

For row-major memory:

Better:

```text
Row
 └── Pixel → Pixel → Pixel
```

Less cache-friendly:

```text
Column
 │
 │ Jump
 │
 ▼
Next Row
```

For large images, memory access patterns can strongly affect performance.

---

# 16. Avoid Repeated Calculations

Bad:

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    double value =
        std::sqrt(
            someConstant * someConstant
        );

    process(value);
}
```

If the value is constant:

```cpp
const double value =
    std::sqrt(
        someConstant * someConstant
    );

for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    process(value);
}
```

Calculate once instead of millions of times.

---

# 17. Reserve Vector Capacity

Bad:

```cpp
std::vector<int> pixels;

for (int i = 0;
     i < 1000000;
     ++i)
{
    pixels.push_back(i);
}
```

The vector may reallocate multiple times.

Better:

```cpp
std::vector<int> pixels;

pixels.reserve(1000000);

for (int i = 0;
     i < 1000000;
     ++i)
{
    pixels.push_back(i);
}
```

---

# 18. Preallocate Image Memory

For a known image size:

```cpp
std::vector<std::uint16_t> image(
    width * height
);
```

This is often preferable to repeated growth.

---

# 19. Use References Where Appropriate

Bad:

```cpp
for (auto pixel : image)
{
    process(pixel);
}
```

For small pixel types, copying may be insignificant.

For larger objects:

```cpp
for (const auto& pixel : image)
{
    process(pixel);
}
```

For modification:

```cpp
for (auto& pixel : image)
{
    pixel = transform(pixel);
}
```

---

# 20. Avoid Excessive Dynamic Allocation

Bad design:

```text
Process Pixel
     ↓
new
     ↓
delete

Repeat millions of times
```

Repeated allocations are expensive.

Better:

```text
Allocate Buffer Once
       ↓
Reuse Buffer
```

Example:

```cpp
std::vector<float> buffer(
    width * height
);

// Reuse buffer
```

---

# 21. Memory Reuse

Bad:

```cpp
for (...)
{
    std::vector<float> temporary(
        width * height
    );

    process(temporary);
}
```

Better:

```cpp
std::vector<float> temporary(
    width * height
);

for (...)
{
    process(temporary);
}
```

This avoids repeated large allocations.

---

# 22. Avoid Frequent UI Updates

Medical image processing:

Bad:

```text
Process Pixel
     ↓
Update UI

Process Pixel
     ↓
Update UI
```

This can be extremely slow.

Better:

```text
Process Image Region
      ↓
Update Progress Occasionally
```

Example:

```text
0%
10%
20%
...
100%
```

---

# 23. Profiling

A profiler helps identify where time is spent.

Concept:

```text
Application
     ↓
Profiler
     ↓
Function Timing
     ↓
CPU Usage
     ↓
Memory Usage
     ↓
Hotspots
```

Look for:

* Functions consuming most CPU time
* Excessive allocations
* Memory bottlenecks
* Lock contention
* Expensive loops

---

# 24. Simple Timing With `std::chrono`

```cpp
#include <chrono>

auto start =
    std::chrono::high_resolution_clock::now();

// Image processing

auto end =
    std::chrono::high_resolution_clock::now();

auto duration =
    std::chrono::duration_cast<
        std::chrono::milliseconds
    >(
        end - start
    );

std::cout
    << duration.count()
    << " ms\n";
```

---

# 25. Benchmarking Correctly

Do not measure only once.

Bad:

```text
Run once
   ↓
Result = 100 ms
```

Better:

```text
Warm-up
Run multiple times
Measure
Average / distribution
```

Performance can vary due to:

* CPU scheduling
* Background applications
* Cache state
* Thermal conditions

---

# 26. Compiler Optimization

Compilers can optimize code.

Common optimization levels include concepts such as:

```text
Debug Build
     ↓
Minimal optimization

Release Build
     ↓
Optimized code
```

Always benchmark performance-sensitive code using an appropriate optimized build configuration.

Debug builds can be dramatically slower.

---

# 27. Function Call Overhead

For tiny functions called millions of times:

```cpp
int square(int x)
{
    return x * x;
}
```

Modern compilers may inline suitable functions.

However:

> Do not manually optimize everything with `inline`.

The compiler is generally better positioned to make many inlining decisions.

---

# 28. SIMD Fundamentals

SIMD means:

```text
Single Instruction
Multiple Data
```

Instead of:

```text
Instruction → Pixel 1

Instruction → Pixel 2

Instruction → Pixel 3

Instruction → Pixel 4
```

SIMD conceptually performs:

```text
One Instruction
      ↓
Pixel 1
Pixel 2
Pixel 3
Pixel 4
```

This can be useful for:

* Image filtering
* Brightness adjustment
* Image arithmetic
* Pixel transformations

---

# 29. SIMD Conceptual Example

Scalar:

```text
Pixel 1 → ×2
Pixel 2 → ×2
Pixel 3 → ×2
Pixel 4 → ×2
```

SIMD:

```text
[Pixel1 Pixel2 Pixel3 Pixel4]
              ↓
             ×2
              ↓
[Result Result Result Result]
```

Modern compilers may automatically vectorize suitable loops.

---

# 30. Help the Compiler Vectorize

Simple loops are often easier to optimize.

Example:

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    output[i] =
        input[i] * 2.0f;
}
```

Complex dependencies may prevent vectorization.

---

# 31. Multithreading vs SIMD

```text
Multithreading
      ↓
Multiple CPU Cores
```

```text
SIMD
      ↓
Multiple Data Elements Per Core Instruction
```

They can often work together:

```text
CPU
│
├── Core 1 → SIMD
├── Core 2 → SIMD
├── Core 3 → SIMD
└── Core 4 → SIMD
```

---

# 32. Memory Bandwidth

Image processing may become limited by memory bandwidth.

Example:

```text
CPU can calculate very fast
          ↓
But
          ↓
Data cannot arrive fast enough
```

This is why adding more threads does not always improve performance.

---

# 33. Minimize Memory Passes

Suppose we perform:

```text
Pass 1 → Brightness
Pass 2 → Contrast
Pass 3 → Clamp
```

Potentially:

```text
Read image 3 times
Write image multiple times
```

Sometimes operations can be combined:

```text
Read Pixel
    ↓
Brightness
    ↓
Contrast
    ↓
Clamp
    ↓
Write Result
```

One pass can reduce memory traffic when mathematically and architecturally appropriate.

---

# 34. Example: Combined Processing

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    float value =
        static_cast<float>(image[i]);

    value += brightness;

    value *= contrast;

    value =
        std::clamp(
            value,
            minValue,
            maxValue
        );

    output[i] = value;
}
```

This avoids creating unnecessary intermediate full-size images.

---

# 35. Thread Pool Concept

Bad:

```text
Task
 ↓
Create Thread
 ↓
Finish
 ↓
Destroy Thread

Repeat
```

Thread creation has overhead.

Better:

```text
Thread Pool

Worker 1
Worker 2
Worker 3
Worker 4
     ↓
Reusable Tasks
```

This is especially useful when processing many image-processing tasks.

---

# 36. Avoid Lock Contention

Bad:

```text
Thread 1 ──┐
Thread 2 ──┼── Wait for Mutex
Thread 3 ──┤
Thread 4 ──┘
```

Too much waiting:

```text
Parallel Program
       ↓
Becomes Almost Serial
```

Better:

```text
Independent Work
      ↓
Local Results
      ↓
Minimal Synchronization
```

---

# 37. Optimize Data Layout

For image processing, data layout matters.

### Array of Structures

```text
Pixel
├── R
├── G
└── B
```

Memory:

```text
RGB RGB RGB RGB
```

### Structure of Arrays

```text
R R R R
G G G G
B B B B
```

The better choice depends on the algorithm and access pattern.

---

# 38. Example Medical Image Pipeline

```text
DICOM File
    ↓
Read Pixel Data
    ↓
Convert to Processing Type
    ↓
Filtering
    ↓
Segmentation
    ↓
Statistics
    ↓
Display
```

Possible optimization areas:

```text
File I/O
Memory Copies
Algorithms
CPU Cache
Multithreading
SIMD
GPU
```

---

# 39. Optimization Order

A recommended order:

```text
1. Correctness
      ↓
2. Measure
      ↓
3. Better Algorithm
      ↓
4. Better Data Structure
      ↓
5. Reduce Copies
      ↓
6. Improve Memory Access
      ↓
7. Multithreading
      ↓
8. SIMD
      ↓
9. GPU
      ↓
10. Measure Again
```

---

# 40. Common Performance Mistakes

### Mistake 1: Optimizing Before Measuring

```text
Guess
 ↓
Optimize
```

Wrong approach.

---

### Mistake 2: Copying Large Images

```cpp
process(imageCopy);
```

may consume significant time and memory.

---

### Mistake 3: Using Debug Builds for Benchmarking

Debug performance does not represent optimized release performance.

---

### Mistake 4: Creating Too Many Threads

More threads:

```text
≠ Always Faster
```

---

### Mistake 5: Locking Every Pixel

```text
Lock
Process Pixel
Unlock
```

This can severely reduce parallel performance.

---

### Mistake 6: Repeated Memory Allocation

Repeated:

```text
new
delete
malloc
free
vector allocation
```

inside hot loops can be expensive.

---

### Mistake 7: Ignoring Algorithm Complexity

Changing:

```text
O(n²)
```

to:

```text
slightly faster O(n²)
```

may still be worse than finding an:

```text
O(n log n)
```

or:

```text
O(n)
```

solution.

---

# 41. Practical Exercise

Create a large image:

```cpp
std::vector<float> image(
    4096 * 4096
);
```

Implement brightness processing.

### Version 1

```text
Basic single-threaded loop
```

### Version 2

```text
Avoid unnecessary copies
```

### Version 3

```text
Reuse output buffers
```

### Version 4

```text
Multithread the work
```

Measure every version.

Create a comparison:

| Version          | Time | Memory |
| ---------------- | ---: | -----: |
| Basic            |    ? |      ? |
| Optimized memory |    ? |      ? |
| Multithreaded    |    ? |      ? |

---

# 42. Mini Project

## High-Performance Medical Image Processor

Create:

```text
HighPerformanceImageProcessor
```

Pipeline:

```text
Input Image
     ↓
Memory-Efficient Buffer
     ↓
Optimized Algorithm
     ↓
Parallel Processing
     ↓
SIMD-Friendly Operations
     ↓
Output Image
```

Features:

* Brightness
* Contrast
* Thresholding
* Min/Max
* Mean
* Histogram
* Parallel processing
* Timing

---

# 43. Interview Questions

## Beginner

1. What is performance optimization?
2. What is a bottleneck?
3. Why should we measure before optimizing?
4. What is `O(n)` complexity?
5. Why can image copying be expensive?

## Intermediate

1. Why is cache locality important?
2. Why does loop order matter?
3. What does `reserve()` do?
4. What is move semantics?
5. Why should temporary image buffers be reused?

## Advanced

1. How would you optimize a slow 3D CT processing pipeline?
2. Why might multithreading not improve performance?
3. What is memory bandwidth limitation?
4. How can SIMD improve image processing?
5. How would you profile and optimize a medical imaging application?
6. When should you use a thread pool?
7. How can reducing memory passes improve performance?

---

# 44. Chapter Summary

You learned:

* Performance measurement
* Bottleneck identification
* Algorithm complexity
* Avoiding unnecessary copies
* Move semantics
* Cache efficiency
* Sequential memory access
* Loop ordering
* Preallocation
* `reserve()`
* Memory reuse
* UI performance
* Profiling
* Benchmarking
* Release vs debug performance
* Compiler optimization
* SIMD fundamentals
* Multithreading performance
* Memory bandwidth
* Thread pools
* Lock contention
* Data layout
* Optimization strategy

---

# Level 0 → Module 1 Progress

✅ Chapter 7 — STL for Image Processing
✅ Chapter 8 — Templates
✅ Chapter 9 — Multithreading Fundamentals
✅ Chapter 10 — Performance Optimization

## Level 0 / Module 1 foundation is now progressing correctly according to your index.

The next chapter should follow **the exact index you provide**.
