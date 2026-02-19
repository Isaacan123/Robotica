# 📚 Work 4: Forward Kinematics

> Documentación del análisis cinemático directo para diferentes configuraciones robóticas utilizando la convención Denavit-Hartenberg (DH).

---

## 1) Resumen

- **Nombre del proyecto:** Work 4: Forward Kinematics
- **Equipo / Autor(es):** [Tu Nombre]
- **Curso / Asignatura:** [Nombre de tu materia]
- **Fecha:** 19/02/2026
- **Descripción breve:** Asignación de marcos de coordenadas y extracción de parámetros Denavit-Hartenberg para modelar la estructura cinemática de cinco configuraciones de robots.

!!! tip "Consejo"
    Recuerda los 4 parámetros clásicos de DH para llenar tus tablas: 
    $\theta$ (rotación en Z), $d$ (traslación en Z), $a$ (traslación en X), y $\alpha$ (rotación en X).

---

## 2) Objetivos (Activity Goals)

- **General:** Modelar la cinemática directa de varios manipuladores.
- **Específicos:**
  - Correctly assign coordinate frames to each joint following the DH convention.
  - Identify the four specific parameters for each link.
  - Organize the extracted values into a standard DH parameter table to represent the robot's kinematic structure.

---

## 3) Alcance y Exclusiones

- **Incluye:** Identificación de eslabones (links), articulaciones (joints) y la creación de tablas DH para 5 ejercicios.
- **No incluye:** El cálculo matemático de las matrices de transformación homogénea resultantes o la cinemática inversa.

---

## 4) Requisitos y Materiales

**Materiales**
- No materials required. (Solo análisis analítico y diagramas espaciales).

---

## 5) Análisis (Exercises)



### Exercise 1
- **Descripción:** This exercise only has 2 movements: Prismatic and revolution (RP configuration).
- **Análisis:** Al tener solo dos grados de libertad, la asignación de los ejes Z se limitará al eje de rotación y al eje de traslación lineal.

### Exercise 2

- **Descripción:** For this exercise we have a robot with 3 prismatic movements and the tool.
- **Análisis:** Esta es una configuración puramente cartesiana (PPP). Los ejes Z de los tres marcos de coordenadas apuntarán en las direcciones de los desplazamientos lineales ortogonales.

### Exercise 3
- **Descripción:** For this exercise the robot has more movements than the last one, for that we have more joints, movements and a tool.
- **Análisis:** Al aumentar los grados de libertad, es crucial verificar cuidadosamente la regla de la mano derecha al asignar los ejes X (que deben ser perpendiculares tanto al eje Z actual como al eje Z anterior).

### Exercise 4
- **Descripción:** This exercise is a little confusing because we have movements, joints and tool, for that we can rewrite the robot for do more easy the analysis.
- **Análisis:** Se recomienda dibujar un diagrama esquemático cinemático (esqueleto) simplificado en 2D o 3D antes de intentar colocar los marcos coordenados, para visualizar claramente las distancias $a$ y $d$.

### Exercise 5
- **Descripción:** This exercise is the same to the last one, we have to rewrite the robot for do more easy the analysis.
- **Análisis:** Al igual que en el ejercicio anterior, abstraer el robot físico a un diagrama de líneas y cilindros/prismas facilitará identificar si los ejes se cruzan, son paralelos o se intersecan (lo cual define directamente si los parámetros $a$ o $\alpha$ se vuelven cero).