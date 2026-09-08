# Level 0 → Module 1 → Chapter 8: Templates

## 1. Why Templates Are Important in Image Processing

An image-processing program often needs to support different pixel types:

```cpp
uint8_t
uint16_t
int16_t
float
double
```

Without templates, we might need separate classes:

```text
Image8Bit
Image16Bit
ImageFloat
ImageDouble
```

This creates duplicated code.

Templates allow us to write:

```cpp
Image<uint8_t>
Image<int16_t>
Image<float>
Image<double>
```

One generic implementation → multiple pixel types.

---

# 2. What Is a Template?

A template is a blueprint for generating code.

Example:

```cpp
template<typename T>
T add(T a, T b)
{
    return a + b;
}
```

Usage:

```cpp
int result1 = add(10, 20);

float result2 = add(10.5f, 20.5f);
```

The compiler creates appropriate versions based on the type.

Concept:

```text
Template
   │
   ├── int version
   ├── float version
   └── double version
```

---

# 3. Function Templates

Basic example:

```cpp
template<typename T>
T maximum(T a, T b)
{
    return (a > b)
        ? a
        : b;
}
```

Usage:

```cpp
int a = maximum(10, 20);

float b = maximum(10.5f, 20.5f);
```

---

# 4. Image Processing Example

Find the maximum pixel:

```cpp
template<typename T>
T maxPixel(T a, T b)
{
    return a > b
        ? a
        : b;
}
```

Works with:

```cpp
uint8_t
uint16_t
int16_t
float
double
```

---

# 5. Template Syntax

Basic syntax:

```cpp
template<typename T>
```

or:

```cpp
template<class T>
```

Both are valid.

Example:

```cpp
template<typename PixelType>
class Image
{
};
```

`PixelType` is just a descriptive template parameter name.

---

# 6. Generic Image Class

Let's create a reusable image class:

```cpp
#include <vector>
#include <cstddef>

template<typename T>
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

private:

    std::size_t m_width;
    std::size_t m_height;

    std::vector<T> m_pixels;
};
```

Now we can create different image types.

---

# 7. Using the Generic Image Class

8-bit image:

```cpp
Image<std::uint8_t> image8(
    512,
    512
);
```

16-bit CT image:

```cpp
Image<std::int16_t> ctImage(
    512,
    512
);
```

Floating-point processing image:

```cpp
Image<float> processedImage(
    512,
    512
);
```

Concept:

```text
Same Image Class

       Image<T>
          │
    ┌─────┼─────┐
    │     │     │
 uint8  int16  float
```

---

# 8. Adding Pixel Access

```cpp
template<typename T>
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

    T& pixel(
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

    std::vector<T> m_pixels;
};
```

Usage:

```cpp
Image<std::uint8_t> image(
    512,
    512
);

image.pixel(10, 20) = 255;
```

---

# 9. `const` Pixel Access

We should provide two versions:

```cpp
T& pixel(
    std::size_t x,
    std::size_t y)
{
    return m_pixels[
        y * m_width + x
    ];
}
```

For read-only objects:

```cpp
const T& pixel(
    std::size_t x,
    std::size_t y) const
{
    return m_pixels[
        y * m_width + x
    ];
}
```

This is important for efficient image processing.

---

# 10. Complete Basic Template Image Class

```cpp
#include <vector>
#include <cstddef>
#include <cstdint>

template<typename T>
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

    std::size_t width() const
    {
        return m_width;
    }

    std::size_t height() const
    {
        return m_height;
    }

    T& pixel(
        std::size_t x,
        std::size_t y)
    {
        return m_pixels[
            y * m_width + x
        ];
    }

    const T& pixel(
        std::size_t x,
        std::size_t y) const
    {
        return m_pixels[
            y * m_width + x
        ];
    }

private:

    std::size_t m_width;
    std::size_t m_height;

    std::vector<T> m_pixels;
};
```

---

# 11. Template Function for Image Inversion

```cpp
template<typename T>
void invertImage(
    Image<T>& image,
    T maximumValue)
{
    for (
        std::size_t y = 0;
        y < image.height();
        ++y)
    {
        for (
            std::size_t x = 0;
            x < image.width();
            ++x)
        {
            image.pixel(x, y) =
                maximumValue -
                image.pixel(x, y);
        }
    }
}
```

Usage:

```cpp
Image<std::uint8_t> image(
    512,
    512
);

invertImage(
    image,
    static_cast<std::uint8_t>(255)
);
```

---

# 12. Multiple Template Parameters

Templates can have multiple types.

```cpp
template<
    typename InputType,
    typename OutputType
>
OutputType convert(
    InputType value)
{
    return static_cast<OutputType>(
        value
    );
}
```

Usage:

```cpp
float value =
    convert<
        std::int16_t,
        float
    >(100);
```

This is useful in image conversion.

---

# 13. Image Type Conversion

Example:

```cpp
template<
    typename InputType,
    typename OutputType
>
Image<OutputType> convertImage(
    const Image<InputType>& input)
{
    Image<OutputType> output(
        input.width(),
        input.height()
    );

    for (
        std::size_t y = 0;
        y < input.height();
        ++y)
    {
        for (
            std::size_t x = 0;
            x < input.width();
            ++x)
        {
            output.pixel(x, y) =
                static_cast<OutputType>(
                    input.pixel(x, y)
                );
        }
    }

    return output;
}
```

Usage:

```cpp
Image<std::int16_t> ctImage(
    512,
    512
);

Image<float> floatImage =
    convertImage<
        std::int16_t,
        float
    >(ctImage);
```

---

# 14. Why Generic Programming Is Powerful

Without templates:

```text
processUint8Image()

processInt16Image()

processFloatImage()

processDoubleImage()
```

With templates:

```cpp
processImage<T>()
```

Concept:

```text
One Algorithm
     ↓
Many Data Types
```

Benefits:

* Less duplicated code
* Better maintainability
* Compile-time type safety
* Reusable algorithms
* Zero-cost abstraction in many cases

---

# 15. Class Templates

Example:

```cpp
template<typename T>
class PixelProcessor
{
public:

    T multiply(
        T value,
        T factor)
    {
        return value * factor;
    }
};
```

Usage:

```cpp
PixelProcessor<float> processor;
```

---

# 16. Non-Type Template Parameters

Templates can also receive values.

Example:

```cpp
template<
    typename T,
    std::size_t Size
>
class Kernel
{
private:

    T m_data[Size];
};
```

Usage:

```cpp
Kernel<float, 9> kernel;
```

A 3×3 kernel contains:

```text
9 elements
```

---

# 17. Fixed-Size Image Kernel

Example:

```cpp
#include <array>

template<
    typename T,
    std::size_t Width,
    std::size_t Height
>
class ImageKernel
{
public:

    static constexpr
    std::size_t Size =
        Width * Height;

private:

    std::array<T, Size>
        m_values;
};
```

Usage:

```cpp
ImageKernel<float, 3, 3>
gaussianKernel;
```

This can represent:

```text
3 × 3

[ ][ ][ ]
[ ][ ][ ]
[ ][ ][ ]
```

---

# 18. `constexpr` and Templates

Example:

```cpp
template<typename T>
constexpr T square(T value)
{
    return value * value;
}
```

Usage:

```cpp
constexpr int value =
    square(5);
```

Result:

```text
25
```

For suitable inputs, the compiler can evaluate this during compilation.

---

# 19. Template Specialization

Sometimes one type requires special behavior.

General template:

```cpp
template<typename T>
class PixelInfo
{
public:

    static const char* name()
    {
        return "Generic Pixel";
    }
};
```

Specialization:

```cpp
template<>
class PixelInfo<std::uint8_t>
{
public:

    static const char* name()
    {
        return "8-bit Pixel";
    }
};
```

Usage:

```cpp
PixelInfo<std::uint8_t>::name();
```

---

# 20. Why Specialization Can Be Useful

Different pixel types may require different processing rules.

Example:

```text
uint8_t
   ↓
0–255 display range

int16_t
   ↓
Signed medical intensity

float
   ↓
Processing precision
```

Templates provide generic behavior, while specialization allows specific behavior where necessary.

---

# 21. Function Template Specialization

Generic:

```cpp
template<typename T>
T defaultPixel()
{
    return T{};
}
```

Usage:

```cpp
int value =
    defaultPixel<int>();
```

For many cases, overloads are often clearer than function-template specialization when behavior differs substantially.

---

# 22. Template Type Traits

C++ provides information about types.

Include:

```cpp
#include <type_traits>
```

Example:

```cpp
std::is_integral<T>::value
```

Check whether a type is an integer.

Example:

```cpp
template<typename T>
void process(T value)
{
    static_assert(
        std::is_arithmetic<T>::value,
        "T must be numeric"
    );
}
```

This prevents invalid types.

---

# 23. Restricting Image Pixel Types

Example:

```cpp
#include <type_traits>

template<typename T>
class Image
{
    static_assert(
        std::is_arithmetic<T>::value,
        "Pixel type must be numeric"
    );

    // Image implementation
};
```

Now:

```cpp
Image<int>
```

is allowed.

But:

```cpp
Image<std::string>
```

causes a compile-time error.

---

# 24. `if constexpr`

C++17 provides:

```cpp
if constexpr
```

Example:

```cpp
template<typename T>
void printType()
{
    if constexpr (
        std::is_integral<T>::value)
    {
        std::cout
            << "Integer";
    }
    else
    {
        std::cout
            << "Other";
    }
}
```

The compiler selects the appropriate branch based on the type.

---

# 25. Image Processing Example With `if constexpr`

```cpp
template<typename T>
T normalizeValue(
    T value)
{
    if constexpr (
        std::is_integral<T>::value)
    {
        return value;
    }
    else
    {
        return value;
    }
}
```

The above example demonstrates compile-time branching, although real normalization would normally use different scaling logic.

---

# 26. Template-Based 3D Image

Medical imaging commonly uses volumes.

```cpp
template<typename T>
class Volume
{
public:

    Volume(
        std::size_t width,
        std::size_t height,
        std::size_t depth)
        : m_width(width),
          m_height(height),
          m_depth(depth),
          m_data(
              width *
              height *
              depth)
    {
    }

    T& voxel(
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

    std::vector<T> m_data;
};
```

Usage:

```cpp
Volume<std::int16_t>
ctVolume(
    512,
    512,
    300
);
```

---

# 27. Template-Based Image Processing Pipeline

```text
Image<int16_t>
       │
       ▼
convertImage<int16_t, float>
       │
       ▼
Image<float>
       │
       ▼
Filtering
       │
       ▼
Image<float>
       │
       ▼
Display Conversion
       │
       ▼
Image<uint8_t>
```

Templates make the same image framework reusable throughout this pipeline.

---

# 28. Template Aliases

C++ allows aliases.

Example:

```cpp
template<typename T>
using ImageBuffer =
    std::vector<T>;
```

Usage:

```cpp
ImageBuffer<std::uint16_t>
pixels;
```

Medical aliases:

```cpp
using CTImage =
    Image<std::int16_t>;

using DoseImage =
    Image<float>;
```

This improves readability.

---

# 29. Common Template Mistakes

## Mistake 1: Putting Template Implementation Only in `.cpp`

Templates generally need their definitions visible where they are instantiated.

Example structure:

```text
Image.h
```

Usually contains:

```cpp
template<typename T>
class Image
{
    // Definition
};
```

Otherwise linker errors can occur depending on how the template is organized.

---

## Mistake 2: Unnecessary Complex Templates

Avoid creating templates when normal code is simpler.

Bad idea:

```text
Everything is a template
```

Better:

```text
Use templates
when generic behavior is needed.
```

---

## Mistake 3: Too Many Template Parameters

Bad:

```cpp
template<
    typename T1,
    typename T2,
    typename T3,
    typename T4,
    typename T5
>
```

unless genuinely necessary.

Complex templates can become difficult to understand and maintain.

---

## Mistake 4: Poor Error Messages

Template errors can be complex.

Use:

```cpp
static_assert
```

and meaningful constraints to improve compile-time diagnostics.

---

# 30. Templates vs Inheritance

| Templates                                 | Inheritance                           |
| ----------------------------------------- | ------------------------------------- |
| Compile-time polymorphism                 | Runtime polymorphism                  |
| Different types generated at compile time | Virtual functions often used          |
| No virtual dispatch required              | Can support runtime behavior changes  |
| Excellent for generic image pixel types   | Useful for runtime-extensible designs |

For image pixel types:

```cpp
Image<T>
```

templates are often a natural design.

---

# 31. Templates in Medical Imaging Libraries

Generic programming is widely used in image-processing libraries.

Conceptually:

```text
Image<PixelType>
```

allows algorithms to operate on:

```text
8-bit
16-bit
32-bit
float
double
```

This approach is particularly useful when the algorithm is independent of the specific pixel storage type.

---

# 32. Practical Exercise

Create:

```cpp
template<typename T>
class Image
```

with:

```text
Width
Height
Pixel Buffer
Pixel Access
```

Then create:

```cpp
Image<std::uint8_t>
Image<std::int16_t>
Image<float>
```

Test:

```text
Pixel Read
Pixel Write
Width
Height
```

---

# 33. Mini Project

## Generic Medical Image Container

Design:

```text
Image<T>
```

Features:

```text
Image<T>
│
├── Width
├── Height
├── Pixel Buffer
├── Pixel Access
├── Fill()
├── Min()
├── Max()
└── Convert<U>()
```

Example:

```cpp
Image<std::int16_t>
ctImage;
```

Convert:

```cpp
Image<float>
processingImage;
```

Then later:

```cpp
Image<std::uint8_t>
displayImage;
```

---

# 34. Interview Questions

### Beginner

1. What is a template?
2. What is a function template?
3. What is a class template?
4. What is `typename T`?
5. What are template parameters?

### Intermediate

1. Why are templates useful in image processing?
2. What is template specialization?
3. What are non-type template parameters?
4. What is `static_assert`?
5. What is `if constexpr`?

### Advanced

1. Why are templates useful for generic image containers?
2. Templates vs runtime polymorphism?
3. Why must template definitions often be visible at the point of instantiation?
4. How would you restrict a template to numeric pixel types?
5. How would you design `Image<T>` for 2D and 3D medical images?

---

# 35. Chapter Summary

You learned:

* Function templates
* Class templates
* Generic programming
* `Image<T>`
* Multiple template parameters
* Type conversion templates
* Non-type template parameters
* Fixed-size kernels
* `constexpr`
* Template specialization
* Type traits
* `static_assert`
* `if constexpr`
* Template aliases
* Template-based medical image containers
* 3D volume templates

---

## Current Progress

* **Completed:** Chapter 7 — STL for Image Processing
* **Completed:** Chapter 8 — Templates
* **Next:** **Chapter 9 — Multithreading Fundamentals**

In the next chapter, we'll learn how to speed up large image-processing operations using:

```text
Threads
Race Conditions
Mutex
Synchronization
Thread Safety
Parallel Image Processing
```

Say **Next** to continue.
