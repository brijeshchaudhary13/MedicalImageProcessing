# Chapter 34: Magnetic Resonance Imaging (MRI)

**Magnetic Resonance Imaging (MRI)** is a medical imaging modality that uses **strong magnetic fields, radiofrequency (RF) pulses, and magnetic resonance signals** to create detailed images of the body.

Unlike CT and X-ray:

```text
X-Ray / CT → Uses X-rays

MRI → Uses Magnetic Fields + RF Signals
```

MRI does **not use ionizing X-rays**.

---

# 1. Basic MRI Concept

The simplified MRI process is:

```text
Patient
   ↓
Strong Magnetic Field
   ↓
Hydrogen Nuclei Alignment
   ↓
RF Excitation
   ↓
RF Signal Measurement
   ↓
Image Reconstruction
   ↓
MRI Image
```

---

# 2. Why MRI Is Important?

MRI provides excellent visualization of many soft tissues.

Common applications include:

* Brain imaging
* Spinal imaging
* Musculoskeletal imaging
* Cardiac imaging
* Abdominal imaging
* Pelvic imaging
* Tumor evaluation

MRI is especially important when differences between soft tissues need to be visualized.

---

# 3. Basic MRI Scanner

A simplified MRI system:

```text
              MRI SYSTEM
                   │
      ┌────────────┼────────────┐
      ▼            ▼            ▼
Main Magnet   Gradient System   RF System
      │            │            │
      └────────────┼────────────┘
                   ▼
                Patient
                   │
                   ▼
              MR Signals
                   │
                   ▼
            Reconstruction
                   │
                   ▼
              MRI Image
```

---

# 4. Main MRI Components

Important components include:

```text
MRI System
│
├── Main Magnet
├── Gradient Coils
├── RF Transmitter
├── RF Receiver
├── Computer System
└── Patient Table
```

---

# 5. The Main Magnetic Field

The MRI scanner produces a strong static magnetic field.

This field is commonly represented as:

$$
B_0
$$

Conceptually:

```text
Without Strong Magnetic Field
Random Nuclear Orientation
```

```text
With Strong Magnetic Field B₀
Nuclei Tend to Align
```

---

# 6. Hydrogen and MRI

MRI commonly relies heavily on signals from hydrogen nuclei.

Why hydrogen?

```text
Human Body
│
├── Water
└── Fat
     ↓
Large Amount of Hydrogen
```

Because the human body contains abundant water and fat, hydrogen is useful for MRI signal generation.

---

# 7. Nuclear Spin

Certain atomic nuclei have a property called **spin**.

Conceptually:

```text
Nucleus
   ↓
Magnetic Property
   ↓
Behaves Like a Tiny Magnet
```

Hydrogen nuclei are particularly important in clinical MRI.

---

# 8. Alignment in a Magnetic Field

Without an external magnetic field:

```text
↑   ↙   →   ↓   ↗
Random Orientation
```

When placed inside the MRI magnetic field:

```text
        B₀
        ↑

   ↑   ↑   ↑   ↑
   ↑   ↑   ↑   ↑
```

There is a net magnetization aligned with the main magnetic field.

---

# 9. Net Magnetization

Individual magnetic moments do not all behave identically.

However, the combined effect produces:

$$
\boxed{Net\ Magnetization}
$$

Conceptually:

```text
Many Small Magnetic Moments
            ↓
      Net Magnetization
```

MRI detects signals associated with this magnetization.

---

# 10. Precession

Hydrogen nuclei do not simply point in one direction.

They also undergo a motion called **precession**.

Conceptually:

```text
      B₀
      ↑
      │
      │
     ↺
   Precession
```

The precession frequency is related to the magnetic field strength.

---

# 11. Larmor Frequency

The precession frequency is described by the **Larmor equation**:

$$
\omega_0 = \gamma B_0
$$

Where:

* \(\omega_0\) = angular frequency
* \(\gamma\) = gyromagnetic ratio
* \(B_0\) = magnetic field strength

This relationship is fundamental to MRI.

---

# 12. Radiofrequency Excitation

MRI uses radiofrequency energy to excite the system.

```text
Net Magnetization
       │
       ▼
   RF Pulse
       │
       ▼
Magnetization Changes
       │
       ▼
MR Signal Generated
```

The RF excitation is applied near the appropriate resonance frequency.

---

# 13. Resonance

MRI depends on the concept of **magnetic resonance**.

Simplified:

```text
Correct RF Frequency
        ↓
Efficient Energy Transfer
        ↓
Magnetic Resonance
```

The resonance frequency depends on the magnetic field and the nucleus being observed.

---

# 14. Longitudinal Magnetization

Before RF excitation, the net magnetization is primarily aligned with:

$$
B_0
$$

This direction is commonly called the:

$$
\boxed{Longitudinal\ Direction}
$$

---

# 15. Transverse Magnetization

After RF excitation, magnetization can develop a component perpendicular to \(B_0\).

```text
        B₀
        ↑
        │
        │
        ● ───→ Transverse
```

The transverse component is important for detecting MR signals.

---

# 16. MRI Signal Detection

After excitation:

```text
RF Excitation
      ↓
Magnetization Response
      ↓
Changing Magnetic Field
      ↓
Induced Electrical Signal
      ↓
Receiver Coil
```

The MRI system measures these signals.

---

# 17. Relaxation

After RF excitation, the magnetization gradually returns toward equilibrium.

This process is called:

$$
\boxed{Relaxation}
$$

Two major relaxation concepts are:

```text
Relaxation
│
├── T1 Relaxation
│
└── T2 Relaxation
```

---

# 18. T1 Relaxation

T1 relaxation is commonly associated with recovery of longitudinal magnetization.

```text
After RF Pulse
     ↓
Reduced Longitudinal Magnetization
     ↓
Recovery
     ↓
Equilibrium
```

T1 is often called:

```text
Longitudinal Relaxation
```

---

# 19. T2 Relaxation

T2 relaxation is associated with loss of coherence of transverse magnetization.

```text
Initially
↑ ↑ ↑ ↑ ↑
Signals More Coherent

Over Time
↗ ↓ ↖ → ↙
Dephasing
```

This causes the measurable transverse signal to decrease.

---

# 20. T1 vs T2

| T1                                            | T2                                |
| --------------------------------------------- | --------------------------------- |
| Longitudinal relaxation                       | Transverse relaxation             |
| Recovery toward equilibrium                   | Loss of transverse coherence      |
| Describes longitudinal magnetization recovery | Describes transverse signal decay |

Different tissues have different relaxation properties.

---

# 21. MRI Contrast

MRI contrast depends on several factors.

```text
MRI Contrast
│
├── T1 Properties
├── T2 Properties
├── Proton Density
├── Sequence Parameters
└── Other Acquisition Factors
```

This allows MRI to generate many different types of tissue contrast.

---

# 22. Proton Density

**Proton density** is related to the amount of MR-visible hydrogen nuclei within a region.

Conceptually:

```text
More Relevant Hydrogen
        ↓
Potentially More MR Signal
```

The final image appearance also depends on the sequence and acquisition parameters.

---

# 23. MRI Pulse Sequences

A **pulse sequence** defines how MRI signals are generated and acquired.

Conceptually:

```text
RF Pulses
   +
Gradients
   +
Timing
   ↓
Pulse Sequence
   ↓
Specific Image Contrast
```

Examples include:

* T1-weighted imaging
* T2-weighted imaging
* Proton-density-weighted imaging

---

# 24. T1-Weighted Imaging

A T1-weighted image emphasizes differences related to T1 relaxation.

```text
T1 Properties
      ↓
Different Tissue Response
      ↓
T1-Weighted Image
```

The appearance depends on tissue characteristics and sequence parameters.

---

# 25. T2-Weighted Imaging

A T2-weighted image emphasizes differences related to T2 relaxation.

```text
T2 Properties
      ↓
Different Signal Decay
      ↓
T2-Weighted Image
```

---

# 26. MRI Spatial Encoding

MRI must determine **where signals originate**.

This is achieved using gradient magnetic fields.

```text
MR Signal
    ↓
Spatial Encoding
    ↓
Position Information
    ↓
Image Reconstruction
```

---

# 27. Gradient Coils

Gradient coils create controlled spatial variations in the magnetic field.

```text
Magnetic Field
      ↓
Spatial Variation
      ↓
Position Encoding
```

Important spatial encoding concepts include:

* Slice selection
* Frequency encoding
* Phase encoding

---

# 28. Slice Selection

MRI can select a specific imaging region.

```text
Patient Volume
      │
──────┼──────
 Selected Slice
──────┼──────
```

This allows acquisition of images from selected anatomical locations.

---

# 29. Frequency Encoding

Different spatial positions can be associated with different signal frequencies.

Conceptually:

```text
Position A → Frequency A
Position B → Frequency B
Position C → Frequency C
```

---

# 30. Phase Encoding

Spatial information can also be encoded through phase differences.

Conceptually:

```text
Position A → Phase A
Position B → Phase B
Position C → Phase C
```

Frequency and phase encoding work together to provide spatial localization.

---

# 31. K-Space

Raw MRI data is commonly represented in a domain called:

$$
\boxed{k-space}
$$

Important concept:

```text
MRI Scanner
     ↓
MR Signal Acquisition
     ↓
K-Space
     ↓
Mathematical Reconstruction
     ↓
MRI Image
```

---

# 32. What Is K-Space?

K-space is **not directly the final MRI image**.

It represents spatial-frequency information collected during MRI acquisition.

```text
K-Space
   ↓
Fourier Transform
   ↓
MRI Image
```

---

# 33. Fourier Transform in MRI

A simplified relationship is:

$$
Image = FourierTransform(k-space)
$$

More precisely:

$$
I(x,y)
=
\mathcal{F}^{-1}
\{K(k_x,k_y)\}
$$

Where:

* \(K(k_x,k_y)\) = acquired k-space data
* \(I(x,y)\) = reconstructed image

---

# 34. MRI Reconstruction Pipeline

```text
Patient
   ↓
Magnetic Field
   ↓
RF Excitation
   ↓
MR Signal
   ↓
Spatial Encoding
   ↓
K-Space
   ↓
Fourier Reconstruction
   ↓
MRI Image
```

---

# 35. MRI Image Matrix

An MRI image may be represented as:

$$
I(x,y)
$$

or:

$$
I(x,y,z)
$$

for volumetric imaging.

Example:

```text
256 × 256
```

or:

```text
512 × 512
```

depending on the acquisition.

---

# 36. MRI Voxel

A 3D MRI dataset contains:

$$
\boxed{Voxels}
$$

Example:

$$
1mm \times 1mm \times 1mm
$$

represents an isotropic voxel.

---

# 37. MRI Resolution

Important concepts include:

```text
MRI Resolution
│
├── Spatial Resolution
├── Temporal Resolution
└── Contrast Resolution
```

These involve trade-offs with factors such as:

* Acquisition time
* Signal-to-noise ratio
* Image quality

---

# 38. MRI Noise

MRI images can contain noise.

Conceptually:

$$
Measured\ Signal
=
True\ Signal
+
Noise
$$

Noise can affect:

* Image quality
* Low-contrast structures
* Segmentation
* Registration
* AI algorithms

---

# 39. MRI Signal-to-Noise Ratio

A simplified concept:

$$
SNR
=
\frac{Signal}{Noise}
$$

Higher SNR generally makes useful structures easier to distinguish from noise.

---

# 40. MRI Artifacts

Common MRI artifact categories include:

```text
MRI Artifacts
│
├── Motion
├── Metal-Related Effects
├── Field Inhomogeneity
├── Aliasing
└── Acquisition-Related Effects
```

---

# 41. Motion Artifacts

Patient movement during acquisition can cause inconsistencies.

```text
Patient Motion
      ↓
Signal Inconsistency
      ↓
Artifact
```

---

# 42. Aliasing

Aliasing can occur when information outside the intended field of view affects the reconstructed image.

Conceptually:

```text
Object Outside FOV
        ↓
Incorrect Spatial Representation
        ↓
Aliasing Artifact
```

---

# 43. MRI Field of View

The **Field of View (FOV)** is the spatial region represented in an image.

```text
Patient
┌───────────────────┐
│        FOV        │
│                   │
└───────────────────┘
```

---

# 44. MRI DICOM Data

MRI data can be stored with important metadata.

```text
MRI DICOM
│
├── Pixel Data
├── Dimensions
├── Pixel Spacing
├── Slice Information
├── Orientation
├── Position
├── Sequence Information
└── Other Acquisition Metadata
```

Correct metadata handling is essential.

---

# 45. MRI Processing Pipeline

```text
MRI DICOM
     ↓
Load Image + Metadata
     ↓
Volume Construction
     ↓
Preprocessing
     ↓
Registration
     ↓
Segmentation
     ↓
Visualization
     ↓
Clinical / Research Workflow
```

---

# 46. MRI Image Processing

Common MRI processing tasks include:

```text
MRI Processing
│
├── Noise Reduction
├── Bias Field Correction
├── Registration
├── Segmentation
├── Skull Stripping
├── Feature Extraction
└── Visualization
```

We will study these in later modules.

---

# 47. MRI vs CT

| MRI                                        | CT                                         |
| ------------------------------------------ | ------------------------------------------ |
| Magnetic resonance                         | X-ray attenuation                          |
| No ionizing X-rays                         | Uses ionizing X-rays                       |
| Strong soft-tissue contrast                | Attenuation-based imaging                  |
| K-space acquisition                        | Projection acquisition                     |
| Fourier-based reconstruction commonly used | FBP/iterative reconstruction commonly used |

---

# 48. MRI vs X-Ray

| MRI                                        | X-Ray                         |
| ------------------------------------------ | ----------------------------- |
| Magnetic field + RF                        | X-rays                        |
| Usually cross-sectional/volumetric imaging | Usually projection imaging    |
| No ionizing X-rays                         | Uses ionizing radiation       |
| Complex signal acquisition                 | Direct projection acquisition |

---

# 49. MRI in Medical Image Processing

From a software-engineering perspective:

```text
MRI Dataset
      ↓
Read Metadata
      ↓
Load Pixel/Voxel Data
      ↓
Construct Volume
      ↓
Preprocessing
      ↓
Image Processing
      ↓
Visualization
```

Important concepts include:

* K-space
* Fourier reconstruction
* Voxel geometry
* Orientation
* Registration
* Segmentation
* MRI-specific intensity characteristics

---

# 50. MRI and Multimodal Imaging

MRI can be combined with other modalities.

```text
MRI
 +
CT
 ↓
Registration
 ↓
Combined Anatomical Information
```

Or:

```text
MRI
 +
PET
 ↓
Combined Structural + Functional Information
```

---

# 51. MRI in Radiation Therapy

MRI is increasingly important in radiation therapy workflows.

Conceptually:

```text
MRI
 ↓
Soft-Tissue Visualization
 ↓
Structure Delineation
 ↓
Registration with CT
 ↓
Treatment Planning
```

CT remains important in many workflows because density-related information is required for certain planning and dose-calculation processes.

---

# 52. Important Terms

### \(B_0\)

Main static magnetic field.

### RF Pulse

Radiofrequency energy used for excitation.

### Larmor Frequency

Precession frequency related to magnetic field strength.

### T1

Longitudinal relaxation.

### T2

Transverse relaxation.

### Gradient

Spatially varying magnetic field used for encoding position.

### K-Space

Spatial-frequency domain data acquired in MRI.

### Fourier Transform

Mathematical transformation used in MRI reconstruction.

### FOV

Field of View.

### Voxel

Three-dimensional image element.

---

# 53. Complete MRI Workflow

```text
                   PATIENT
                      │
                      ▼
             MAIN MAGNET (B₀)
                      │
                      ▼
           HYDROGEN ALIGNMENT
                      │
                      ▼
                RF EXCITATION
                      │
                      ▼
             MAGNETIC RESPONSE
                      │
                      ▼
              MR SIGNAL DETECTED
                      │
                      ▼
             SPATIAL ENCODING
          ┌───────────┼───────────┐
          ▼           ▼           ▼
      SLICE       FREQUENCY     PHASE
     SELECTION     ENCODING    ENCODING
          │           │           │
          └───────────┼───────────┘
                      ▼
                   K-SPACE
                      │
                      ▼
             FOURIER TRANSFORM
                      │
                      ▼
                  MRI IMAGE
                      │
                      ▼
              IMAGE PROCESSING
                      │
                      ▼
                VISUALIZATION
```

---

# 54. Practice Questions

### Question 1: Does MRI use ionizing X-rays?

**Answer:** No. MRI uses magnetic fields and radiofrequency signals rather than ionizing X-rays.

---

### Question 2: Why is hydrogen important in MRI?

**Answer:** Hydrogen is abundant in the body, particularly in water and fat, and its nuclei can generate MR signals.

---

### Question 3: What is \(B_0\)?

**Answer:** \(B_0\) is the main static magnetic field of the MRI scanner.

---

### Question 4: What is the Larmor equation?

$$
\omega_0 = \gamma B_0
$$

It relates precession frequency to magnetic field strength.

---

### Question 5: What is the difference between T1 and T2?

**Answer:**

* **T1:** longitudinal magnetization recovery.
* **T2:** transverse magnetization coherence decay.

---

### Question 6: What is K-space?

**Answer:** K-space is the spatial-frequency-domain representation of MRI data acquired before image reconstruction.

---

### Question 7: How is an MRI image reconstructed?

A simplified workflow is:

```text
MR Signals
    ↓
K-Space
    ↓
Fourier Transform
    ↓
MRI Image
```

---

### Question 8: What are the three major spatial encoding concepts?

* Slice selection
* Frequency encoding
* Phase encoding

---

# 55. Chapter Summary

In **Chapter 34: Magnetic Resonance Imaging (MRI)**, you learned:

* MRI fundamentals
* Main MRI components
* Static magnetic field \(B_0\)
* Hydrogen nuclei
* Nuclear spin
* Net magnetization
* Precession
* Larmor frequency
* RF excitation
* Magnetic resonance
* Longitudinal and transverse magnetization
* MRI signal detection
* T1 relaxation
* T2 relaxation
* MRI contrast
* Proton density
* Pulse sequences
* T1-weighted imaging
* T2-weighted imaging
* Spatial encoding
* Gradient coils
* Slice selection
* Frequency encoding
* Phase encoding
* K-space
* Fourier reconstruction
* MRI voxels and resolution
* Noise and SNR
* MRI artifacts
* Field of View
* MRI DICOM data
* MRI image processing
* MRI vs CT
* MRI multimodal imaging
* MRI in radiation therapy

---

## Progress

```text
LEVEL 1 — MEDICAL IMAGING FUNDAMENTALS

Module 4 — Medical Imaging Modalities

✅ Chapter 31: Introduction to Medical Imaging Modalities
✅ Chapter 32: X-Ray Imaging
✅ Chapter 33: Computed Tomography (CT)
✅ Chapter 34: Magnetic Resonance Imaging (MRI)
⬜ Chapter 35: Positron Emission Tomography (PET)
⬜ Chapter 36: Single Photon Emission Computed Tomography (SPECT)
⬜ Chapter 37: Ultrasound Imaging
⬜ Chapter 38: Cone Beam CT (CBCT)
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 35 — Positron Emission Tomography (PET)**
