# Level 0 → Module 1 → Chapter 9: Multithreading Fundamentals

Multithreading is extremely important in **medical image processing** because images and volumes can be very large.

For example:

```text
CT Volume
512 × 512 × 300
```

That contains:

```text
78,643,200 voxels
```

Processing millions of pixels sequentially can be slow. Multithreading allows work to be divided across CPU cores.

---

# 1. What Is a Thread?

A **process** is a running program.

A **thread** is a path of execution inside a process.

```text
Process
│
├── Thread 1
├── Thread 2
├── Thread 3
└── Thread 4
```

Example:

```text
Medical Imaging Application
│
├── UI Thread
├── Image Loading Thread
├── Image Processing Thread
└── DICOM Processing Thread
```

---

# 2. Single-Threaded Processing

Suppose we process an image:

```text
Pixel 1
   ↓
Pixel 2
   ↓
Pixel 3
   ↓
Pixel 4
```

Only one task is processed at a time.

Example:

```cpp
for (std::size_t i = 0;
     i < image.size();
     ++i)
{
    image[i] =
        image[i] * 2;
}
```

For a large image, this may take significant time.

---

# 3. Multithreaded Processing

Instead of one thread:

```text
Thread 1

[--------------------------]
```

We divide work:

```text
Image

┌──────────┬──────────┬──────────┬──────────┐
│ Thread 1 │ Thread 2 │ Thread 3 │ Thread 4 │
└──────────┴──────────┴──────────┴──────────┘
```

Conceptually:

```text
Thread 1 → Pixels 0–25%
Thread 2 → Pixels 25–50%
Thread 3 → Pixels 50–75%
Thread 4 → Pixels 75–100%
```

---

# 4. Creating a Thread in C++

Include:

```cpp
#include <thread>
```

Example:

```cpp
#include <iostream>
#include <thread>

void process()
{
    std::cout << "Processing image\n";
}

int main()
{
    std::thread worker(process);

    worker.join();

    return 0;
}
```

---

# 5. What Is `join()`?

When a thread starts:

```cpp
std::thread worker(process);
```

The main thread and worker thread may run concurrently.

```text
Main Thread
    │
    ├───────┐
    │       │
    │       ▼
    │   Worker Thread
    │       │
    │       ▼
    └── wait using join()
```

```cpp
worker.join();
```

means:

> Wait until the worker thread finishes.

---

# 6. What Happens Without `join()`?

This is dangerous:

```cpp
std::thread worker(process);

// Program exits
```

A joinable `std::thread` must be handled before destruction.

Usually you must:

```cpp
worker.join();
```

or:

```cpp
worker.detach();
```

---

# 7. `detach()` — Use Carefully

Example:

```cpp
std::thread worker(process);

worker.detach();
```

The thread continues independently.

```text
Main Thread
    ↓
Continues

Worker Thread
    ↓
Continues independently
```

For medical image processing, careless use of `detach()` can be dangerous because the worker may access objects that have already been destroyed.

Prefer controlled thread ownership.

---

# 8. Lambda With Threads

A common approach:

```cpp
std::thread worker(
    []()
    {
        std::cout
            << "Processing";
    }
);

worker.join();
```

For image processing:

```cpp
std::thread worker(
    [&image]()
    {
        for (auto& pixel : image)
        {
            pixel *= 2;
        }
    }
);

worker.join();
```

Be careful with reference lifetime.

---

# 9. Passing Arguments to Threads

Example:

```cpp
#include <thread>

void processImage(
    int width,
    int height)
{
}

int main()
{
    std::thread worker(
        processImage,
        512,
        512
    );

    worker.join();
}
```

---

# 10. Why Multithreading Is Useful for Images

Imagine:

```text
Image Size:

4096 × 4096
```

Total pixels:

```text
16,777,216
```

A filter may need to process every pixel.

Single thread:

```text
Thread 1
████████████████████████
```

Multiple threads:

```text
Thread 1 ██████
Thread 2 ██████
Thread 3 ██████
Thread 4 ██████
```

Potential result:

```text
Less processing time
```

Actual speedup depends on CPU cores, memory bandwidth, algorithm, synchronization, and overhead.

---

# 11. `std::thread::hardware_concurrency()`

You can query the approximate number of hardware threads:

```cpp
unsigned int count =
    std::thread::hardware_concurrency();
```

Example:

```text
CPU

8 cores / logical processors available
```

However:

```text
hardware_concurrency()
```

is only a hint and may return `0`.

Do not blindly assume its value is always usable without a fallback.

---

# 12. Dividing Image Work

Suppose:

```text
Image pixels = 1,000,000

Threads = 4
```

Each thread can process approximately:

```text
250,000 pixels
```

Concept:

```text
0 -------------------------------- 1,000,000

|------|------|------|------|
   T1     T2     T3     T4
```

---

# 13. Parallel Image Processing Example

```cpp
#include <thread>
#include <vector>
#include <cstdint>

void processRange(
    std::vector<std::uint8_t>& image,
    std::size_t start,
    std::size_t end)
{
    for (
        std::size_t i = start;
        i < end;
        ++i)
    {
        image[i] =
            static_cast<std::uint8_t>(
                image[i] / 2
            );
    }
}
```

Create threads:

```cpp
std::size_t total =
    image.size();

std::size_t midpoint =
    total / 2;

std::thread thread1(
    processRange,
    std::ref(image),
    0,
    midpoint
);

std::thread thread2(
    processRange,
    std::ref(image),
    midpoint,
    total
);

thread1.join();
thread2.join();
```

---

# 14. Why `std::ref()` Is Used

Threads normally store copies of their arguments.

If we write:

```cpp
std::thread(
    processRange,
    image,
    0,
    midpoint
);
```

the vector may be copied.

Instead:

```cpp
std::ref(image)
```

passes a reference wrapper.

Therefore:

```text
Thread
   ↓
Original Image
```

instead of:

```text
Thread
   ↓
Copied Image
```

For large medical images, avoiding unnecessary copies is very important.

---

# 15. Race Condition

A **race condition** occurs when multiple threads access shared data and the result depends on timing.

Example:

```cpp
int counter = 0;
```

Thread 1:

```cpp
counter++;
```

Thread 2:

```cpp
counter++;
```

Expected:

```text
2
```

But the result may be:

```text
1
```

because:

```text
Thread 1 reads counter = 0
Thread 2 reads counter = 0

Thread 1 writes 1
Thread 2 writes 1
```

One update is lost.

---

# 16. Why Race Conditions Matter in Image Processing

Suppose multiple threads calculate:

```text
Total Intensity
```

Shared variable:

```cpp
std::uint64_t sum = 0;
```

Threads:

```cpp
sum += pixel;
```

Multiple simultaneous updates can cause incorrect results.

---

# 17. Mutex

A mutex protects shared resources.

Include:

```cpp
#include <mutex>
```

Example:

```cpp
std::mutex mutex;
```

Use:

```cpp
mutex.lock();

counter++;

mutex.unlock();
```

But manual locking is risky.

Better:

```cpp
std::lock_guard<std::mutex>
    lock(mutex);

counter++;
```

The mutex is automatically released when `lock` goes out of scope.

---

# 18. Race Condition Fixed With Mutex

```cpp
#include <thread>
#include <mutex>

int counter = 0;

std::mutex mutex;

void increment()
{
    std::lock_guard<std::mutex>
        lock(mutex);

    ++counter;
}
```

Now only one thread modifies `counter` at a time.

---

# 19. Important Performance Lesson

Avoid locking every pixel operation.

Bad:

```cpp
for (auto pixel : image)
{
    std::lock_guard<std::mutex>
        lock(mutex);

    sum += pixel;
}
```

This creates excessive synchronization.

Better:

```text
Each Thread
    ↓
Calculate Local Sum
    ↓
Combine Results
```

---

# 20. Local Accumulation

Example:

```cpp
void calculateSum(
    const std::vector<std::uint16_t>& image,
    std::size_t start,
    std::size_t end,
    std::uint64_t& result)
{
    std::uint64_t localSum = 0;

    for (
        std::size_t i = start;
        i < end;
        ++i)
    {
        localSum += image[i];
    }

    result = localSum;
}
```

Each thread calculates independently.

```text
Thread 1 → Local Sum 1
Thread 2 → Local Sum 2
Thread 3 → Local Sum 3
Thread 4 → Local Sum 4
```

Then:

```text
Total =

Sum1 + Sum2 + Sum3 + Sum4
```

This is often better than locking for every pixel.

---

# 21. Thread-Safe vs Non-Thread-Safe

### Thread-safe

Multiple threads can safely use the code simultaneously.

### Non-thread-safe

Concurrent access may cause:

* Race conditions
* Data corruption
* Crashes
* Undefined behavior

Example:

```text
Shared Image Buffer
       │
       ├── Thread 1 writes
       └── Thread 2 writes

Possible conflict
```

---

# 22. Read-Only Access Is Easier

Multiple threads can generally safely read immutable data concurrently.

Example:

```text
Image

Thread 1 → Read
Thread 2 → Read
Thread 3 → Read
Thread 4 → Read
```

No thread modifies the image.

This is ideal for many image-analysis operations.

---

# 23. Separate Output Regions

A very useful pattern:

```text
Input Image

[ Read Only ]
      │
      ▼

Thread 1 → Output Region 1
Thread 2 → Output Region 2
Thread 3 → Output Region 3
Thread 4 → Output Region 4
```

If each thread writes to a different valid memory region, synchronization may not be required for those writes.

Example:

```text
Input: Read-only

Output:

0–25%   → Thread 1
25–50%  → Thread 2
50–75%  → Thread 3
75–100% → Thread 4
```

This is common in image filtering.

---

# 24. Atomic Variables

For simple shared values:

```cpp
#include <atomic>

std::atomic<int> counter = 0;
```

Then:

```cpp
counter++;
```

is atomic.

Example:

```cpp
std::atomic<bool> cancelRequested =
    false;
```

A worker can check:

```cpp
if (cancelRequested)
{
    return;
}
```

Useful for:

* Cancellation flags
* Progress counters
* Simple states

Not every shared problem should be solved with atomics.

---

# 25. Condition Variables

A condition variable allows threads to wait for a condition.

Example concept:

```text
Thread A
   ↓
Wait for image loading
   ↓
Image loaded
   ↓
Thread A continues
```

Main components:

```cpp
std::mutex
std::condition_variable
```

This is useful for producer-consumer designs.

---

# 26. Producer–Consumer Concept

```text
DICOM Loader Thread
        │
        ▼
   Image Queue
        │
        ▼
Processing Thread
```

Producer:

```text
Loads Data
```

Consumer:

```text
Processes Data
```

This architecture is common in large medical-image applications.

---

# 27. Basic `std::async`

Another C++ approach:

```cpp
#include <future>

auto result =
    std::async(
        std::launch::async,
        []()
        {
            return 100;
        }
    );

int value =
    result.get();
```

Concept:

```text
Task
  ↓
Future
  ↓
Result
```

`get()` retrieves the result and waits if necessary.

---

# 28. Parallel Image Statistics Using `std::async`

Concept:

```text
Image
 │
 ├── Part 1 → Task 1
 ├── Part 2 → Task 2
 ├── Part 3 → Task 3
 └── Part 4 → Task 4

        ↓

Combine Results
```

Example:

```cpp
std::future<std::uint64_t> future =
    std::async(
        std::launch::async,
        calculateRange
    );
```

The result can later be obtained using:

```cpp
std::uint64_t result =
    future.get();
```

---

# 29. Exception Handling

With:

```cpp
std::thread
```

exceptions generally must be handled inside the thread function; an uncaught exception escaping a thread can terminate the program.

With:

```cpp
std::async
```

exceptions from the asynchronous task can be stored and rethrown when calling:

```cpp
future.get();
```

This can make result and exception handling more convenient.

---

# 30. UI Thread vs Worker Thread

In Qt applications:

```text
UI Thread
   │
   ├── Buttons
   ├── Windows
   └── Rendering coordination

Worker Thread
   │
   ├── Image Loading
   ├── Filtering
   └── Heavy Computation
```

Heavy processing should generally not block the UI thread.

Otherwise:

```text
Processing starts
      ↓
UI freezes
      ↓
Poor user experience
```

---

# 31. Medical Image Example

Suppose:

```text
CT Volume

512 × 512 × 300
```

Processing:

```text
Thread 1 → Slice 0–74

Thread 2 → Slice 75–149

Thread 3 → Slice 150–224

Thread 4 → Slice 225–299
```

Each thread processes separate slices.

This is a natural parallelization strategy for many volume operations.

---

# 32. Thread Count

More threads do not always mean better performance.

Example:

```text
CPU cores = 8

Threads = 1000
```

Possible problems:

* Context switching
* Memory pressure
* Scheduling overhead
* Synchronization overhead

A reasonable thread count depends on:

* Hardware
* CPU workload
* Memory bandwidth
* Algorithm
* Other running tasks

---

# 33. False Sharing

False sharing can occur when different threads modify different variables located close together in memory, causing cache-coherency overhead.

Conceptually:

```text
Cache Line

Thread 1 data | Thread 2 data
```

Both threads repeatedly modify nearby data.

Possible result:

```text
Cache traffic
      ↓
Performance reduction
```

This becomes important in high-performance image processing.

---

# 34. Deadlock

A deadlock occurs when threads wait forever for each other.

Example:

```text
Thread 1
holds Mutex A
waits for Mutex B

Thread 2
holds Mutex B
waits for Mutex A
```

Result:

```text
Program stops progressing
```

Avoid inconsistent lock ordering.

---

# 35. Multithreading Strategy for Images

A good general approach:

```text
Large Image
    ↓
Divide Into Independent Regions
    ↓
Assign Regions to Workers
    ↓
Each Worker Processes Locally
    ↓
Combine Results
```

Try to minimize:

```text
Shared Data
Mutex Usage
Memory Allocation
Thread Creation
```

---

# 36. Practical Example: Parallel Brightness Processing

```cpp
#include <thread>
#include <vector>
#include <cstdint>
#include <algorithm>

void adjustBrightness(
    std::vector<std::uint8_t>& image,
    std::size_t start,
    std::size_t end,
    int brightness)
{
    for (
        std::size_t i = start;
        i < end;
        ++i)
    {
        int value =
            static_cast<int>(
                image[i]
            ) + brightness;

        value =
            std::clamp(
                value,
                0,
                255
            );

        image[i] =
            static_cast<std::uint8_t>(
                value
            );
    }
}
```

Divide the image:

```cpp
std::size_t middle =
    image.size() / 2;

std::thread thread1(
    adjustBrightness,
    std::ref(image),
    0,
    middle,
    50
);

std::thread thread2(
    adjustBrightness,
    std::ref(image),
    middle,
    image.size(),
    50
);

thread1.join();
thread2.join();
```

Each thread processes a different region.

---

# 37. Common Mistakes

### Mistake 1: Too Many Threads

Creating one thread per pixel:

```text
1,000,000 pixels
       ↓
1,000,000 threads
```

Very bad.

---

### Mistake 2: Shared Data Without Protection

```cpp
sum += pixel;
```

from multiple threads can create a race condition.

---

### Mistake 3: Forgetting `join()`

Always manage thread lifetime correctly.

---

### Mistake 4: Locking Too Frequently

```text
Lock
Process one pixel
Unlock

Lock
Process one pixel
Unlock
```

This can destroy performance.

---

### Mistake 5: Modifying the Same Pixel

```text
Thread 1 → Pixel 100

Thread 2 → Pixel 100
```

Potential data race.

Better:

```text
Thread 1 → Pixels 0–499

Thread 2 → Pixels 500–999
```

---

# 38. Practical Exercise

Create a:

```text
4096 × 4096 image
```

Perform brightness adjustment.

### Version 1

Use:

```text
Single Thread
```

### Version 2

Use:

```text
4 Threads
```

Measure:

```text
Processing Time
```

Compare:

```text
Single Thread vs Multithreading
```

---

# 39. Mini Project

## Parallel Image Processor

Create:

```text
ParallelImageProcessor
```

Features:

```text
Input Image
    │
    ├── Divide Work
    │
    ├── Worker Threads
    │
    ├── Brightness
    │
    ├── Inversion
    │
    └── Statistics
           │
           ▼
        Result
```

Later improvements:

```text
Thread Pool
Task Queue
Cancellation
Progress Reporting
SIMD
GPU Processing
```

---

# 40. Interview Questions

### Beginner

1. What is a thread?
2. What is multithreading?
3. What does `join()` do?
4. What is a race condition?
5. What is a mutex?

### Intermediate

1. What is the difference between `join()` and `detach()`?
2. What is `std::ref()` and why is it useful?
3. What is an atomic variable?
4. What is a deadlock?
5. What is false sharing?

### Advanced

1. How would you parallelize a 3D medical image?
2. How would you avoid race conditions while calculating image statistics?
3. Why can too many threads reduce performance?
4. How would you divide image data efficiently?
5. Why is creating a thread per pixel a bad design?

---

# 41. Chapter Summary

You learned:

* Process vs thread
* `std::thread`
* `join()`
* `detach()`
* Lambda threads
* Passing arguments
* `std::ref`
* Race conditions
* Mutex
* `std::lock_guard`
* Local accumulation
* Thread safety
* Atomic variables
* Condition variables
* Producer-consumer concept
* `std::async`
* `std::future`
* UI thread vs worker thread
* Parallel image processing
* Deadlock
* False sharing
* Performance considerations

---

## Current Progress

* **Completed:** Chapter 7 — STL for Image Processing
* **Completed:** Chapter 8 — Templates
* **Completed:** Chapter 9 — Multithreading Fundamentals
* **Next:** **Chapter 10 — Performance Optimization**

In **Chapter 10**, we will focus on optimizing medical-image processing using:

```text
Algorithm Complexity
Memory Optimization
Cache Efficiency
Avoiding Copies
Move Semantics
Profiling
SIMD Fundamentals
Multithreading Performance
```

Say **Next** to continue with **Chapter 10: Performance Optimization**.
