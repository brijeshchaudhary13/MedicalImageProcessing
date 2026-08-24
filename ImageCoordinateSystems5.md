# Chapter 5 — Image Coordinate Systems

We now move to **Chapter 5 exactly according to your index**. The required topics are:

1. Pixel coordinates
2. Image coordinates
3. Cartesian coordinates
4. Physical coordinates
5. World coordinates
6. Origin
7. Spacing
8. Direction
9. Orientation

This chapter is especially important for **CT/MRI, DICOM, ITK, VTK, MPR, registration, and 3D visualization**, which come later in your course. 

---

# 5.1 Why Coordinate Systems Matter

Suppose a CT viewer tells you:

> "The tumor is at `(120, 180, 50)`."

That statement is incomplete.

We need to know:

* Is `(120,180,50)` a voxel index?
* Is it measured in millimeters?
* What is the origin?
* What is the voxel spacing?
* Which direction is positive?
* How is the patient oriented?

Therefore:

> **A coordinate is meaningful only when we know the coordinate system it belongs to.**

The basic hierarchy is:

```text id="coor1"
Pixel / Voxel Index
        ↓
Image Coordinates
        ↓
Physical Coordinates
        ↓
World / Patient Coordinates
```

---

# 5.2 Pixel Coordinates

A pixel coordinate identifies a pixel using discrete indices.

For a 2D image:

[
(x,y)
]

or, depending on the programming/library convention:

[
(column,row)
]

For example:

```text id="pcx1"
+----+----+----+----+
|    |    |    |    |
+----+----+----+----+
|    |  X |    |    |
+----+----+----+----+
|    |    |    |    |
+----+----+----+----+
```

The pixel can be identified by its index.

---

# 5.3 Pixel Index vs Physical Position

This distinction is extremely important.

Suppose:

```text id="pco2"
Pixel index:
(100,200)
```

This does **not necessarily mean**:

```text id="pco3"
100 mm, 200 mm
```

It means something like:

> 100 pixels/columns and 200 pixels/rows from the image origin.

The physical position depends on **spacing, origin, and direction**.

---

# 5.4 Image Coordinates

Image coordinates describe positions in the image's own coordinate system.

For a 2D image:

[
(i,j)
]

For a 3D image:

[
(i,j,k)
]

where:

* (i) = first image dimension
* (j) = second image dimension
* (k) = third image dimension

Think:

```text id="imgco"
Image Coordinate
       │
       ├── i
       ├── j
       └── k
```

These are generally **indices**, not physical measurements.

---

# 5.5 2D Image Indexing

Suppose:

[
I=
\begin{bmatrix}
10&20&30\
40&50&60\
70&80&90
\end{bmatrix}
]

The image has:

```text id="idx2"
3 columns
3 rows
```

Conceptually:

```text id="idx3"
       i →
     0   1   2

j 0 10  20  30
↓ 1 40  50  60
  2 70  80  90
```

The exact naming of the axes varies between software libraries, so always check the library's convention.

---

# 5.6 Cartesian Coordinates

The traditional mathematical coordinate system is Cartesian.

For 2D:

```text id="cart1"
          y
          ↑
          │
          │
          │
──────────┼──────────→ x
          │
          │
```

A point is:

[
(x,y)
]

For example:

[
(3,2)
]

means:

```text id="cart2"
x = 3
y = 2
```

---

# 5.7 Image Coordinates vs Cartesian Coordinates

Here is a very important difference.

### Mathematical Cartesian system

Typically:

```text id="cart3"
          ↑ y
          │
          │
──────────┼──────────→ x
          │
```

Positive (y) generally points **up**.

### Typical image indexing

Often:

```text id="img4"
(0,0) ─────────→ x / column
  │
  │
  │
  ↓
  y / row
```

Positive row direction points **down**.

Therefore:

```text id="diff1"
Cartesian:
+Y ↑

Image:
+row ↓
```

This difference causes many bugs in image-processing software.

---

# 5.8 Why This Matters in C++

Suppose you have:

```cpp
image(y, x)
```

but you think of it as:

```cpp
image(x, y)
```

You may accidentally swap the axes.

For example:

```text id="swap1"
Expected:
(x,y)

Actually used:
(y,x)
```

The resulting image may be:

* transposed
* mirrored
* incorrectly measured
* incorrectly transformed

---

# 5.9 Physical Coordinates

Now we move from **pixel index** to **real-world distance**.

Suppose an image has:

```text id="phys1"
Pixel spacing = 0.5 mm
```

Then moving by one pixel corresponds to:

[
0.5\text{ mm}
]

in physical space.

So:

```text id="phys2"
Pixel index
    ↓
Spacing
    ↓
Physical distance
```

---

# 5.10 Simple 2D Physical Coordinate

Suppose:

[
spacing_x=0.5\text{ mm}
]

and:

[
spacing_y=0.5\text{ mm}
]

If a pixel is at:

[
(i,j)=(100,200)
]

and assume for now that the origin is:

[
(0,0)
]

and there is no rotation, then approximately:

[
x=100(0.5)=50\text{ mm}
]

[
y=200(0.5)=100\text{ mm}
]

Therefore:

```text id="phys3"
Pixel:
(100,200)

      ↓ spacing

Physical:
(50 mm,100 mm)
```

This is the basic idea.

---

# 5.11 Origin

The **origin** tells us where the image coordinate system begins in physical/world space.

For example:

[
O=(10,20)
]

means the image origin is physically located at:

```text id="orig1"
x = 10 mm
y = 20 mm
```

If:

[
O=(0,0)
]

then the image starts at the physical coordinate origin.

---

# 5.12 Origin + Spacing

Without direction/orientation for the moment, a simplified formula is:

[
x=O_x+iS_x
]

[
y=O_y+jS_y
]

where:

* (O) = origin
* (S) = spacing
* (i,j) = pixel indices

Example:

[
O_x=10
]

[
S_x=0.5
]

[
i=100
]

Then:

[
x=10+(100)(0.5)
]

[
x=60\text{ mm}
]

---

# 5.13 Why Origin Matters

Imagine two CT images:

```text id="orig2"
Image A
origin = (0,0,0)

Image B
origin = (100,200,50)
```

Both might contain:

```text
512 × 512 × 300
```

and even have identical spacing.

But they don't necessarily represent the same physical region.

Therefore:

```text id="orig3"
Same dimensions
      ≠
Same physical location
```

---

# 5.14 Spacing

Spacing tells us the physical distance between adjacent samples.

For a 3D image:

[
S=(S_x,S_y,S_z)
]

For example:

[
S=(0.5,0.5,1.0)\text{ mm}
]

means:

```text id="space1"
X spacing = 0.5 mm
Y spacing = 0.5 mm
Z spacing = 1.0 mm
```

---

# 5.15 Why Z Spacing Can Differ

In medical imaging, the spacing between slices can differ from the in-plane pixel spacing.

Example:

```text id="space2"
Pixel:
0.5 × 0.5 mm

Slice:
1.0 mm
```

Therefore the voxels are not necessarily perfect cubes.

They may be:

```text id="space3"
     0.5 mm
  ┌─────────┐
  │         │
  │         │ 1.0 mm
  │         │
  └─────────┘
```

Such voxels are called **anisotropic** when their spacing differs by direction.

---

# 5.16 Isotropic vs Anisotropic

### Isotropic

Same spacing in all dimensions:

[
0.5\times0.5\times0.5\text{ mm}
]

```text id="iso1"
X = 0.5
Y = 0.5
Z = 0.5
```

### Anisotropic

Different spacing:

[
0.5\times0.5\times1.0\text{ mm}
]

```text id="iso2"
X = 0.5
Y = 0.5
Z = 1.0
```

This distinction becomes important for:

* 3D visualization
* segmentation
* registration
* resampling
* MPR

---

# 5.17 Direction

Now we reach a more advanced and extremely important concept.

**Direction** tells us how the image axes are oriented in physical/world space.

Imagine a simple image:

```text id="dir1"
Image X →
Image Y ↓
```

If the image is rotated in physical space, the image axes no longer align with the world axes.

So we need to know:

> Which physical direction does each image axis point toward?

---

# 5.18 Direction Matrix

In 2D, direction can be represented by a matrix such as:

[
D=
\begin{bmatrix}
d_{11}&d_{12}\
d_{21}&d_{22}
\end{bmatrix}
]

In 3D:

[
D=
\begin{bmatrix}
d_{11}&d_{12}&d_{13}\
d_{21}&d_{22}&d_{23}\
d_{31}&d_{32}&d_{33}
\end{bmatrix}
]

This matrix describes the orientation of the image axes.

---

# 5.19 Direction Intuition

Imagine a CT volume:

```text id="dir2"
Image axes:

I →
J ↓
K ↓ into volume
```

Now imagine the patient is rotated relative to the scanner.

The image axes may correspond to different physical directions.

So:

```text id="dir3"
Image coordinates
      ↓
Direction matrix
      ↓
Physical coordinates
```

This is why simply multiplying indices by spacing is not always enough.

---

# 5.20 Full Physical Coordinate Formula

For a 3D image, a useful conceptual formula is:

[
\mathbf{p}
==========

\mathbf{o}
+
D
\begin{bmatrix}
iS_x\
jS_y\
kS_z
\end{bmatrix}
]

where:

* (\mathbf{p}) = physical point
* (\mathbf{o}) = origin
* (D) = direction matrix
* (i,j,k) = image indices
* (S_x,S_y,S_z) = spacing

This is one of the most important equations in medical imaging.

---

# 5.21 Let's Break It Down

The equation:

[
\mathbf{p}
==========

\mathbf{o}
+
D
\begin{bmatrix}
iS_x\
jS_y\
kS_z
\end{bmatrix}
]

contains three major transformations.

### Step 1 — Index

```text id="step1"
(i,j,k)
```

### Step 2 — Apply spacing

```text id="step2"
(iSx, jSy, kSz)
```

Now we have physical distances along image axes.

### Step 3 — Apply direction

```text id="step3"
D × distance_vector
```

Now the vector is oriented correctly in physical space.

### Step 4 — Add origin

```text id="step4"
origin + vector
```

Now we obtain the physical position.

---

# 5.22 Simple Example Without Rotation

Suppose:

[
O=
\begin{bmatrix}
10\20\30
\end{bmatrix}
]

spacing:

[
S=(1,2,3)
]

direction:

[
D=I
]

where (I) is the identity matrix.

For:

[
(i,j,k)=(5,4,2)
]

first:

[
(iS_x,jS_y,kS_z)
]

becomes:

[
(5,8,6)
]

Then:

[
P=O+(5,8,6)
]

so:

[
P=(15,28,36)
]

Therefore:

```text id="exmp1"
Index:
(5,4,2)

        ↓ spacing

(5,8,6)

        ↓ origin

(15,28,36) mm
```

---

# 5.23 Orientation

Orientation describes how an image or patient is positioned relative to a reference coordinate system.

In medical imaging, orientation is extremely important.

For example, you may encounter concepts such as:

* Left
* Right
* Anterior
* Posterior
* Superior
* Inferior

These describe anatomical directions.

---

# 5.24 Why Orientation Matters

Imagine displaying a brain MRI.

If orientation is interpreted incorrectly, you could accidentally display:

```text id="ori1"
LEFT ↔ RIGHT
```

or:

```text id="ori2"
SUPERIOR ↔ INFERIOR
```

That is not a cosmetic issue.

In medical software, incorrect orientation can have serious consequences.

Therefore:

> **Medical imaging software must preserve and correctly interpret spatial orientation.**

---

# 5.25 Patient/World Coordinates

A world or physical coordinate system allows us to describe a point in a common spatial reference frame.

Instead of saying:

```text id="world1"
Voxel (100,200,50)
```

we can describe its physical location:

```text id="world2"
(physical X,
 physical Y,
 physical Z)
```

This allows different images to be compared spatially.

---

# 5.26 Why World Coordinates Are Important

Suppose we have:

```text id="world3"
CT
+
MRI
```

Their voxel grids may be completely different.

For example:

```text id="world4"
CT:
512 × 512 × 300
spacing = 0.5 × 0.5 × 1.0

MRI:
256 × 256 × 180
spacing = 1.0 × 1.0 × 1.0
```

You cannot simply compare:

```text id="world5"
CT voxel (100,100,100)
```

with:

```text id="world6"
MRI voxel (100,100,100)
```

because those indices don't necessarily represent the same physical location.

You need spatial coordinates.

---

# 5.27 Registration Connection

This is the foundation of image registration.

Suppose:

```text id="reg1"
CT
 │
 └── physical coordinates

MRI
 │
 └── physical coordinates
```

Registration attempts to find a transformation such that corresponding anatomy aligns in a common coordinate system.

Conceptually:

```text id="reg2"
CT
 ↓
Physical space
 ↑
MRI
```

This is why coordinate systems are fundamental to Chapter 39 later in your course.

---

# 5.28 MPR Connection

MPR means:

> **Multiplanar Reconstruction**

Suppose we have a 3D CT volume:

```text id="mpr1"
       3D Volume
           │
    ┌──────┼──────┐
    ↓      ↓      ↓
 Axial  Sagittal Coronal
```

To correctly generate these planes, the software must understand:

* voxel spacing
* orientation
* physical coordinates
* direction

Without correct spatial information, MPR can be geometrically wrong.

---

# 5.29 VTK / ITK Connection

Later you will work with libraries such as ITK and VTK.

These libraries have concepts for:

```text id="lib1"
Image
 ├── Origin
 ├── Spacing
 ├── Direction
 └── Dimensions
```

This is not accidental.

They are representing the geometry of the medical image.

---

# 5.30 DICOM Connection

Later, when we study DICOM, you will encounter metadata that describes spatial relationships.

For example, DICOM can contain information about:

* image position
* image orientation
* pixel spacing
* slice-related geometry

We will study the exact DICOM tags later in the appropriate chapters.

For now, understand the principle:

```text id="dicom1"
DICOM
  ↓
Spatial Metadata
  ↓
Image Geometry
  ↓
Correct Physical Position
```

---

# 5.31 Coordinate Conversion

The fundamental workflow is:

```text id="conv1"
Image Index
(i,j,k)
    ↓
Apply spacing
    ↓
Physical distance
    ↓
Apply direction
    ↓
Oriented physical vector
    ↓
Add origin
    ↓
Physical coordinate
```

Mathematically:

[
\boxed{
P=O+D
\begin{bmatrix}
iS_x\
jS_y\
kS_z
\end{bmatrix}
}
]

Remember this equation.

---

# 5.32 Reverse Conversion

Sometimes we have a physical coordinate and want to know:

> Which voxel corresponds to this physical point?

Conceptually:

```text id="rev1"
Physical coordinate
       ↓
Remove origin
       ↓
Undo direction
       ↓
Divide by spacing
       ↓
Image index
```

This is the reverse mapping.

It becomes important for:

* mouse interaction
* measurements
* annotation
* segmentation
* picking
* registration
* 3D visualization

---

# 5.33 Example: Mouse Click in a Medical Viewer

Imagine the user clicks:

```text id="mouse1"
Screen coordinate
      ↓
(400,300)
```

The viewer needs to determine:

```text id="mouse2"
Which image pixel?
      ↓
Which physical point?
      ↓
Which anatomical location?
```

The chain is roughly:

```text id="mouse3"
Screen
 ↓
Image coordinates
 ↓
Voxel index
 ↓
Physical coordinates
 ↓
Anatomical/world location
```

This is a real feature in medical imaging software.

---

# 5.34 Coordinate Systems Summary

Let's compare them.

| Coordinate System     | Represents                     | Typical Units         |
| --------------------- | ------------------------------ | --------------------- |
| Pixel/Voxel Index     | Discrete array location        | index                 |
| Image Coordinates     | Image grid position            | index                 |
| Cartesian Coordinates | Mathematical position          | arbitrary             |
| Physical Coordinates  | Real spatial position          | mm                    |
| World Coordinates     | Common/reference spatial frame | mm                    |
| Origin                | Reference starting point       | mm                    |
| Spacing               | Distance between samples       | mm                    |
| Direction             | Axis orientation               | dimensionless matrix  |
| Orientation           | Anatomical/spatial positioning | anatomical directions |

---

# 5.35 The Most Important Difference

Never confuse:

```text id="important1"
Voxel index
```

with:

```text id="important2"
Physical coordinate
```

For example:

```text id="important3"
Voxel:
(100,200,50)
```

does **not** necessarily mean:

```text id="important4"
(100 mm,200 mm,50 mm)
```

The conversion requires:

```text id="important5"
Index
+
Spacing
+
Direction
+
Origin
```

---

# 5.36 C++ Example

A simplified conversion without direction:

```cpp id="cppcoord"
#include <iostream>

int main()
{
    double originX = 10.0;
    double originY = 20.0;

    double spacingX = 0.5;
    double spacingY = 0.5;

    int i = 100;
    int j = 200;

    double x = originX + i * spacingX;
    double y = originY + j * spacingY;

    std::cout << "Physical X = " << x << " mm\n";
    std::cout << "Physical Y = " << y << " mm\n";

    return 0;
}
```

Result:

```text id="cppres"
Physical X = 60 mm
Physical Y = 120 mm
```

This is a simplified case where direction is identity.

---

# 5.37 Python Example

```python id="pycoord"
origin = (10.0, 20.0)
spacing = (0.5, 0.5)

i = 100
j = 200

x = origin[0] + i * spacing[0]
y = origin[1] + j * spacing[1]

print(x, y)
```

Output:

```text id="pyres"
60.0 120.0
```

---

# 5.38 Common Coordinate-System Bugs

These are extremely common in medical imaging software:

### 1. X/Y swapped

```text id="bug1"
(x,y)
 ↓
(y,x)
```

### 2. Wrong origin

```text id="bug2"
Correct origin
      ↓
wrong physical location
```

### 3. Ignoring spacing

```text id="bug3"
Voxel index
      ↓
incorrect physical measurement
```

### 4. Ignoring direction

```text id="bug4"
Correct dimensions
      +
wrong orientation
      ↓
wrong anatomy
```

### 5. Mixing coordinate systems

```text id="bug5"
Screen coordinates
      ↓
treated as physical coordinates
```

### 6. Assuming isotropic voxels

```text id="bug6"
0.5 × 0.5 × 1.0
```

incorrectly treated as:

```text id="bug7"
0.5 × 0.5 × 0.5
```

These mistakes can affect measurements, MPR, segmentation, and registration.

---

# 5.39 Chapter 5 Mental Model

Memorize this:

```text id="mental5"
                MEDICAL IMAGE
                     │
                     ↓
               Voxel Index
                (i,j,k)
                     │
                     ↓
                 Spacing
               (Sx,Sy,Sz)
                     │
                     ↓
                 Direction
                     │
                     ↓
                  Origin
                     │
                     ↓
           Physical Coordinates
                  (X,Y,Z)
                     │
                     ↓
            World/Patient Space
```

And the central equation:

[
\boxed{
P=O+D
\begin{bmatrix}
iS_x\
jS_y\
kS_z
\end{bmatrix}
}
]

---

# Chapter 5 — Knowledge Check

### Fundamentals

1. What is a pixel coordinate?
2. What is a voxel index?
3. What is the difference between image coordinates and Cartesian coordinates?
4. Why can image (Y) direction differ from mathematical Cartesian (Y)?
5. What are physical coordinates?
6. What are world coordinates?

### Geometry

7. What is the origin?
8. What is spacing?
9. What is direction?
10. What is orientation?
11. What is the difference between isotropic and anisotropic spacing?

### Medical imaging

12. Why is spacing important for CT measurements?
13. Why is direction important for medical image orientation?
14. Why can't you directly compare voxel `(100,100,100)` from two different images?
15. Why are physical coordinates important for image registration?
16. Why are coordinate systems important for MPR?

### Formula

17. Explain:

[
P=O+D
\begin{bmatrix}
iS_x\
jS_y\
kS_z
\end{bmatrix}
]

18. What does each term represent?

### Numerical Exercise

Given:

[
O=(10,20,30)
]

[
S=(0.5,0.5,1.0)
]

[
D=I
]

and voxel:

[
(i,j,k)=(100,200,50)
]

calculate the physical coordinate.

You should calculate:

[
x=10+100(0.5)
]

[
y=20+200(0.5)
]

[
z=30+50(1.0)
]

Then explain why the result is **not** simply `(100,200,50) mm`.

---

## Mini Practical Exercise

Imagine you are developing a CT viewer.

The user clicks on voxel:

```text
(250,300,100)
```

The CT has:

```text
Spacing = 0.6 × 0.6 × 1.2 mm
Origin  = (20,30,40) mm
```

Assume identity direction.

Calculate the physical location.

Then answer:

> If you forgot the spacing and used `(250,300,100)` directly as millimeters, what kind of error would you introduce?

---

**Chapter 5 is complete.**

The next chapter in your exact index is:

# Chapter 6 — Image Intensity

Topics:

* Intensity meaning
* Brightness
* Contrast
* Intensity transformations
* Linear transformations
* Gamma correction
* Log transform
* Exponential transform
* Windowing
* Level
* Contrast stretching
* Histogram-based intensity operations.
