#  ROS2 Service Server: Reset Counter
#  Lab 01 – ROS2 Service Server Implementation


- **Nombre del proyecto:** ROS 2 publisher–subscriber architecture
- **Equipo / Autor(es):** Isaac Antonio Pérez Alemán
- **Curso / Asignatura:** Applied Robotics
- **Fecha:** 19/02/2026

---

## 1. Activity Goals
- This project sets up a basic ROS 2 publisher–subscriber system with two custom nodes. One node periodically publishes a fixed numeric value, which is received by the second node. The receiving node adds the value to an internal counter and then republishes the updated result.
---

##  2. Materials
- No materials required 

---

##  3. Node

### 1. robot1_publisher Node
In this part, we have to add the necessary dependencies in our `package.xml`:

- This node remains unchanged; it simply outputs a constant integer at regular intervals.

Topic Published: /robot_pub1  
Message Format: example_interfaces/msg/Int64  
Rate of Publication: 1 message each second  
Behavior:  
- Consistently sends the integer value 1  
- Displays every transmitted value in the terminal output  

### 2. number_counter Node
This node listens to the published integer, adds it to an ongoing total, and then republishes the updated sum. In this version, a service has been included: whenever it receives a True signal, the counter is reset to zero.

Subscribed Topic: /robot_pub1  
Published Topic: /robot_pub2  
Message Format: example_interfaces/msg/Int64  
Service Import: from std_srvs.srv import SetBool  

Behavior:  
- Keeps track of an internal counter  
- Increments the counter with each received message  
- Immediately republishes the updated counter value from the subscriber callback  
- Includes a service that, when triggered with a True request, resets the counter back to zero  

```xml
        self.server_= self.create_service(SetBool, #Service TYPE
                                          "reset_counter", #service Name
                                          self.reset_callback
                                          )
        self.get_logger().info("Service Server is ready")

    def reset_callback(self, request, response):
        if request.data:
            self.counter = 0
            self.get_logger().info("Counter reset!")
            response.success = True
            response.message = "Counter reset"
        else:
            response.success = True
            response.message = "Reset not executed"

        return response
```

## Call the Service

Client: The terminal will act as the client.  
Additional Step: In Ubuntu, include this line so the service has a client connection and can reset the counter.  

Purpose:  
- This command enables the service to interact with the terminal as its client.  
- When executed, it allows the counter to be reset through the service call.  


```ros2 service call /reset_counter std_srvs/srv/SetBool "{data: true}"
```
System Overview  

## architecture 
Nodes:  
- robot1_publisher  
- number_counter_code  

Topics:  
- /robot_pub1  
- /robot_pub2  

Service:  
- /reset_counter  

## Node Implementation 
 
 -robot1_publisher 

### Node code 
```
import rclpy
from rclpy.node import Node
from example_interfaces.msg import Int64

# Node that publishes a constant number periodically
class numPublisher(Node):
    def __init__(self):
        super().__init__("robot1_publisher")

        self.publisher_ = self.create_publisher(Int64, "/robot_pub1", 10)
        self.create_timer(1.0, self.number)

        self.number = 1
        self.get_logger().info("Sending number")

    def number(self):
        msg = Int64()
        msg.data = self.number
        self.publisher_.publish(msg)
        self.get_logger().info(f"Published: {msg.data}")

def main(args=None):
    rclpy.init(args=args)
    my_publisher_node = numPublisher()
    rclpy.spin(my_publisher_node)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```

-number_counter 

## Node Code

First, we run the nodes for the publisher and the counter in separate Ubuntu terminals:

```bash
import rclpy
from rclpy.node import Node
from example_interfaces.msg import Int64
from std_srvs.srv import SetBool


class myNode_function(Node):

    def __init__(self):
        super().__init__('number_counter')
        self.counter = 0
        self.get_logger().info('beep Boop R2D2 is operational.')
        self.subscriber = self.create_subscription(Int64, "/robot_pub1", self.callback_receive_info,10) #
        self.publisher = self.create_publisher(Int64, "/robot_pub2", 10)


        self.server_= self.create_service(SetBool, #Service TYPE
                                          "reset_counter", #service Name
                                          self.reset_callback
                                          )
        self.get_logger().info("Service Server is ready")

    def reset_callback(self, request, response):
        if request.data:
            self.counter = 0
            self.get_logger().info("Counter reset!")
            response.success = True
            response.message = "Counter reset"
        else:
            response.success = True
            response.message = "Reset not executed"

        return response

    def callback_receive_info(self, msg: Int64):
        # Add the received number to the counter
        self.counter += msg.data
        # Create a message with the updated counter value
        out_msg = Int64()
        out_msg.data = self.counter
        # Log received and accumulated values
        self.get_logger().info(f"Received: {msg.data} | Counter: {self.counter}" )
         # Publish the updated counter
        self.publisher.publish(out_msg)

def main(args=None):
    rclpy.init(args=args)
    second_code =myNode_function()
    rclpy.spin(second_code)
    rclpy.shutdown()

if __name__ == "__main__":
    main()
```
##  results

![publish](../docs/recursos/imgs/ros2_1.png)
![publish](../docs/recursos/imgs/ros2_2.png)
![publish](../docs/recursos/imgs/ros2_3.png)
![publish](../docs/recursos/imgs/ros2_4.png)
![publish](../docs/recursos/imgs/ros2_5.png)