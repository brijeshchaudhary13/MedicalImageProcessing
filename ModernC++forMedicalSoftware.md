# Level 0 → Module 1 → Chapter 3: Modern C++ for Medical Software

## 1. Chapter Overview

Modern C++ means using safer and more powerful C++ features introduced primarily in:

* C++11
* C++14
* C++17
* C++20

For medical imaging software, Modern C++ helps us build software that is:

* Safer
* Faster
* Easier to maintain
* Less prone to memory leaks
* Better suited for multithreading
* Suitable for large image volumes

A simplified evolution:

```text
C++98
  ↓
Manual memory management
  ↓
C++11+
  ↓
RAII + smart pointers + move semantics + modern STL
```

---

# 2. Why Modern C++ Is Important in Medical Software

Medical imaging applications may handle:

```text
CT Volume
    +
MRI Volume
    +
Segmentation Mask
    +
Dose Grid
    +
3D Visualization
```

These objects can consume hundreds of megabytes or even more.

Bad memory management can cause:

* Memory leaks
* Crashes
* Slow performance
* Undefined behavior
* Incorrect clinical results

Therefore, modern C++ emphasizes:

```text
Safety
+
Clear ownership
+
Performance
+
Maintainability
```

---

# 3. The Most Important Principle: RAII

## What is RAII?

**RAII = Resource Acquisition Is Initialization**

The basic idea:

> A C++ object should automatically manage its resources.

Resources can include:

* Memory
* Files
* Locks
* GPU resources
* Network connections

Example:

```cpp
{
    std::vector<int> data(1000);

} // data is automatically destroyed here
```

You do not need:

```cpp
delete[] data;
```

---

# 4. Raw Pointer vs Modern Ownership

## Old Style

```cpp
Image* image = new Image();

// Process image

delete image;
```

Problem:

If an exception occurs before `delete`:

```text
Memory Leak
```

---

## Modern C++

```cpp
#include <memory>

auto image = std::make_unique<Image>();
```

When `image` leaves scope:

```text
Image automatically destroyed
```

This is much safer.

---

# 5. `std::unique_ptr`

Use `std::unique_ptr` when one object has exclusive ownership.

```cpp
#include <memory>

auto image =
    std::make_unique<Image>();
```

Concept:

```text
unique_ptr
    │
    └── One owner
```

Example medical architecture:

```cpp
class ImageViewer
{
private:
    std::unique_ptr<Image> m_image;
};
```

The viewer owns the image.

---

# 6. `std::shared_ptr`

Use `std::shared_ptr` when multiple objects genuinely need shared ownership.

```cpp
auto image =
    std::make_shared<Image>();
```

Example:

```text
              Image
             /     \
            /       \
        Viewer     Processor
```

Both may access the same object.

However:

> Do not use `shared_ptr` everywhere.

It has:

* Reference counting overhead
* More complicated ownership
* Risk of circular references

---

# 7. `std::weak_ptr`

Suppose:

```text
Viewer ─────→ Image
  ↑             │
  └─────────────┘
```

Two `shared_ptr` objects can create a circular ownership problem.

`std::weak_ptr` provides a non-owning reference.

Conceptually:

```text
shared_ptr = ownership

weak_ptr = observation
```

---

# 8. Move Semantics

Medical images can be very large.

Consider:

```cpp
Image image1 = loadImage();

Image image2 = image1;
```

This may copy a huge image.

Move semantics can transfer resources instead:

```cpp
Image image2 = std::move(image1);
```

Concept:

```text
Before:

image1 → Large Image Data

After move:

image1 → Empty/valid state

image2 → Large Image Data
```

This can avoid expensive copying.

---

# 9. Move Constructor Concept

Example:

```cpp
class Image
{
public:
    Image(Image&& other) noexcept
    {
        // Transfer resources
    }
};
```

In production code, prefer standard containers where possible because:

```cpp
std::vector
std::string
std::unique_ptr
```

already support efficient move semantics.

---

# 10. `auto`

Instead of:

```cpp
std::vector<std::uint16_t>::iterator it;
```

Modern C++:

```cpp
auto it = image.begin();
```

Use `auto` when the type is obvious from the right-hand side.

Good:

```cpp
auto image =
    std::make_unique<Image>();
```

Avoid unclear usage:

```cpp
auto x = getSomethingComplex();
```

when knowing the actual type is important.

---

# 11. Range-Based Loops

Old style:

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    image[i] = 0;
}
```

Modern C++:

```cpp
for (auto& pixel : image)
{
    pixel = 0;
}
```

For read-only operations:

```cpp
for (const auto& pixel : image)
{
    // Read pixel
}
```

---

# 12. `nullptr`

Old C++:

```cpp
Image* image = NULL;
```

Modern C++:

```cpp
Image* image = nullptr;
```

Prefer:

```cpp
nullptr
```

because it is type-safe.

---

# 13. `constexpr`

Use `constexpr` for values known at compile time.

```cpp
constexpr int MaxPixelValue = 255;
```

Example:

```cpp
constexpr int width = 512;
constexpr int height = 512;
```

Benefits:

* Compile-time evaluation where applicable
* Clear intent
* Potential optimization

---

# 14. `enum class`

Old style:

```cpp
enum Modality
{
    CT,
    MRI,
    PET
};
```

Modern C++:

```cpp
enum class Modality
{
    CT,
    MRI,
    PET
};
```

Usage:

```cpp
Modality modality = Modality::CT;
```

This provides stronger type safety.

---

# 15. `std::optional`

Sometimes a function may not have a valid result.

Example:

```cpp
#include <optional>

std::optional<Image>
loadImage(const std::string& fileName);
```

Possible result:

```text
Image loaded
```

or:

```text
No value
```

Usage:

```cpp
auto image = loadImage("scan.dcm");

if (image)
{
    // Image exists
}
```

Useful for APIs where absence of a value is expected.

---

# 16. Lambdas

A lambda is an anonymous function.

Example:

```cpp
auto increaseBrightness =
    [](int pixel)
    {
        return pixel + 20;
    };
```

Lambdas are useful with:

* Algorithms
* Callbacks
* Threads
* Event processing

Example:

```cpp
std::for_each(
    image.begin(),
    image.end(),
    [](auto& pixel)
    {
        pixel = 0;
    });
```

---

# 17. Modern C++ Image Container

A simple modern design:

```cpp
#include <vector>
#include <cstdint>
#include <stdexcept>

class Image
{
public:
    Image(int width, int height)
        : m_width(width),
          m_height(height),
          m_pixels(
              static_cast<std::size_t>(width) *
              static_cast<std::size_t>(height)
          )
    {
        if (width <= 0 || height <= 0)
        {
            throw std::invalid_argument(
                "Invalid image dimensions"
            );
        }
    }

    std::uint16_t& pixel(int x, int y)
    {
        return m_pixels.at(
            static_cast<std::size_t>(y) *
            static_cast<std::size_t>(m_width) +
            static_cast<std::size_t>(x)
        );
    }

    const std::uint16_t& pixel(
        int x,
        int y) const
    {
        return m_pixels.at(
            static_cast<std::size_t>(y) *
            static_cast<std::size_t>(m_width) +
            static_cast<std::size_t>(x)
        );
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

    std::vector<std::uint16_t> m_pixels;
};
```

### Important Modern C++ Features Used

```text
std::vector
const correctness
std::uint16_t
constructor initialization
exception handling
RAII
```

---

# 18. Rule of Zero

This is extremely important.

Suppose your class uses:

```cpp
std::vector
std::string
std::unique_ptr
```

Then usually:

> Do not manually write copy constructor, destructor, copy assignment, or move assignment unless necessary.

Example:

```cpp
class Image
{
private:
    std::vector<std::uint16_t> m_data;
};
```

The compiler-generated operations are usually sufficient.

This is called the **Rule of Zero**.

---

# 19. Rule of Five

If your class manually manages resources, you may need:

1. Destructor
2. Copy constructor
3. Copy assignment operator
4. Move constructor
5. Move assignment operator

Example:

```text
Manual Resource Management
        ↓
Rule of Five
```

But modern C++ encourages:

```text
RAII Containers
        ↓
Rule of Zero
```

For medical imaging software, **Rule of Zero is generally preferable when practical**.

---

# 20. Exception Safety

Suppose image loading fails.

Bad code may leave the application in an invalid state.

Modern C++ should aim for safe cleanup.

Example:

```cpp
try
{
    auto image = loadImage();
}
catch (const std::exception& error)
{
    // Handle error
}
```

RAII ensures local resources are cleaned up during stack unwinding.

---

# 21. Modern C++ and Large Medical Images

Consider:

```text
CT Volume
512 × 512 × 1000
16-bit
```

Memory:

$$
512 \times 512 \times 1000 \times 2
$$

$$
= 524,288,000\ bytes
$$

Approximately:

$$
500\ MB
$$

Now imagine unnecessary copies:

```text
Original CT      500 MB
Copy             500 MB
Processing Copy  500 MB
Backup Copy      500 MB
```

Total:

```text
2 GB+
```

Therefore:

```text
Avoid unnecessary copies
        +
Use move semantics
        +
Use references
        +
Use efficient ownership
```

---

# 22. Modern C++ Processing Example

Brightness adjustment:

```cpp
#include <vector>
#include <algorithm>
#include <cstdint>

void adjustBrightness(
    const std::vector<std::uint16_t>& input,
    std::vector<std::uint16_t>& output,
    int brightness)
{
    output.resize(input.size());

    for (std::size_t i = 0;
         i < input.size();
         ++i)
    {
        const int value =
            static_cast<int>(input[i]) +
            brightness;

        output[i] =
            static_cast<std::uint16_t>(
                std::clamp(
                    value,
                    0,
                    65535
                )
            );
    }
}
```

---

# 23. Performance Considerations

## Prefer Contiguous Memory

```cpp
std::vector<T>
```

provides contiguous storage.

Good for:

* CPU cache
* Image traversal
* SIMD
* GPU transfer

---

## Avoid Repeated Allocation

Bad:

```cpp
for (...)
{
    std::vector<int> temp(1000000);
}
```

Repeated allocation can be expensive.

Prefer reusing buffers when appropriate.

---

## Reserve Capacity

```cpp
std::vector<Image> images;

images.reserve(100);
```

This can reduce reallocations.

---

# 24. Modern C++ and Multithreading

Later, we will study this deeply.

Basic example:

```cpp
#include <thread>

std::thread worker(
    []
    {
        // Heavy image processing
    }
);

worker.join();
```

Medical imaging use:

```text
UI Thread
    ↓
User Interaction

Worker Thread
    ↓
Image Processing
```

Important:

> Never update Qt GUI objects directly from arbitrary worker threads.

---

# 25. Common Mistakes

## Mistake 1: Using `shared_ptr` Everywhere

Wrong idea:

```text
shared_ptr = safest pointer
```

Not necessarily.

Prefer:

```text
Value object
    ↓
unique_ptr when exclusive ownership is needed
    ↓
shared_ptr only for genuine shared ownership
```

---

## Mistake 2: Excessive `std::move`

Do not use:

```cpp
return std::move(image);
```

in every return statement.

Often:

```cpp
return image;
```

is sufficient because of move semantics and copy elision.

---

## Mistake 3: Raw `new` and `delete`

Avoid this:

```cpp
Image* image = new Image();

delete image;
```

Prefer:

```cpp
auto image =
    std::make_unique<Image>();
```

or direct value ownership:

```cpp
Image image;
```

---

## Mistake 4: Ignoring Integer Overflow

For large volumes:

```cpp
int size = width * height * depth;
```

can overflow.

Better:

```cpp
const std::size_t size =
    static_cast<std::size_t>(width) *
    static_cast<std::size_t>(height) *
    static_cast<std::size_t>(depth);
```

---

# 26. Medical Imaging Engineering Connection

```text
Modern C++
      │
      ├── RAII
      │      ↓
      │   Safe image memory
      │
      ├── Smart Pointers
      │      ↓
      │   Clear ownership
      │
      ├── Move Semantics
      │      ↓
      │   Efficient large-volume handling
      │
      ├── std::thread
      │      ↓
      │   Parallel processing
      │
      └── STL
             ↓
        Efficient algorithms
```

---

# Interview Questions

## Basic

1. What is RAII?
2. What is `std::unique_ptr`?
3. What is `std::shared_ptr`?
4. What is move semantics?
5. What is `nullptr`?

## Intermediate

1. Explain the Rule of Zero.
2. Explain the Rule of Five.
3. When should `shared_ptr` be avoided?
4. Why is move semantics important for large medical images?
5. What is `std::optional`?

## Advanced

1. How would you design ownership for a medical image viewer?
2. How would you avoid copying a 500 MB CT volume?
3. How would you design an image-processing pipeline using Modern C++?
4. How would RAII improve reliability in medical software?
5. What problems can circular `shared_ptr` references cause?

---

# Exercises

### Beginner

1. Replace a raw pointer with `std::unique_ptr`.
2. Create an `enum class` for:

```text
CT
MRI
PET
```

3. Create a `std::vector<std::uint16_t>` image buffer.

### Intermediate

1. Create an `Image` class following the Rule of Zero.
2. Add safe pixel access.
3. Implement move-based image transfer.
4. Use `std::optional` for image loading results.

### Advanced

Design a `Volume<T>` class supporting:

```text
Width
Height
Depth
Voxel Type
Contiguous Memory
Safe Access
```

---

# Key Takeaways

* Modern C++ is essential for safe, high-performance medical software.
* Prefer **RAII** over manual resource cleanup.
* Use `std::vector` for managed contiguous image buffers.
* Use `unique_ptr` for exclusive ownership.
* Use `shared_ptr` only when ownership is truly shared.
* Move semantics can avoid expensive large-image copies.
* Prefer the **Rule of Zero** where possible.
* Always consider integer overflow when calculating volume sizes.
* Modern C++ helps create reliable, maintainable medical imaging applications.

---

## Current Progress

* **Completed:** Chapter 2
* **Current:** **Level 0 → Module 1 → Chapter 3: Modern C++ for Medical Software**
* **Next:** **Level 0 → Module 1 → Chapter 4: Memory Management**

Say **Next** when you want to continue to Chapter 4.
