# Chapter 36: Single Photon Emission Computed Tomography (SPECT)

**Single Photon Emission Computed Tomography (SPECT)** is a **nuclear medicine imaging modality** that creates 3D images showing the distribution of a radioactive tracer inside the body.

Unlike PET:

```text
PET → Detects pairs of annihilation photons

SPECT → Detects individual gamma photons
```

---

# 1. Basic SPECT Concept

The simplified SPECT workflow is:

```text
Radiopharmaceutical
        ↓
      Patient
        ↓
Radioactive Decay
        ↓
Gamma Photon Emission
        ↓
Gamma Camera Detection
        ↓
Multiple Angular Projections
        ↓
Image Reconstruction
        ↓
SPECT Volume
```

---

# 2. What Does SPECT Measure?

SPECT visualizes the distribution of a radioactive tracer within the body.

```text
Radiotracer Distribution
          ↓
Physiological / Biological Process
          ↓
Gamma Photon Emission
          ↓
SPECT Image
```

Depending on the radiotracer and examination, SPECT can provide information related to:

* Blood flow
* Organ function
* Tissue activity
* Biological processes

---

# 3. Basic Principle of SPECT

A radioactive tracer emits gamma photons from inside the patient's body.

```text
      Patient
         │
         ▼
 Radioactive Tracer
         │
         ▼
 Gamma Photon
         │
         ▼
  Gamma Camera
```

The gamma camera rotates around the patient and collects projections from different angles.

```text
Projection 1
     ↓
Projection 2
     ↓
Projection 3
     ↓
Projection N
     ↓
Reconstruction
     ↓
3D SPECT Image
```

---

# 4. What Is the Difference Between Planar Imaging and SPECT?

### Planar Nuclear Imaging

```text
Patient
   ↓
Gamma Camera
   ↓
2D Projection
```

### SPECT

```text
Patient
   ↓
Multiple Angular Projections
   ↓
Reconstruction
   ↓
3D Volume
```

So:

```text
Planar Imaging → 2D

SPECT → 3D Tomographic Imaging
```

---

# 5. Main Components of a SPECT System

A simplified system contains:

```text
SPECT System
│
├── Gamma Camera
├── Collimator
├── Scintillation Crystal
├── Photodetectors
├── Rotating Gantry
└── Reconstruction Computer
```

---

# 6. Gamma Camera

The gamma camera detects gamma photons emitted from the patient.

```text
Gamma Photon
      ↓
┌────────────────┐
│  GAMMA CAMERA  │
└────────────────┘
      ↓
Electrical Signal
      ↓
Image Data
```

---

# 7. The Collimator

A **collimator** is positioned in front of the detector.

Its main purpose is to provide directional information.

```text
Gamma Photon
     ↓
┌─────────────┐
│ COLLIMATOR  │
│  │ │ │ │ │  │
└─────────────┘
     ↓
DETECTOR
```

Only photons traveling through acceptable directions are more likely to reach the detector.

---

# 8. Why Is a Collimator Important?

Without directional selection:

```text
Photon Direction
↗ ↑ ← ↓ → ↘
      ↓
Difficult Localization
```

With a collimator:

```text
Selected Directions
       ↓
Better Spatial Information
```

However, collimation also reduces the number of detected photons.

Therefore:

```text
Spatial Information
       ↔
Detection Sensitivity
```

This trade-off is important in SPECT imaging.

---

# 9. Scintillation Crystal

After passing through the collimator, gamma photons interact with a scintillation crystal.

```text
Gamma Photon
      ↓
Scintillation Crystal
      ↓
Light
```

The crystal converts high-energy gamma photon interactions into light signals.

---

# 10. Photodetection

The generated light is detected and converted into electrical signals.

```text
Gamma Photon
      ↓
Crystal
      ↓
Light
      ↓
Photodetector
      ↓
Electrical Signal
```

These signals are processed to estimate information about the detected event.

---

# 11. Gamma Camera Detection Chain

```text
Gamma Photon
      ↓
 Collimator
      ↓
Scintillation Crystal
      ↓
    Light
      ↓
Photodetector
      ↓
Electrical Signal
      ↓
Position + Energy Information
```

---

# 12. SPECT Data Acquisition

SPECT acquires multiple projections around the patient.

```text
             Camera
               ●
           ↙       ↘

       ●      PATIENT      ●

           ↘       ↙
               ●
```

Each camera position provides a projection.

---

# 13. SPECT Projections

A projection is a 2D measurement from one viewing angle.

```text
Angle 1 → Projection 1
Angle 2 → Projection 2
Angle 3 → Projection 3
Angle N → Projection N
```

These projections are used to reconstruct the 3D tracer distribution.

---

# 14. SPECT Reconstruction

The reconstruction process estimates the tracer distribution inside the body.

```text
Multiple Projections
        ↓
Preprocessing / Corrections
        ↓
Reconstruction
        ↓
SPECT Volume
```

---

# 15. Analytical Reconstruction

One approach uses analytical reconstruction techniques.

Conceptually:

```text
Projection Data
       ↓
Filtering
       ↓
Back Projection
       ↓
Reconstructed Volume
```

---

# 16. Iterative Reconstruction

Another approach is iterative reconstruction.

```text
Initial Estimate
       ↓
Forward Projection
       ↓
Compare With Measured Data
       ↓
Calculate Difference
       ↓
Update Estimate
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

# 17. SPECT Corrections

Raw SPECT data can be affected by physical phenomena.

Important corrections include:

```text
SPECT Corrections
│
├── Attenuation Correction
├── Scatter Correction
└── Detector Corrections
```

---

# 18. Attenuation Correction

Gamma photons can lose intensity while traveling through tissue.

```text
Radioactive Source
        ↓
   Gamma Photon
        ↓
 Patient Tissue
        ↓
 Attenuation
        ↓
    Detector
```

This can affect the measured signal.

---

# 19. Scatter Correction

Gamma photons may change direction after interactions inside the body.

```text
Original Direction
       ↓
   Interaction
       ↓
 Scattered Photon
       ↓
    Detector
```

Scatter can reduce image quality and affect quantitative accuracy.

---

# 20. SPECT as a 3D Volume

After reconstruction:

$$
I(x,y,z)
$$

represents the reconstructed volume.

Each 3D element is a:

$$
\boxed{Voxel}
$$

```text
Slice 1
Slice 2
Slice 3
   ↓
SPECT Volume
```

---

# 21. SPECT Image Intensity

The intensity in a reconstructed SPECT image represents information related to detected radiotracer activity.

```text
Higher Activity
      ↓
Higher Reconstructed Signal

Lower Activity
      ↓
Lower Reconstructed Signal
```

The exact interpretation depends on the radiotracer, acquisition protocol, corrections, and reconstruction method.

---

# 22. SPECT Image Quality

Important image-quality characteristics include:

```text
SPECT Image Quality
│
├── Spatial Resolution
├── Sensitivity
├── Contrast
├── Noise
└── Quantitative Accuracy
```

---

# 23. Spatial Resolution

Spatial resolution determines how well nearby structures can be distinguished.

Factors include:

* Collimator design
* Detector characteristics
* Distance from detector
* Acquisition geometry
* Reconstruction

---

# 24. Sensitivity

Sensitivity describes how effectively the system detects emitted gamma photons.

```text
More Detected Events
        ↓
Higher Sensitivity
```

Collimator design strongly affects this.

---

# 25. Resolution-Sensitivity Trade-off

A major concept in SPECT is:

```text
Higher Spatial Resolution
          ↔
Lower Sensitivity
```

or:

```text
Higher Sensitivity
          ↔
Potentially Lower Spatial Resolution
```

The balance depends on the imaging system and clinical application.

---

# 26. SPECT Noise

SPECT data is based on detected photon events.

Therefore, statistical variation contributes to noise.

Conceptually:

$$
Measured\ Image
=
True\ Signal
+
Noise
$$

Noise affects:

* Small structure visibility
* Contrast
* Quantitative analysis
* Segmentation

---

# 27. SPECT Artifacts

Common artifact categories include:

```text
SPECT Artifacts
│
├── Patient Motion
├── Attenuation Effects
├── Scatter Effects
├── Detector Problems
└── Reconstruction Effects
```

---

# 28. SPECT and CT

Modern systems may combine:

$$
\boxed{SPECT/CT}
$$

```text
        SPECT
          +
         CT
          ↓
      SPECT/CT
```

---

# 29. Why Combine SPECT with CT?

```text
SPECT → Functional Information

CT → Anatomical Information
```

CT can also support attenuation correction in integrated workflows.

---

# 30. SPECT/CT Workflow

```text
Patient
   │
   ├──────────► CT Acquisition
   │
   └──────────► SPECT Acquisition
                    │
                    ▼
              Reconstruction
                    │
                    ▼
              Image Alignment
                    │
                    ▼
            SPECT + CT Fusion
                    │
                    ▼
              Visualization
```

---

# 31. SPECT Applications

SPECT has applications in areas such as:

```text
SPECT Applications
│
├── Cardiology
├── Neurology
├── Bone Imaging
├── Organ Function Studies
└── Other Nuclear Medicine Workflows
```

---

# 32. SPECT in Cardiology

SPECT is commonly used in cardiac imaging workflows.

Conceptually:

```text
Radiotracer
    ↓
Heart Distribution
    ↓
Gamma Detection
    ↓
SPECT Reconstruction
    ↓
Functional Assessment
```

---

# 33. SPECT in Neurology

SPECT can provide information related to brain physiology and tracer distribution.

```text
Radiotracer
    ↓
Brain
    ↓
Gamma Photon Detection
    ↓
SPECT Image
```

---

# 34. SPECT in Bone Imaging

Some radiotracers can provide information relevant to bone activity.

```text
Radiotracer
    ↓
Bone Distribution
    ↓
SPECT Imaging
    ↓
3D Functional Information
```

---

# 35. SPECT and Medical Image Processing

From a software perspective:

```text
Raw Projection Data
        ↓
Corrections
        ↓
Reconstruction
        ↓
SPECT Volume
        ↓
Registration
        ↓
Fusion with CT
        ↓
Visualization
        ↓
Analysis
```

Important topics include:

* Projection processing
* Reconstruction
* Noise reduction
* Attenuation correction
* Scatter correction
* Registration
* Multimodal visualization

---

# 36. SPECT DICOM Data

A SPECT dataset can contain:

```text
SPECT DICOM
│
├── Pixel / Voxel Data
├── Image Geometry
├── Image Position
├── Image Orientation
├── Acquisition Information
├── Reconstruction Information
└── Radiopharmaceutical Information
```

Correct interpretation of metadata is essential for medical imaging software.

---

# 37. SPECT Visualization

SPECT data can be displayed using:

```text
SPECT Volume
      │
      ├── Axial View
      ├── Coronal View
      ├── Sagittal View
      └── 3D Visualization
```

Functional images are also frequently displayed using color mapping.

---

# 38. SPECT Color Mapping

Conceptually:

```text
Low Activity
      ↓
Color A

Medium Activity
      ↓
Color B

High Activity
      ↓
Color C
```

Color mapping can help visualize changes in tracer distribution.

---

# 39. SPECT vs PET

| SPECT                              | PET                                     |
| ---------------------------------- | --------------------------------------- |
| Detects individual gamma photons   | Detects photon pairs from annihilation  |
| Uses gamma camera + collimator     | Uses coincidence detector system        |
| Uses emitted single photons        | Uses two approximately opposite photons |
| Tomographic nuclear imaging        | Tomographic nuclear imaging             |
| Can provide functional information | Can provide functional information      |

---

# 40. SPECT vs CT

| SPECT                     | CT                            |
| ------------------------- | ----------------------------- |
| Functional imaging        | Primarily anatomical imaging  |
| Radiotracer-based         | X-ray attenuation-based       |
| Gamma photon detection    | X-ray detection               |
| Nuclear medicine modality | X-ray imaging modality        |
| Often combined with CT    | Provides anatomical reference |

---

# 41. Important Terms

### Radiotracer

A radioactive substance used to produce detectable signals.

### Gamma Photon

A high-energy photon emitted during radioactive decay.

### Gamma Camera

A detector system used to measure emitted gamma photons.

### Collimator

A structure that provides directional information for gamma photons.

### Scintillation Crystal

A material that converts gamma photon interactions into light.

### Projection

A measurement acquired from a specific angle.

### Reconstruction

The process of estimating a 3D distribution from multiple projections.

### Attenuation Correction

Correction for photon loss while traveling through tissue.

### Scatter Correction

Correction for effects caused by scattered photons.

---

# 42. Complete SPECT Workflow

```text
              RADIOPHARMACEUTICAL
                      │
                      ▼
                   PATIENT
                      │
                      ▼
              RADIOACTIVE DECAY
                      │
                      ▼
              GAMMA PHOTON EMISSION
                      │
                      ▼
                COLLIMATOR
                      │
                      ▼
            SCINTILLATION CRYSTAL
                      │
                      ▼
                    LIGHT
                      │
                      ▼
                PHOTODETECTOR
                      │
                      ▼
               ELECTRICAL SIGNAL
                      │
                      ▼
             MULTIPLE PROJECTIONS
                      │
                      ▼
                  CORRECTIONS
                      │
                      ▼
               RECONSTRUCTION
                      │
                      ▼
                SPECT VOLUME
                      │
                      ▼
                 SPECT / CT
                      │
                      ▼
               VISUALIZATION
                      │
                      ▼
             CLINICAL WORKFLOW
```

---

# 43. Practice Questions

### Question 1: What does SPECT stand for?

**Answer:** Single Photon Emission Computed Tomography.

---

### Question 2: What type of radiation does SPECT detect?

**Answer:** Gamma photons emitted from a radioactive tracer.

---

### Question 3: What is the role of a collimator?

**Answer:** It provides directional selection of gamma photons to help obtain spatial information.

---

### Question 4: Why does a gamma camera use a scintillation crystal?

**Answer:** The crystal converts gamma photon interactions into light, which can then be detected and converted into electrical signals.

---

### Question 5: Why does SPECT acquire projections from multiple angles?

**Answer:** Multiple angular projections are required to reconstruct the 3D distribution of radiotracer activity.

---

### Question 6: What is SPECT/CT?

**Answer:**

```text
SPECT → Functional Information
CT → Anatomical Information
```

SPECT/CT combines these complementary types of information.

---

# 44. Chapter Summary

In **Chapter 36: Single Photon Emission Computed Tomography (SPECT)**, you learned:

* SPECT fundamentals
* Functional imaging
* Radiotracers
* Gamma photon emission
* Gamma cameras
* Collimators
* Scintillation crystals
* Photodetection
* Gamma camera detection chain
* Angular projections
* SPECT reconstruction
* Analytical reconstruction
* Iterative reconstruction
* Attenuation correction
* Scatter correction
* SPECT volumes and voxels
* Image intensity
* Spatial resolution
* Sensitivity
* Resolution-sensitivity trade-off
* Noise
* Artifacts
* SPECT/CT
* Cardiology applications
* Neurology applications
* Bone imaging
* SPECT DICOM data
* SPECT visualization
* Color mapping
* SPECT vs PET
* SPECT vs CT

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
⬜ Chapter 37: Ultrasound Imaging
⬜ Chapter 38: Cone Beam CT (CBCT)
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 37 — Ultrasound Imaging**
