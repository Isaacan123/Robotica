# Activity 5: Robot Modeling with URDF
- **Team:** Isaac Antonio Pérez Alemán & Carlos Galicia

This activity focuses on replicating **five robots** within a simple 3D environment in Linux, using the **Unified Robot Description Format (URDF)**.  
The main objective is to simulate robot motion by combining three basic geometric shapes:

- **Cylinder**
- **Cube**
- **Sphere**

## URDF Model Features
- Each robot is defined in its **own URDF file**.
- The models include **joints**, **links**, and **motion limits** to achieve realistic simulation.
- A dedicated **launch configuration** is provided for execution.
- **Visualization screenshots** are generated to illustrate robot behavior in the simulated environment.

## Learning Goals
Through this activity, you will:
- Understand how URDF files are structured and organized.
- Explore the relationship between simple geometry and robot motion.
- Learn the importance of joints, links, and constraints in accurate robot simulation.
- Gain experience in modular robot modeling for reproducible workflows.

To launch one of your robots, use the following command in your terminal:

```bash
isaac@Aleman:~/ros2_ws/src/my_robot_example$ ros2 launch urdf_tutorial display.launch.py model:=$HOME/ros2_ws/src/my_robot_example/urdf/my_robot.urdf
```

### Code 1

```
<?xml version="1.0"?>
<robot name="my_robot">
 <link name="base_link">
    <visual>
      <geometry>
        <box size="0.5 0.5 0.1"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <material name="red">
        <color rgba="1 0 0 1"/>
      </material>
    </visual>
  </link>
  
  <joint name="joint" type="fixed">
    <parent link="base_link"/>
    <child link="shoulder_link"/>
    <origin xyz="0 0 0" rpy="0 0 0"/>
  </joint>

  <link name="shoulder_link">
    <visual>
      <geometry>
        <cylinder length="0.06" radius="0.04"/>
      </geometry>
      <origin xyz="0 0 0.1" rpy="0 1.5708 0"/>
      <material name="blue">
        <color rgba="0 0 1 1"/>
      </material>
    </visual> 
  </link>
  
  <joint name="joint_1" type="revolute">
    <parent link="shoulder_link"/>
    <child link="shoulder_extension_link"/>
    <origin xyz="0 0 0.2" rpy="0 0 1.5708"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <link name="shoulder_extension_link">
    <visual>
      <geometry>
        <cylinder length="0.3" radius="0.05"/>
      </geometry>
      <origin xyz="0 0 0.1" rpy="0 0 0"/>
      <material name="green">
        <color rgba="0 1 0 1"/>
      </material>
    </visual>

       <visual>
      <geometry>
        <cylinder length="0.1" radius="0.05"/>
      </geometry>
      <origin xyz="0.1 0 0.2" rpy="0 1.5708 0"/>
      <material name="yellow">
        <color rgba="1 1 0 1"/>
      </material>
    </visual>
  </link>
  
   
  <joint name="joint_2" type="prismatic">
    <parent link="shoulder_extension_link"/>
    <child link="elbow_link"/>
    <origin xyz="0.15 0 0" rpy="0 0 0"/>
    <axis xyz="0 0 1"/>
    <limit lower="-0.15" upper="0.15" effort="10" velocity="1"/>
  </joint>

  <link name="elbow_link">
  
    <visual>
      <geometry>
        <cylinder length="0.06" radius="0.03"/>
      </geometry>
      <origin xyz="0.05 0 0.2" rpy="1.5708 0 0"/>
      <material name="blue">
        <color rgba="1 0 0 1"/>
      </material>
    </visual> 
    <visual>
      <geometry>
        <cylinder length="0.3" radius="0.05"/>
      </geometry>
      <origin xyz="0.05 0 0.2" rpy="0 0 0"/>
      <material name="green">
        <color rgba="0 1 0 1"/>
      </material>
    </visual>
</link>

</robot>
```
![Diagrama del sistema](recursos/imgs/R1.png)

### Code 2

```
<?xml version="1.0"?>
<robot name="my_robot_v2">

  <material name="purple">
    <color rgba="0.5 0 0.5 1"/>
  </material>

  <material name="blue">
    <color rgba="0 0 1 1"/>
  </material>

  <material name="red">
    <color rgba="1 0 0 1"/>
  </material>

  <material name="green">
    <color rgba="0 1 0 1"/>
  </material>

  <material name="yellow">
    <color rgba="1 1 0 1"/>
  </material>

  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.1 0.1 0.2"/>
      </geometry>
      <origin xyz="0 0 0.1" rpy="0 0 0"/>
      <material name="purple"/>
    </visual>
  </link>

  <!-- LINK: shoulder -->
  <link name="shoulder_link">
    <visual>
      <geometry>
        <box size="0.1 0.1 0.2"/>
      </geometry>
      <origin xyz="0 0 0.1" rpy="1.57 0 0"/>
      <material name="blue"/>
    </visual>
  </link>

  <!-- LINK: elbow -->
  <link name="elbow_link">
    <visual>
      <geometry>
        <box size="0.1 0.1 0.2"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 1.57 0"/>
      <material name="pink"/>
    </visual>
  </link>

  <!-- LINK: wrist -->
  <link name="wrist_link">
    <visual>
      <geometry>
        <box size="0.1 0.1 0.2"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <material name="green"/>
    </visual>
  </link>

  <!-- JOINTS -->

  <joint name="base_shoulder_joint" type="prismatic">
    <parent link="base_link"/>
    <child link="shoulder_link"/>
    <origin xyz="0 0 0.2" rpy="0 0 0"/>
    <axis xyz="0 1 0"/>
    <limit lower="0.0" upper="0.2" effort="100" velocity="100"/>
  </joint>

  <joint name="shoulder_elbow_joint" type="prismatic">
    <parent link="shoulder_link"/>
    <child link="elbow_link"/>
    <origin xyz="0 0.2 0.1" rpy="0 0 0"/>
    <axis xyz="1 0 0"/>
    <limit lower="0.0" upper="0.2" effort="100" velocity="100"/>
  </joint>

  <joint name="elbow_wrist_joint" type="prismatic">
    <parent link="elbow_link"/>
    <child link="wrist_link"/>
    <origin xyz="0.2 0.1 0" rpy="0 0 0"/>
    <axis xyz="0 0 1"/>
    <limit lower="-0.18" upper="0.18" effort="100" velocity="100"/>
  </joint>
</robot>
```

![Diagrama del sistema](recursos/imgs/R2.png)

### Code 3

```
<?xml version="1.0"?>
<robot name="my_robot_v3">

  <material name="blue">
    <color rgba="0 0 1 1"/>
  </material>

  <material name="green">
    <color rgba="0 1 0 1"/>
  </material>

  <material name="gray">
    <color rgba="0.6 0.6 0.6 1"/>
  </material>

  <material name="red">
    <color rgba="1 0 0 1"/>
  </material>

  <material name="white">
    <color rgba="1 1 1 1"/>
  </material>

  <material name="yellow">
    <color rgba="1 1 0 1"/>
  </material>

  <joint name="base_rotation_shoulder_joint" type="revolute">
    <origin xyz="0 0 0.1" rpy="0 0 0"/>
    <parent link="base_link"/>
    <child link="rotation_shoulder_link"/>
    <axis xyz="0 0 1"/>
    <limit lower="-2.618" upper="2.618" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: base -->
  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.5 0.4 0.3"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <material name="green"/>
    </visual>
  </link>

  <!-- JOINT: rotation shoulder to shoulder -->
  <joint name="rotation_shoulder_shoulder_joint" type="revolute">
    <origin xyz="0 0 0.1" rpy="0 0 0"/>
    <parent link="rotation_shoulder_link"/>
    <child link="shoulder_link"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: rotation shoulder -->
  <link name="rotation_shoulder_link">
    <visual>
      <geometry>
        <cylinder length="0.01" radius="0.01"/>
      </geometry>
      <origin xyz="0 0 0.005" rpy="0 0 0"/>
      <material name="green"/>
    </visual>
  </link>

  <!-- JOINT: shoulder to elbow -->
  <joint name="shoulder_elbow_joint" type="revolute">
    <origin xyz="0 0 0.75" rpy="0 0 0"/>
    <parent link="shoulder_link"/>
    <child link="elbow_link"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: shoulder -->
  <link name="shoulder_link">
    <visual>
      <geometry>
        <cylinder length="0.75" radius="0.15"/>
      </geometry>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <material name="red"/>
    </visual>
  </link>

  <!-- JOINT: elbow to wrist1 -->
  <joint name="elbow_wrist1_joint" type="revolute">
    <origin xyz="0.75 0 0" rpy="0 0 0"/>
    <parent link="elbow_link"/>
    <child link="wrist_link1"/>
    <axis xyz="1 0 0"/>
    <limit lower="-3.14" upper="3.14" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: elbow -->
  <link name="elbow_link">
    <visual>
      <geometry>
        <cylinder length="0.75" radius="0.1"/>
      </geometry>
      <origin xyz="0.375 0 0" rpy="0 1.5708 0"/>
      <material name="blue"/>
    </visual>
  </link>

  <!-- JOINT: wrist1 to wrist2 -->
  <joint name="wrist1_wrist2_joint" type="revolute">
    <origin xyz="0.3 0 0" rpy="0 0 0"/>
    <parent link="wrist_link1"/>
    <child link="wrist_link2"/>
    <axis xyz="0 0 1"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: wrist1 -->
  <link name="wrist_link1">
    <visual>
      <geometry>
        <cylinder length="0.3" radius="0.08"/>
      </geometry>
      <origin xyz="0.15 0 0" rpy="0 1.5708 0"/>
      <material name="yellow"/>
    </visual>
  </link>

  <!-- JOINT: wrist2 to wrist3 -->
  <joint name="wrist2_wrist3_joint" type="revolute">
    <origin xyz="0.2 0 0" rpy="0 0 0"/>
    <parent link="wrist_link2"/>
    <child link="wrist_link3"/>
    <axis xyz="1 0 0"/>
    <limit lower="-3.14" upper="3.14" effort="10" velocity="1"/>
  </joint>

  <!-- LINK: wrist2 -->
  <link name="wrist_link2">
    <visual>
      <geometry>
        <cylinder length="0.2" radius="0.07"/>
      </geometry>
      <origin xyz="0.1 0 0" rpy="0 1.5708 0"/>
      <material name="gray"/>
    </visual>
  </link>

  <!-- LINK: wrist3 -->
  <link name="wrist_link3">
    <visual>
      <geometry>
        <box size="0.2 0.2 0.2"/>
      </geometry>
      <origin xyz="0.1 0 0" rpy="0 0 0"/>
      <material name="white"/>
    </visual>
  </link>

</robot>
```
![Diagrama del sistema](recursos/imgs/R3.png)

### Code 4

```
<?xml version="1.0"?>

<robot name="4">
        <material name="blue">
            <color rgba = "0 0 1 1" />
        </material>

        <material name="gray">
            <color rgba = "0.5 0.5 0.5 1" />
        </material>

         <material name="red">
            <color rgba = "1 0 0 1" />
        </material>

         <material name="green">
            <color rgba = "0 1 0 1" />
        </material>

    <link name="base_link">
        <visual>
            <geometry>
                <box size="0.4 0.4 0.2" />
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <material name="red"/>
        </visual>
    </link>

    <link name="shoulder_link">
     <visual>
            <geometry>
                <cylinder length="0.3" radius="0.15"/>
            </geometry>
            <origin xyz="0 0 0.1" rpy="0 0 0"/>
            <material name="green"/>
        </visual>
        <visual>
            <geometry>
                <cylinder length="1.0" radius="0.1"/>
            </geometry>
            <origin xyz="0 0 0.5" rpy="0 0 0"/>
        </visual>
    </link>

     <link name="elbow_link">
        <visual>
            <geometry>
                <cylinder length="0.2" radius="0.1"/>
            </geometry>
            <origin xyz="0 0 0.0" rpy="1.57 0 0"/>
            <material name="blue"/>
        </visual>
    </link>

    <link name="wrist_link">
        <visual>
            <geometry>
                <box size="0.7 0.3 0.2" />
            </geometry>
            <origin xyz="0 0 0" rpy="0 0 0"/>
            <material name="green"/>
        </visual>
    </link>

      <link name="finger_link">
        <visual>
            <geometry>
                <cylinder length="0.2" radius="0.1"/>
            </geometry>
            <origin xyz="0 -0.2 0" rpy="1.57 0 0"/>
            <material name="blue"/>
        </visual>
    </link>

       <link name="nail_link">
        <visual>
            <geometry>
                <box size="0.7 0.07 0.2" />
            </geometry>
            <origin xyz="0.3 -0.2 0" rpy="0 0 0"/>
            <material name="green"/>
        </visual>
    </link>

    <link name="nail1_link">
        <visual>
            <geometry>
                <cylinder length="0.2" radius="0.1"/>
            </geometry>
            <origin xyz="0 0.155 0" rpy="0 1.57 0"/>
            <material name="blue"/>
        </visual>
    </link>


    <link name="nail2_link">
        <visual>
            <geometry>
                <cylinder length="0.3" radius="0.1"/>
            </geometry>
            <origin xyz="0 0.155 0" rpy="1.57 0 0"/>
            <material name="red"/>
        </visual>
    </link>

      <link name="nail3_link">
        <visual>
            <geometry>
                <cylinder length="0.4" radius="0.1"/>
            </geometry>
            <origin xyz="0 0.155 -0.2" rpy="0 0 0"/>
            <material name="gray"/>
        </visual>
    </link>

     <link name="tip_link"> 
    </link>


   <joint name="base_shoulder_joint" type="revolute">
        <origin xyz="0 0 0.1" rpy="0 0 0"/>
        <parent link="base_link"/>
        <child link= "shoulder_link"/>
        <axis xyz="0 0 1"/>
        <limit lower="-1.57" upper="1.57" effort="100" velocity="100"/>
    </joint>

    <joint name="shoulder_elbow_joint" type="revolute">
        <origin xyz="0 0 1.1" rpy="0 0 0"/>
        <parent link="shoulder_link"/>
        <child link= "elbow_link"/>
        <axis xyz="0 1 0"/>
        <limit lower="0.0" upper="0.4" effort="100" velocity="100"/>
    </joint>

       <joint name="elbow_wrist_joint" type="fixed">
        <parent link="elbow_link"/>
        <child link="wrist_link"/>
        <origin xyz="0.45 0 0.0" rpy="0 0 0"/>
    </joint> 

    <joint name="wirst_finger_joint" type="revolute">
        <origin xyz="0.2 0 0" rpy="0 0 0"/>
        <parent link="wrist_link"/>
        <child link= "finger_link"/>
        <axis xyz="0 1 0"/>
        <limit lower="0.0" upper="0.4" effort="100" velocity="100"/>
    </joint>

      <joint name="finger_nailt_joint" type="fixed">
        <parent link="finger_link"/>
        <child link="nail_link"/>
        <origin xyz="0 0 0.0" rpy="0 0 0"/>
    </joint> 

     <joint name="nail_nail1_joint" type="revolute">
        <origin xyz="0.7 -0.35 0" rpy="0 0 0"/>
        <parent link="nail_link"/>
        <child link= "nail1_link"/>
        <axis xyz="1 0 0"/>
        <limit lower="0.0" upper="0.4" effort="100" velocity="100"/>
    </joint>

     <joint name="nail1_nail2_joint" type="revolute">
        <origin xyz="0.15 0 0" rpy="0 0 0"/>
        <parent link="nail1_link"/>
        <child link= "nail2_link"/>
        <axis xyz="0 1 0"/>
        <limit lower="0.0" upper="0.4" effort="100" velocity="100"/>
    </joint>

     <joint name="nail2_nail3_joint" type="revolute">
        <origin xyz="0 0 0" rpy="0 0 0"/>
        <parent link="nail2_link"/>
        <child link= "nail3_link"/>
        <axis xyz="0 0 1"/>
        <limit lower="0.0" upper="0.4" effort="100" velocity="100"/>
    </joint>

         <joint name="nail3_tip_joint" type="fixed">
        <parent link="nail3_link"/>
        <child link="tip_link"/>
        <origin xyz="0 0 0.0" rpy="0 0 0"/>
    </joint> 

</robot>

```

![Diagrama del sistema](recursos/imgs/R4.png)

### Code 5

```
<?xml version="1.0"?>
<robot name="my_robot">

    <material name="gray">
        <color rgba="0.6 0.6 0.6 1"/>
      </material>

         <material name="blue">
          <color rgba="0 0 1 1"/>
         </material>

         <material name="red">
        <color rgba="1 0 0 1"/>
      </material>

       <material name="green">
        <color rgba="0 1 0 1"/>
      </material>
      
  <link name="base_link">
    <visual>
      <geometry>
        <box size="1 1 0.2"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <material name="blue">
        <color rgba="0 0 1 1"/>
      </material>
    </visual>
  </link>

  <link name="rotation_shoulder_link">
    <visual>
      <geometry>
        <cylinder length="0.1" radius="0.3"/>
      </geometry>
      <origin xyz="0 0 0.005" rpy="0 0 0"/>
      <material name="green">
      </material>
    </visual>
  </link>

  <link name="shoulder_link">
    <visual>
      <geometry>
        <cylinder length="0.75" radius="0.2"/>
      </geometry>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <material name="red"/>
    </visual>
  </link>


  <link name="elbow_link">
  <visual>
      <geometry>
        <cylinder length="0.2" radius="0.2"/>
      </geometry>
      <origin xyz="0 0 0" rpy="1.5708 0 0"/>
      <material name="green"/>
    </visual>
    <visual>
      <geometry>
        <cylinder length="0.75" radius="0.1"/>
      </geometry>
      <origin xyz="0.375 0 0" rpy="0 1.5708 0"/>
      <material name="red"/>
    </visual>
  </link>


  <link name="wrist_link1">
    <visual>
      <geometry>
        <cylinder length="0.3" radius="0.08"/>
      </geometry>
      <origin xyz="0.15 0 0" rpy="0 1.5708 0"/>
      <material name="green"/>
    </visual>
  </link>


  <link name="wrist_link2">
 <visual>
      <geometry>
        <cylinder length="0.1" radius="0.1"/>
      </geometry>
      <origin xyz="0 0 0" rpy="1.5708 0 0"/>
      <material name="red"/>
    </visual>
    <visual>
      <geometry>
        <cylinder length="0.2" radius="0.07"/>
      </geometry>
      <origin xyz="0.1 0 0" rpy="0 1.5708 0"/>
      <material name="gray"/>
    </visual>
  </link>


  <link name="wrist_link3">
    <visual>
      <geometry>
        <sphere radius="0.08"/>
      </geometry>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <material name="red">
        <color rgba="1 0 0 1"/>
      </material>
    </visual>
  </link>



  <joint name="base_rotation_shoulder_joint" type="revolute">
    <origin xyz="0 0 0.1" rpy="0 0 0"/>
    <parent link="base_link"/>
    <child link="rotation_shoulder_link"/>
    <axis xyz="0 0 1"/>
    <limit lower="-2.618" upper="2.618" effort="10" velocity="1"/>
  </joint>

  <joint name="rotation_shoulder_shoulder_joint" type="revolute">
    <origin xyz="0 0 0.1" rpy="0 0 0"/>
    <parent link="rotation_shoulder_link"/>
    <child link="shoulder_link"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <joint name="shoulder_elbow_joint" type="revolute">
    <origin xyz="0 0 0.75" rpy="0 0 0"/>
    <parent link="shoulder_link"/>
    <child link="elbow_link"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <joint name="elbow_wrist1_joint" type="revolute">
    <origin xyz="0.75 0 0" rpy="0 0 0"/>
    <parent link="elbow_link"/>
    <child link="wrist_link1"/>
    <axis xyz="1 0 0"/>
    <limit lower="-3.14" upper="3.14" effort="10" velocity="1"/>
  </joint>

  <joint name="wrist1_wrist2_joint" type="revolute">
    <origin xyz="0.3 0 0" rpy="0 0 0"/>
    <parent link="wrist_link1"/>
    <child link="wrist_link2"/>
    <axis xyz="0 1 0"/>
    <limit lower="-1.57" upper="1.57" effort="10" velocity="1"/>
  </joint>

  <joint name="wrist2_wrist3_joint" type="revolute">
    <origin xyz="0.2 0 0" rpy="0 0 0"/>
    <parent link="wrist_link2"/>
    <child link="wrist_link3"/>
    <axis xyz="1 0 0"/>
    <limit lower="-3.14" upper="3.14" effort="10" velocity="1"/>
  </joint>

</robot>
```
![Diagrama del sistema](recursos/imgs/R5.png)
