# Chapter 23 — Image Compression

We continue **strictly according to your index**.

This chapter is particularly important for your medical imaging software because a DICOM study can contain:

```text
CT      → hundreds/thousands of slices
MRI     → many series and volumes
X-Ray   → large 2D images
Mammography → very high-resolution images
```

Compression can significantly reduce storage and network bandwidth, but in medical imaging the distinction between **lossless and lossy** compression is critical.

---

# 23.1 Why Image Compression Is Required

Suppose one CT slice is:

[
512\times512
]

with 16-bit pixels.

Storage:

[
512\times512\times2
]

[
=524,288\text{ bytes}
]

approximately:

[
\boxed{512\text{ KB}}
]

For 500 slices:

[
512\times500=256,000\text{ KB}
]

approximately:

[
\boxed{250\text{ MB}}
]

for just one series, before considering metadata and other series.

A large medical study can therefore become very large.

---

# 23.2 Compression Goal

Compression attempts to represent the same information using fewer bits.

```text
Original Image
      ↓
 Compression
      ↓
Compressed Data
```

Then:

```text
Compressed Data
      ↓
Decompression
      ↓
Image
```

---

# 23.3 Compression Ratio

A common definition is:

[
\boxed{
CR=
\frac{\text{Original Size}}
{\text{Compressed Size}}
}
]

Example:

Original:

[
100\text{ MB}
]

Compressed:

[
25\text{ MB}
]

Then:

[
CR=\frac{100}{25}=4
]

Therefore:

[
\boxed{
4:1
}
]

---

# 23.4 Storage Reduction

The percentage reduction is:

[
\boxed{
Reduction=
\left(
1-\frac{Compressed}{Original}
\right)\times100
}
]

For 100 MB → 25 MB:

[
(1-0.25)\times100
]

[
=\boxed{75%}
]

---

# 23.5 Redundancy

Compression works by exploiting redundancy.

Major types include:

```text
Redundancy
 │
 ├── Coding redundancy
 ├── Spatial redundancy
 └── Psychovisual redundancy
```

For medical images, spatial and coding redundancy are particularly important.

---

# 23.6 Coding Redundancy

Suppose some values occur much more frequently than others:

```text
A → 80%
B → 10%
C → 5%
D → 5%
```

Using equal-length codes wastes bits.

Instead:

```text
A → short code
B → longer
C → longer
D → longer
```

This is the basic idea behind entropy coding.

---

# 23.7 Spatial Redundancy

Neighboring image pixels are often correlated.

Example:

```text id="g6z9f1"
100 101 101 102 102
100 100 101 102 103
101 101 102 102 103
```

Instead of storing every value independently, we can exploit relationships between neighboring values.

---

# 23.8 Lossless Compression

Lossless compression means:

[
\boxed{
Decompress(Compress(I))=I
}
]

exactly.

No pixel/sample information is intentionally lost.

```text
Original
   ↓
Lossless Compression
   ↓
Compressed
   ↓
Decompression
   ↓
Exact Original
```

---

# 23.9 Lossy Compression

Lossy compression intentionally discards information.

Therefore:

[
\boxed{
Decompressed(I)\neq I
}
]

pixel-for-pixel.

The goal is usually:

```text
Much smaller file
+
Acceptable visual quality
```

---

# 23.10 Lossless vs Lossy

| Lossless                            | Lossy                                          |
| ----------------------------------- | ---------------------------------------------- |
| Exact reconstruction                | Approximate reconstruction                     |
| No information loss                 | Information discarded                          |
| Usually lower compression           | Usually higher compression                     |
| Important for original medical data | Requires careful clinical/technical evaluation |
| PNG, JPEG-LS lossless modes         | JPEG lossy                                     |

---

# 23.11 Why Medical Imaging Is Special

For a normal photograph:

```text
Small visual differences
```

may be acceptable.

For medical data:

```text
Small numerical difference
```

could potentially affect:

* measurements
* segmentation
* quantitative analysis
* AI processing
* window/level behavior
* diagnostic interpretation

Therefore compression policy must be designed carefully.

---

# 23.12 Run-Length Encoding

RLE means:

[
\boxed{
Run\text{-}Length\ Encoding
}
]

It compresses repeated values.

Example:

```text id="q8d4t0"
AAAAAAA
```

becomes:

```text id="qk1p3z"
7A
```

---

# 23.13 RLE Image Example

Suppose:

```text id="e7h9k2"
0 0 0 0 0 1 1 1 0 0
```

RLE:

```text id="j2m7x8"
5×0
3×1
2×0
```

or:

```text
(0,5)
(1,3)
(0,2)
```

---

# 23.14 When RLE Works Well

RLE is effective when there are long runs:

```text id="v3m0v1"
000000000000
111111111111
```

It works poorly when values change frequently:

```text id="5c1k7s"
0 1 0 1 1 0 1 0 1 0
```

---

# 23.15 RLE in Medical Imaging

RLE can be useful for:

* binary masks
* segmentation labels
* simple images
* sparse data

Example segmentation:

```text id="q6p3m1"
0000000000
0001111000
0011111100
0001111000
0000000000
```

Long zero runs can be compressed efficiently.

---

# 23.16 Huffman Coding

Huffman coding is a variable-length entropy coding technique.

Frequently occurring symbols get shorter codes.

Rare symbols get longer codes.

Example:

```text id="8a1n5q"
A → 0
B → 10
C → 110
D → 111
```

The most frequent symbol receives the shortest code.

---

# 23.17 Huffman Tree

Conceptually:

```text id="2t9m4k"
             Root
            /    \
           0      1
          A      / \
                0   1
                B   / \
                   C   D
```

Codes:

```text
A → 0
B → 10
C → 110
D → 111
```

---

# 23.18 Why Huffman Works

Suppose:

```text
A = 70%
B = 15%
C = 10%
D = 5%
```

Using:

```text
A → 1 bit
B → 2 bits
C → 3 bits
D → 3 bits
```

can reduce average code length compared with assigning 2 bits to every symbol.

---

# 23.19 Prefix-Free Property

A Huffman code is prefix-free.

That means:

```text
No complete code is the prefix of another code.
```

Therefore decoding is unambiguous.

For example:

```text
A → 0
B → 10
C → 110
D → 111
```

works because:

```text
0
10
110
111
```

do not create decoding ambiguity.

---

# 23.20 Arithmetic Coding

Arithmetic coding represents an entire sequence using a fractional interval.

Instead of assigning an individual codeword to every symbol:

```text
Symbols
 ↓
Probability model
 ↓
Interval
 ↓
Compressed representation
```

It can achieve compression closer to the source entropy than basic Huffman coding in many situations.

---

# 23.21 Arithmetic Coding Intuition

Start with:

[
[0,1)
]

Suppose:

```text
A = 0.6
B = 0.3
C = 0.1
```

Partition:

```text
0       0.6    0.9    1
|---------|------|-----|
    A        B      C
```

Each next symbol narrows the interval.

Eventually a number inside the final interval represents the sequence.

---

# 23.22 Huffman vs Arithmetic

| Huffman               | Arithmetic                                   |
| --------------------- | -------------------------------------------- |
| Variable-length codes | Fractional interval representation           |
| Simple                | More mathematically involved                 |
| Fast                  | Can provide better compression in some cases |
| Widely used           | Used in several advanced codecs              |

---

# 23.23 LZW

LZW means:

[
\boxed{
Lempel\text{-}Ziv\text{-}Welch
}
]

It is a dictionary-based lossless compression technique.

Instead of repeatedly storing patterns:

```text
ABABABABAB
```

the algorithm builds a dictionary of repeated sequences.

---

# 23.24 LZW Concept

```text
Input
 ↓
Find repeated patterns
 ↓
Dictionary
 ↓
Output dictionary codes
```

Example conceptually:

```text
AB → code 256
ABA → code 257
```

Then repeated sequences can be represented by compact codes.

---

# 23.25 PNG

PNG is a lossless image format.

Conceptually:

```text
Image
 ↓
Filtering
 ↓
DEFLATE compression
 ↓
PNG
```

PNG is particularly effective for:

* screenshots
* diagrams
* graphics
* images with large uniform regions

---

# 23.26 PNG Filtering

PNG applies predictive filters before entropy compression.

For example:

```text
Current pixel
    ↓
Predict from neighboring pixels
    ↓
Store difference
    ↓
Compress differences
```

Because neighboring pixels are correlated, differences may have smaller values and more redundancy.

---

# 23.27 JPEG

JPEG is a widely used **lossy image compression** standard, although JPEG also has less commonly used lossless modes.

The common lossy pipeline is approximately:

```text
RGB
 ↓
Color transform
 ↓
Chroma subsampling
 ↓
8×8 blocks
 ↓
DCT
 ↓
Quantization
 ↓
Entropy coding
 ↓
JPEG
```

---

# 23.28 JPEG Color Transformation

JPEG typically converts RGB into a luminance/chrominance representation such as:

```text
RGB
 ↓
YCbCr
```

This helps exploit human visual sensitivity differences.

---

# 23.29 Chroma Subsampling

Human vision is generally less sensitive to high-frequency chroma detail than luminance detail.

Therefore JPEG may reduce chroma resolution.

Common notation:

```text
4:4:4
4:2:2
4:2:0
```

---

# 23.30 4:4:4

No chroma subsampling.

```text
Y Y Y Y
C C C C
C C C C
```

Highest chroma resolution among these common schemes.

---

# 23.31 4:2:2

Horizontal chroma resolution is reduced.

Conceptually:

```text
Y Y Y Y
C   C
C   C
```

---

# 23.32 4:2:0

Chroma is reduced both horizontally and vertically.

It provides greater compression but less color detail.

This is common in consumer image/video applications.

---

# 23.33 DCT

JPEG uses the Discrete Cosine Transform.

For an image block:

```text
8×8 pixels
    ↓
DCT
    ↓
frequency coefficients
```

Instead of representing the block directly as pixels, it represents spatial frequency components.

---

# 23.34 Low vs High Frequency

Low-frequency components:

```text
Smooth large-scale changes
```

High-frequency components:

```text
Edges
Fine details
Noise
```

JPEG can exploit this because high-frequency information can often be reduced more aggressively for visually acceptable results.

---

# 23.35 Quantization

After DCT:

```text
DCT coefficients
      ↓
Quantization
      ↓
Smaller / zero coefficients
```

Conceptually:

[
\boxed{
Q=\operatorname{round}\left(\frac{DCT}{QTable}\right)
}
]

This is where the common lossy JPEG process discards information.

---

# 23.36 Why JPEG Gets Smaller

After quantization:

```text
Many coefficients
       ↓
Become zero
```

Then entropy coding becomes highly effective.

Pipeline:

```text
DCT
 ↓
Quantization
 ↓
Many zeros
 ↓
Entropy coding
 ↓
Small file
```

---

# 23.37 JPEG Artifacts

At high compression:

```text
Original
 ↓
JPEG
```

can produce:

* blocking
* ringing
* loss of fine detail
* color artifacts

Example:

```text
8×8 blocks
████████
████████
```

may become visually apparent around sharp edges.

---

# 23.38 Medical Imaging Concern

Fine structures may be clinically important.

For example:

```text
Tiny lesion
Fine vessel
Subtle boundary
Microcalcification
```

Aggressive lossy compression can potentially obscure or alter such information.

Therefore medical workflows must use compression according to validated clinical and regulatory requirements.

---

# 23.39 JPEG 2000

JPEG 2000 is a more advanced image compression standard.

It uses wavelet-based processing rather than the conventional JPEG 8×8 DCT pipeline.

Conceptually:

```text
Image
 ↓
Wavelet Transform
 ↓
Quantization
 ↓
Entropy Coding
 ↓
JPEG 2000
```

---

# 23.40 JPEG 2000 Advantages

Features can include:

* lossless compression
* lossy compression
* progressive decoding
* resolution scalability
* quality scalability
* region-of-interest capabilities

These can be valuable for large medical images.

---

# 23.41 JPEG 2000 in Medical Imaging

It has been used in medical imaging because large images can benefit from:

```text
Large image
 ↓
Progressive access
 ↓
Resolution / quality layers
```

However, actual support depends on the DICOM transfer syntax and implementation.

---

# 23.42 JPEG-LS

JPEG-LS is designed primarily for efficient lossless and near-lossless image compression.

It uses predictive coding.

Conceptually:

```text
Neighboring pixels
 ↓
Predict current pixel
 ↓
Encode prediction error
 ↓
Compress
```

---

# 23.43 JPEG-LS Prediction

Suppose:

```text id="j8s1x0"
Left   = 100
Above  = 102
```

and the current pixel is expected to be near:

```text
101
```

Instead of encoding:

```text
101
```

the codec can encode a small prediction residual.

Small residuals are easier to compress.

---

# 23.44 JPEG-LS Medical Importance

JPEG-LS is particularly relevant when:

```text
Exact image reconstruction
+
Efficient compression
```

are desired.

It is one of the compression standards encountered in medical/DICOM workflows.

---

# 23.45 DICOM and Compression

DICOM does not simply mean:

```text
.dcm = one fixed compression format
```

DICOM uses:

[
\boxed{
Transfer\ Syntax
}
]

to specify how the dataset and pixel data are encoded.

---

# 23.46 Transfer Syntax

Transfer Syntax describes things such as:

* byte ordering
* VR encoding
* compression
* encapsulation

For pixel data, this can determine whether the image is:

```text
Uncompressed
JPEG
JPEG-LS
JPEG 2000
RLE
etc.
```

---

# 23.47 DICOM Pixel Data Pipeline

Your DICOM loader should conceptually do:

```text
DICOM File
 ↓
Parse Meta Information
 ↓
Read Transfer Syntax UID
 ↓
Determine pixel encoding
 ↓
Decode compressed Pixel Data
 ↓
Decoded Pixel Buffer
 ↓
Modality LUT
 ↓
VOI / Window-Level
 ↓
Display
```

---

# 23.48 DICOM Encapsulation

Compressed pixel data in DICOM is generally stored as encapsulated Pixel Data.

Conceptually:

```text
DICOM Dataset
      │
      └── Pixel Data
             │
             ├── Fragment 1
             ├── Fragment 2
             ├── Fragment 3
             └── ...
```

The codec reconstructs the image from these fragments.

---

# 23.49 DICOM Compression Examples

Common DICOM transfer syntaxes can include:

```text
JPEG Baseline
JPEG Extended
JPEG-LS
JPEG 2000
RLE Lossless
```

The exact UID and capabilities depend on the DICOM standard and implementation.

---

# 23.50 DICOM RLE

DICOM also defines a specific RLE Lossless transfer syntax.

Important:

```text
DICOM RLE
```

is not simply the same thing as saying:

```text
any generic RLE algorithm
```

The DICOM transfer syntax defines how the encoded pixel data is represented.

---

# 23.51 Lossless Medical Workflow

A conservative workflow:

```text
Acquisition
 ↓
Original DICOM
 ↓
Lossless storage
 ↓
PACS
 ↓
Viewer
 ↓
Exact pixel reconstruction
```

This preserves the original image samples after decompression.

---

# 23.52 Lossy Medical Workflow

If lossy compression is permitted:

```text
Original
 ↓
Validated lossy codec
 ↓
Compressed DICOM
 ↓
PACS
 ↓
Viewer
```

The system should retain metadata and provenance indicating the compression status according to the relevant DICOM requirements.

---

# 23.53 Compression and Diagnostic Quality

Do not assume:

[
\boxed{
Higher\ compression = always\ bad
}
]

or:

[
\boxed{
Lossless = always\ required
}
]

The correct policy depends on:

* modality
* clinical task
* image type
* intended use
* validated compression ratio
* applicable regulations
* institutional policy

---

# 23.54 PSNR

PSNR means:

[
\boxed{
Peak\ Signal\text{-}to\text{-}Noise\ Ratio
}
]

It compares reconstructed and original images.

First calculate MSE:

[
\boxed{
MSE=
\frac1N\sum_i(I_i-\hat I_i)^2
}
]

Then:

[
\boxed{
PSNR=
10\log_{10}
\left(
\frac{MAX_I^2}{MSE}
\right)
}
]

where (MAX_I) is the maximum possible pixel value.

---

# 23.55 PSNR Interpretation

Higher PSNR generally means smaller numerical error.

```text
High PSNR
   ↓
Low MSE
   ↓
Closer reconstruction
```

But:

[
\boxed{
PSNR \neq human\ perception
}
]

and it does not by itself establish diagnostic acceptability.

---

# 23.56 SSIM

SSIM means:

[
\boxed{
Structural\ Similarity\ Index
}
]

It compares images based on aspects such as:

* luminance
* contrast
* structure

A simplified conceptual model is:

[
SSIM(x,y)
=========

l(x,y)c(x,y)s(x,y)
]

where the terms compare luminance, contrast, and structure.

---

# 23.57 PSNR vs SSIM

| PSNR                                  | SSIM                                          |
| ------------------------------------- | --------------------------------------------- |
| Pixel-error based                     | Structural similarity                         |
| Easy to calculate                     | More perceptually oriented                    |
| Higher generally means lower MSE      | Often more aligned with structural appearance |
| Not sufficient for medical validation | Also not sufficient by itself                 |

---

# 23.58 Why Metrics Aren't Enough

Suppose:

```text
Image A
```

and:

```text
Image B
```

have excellent:

[
PSNR
]

but a tiny clinically important structure is altered.

A simple numerical metric may not capture the clinical significance.

Therefore:

[
\boxed{
Compression\ validation
must\ be\ task-specific.
}
]

---

# 23.59 Compression Artifacts

Important artifacts include:

### Blocking

Common in block-based codecs.

```text
|  |  |  |
████████
|  |  |  |
```

### Ringing

Oscillations around sharp edges.

### Blurring

Fine details disappear.

### Loss of texture

Small intensity variations are reduced.

---

# 23.60 Medical Example

Suppose a high-resolution mammography image contains:

```text
Fine structures
+
Microcalcifications
```

Aggressive compression could potentially affect subtle structures.

Therefore compression decisions for diagnostic images must be validated for the specific clinical task.

---

# 23.61 Lossless vs Near-Lossless

Near-lossless compression is between:

```text
Lossless
```

and:

```text
Strongly lossy
```

It limits the maximum reconstruction error.

For example:

[
|I-\hat I|\leq\epsilon
]

for a defined error bound, depending on the codec and mode.

---

# 23.62 Why Near-Lossless Can Be Useful

It may provide:

```text
Higher compression
+
Bounded numerical error
```

But "bounded error" does not automatically mean "clinically acceptable."

Clinical validation is still necessary.

---

# 23.63 Compression and Quantitative Imaging

Suppose your application performs:

```text
DICOM
 ↓
Segmentation
 ↓
Measurement
```

If the source has undergone lossy compression:

```text
Original values
      ↓
Compression
      ↓
Modified values
      ↓
Measurement
```

then the downstream measurement could potentially differ.

Therefore quantitative workflows need clearly defined input-data requirements.

---

# 23.64 Compression and AI

AI models may also be affected by compression.

Pipeline:

```text
Original
 ↓
Lossy compression
 ↓
AI model
 ↓
Prediction
```

Compression artifacts can potentially become distribution shifts.

Therefore AI pipelines should be validated with the actual compression conditions expected in deployment.

---

# 23.65 Compression Architecture for Your Viewer

For your DICOM viewer:

```text id="r7g0zn"
DICOM File
      ↓
DicomLoader
      ↓
Transfer Syntax
      ↓
Codec Manager
      │
      ├── Uncompressed
      ├── JPEG
      ├── JPEG-LS
      ├── JPEG 2000
      └── RLE
      ↓
Decoded Pixel Buffer
      ↓
Medical Image
      ↓
Window/Level
      ↓
Color LUT
      ↓
GPU/QML
```

---

# 23.66 Codec Manager

A clean architecture:

```cpp
class IImageCodec
{
public:
    virtual ~IImageCodec() = default;

    virtual Image decode(
        const CompressedData& data) = 0;

    virtual CompressedData encode(
        const Image& image) = 0;
};
```

Implementations:

```text
RawCodec
JpegCodec
JpegLsCodec
Jpeg2000Codec
DicomRleCodec
```

---

# 23.67 Codec Selection

Your DICOM loader can determine:

```text
Transfer Syntax UID
       ↓
CodecRegistry
       ↓
Appropriate codec
```

For example:

```cpp
auto codec =
    codecRegistry.find(transferSyntaxUid);
```

Then:

```cpp
Image image = codec->decode(pixelData);
```

This is much cleaner than putting every codec inside `DicomLoader`.

---

# 23.68 Why Separate Codec and DICOM Loader?

Because:

```text
DicomLoader
```

should primarily understand:

```text
DICOM structure
```

while:

```text
Codec
```

should understand:

```text
compression format
```

Architecture:

```text
DicomLoader
     ↓
TransferSyntax
     ↓
CodecManager
     ↓
Codec
     ↓
Decoded Image
```

This follows separation of concerns.

---

# 23.69 Medical Imaging Stack

For your project:

```text
                    DICOM
                      │
                 DCMTK / DICOM
                      │
                      ↓
              Transfer Syntax
                      │
                      ↓
                 Codec Layer
             ┌────────┼────────┐
             ↓        ↓        ↓
           JPEG     JPEG-LS  JPEG2000
             │        │        │
             └────────┼────────┘
                      ↓
                ITK / Image
                      ↓
             Processing Pipeline
                      ↓
                Qt/QML Viewer
```

This is a good enterprise-level separation.

---

# 23.70 DCMTK Role

For your medical imaging application, DCMTK can handle important DICOM-related functionality.

Conceptually:

```text
DICOM
 ↓
DCMTK
 ├── Dataset
 ├── Transfer Syntax
 ├── Pixel Data
 └── DICOM networking
```

Codec support may involve additional codec modules/dependencies depending on the transfer syntax.

---

# 23.71 ITK Role

ITK can work with decoded medical image data for:

* filtering
* segmentation
* registration
* volumetric processing

So:

```text
DCMTK
 ↓
DICOM decoding
 ↓
ITK
 ↓
Medical image processing
```

is a sensible architectural direction.

---

# 23.72 OpenCV Role

OpenCV can be useful after decoding:

```text
Decoded image
 ↓
OpenCV
 ├── image processing
 ├── color conversion
 ├── feature extraction
 └── computer vision
```

Again, select the library based on the specific feature rather than forcing all operations through one framework.

---

# 23.73 Compression and GPU

Compression/decompression can be CPU-intensive.

A high-performance viewer may use:

```text
Disk
 ↓
Compressed DICOM
 ↓
CPU Decode
 ↓
Decoded Buffer
 ↓
GPU Upload
 ↓
Rendering
```

The GPU generally handles visualization more naturally than general DICOM codec decoding.

---

# 23.74 Caching

For a large CT study:

```text
Disk
 ↓
Decode slice
 ↓
Cache
 ↓
Display
```

A cache can avoid repeatedly decoding the same image.

For example:

```text
LRU Cache
```

can keep recently viewed slices in memory.

---

# 23.75 Multi-Resolution Images

For extremely large images:

```text
Full resolution
 ↓
Reduced resolution
 ↓
Thumbnail
```

can be generated or stored.

The viewer can display low-resolution data during navigation and load higher-resolution data when the user zooms.

This is particularly useful for:

* digital pathology
* mammography
* very large radiographs

---

# 23.76 Progressive Loading

An enterprise viewer can use:

```text
Study
 ↓
Thumbnail
 ↓
Low resolution
 ↓
Full resolution
```

The user gets visual feedback quickly instead of waiting for the entire dataset.

---

# 23.77 Compression vs Encoding

Don't confuse:

```text
Encoding
```

with:

```text
Compression
```

Encoding can simply mean converting information into another representation.

Compression specifically attempts to reduce representation size.

For example:

```text
DICOM encoding
```

and:

```text
JPEG compression
```

are related but not identical concepts.

---

# 23.78 Compression Pipeline Summary

```text
Original Image
      ↓
Analyze redundancy
      ↓
Prediction / Transform
      ↓
Quantization? ← only lossy
      ↓
Entropy Coding
      ↓
Compressed Data
```

Lossless:

```text
No irreversible quantization
```

Lossy:

```text
Irreversible information reduction
```

---

# 23.79 Enterprise Compression Architecture

```text
                    IMAGE COMPRESSION
                           │
            ┌──────────────┴──────────────┐
            ↓                             ↓
         Lossless                        Lossy
            │                             │
      ┌─────┼─────┐                  ┌────┴────┐
      ↓     ↓     ↓                  ↓         ↓
     RLE   JPEG-LS PNG              JPEG     JPEG2000
      │
      └──────────────┐
                     ↓
               Exact Decode
```

---

# 23.80 Key Formulas

### Compression Ratio

[
\boxed{
CR=
\frac{Original\ Size}
{Compressed\ Size}
}
]

### Reduction

[
\boxed{
Reduction=
\left(1-\frac{Compressed}{Original}\right)\times100
}
]

### MSE

[
\boxed{
MSE=
\frac1N\sum_i(I_i-\hat I_i)^2
}
]

### PSNR

[
\boxed{
PSNR=
10\log_{10}
\left(
\frac{MAX_I^2}{MSE}
\right)
}
]

---

# 23.81 Important Interview Questions

### Fundamentals

1. What is image compression?
2. Why is compression required?
3. What is redundancy?
4. What is lossless compression?
5. What is lossy compression?
6. What is compression ratio?

### Algorithms

7. What is RLE?
8. When does RLE perform well?
9. What is Huffman coding?
10. Why are Huffman codes variable length?
11. What is arithmetic coding?
12. What is LZW?
13. What is predictive coding?

### JPEG

14. What is DCT?
15. Why does JPEG use DCT?
16. What is quantization?
17. Where does JPEG lose information?
18. What is chroma subsampling?
19. What is 4:4:4?
20. What is 4:2:2?
21. What is 4:2:0?
22. What are JPEG artifacts?

### Medical Imaging

23. Why is lossless compression important in medical imaging?
24. What is JPEG-LS?
25. What is JPEG 2000?
26. What is DICOM Transfer Syntax?
27. What is encapsulated Pixel Data?
28. What is DICOM RLE?
29. Why shouldn't the DICOM loader assume all pixel data is uncompressed?
30. Why can lossy compression matter for quantitative imaging?
31. Why is compression validation task-specific?

---

# 23.82 Practical Exercise — Compression Ratio

Original:

[
512\text{ MB}
]

Compressed:

[
128\text{ MB}
]

Calculate:

1. Compression ratio
2. Storage reduction percentage

---

# 23.83 Practical Exercise — PSNR

Suppose:

[
MSE=4
]

and:

[
MAX_I=255
]

Calculate:

[
PSNR=
10\log_{10}
\left(
\frac{255^2}{4}
\right)
]

Then explain why a high PSNR does not automatically prove that a medical image is clinically acceptable.

---

# 23.84 Practical Exercise — DICOM Viewer

Design the following:

```text
DICOM File
 ↓
Read Transfer Syntax
 ↓
Is compressed?
 ├── No → read pixel buffer
 │
 └── Yes
       ↓
   Find codec
       ↓
   Decode
       ↓
Pixel Buffer
 ↓
Modality LUT
 ↓
Window/Level
 ↓
Color LUT
 ↓
Overlay
 ↓
GPU
 ↓
QML
```

Then identify which component should own each step.

---

# 23.85 Chapter 23 Mental Model

Remember:

```text
Compression
     │
     ├── Lossless
     │     ├── RLE
     │     ├── Huffman
     │     ├── LZW
     │     ├── PNG
     │     └── JPEG-LS
     │
     └── Lossy
           ├── JPEG
           └── JPEG 2000 lossy mode
```

And:

[
\boxed{
DICOM
\rightarrow
Transfer\ Syntax
\rightarrow
Codec
\rightarrow
Decoded\ Pixel\ Data
}
]

The most important medical-imaging rule:

[
\boxed{
Never\ confuse\ compressed\ representation
with\ the\ original\ medical\ measurement.
}
]

**Chapter 23 complete.**

### Next, strictly according to your index:

# Chapter 24 — Image Restoration

Topics:

* Image degradation
* Noise models
* Gaussian noise
* Salt-and-pepper noise
* Poisson noise
* Speckle noise
* Periodic noise
* Blur
* Point Spread Function
* Degradation model
* Inverse filtering
* Wiener filtering
* Constrained least squares
* Median filtering
* Adaptive filtering
* Denoising
* Deblurring
* Medical image restoration
* CT/MRI/X-ray noise
* Restoration vs enhancement
