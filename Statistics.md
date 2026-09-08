# Module 2 → Chapter 23: Statistics

Statistics is essential for:

* Digital image processing
* Medical image processing
* Computer vision
* Image enhancement
* Noise analysis
* Machine learning
* Medical image analysis

While **probability** deals with uncertainty theoretically, **statistics** helps us analyze actual data.

---

# 1. What Is Statistics?

Statistics is the science of:

```text
Collecting
    ↓
Organizing
    ↓
Analyzing
    ↓
Interpreting
    ↓
Data
```

In image processing, pixel values are data.

For example:

$$
I(x,y)
$$

represents the intensity value at pixel position:

$$
(x,y)
$$

---

# 2. Statistics in Image Processing

Consider pixel values:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

We can calculate:

* Mean
* Median
* Mode
* Variance
* Standard deviation
* Minimum
* Maximum
* Range

These measurements describe the image intensity distribution.

---

# 3. Population and Sample

## Population

The complete collection of data.

Example:

```text
All pixels in a complete CT volume
```

---

## Sample

A smaller subset selected from the population.

Example:

```text
Pixels from a selected Region of Interest (ROI)
```

---

# 4. Types of Data

Data can broadly be categorized as:

```text
Data
│
├── Qualitative
│
└── Quantitative
     │
     ├── Discrete
     │
     └── Continuous
```

### Qualitative Data

Categories such as:

* Tissue type
* Tumor class
* Image modality

### Quantitative Data

Numerical values such as:

* Pixel intensity
* Radiation dose
* Tumor volume

---

# 5. Measures of Central Tendency

The three major measures are:

```text
Central Tendency
│
├── Mean
├── Median
└── Mode
```

---

# 6. Mean

The arithmetic mean is:

$$
\boxed{
\mu=
\frac{1}{N}
\sum_{i=1}^{N}x_i
}
$$

Where:

* \(x_i\) = individual value
* \(N\) = total number of values

---

# 7. Mean Example

Given:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

Sum:

$$
10+20+30+40+50=150
$$

Number of values:

$$
N=5
$$

Mean:

$$
\mu=
\frac{150}{5}
$$

$$
\boxed{\mu=30}
$$

---

# 8. Image Mean Intensity

For an image containing \(N\) pixels:

$$
\boxed{
\mu=
\frac{1}{N}
\sum_{i=1}^{N}I_i
}
$$

Interpretation:

* Lower mean → darker average intensity
* Higher mean → brighter average intensity

This depends on the image representation and intensity scale.

---

# 9. Median

The median is the middle value after sorting the data.

Example:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

Median:

$$
\boxed{30}
$$

---

## Even Number of Values

Example:

$$
10,\ 20,\ 30,\ 40
$$

The middle values are:

$$
20,\ 30
$$

Therefore:

$$
\text{Median}
=
\frac{20+30}{2}
$$

$$
\boxed{25}
$$

---

# 10. Mode

The mode is the most frequently occurring value.

Example:

$$
10,\ 20,\ 20,\ 30,\ 40
$$

The value occurring most often is:

$$
\boxed{20}
$$

---

# 11. Mean vs Median vs Mode

| Measure | Meaning             | Sensitive to Outliers?  |
| ------- | ------------------- | ----------------------- |
| Mean    | Average             | Yes                     |
| Median  | Middle value        | Less sensitive          |
| Mode    | Most frequent value | Depends on distribution |

---

# 12. Example With an Outlier

Data:

$$
10,\ 20,\ 30,\ 40,\ 1000
$$

Mean:

$$
\frac{1100}{5}=220
$$

Median:

$$
30
$$

The mean is strongly affected by the extreme value.

The median is much more robust in this example.

---

# 13. Minimum and Maximum

For:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

Minimum:

$$
\boxed{10}
$$

Maximum:

$$
\boxed{50}
$$

---

# 14. Range

The range is:

$$
\boxed{
Range=Maximum-Minimum
}
$$

Example:

$$
50-10
=
\boxed{40}
$$

---

# 15. Variance

Variance measures how spread out values are around the mean.

For a population:

$$
\boxed{
\sigma^2=
\frac{1}{N}
\sum_{i=1}^{N}
(x_i-\mu)^2
}
$$

---

# 16. Variance Example

Data:

$$
2,\ 4,\ 6
$$

Mean:

$$
\mu=4
$$

Differences:

$$
2-4=-2
$$

$$
4-4=0
$$

$$
6-4=2
$$

Squares:

$$
4,\ 0,\ 4
$$

Variance:

$$
\sigma^2=
\frac{4+0+4}{3}
$$

$$
\boxed{
\sigma^2=\frac{8}{3}
}
$$

---

# 17. Sample Variance

For a sample:

$$
\boxed{
s^2=
\frac{1}{N-1}
\sum_{i=1}^{N}
(x_i-\bar{x})^2
}
$$

Notice:

$$
N-1
$$

instead of:

$$
N
$$

This is commonly called **Bessel's correction**.

---

# 18. Standard Deviation

Standard deviation is the square root of variance:

$$
\boxed{
\sigma=
\sqrt{\sigma^2}
}
$$

It describes spread in the same units as the original data.

---

# 19. Standard Deviation Example

If:

$$
\sigma^2=25
$$

Then:

$$
\sigma=\sqrt{25}
$$

$$
\boxed{5}
$$

---

# 20. Image Variance

For image intensities:

$$
\boxed{
\sigma^2=
\frac{1}{N}
\sum_{i=1}^{N}
(I_i-\mu)^2
}
$$

Interpretation:

* Low variance → intensities are relatively similar.
* High variance → intensities are more spread out.

---

# 21. Histogram

A histogram represents the frequency distribution of values.

Example:

```text
Intensity     Frequency

0             ███
1             ██████
2             █████████
3             ████
4             ██
```

For images:

```text
X-axis → Intensity
Y-axis → Number of Pixels
```

---

# 22. Histogram Interpretation

### Dark Image

More pixels at lower intensities.

```text
Frequency
   ███████
   █████
   ███
   ██
──────────────
Low       High
Intensity
```

### Bright Image

More pixels at higher intensities.

```text
Frequency
              ███████
              █████
              ███
              ██
──────────────
Low       High
Intensity
```

---

# 23. Normalized Histogram

A normalized histogram is:

$$
\boxed{
p(i)=\frac{n_i}{N}
}
$$

Where:

* \(n_i\) = number of pixels having intensity \(i\)
* \(N\) = total number of pixels

Then:

$$
\sum_i p(i)=1
$$

This can be interpreted as an empirical probability distribution.

---

# 24. Skewness

Skewness measures asymmetry in a distribution.

```text
Distribution
│
├── Symmetric
├── Positive Skew
└── Negative Skew
```

---

## Positive Skew

```text
██████
████
██
█
──────────────→
        Long Tail
```

The distribution has a longer tail toward higher values.

---

## Negative Skew

```text
        ██████
          ████
            ██
             █
← Long Tail
──────────────
```

The distribution has a longer tail toward lower values.

---

# 25. Kurtosis

Kurtosis describes characteristics of a distribution's tails and concentration relative to a reference distribution.

It can help describe whether distributions have relatively heavier or lighter tails, depending on the kurtosis definition used.

---

# 26. Covariance

Covariance measures how two variables vary together.

For variables \(X\) and \(Y\):

$$
\boxed{
Cov(X,Y)
=
E[(X-\mu_X)(Y-\mu_Y)]
}
$$

Interpretation:

* Positive covariance → variables tend to increase together.
* Negative covariance → one tends to decrease when the other increases.
* Near-zero covariance → little linear co-variation, though this does not necessarily imply independence.

---

# 27. Correlation

Correlation is a normalized measure of linear association.

$$
\boxed{
\rho_{XY}
=
\frac{
Cov(X,Y)
}{
\sigma_X\sigma_Y
}
}
$$

Typically:

$$
-1\leq\rho\leq1
$$

---

## Interpretation

```text
+1 → Strong positive linear relationship
 0 → No linear correlation
-1 → Strong negative linear relationship
```

---

# 28. Statistics in Medical Image Processing

Statistics is used for:

### Image Quality Analysis

Measuring:

* Mean intensity
* Noise variation
* Contrast-related metrics
* Signal statistics

---

### Region of Interest Analysis

Example:

```text
Tumor ROI
    ↓
Extract Pixel Values
    ↓
Mean
Variance
Histogram
Percentiles
Texture Features
```

---

### Image Segmentation

Statistics can help distinguish regions.

For example:

```text
Region A
Mean = 50

Region B
Mean = 150
```

Intensity statistics can help separate classes when distributions differ.

---

# 29. Contrast and Statistics

A simple statistical measure of local or global intensity variation can help characterize contrast.

For example:

```text
Low variation
     ↓
Potentially lower contrast

High variation
     ↓
Potentially greater intensity differences
```

However, variance alone is not a complete measure of perceived image contrast.

---

# 30. Noise Analysis

Suppose:

$$
I_{observed}
=
I_{true}+N
$$

Statistics can describe noise using:

$$
\mu_N
$$

and:

$$
\sigma_N
$$

Where:

* Mean → possible bias or average offset.
* Standard deviation → amount of variation.

---

# 31. Signal-to-Noise Ratio (SNR)

A simplified form is:

$$
\boxed{
SNR=
\frac{\text{Signal}}
{\text{Noise}}
}
$$

In logarithmic form, depending on the application:

$$
SNR_{dB}
=
20\log_{10}
\left(
\frac{A_{signal}}
{A_{noise}}
\right)
$$

Higher SNR generally indicates a stronger signal relative to noise.

---

# 32. Contrast-to-Noise Ratio (CNR)

CNR measures contrast relative to noise.

A common simplified form is:

$$
\boxed{
CNR=
\frac{
|\mu_1-\mu_2|
}{
\sigma_{noise}
}
}
$$

Where:

* \(\mu_1\) and \(\mu_2\) are mean intensities of two regions.
* \(\sigma_{noise}\) represents a noise estimate.

Exact definitions can vary by imaging modality and application.

---

# 33. Mean Squared Error (MSE)

MSE measures average squared error between two images.

$$
\boxed{
MSE=
\frac{1}{N}
\sum_{i=1}^{N}
(I_i-J_i)^2
}
$$

Where:

* \(I\) = reference image
* \(J\) = comparison image

---

# 34. Root Mean Squared Error (RMSE)

$$
\boxed{
RMSE=
\sqrt{MSE}
}
$$

RMSE is in the same units as the original intensity values.

---

# 35. Peak Signal-to-Noise Ratio (PSNR)

PSNR is often calculated from MSE:

$$
\boxed{
PSNR=
10\log_{10}
\left(
\frac{MAX^2}{MSE}
\right)
}
$$

Where \(MAX\) is the maximum representable pixel value or defined data range.

For an 8-bit image, a common value is:

$$
MAX=255
$$

Higher PSNR generally means lower squared error relative to the reference.

---

# 36. C++ Example: Calculate Mean

```cpp
#include <iostream>

double calculateMean(
    const int values[],
    int size)
{
    double sum = 0.0;

    for (int i = 0; i < size; ++i)
    {
        sum += values[i];
    }

    return sum / size;
}

int main()
{
    int values[] = {10, 20, 30, 40, 50};

    double mean =
        calculateMean(values, 5);

    std::cout << mean;

    return 0;
}
```

Output:

```text
30
```

---

# 37. C++ Example: Population Variance

```cpp
#include <iostream>

double calculateMean(
    const int values[],
    int size)
{
    double sum = 0.0;

    for (int i = 0; i < size; ++i)
    {
        sum += values[i];
    }

    return sum / size;
}

double calculateVariance(
    const int values[],
    int size)
{
    double mean =
        calculateMean(values, size);

    double sum = 0.0;

    for (int i = 0; i < size; ++i)
    {
        double difference =
            values[i] - mean;

        sum += difference * difference;
    }

    return sum / size;
}
```

---

# 38. Important Formulas

### Mean

$$
\mu=
\frac{1}{N}
\sum x_i
$$

### Median

Middle value after sorting.

### Mode

Most frequent value.

### Range

$$
Range=Maximum-Minimum
$$

### Population Variance

$$
\sigma^2=
\frac{1}{N}
\sum(x_i-\mu)^2
$$

### Sample Variance

$$
s^2=
\frac{1}{N-1}
\sum(x_i-\bar{x})^2
$$

### Standard Deviation

$$
\sigma=
\sqrt{\sigma^2}
$$

### Covariance

$$
Cov(X,Y)
=
E[(X-\mu_X)(Y-\mu_Y)]
$$

### Correlation

$$
\rho=
\frac{Cov(X,Y)}
{\sigma_X\sigma_Y}
$$

### MSE

$$
MSE=
\frac{1}{N}
\sum(I_i-J_i)^2
$$

### RMSE

$$
RMSE=\sqrt{MSE}
$$

### PSNR

$$
PSNR=
10\log_{10}
\left(
\frac{MAX^2}{MSE}
\right)
$$

---

# 39. Practice Questions

### Question 1

Find the mean:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

**Answer:**

$$
\boxed{30}
$$

---

### Question 2

Find the median:

$$
10,\ 20,\ 30,\ 40,\ 50
$$

**Answer:**

$$
\boxed{30}
$$

---

### Question 3

Find the mode:

$$
10,\ 20,\ 20,\ 30,\ 40
$$

**Answer:**

$$
\boxed{20}
$$

---

### Question 4

What does a high standard deviation generally indicate?

**Answer:** Greater spread or variation in the data.

---

### Question 5

What does an image histogram represent?

**Answer:** The distribution of pixel intensity values.

---

### Question 6

What does normalized histogram represent?

**Answer:** An empirical probability distribution of intensity values.

---

### Question 7

What does PSNR compare?

**Answer:** It expresses squared-error-based similarity relative to the signal's maximum value; it is commonly used to compare a reconstructed or processed image with a reference image.

---

# 40. Common Mistakes

### Mistake 1: Confusing Mean and Median

Mean:

$$
\text{Average}
$$

Median:

$$
\text{Middle Value}
$$

---

### Mistake 2: Forgetting to Sort for Median

The data must be sorted before finding the median.

---

### Mistake 3: Confusing Population and Sample Variance

Population:

$$
N
$$

Sample:

$$
N-1
$$

---

### Mistake 4: Assuming Correlation Means Causation

Correlation indicates statistical association, not necessarily causation.

---

### Mistake 5: Treating PSNR as a Complete Image Quality Measure

PSNR does not always match human or clinical perception of image quality.

---

# 41. Chapter Summary

You learned:

* What statistics is
* Population and sample
* Types of data
* Mean
* Median
* Mode
* Minimum and maximum
* Range
* Population variance
* Sample variance
* Standard deviation
* Histogram
* Normalized histogram
* Skewness
* Kurtosis
* Covariance
* Correlation
* Image statistics
* ROI statistics
* Noise statistics
* SNR
* CNR
* MSE
* RMSE
* PSNR
* Statistics in medical image processing

---

## Current Progress

```text
Module 2 — Mathematics Foundation

✅ Chapter 11: Algebra Fundamentals
✅ Chapter 12: Functions
✅ Chapter 13: Trigonometry
✅ Chapter 14: Vectors
✅ Chapter 15: Matrices
✅ Chapter 16: Matrix Operations
✅ Chapter 17: Linear Transformations
✅ Chapter 18: Eigenvalues and Eigenvectors
✅ Chapter 19: Calculus Fundamentals
✅ Chapter 20: Partial Derivatives
✅ Chapter 21: Gradient
✅ Chapter 22: Probability
✅ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 24 — Optimization Fundamentals**
