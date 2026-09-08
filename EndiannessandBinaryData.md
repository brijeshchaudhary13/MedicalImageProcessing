# Level 0 → Module 1 → Chapter 8: Endianness and Binary Data

## 1. Chapter Overview

Computers store image data as **binary bytes**.

This is especially important in medical imaging because formats such as DICOM may contain:

* 8-bit pixel data
* 16-bit pixel data
* Signed pixel values
* Floating-point values
* Metadata stored as binary data

To correctly interpret binary image data, we must understand:

* Bits
* Bytes
* Binary representation
* Endianness
* Little-endian
* Big-endian
* Byte swapping

---

# 2. What Is Binary Data?

Computers store information using:

```text
0 and 1
```

These are called **bits**.

Example:

```text
10101100
```

This is an 8-bit binary value.

---

# 3. Bit and Byte

### Bit

A single binary digit:

```text
0
or
1
```

### Byte

Usually:

```text
8 bits
```

Example:

```text
10101100
```

One byte can represent:

$$
2^8 = 256
$$

possible values.

For unsigned 8-bit data:

$$
0 \rightarrow 255
$$

---

# 4. Binary Representation of Numbers

Consider decimal:

```text
10
```

Binary:

```text
00001010
```

Because:

$$
10 = 8 + 2
$$

Binary positions:

```text
128 64 32 16 8 4 2 1
 0  0  0  0 1 0 1 0
```

Therefore:

```text
00001010 = 10
```

---

# 5. Why Binary Data Matters for Images

Suppose an 8-bit image contains:

```text
Pixel values:

10  20  30  40
```

Memory stores them as bytes:

```text
00001010
00010100
00011110
00101000
```

For an 8-bit image:

```text
1 Pixel = 1 Byte
```

---

# 6. 16-bit Pixel Representation

For a 16-bit pixel:

```text
1 Pixel = 16 bits
        = 2 bytes
```

Example value:

```text
0x1234
```

The value consists of two bytes:

```text
0x12
0x34
```

The question is:

> In what order are these bytes stored in memory?

This is where **endianness** becomes important.

---

# 7. What Is Endianness?

**Endianness** defines the order in which multi-byte values are stored in memory.

For:

```text
0x1234
```

There are two common formats:

```text
Little Endian
Big Endian
```

---

# 8. Little-Endian

In little-endian format, the **least significant byte** is stored first.

Value:

```text
0x1234
```

Memory:

```text
Address

1000 → 0x34
1001 → 0x12
```

Concept:

```text
Value: 0x1234

Memory:

34 12
```

---

# 9. Big-Endian

In big-endian format, the **most significant byte** is stored first.

Value:

```text
0x1234
```

Memory:

```text
Address

1000 → 0x12
1001 → 0x34
```

Concept:

```text
Value: 0x1234

Memory:

12 34
```

---

# 10. Little-Endian vs Big-Endian

| Value        | Little-Endian Memory | Big-Endian Memory |
| ------------ | -------------------- | ----------------- |
| `0x1234`     | `34 12`              | `12 34`           |
| `0xABCD`     | `CD AB`              | `AB CD`           |
| `0x12345678` | `78 56 34 12`        | `12 34 56 78`     |

Important:

> The numerical value does not change. Only the byte storage order changes.

---

# 11. Why Endianness Matters in Medical Imaging

Suppose a 16-bit pixel value is:

```text
0x1234
```

Correct interpretation:

```text
4660 decimal
```

But if bytes are interpreted in the wrong order:

```text
0x3412
```

Decimal:

```text
13330
```

Therefore:

```text
Wrong Byte Order
       ↓
Wrong Pixel Value
       ↓
Incorrect Image
```

Possible result:

```text
Incorrect brightness
Incorrect contrast
Corrupted image appearance
Wrong measurements
```

---

# 12. Example: Byte-Level View

Suppose memory contains:

```text
34 12
```

If interpreted as little-endian:

```text
0x1234
```

If interpreted as big-endian:

```text
0x3412
```

Same bytes:

```text
34 12
```

Different interpretation:

```text
Endianness
    ↓
Different Number
```

---

# 13. Checking Endianness in C++

A modern approach using C++20:

```cpp
#include <bit>
#include <iostream>

int main()
{
    if constexpr (
        std::endian::native ==
        std::endian::little)
    {
        std::cout << "Little Endian";
    }
    else if constexpr (
        std::endian::native ==
        std::endian::big)
    {
        std::cout << "Big Endian";
    }
}
```

---

# 14. Manual Endianness Check

A classic method:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::uint16_t value = 0x0001;

    std::uint8_t* pointer =
        reinterpret_cast<std::uint8_t*>(
            &value
        );

    if (*pointer == 1)
    {
        std::cout << "Little Endian";
    }
    else
    {
        std::cout << "Big Endian";
    }

    return 0;
}
```

Concept:

```text
Value = 0x0001

Little Endian Memory:

01 00
↑
First byte = 1
```

---

# 15. Byte Swapping

Sometimes data must be converted between byte orders.

For:

```text
0x1234
```

Byte swap:

```text
0x3412
```

Concept:

```text
Original:

12 34

Swap:

34 12
```

---

# 16. Manual 16-bit Byte Swap

```cpp
#include <cstdint>

std::uint16_t swapBytes(
    std::uint16_t value)
{
    return
        (value >> 8) |
        (value << 8);
}
```

Example:

```text
0x1234
```

Result:

```text
0x3412
```

---

# 17. Understanding the Byte Swap

Original:

```text
0x1234

High Byte = 0x12
Low Byte  = 0x34
```

After:

```text
0x3412
```

The bytes exchange positions.

Bit operations:

```text
value >> 8
```

moves the high byte down.

```text
value << 8
```

moves the low byte upward.

Then:

```text
|
```

combines them.

---

# 18. 32-bit Byte Swapping

Example:

```text
0x12345678
```

Swap:

```text
0x78563412
```

Implementation:

```cpp
#include <cstdint>

std::uint32_t swapBytes32(
    std::uint32_t value)
{
    return
        ((value & 0x000000FF) << 24) |
        ((value & 0x0000FF00) << 8)  |
        ((value & 0x00FF0000) >> 8)  |
        ((value & 0xFF000000) >> 24);
}
```

---

# 19. Binary File Data

An image file may contain:

```text
┌─────────────────────────┐
│ Header                  │
├─────────────────────────┤
│ Metadata                │
├─────────────────────────┤
│ Pixel Data              │
└─────────────────────────┘
```

For medical imaging:

```text
DICOM File
    │
    ├── Metadata
    │
    └── Pixel Data
```

The program must correctly interpret:

* Data type
* Number of bytes
* Signed/unsigned representation
* Endianness

---

# 20. Reading Binary Data in C++

Example:

```cpp
#include <fstream>
#include <cstdint>

std::uint16_t value;

std::ifstream file(
    "image.bin",
    std::ios::binary
);

file.read(
    reinterpret_cast<char*>(&value),
    sizeof(value)
);
```

Important:

The bytes read from the file may require endianness conversion before the value is correct on the current system.

---

# 21. `reinterpret_cast` and Binary Data

This:

```cpp
reinterpret_cast<char*>(&value)
```

allows the memory of:

```cpp
std::uint16_t
```

to be accessed as raw bytes.

Concept:

```text
uint16_t value

┌─────────────┐
│ Byte 0      │
│ Byte 1      │
└─────────────┘

reinterpret_cast
      ↓

Raw byte access
```

Low-level binary processing must be handled carefully.

---

# 22. DICOM and Endianness

DICOM data uses defined transfer syntaxes that determine how dataset values are encoded, including byte ordering for applicable uncompressed multi-byte values.

Conceptually:

```text
DICOM File
      ↓
Transfer Syntax
      ↓
Encoding Rules
      ↓
Correct Interpretation
      ↓
Pixel Data
```

Therefore, a DICOM parser should not simply assume one byte order without considering the dataset's encoding rules.

Libraries such as DCMTK handle much of this complexity.

---

# 23. Binary Data and Pixel Buffers

Suppose a 16-bit image has pixels:

```text
100
200
300
400
```

Memory conceptually contains:

```text
Pixel 0 → 2 bytes
Pixel 1 → 2 bytes
Pixel 2 → 2 bytes
Pixel 3 → 2 bytes
```

Total:

```text
4 × 2 = 8 bytes
```

A binary parser must know:

```text
Where Pixel Data Starts
        +
Pixel Data Type
        +
Byte Order
        +
Number of Pixels
```

---

# 24. Reading Raw Pixel Data

Suppose:

```text
Width = 512
Height = 512

Pixel Type = uint16_t
```

Total pixels:

$$
512 \times 512
=
262,144
$$

Total bytes:

$$
262,144 \times 2
=
524,288
$$

C++:

```cpp
std::vector<std::uint16_t> pixels(
    width * height
);

file.read(
    reinterpret_cast<char*>(
        pixels.data()
    ),
    pixels.size() *
    sizeof(std::uint16_t)
);
```

This assumes the binary layout is compatible with the expected representation. Real file parsing should validate the file format and encoding rules.

---

# 25. Binary Data Structure

Consider:

```cpp
struct Pixel
{
    std::uint16_t intensity;
};
```

Memory:

```text
intensity

Byte 0
Byte 1
```

For more complex structures:

```cpp
struct ImageHeader
{
    std::uint32_t width;
    std::uint32_t height;
    std::uint16_t bitDepth;
};
```

However, directly reading arbitrary file bytes into a C++ struct can be unsafe or incorrect because of:

* Padding
* Alignment
* Endianness
* Format version differences

A robust parser reads fields according to the file specification.

---

# 26. Struct Padding

Example:

```cpp
struct Example
{
    std::uint8_t value;
    std::uint32_t number;
};
```

You might expect:

```text
1 + 4 = 5 bytes
```

But the compiler may add padding.

Possible layout:

```text
Byte 0 → value

Byte 1–3 → padding

Byte 4–7 → number
```

Possible size:

```text
8 bytes
```

This is why binary file parsing must consider structure layout carefully.

---

# 27. Alignment

CPUs often access certain data types more efficiently when memory is properly aligned.

Example:

```text
uint32_t

Aligned Address:

0x1000
```

Misaligned data may be slower or problematic on some architectures.

Concept:

```text
Memory
│
├── Alignment
├── Padding
└── Data
```

Image libraries often manage alignment internally.

---

# 28. Important Binary Image Concepts

When reading raw image data, determine:

```text
Width
Height
Depth

Pixel Type

Signed / Unsigned

Bit Depth

Byte Order

Number of Channels

Stride

File Offset
```

Without this information, raw bytes cannot be reliably interpreted as an image.

---

# 29. Example: Wrong Endianness

Actual pixel bytes:

```text
01 02
```

Correct big-endian interpretation:

```text
0x0102
```

Decimal:

$$
258
$$

Incorrect little-endian interpretation:

```text
0x0201
```

Decimal:

$$
513
$$

Result:

```text
258
≠
513
```

A wrong byte order changes the numerical pixel value.

---

# 30. Binary vs Text Files

## Text

Example:

```text
123
456
789
```

Characters are stored as encoded text.

## Binary

Example:

```text
7B 00 C8 01
```

These are raw bytes.

Advantages of binary image storage:

* Compact
* Fast to read
* Direct memory representation

Challenges:

* Not human-readable
* Requires format knowledge
* Endianness matters

---

# 31. Debugging Binary Image Problems

When an image looks incorrect, check:

### 1. Pixel Type

```text
uint8_t?
uint16_t?
int16_t?
float?
```

### 2. Endianness

```text
Little?
Big?
```

### 3. Signed Representation

```text
Signed?
Unsigned?
```

### 4. Dimensions

```text
Width × Height × Depth
```

### 5. Expected File Size

$$
Width
\times
Height
\times
Depth
\times
BytesPerPixel
$$

### 6. Offset

Are you reading from the correct location?

### 7. Stride

Are rows tightly packed?

---

# 32. Medical Imaging Example

Imagine a CT slice:

```text
512 × 512

16-bit pixels
```

Pixel memory:

$$
512
\times
512
\times
2
$$

$$
=
524,288\ bytes
$$

Processing:

```text
Binary File
      ↓
Read Metadata
      ↓
Determine Encoding
      ↓
Read Pixel Data
      ↓
Interpret Pixel Type
      ↓
Apply Required Conversion
      ↓
Image Buffer
      ↓
Image Processing
```

---

# 33. Interview Questions

### Basic

1. What is a bit?
2. What is a byte?
3. What is endianness?
4. What is little-endian?
5. What is big-endian?

### Intermediate

1. Why does endianness matter for 16-bit images?
2. What is byte swapping?
3. How do you calculate binary image memory size?
4. Why should binary files be opened with `std::ios::binary`?
5. What problems can struct padding cause?

### Advanced

1. How would you design a portable binary image reader?
2. How would you safely parse multi-byte fields?
3. Why is directly reading arbitrary file data into a C++ struct risky?
4. How would you detect and handle byte order differences?
5. What information is required to correctly interpret raw pixel data?

---

# 34. Exercises

## Beginner

1. Convert decimal `10` to binary.
2. How many bits are in 4 bytes?
3. Show the little-endian memory representation of:

```text
0x1234
```

4. Show the big-endian representation of:

```text
0x1234
```

---

## Intermediate

Write a function that swaps bytes of:

```cpp
std::uint16_t
```

Then test:

```text
0x1234
```

Expected:

```text
0x3412
```

---

## Advanced

Design a simple raw image format containing:

```text
Width
Height
Pixel Type
Bit Depth
Byte Order
Pixel Data
```

Write pseudocode for:

```text
Open File
   ↓
Read Header
   ↓
Validate Dimensions
   ↓
Determine Data Type
   ↓
Check Endianness
   ↓
Read Pixel Buffer
   ↓
Swap Bytes if Required
   ↓
Create Image
```

---

# Key Takeaways

* Computers store image data as binary bytes.
* **1 byte = 8 bits**.
* Multi-byte pixel values require a defined byte order.
* **Little-endian** stores the least significant byte first.
* **Big-endian** stores the most significant byte first.
* Wrong endianness can produce incorrect pixel values.
* Binary file parsing must consider data type, signedness, byte order, dimensions, stride, and offset.
* Directly reading arbitrary binary files into C++ structs can fail because of padding, alignment, and encoding differences.
* Medical image parsers must follow the file format's encoding rules.
* Endianness is an important foundation for understanding DICOM and other binary image formats.

---

## Current Progress

* **Completed:** Chapter 7
* **Current:** **Level 0 → Module 1 → Chapter 8: Endianness and Binary Data**
* **Next:** **Level 0 → Module 1 → Chapter 9: Memory Management for Large Images**

Say **Next** to continue.
