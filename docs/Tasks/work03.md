# 📚 Work 3: Activity 01 - ROS2 Topics

> Documentación de la implementación del patrón de comunicación Publisher/Subscriber mediante Tópicos en ROS2.

---

## 1) Resumen

- **Nombre del proyecto:** Work 3: Activity 01 - ROS2 Topics
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Ingeniería Mecatrónica
- **Fecha:** 19/02/2026


## 2) Activity Goals

-  Understand the Publisher/Subscriber      communication pattern.
  - Create a ROS2 node that publishes messages to a specific topic.
  - Develop a Subscriber node to receive and process data from a topic.
  - Utilize ROS2 CLI tools such as `ros2 topic list`, `echo`, and `info` for debugging.
  - Configure the `package.xml` and `setup.py` (or CMakeLists.txt) files correctly.

---

## 3) Materiales
- No materials required.

## 4) Code
### Publisher node


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

### Listener node
import rclpy
 from rclpy.node
  import Node from example_interfaces.
   import Int64

class NumberCounter(Node):def _init_(self):
    super()._init_('number_counter')
    self.counter = 0
    self.subscription = self.create_subscription(
        Int64,
        '/number',
        self.listener_callback,
        10
    )
    self.publisher_ = self.create_publisher(
        Int64,
        '/number_count',
        10
    )
    self.get_logger().info('Number counter working...')

def listener_callback(self, msg):
    self.counter += msg.data
    out_msg = Int64()
    out_msg.data = self.counter
    self.publisher_.publish(out_msg)
    self.get_logger().info(
        f'Recibido: {msg.data} | Total: {self.counter}'
    )
    def main(args=None):
     rclpy.init(args=args) 
     node = NumberCounter() rclpy.spin(nodito) rclpy.shutdown()

if name == 'main': main() with the two codes, we have our program:

![Successive Rotations around Fixed Axes to 45 degrees](14.png)
