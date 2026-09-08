# Chapter 39: Mammography

**Mammography** is a specialized **X-ray imaging technique for breast imaging**. It is primarily used to examine breast tissue and detect abnormalities that may not be easily visible or palpable.

---

# 1. Basic Mammography Concept

```text
X-Ray Source
     ↓
Breast Compression
     ↓
Low-Energy X-Rays
     ↓
Breast Tissue
     ↓
Detector
     ↓
Mammographic Image
```

Mammography creates high-detail X-ray images of the breast.

---

# 2. Why Is Mammography Important?

Mammography is used to examine breast tissue and may help identify:

* Masses
* Calcifications
* Tissue asymmetry
* Structural abnormalities

It plays an important role in breast imaging workflows.

---

# 3. Basic Mammography System

```text
Mammography System
│
├── X-Ray Tube
├── Compression Paddle
├── Breast Support Platform
├── X-Ray Detector
└── Image Processing System
```

Conceptually:

```text
        X-Ray Tube
             │
             ▼
      ┌─────────────┐
      │ Compression │
      │   Paddle    │
      └─────────────┘
             │
          BREAST
             │
             ▼
       X-Ray Detector
```

---

# 4. X-Ray Imaging Principle

Mammography uses differences in X-ray attenuation.

```text
X-Ray Beam
     ↓
Breast Tissue
     ↓
Different Attenuation
     ↓
Detector
     ↓
Image
```

Different tissues attenuate X-rays differently, producing image contrast.

---

# 5. Breast Compression

Compression is an important part of mammography acquisition.

```text
Compression Paddle
        ↓
      BREAST
        ↓
Support Platform
```

Compression helps:

* Reduce tissue thickness
* Reduce tissue overlap
* Improve image quality
* Improve visualization consistency

---

# 6. Why Reduce Tissue Overlap?

Breast tissue is a three-dimensional structure, but a conventional mammogram is a 2D projection.

```text
3D Breast Tissue
       ↓
2D Projection
       ↓
Possible Tissue Overlap
```

Compression and different views help reduce some overlap effects.

---

# 7. Mammographic Views

Common mammography acquisitions include different projection views.

Two commonly discussed views are:

```text
Mammography Views
│
├── CC View
│
└── MLO View
```

### CC — Craniocaudal

```text
Top
 ↓
Breast
 ↓
Detector
```

### MLO — Mediolateral Oblique

```text
Angled X-Ray Projection
        ↓
      Breast
        ↓
      Detector
```

Different views provide complementary visualization.

---

# 8. Digital Mammography

Modern mammography commonly uses digital detectors.

```text
X-Rays
   ↓
Digital Detector
   ↓
Digital Image
   ↓
Image Processing
   ↓
Display
```

Digital imaging allows software-based processing and visualization.

---

# 9. Image Resolution

High spatial resolution is important in mammography.

```text
Mammography Image
│
├── Fine Structures
├── Small Calcifications
└── Tissue Detail
```

Small structures may require careful image acquisition and processing.

---

# 10. Pixel Size

Digital mammography images contain pixels.

```text
Mammography Image
        ↓
┌──┬──┬──┐
│P │P │P │
├──┼──┼──┤
│P │P │P │
├──┼──┼──┤
│P │P │P │
└──┴──┴──┘
```

Smaller pixel sizes can support visualization of fine image details.

---

# 11. Image Contrast

Image contrast is important for distinguishing tissue structures.

```text
Tissue A
    ↓
Different X-Ray Attenuation
    ↓
Tissue B
    ↓
Different Image Intensity
```

Contrast processing is therefore important in digital mammography.

---

# 12. Calcifications

Calcifications are small deposits that may appear as high-intensity structures in mammographic images.

```text
Breast Image
    ↓
Small Bright Structures
    ↓
Possible Calcifications
```

Their appearance and pattern are evaluated in clinical interpretation.

---

# 13. Masses

A mass may appear as an area with characteristics different from surrounding tissue.

```text
Normal Tissue
      +
Different Region
      ↓
Image Finding
```

Image-processing methods may assist with visualization and analysis.

---

# 14. Breast Density

Breast density influences mammographic appearance.

Conceptually:

```text
More Dense Tissue
        ↓
Different X-Ray Attenuation
        ↓
Different Image Appearance
```

Breast density can affect tissue visualization.

---

# 15. Mammography Image Processing

A simplified processing pipeline:

```text
X-Ray Acquisition
      ↓
Raw Detector Data
      ↓
Detector Correction
      ↓
Image Processing
      ↓
Contrast Enhancement
      ↓
Noise Reduction
      ↓
Mammographic Image
```

---

# 16. Contrast Enhancement

Contrast enhancement may improve visualization of image structures.

```text
Original Image
      ↓
Contrast Processing
      ↓
Improved Visibility
```

However, processing must preserve clinically relevant image information.

---

# 17. Noise Reduction

Digital mammography images may contain noise.

```text
Measured Image
       =
Useful Signal
       +
Noise
```

Noise reduction methods must balance:

```text
Noise Reduction
      ↔
Fine Detail Preservation
```

This is especially important for small structures.

---

# 18. Image Segmentation

Segmentation can be used in research and computer-assisted analysis.

```text
Mammography Image
       ↓
Preprocessing
       ↓
Feature Extraction
       ↓
Segmentation
       ↓
Region Analysis
```

Possible targets include:

* Breast boundary
* Suspicious regions
* Mass candidates
* Calcification candidates

---

# 19. Feature Extraction

Computer algorithms can analyze image features such as:

```text
Image Features
│
├── Intensity
├── Texture
├── Shape
├── Edges
└── Spatial Patterns
```

These features can support computer-assisted image analysis.

---

# 20. Computer-Aided Detection

Computer-aided systems can assist image analysis.

```text
Mammography Image
        ↓
Image Processing
        ↓
Feature Extraction
        ↓
Candidate Detection
        ↓
Computer-Assisted Analysis
```

The software supports analysis; clinical interpretation remains part of the medical workflow.

---

# 21. AI in Mammography

AI methods can be applied to mammographic images.

```text
Mammography Image
       ↓
AI / Deep Learning Model
       ↓
Feature Learning
       ↓
Output / Analysis
```

Possible technical tasks include:

* Image classification
* Region detection
* Segmentation
* Image-quality analysis

---

# 22. 2D Mammography Limitation

A conventional mammogram is fundamentally a projection image.

```text
3D Breast
    ↓
X-Ray Projection
    ↓
2D Image
```

This can result in overlapping tissue structures.

---

# 23. Digital Breast Tomosynthesis (DBT)

**Digital Breast Tomosynthesis (DBT)** acquires multiple low-angle projection images.

```text
Multiple X-Ray Projections
           ↓
      Reconstruction
           ↓
     Breast Slices
```

Conceptually:

```text
Projection 1
Projection 2
Projection 3
      ↓
Reconstruction
      ↓
Tomosynthesis Images
```

DBT provides additional depth-related information compared with a single 2D projection.

---

# 24. Mammography vs DBT

| Mammography                      | DBT                          |
| -------------------------------- | ---------------------------- |
| 2D projection imaging            | Multiple projections         |
| Single projection view           | Multiple-angle acquisition   |
| Tissue overlap can occur         | Reduces some overlap effects |
| Standard breast imaging workflow | Tomographic breast imaging   |

---

# 25. Mammography Artifacts

Common artifact categories include:

```text
Mammography Artifacts
│
├── Motion
├── Positioning
├── Detector Artifacts
├── Processing Artifacts
└── External Objects
```

Artifacts can affect image interpretation.

---

# 26. Motion Artifacts

If movement occurs during acquisition:

```text
Patient Movement
       ↓
Image Blur
       ↓
Reduced Detail
```

---

# 27. Positioning

Correct positioning is important for consistent image acquisition.

```text
Correct Position
      ↓
Appropriate Anatomy Coverage
      ↓
Better Diagnostic Image
```

Poor positioning can reduce visualization of relevant tissue.

---

# 28. Mammography Image Quality

Important factors include:

```text
Image Quality
│
├── Spatial Resolution
├── Contrast
├── Noise
├── Sharpness
└── Artifact Level
```

---

# 29. DICOM in Mammography

Digital mammography images can be stored using DICOM.

A dataset may contain:

```text
Mammography DICOM
│
├── Pixel Data
├── Image Geometry
├── Acquisition Information
├── Detector Information
├── View Information
└── Processing Information
```

Correct handling of metadata is important for medical imaging software.

---

# 30. Mammography Processing Pipeline

```text
Patient
   ↓
Breast Positioning
   ↓
Compression
   ↓
X-Ray Exposure
   ↓
Digital Detector
   ↓
Raw Image
   ↓
Detector Correction
   ↓
Image Processing
   ↓
Visualization
   ↓
Clinical Workflow
```

---

# 31. Mammography Visualization

Mammography images may require:

```text
Raw Image
    ↓
Windowing
    ↓
Zoom
    ↓
Pan
    ↓
Contrast Adjustment
    ↓
Detailed Visualization
```

High-resolution display is important because mammography can involve subtle image details.

---

# 32. Mammography in Medical Image Processing

From a software perspective:

```text
DICOM Mammography
       ↓
Image Loading
       ↓
Pixel Processing
       ↓
Contrast Enhancement
       ↓
Noise Reduction
       ↓
Feature Extraction
       ↓
Segmentation / Detection
       ↓
Visualization
```

Important topics include:

* High-resolution image handling
* DICOM metadata
* Image processing
* Noise analysis
* Contrast enhancement
* Segmentation
* AI-based analysis

---

# 33. Mammography vs CT

| Mammography                            | CT                                |
| -------------------------------------- | --------------------------------- |
| Specialized breast X-ray imaging       | General tomographic X-ray imaging |
| Primarily projection-based             | 3D reconstructed volume           |
| High spatial detail for breast imaging | Cross-sectional anatomy           |
| Breast-focused workflow                | Broad anatomical imaging          |

---

# 34. Mammography vs Ultrasound

| Mammography             | Ultrasound                               |
| ----------------------- | ---------------------------------------- |
| X-ray-based             | Sound-wave-based                         |
| Uses ionizing radiation | No ionizing radiation                    |
| Projection imaging      | Real-time imaging possible               |
| Digital X-ray detector  | Ultrasound transducer                    |
| Breast imaging modality | Can provide complementary breast imaging |

---

# 35. Mammography vs MRI

| Mammography                | MRI                        |
| -------------------------- | -------------------------- |
| X-ray-based                | Magnetic resonance         |
| Projection imaging         | Volumetric imaging         |
| Specialized breast imaging | Strong soft-tissue imaging |
| Uses ionizing radiation    | No ionizing radiation      |

---

# 36. Important Terms

### Mammography

Specialized X-ray imaging of the breast.

### Compression

Controlled compression of breast tissue during image acquisition.

### CC View

Craniocaudal projection.

### MLO View

Mediolateral oblique projection.

### Digital Detector

Detector that converts X-ray information into digital image data.

### Calcification

Small mineralized deposits that may appear as bright structures.

### Breast Density

Relative composition of breast tissue affecting mammographic appearance.

### DBT

Digital Breast Tomosynthesis.

### CAD

Computer-Aided Detection.

---

# 37. Complete Mammography Workflow

```text
                  PATIENT
                     │
                     ▼
            BREAST POSITIONING
                     │
                     ▼
               COMPRESSION
                     │
                     ▼
               X-RAY SOURCE
                     │
                     ▼
               BREAST TISSUE
                     │
                     ▼
            X-RAY ATTENUATION
                     │
                     ▼
             DIGITAL DETECTOR
                     │
                     ▼
               RAW IMAGE DATA
                     │
                     ▼
             DETECTOR CORRECTION
                     │
                     ▼
              IMAGE PROCESSING
                     │
          ┌──────────┼──────────┐
          ▼          ▼          ▼
      CONTRAST     NOISE      DETAIL
     ENHANCEMENT  REDUCTION  PRESERVATION
                     │
                     ▼
              MAMMOGRAPHIC IMAGE
                     │
                     ▼
               VISUALIZATION
                     │
                     ▼
              CLINICAL WORKFLOW
```

---

# 38. Practice Questions

### Question 1: What is mammography?

**Answer:** A specialized X-ray imaging technique for breast imaging.

---

### Question 2: Why is compression used?

**Answer:** Compression reduces tissue thickness and overlap and helps improve image acquisition and visualization.

---

### Question 3: What are the common mammography views?

**Answer:**

* CC — Craniocaudal
* MLO — Mediolateral Oblique

---

### Question 4: Why is high spatial resolution important?

**Answer:** It helps visualize fine structures and small image details.

---

### Question 5: What is DBT?

**Answer:** Digital Breast Tomosynthesis, which uses multiple X-ray projections to reconstruct tomographic breast images.

---

### Question 6: What can affect mammography image quality?

**Answer:**

* Spatial resolution
* Contrast
* Noise
* Motion
* Positioning
* Artifacts

---

# 39. Chapter Summary

In **Chapter 39: Mammography**, you learned:

* Mammography fundamentals
* Breast X-ray imaging
* Mammography system components
* X-ray attenuation
* Breast compression
* Tissue overlap
* CC and MLO views
* Digital mammography
* Image resolution
* Pixel size
* Image contrast
* Calcifications
* Masses
* Breast density
* Image processing
* Contrast enhancement
* Noise reduction
* Segmentation
* Feature extraction
* Computer-aided detection
* AI in mammography
* Digital Breast Tomosynthesis
* Artifacts
* Positioning
* Image quality
* Mammography DICOM
* Visualization
* Medical image-processing workflow
* Comparison with CT, ultrasound, and MRI

---

## Progress

```text
LEVEL 1 — MEDICAL IMAGING FUNDAMENTALS

Module 4 — Medical Imaging Modalities

✅ Chapter 31: Introduction to Medical Imaging Modalities
✅ Chapter 32: X-Ray Imaging
✅ Chapter 33: Computed Tomography (CT)
✅ Chapter 34: Magnetic Resonance Imaging (MRI)
✅ Chapter 35: Positron Emission Tomography (PET)
✅ Chapter 36: Single Photon Emission Computed Tomography (SPECT)
✅ Chapter 37: Ultrasound Imaging
✅ Chapter 38: Cone Beam CT (CBCT)
✅ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 40 — Fluoroscopy**
