# 📚 Work 2: Transform Nomenclature

> Documentación del análisis matemático para rotaciones y traslaciones espaciales.

![Homogeneous Transformation Coordinate Frames](image_0.png)

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

## 2) Objetivos

- **General:** Understand the mathematical representation of point rotations in space.
- **Específicos:**
  - Apply the left-multiplication rule for rotations about fixed reference axes.
  - Perform visual analysis of geometric diagrams to extract spatial data.
  - Integrate rotation and translation vectors into a single coordinate frame.

---

## 3) Alcance y Exclusiones

- **Incluye:** Desarrollo de matrices de rotación (3x3) y matrices de transformación homogénea (4x4) para los tres escenarios planteados.
- **No incluye:** La deducción exacta de la matriz de rotación del marco C en el Ejercicio 3, ya que requiere el análisis visual del diagrama geométrico original.

---

## 4) Requisitos

**Software**
- Software para operaciones matriciales como MATLAB o Python con NumPy (Opcional, para comprobación).

**Materiales**
- No materials required.

**Conocimientos previos**
- Álgebra lineal (multiplicación de matrices).
- Regla de multiplicación por la izquierda para marcos fijos.

---

## 5) Análisis y Resultados

### Exercise 1: Multiplicación por la Izquierda

![Successive Rotations around Fixed Axes](image_1.png)

First Rotation in YA with angle = 45 degrees.
Second Rotation in XA with angle = 60 degrees.

1) Matriz Ry(45°):
|  0.707 | 0 |  0.707 |
|  0.000 | 1 |  0.000 |
| -0.707 | 0 |  0.707 |

2) Matriz Rx(60°):
| 1 |  0.000 |  0.000 |
| 0 |  0.500 | -0.866 |
| 0 |  0.866 |  0.500 |

3) Matriz Resultante (R = Rx * Ry):
|  0.707 | 0.000 |  0.707 |
|  0.612 | 0.500 | -0.612 |
| -0.354 | 0.866 |  0.354 |

---

### Exercise 2: Integración de Rotación y Traslación

![Combined Translation and Rotation](image_2.png)

The frame B is rotating relative to A in X with angle = 30 degrees, with ApB origin = [5, 10, 0].

Matriz de transformación final (A_B T):
Se coloca la rotación en X (30°) en el bloque 3x3 y el vector de origen en la última columna.

| 1 |  0.000 |  0.000 |  5 |
| 0 |  0.866 | -0.500 | 10 |
| 0 |  0.500 |  0.866 |  0 |
| 0 |  0.000 |  0.000 |  1 |

---

### Exercise 3: Análisis de Traslaciones Múltiples

![Multiple Reference Frames](image_3.png)

For A_B T we have our origin in ApB origin [3, 0, 0].
Primera matriz de traslación pura (A_B T):

| 1 | 0 | 0 | 3 |
| 0 | 1 | 0 | 0 |
| 0 | 0 | 1 | 0 |
| 0 | 0 | 0 | 1 |

For our second matrix (A_C T), origin of C in [3, 0, 2]. 
(Nota: Se deja expresado Rc hasta extraer los ángulos del diagrama visual).

|    |    |    | 3 |
|    | Rc |    | 0 |
|    |    |    | 2 |
|  0 |  0 |  0 | 1 |