# Module 2 → Chapter 22: Probability

Probability is extremely important for:

* Image processing
* Medical image processing
* Computer vision
* Machine learning
* AI
* Image segmentation
* Noise modeling
* Statistical image analysis

Probability helps us mathematically describe **uncertainty**.

---

# 1. What Is Probability?

Probability measures the likelihood that an event will occur.

$$
0 \leq P(A) \leq 1
$$

Where:

* \(P(A)=0\) → impossible event
* \(P(A)=1\) → certain event
* \(0<P(A)<1\) → uncertain event

Example:

```text
P(Rain) = 0.7
```

This means the event has probability:

$$
70\%
$$

---

# 2. Experiment, Outcome and Event

## Experiment

An action that produces an outcome.

Examples:

* Rolling a dice
* Tossing a coin
* Selecting a pixel
* Measuring image noise

---

## Outcome

A possible result of an experiment.

For a coin:

```text
Head
Tail
```

---

## Sample Space

The collection of all possible outcomes.

For a coin:

$$
S=\{H,T\}
$$

For a six-sided dice:

$$
S=\{1,2,3,4,5,6\}
$$

---

## Event

An event is a set of one or more outcomes.

Example:

```text
Dice Result = Even Number
```

Then:

$$
A=\{2,4,6\}
$$

---

# 3. Basic Probability Formula

If all outcomes are equally likely:

$$
\boxed{
P(A)=
\frac{\text{Number of favorable outcomes}}
{\text{Total number of possible outcomes}}
}
$$

Example:

Probability of rolling a 4:

$$
P(4)=\frac{1}{6}
$$

---

# 4. Probability Example

A dice has:

$$
6
$$

possible outcomes.

Find the probability of getting an even number.

Favorable outcomes:

$$
\{2,4,6\}
$$

Therefore:

$$
P(\text{Even})
=
\frac{3}{6}
=
\frac{1}{2}
$$

---

# 5. Complement of an Event

The complement of event \(A\) means:

> Event \(A\) does not occur.

It is written:

$$
A^c
$$

The probability is:

$$
\boxed{
P(A^c)=1-P(A)
}
$$

Example:

If:

$$
P(A)=0.7
$$

Then:

$$
P(A^c)=1-0.7
$$

$$
=0.3
$$

---

# 6. Union of Events

The union means:

```text
A OR B
```

Written as:

$$
A\cup B
$$

The probability formula is:

$$
\boxed{
P(A\cup B)
=
P(A)+P(B)-P(A\cap B)
}
$$

Where:

$$
A\cap B
$$

means both events occur.

---

# 7. Example of Union

Suppose:

$$
P(A)=0.4
$$

$$
P(B)=0.5
$$

$$
P(A\cap B)=0.2
$$

Then:

$$
P(A\cup B)
=
0.4+0.5-0.2
$$

$$
=
\boxed{0.7}
$$

---

# 8. Intersection of Events

Intersection means:

```text
A AND B
```

Written as:

$$
A\cap B
$$

Example:

```text
Event A → Pixel is bright
Event B → Pixel belongs to tumor region
```

The intersection represents pixels satisfying both conditions.

---

# 9. Mutually Exclusive Events

Two events are mutually exclusive if they cannot happen together.

Example:

When rolling one dice:

```text
A = Roll a 2
B = Roll a 5
```

They cannot occur simultaneously.

Therefore:

$$
P(A\cap B)=0
$$

Thus:

$$
P(A\cup B)=P(A)+P(B)
$$

---

# 10. Independent Events

Events are independent if one event does not affect the probability of another.

For independent events:

$$
\boxed{
P(A\cap B)=P(A)P(B)
}
$$

Example:

Two independent coin tosses:

$$
P(H,H)
=
\frac{1}{2}
\times
\frac{1}{2}
$$

$$
=
\boxed{\frac{1}{4}}
$$

---

# 11. Conditional Probability

Conditional probability means:

> What is the probability of \(A\) given that \(B\) has already occurred?

Written as:

$$
P(A|B)
$$

Formula:

$$
\boxed{
P(A|B)
=
\frac{P(A\cap B)}
{P(B)}
}
$$

provided:

$$
P(B)>0
$$

---

# 12. Conditional Probability Example

Suppose:

$$
P(A\cap B)=0.2
$$

and:

$$
P(B)=0.5
$$

Then:

$$
P(A|B)
=
\frac{0.2}{0.5}
$$

$$
=
\boxed{0.4}
$$

---

# 13. Bayes' Theorem

Bayes' theorem is extremely important in:

* Medical diagnosis
* AI
* Machine learning
* Classification

The formula is:

$$
\boxed{
P(A|B)
=
\frac{P(B|A)P(A)}
{P(B)}
}
$$

---

# 14. Medical Example of Bayes' Theorem

Suppose:

```text
A = Patient has disease
B = Medical test is positive
```

Then:

$$
P(A|B)
$$

means:

> Probability that the patient has the disease given that the test is positive.

Bayes' theorem combines:

* Prior probability
* Test sensitivity
* Evidence probability

---

# 15. Random Variables

A random variable assigns a numerical value to outcomes.

Example: Dice roll.

$$
X\in\{1,2,3,4,5,6\}
$$

There are two major types:

```text
Random Variables
│
├── Discrete
│
└── Continuous
```

---

# 16. Discrete Random Variable

A discrete random variable has countable values.

Examples:

* Number of detected objects
* Number of tumors
* Number of noisy pixels

Example:

$$
X\in\{0,1,2,3\}
$$

---

# 17. Continuous Random Variable

A continuous random variable can take values over a continuous range.

Examples:

* Temperature
* Time
* Image intensity in a continuous physical model
* Radiation dose

Example:

$$
X\in[0,100]
$$

---

# 18. Probability Mass Function (PMF)

For a discrete random variable:

$$
P(X=x)
$$

describes the probability of a particular value.

Example:

| X | P(X) |
| - | ---- |
| 1 | 0.2  |
| 2 | 0.3  |
| 3 | 0.5  |

Important rule:

$$
\sum P(X=x)=1
$$

---

# 19. Probability Density Function (PDF)

For continuous random variables, probability is described using a density function:

$$
f(x)
$$

Important:

$$
P(a\leq X\leq b)
=
\int_a^b f(x)dx
$$

For a continuous random variable:

$$
P(X=x)=0
$$

The probability of an interval is obtained from the area under the PDF.

---

# 20. Cumulative Distribution Function

The cumulative distribution function is:

$$
\boxed{
F(x)=P(X\leq x)
}
$$

For a continuous variable:

$$
F(x)
=
\int_{-\infty}^{x}f(t)dt
$$

---

# 21. Expected Value

The expected value represents the long-run average.

For a discrete variable:

$$
\boxed{
E[X]
=
\sum xP(X=x)
}
$$

---

# 22. Expected Value Example

Suppose:

| X | P(X) |
| - | ---- |
| 1 | 0.5  |
| 2 | 0.3  |
| 3 | 0.2  |

Then:

$$
E[X]
=
1(0.5)+2(0.3)+3(0.2)
$$

$$
=
0.5+0.6+0.6
$$

$$
=
\boxed{1.7}
$$

---

# 23. Variance

Variance measures how much values spread around the mean.

$$
\boxed{
Var(X)
=
E[(X-\mu)^2]
}
$$

Another formula:

$$
\boxed{
Var(X)
=
E[X^2]-E[X]^2
}
$$

Where:

$$
\mu=E[X]
$$

---

# 24. Standard Deviation

Standard deviation is:

$$
\boxed{
\sigma=
\sqrt{Var(X)}
}
$$

A larger standard deviation generally indicates greater variation.

---

# 25. Common Probability Distributions

Important distributions include:

```text
Probability Distributions
│
├── Uniform
├── Bernoulli
├── Binomial
├── Gaussian / Normal
├── Poisson
└── Exponential
```

---

# 26. Uniform Distribution

Every value within the defined discrete set or continuous range follows the appropriate uniform model.

Example for a discrete fair dice:

$$
P(X=x)=\frac{1}{6}
$$

for:

$$
x\in\{1,2,3,4,5,6\}
$$

---

# 27. Bernoulli Distribution

A Bernoulli variable has two possible outcomes.

```text
Success = 1
Failure = 0
```

Probability:

$$
P(X=1)=p
$$

$$
P(X=0)=1-p
$$

Example:

```text
Disease Present
Disease Absent
```

---

# 28. Binomial Distribution

The binomial distribution models the number of successes in repeated independent Bernoulli trials.

Example:

```text
Number of successful detections
out of 10 independent trials
```

---

# 29. Gaussian Distribution

The Gaussian distribution is one of the most important distributions in image processing.

Its PDF is:

$$
\boxed{
f(x)
=
\frac{1}
{\sigma\sqrt{2\pi}}
e^{
-\frac{(x-\mu)^2}
{2\sigma^2}
}
}
$$

Where:

* \(\mu\) = mean
* \(\sigma\) = standard deviation

---

# 30. Gaussian Distribution Shape

```text
              /\
             /  \
            /    \
-----------/------\-----------
           μ
```

The mean controls the center.

The standard deviation controls the spread.

---

# 31. Gaussian Noise in Images

A common image model is:

$$
I_{observed}
=
I_{original}
+
N
$$

Where:

$$
N\sim N(\mu,\sigma^2)
$$

This represents Gaussian-distributed noise under the model assumptions.

---

# 32. Why Probability Matters in Medical Imaging

Medical imaging contains uncertainty due to factors such as:

* Sensor noise
* Acquisition noise
* Reconstruction effects
* Patient motion
* Measurement uncertainty

Probability helps model this uncertainty.

---

# 33. Probability in Image Segmentation

Suppose each pixel belongs to one of two classes:

```text
Background
Tumor
```

A probabilistic model may estimate:

$$
P(Tumor|Features)
$$

The system can then assign probabilities to possible classifications.

---

# 34. Probability in Machine Learning

Machine learning models may estimate:

$$
P(Class|Input)
$$

Example:

$$
P(
\text{Tumor}
|
\text{Image Features}
)
$$

Probability can help represent prediction confidence or model-estimated class likelihood, depending on the model and calibration.

---

# 35. Probability and Image Noise

Different imaging systems can have different noise characteristics.

Examples include:

* Gaussian noise
* Poisson noise
* Signal-dependent noise

For example, Poisson models are commonly relevant where measured counts follow counting statistics.

---

# 36. Histogram and Probability

An image histogram shows frequency information.

A normalized histogram can approximate a probability distribution:

$$
\boxed{
P(i)
=
\frac{n_i}{N}
}
$$

Where:

* \(n_i\) = number of pixels with intensity \(i\)
* \(N\) = total number of pixels

Then:

$$
\sum_i P(i)=1
$$

This is important for:

* Thresholding
* Image enhancement
* Statistical image analysis

---

# 37. Example: Image Intensity Probability

Suppose an image contains:

```text
Intensity 0 → 100 pixels
Intensity 1 → 300 pixels
Intensity 2 → 600 pixels
```

Total:

$$
N=1000
$$

Therefore:

$$
P(0)=\frac{100}{1000}=0.1
$$

$$
P(1)=\frac{300}{1000}=0.3
$$

$$
P(2)=\frac{600}{1000}=0.6
$$

Check:

$$
0.1+0.3+0.6=1
$$

---

# 38. C++ Example: Basic Probability

```cpp
#include <iostream>

int main()
{
    int favorable = 3;
    int total = 6;

    double probability =
        static_cast<double>(favorable) / total;

    std::cout << probability;

    return 0;
}
```

Output:

```text
0.5
```

---

# 39. C++ Example: Conditional Probability

```cpp
double conditionalProbability(
    double intersection,
    double probabilityB)
{
    if (probabilityB == 0.0)
        return 0.0;

    return intersection / probabilityB;
}
```

Example:

```cpp
double result =
    conditionalProbability(0.2, 0.5);
```

Result:

$$
0.4
$$

---

# 40. Important Formulas

### Basic Probability

$$
P(A)
=
\frac{\text{Favorable Outcomes}}
{\text{Total Outcomes}}
$$

### Complement

$$
P(A^c)=1-P(A)
$$

### Union

$$
P(A\cup B)
=
P(A)+P(B)-P(A\cap B)
$$

### Independent Events

$$
P(A\cap B)
=
P(A)P(B)
$$

### Conditional Probability

$$
P(A|B)
=
\frac{P(A\cap B)}
{P(B)}
$$

### Bayes' Theorem

$$
P(A|B)
=
\frac{P(B|A)P(A)}
{P(B)}
$$

### Expected Value

$$
E[X]
=
\sum xP(X=x)
$$

### Variance

$$
Var(X)
=
E[(X-\mu)^2]
$$

### Standard Deviation

$$
\sigma=\sqrt{Var(X)}
$$

---

# 41. Practice Questions

### Question 1

A fair coin is tossed once. What is:

$$
P(Head)
$$

**Answer:**

$$
\boxed{\frac{1}{2}}
$$

---

### Question 2

If:

$$
P(A)=0.8
$$

Find:

$$
P(A^c)
$$

**Answer:**

$$
1-0.8
=
\boxed{0.2}
$$

---

### Question 3

If:

$$
P(A)=0.4
$$

$$
P(B)=0.5
$$

$$
P(A\cap B)=0.1
$$

Find:

$$
P(A\cup B)
$$

**Answer:**

$$
0.4+0.5-0.1
=
\boxed{0.8}
$$

---

### Question 4

If:

$$
P(A\cap B)=0.3
$$

and:

$$
P(B)=0.6
$$

Find:

$$
P(A|B)
$$

**Answer:**

$$
\frac{0.3}{0.6}
=
\boxed{0.5}
$$

---

### Question 5

What does a normalized image histogram represent approximately?

**Answer:** An empirical probability distribution of pixel intensity values.

---

# 42. Common Mistakes

### Mistake 1: Probability Greater Than 1

Incorrect:

$$
P(A)=1.5
$$

Correct:

$$
0\leq P(A)\leq1
$$

---

### Mistake 2: Forgetting Intersection in the Union Formula

Incorrect:

$$
P(A\cup B)=P(A)+P(B)
$$

This is only generally valid when \(A\) and \(B\) are mutually exclusive.

Correct:

$$
P(A\cup B)
=
P(A)+P(B)-P(A\cap B)
$$

---

### Mistake 3: Confusing Independence and Mutual Exclusivity

Independent events:

$$
P(A\cap B)=P(A)P(B)
$$

Mutually exclusive events:

$$
P(A\cap B)=0
$$

They are different concepts.

---

### Mistake 4: Confusing PDF With Probability

For continuous variables:

$$
f(x)
$$

is a density.

Probability over an interval is:

$$
P(a\leq X\leq b)
=
\int_a^b f(x)dx
$$

---

# 43. Chapter Summary

You learned:

* Probability fundamentals
* Experiments, outcomes and events
* Sample space
* Basic probability
* Complement
* Union
* Intersection
* Independent events
* Mutually exclusive events
* Conditional probability
* Bayes' theorem
* Random variables
* Discrete and continuous variables
* PMF
* PDF
* CDF
* Expected value
* Variance
* Standard deviation
* Uniform distribution
* Bernoulli distribution
* Binomial distribution
* Gaussian distribution
* Poisson distribution
* Probability in image noise
* Probability in medical imaging
* Probability in segmentation
* Probability in machine learning
* Histogram as probability distribution

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
⬜ Chapter 23: Statistics
⬜ Chapter 24: Optimization Fundamentals
```

## Next: **Chapter 23 — Statistics**
