# 📚 Work: Forward Kinematics for KUKA and UR Robots

> Documentación del análisis cinemático directo y obtención de matrices de transformación mediante la convención Denavit-Hartenberg (DH) para manipuladores KUKA y Universal Robots (UR).

---

## 1) Resumen

- **Nombre del proyecto:** Forward Kinematics for KUKA and UR Robots
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica
- **Fecha:** 19/02/2026
- **Descripción breve:** Extracción de parámetros DH y cálculo computacional (vía MATLAB) de la cinemática directa de dos robots industriales de 6 grados de libertad (KUKA y UR).

!!! tip "Consejo"
    Presta especial atención al sentido de giro positivo que define el fabricante para cada articulación. Esto determinará si tus parámetros $\theta_i$ llevan un desfase (como $+ \pi/2$ o $- \pi/2$) en la tabla DH.

---

## 2) Objetivos (Activity Goals)

- **General:** Modelar y calcular la cinemática de los robots KUKA y UR.
- **Específicos:**
  - Correctly assign coordinate frames to each joint following the DH convention.
  - Identify the four specific parameters ($\theta, d, a, \alpha$) for each link.
  - Organize the extracted values into a standard DH parameter table to represent the robot's kinematic structure.
  - Get the DH parameters of each robot.
  - Know the kinematics of each robot.
  - Know how the movement is in each joint of the KUKA and UR robot, looking at how the manufacturer sets each positive turn.

---

## 3) Alcance y Exclusiones

- **Incluye:** Análisis de 6 articulaciones de revoluta, tablas DH abstractas y scripts de MATLAB para multiplicar las matrices de transformación homogénea.
- **No incluye:** Análisis de cinemática inversa o control de trayectorias.

---

## 4) Requisitos y Materiales

**Software**
- MATLAB (Live Scripts recomendados para el cálculo de matrices simbólicas).

**Materiales**
- No materials required.

---

## 5) Análisis y Resultados (Analysis)

### Exercise 1: KUKA Robot



- **Descripción:** This exercise only has 6 movements. All movements are revolution.
- **Metodología:** To make the analysis of the robot easier, we can rewrite (sketch) each joint and link.
- **Cálculo de la Matriz Final:**
  After getting the DH parameters table and the matrix of each link, we can compute the final matrix. For this step, we use a MATLAB Live Script to simplify the calculations.

```matlab
%% CODE MATRIX FINAL - KUKA
syms q0 q1 q2 q3 q4 q5 real

% I define the table [theta, d, a, alpha]
DH = [
    q0,          -420,  240,  pi/2;
    q1 + pi/2,      0, -670,     0;
    q2,             0,    0, -pi/2;
    q3,          -628,    0,  pi/2;
    q4,             0,    0, -pi/2;
    q5,          -135,    0,     0
];

T_final = eye(4);

for i = 1:6
    th = DH(i,1); d = DH(i,2); a = DH(i,3); al = DH(i,4);

    % Matriz de paso i-1 a i
    A = [cos(th), -sin(th)*cos(al),  sin(th)*sin(al), a*cos(th);
         sin(th),  cos(th)*cos(al), -cos(th)*sin(al), a*sin(th);
         0,        sin(al),          cos(al),         d;
         0,        0,                0,               1];

    T_final = simplify(T_final * A);
end

disp('Matriz de Transformación Final T_0_6 (KUKA):');
pretty(T_final)