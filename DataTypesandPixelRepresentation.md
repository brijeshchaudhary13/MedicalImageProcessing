# Level 0 → Module 1 → Chapter 7: Data Types and Pixel Representation

## 1. Chapter Overview

A digital image is ultimately a collection of numbers.

For example:

```text
Pixel Values:

0   25   80
120 180 255
```

The **data type** determines:

* How much memory each pixel uses
* Minimum value
* Maximum value
* Whether negative values are supported
* Precision
* Memory consumption

In medical imaging, choosing the correct pixel representation is extremely important.

```text
Image Data
    ↓
Data Type
    ↓
Pixel Value Range
    ↓
Memory + Precision
```

---

# 2. Common C++ Data Types

## Integer Types

```cpp
std::uint8_t
std::int8_t

std::uint16_t
std::int16_t

std::uint32_t
std::int32_t

std::uint64_t
std::int64_t
```

Include:

```cpp
#include <cstdint>
```

---

## Floating-Point Types

```cpp
float
double
```

These are important for:

* Image processing
* Image registration
* Interpolation
* Dose calculation
* Machine learning

---

# 3. Signed vs Unsigned

## Unsigned

Cannot represent negative values.

Example:

```cpp
std::uint8_t pixel = 200;
```

Typical range:

$$
0 \rightarrow 255
$$

---

## Signed

Can represent negative and positive values.

Example:

```cpp
std::int16_t value = -100;
```

This is important for CT images.

---

# 4. `uint8_t` — 8-bit Unsigned Pixel

```cpp
std::uint8_t pixel;
```

Typical range:

$$
0 \rightarrow 255
$$

Memory:

```text
1 byte per pixel
```

Common use:

```text
Grayscale Images
Binary Masks
Segmentation Masks
8-bit Images
```

Example:

```cpp
std::uint8_t pixel = 255;
```

---

# 5. Why 8-bit Images Are Common

For an 8-bit grayscale image:

```text
0
↓
Black
```

```text
255
↓
White
```

Values between:

```text
1 → 254
```

represent different gray levels.

Conceptually:

```text
0 ─────────────────── 255

Black                White
```

---

# 6. `uint16_t` — 16-bit Unsigned Pixel

```cpp
std::uint16_t pixel;
```

Range:

$$
0 \rightarrow 65535
$$

Memory:

```text
2 bytes per pixel
```

Useful for:

* High bit-depth images
* Scientific imaging
* Some medical imaging data
* Processing intermediate values

Example:

```cpp
std::uint16_t pixel = 50000;
```

---

# 7. `int16_t` — 16-bit Signed Pixel

```cpp
std::int16_t pixel;
```

Range:

$$
-32768 \rightarrow 32767
$$

This type is particularly important in CT imaging.

A simplified CT intensity range can include negative values.

Example:

```text
Air      → Negative
Water    → Around 0
Bone     → Positive
```

Therefore, signed image data can be necessary.

---

# 8. CT and Hounsfield Units

CT images are commonly interpreted using **Hounsfield Units (HU)**.

Typical examples:

| Material    | Approximate HU |
| ----------- | -------------: |
| Air         |          -1000 |
| Lung        |   -700 to -500 |
| Fat         |    -100 to -50 |
| Water       |              0 |
| Soft Tissue |      20 to 100 |
| Bone        | +700 to +3000+ |

Concept:

```text
-1000 ───── 0 ───── +1000 ───── +3000
 Air       Water       Bone
```

Because CT values can be negative:

```cpp
std::int16_t
```

is often useful for representing CT intensity data.

---

# 9. Important: Stored Pixel Value vs Physical Value

In medical imaging, the stored pixel value may not always directly equal the physical intensity.

For example:

```text
Stored Pixel Value
        ↓
Rescale Transformation
        ↓
Physical Value / HU
```

A common linear relationship is:

$$
\boxed{
Output = StoredValue \times Slope + Intercept
}
$$

For CT:

$$
HU =
StoredValue \times RescaleSlope
+
RescaleIntercept
$$

Example:

```text
Stored Value = 1000
Slope = 1
Intercept = -1024
```

Then:

$$
HU = 1000 \times 1 - 1024
$$

$$
HU = -24
$$

This distinction is extremely important when loading DICOM images.

---

# 10. `float` Pixel Representation

```cpp
float pixel;
```

A floating-point value can represent decimal values.

Example:

```cpp
float value = 125.75f;
```

Useful for:

* Filtering
* Interpolation
* Normalization
* Registration
* Dose calculation
* AI preprocessing

Example:

```text
Original Pixel
      ↓
Gaussian Filter
      ↓
Floating-Point Result
```

Intermediate results may not be whole numbers.

---

# 11. Why Integer Processing Can Lose Information

Suppose:

```text
Pixel = 100
```

During processing:

$$
100 \div 3 = 33.333
$$

With integer storage:

```text
33
```

Decimal information is lost.

With:

```cpp
float
```

you can store approximately:

```text
33.333
```

Therefore, many algorithms convert input data into floating-point form during processing.

---

# 12. `double`

```cpp
double value;
```

Provides greater precision than `float`.

Common uses:

* Scientific computation
* Mathematical algorithms
* Geometric calculations
* Image registration
* Dose calculation

However:

```text
double
```

generally uses more memory than:

```text
float
```

Therefore:

```text
More Precision
       ↕
More Memory
```

Choose according to actual algorithm requirements.

---

# 13. Pixel Representation Table

| Type       |      Typical Size | Typical Range                   |
| ---------- | ----------------: | ------------------------------- |
| `uint8_t`  |            1 byte | 0 to 255                        |
| `int8_t`   |            1 byte | -128 to 127                     |
| `uint16_t` |           2 bytes | 0 to 65535                      |
| `int16_t`  |           2 bytes | -32768 to 32767                 |
| `uint32_t` |           4 bytes | 0 to 4,294,967,295              |
| `int32_t`  |           4 bytes | -2,147,483,648 to 2,147,483,647 |
| `float`    | typically 4 bytes | Floating-point                  |
| `double`   | typically 8 bytes | Higher precision floating-point |

---

# 14. Bit Depth

**Bit depth** means how many bits are used to represent one pixel.

Examples:

```text
8-bit
16-bit
32-bit
```

Number of possible values:

$$
2^{bits}
$$

Examples:

### 8-bit

$$
2^8 = 256
$$

Values:

```text
0 → 255
```

### 16-bit

$$
2^{16} = 65536
$$

Values:

```text
0 → 65535
```

---

# 15. Bit Depth and Image Quality

Higher bit depth allows more possible intensity values.

```text
8-bit
256 levels
```

```text
16-bit
65536 levels
```

Concept:

```text
More Bits
   ↓
More Intensity Levels
   ↓
Potentially Better Intensity Precision
```

However:

```text
More Bits
   ↓
More Memory
```

---

# 16. Memory Calculation

Formula:

$$
Memory =
Width \times Height \times BytesPerPixel
$$

Example:

```text
1024 × 1024
16-bit
```

Since:

$$
16\ bits = 2\ bytes
$$

$$
1024 \times 1024 \times 2
$$

$$
= 2,097,152\ bytes
$$

Approximately:

```text
2 MB
```

---

# 17. 3D Volume Memory

For:

```text
512 × 512 × 500
```

with:

```text
16-bit voxels
```

$$
512 \times 512 \times 500 \times 2
$$

$$
= 262,144,000\ bytes
$$

Approximately:

```text
250 MB
```

If converted to `float`:

```text
4 bytes per voxel
```

Memory becomes approximately:

```text
500 MB
```

This is an important processing trade-off.

---

# 18. Pixel Type Conversion

Example:

```cpp
std::uint8_t pixel8 = 100;

std::uint16_t pixel16 =
    static_cast<std::uint16_t>(pixel8);
```

This is safe because the 8-bit value fits inside 16-bit storage.

---

## Dangerous Conversion

```cpp
std::uint16_t value = 1000;

std::uint8_t small =
    static_cast<std::uint8_t>(value);
```

`uint8_t` cannot represent 1000.

Information may be lost.

---

# 19. Overflow

Example:

```cpp
std::uint8_t value = 250;

value = value + 20;
```

The result cannot fit in an 8-bit unsigned value.

Therefore, image-processing operations should often use a larger intermediate type.

Example:

```cpp
int value =
    static_cast<int>(pixel) +
    brightness;
```

Then clamp:

```cpp
if (value > 255)
{
    value = 255;
}

if (value < 0)
{
    value = 0;
}
```

Finally:

```cpp
pixel =
    static_cast<std::uint8_t>(value);
```

---

# 20. Clamping Pixel Values

A common image-processing operation:

```cpp
#include <algorithm>
#include <cstdint>

std::uint8_t adjustBrightness(
    std::uint8_t pixel,
    int brightness)
{
    int value =
        static_cast<int>(pixel) +
        brightness;

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
```

This prevents values from going outside:

$$
0 \rightarrow 255
$$

---

# 21. Signed vs Unsigned Problem

Consider:

```cpp
std::uint8_t pixel = 10;

pixel = pixel - 50;
```

You might expect:

```text
-40
```

But an unsigned value cannot represent negative numbers.

This can produce an unexpected wrapped result.

Better:

```cpp
int value =
    static_cast<int>(pixel) - 50;

value =
    std::max(value, 0);
```

Then convert back safely.

---

# 22. Binary Images

A binary image contains two states.

Example:

```text
0 → Background
1 → Foreground
```

Example:

```cpp
std::uint8_t mask[5] =
{
    0,
    1,
    1,
    0,
    1
};
```

Medical use:

```text
Tumor Mask

0 → Not Tumor
1 → Tumor
```

Binary masks are widely used in segmentation.

---

# 23. Label Images

A label image can represent multiple structures.

Example:

```text
0 → Background
1 → Liver
2 → Kidney
3 → Tumor
4 → Bone
```

Possible representation:

```cpp
std::uint8_t label;
```

or a larger integer type when more labels are required.

---

# 24. Grayscale Image Representation

Example:

```text
0     64     128     192     255

Black                        White
```

For a grayscale image:

```cpp
std::vector<std::uint8_t> image;
```

Each element represents intensity.

---

# 25. Color Image Representation

A color pixel commonly contains multiple channels.

Example:

```text
RGB

Red
Green
Blue
```

For an 8-bit RGB pixel:

```text
R = 255
G = 0
B = 0
```

Result:

```text
Red
```

Memory layout may be:

```text
R G B R G B R G B
```

This is called interleaved storage.

---

# 26. Pixel Channels

Examples:

```text
Grayscale → 1 channel

RGB → 3 channels

RGBA → 4 channels
```

Memory calculation:

$$
Memory =
Width
\times
Height
\times
Channels
\times
BytesPerChannel
$$

Example:

```text
1920 × 1080
RGB
8-bit/channel
```

$$
1920 \times 1080 \times 3
$$

$$
= 6,220,800\ bytes
$$

Approximately:

```text
6 MB
```

---

# 27. Medical Image Pixel Types

Common examples:

| Modality / Data   | Possible Representation                                          |
| ----------------- | ---------------------------------------------------------------- |
| X-ray             | 8-bit or 16-bit                                                  |
| CT Stored Pixels  | Often integer data                                               |
| CT Processing     | `int16_t`, `float`, etc.                                         |
| MRI               | Integer or floating-point depending on representation/processing |
| PET               | Integer or floating-point depending on representation/processing |
| Segmentation Mask | `uint8_t`                                                        |
| Dose Grid         | Often floating-point                                             |
| AI Input          | Often `float`                                                    |

The exact representation depends on acquisition, file format, metadata, and processing pipeline.

---

# 28. Data Type Selection

When choosing a pixel type, ask:

### 1. What range is required?

```text
0–255?
```

or:

```text
-1000–3000?
```

### 2. Are negative values required?

If yes:

```text
Signed type
```

### 3. Are decimal values required?

If yes:

```text
float / double
```

### 4. How much memory is available?

Large volumes may strongly affect the choice.

---

# 29. Processing Pipeline Example

Consider CT processing:

```text
DICOM Stored Pixels
        ↓
Apply Rescale
        ↓
HU Values
        ↓
Convert for Processing
        ↓
Filtering
        ↓
Segmentation
        ↓
Visualization
```

Different stages may use different data types.

Example:

```text
Stored Pixels
      ↓
int16_t
      ↓
Processing
      ↓
float
      ↓
Final Display
      ↓
uint8_t
```

This is called **data type conversion through the pipeline**.

---

# 30. Windowing Example

A CT image may contain values:

```text
-1000 → Air
0     → Water
1000+ → Bone
```

A monitor cannot always display the entire intensity range effectively using only 8-bit display values.

Therefore:

```text
Wide Medical Range
        ↓
Windowing
        ↓
Selected Range
        ↓
8-bit Display
```

For example:

```text
HU Range
-1000 to +3000
```

can be mapped to:

```text
Display
0 to 255
```

We will study windowing deeply later.

---

# 31. Common Mistakes

## Mistake 1: Using `uint8_t` for Everything

```text
8-bit
```

may not have enough range for medical data.

---

## Mistake 2: Ignoring Negative Values

CT values can include negative intensities.

Using the wrong unsigned representation can cause incorrect processing.

---

## Mistake 3: Integer Overflow

Example:

```cpp
std::uint8_t a = 200;
std::uint8_t b = 100;
```

Direct operations require careful handling when results exceed the target range.

---

## Mistake 4: Losing Precision

```text
float → int
```

can discard fractional information.

---

## Mistake 5: Ignoring Metadata

The raw stored value may require interpretation using metadata.

For medical images:

```text
Stored Value
+
Metadata
=
Meaningful Physical Value
```

---

# 32. Interview Questions

### Basic

1. What is bit depth?
2. What is the range of `uint8_t`?
3. What is the range of `uint16_t`?
4. What is the difference between signed and unsigned integers?
5. Why use floating-point numbers in image processing?

### Intermediate

1. Why are negative values important in CT?
2. What is pixel type conversion?
3. What is overflow?
4. How do you safely adjust brightness in an 8-bit image?
5. What is the difference between a binary and label image?

### Advanced

1. How would you design a pixel type abstraction?
2. How do stored pixel values differ from physical values?
3. Why might a CT pipeline use multiple data types?
4. How does bit depth affect memory consumption?
5. How would you prevent precision loss during an image-processing pipeline?

---

# 33. Exercises

## Beginner

1. Calculate the range of an 8-bit unsigned pixel.
2. Calculate the number of values represented by 12 bits.
3. Calculate memory for:

```text
512 × 512
16-bit image
```

---

## Intermediate

1. Write a function that clamps an integer value to:

```text
0–255
```

2. Convert:

```text
uint8_t
```

to:

```text
float
```

3. Explain why:

```text
uint8_t → uint16_t
```

is generally safer than:

```text
uint16_t → uint8_t
```

---

## Advanced

Design a medical image pipeline supporting:

```text
Input:
16-bit CT data

Processing:
float

Output:
8-bit display
```

Explain:

```text
Data Conversion
Precision
Memory
Clamping
Windowing
```

---

# Key Takeaways

* Every pixel is represented using a data type.
* Bit depth determines how many values a pixel can represent.
* `uint8_t` represents 0–255.
* `uint16_t` represents 0–65535.
* `int16_t` supports negative values.
* `float` and `double` support fractional values.
* Medical image processing often requires data-type conversion.
* Stored pixel values may require metadata-based transformation.
* CT values are commonly interpreted in Hounsfield Units.
* Overflow and precision loss must be handled carefully.
* Data type selection affects both accuracy and memory consumption.

---

## Current Progress

* **Completed:** Chapter 6
* **Current:** **Level 0 → Module 1 → Chapter 7: Data Types and Pixel Representation**
* **Next:** **Level 0 → Module 1 → Chapter 8: Endianness and Binary Data**

Say **Next** to continue to Chapter 8.
