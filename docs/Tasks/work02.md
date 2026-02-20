# 📚 Work 2: Transform Nomenclature

> Documentación del análisis matemático para rotaciones y traslaciones espaciales.

![Homogeneous Transformation Coordinate Frames]

---

## 1) Resumen

- **Nombre del proyecto:** Work 2: Transform Nomenclature
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica
- **Fecha:** 19/02/2026
- **Descripción breve:** Actividad enfocada en la representación matemática de rotaciones en el espacio, integrando vectores de rotación y traslación en matrices de transformación homogénea.

!!! tip "Consejo"
    Para rotaciones sucesivas sobre ejes de referencia fijos, recuerda aplicar siempre la regla de multiplicación por la izquierda (pre-multiplicación).

---

## 2) Activity Goals

- Understand the mathematical representation of point rotations in space.
  - Apply the left-multiplication rule for rotations about fixed reference axes.
  - Perform visual analysis of geometric diagrams to extract spatial data.
  - Integrate rotation and translation vectors into a single coordinate frame.

---

## 4) Materials

- No materials required.

## 5) Analysis

### Exercise 1:

First Rotation in YA with angle = 45 degrees.

![Successive Rotations around Fixed Axes to 45 degrees](08.jpeg)

Second Rotation in XA with angle = 60 degrees.

2) Matriz Rx(60°):
| 1 |  0.000 |  0.000 |
| 0 |  0.500 | -0.866 |
| 0 |  0.866 |  0.500 |

![Successive Rotations around Fixed Axes to 45 degrees](09.jpeg)

---

### Exercise 2: 

The frame B is rotating relative to A in X with angle = 30 degrees, with ApB origin = [5, 10, 0].

![Combined Translation and Rotation Resultant](10.jpeg)

The rotation in X: we have an angle=30 degrees, we have to make the final matrix 

![Combined Translation and Rotation Resultant](11.jpeg)

---

### Exercise 3: Análisis de Traslaciones Múltiples

For A_B T we have our origin in ApB origin [3, 0, 0].
So for our first translation we have:

![Combined Translation and Rotation Resultant](12.jpeg)

For the second matrix (A_C T), we have to determinate the rotation of C relative to A, we have the origin of C in [3 0 2], so:

![Combined Translation and Rotation Resultant](13.jpeg)
