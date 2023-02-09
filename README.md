# Autonomous-Mobile-Robot
The Autonomous Mobile Robot (AMR) was designed and developed as a multipurpose robot for warehouse applications with a payload capacity of 100 to 120kgs.


![AMR](https://user-images.githubusercontent.com/9202531/217922332-2037ee0f-a8ad-46fb-b295-517001a14436.png)


# Software Requirements
Robot Operating System
<br> Python 3.0

# Hardware Requirements 
Aluminum extrusions
<br> Suspension System 
<br> Hoverboard Wheels 
<br> 3D printed Castor wheels

# Electrical Requirements
Jetson Nano
<br> STM 32 Flash drive
<br> LiDars
<br> Hoverboard hub motors - BLDC motor
<br> Hoverboard programmer
<br> 32V- Li-ion battery
<br> 12V Li-ion battery


# Methodology
  <b> Building the Chassis </b>
1. Disassembled the Hoverboard to extract the Hoverboard wheels , hub motors and the hoverboard programmer.
2. Cut aluminum extrusions according to the required size to build the chassis.

  <b>Building the Drive Wheel System </b>
 <br> 1.Flashed the hoverboard programmer using the STM Link 32.
    This was done so that , we could tailor the programmer according to our AMR application.
    A hex file was generated using the Platform IO by flashing the Hoverboard Firmware Hack FOC where a USART communication was established between the hoverboard         programmer and the Jetson Nano.
    ![Programmer](https://user-images.githubusercontent.com/9202531/217925351-5c5310a1-10e2-46dd-bf68-82f517d15d3b.png)
    
 <br> 2.Connection beteween the Jetson nano and the programmer and running ROS on the microcomputer
     The TX-RX and the GND was connected to the USART pins and the GPIO pins of the Jetson. ROS -melodic was installed and tele-op operations were run using the Hoverboard-driver ROS package.
     Refered : https://github.com/alex-makarov/hoverboard-driver
     The ROS package was installed on the Jetson and the parameters were changed to the pins connected.
     The Teleop keyboard package was launched and the hoverboard wheels were controlled using the keyboard. The max_jerk_limit was defined to zero to stop the jerking effect while controlling the wheels.
     
 <br> <b> Simulation in Gazebo </b>
  <br> 1.Designed the Robot model in Autdesk Fusion 360 and converted it to the URDF (Universal Robot Description Format ) file which was to be used in Gazebo for simulation.
  <br> 2. A simulated map was also created in Fusion 360 and Blender where the Robot model was spawned and mapped.
  <br> 3. The Xacro file in the URDF was changed and plug-ins for the LiDARs and Differential Drive were added.
  <br> 4. The designed map was reopened in Gazebo and was saved as the .world file and  changed in gazebo.launch file were made.
  <br> 5. The Mobile robot was then spawned in the map and the mapping was performed using the Lidars using Gmapping node , thus implementing SLAM in ROS.
  <br> 6. R-Viz was utilized as the visualization software and the map visualized by the Lidars was seen in the environment.
  <br> 7. We needed to make sure that there was no error in the Robot Mdoel and the topic '/scan' data in R-Viz.
  <br> 8. After the map was saved by scanning the Lidar data in Gazrbo, autonomous navigation was performed using the AMCL package. The AMCL package is Adaptive Monte-Carlo Localization algorithm which uses a probablistic approch for localizing the robot and navigating within a known map.
  
 <br> <b> Integrating the Simulation parameters with the manufactured AMR </b>

