# Chapter 35: Positron Emission Tomography (PET)

**Positron Emission Tomography (PET)** is a **nuclear medicine imaging modality** that provides information about biological and metabolic activity inside the body.

Unlike CT:

```text
CT → Mainly anatomical / structural information

PET → Functional / metabolic information
```

---

# 1. Basic PET Concept

PET imaging works using a **radiopharmaceutical (radiotracer)** introduced into the patient.

```text
Radiotracer
    ↓
Patient
    ↓
Radioactive Decay
    ↓
Positron Emission
    ↓
Annihilation
    ↓
Gamma Photons
    ↓
PET Detectors
    ↓
PET Reconstruction
    ↓
PET Image
```

---

# 2. What Does PET Measure?

PET is primarily used to visualize the distribution of a radiotracer within the body.

```text
Radiotracer Distribution
          ↓
Biological Process
          ↓
Functional Information
          ↓
PET Image
```

Depending on the tracer and clinical application, PET can provide information related to:

* Metabolism
* Tissue activity
* Molecular processes
* Physiological processes

---

# 3. PET vs Anatomical Imaging

```text
CT
↓
Where is the structure?
```

```text
MRI
↓
Detailed soft-tissue structure
```

```text
PET
↓
Where is radiotracer activity occurring?
```

Therefore, PET is often combined with anatomical imaging.

---

# 4. PET Radiotracers

A PET examination uses a radiotracer.

Conceptually:

```text
Radiopharmaceutical
│
├── Biological Molecule
│
└── Radioactive Isotope
```

After administration:

```text
Radiotracer
    ↓
Patient
    ↓
Distribution in Body
    ↓
PET Detection
```

---

# 5. Positron Emission

Some radioactive isotopes undergo decay that emits a **positron**.

```text
Radioactive Nucleus
        ↓
      Decay
        ↓
     Positron
```

A positron is the antimatter counterpart of an electron.

---

# 6. Positron Travel

After emission, the positron travels a short distance through tissue.

```text
Radioactive Decay
       ↓
    Positron
       ↓
Short Distance
       ↓
     Electron
```

Eventually, the positron interacts with an electron.

---

# 7. Annihilation

When a positron and electron interact:

```text
Positron (+)
     +
Electron (-)
     ↓
Annihilation
```

This produces two gamma photons.

```text
Annihilation
     ↓
Photon ← → Photon
```

The photons travel in approximately opposite directions.

---

# 8. The 511 keV Photons

The annihilation process produces gamma photons with an energy of approximately:

$$
\boxed{511\ keV}
$$

Conceptually:

```text
       511 keV
          ←
Electron + Positron
          →
       511 keV
```

These photons are detected by the PET scanner.

---

# 9. PET Detector Ring

PET scanners contain detectors arranged around the patient.

```text
       ┌───────────────┐
       │ ● ● ● ● ● ● ● │
       │ ●           ● │
       │ ●  PATIENT  ● │
       │ ●           ● │
       │ ● ● ● ● ● ● ● │
       └───────────────┘
```

The detector system identifies gamma photons.

---

# 10. Coincidence Detection

PET uses **coincidence detection**.

```text
Detector A
     ↑
  Photon
     ↑
Annihilation
     ↓
  Photon
     ↓
Detector B
```

If two photons are detected within an appropriate timing window, the system can identify them as a coincidence event.

---

# 11. Line of Response (LOR)

When two detectors detect a coincidence event:

```text
Detector A ●────────────● Detector B
                ↑
       Possible Event Location
```

The line connecting the two detectors is called the:

$$
\boxed{Line\ of\ Response\ (LOR)}
$$

The annihilation event occurred somewhere along this line, within the basic coincidence model.

---

# 12. PET Data Acquisition

PET data is collected as many detected events.

```text
Annihilation Event
       ↓
Coincidence Detection
       ↓
Line of Response
       ↓
PET Raw Data
       ↓
Reconstruction
       ↓
PET Image
```

---

# 13. PET Image Reconstruction

PET reconstruction estimates the spatial distribution of radiotracer activity.

```text
Detected Events
      ↓
Raw PET Data
      ↓
Reconstruction
      ↓
Activity Distribution
      ↓
PET Image
```

---

# 14. PET as a 3D Imaging Modality

PET typically produces volumetric data.

$$
I(x,y,z)
$$

Each element is a:

$$
\boxed{Voxel}
$$

Conceptually:

```text
PET Slice 1
PET Slice 2
PET Slice 3
      ↓
   PET Volume
```

---

# 15. PET Image Intensity

PET voxel values represent reconstructed information related to radiotracer activity.

```text
High Activity
     ↓
Higher Reconstructed Signal

Low Activity
     ↓
Lower Reconstructed Signal
```

The exact quantitative interpretation depends on acquisition, reconstruction, corrections, and clinical methodology.

---

# 16. PET Reconstruction Methods

PET reconstruction approaches include:

```text
PET Reconstruction
│
├── Analytical Methods
│
└── Iterative Methods
```

A common conceptual iterative workflow is:

```text
Initial Estimate
      ↓
Predict Measurements
      ↓
Compare With Actual Data
      ↓
Update Estimate
      ↓
Repeat
```

---

# 17. PET Corrections

Raw PET data requires important corrections.

Conceptually:

```text
Raw PET Data
     ↓
Corrections
     ↓
Reconstruction
     ↓
PET Image
```

Common correction categories include:

* Attenuation correction
* Scatter correction
* Random coincidence correction
* Detector normalization

---

# 18. Attenuation Correction

Gamma photons can be attenuated while traveling through the body.

```text
Annihilation
      ↓
Gamma Photon
      ↓
Patient Tissue
      ↓
Attenuation
      ↓
Detector
```

Without proper correction, measured activity may be affected.

---

# 19. Scatter in PET

A gamma photon may scatter before reaching the detector.

```text
True Direction
      ↓
Photon Interaction
      ↓
Scattered Photon
      ↓
Detector
```

Scatter can affect localization and quantitative accuracy.

---

# 20. Random Coincidences

Sometimes two unrelated photons are detected within the coincidence timing window.

```text
Photon A ──┐
           ├── Random Coincidence
Photon B ──┘
```

These events do not represent a true pair from the same annihilation event.

---

# 21. True Coincidences

A true coincidence occurs when both detected photons originate from the same annihilation event.

```text
One Annihilation
       ↓
Two Photons
       ↓
Two Detectors
       ↓
True Coincidence
```

---

# 22. PET Image Quality

Important PET image-quality factors include:

```text
PET Image Quality
│
├── Spatial Resolution
├── Sensitivity
├── Noise
├── Contrast
└── Quantitative Accuracy
```

---

# 23. Spatial Resolution

Spatial resolution determines how well small structures can be distinguished.

Factors include:

* Detector characteristics
* Positron range
* Photon detection properties
* Reconstruction
* Patient motion

---

# 24. PET Sensitivity

Sensitivity describes how effectively the system detects relevant events.

Conceptually:

```text
More Detected Relevant Events
           ↓
Higher Sensitivity
```

---

# 25. PET Noise

PET imaging is based on detected events, which can have statistical variation.

Conceptually:

$$
Measured\ PET\ Data
=
True\ Signal
+
Statistical\ Variation
$$

Noise affects:

* Image quality
* Small lesion visibility
* Quantitative measurements

---

# 26. PET Resolution vs Noise

A common trade-off exists:

```text
More Image Detail
      ↔
Potential Noise Effects
```

Reconstruction settings can influence the balance between image resolution and noise.

---

# 27. PET and CT

Modern clinical systems often combine PET with CT.

```text
        PET
         +
        CT
         ↓
     PET/CT
```

This combines:

```text
PET → Functional Information

CT → Anatomical Information
```

---

# 28. PET/CT Workflow

```text
Patient
   ↓
CT Acquisition
   ↓
PET Acquisition
   ↓
Image Reconstruction
   ↓
Image Alignment
   ↓
PET + CT Visualization
```

This allows functional information to be viewed with anatomical context.

---

# 29. PET/MRI

PET can also be combined with MRI.

```text
PET
 +
MRI
 ↓
PET/MRI
```

Conceptually:

```text
PET → Functional Information

MRI → Soft-Tissue Information
```

---

# 30. Multimodal Imaging

PET demonstrates the importance of multimodal imaging.

```text
PET Activity
     +
CT Anatomy
     ↓
Combined Information
```

Or:

```text
PET Activity
     +
MRI Soft Tissue
     ↓
Combined Information
```

---

# 31. PET in Oncology

One important application area is oncology.

Conceptually:

```text
Radiotracer
    ↓
Patient
    ↓
Tracer Distribution
    ↓
PET Imaging
    ↓
Assessment of Relevant Biological Activity
```

PET may be used in workflows involving:

* Disease assessment
* Staging
* Treatment response assessment
* Follow-up

Specific interpretation depends on the tracer and clinical context.

---

# 32. PET in Neurology

PET can also be used in neurological applications.

```text
Brain
  ↓
PET Tracer Distribution
  ↓
Functional Imaging
```

---

# 33. PET in Cardiology

PET can provide functional information relevant to some cardiac applications.

```text
Heart
  ↓
Tracer Distribution
  ↓
Functional Assessment
```

---

# 34. PET in Radiation Therapy

This is especially relevant to your medical imaging and TPS learning.

```text
PET/CT
   ↓
Tumor / Activity Information
   ↓
Image Registration
   ↓
Structure Delineation Support
   ↓
Treatment Planning Workflow
```

PET may provide complementary functional information alongside CT anatomy.

---

# 35. PET and Image Registration

PET and CT datasets may need spatial alignment.

```text
PET Volume
    +
CT Volume
    ↓
Registration / Alignment
    ↓
Combined Visualization
```

Correct spatial geometry is extremely important.

---

# 36. PET DICOM Data

PET data may include:

```text
PET Dataset
│
├── Pixel / Voxel Data
├── Spatial Geometry
├── Image Position
├── Orientation
├── Acquisition Information
├── Reconstruction Information
└── Radiopharmaceutical Information
```

Medical image-processing software must correctly handle this information.

---

# 37. PET Processing Pipeline

```text
PET Acquisition
      ↓
Detected Events
      ↓
Corrections
      ↓
Reconstruction
      ↓
PET Volume
      ↓
Registration
      ↓
Fusion With CT/MRI
      ↓
Visualization
      ↓
Clinical Workflow
```

---

# 38. PET Image Visualization

PET images can be visualized using:

```text
PET Volume
    ↓
Axial View
Coronal View
Sagittal View
    ↓
Color Mapping
    ↓
Multimodal Fusion
```

PET often benefits from color visualization because activity distribution can be easier to interpret with a suitable color map.

---

# 39. PET and Color Mapping

Conceptually:

```text
Low Activity
     ↓
Color Value A

Medium Activity
     ↓
Color Value B

High Activity
     ↓
Color Value C
```

The choice of color map affects visualization and should be handled carefully in clinical software.

---

# 40. PET vs CT

| PET                      | CT                               |
| ------------------------ | -------------------------------- |
| Nuclear medicine imaging | X-ray-based imaging              |
| Functional information   | Anatomical information           |
| Radiotracer-based        | X-ray attenuation-based          |
| Activity distribution    | Attenuation distribution         |
| Commonly fused with CT   | Can provide anatomical reference |

---

# 41. PET vs MRI

| PET                                    | MRI                         |
| -------------------------------------- | --------------------------- |
| Radiotracer activity                   | Magnetic resonance signals  |
| Functional/molecular information       | Strong soft-tissue contrast |
| Nuclear medicine                       | Magnetic resonance modality |
| Often combined with anatomical imaging | Can be combined with PET    |

---

# 42. Important Terms

### Radiotracer

A radiopharmaceutical used to provide detectable PET signals.

### Positron

The antimatter counterpart of an electron.

### Annihilation

Interaction between a positron and electron resulting in photon production.

### 511 keV

Approximate energy of each annihilation photon.

### Coincidence Detection

Detection of two related photons within a defined timing window.

### Line of Response (LOR)

Line connecting two detectors that detect a coincidence event.

### Attenuation Correction

Correction for photon loss due to tissue attenuation.

### Scatter Correction

Correction for photons affected by scattering.

### Random Coincidence

Detection of unrelated photons as if they were a pair.

---

# 43. Complete PET Workflow

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
                   POSITRON EMISSION
                          │
                          ▼
             POSITRON + ELECTRON
                          │
                          ▼
                    ANNIHILATION
                          │
                 ┌────────┴────────┐
                 ▼                 ▼
            511 keV            511 keV
             Photon             Photon
                 │                 │
                 ▼                 ▼
             DETECTOR          DETECTOR
                 └────────┬────────┘
                          ▼
                 COINCIDENCE EVENT
                          │
                          ▼
                 LINE OF RESPONSE
                          │
                          ▼
                     RAW PET DATA
                          │
                          ▼
                    CORRECTIONS
                          │
                          ▼
                  RECONSTRUCTION
                          │
                          ▼
                     PET VOLUME
                          │
                          ▼
                PET/CT OR PET/MRI
                          │
                          ▼
                    VISUALIZATION
                          │
                          ▼
                 CLINICAL WORKFLOW
```

---

# 44. Practice Questions

### Question 1: What type of information does PET primarily provide?

**Answer:** PET primarily provides information about radiotracer distribution and related biological or functional processes.

---

### Question 2: What happens after positron emission?

**Answer:** The positron travels a short distance, interacts with an electron, and undergoes annihilation.

---

### Question 3: What is produced during annihilation?

**Answer:** Two gamma photons, each with approximately:

$$
511\ keV
$$

---

### Question 4: What is coincidence detection?

**Answer:** Detection of two photons within an appropriate timing window so they can be associated with a potential annihilation event.

---

### Question 5: What is a Line of Response?

**Answer:** The line connecting two detectors that detect a coincidence event.

---

### Question 6: Why is attenuation correction important?

**Answer:** Gamma photons can be attenuated within the body, affecting measured PET signals and quantitative interpretation.

---

### Question 7: Why combine PET with CT?

**Answer:**

```text
PET → Functional Information

CT → Anatomical Information
```

Together they provide complementary information.

---

# 45. Chapter Summary

In **Chapter 35: Positron Emission Tomography (PET)**, you learned:

* PET fundamentals
* Functional imaging
* Radiotracers
* Positron emission
* Positron travel
* Electron-positron annihilation
* 511 keV photons
* PET detector rings
* Coincidence detection
* Line of Response
* PET data acquisition
* PET reconstruction
* PET volumes and voxels
* PET image intensity
* Analytical and iterative reconstruction
* Attenuation correction
* Scatter correction
* Random coincidences
* True coincidences
* PET image quality
* Spatial resolution
* Sensitivity
* Noise
* PET/CT
* PET/MRI
* Multimodal imaging
* Oncology applications
* Neurology applications
* Cardiology applications
* PET in radiation therapy
* Image registration
* PET DICOM data
* PET processing pipeline
* PET visualization
* Color mapping

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
⬜ Chapter 36: Single Photon Emission Computed Tomography (SPECT)
⬜ Chapter 37: Ultrasound Imaging
⬜ Chapter 38: Cone Beam CT (CBCT)
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 36 — Single Photon Emission Computed Tomography (SPECT)**
