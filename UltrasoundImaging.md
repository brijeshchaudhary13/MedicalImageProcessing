# Chapter 37: Ultrasound Imaging

**Ultrasound imaging (Ultrasonography)** is a medical imaging modality that uses **high-frequency sound waves** to create images of internal body structures.

Unlike CT and X-ray:

```text
CT / X-Ray → Uses ionizing radiation

MRI → Uses magnetic field + RF

Ultrasound → Uses high-frequency sound waves
```

---

# 1. Basic Ultrasound Concept

```text
Ultrasound Transducer
        ↓
 Sends Sound Waves
        ↓
       Body
        ↓
Reflection / Echoes
        ↓
Transducer Receives Echoes
        ↓
Signal Processing
        ↓
Ultrasound Image
```

The basic principle is:

> **Send sound → receive echoes → calculate location → create image**

---

# 2. What Is Ultrasound?

Ultrasound refers to sound frequencies above the normal human hearing range.

Medical ultrasound uses much higher frequencies than humans can hear.

```text
Electrical Signal
       ↓
Transducer
       ↓
Ultrasound Wave
       ↓
Patient Tissue
       ↓
Echo
       ↓
Transducer
       ↓
Electrical Signal
       ↓
Image
```

---

# 3. Main Components of an Ultrasound System

```text
Ultrasound System
│
├── Transducer / Probe
├── Transmitter
├── Receiver
├── Signal Processing System
├── Beamformer
├── Image Processing System
└── Display
```

---

# 4. Ultrasound Transducer

The **transducer**, commonly called the **probe**, is one of the most important components.

It performs two major functions:

```text
TRANSMIT
Electrical Energy
      ↓
Sound Wave
```

and:

```text
RECEIVE
Echo
  ↓
Electrical Signal
```

Therefore:

```text
Transducer
    ↓
Transmit + Receive
```

---

# 5. Piezoelectric Effect

Ultrasound transducers use the **piezoelectric effect**.

Simplified:

```text
Electrical Energy
      ↓
Piezoelectric Material
      ↓
Mechanical Vibration
      ↓
Ultrasound Wave
```

When echoes return:

```text
Mechanical Vibration
      ↓
Piezoelectric Material
      ↓
Electrical Signal
```

---

# 6. Ultrasound Wave Propagation

The transducer sends ultrasound waves into the body.

```text
      PROBE
        │
        ▼
~~~~~~~~~~~~~~~~
~~~~~~~~~~~~~~~~
     BODY
~~~~~~~~~~~~~~~~
```

As sound travels through tissue, interactions occur at boundaries between different materials.

---

# 7. Acoustic Impedance

Different tissues have different acoustic properties.

An important concept is **acoustic impedance**:

$$
Z = \rho c
$$

Where:

* \(Z\) = acoustic impedance
* \(\rho\) = density of the medium
* \(c\) = speed of sound

Differences in acoustic impedance influence reflection.

---

# 8. Reflection of Ultrasound Waves

When ultrasound reaches a boundary between tissues:

```text
Tissue A
──────────────
      ↓
 Ultrasound
      ↓
──────────────
Tissue B
```

Part of the sound may continue, and part may reflect.

```text
Incident Wave
      ↓
   Boundary
    ↙    ↘
Echo    Transmission
```

The reflected signal is called an:

$$
\boxed{Echo}
$$

---

# 9. Echo Formation

The imaging principle is:

```text
Send Pulse
    ↓
Travel Through Tissue
    ↓
Hit Tissue Boundary
    ↓
Reflection
    ↓
Echo Returns
    ↓
Transducer Detects Echo
```

The ultrasound system processes these echoes to construct an image.

---

# 10. Depth Calculation

The ultrasound system measures the time required for an echo to return.

Conceptually:

```text
Pulse Sent
     ↓
Travel to Structure
     ↓
Reflection
     ↓
Echo Returns
```

A simplified depth relationship is:

$$
d = \frac{ct}{2}
$$

Where:

* \(d\) = depth
* \(c\) = speed of sound
* \(t\) = round-trip travel time

The division by 2 is required because the sound travels:

```text
Probe → Structure → Probe
```

---

# 11. Why Ultrasound Gel Is Used

Air between the probe and skin can significantly interfere with ultrasound transmission.

Therefore:

```text
Without Gel
Probe
 ↓
Air
 ↓
Poor Transmission
```

With coupling gel:

```text
Probe
 ↓
Gel
 ↓
Skin
 ↓
Better Sound Transmission
```

---

# 12. Ultrasound Frequency

Frequency affects image characteristics.

Conceptually:

```text
Higher Frequency
      ↓
Better Detail
      ↓
Lower Penetration
```

```text
Lower Frequency
      ↓
Greater Penetration
      ↓
Potentially Lower Detail
```

This creates an important trade-off.

---

# 13. Frequency vs Penetration

| Higher Frequency                  | Lower Frequency              |
| --------------------------------- | ---------------------------- |
| Better spatial detail             | Greater penetration          |
| More attenuation                  | Can reach deeper structures  |
| Useful for superficial structures | Useful for deeper structures |

---

# 14. Ultrasound Attenuation

As sound travels through tissue, its energy decreases.

```text
Probe
  ↓
Strong Signal
  ↓
Tissue
  ↓
Reduced Signal
  ↓
Greater Depth
```

This reduction is called:

$$
\boxed{Attenuation}
$$

---

# 15. Ultrasound Resolution

Important resolution concepts include:

```text
Ultrasound Resolution
│
├── Axial Resolution
├── Lateral Resolution
└── Temporal Resolution
```

---

# 16. Axial Resolution

Axial resolution refers to the ability to distinguish structures along the direction of the ultrasound beam.

```text
Probe
  ↓
Object A
  ↓
Object B
```

Better axial resolution allows closely spaced structures along the beam direction to be distinguished.

---

# 17. Lateral Resolution

Lateral resolution refers to the ability to distinguish structures located side-by-side.

```text
Probe
   ↓
 A     B
 │     │
```

Beam characteristics strongly influence lateral resolution.

---

# 18. Temporal Resolution

Temporal resolution refers to how well the system represents changes over time.

```text
Frame 1
   ↓
Frame 2
   ↓
Frame 3
```

This is especially important for moving structures such as the heart.

---

# 19. Ultrasound Imaging Modes

Common ultrasound imaging modes include:

```text
Ultrasound Modes
│
├── A-Mode
├── B-Mode
├── M-Mode
└── Doppler Imaging
```

---

# 20. A-Mode

A-mode means **Amplitude Mode**.

It displays echo amplitude as a function of depth.

```text
Amplitude
    ↑
    │     /\
    │    /  \
    │___/____\________→ Depth
```

---

# 21. B-Mode

B-mode means **Brightness Mode**.

Echo strength is displayed using brightness.

```text
Strong Echo
     ↓
Bright Pixel

Weak Echo
     ↓
Darker Pixel
```

B-mode is one of the most commonly used ultrasound imaging modes.

---

# 22. M-Mode

M-mode means **Motion Mode**.

It displays motion over time.

```text
Position
   ↑
   │
   │ Wave Pattern
   │
   └────────────────→ Time
```

It is useful for evaluating moving structures.

---

# 23. Doppler Ultrasound

Doppler ultrasound is used to analyze motion, particularly blood flow.

```text
Transducer
     ↓
 Ultrasound
     ↓
Moving Blood
     ↓
Frequency Change
     ↓
Doppler Analysis
```

---

# 24. Doppler Effect

The Doppler effect involves a frequency change associated with relative motion.

Conceptually:

```text
Stationary Object
      ↓
Minimal Motion-Related Shift
```

```text
Moving Blood
      ↓
Frequency Shift
      ↓
Doppler Information
```

---

# 25. Doppler Applications

Doppler ultrasound can provide information related to:

* Blood flow
* Flow direction
* Relative velocity
* Cardiac motion

---

# 26. Color Doppler

Color Doppler overlays flow information on a grayscale ultrasound image.

```text
B-Mode Image
      +
Doppler Flow Data
      ↓
Color Doppler Image
```

Conceptually:

```text
Anatomy → Grayscale

Flow → Color Overlay
```

---

# 27. Spectral Doppler

Spectral Doppler displays frequency or velocity-related information over time.

```text
Velocity / Frequency
        ↑
        │  Waveform
        │ /\/\/\/\
        └────────────→ Time
```

---

# 28. Ultrasound Beam

The ultrasound probe does not produce an infinitely thin beam.

```text
Probe
  │
  ▼
   \   /
    \ /
     |
     |
```

Beam width affects spatial resolution.

---

# 29. Focusing

Ultrasound systems can focus the sound beam.

```text
Without Focus
   \      /
    \    /
     \  /
      \/
```

```text
Focused Beam
      \ /
       V
       │
```

Focusing can improve spatial resolution in the focal region.

---

# 30. Beamforming

Modern ultrasound systems use multiple transducer elements.

```text
Transducer Array

| | | | | | | |
```

By controlling timing:

```text
Multiple Elements
       ↓
Timing Control
       ↓
Beam Formation
       ↓
Focused Ultrasound Beam
```

This process is called:

$$
\boxed{Beamforming}
$$

---

# 31. Ultrasound Scan Lines

An ultrasound image is created from multiple scan lines.

```text
Probe
┌─────────────┐
      ↓
     \|/
    \ | /
   \  |  /
```

Each line provides depth-related information.

Multiple lines form a 2D image.

---

# 32. Ultrasound Image Formation

```text
Transmit Pulse
      ↓
Echoes Return
      ↓
Depth Calculation
      ↓
Echo Amplitude
      ↓
Brightness Mapping
      ↓
Multiple Scan Lines
      ↓
2D Ultrasound Image
```

---

# 33. Ultrasound Artifacts

Common ultrasound artifacts include:

```text
Ultrasound Artifacts
│
├── Acoustic Shadowing
├── Posterior Enhancement
├── Reverberation
├── Refraction
└── Motion-Related Effects
```

---

# 34. Acoustic Shadowing

Some structures strongly attenuate ultrasound.

```text
Probe
  ↓
██████ Strong Attenuator
  ↓
██████
       ↓
    Shadow
```

The region behind the structure may receive reduced ultrasound energy.

---

# 35. Posterior Acoustic Enhancement

Some structures allow relatively greater sound transmission.

```text
Probe
  ↓
Low Attenuation Region
  ↓
More Sound Continues
  ↓
Brighter Region Beyond
```

This is called posterior acoustic enhancement.

---

# 36. Reverberation

Reverberation occurs when sound repeatedly reflects between interfaces.

```text
Probe
  ↓
Boundary
  ↑
Probe
  ↓
Boundary
```

This can produce repeated echo patterns.

---

# 37. Ultrasound Noise

Ultrasound images can contain a granular pattern called:

$$
\boxed{Speckle}
$$

Conceptually:

```text
████░███
██░█████
██████░█
```

Speckle is an important characteristic of ultrasound images.

---

# 38. Speckle Reduction

Medical image processing may apply methods to reduce speckle while attempting to preserve important structures.

```text
Ultrasound Image
       ↓
Speckle Reduction
       ↓
Improved Visualization
```

This is an important ultrasound image-processing topic.

---

# 39. Ultrasound Segmentation

Possible processing workflow:

```text
Ultrasound Image
       ↓
Preprocessing
       ↓
Noise / Speckle Reduction
       ↓
Feature Extraction
       ↓
Segmentation
```

Segmentation can be challenging because of:

* Speckle
* Low contrast
* Artifacts
* Variable anatomy

---

# 40. Ultrasound in 3D

Ultrasound can also generate volumetric information.

```text
2D Frames
    ↓
Spatial Acquisition
    ↓
Volume Construction
    ↓
3D Ultrasound
```

---

# 41. 4D Ultrasound

Conceptually:

```text
3D Ultrasound
      +
Time
      ↓
4D Ultrasound
```

This allows visualization of changing 3D structures over time.

---

# 42. Ultrasound Applications

```text
Ultrasound Applications
│
├── Obstetrics
├── Cardiology
├── Abdominal Imaging
├── Vascular Imaging
├── Musculoskeletal Imaging
└── General Clinical Imaging
```

---

# 43. Obstetric Ultrasound

Ultrasound is widely used for imaging during pregnancy.

```text
Probe
  ↓
Sound Waves
  ↓
Fetal Structures
  ↓
Echoes
  ↓
Image
```

---

# 44. Echocardiography

Ultrasound imaging of the heart is called:

$$
\boxed{Echocardiography}
$$

It can provide information about:

* Cardiac structure
* Motion
* Blood flow

---

# 45. Ultrasound DICOM Data

Ultrasound studies can contain:

```text
Ultrasound DICOM
│
├── Image Pixel Data
├── Spatial Information
├── Frame Information
├── Doppler Information
└── Acquisition Metadata
```

Some ultrasound data may also be stored as multi-frame images.

---

# 46. Ultrasound Processing Pipeline

```text
Transducer
      ↓
Transmit Ultrasound
      ↓
Receive Echoes
      ↓
Signal Processing
      ↓
Beamforming
      ↓
Image Formation
      ↓
Ultrasound Image
      ↓
Image Processing
      ↓
Visualization
```

---

# 47. Ultrasound vs CT

| Ultrasound                     | CT                                     |
| ------------------------------ | -------------------------------------- |
| Sound waves                    | X-rays                                 |
| No ionizing radiation          | Uses ionizing radiation                |
| Real-time imaging possible     | Reconstruction-based imaging           |
| Often portable                 | Typically larger scanner system        |
| Operator-dependent acquisition | More standardized acquisition geometry |

---

# 48. Ultrasound vs MRI

| Ultrasound                   | MRI                            |
| ---------------------------- | ------------------------------ |
| Sound waves                  | Magnetic field + RF            |
| Real-time imaging capability | Complex signal acquisition     |
| Often portable               | Large scanner                  |
| Usually lower cost           | Typically more expensive       |
| Operator-dependent           | Different acquisition workflow |

---

# 49. Important Terms

### Transducer

Device that transmits and receives ultrasound signals.

### Piezoelectric Effect

Conversion between electrical and mechanical energy in the transducer.

### Echo

Reflected ultrasound signal.

### Acoustic Impedance

Property affecting ultrasound wave reflection:

$$
Z = \rho c
$$

### Attenuation

Reduction of ultrasound energy during propagation.

### B-Mode

Brightness-based ultrasound imaging.

### M-Mode

Motion visualization over time.

### Doppler

Analysis of frequency shifts associated with motion.

### Beamforming

Combining signals from multiple transducer elements to form and steer/focus the ultrasound beam.

### Speckle

Granular appearance characteristic of ultrasound images.

---

# 50. Complete Ultrasound Workflow

```text
                 TRANSDUCER
                      │
                      ▼
             ELECTRICAL ENERGY
                      │
                      ▼
            PIEZOELECTRIC EFFECT
                      │
                      ▼
              ULTRASOUND PULSE
                      │
                      ▼
                  PATIENT
                      │
              ┌───────┴────────┐
              ▼                ▼
         TRANSMISSION       REFLECTION
                               │
                               ▼
                              ECHO
                               │
                               ▼
                          TRANSDUCER
                               │
                               ▼
                       ELECTRICAL SIGNAL
                               │
                               ▼
                         BEAMFORMING
                               │
                               ▼
                        SIGNAL PROCESSING
                               │
                               ▼
                         IMAGE FORMATION
                               │
                               ▼
                         ULTRASOUND IMAGE
                               │
                  ┌────────────┼────────────┐
                  ▼            ▼            ▼
                B-MODE       M-MODE       DOPPLER
                               │
                               ▼
                        VISUALIZATION
```

---

# 51. Practice Questions

### Question 1: What does ultrasound imaging use?

**Answer:** High-frequency sound waves.

---

### Question 2: What is the main function of the transducer?

**Answer:** It converts electrical energy into ultrasound waves and converts returning echoes into electrical signals.

---

### Question 3: What is an echo?

**Answer:** A reflected ultrasound wave received by the transducer.

---

### Question 4: How is depth estimated?

$$
d = \frac{ct}{2}
$$

The system uses the round-trip travel time of the ultrasound pulse.

---

### Question 5: What is the piezoelectric effect?

**Answer:** The ability of certain materials to convert electrical energy into mechanical vibration and mechanical vibration into electrical signals.

---

### Question 6: What is B-mode?

**Answer:** Brightness-mode imaging where echo strength is represented by pixel brightness.

---

### Question 7: What is Doppler ultrasound?

**Answer:** Ultrasound imaging that analyzes frequency changes associated with motion, particularly blood flow.

---

### Question 8: What is speckle?

**Answer:** A granular texture characteristic commonly seen in ultrasound images.

---

# 52. Chapter Summary

In **Chapter 37: Ultrasound Imaging**, you learned:

* Ultrasound fundamentals
* Ultrasound waves
* Transducers
* Piezoelectric effect
* Acoustic impedance
* Reflection and echoes
* Depth calculation
* Ultrasound gel
* Frequency and penetration
* Attenuation
* Axial resolution
* Lateral resolution
* Temporal resolution
* A-mode
* B-mode
* M-mode
* Doppler ultrasound
* Color Doppler
* Spectral Doppler
* Ultrasound beam
* Focusing
* Beamforming
* Scan lines
* Image formation
* Acoustic shadowing
* Posterior enhancement
* Reverberation
* Speckle
* Speckle reduction
* Ultrasound segmentation
* 3D ultrasound
* 4D ultrasound
* Clinical applications
* Echocardiography
* Ultrasound DICOM
* Ultrasound processing pipeline
* Comparison with CT and MRI

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
⬜ Chapter 38: Cone Beam CT (CBCT)
⬜ Chapter 39: Mammography
⬜ Chapter 40: Fluoroscopy
```

## Next: **Chapter 38 — Cone Beam CT (CBCT)**
