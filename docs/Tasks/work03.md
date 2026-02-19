# 📚 Work 3: Activity 01 - ROS2 Topics

> Documentación de la implementación del patrón de comunicación Publisher/Subscriber mediante Tópicos en ROS2.

---

## 1) Resumen

- **Nombre del proyecto:** Work 3: Activity 01 - ROS2 Topics
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica
- **Fecha:** 19/02/2026
- **Descripción breve:** Creación de dos nodos en ROS2 (un publicador y un suscriptor) para comprender el flujo de datos unidireccional a través de tópicos, utilizando herramientas CLI para su depuración.

!!! tip "Consejo"
    Puedes utilizar comandos como `ros2 topic list`, `ros2 topic echo /number` y `ros2 topic info /number` en tu terminal para monitorear el flujo de datos entre los nodos en tiempo real sin necesidad de ver el código.

---

## 2) Objetivos

- **General:** Understand the Publisher/Subscriber communication pattern.
- **Específicos:**
  - Create a ROS2 node that publishes messages to a specific topic.
  - Develop a Subscriber node to receive and process data from a topic.
  - Utilize ROS2 CLI tools such as `ros2 topic list`, `echo`, and `info` for debugging.
  - Configure the `package.xml` and `setup.py` (or CMakeLists.txt) files correctly.

---

## 3) Alcance y Exclusiones

- **Incluye:** Desarrollo de un nodo publicador (`NumberPublisher`) que emite números enteros secuenciales y un nodo suscriptor (`NumberCounter`) que los recibe y acumula.
- **No incluye:** La configuración detallada del `package.xml` y `setup.py` en este documento (se asume la estructura estándar del paquete `ament_python`).

---

## 4) Requisitos

**Software**
- Ubuntu con ROS2 instalado.
- Python 3.x
- Dependencias: `rclpy`, `example_interfaces`.

**Materiales**
- No materials required.

---

## 5) Código Fuente

### Nodo Publicador (Publisher)

Este nodo genera un conteo y lo publica en el tópico `/number`. Se ajustó el tipo de mensaje a `Int64` para mantener compatibilidad con el suscriptor.

```python
#!/usr/bin/env python3
import rclpy 
from rclpy.node import Node 
from example_interfaces.msg import Int64

class NumberPublisher(Node): 
    def __init__(self): 
        super().__init__('number_publisher') 
        self.get_logger().info('Beep Boop R2D2 is publishing') 
        self.counter = 0 
        
        # (Message Type, Topic Name, Queue Size)
        self.publisher_ = self.create_publisher(Int64, '/number', 10) 
        self.create_timer(1.0, self.r2d2_number) 

    def r2d2_number(self): 
        msg = Int64() 
        msg.data = self.counter 
        self.get_logger().info(f'R2D2 says number: {msg.data}') 
        self.publisher_.publish(msg) 
        self.counter += 1

def main(args=None): 
    rclpy.init(args=args) 
    r2d2_node = NumberPublisher() 
    rclpy.spin(r2d2_node) 
    rclpy.shutdown() 

if __name__ == "__main__": 
    main()