# Chapter 38: Cone Beam Computed Tomography (CBCT)

**Cone Beam Computed Tomography (CBCT)** is an X-ray imaging modality that uses a **cone-shaped X-ray beam** and a flat-panel detector to acquire projection images around the patient and reconstruct a **3D volumetric image**.

CBCT is especially important in:

* Dentistry
* Oral and maxillofacial imaging
* Orthopedics
* Image-guided radiation therapy (IGRT)
* Radiation treatment positioning

---

# 1. Basic CBCT Concept

```text
Cone-Shaped X-Ray Beam
        ↓
      Patient
        ↓
Flat Panel Detector
        ↓
Multiple Projections
        ↓
Image Reconstruction
        ↓
3D CBCT Volume
```

Unlike conventional CT, CBCT typically uses a cone-shaped beam and acquires a large number of 2D projections during gantry rotation.

---

# 2. Basic CBCT System

A simplified CBCT system contains:

```text
CBCT System
│
├── X-Ray Source
├── Cone-Shaped X-Ray Beam
├── Patient
├── Flat Panel Detector
├── Rotating Gantry
└── Reconstruction System
```

Conceptually:

```text
       X-Ray Source
            ●
           /|\
          / | \
         /  |  \
        /   |   \
       PATIENT
          │
          ▼
   Flat Panel Detector
```

---

# 3. Why Is It Called Cone Beam CT?

The X-ray beam spreads in multiple directions and forms a cone-like geometry.

```text
        X-Ray Source
             ●
           /   \
         /       \
       /           \
     /               \
       PATIENT
            ↓
      Flat Detector
```

Therefore:

$$
\boxed{Cone\ Beam\ Computed\ Tomography}
$$

---

# 4. CBCT vs Conventional CT

```text
Conventional CT
      ↓
Fan-Shaped Beam
      ↓
Detector Array
```

```text
CBCT
      ↓
Cone-Shaped Beam
      ↓
Flat Panel Detector
```

Both can reconstruct volumetric images, but their acquisition geometry differs.

---

# 5. CBCT Image Acquisition

During acquisition:

```text
X-Ray Source + Detector
          ↓
      Rotation
          ↓
      Patient
          ↓
Multiple 2D Projections
```

Conceptually:

```text
        Projection 1
             ↓

Projection 2 ← PATIENT → Projection 3

             ↑
        Projection N
```

Many projections are collected from different angles.

---

# 6. Projection Images

Each CBCT projection is a 2D X-ray image.

```text
3D Patient
    ↓
X-Ray Projection
    ↓
2D Detector Image
```

Multiple projections are required for reconstruction.

```text
Projection 1
Projection 2
Projection 3
     ↓
Projection N
     ↓
Reconstruction
     ↓
3D Volume
```

---

# 7. CBCT Reconstruction

The reconstruction process converts projection data into a 3D volume.

```text
2D Projections
      ↓
Preprocessing
      ↓
Geometric Corrections
      ↓
Reconstruction
      ↓
3D CBCT Volume
```

A commonly discussed reconstruction approach for CBCT is based on cone-beam reconstruction algorithms such as:

$$
\boxed{FDK\ Algorithm}
$$

---

# 8. FDK Reconstruction

**FDK** stands for the **Feldkamp-Davis-Kress algorithm**.

Conceptually:

```text
Projection Data
      ↓
Filtering
      ↓
Cone Beam Backprojection
      ↓
3D CBCT Volume
```

It is an important classical reconstruction approach in CBCT.

---

# 9. CBCT Volume

After reconstruction:

$$
I(x,y,z)
$$

represents a 3D image volume.

Each element is called a:

$$
\boxed{Voxel}
$$

```text
CBCT Volume
│
├── Axial Slices
├── Coronal Slices
└── Sagittal Slices
```

---

# 10. Voxel Intensity

Voxel intensity represents reconstructed X-ray attenuation-related information.

Conceptually:

```text
Higher Attenuation
       ↓
Higher Reconstructed Signal

Lower Attenuation
       ↓
Lower Reconstructed Signal
```

However, CBCT intensity values may not have the same quantitative consistency as conventional CT values.

---

# 11. CBCT Geometry

CBCT reconstruction depends heavily on acquisition geometry.

Important components include:

```text
CBCT Geometry
│
├── Source Position
├── Detector Position
├── Source-to-Patient Distance
├── Source-to-Detector Distance
├── Rotation Angle
└── Projection Geometry
```

Correct geometry is essential for accurate reconstruction.

---

# 12. Gantry Rotation

The X-ray source and detector rotate around the patient.

```text
        ● Source
           \
            \
          PATIENT
            /
           /
        Detector
```

During rotation:

```text
Angle 1
   ↓
Angle 2
   ↓
Angle 3
   ↓
...
   ↓
Angle N
```

Projection images are acquired at different angles.

---

# 13. Field of View (FOV)

The **Field of View (FOV)** represents the anatomical region included in the scan.

```text
Large FOV
   ↓
More Anatomy
```

```text
Small FOV
   ↓
Smaller Anatomical Region
```

FOV selection is important for imaging workflow and image quality.

---

# 14. Spatial Resolution

CBCT can provide high spatial resolution.

Resolution depends on factors such as:

* Detector characteristics
* Pixel size
* Voxel size
* Acquisition geometry
* Number of projections
* Reconstruction method
* Patient motion

Conceptually:

```text
Smaller Voxel
      ↓
Potentially More Spatial Detail
```

---

# 15. Voxel Size

CBCT datasets may use small voxels.

```text
Large Voxel
┌─────┐
│     │
└─────┘
```

```text
Small Voxel
┌──┐
│  │
└──┘
```

Smaller voxels can improve spatial detail but may increase data size and influence noise characteristics.

---

# 16. CBCT Image Noise

CBCT images can contain noise.

```text
Measured Image
       =
True Signal
       +
Noise
```

Noise can influence:

* Visualization
* Segmentation
* Registration
* Image analysis

---

# 17. Scatter in CBCT

Because CBCT uses cone-beam geometry, scattered radiation can significantly affect image quality.

```text
X-Ray
  ↓
Patient
  ↓
Scatter
 ↙   ↘
Detector
```

Scatter can contribute to:

* Reduced contrast
* Image artifacts
* Intensity inaccuracies

---

# 18. Beam Hardening

X-ray beams contain photons with different energies.

As X-rays pass through matter:

```text
X-Ray Beam
     ↓
Patient
     ↓
Low-Energy Photons Reduced
     ↓
Changed Beam Spectrum
```

This phenomenon is associated with:

$$
\boxed{Beam\ Hardening}
$$

It can contribute to image artifacts.

---

# 19. Motion Artifacts

Patient movement during acquisition can produce artifacts.

```text
Projection 1 → Position A

Projection 2 → Position B

Projection 3 → Position C
```

If the anatomy moves:

```text
Inconsistent Projections
        ↓
Reconstruction Errors
        ↓
Motion Artifacts
```

---

# 20. Metal Artifacts

Metal objects can strongly affect X-ray imaging.

Examples include:

* Dental fillings
* Implants
* Surgical hardware

Conceptually:

```text
X-Ray
  ↓
METAL
  ↓
Strong Attenuation
  ↓
Artifact
```

Metal artifacts can reduce image quality.

---

# 21. Common CBCT Artifacts

```text
CBCT Artifacts
│
├── Scatter
├── Beam Hardening
├── Motion
├── Metal Artifacts
├── Ring Artifacts
└── Reconstruction Artifacts
```

---

# 22. CBCT Reconstruction Pipeline

```text
X-Ray Acquisition
       ↓
2D Projections
       ↓
Detector Corrections
       ↓
Geometry Corrections
       ↓
Filtering
       ↓
Reconstruction
       ↓
3D CBCT Volume
       ↓
Image Processing
       ↓
Visualization
```

---

# 23. CBCT Image Processing

After reconstruction, common processing tasks include:

```text
CBCT Volume
     ↓
Noise Reduction
     ↓
Registration
     ↓
Segmentation
     ↓
Visualization
     ↓
Clinical Analysis
```

---

# 24. CBCT Image Registration

CBCT is often registered with another imaging dataset.

For example:

```text
Planning CT
      +
CBCT
      ↓
Image Registration
      ↓
Aligned Images
```

Registration may involve:

* Translation
* Rotation
* Rigid registration
* Deformable registration

---

# 25. CBCT in Radiation Therapy

This is particularly important for your medical imaging and TPS learning.

CBCT is commonly used in **Image-Guided Radiation Therapy (IGRT)**.

```text
Planning CT
     ↓
Treatment Plan
     ↓
Patient Setup
     ↓
CBCT Acquisition
     ↓
Registration With Planning CT
     ↓
Position Verification
     ↓
Treatment
```

---

# 26. Planning CT vs Treatment CBCT

Conceptually:

```text
Planning CT
      ↓
Reference Anatomy
      ↓
Treatment Planning
```

```text
CBCT
      ↓
Current Patient Position
      ↓
Position Verification
```

The images can be registered to determine positioning differences.

---

# 27. Image Registration Workflow

```text
Planning CT
     +
Daily CBCT
     ↓
Registration Algorithm
     ↓
Transformation
     ↓
Alignment
     ↓
Position Difference
```

This may help determine patient setup corrections.

---

# 28. CBCT and Adaptive Radiotherapy

CBCT can also contribute to adaptive radiation therapy workflows.

```text
Planning CT
      ↓
Original Treatment Plan
      ↓
CBCT
      ↓
Anatomical Changes
      ↓
Evaluation
      ↓
Possible Adaptive Workflow
```

---

# 29. CBCT in Dentistry

CBCT is widely used in dental imaging.

Applications can include:

```text
Dental CBCT
│
├── Teeth
├── Jaw
├── Implant Planning
├── Orthodontic Assessment
└── Maxillofacial Imaging
```

---

# 30. CBCT in Orthopedics

CBCT may also be used for certain orthopedic imaging applications.

```text
Bone Structure
      ↓
CBCT Acquisition
      ↓
3D Volume
      ↓
Visualization
```

---

# 31. CBCT Visualization

CBCT volumes are commonly displayed using:

```text
        CBCT Volume
             │
      ┌──────┼──────┐
      ▼      ▼      ▼
   Axial  Coronal Sagittal
```

This is often called:

$$
\boxed{Multiplanar\ Reconstruction\ (MPR)}
$$

---

# 32. MPR Visualization

```text
          3D Volume
             │
   ┌─────────┼─────────┐
   ▼         ▼         ▼
 Axial    Coronal   Sagittal
```

MPR is important in medical imaging applications because it allows visualization from multiple anatomical planes.

---

# 33. CBCT Windowing

CBCT images can use windowing to improve visualization.

```text
Raw Intensity
      ↓
Window / Level
      ↓
Display Intensity
```

Conceptually:

```text
Window Width
      ↓
Displayed Range
```

```text
Window Level
      ↓
Center of Display Range
```

---

# 34. CBCT Segmentation

CBCT image processing may involve segmentation.

```text
CBCT Volume
      ↓
Preprocessing
      ↓
Feature Extraction
      ↓
Segmentation
      ↓
Anatomical Structures
```

Possible structures include:

* Bone
* Teeth
* Anatomical regions

Segmentation can be challenging because of noise and artifacts.

---

# 35. CBCT DICOM Data

CBCT datasets may be stored using medical imaging formats and metadata.

Conceptually:

```text
CBCT Dataset
│
├── Pixel Data
├── Image Position
├── Image Orientation
├── Slice Geometry
├── Acquisition Information
└── Reconstruction Information
```

Correct geometry handling is essential.

---

# 36. CBCT Processing Pipeline

```text
Patient
   ↓
Cone-Beam X-Ray Acquisition
   ↓
Multiple 2D Projections
   ↓
Detector / Geometry Corrections
   ↓
Reconstruction
   ↓
CBCT Volume
   ↓
DICOM / Medical Image Data
   ↓
Registration
   ↓
Segmentation
   ↓
Visualization
   ↓
Clinical Workflow
```

---

# 37. CBCT vs Conventional CT

| CBCT                                         | Conventional CT                     |
| -------------------------------------------- | ----------------------------------- |
| Cone-shaped X-ray beam                       | Typically fan-shaped X-ray geometry |
| Flat-panel detector common                   | Detector array                      |
| Many 2D projections                          | CT projection acquisition           |
| High spatial detail for certain applications | Strong quantitative consistency     |
| Common in dentistry and IGRT                 | Broad diagnostic imaging use        |

---

# 38. CBCT vs X-Ray

| CBCT                       | X-Ray                          |
| -------------------------- | ------------------------------ |
| 3D volumetric imaging      | 2D projection                  |
| Multiple projections       | Usually single/few projections |
| Reconstruction required    | Direct projection image        |
| Provides depth information | Structures can overlap         |

---

# 39. CBCT vs MRI

| CBCT                                             | MRI                              |
| ------------------------------------------------ | -------------------------------- |
| X-ray-based                                      | Magnetic resonance               |
| Strong for high-contrast structures such as bone | Strong soft-tissue contrast      |
| Uses ionizing radiation                          | No ionizing radiation            |
| Fast volumetric acquisition workflows possible   | Different acquisition principles |

---

# 40. Important Terms

### Cone Beam

An X-ray beam that diverges in a cone-shaped geometry.

### Projection

A 2D X-ray image acquired from a particular angle.

### Flat Panel Detector

A detector used to capture X-ray projection images.

### FDK Algorithm

A classical cone-beam reconstruction algorithm.

### Voxel

A three-dimensional image element.

### Field of View (FOV)

The anatomical region included in the scan.

### Beam Hardening

Change in X-ray beam energy spectrum after passing through matter.

### Scatter

X-ray photons changing direction after interactions.

### Image Registration

Spatial alignment of two or more image datasets.

### MPR

Multiplanar reconstruction of a 3D volume.

---

# 41. Complete CBCT Workflow

```text
                  X-RAY SOURCE
                       │
                       ▼
                 CONE BEAM
                       │
                       ▼
                    PATIENT
                       │
                       ▼
              FLAT PANEL DETECTOR
                       │
                       ▼
                2D PROJECTION
                       │
                       ▼
             GANTRY ROTATION
                       │
                       ▼
            MULTIPLE PROJECTIONS
                       │
                       ▼
             DETECTOR CORRECTIONS
                       │
                       ▼
             GEOMETRY CORRECTIONS
                       │
                       ▼
                RECONSTRUCTION
                       │
                       ▼
                  CBCT VOLUME
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       AXIAL       CORONAL      SAGITTAL
                       │
                       ▼
                 REGISTRATION
                       │
                       ▼
             PLANNING CT + CBCT
                       │
                       ▼
             PATIENT POSITIONING
                       │
                       ▼
              CLINICAL WORKFLOW
```

---

# 42. Practice Questions

### Question 1: What does CBCT stand for?

**Answer:** Cone Beam Computed Tomography.

---

### Question 2: What type of beam does CBCT use?

**Answer:** A cone-shaped X-ray beam.

---

### Question 3: What does CBCT acquire?

**Answer:** Multiple 2D X-ray projection images from different angles.

---

### Question 4: What is reconstructed from the projections?

**Answer:** A 3D volumetric CBCT image.

---

### Question 5: What is the FDK algorithm?

**Answer:** Feldkamp-Davis-Kress, a classical reconstruction algorithm for cone-beam CT.

---

### Question 6: Why is CBCT important in radiation therapy?

**Answer:** It is commonly used in image-guided radiation therapy to compare the patient's current position and anatomy with planning images.

---

### Question 7: What is MPR?

**Answer:** Multiplanar Reconstruction, used to view a 3D volume in axial, coronal, and sagittal planes.

---

# 43. Chapter Summary

In **Chapter 38: Cone Beam Computed Tomography (CBCT)**, you learned:

* CBCT fundamentals
* Cone-beam geometry
* X-ray source
* Flat-panel detectors
* Projection acquisition
* Gantry rotation
* 3D reconstruction
* FDK algorithm
* CBCT volumes and voxels
* CBCT geometry
* Field of View
* Spatial resolution
* Voxel size
* Image noise
* Scatter
* Beam hardening
* Motion artifacts
* Metal artifacts
* CBCT reconstruction pipeline
* Image processing
* Image registration
* CBCT in radiation therapy
* Image-guided radiation therapy
* Planning CT vs CBCT
* Adaptive radiotherapy
* Dental CBCT
* Orthopedic applications
* MPR visualization
* Windowing
* Segmentation
* DICOM data
* CBCT comparisons with CT, X-ray, and MRI

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
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 39 — Mammography**
