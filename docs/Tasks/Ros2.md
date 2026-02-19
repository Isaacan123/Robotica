# 📚 ROS2 Service Server: Reset Counter

> Documentación de la implementación de un Service Server en ROS2 para reiniciar un contador mediante peticiones booleanas.

---

## 1) Resumen

- **Nombre del proyecto:** ROS2 Service Server (Lab 01)
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica 
- **Fecha:** 19/02/2026
- **Descripción breve:** Implementación de un servidor de servicios (`/reset_counter`) dentro de un nodo contador existente (`number_counter`), permitiendo reiniciar su valor a cero a través de una petición con un tipo de dato `SetBool`.

!!! tip "Consejo"
    Asegúrate de haber compilado (`colcon build`) y cargado el entorno (`source install/setup.bash`) antes de intentar ejecutar el nodo o llamar al servicio desde la terminal.

---

## 2) Objetivos

- **General:** Añadir una funcionalidad para reiniciar el contador a cero en tiempo de ejecución.
- **Específicos:**
  - Crear un *Service Server* dentro del nodo `number_counter` existente.
  - Nombrar el servicio como `/reset_counter`.
  - Utilizar el tipo de interfaz personalizada `pablo_interfaces/srv/SetBool`.
  - Evaluar los datos booleanos del *request*: si es `True`, establecer la variable del contador en 0.
  - Llamar al servicio directamente desde la línea de comandos (CLI).
  - (Opcional/Práctica extra) Crear un nodo personalizado para llamar al servicio `/reset_counter`.

---

## 3) Alcance y Exclusiones

- **Incluye:** Modificación de `package.xml` y `setup.py`, creación del nodo en Python (`pablo_counter`) con publicación/suscripción y servidor de servicios.
- **No incluye:** La creación y compilación desde cero del paquete `pablo_interfaces` (se asume que ya está configurado en el *workspace*).

---

## 4) Requisitos

**Software**
- Sistema Operativo: Ubuntu (compatible con tu distribución de ROS2).
- ROS2 (Humble / Iron / Jazzy, etc.).
- Python 3.x.
- Dependencias clave: `rclpy`, `example_interfaces`, `pablo_interfaces`.

**Materiales**
- No materials required (Solo entorno de simulación/desarrollo en PC).

---

## 5) Configuración del Entorno

### Configuración de `package.xml`
Debemos asegurarnos de que el paquete dependa de las interfaces necesarias.

```xml
<?xml version="1.0"?>
<?xml-model href="[http://download.ros.org/schema/package_format3.xsd](http://download.ros.org/schema/package_format3.xsd)" schematypens="[http://www.w3.org/2001/XMLSchema](http://www.w3.org/2001/XMLSchema)"?>
<package format="3">
  <name>my_robot</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="pablo_pablo_@todo.todo">pablo_pablo_</maintainer>
  <license>TODO: License declaration</license>
  
  <depend>example_interfaces</depend>
  <depend>rclpy</depend>
  <depend>my_robot</depend>
  <depend>pablo_interfaces</depend>

  <test_depend>ament_copyright</test_depend>
  <test_depend>ament_flake8</test_depend>
  <test_depend>ament_pep257</test_depend>
  <test_depend>python3-pytest</test_depend>

  <export>
    <build_type>ament_python</build_type>
  </export>
</package>