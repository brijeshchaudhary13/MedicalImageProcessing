# Chapter 4 — Sampling and Quantization

We now move to **Chapter 4 exactly according to your index**. The required topics are:

1. Sampling
2. Spatial sampling
3. Temporal sampling
4. Quantization
5. Aliasing
6. Nyquist theorem
7. Undersampling
8. Oversampling
9. Bit-depth effects

We will stay strictly within these topics. 

---

# 4.1 Why Do We Need Sampling and Quantization?

The real world is generally **continuous**.

A computer works with **discrete numbers**.

So we need a conversion:

```text
REAL WORLD
   ↓
Continuous signal
   ↓
Sampling
   ↓
Discrete positions
   ↓
Quantization
   ↓
Discrete values
   ↓
DIGITAL IMAGE
```

This is one of the most fundamental concepts in digital imaging.

---

# 4.2 Continuous Image

Imagine a real-world scene.

Light intensity may vary continuously across space:

```text
Position →
──────────────────────────────

Intensity
        ╭──────────────╮
       /                \
______/                  \______
```

Mathematically, we could describe this as:

[
I(x,y)
]

where (x) and (y) are continuous spatial coordinates.

But a computer cannot store infinitely many positions.

We therefore have to **sample** the image.

---

# 4.3 What Is Sampling?

### Definition

> **Sampling is the process of measuring a continuous signal at discrete locations or times.**

Imagine measuring temperature along a line.

The temperature is continuous:

```text
────────────────────────────
```

But we take measurements at:

```text
•    •    •    •    •    •
```

These measurement points are **samples**.

For an image:

```text
Continuous image
       ↓
• • • • • • •
• • • • • • •
• • • • • • •
• • • • • • •
```

Each sampled location eventually becomes associated with a pixel.

---

# 4.4 Sampling an Image

Suppose the continuous image is:

[
I(x,y)
]

After sampling:

[
I[m,n]
]

where:

* (m) = discrete horizontal index
* (n) = discrete vertical index

Conceptually:

```text
Continuous coordinates

(x,y)
  ↓
Sampling
  ↓
(m,n)
```

Therefore:

```text
Continuous image
      ↓
Spatial sampling
      ↓
Discrete image grid
```

---

# 4.5 Spatial Sampling

**Spatial sampling** determines how frequently we sample an image in space.

Imagine two sampling grids.

### Low sampling density

```text
•          •          •

     •          •

•          •          •
```

### High sampling density

```text
• • • • • • •
• • • • • • •
• • • • • • •
• • • • • • •
```

The second captures more spatial detail.

---

# 4.6 Sampling Frequency

Sampling frequency describes how frequently samples are taken.

For a one-dimensional signal:

[
f_s
]

represents the sampling frequency.

Its unit is typically:

[
samples/second
]

For spatial sampling, we can use concepts such as:

[
samples/mm
]

or equivalently spatial frequency.

---

# 4.7 Pixel Size and Spatial Sampling

Suppose a detector samples an image every:

[
1\text{ mm}
]

Then:

```text
1 sample
   ↓
1 mm
```

If the sampling interval is:

[
0.5\text{ mm}
]

then samples are twice as dense:

```text
0.5 mm   0.5 mm   0.5 mm
  •        •        •
```

So:

[
\text{smaller sampling interval}
\Rightarrow
\text{higher spatial sampling}
]

---

# 4.8 Sampling Interval

Let:

[
\Delta x
]

be the distance between samples.

Then the sampling frequency is conceptually:

[
f_s=\frac{1}{\Delta x}
]

For example:

[
\Delta x=1\text{ mm}
]

gives:

[
f_s=1\text{ sample/mm}
]

If:

[
\Delta x=0.5\text{ mm}
]

then:

[
f_s=2\text{ samples/mm}
]

Therefore:

```text
smaller Δx
   ↓
more samples
   ↓
higher sampling frequency
```

---

# 4.9 Why Spatial Sampling Matters in Medical Imaging

Consider two tiny structures.

```text
Structure A    Structure B

   ██             ██
   ██             ██
```

If sampling is sufficiently fine:

```text
• • • • • • • • •
• • ██ • • ██ • •
• • ██ • • ██ • •
```

the structures can potentially be distinguished.

If sampling is too coarse:

```text
•       •       •

       ███

•       •       •
```

the structures may not be represented correctly.

This is why detector sampling and reconstructed voxel size matter in:

* CT
* MRI
* X-ray
* ultrasound
* mammography

---

# 4.10 Quantization

Sampling answers:

> **Where do we measure?**

Quantization answers:

> **What numerical value do we assign to the measurement?**

Suppose the real intensity is:

[
127.83
]

but our system only allows integer values.

We may quantize it to:

[
128
]

So:

```text
Continuous value
       ↓
Quantization
       ↓
Discrete value
```

---

# 4.11 Sampling vs Quantization

This distinction is extremely important.

### Sampling

Deals with:

> **Spatial/time positions**

```text
WHERE?
```

### Quantization

Deals with:

> **Value levels**

```text
WHAT VALUE?
```

Together:

```text
Continuous image
       │
       ├── Sampling
       │      ↓
       │   discrete positions
       │
       └── Quantization
              ↓
          discrete values
               │
               ↓
          Digital image
```

---

# 4.12 Example

Suppose a continuous signal contains:

```text
100.2
101.7
105.4
109.8
```

Sampling determines **which points** we measure.

Quantization might turn them into:

```text
100
102
105
110
```

if the quantization step is 1.

If the quantization step is 10, they might become approximately:

```text
100
100
110
110
```

Therefore quantization affects intensity precision.

---

# 4.13 Quantization Levels

If we use (n) bits:

[
L=2^n
]

possible levels.

For example:

### 8-bit

[
2^8=256
]

levels.

### 12-bit

[
2^{12}=4096
]

levels.

### 16-bit

[
2^{16}=65,536
]

levels.

So:

```text
More bits
   ↓
More intensity levels
   ↓
Finer intensity representation
```

---

# 4.14 Quantization Error

Suppose the actual intensity is:

[
100.7
]

and we quantize to:

[
101
]

Then the quantization error is:

[
101-100.7=0.3
]

The error is:

[
e=x_q-x
]

where:

* (x) = original value
* (x_q) = quantized value

Quantization therefore introduces an approximation.

---

# 4.15 Why Quantization Matters in Medical Imaging

Suppose two tissues have very similar intensity values.

If the bit depth is too low:

```text
Tissue A → 100
Tissue B → 101
```

they may be difficult to distinguish.

With more intensity levels:

```text
Tissue A → 100.2
Tissue B → 100.8
```

the underlying difference can potentially be preserved more accurately.

This is one reason medical imaging systems may use higher bit depths.

---

# 4.16 Aliasing

Now we reach one of the most important concepts in sampling.

> **Aliasing occurs when the sampling rate is insufficient to represent the signal correctly, causing different signals or patterns to become indistinguishable after sampling.**

In simple language:

> **The computer samples too slowly and gets the wrong picture of what is actually there.**

---

# 4.17 Simple Aliasing Example

Imagine a rapidly alternating pattern:

```text
████████████████
```

If the sampling grid is dense:

```text
• • • • • • • • • • • •
```

the pattern can be captured.

But if sampling is too sparse:

```text
•       •       •       •
```

the samples may miss important changes.

The resulting digital image can look completely different from the original.

---

# 4.18 The Classic Wagon-Wheel Effect

A famous real-world example is a rotating wheel in a movie.

Suppose:

```text
Wheel rotates →
```

but the camera captures frames at discrete times.

If the frame rate is insufficient, the wheel may appear:

```text
rotating slowly
```

or even:

```text
rotating backward
```

This is temporal aliasing.

The same basic principle applies to spatial images.

---

# 4.19 Spatial Aliasing

Suppose we have a pattern:

```text
| | | | | | | | |
```

with very fine details.

If the sampling grid is too coarse:

```text
•     •     •     •
```

we may produce a false pattern.

This can appear as:

* moiré patterns
* jagged edges
* false structures
* incorrect textures

---

# 4.20 Medical Imaging Aliasing

Aliasing can occur in medical imaging when acquisition or reconstruction does not adequately capture spatial information.

Possible consequences include:

* false patterns
* loss of detail
* distorted structures
* reconstruction artifacts

The exact mechanism differs between modalities, but the fundamental concept remains:

```text
Insufficient sampling
        ↓
Information loss
        ↓
Aliasing
```

---

# 4.21 Nyquist Theorem

The Nyquist sampling theorem gives us a fundamental condition for sampling.

Suppose the highest frequency present in a signal is:

[
f_{max}
]

Then the sampling frequency should satisfy:

[
f_s \geq 2f_{max}
]

The value:

[
2f_{max}
]

is called the **Nyquist rate**.

---

# 4.22 Example of Nyquist

Suppose a signal contains frequencies up to:

[
100\text{ Hz}
]

Then:

[
f_{max}=100\text{ Hz}
]

The minimum theoretical sampling rate is:

[
f_s=2(100)
]

[
=200\text{ Hz}
]

So:

```text
Maximum signal frequency = 100 Hz

Nyquist rate = 200 samples/sec
```

Sampling below this limit can lead to aliasing.

---

# 4.23 Why "At Least Twice" Is Important

Suppose:

[
f_{max}=100
]

and:

[
f_s=150
]

Then:

[
150 < 200
]

So the Nyquist condition is violated.

Aliasing can occur.

If:

[
f_s=250
]

then:

[
250>200
]

and the basic Nyquist requirement is satisfied.

---

# 4.24 Nyquist Frequency

There is an important terminology distinction.

### Nyquist rate

[
2f_{max}
]

### Nyquist frequency

Usually:

[
f_N=\frac{f_s}{2}
]

So if:

[
f_s=1000\text{ Hz}
]

then:

[
f_N=500\text{ Hz}
]

The Nyquist frequency is the highest frequency that can theoretically be represented without aliasing for that sampling rate, under the theorem's assumptions.

---

# 4.25 Important Practical Detail

The simple equation:

[
f_s\ge2f_{max}
]

is a theoretical condition.

Real imaging systems often require additional considerations:

* noise
* finite detector response
* anti-aliasing filters
* reconstruction algorithms
* bandwidth
* hardware limitations

So don't interpret Nyquist as:

> "Just sample exactly twice and everything will always be perfect."

In engineering, a safety margin is often useful.

---

# 4.26 Undersampling

**Undersampling** means sampling at a rate that is too low for the information contained in the signal.

Conceptually:

```text
Signal detail
     ↓
Too few samples
     ↓
Information lost
     ↓
Aliasing / distortion
```

Example:

[
f_{max}=100\text{ Hz}
]

but:

[
f_s=120\text{ Hz}
]

Since:

[
120<200
]

we are undersampling.

---

# 4.27 Effects of Undersampling

Possible effects:

* loss of detail
* false patterns
* aliasing
* jagged edges
* inaccurate structures
* inability to distinguish fine objects

In medical imaging, this can be especially problematic because small anatomical structures may matter.

---

# 4.28 Oversampling

**Oversampling** means sampling more densely than the minimum required.

Suppose:

[
f_{max}=100\text{ Hz}
]

Nyquist rate:

[
200\text{ Hz}
]

If we sample at:

[
500\text{ Hz}
]

we are oversampling.

```text
Required minimum
      ↓
200

Actual
      ↓
500
```

---

# 4.29 Why Oversampling Can Help

Oversampling can provide:

* better representation
* reduced risk of aliasing
* easier filtering
* greater tolerance to system imperfections

But it has costs:

* more data
* more storage
* more memory
* more computation
* potentially more acquisition time

Therefore:

> **More samples are not automatically better in every practical situation.**

---

# 4.30 Undersampling vs Oversampling

| Property      | Undersampling | Oversampling         |
| ------------- | ------------- | -------------------- |
| Sampling rate | Too low       | Higher than required |
| Detail        | May be lost   | Better represented   |
| Aliasing risk | High          | Lower                |
| Data size     | Smaller       | Larger               |
| Computation   | Lower         | Higher               |
| Storage       | Lower         | Higher               |

---

# 4.31 Bit Depth and Quantization

Sampling determines spatial resolution.

Bit depth determines intensity resolution.

Think:

```text
              DIGITAL IMAGE
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      Sampling            Quantization
          │                   │
          ↓                   ↓
   Spatial detail       Intensity detail
          │                   │
          ↓                   ↓
    Pixel spacing          Bit depth
```

This is an extremely important mental model.

---

# 4.32 Spatial Resolution vs Intensity Resolution

### Spatial resolution

How finely can we represent **where something is**?

Controlled strongly by:

* sampling density
* pixel size
* voxel size
* imaging system characteristics

### Intensity resolution

How finely can we represent **how strong a signal/value is**?

Controlled strongly by:

* quantization
* bit depth
* detector/system characteristics

Therefore:

```text
Spatial resolution
→ WHERE?

Intensity resolution
→ HOW MUCH?
```

---

# 4.33 Example

Imagine a CT volume:

```text
512 × 512
```

with:

```text
0.5 mm spacing
```

and:

```text
16-bit values
```

Then:

```text
0.5 mm
   ↓
spatial sampling

16-bit
   ↓
intensity representation
```

These are two different properties.

---

# 4.34 Bit Depth Effects

Suppose an intensity range is:

[
0\ldots1
]

### 2-bit

[
2^2=4
]

levels:

```text
0
0.33
0.67
1
```

The representation is very coarse.

### 8-bit

[
256
]

levels.

Much finer.

### 16-bit

[
65,536
]

levels.

Much finer still.

Conceptually:

```text
Low bit depth

|    |    |    |


High bit depth

||||||||||||||||||||||||||||||||||||||
```

---

# 4.35 Quantization Banding

If an image contains smooth intensity transitions but the bit depth is too low, we may see visible steps.

For example:

```text
Ideal gradient:

████████████████████████


Low-bit representation:

███  ███  ███  ███  ███
```

This is called **banding** or quantization artifacts.

---

# 4.36 Medical Example of Bit Depth

Suppose an imaging system measures a continuous range of values.

With low bit depth:

```text
Actual:
100.1
100.2
100.3
100.4
100.5

Stored approximately:
100
100
100
100
101
```

Information has been quantized.

With higher bit depth, more levels are available, allowing finer representation.

---

# 4.37 Sampling and Quantization Pipeline

The complete process is:

```text
          REAL WORLD
              │
              ▼
      Continuous Signal
              │
              ▼
        SPATIAL SAMPLING
              │
              ▼
       Discrete Positions
              │
              ▼
        QUANTIZATION
              │
              ▼
        Discrete Values
              │
              ▼
       DIGITAL IMAGE
```

Remember this diagram.

It explains why a digital image is discrete in both:

* spatial coordinates
* intensity values

---

# 4.38 A Simple Numerical Example

Suppose a continuous 1D signal is:

[
f(x)=\sin(2\pi x)
]

We sample it at:

[
x=0,\ 0.25,\ 0.5,\ 0.75,\ 1
]

Then:

[
f(0)=0
]

[
f(0.25)=1
]

[
f(0.5)=0
]

[
f(0.75)=-1
]

[
f(1)=0
]

So our samples are:

```text id="d5xx1n"
x       f(x)

0       0
0.25    1
0.5     0
0.75   -1
1       0
```

This is a discrete representation of the continuous signal.

---

# 4.39 From 1D Signal to 2D Image

The same idea applies to images.

1D:

[
f(x)
]

2D:

[
I(x,y)
]

Sampling produces:

[
I[m,n]
]

Therefore:

```text
Continuous signal
       ↓
Sampling
       ↓
Discrete signal
```

and:

```text
Continuous image
       ↓
2D sampling
       ↓
Digital image
```

---

# 4.40 C++ Sampling Example

We can simulate sampling a function:

```cpp id="zv0y7g"
#include <iostream>
#include <cmath>

int main()
{
    const double pi = 3.141592653589793;

    for (int i = 0; i <= 10; ++i)
    {
        double x = i * 0.1;
        double y = std::sin(2.0 * pi * x);

        std::cout
            << "x = " << x
            << ", y = " << y
            << '\n';
    }

    return 0;
}
```

Here we have a continuous mathematical function:

[
\sin(2\pi x)
]

but the computer evaluates it only at discrete positions.

That's sampling.

---

# 4.41 Python Sampling Example

```python id="6j2xks"
import numpy as np

x = np.linspace(0, 1, 11)

y = np.sin(2 * np.pi * x)

for position, value in zip(x, y):
    print(position, value)
```

Again:

```text
Continuous mathematical concept
        ↓
Discrete sample positions
        ↓
Numerical values
```

---

# 4.42 Medical Imaging Connection

Now connect the entire chapter to CT.

A real anatomical structure has continuous physical properties.

```text
Patient anatomy
      ↓
Physical signal
      ↓
Detector/acquisition
      ↓
Sampling
      ↓
Discrete measurements
      ↓
Quantization
      ↓
Digital data
      ↓
Reconstruction
      ↓
Digital medical image
```

The final CT volume therefore depends on how the physical signal was sampled and represented.

---

# 4.43 Important Distinction: Acquisition vs Display

Do not confuse:

```text
Acquisition sampling
```

with:

```text
Display resolution
```

A CT scanner might acquire high-resolution data and then display it on a monitor at a different resolution.

Therefore:

```text
Acquired data
      ≠
Displayed image
```

The viewer may also resample the image during:

* zoom
* MPR
* 3D rendering
* resizing

We will study those transformations later.

---

# 4.44 Chapter 4 Core Mental Model

Remember:

```text
CONTINUOUS WORLD
      │
      ▼
   SAMPLING
      │
      ├── Spatial sampling
      │
      └── Temporal sampling
      │
      ▼
DISCRETE POSITIONS
      │
      ▼
 QUANTIZATION
      │
      ▼
DISCRETE VALUES
      │
      ▼
 DIGITAL IMAGE
```

And:

```text
Sampling
   ↓
Spatial information

Quantization
   ↓
Intensity information
```

Then:

```text
Too little sampling
      ↓
Undersampling
      ↓
Aliasing
```

while:

```text
More sampling than necessary
      ↓
Oversampling
```

And:

[
f_s\ge2f_{max}
]

is the fundamental Nyquist condition.

---

# Chapter 4 — Knowledge Check

Answer these before moving to Chapter 5.

### Fundamentals

1. What is sampling?
2. Why do we need sampling?
3. What is spatial sampling?
4. What is temporal sampling?
5. What is quantization?
6. What is the difference between sampling and quantization?

### Aliasing

7. What is aliasing?
8. Why does undersampling cause aliasing?
9. Give a real-world example of temporal aliasing.
10. What is spatial aliasing?

### Nyquist

11. State the Nyquist sampling theorem.
12. If the highest frequency is 100 Hz, what is the minimum theoretical sampling rate?
13. If the sampling frequency is 1000 Hz, what is the Nyquist frequency?
14. What happens if (f_s < 2f_{max})?

### Sampling

15. What happens when the sampling interval decreases?
16. What happens when spatial sampling density increases?
17. What is undersampling?
18. What is oversampling?
19. What are the disadvantages of oversampling?

### Quantization

20. What is quantization error?
21. How many levels are available with 8-bit data?
22. How many levels are available with 12-bit data?
23. How many levels are available with 16-bit data?
24. What is bit-depth-related banding?

### Medical imaging

25. Why is spatial sampling important in CT?
26. Why is intensity quantization important in medical imaging?
27. What is the difference between spatial resolution and intensity resolution?
28. Why can insufficient sampling cause problems in medical images?

---

## Numerical Exercise

### Exercise 1

A signal has:

[
f_{max}=200\text{ Hz}
]

Calculate the minimum theoretical sampling frequency.

### Exercise 2

A signal has:

[
f_{max}=500\text{ Hz}
]

and is sampled at:

[
f_s=800\text{ Hz}
]

Is it satisfying the basic Nyquist condition?

### Exercise 3

An image uses 10-bit quantization.

How many intensity levels are available?

### Exercise 4

An image uses 14-bit quantization.

How many intensity levels?

### Exercise 5

A CT detector samples every:

[
0.25\text{ mm}
]

What is the corresponding spatial sampling density in samples/mm?

### Exercise 6

Explain in your own words:

> **Why are sampling and quantization two different processes?**

---

**Chapter 4 is now complete.**

The next chapter in your exact index is:

# Chapter 5 — Image Coordinate Systems

Topics:

* Pixel coordinates
* Image coordinates
* Cartesian coordinates
* Physical coordinates
* World coordinates
* Origin
* Spacing
* Direction
* Orientation.
