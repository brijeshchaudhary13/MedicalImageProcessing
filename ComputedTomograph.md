# Chapter 33: Computed Tomography (CT)

**Computed Tomography (CT)** is an X-ray-based medical imaging modality that acquires measurements from multiple angles around the patient and uses mathematical reconstruction algorithms to generate **cross-sectional images and 3D volumes**.

---

# 1. What Is CT?

The basic idea is:

```text
X-Ray Source + Detector
          ↓
     Rotate Around
        Patient
          ↓
Acquire Many Projections
          ↓
   Reconstruction
          ↓
      CT Images
          ↓
      3D Volume
```

Unlike conventional X-ray:

```text
Conventional X-Ray → Usually 2D Projection

CT → Cross-sectional Images + 3D Volume
```

---

# 2. Why Was CT Developed?

A conventional X-ray projects 3D anatomy onto a 2D detector.

```text
3D Anatomy
    ↓
2D Projection
```

This can cause structures to overlap.

CT solves this problem by acquiring projections from multiple angles.

```text
Projection 1
Projection 2
Projection 3
Projection 4
     ↓
Mathematical Reconstruction
     ↓
Cross-Sectional Image
```

---

# 3. Basic CT System

A simplified CT system contains:

```text
             GANTRY
     ┌─────────────────┐
     │   X-Ray Source  │
     │        ↓        │
     │     Patient     │
     │        ↓        │
     │    Detector     │
     └─────────────────┘
              ↓
        Reconstruction
              ↓
          CT Volume
```

Main components include:

* X-ray tube
* Detector system
* Gantry
* Patient table
* Data acquisition system
* Reconstruction computer

---

# 4. The CT Gantry

The **gantry** contains important CT imaging components.

```text
        ┌─────────────┐
        │ X-Ray Tube  │
        │      ↓      │
        │   PATIENT   │
        │      ↓      │
        │  DETECTOR   │
        └─────────────┘
```

During scanning, the X-ray source and detector system acquire measurements from many angles.

---

# 5. CT Data Acquisition

CT acquires multiple X-ray projections.

```text
              0°
               ●
          ↙         ↘
       45°             315°

          PATIENT

       135°            225°
          ↘         ↙
               ●
              180°
```

Conceptually:

```text
Multiple Angles
       ↓
Projection Data
       ↓
Reconstruction
       ↓
CT Image
```

---

# 6. What Is a Projection?

A projection is a set of measurements acquired through the patient from a particular direction.

```text
X-Ray Source
      ↓
   Patient
      ↓
   Detector
      ↓
 Projection Data
```

A CT scanner acquires many such projections.

---

# 7. CT Slice

A reconstructed cross-sectional image is commonly called a **CT slice**.

```text
        Patient
           │
     ───────────
       CT Slice
     ───────────
           │
```

A series of slices can form a volume.

```text
Slice 1
Slice 2
Slice 3
Slice 4
   ↓
CT Volume
```

---

# 8. CT Volume

A CT dataset is generally represented as a 3D volume:

$$
I(x,y,z)
$$

Where:

* \(x\) = horizontal position
* \(y\) = vertical position
* \(z\) = slice/depth direction

Each element is a:

$$
\boxed{Voxel}
$$

---

# 9. CT Image Matrix

A CT slice may have dimensions such as:

$$
512 \times 512
$$

Example:

```text
512 Pixels
    ×
512 Pixels
    ↓
CT Slice
```

A complete study might contain hundreds or thousands of slices depending on the acquisition.

---

# 10. Voxel Spacing

A voxel has physical dimensions.

Example:

$$
0.8mm \times 0.8mm \times 1.0mm
$$

Therefore, image measurements can be converted into physical measurements.

For example:

$$
Distance =
Number\ of\ Pixels
\times
Pixel\ Spacing
$$

---

# 11. X-Ray Attenuation in CT

CT is based on differences in X-ray attenuation.

The simplified attenuation equation is:

$$
I = I_0 e^{-\mu x}
$$

For CT, the X-ray beam passes through many different tissues.

Conceptually:

$$
I = I_0 e^{-\int \mu(s)ds}
$$

Where:

* \(\mu(s)\) represents spatially varying attenuation.

CT reconstruction attempts to estimate the attenuation distribution inside the body.

---

# 12. The Main CT Goal

The main mathematical goal can be written conceptually as:

```text
Measured Projections
        ↓
Estimate Internal Attenuation
        ↓
Reconstructed CT Image
```

Therefore:

$$
Projection\ Data
\rightarrow
Reconstruction
\rightarrow
\mu(x,y)
$$

---

# 13. Sinogram

CT projection data can be represented as a **sinogram**.

Conceptually:

```text
Projection Angle
        ↓
Detector Measurements
        ↓
      Sinogram
```

A sinogram contains projection measurements collected across different angles.

```text
Detector Position
       →
Angle
 ↓
██████████
██████████
██████████
██████████
```

Sinograms are important intermediate representations in CT reconstruction.

---

# 14. CT Reconstruction

The fundamental workflow is:

```text
Raw Projection Data
        ↓
Preprocessing
        ↓
Reconstruction Algorithm
        ↓
CT Slice / Volume
```

Two important categories are:

```text
CT Reconstruction
│
├── Analytical Reconstruction
│
└── Iterative Reconstruction
```

---

# 15. Back Projection

A basic reconstruction concept is **back projection**.

```text
Projection
    ↓
Spread Back Across Image Space
```

If projections from many angles are combined:

```text
Projection 1 ─┐
Projection 2 ─┤
Projection 3 ─┤
Projection N ─┘
       ↓
Back Projection
       ↓
Reconstructed Image
```

Simple back projection produces a blurred image.

---

# 16. Filtered Back Projection (FBP)

To improve reconstruction, projections can be filtered before back projection.

```text
Projection Data
      ↓
Filtering
      ↓
Back Projection
      ↓
Reconstructed CT Image
```

Mathematically:

$$
f(x,y)
=
\int_0^\pi
FilteredProjection(\theta)
d\theta
$$

Conceptually:

```text
Filter
  +
Back Projection
  ↓
Filtered Back Projection
```

---

# 17. Why Filtering Is Needed?

Without appropriate filtering:

```text
Back Projection
      ↓
Blurred Image
```

With filtering:

```text
Projection
      ↓
Filtering
      ↓
Back Projection
      ↓
Improved Reconstruction
```

---

# 18. Iterative Reconstruction

Iterative reconstruction uses repeated estimation and comparison.

```text
Initial Image Estimate
        ↓
Forward Projection
        ↓
Compare With Measured Data
        ↓
Calculate Difference
        ↓
Update Image
        ↓
Repeat
```

Conceptually:

$$
Image_{k+1}
=
Image_k
+
Correction
$$

---

# 19. Analytical vs Iterative Reconstruction

| Analytical Reconstruction                   | Iterative Reconstruction             |
| ------------------------------------------- | ------------------------------------ |
| Mathematical direct reconstruction approach | Repeated estimation approach         |
| Often computationally efficient             | Can require greater computation      |
| Example: FBP                                | Uses iterative updates               |
| Reconstruction based on projections         | Compares estimated and measured data |

---

# 20. Hounsfield Units (HU)

CT intensity is commonly expressed using **Hounsfield Units (HU)**.

The approximate formula is:

$$
HU =
1000
\times
\frac{\mu-\mu_{water}}
{\mu_{water}-\mu_{air}}
$$

Where:

* \(\mu\) = attenuation coefficient of material
* \(\mu_{water}\) = attenuation coefficient of water
* \(\mu_{air}\) = attenuation coefficient of air

Important reference values are approximately:

```text
Air       ≈ -1000 HU
Water     ≈     0 HU
```

Other values vary depending on material and acquisition conditions.

---

# 21. Why HU Is Important?

CT data contains a wide intensity range.

```text
Air
 ↓
-1000 HU

Water
 ↓
0 HU

Dense Materials
 ↓
Higher HU Values
```

HU values can support:

* Visualization
* Thresholding
* Segmentation
* Quantitative analysis
* Radiation therapy workflows

---

# 22. CT Windowing

A CT scanner can contain far more intensity information than a standard display can show simultaneously.

Therefore, **windowing** is used.

```text
Large HU Range
      ↓
Select Relevant Range
      ↓
Map to Display Range
      ↓
Visible Image
```

---

# 23. Window Width and Window Level

Two important concepts are:

### Window Width (WW)

The range of CT intensity values displayed.

### Window Level (WL)

The center of the selected intensity range.

Conceptually:

```text
              Window Width
       ├───────────────────┤

             ↑
             WL
```

The approximate displayed range is:

$$
Lower = WL - \frac{WW}{2}
$$

$$
Upper = WL + \frac{WW}{2}
$$

---

# 24. Example Window Calculation

Suppose:

$$
WL = 50
$$

$$
WW = 400
$$

Then:

$$
Lower = 50 - 200 = -150
$$

$$
Upper = 50 + 200 = 250
$$

Values below the lower limit are displayed toward one end of the grayscale range, while values above the upper limit are displayed toward the other end.

---

# 25. CT Windowing and Your DICOM Viewer

For your medical imaging software:

```text
CT DICOM File
      ↓
Pixel Data
      +
Rescale Information
      ↓
CT Intensity Values
      ↓
Window Width / Level
      ↓
Display Mapping
      ↓
Visible Image
```

This is a fundamental concept for CT visualization.

---

# 26. Axial View

The axial view represents a cross-sectional slice.

```text
Patient

   HEAD
     │
─────┼─────
 AXIAL SLICE
─────┼─────
     │
   FEET
```

---

# 27. Coronal View

The coronal plane divides the body conceptually into front and back portions.

```text
      FRONT
        │
   ─────────
   CORONAL
    PLANE
   ─────────
        │
       BACK
```

---

# 28. Sagittal View

The sagittal plane divides the body conceptually into left and right portions.

```text
LEFT │ RIGHT
     │
     │
SAGITTAL
 PLANE
```

---

# 29. Multiplanar Reconstruction (MPR)

A CT volume can be viewed in multiple planes.

```text
           CT Volume
               │
      ┌────────┼────────┐
      ▼        ▼        ▼
   Axial    Coronal  Sagittal
```

This is called:

$$
\boxed{Multiplanar\ Reconstruction}
$$

---

# 30. 3D Visualization

A CT volume can also be visualized in three dimensions.

```text
CT Slices
    ↓
3D Volume
    ↓
Volume Rendering
    ↓
3D Visualization
```

Common visualization approaches include:

* Slice visualization
* MPR
* Maximum intensity projection
* Volume rendering

---

# 31. Maximum Intensity Projection (MIP)

MIP displays the maximum intensity value encountered along a viewing direction.

Conceptually:

$$
MIP(x,y)
=
\max_z I(x,y,z)
$$

```text
3D Volume
    ↓
View Direction
    ↓
Maximum Intensity
    ↓
2D Projection
```

---

# 32. CT Image Processing

CT image processing may involve:

```text
CT Volume
│
├── Windowing
├── Filtering
├── Segmentation
├── Registration
├── Measurement
├── MPR
└── 3D Visualization
```

---

# 33. CT Noise

CT images contain noise.

Conceptually:

$$
Measured\ Image
=
True\ Image
+
Noise
$$

Noise can affect:

* Visualization
* Low-contrast detail
* Segmentation
* Quantitative analysis

---

# 34. CT Artifacts

Common artifact categories include:

```text
CT Artifacts
│
├── Motion
├── Metal
├── Beam Hardening
├── Partial Volume
└── Reconstruction Effects
```

Artifacts are important because they can affect image quality and processing algorithms.

---

# 35. Motion Artifacts

Patient motion during acquisition can affect reconstruction.

```text
Patient Movement
       ↓
Inconsistent Measurements
       ↓
Reconstruction Artifact
```

---

# 36. Metal Artifacts

Highly attenuating materials can affect projection data.

```text
Metal
   ↓
Strong Attenuation
   ↓
Projection Effects
   ↓
Artifacts
```

---

# 37. Partial Volume Effect

A voxel may contain more than one tissue type.

```text
Voxel
├── Tissue A
└── Tissue B
```

The resulting voxel value can represent a combination of contributions.

This can affect:

* Boundary visualization
* Segmentation
* Quantitative measurements

---

# 38. CT Spatial Resolution

Spatial resolution determines how well small structures can be distinguished.

Factors can include:

* Detector characteristics
* Reconstruction parameters
* Acquisition geometry
* Patient motion

---

# 39. CT Contrast Resolution

CT can distinguish tissues based on differences in X-ray attenuation.

```text
Tissue A
   ↓
HU A

Tissue B
   ↓
HU B
```

The ability to distinguish tissues also depends on noise and other image-quality factors.

---

# 40. CT Dose Considerations

CT uses ionizing X-rays.

Conceptually:

```text
Image Quality
      ↔
Radiation Exposure Considerations
```

Acquisition protocols are designed according to clinical purpose and applicable safety practices.

---

# 41. CT Applications

CT is used in many medical applications.

```text
CT Applications
│
├── Head Imaging
├── Chest Imaging
├── Abdominal Imaging
├── Trauma Imaging
├── Vascular Imaging
├── Oncology
├── Surgical Planning
└── Radiation Therapy
```

---

# 42. CT in Radiation Therapy

This is particularly relevant to your medical software and TPS learning.

```text
Patient
   ↓
CT Simulation
   ↓
CT Volume
   ↓
Structure Delineation
   ↓
Treatment Planning
   ↓
Dose Calculation
   ↓
Plan Evaluation
```

CT provides important anatomical and density-related information used in treatment-planning workflows.

---

# 43. CT and Dose Calculation

Conceptually:

```text
CT Image
    ↓
Image Intensity Information
    ↓
Material / Density-Related Modeling
    ↓
Dose Calculation
```

The exact conversion and modeling depend on the treatment-planning system and clinical configuration.

---

# 44. CT Image Registration

CT may be registered with other modalities.

```text
CT
+
MRI
 ↓
Registration
 ↓
Aligned Images
```

or:

```text
CT
+
PET
 ↓
Registration
 ↓
Combined Information
```

---

# 45. CT DICOM Data

A CT study commonly contains:

```text
CT DICOM Study
      │
      ├── Multiple Slices
      ├── Pixel Data
      ├── Spatial Information
      ├── Image Orientation
      ├── Image Position
      ├── Pixel Spacing
      └── CT-Specific Metadata
```

Correct ordering and spatial interpretation are essential when constructing a CT volume.

---

# 46. CT Software Pipeline

A simplified medical software pipeline:

```text
CT DICOM Files
       ↓
Read Metadata
       ↓
Sort Slices
       ↓
Load Pixel Data
       ↓
Construct 3D Volume
       ↓
Apply Rescale Information
       ↓
Window / Level
       ↓
MPR / Visualization
       ↓
Image Processing
```

---

# 47. CT Volume Memory Example

Suppose a CT volume is:

$$
512 \times 512 \times 500
$$

Number of voxels:

$$
512 \times 512 \times 500
=
131,072,000
$$

If each voxel uses 16 bits:

$$
16\ bits = 2\ bytes
$$

Approximate raw memory:

$$
131,072,000 \times 2
=
262,144,000\ bytes
$$

Approximately:

$$
250\ MiB
$$

This is before considering additional copies, processing buffers, metadata, or GPU memory.

---

# 48. CT vs Conventional X-Ray

| Feature           | X-Ray                         | CT                              |
| ----------------- | ----------------------------- | ------------------------------- |
| Data acquisition  | Projection                    | Multiple projections            |
| Output            | Usually 2D                    | Cross-sectional images/volume   |
| Structure overlap | Common                        | Reduced in reconstructed slices |
| Reconstruction    | Limited projection processing | Major reconstruction component  |
| Data size         | Usually smaller               | Often much larger               |

---

# 49. CT vs MRI

| CT                                                    | MRI                                   |
| ----------------------------------------------------- | ------------------------------------- |
| X-ray based                                           | Magnetic resonance based              |
| Uses ionizing radiation                               | No ionizing X-rays                    |
| Attenuation-based image values                        | Signal depends on MR acquisition      |
| Often fast acquisition                                | Acquisition characteristics vary      |
| Strong role in many structural and planning workflows | Strong soft-tissue imaging capability |

---

# 50. Important Terms

### Projection

Measurement acquired from a particular angle.

### Sinogram

Representation of projection measurements across detector positions and angles.

### Reconstruction

Process of generating an image from projection data.

### CT Slice

Cross-sectional reconstructed image.

### Voxel

3D image element.

### Hounsfield Unit (HU)

Standardized CT intensity scale based on X-ray attenuation relative to reference materials.

### Window Width

Range of intensity values selected for display.

### Window Level

Center of the displayed intensity range.

### MPR

Multiplanar reconstruction.

### MIP

Maximum intensity projection.

### Artifact

Unwanted image feature caused by acquisition, reconstruction, or other effects.

---

# 51. Complete CT Workflow

```text
                    PATIENT
                       │
                       ▼
                  CT SCANNER
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
        X-RAY SOURCE         DETECTORS
             │                   ▲
             └──────┐     ┌──────┘
                    ▼     │
                 PATIENT
                    │
                    ▼
            MULTIPLE PROJECTIONS
                    │
                    ▼
                 SINOGRAM
                    │
                    ▼
            RECONSTRUCTION
          ┌─────────┴─────────┐
          ▼                   ▼
        FBP             ITERATIVE
          │                   │
          └─────────┬─────────┘
                    ▼
                CT VOLUME
                    │
       ┌────────────┼────────────┐
       ▼            ▼            ▼
     AXIAL       CORONAL      SAGITTAL
                    │
                    ▼
             WINDOW / LEVEL
                    │
                    ▼
              VISUALIZATION
                    │
                    ▼
            CLINICAL WORKFLOW
```

---

# 52. Practice Questions

### Question 1: What is the main difference between CT and conventional X-ray?

**Answer:** Conventional X-ray usually produces a 2D projection, while CT acquires measurements from multiple angles and reconstructs cross-sectional images and 3D volumes.

---

### Question 2: What is a CT voxel?

**Answer:** A voxel is a three-dimensional image element representing information at a location in a CT volume.

---

### Question 3: What is a sinogram?

**Answer:** A sinogram is a representation of CT projection measurements collected across detector positions and projection angles.

---

### Question 4: What is Filtered Back Projection?

**Answer:** It is a CT reconstruction approach in which projection data is filtered before being back-projected into image space.

---

### Question 5: What are Hounsfield Units?

**Answer:** HU are standardized CT intensity values related to X-ray attenuation relative to reference materials such as water and air.

---

### Question 6: What is Window Width?

**Answer:** Window Width defines the range of CT intensity values selected for display.

---

### Question 7: What is Window Level?

**Answer:** Window Level defines the center of the selected display intensity range.

---

### Question 8: Why is CT important in radiation therapy?

**Answer:** CT provides anatomical and density-related information used in simulation, structure delineation, treatment planning, and dose-calculation workflows.

---

# 53. Chapter Summary

In **Chapter 33: Computed Tomography (CT)**, you learned:

* CT fundamentals
* Why CT was developed
* CT system components
* Gantry
* CT data acquisition
* Projections
* CT slices
* CT volumes
* Voxels and voxel spacing
* X-ray attenuation
* Sinograms
* CT reconstruction
* Back projection
* Filtered Back Projection
* Iterative reconstruction
* Hounsfield Units
* Window Width and Window Level
* Axial, coronal, and sagittal views
* Multiplanar reconstruction
* 3D visualization
* Maximum Intensity Projection
* CT noise
* CT artifacts
* Motion artifacts
* Metal artifacts
* Partial volume effect
* Spatial and contrast resolution
* Radiation considerations
* CT applications
* CT in radiation therapy
* CT and dose calculation
* CT registration
* CT DICOM data
* CT software pipeline
* CT memory requirements

---

## Progress

```text
LEVEL 1 — MEDICAL IMAGING FUNDAMENTALS

Module 4 — Medical Imaging Modalities

✅ Chapter 31: Introduction to Medical Imaging Modalities
✅ Chapter 32: X-Ray Imaging
✅ Chapter 33: Computed Tomography (CT)
⬜ Chapter 34: Magnetic Resonance Imaging (MRI)
⬜ Chapter 35: Positron Emission Tomography (PET)
⬜ Chapter 36: Single Photon Emission Computed Tomography (SPECT)
⬜ Chapter 37: Ultrasound Imaging
⬜ Chapter 38: Cone Beam CT (CBCT)
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 34 — Magnetic Resonance Imaging (MRI)**
