# Autonomous-Mobile-Robot
The Autonomous Mobile Robot (AMR) was designed and developed as a multipurpose robot for warehouse applications with a payload capacity of 100 to 120kgs.


![AMR](https://user-images.githubusercontent.com/9202531/217922332-2037ee0f-a8ad-46fb-b295-517001a14436.png)


# Software Requirements
Robot Operating System
<br> Python 3.0

# Hardware Requirements 
Aluminum extrusions 
<br> Suspension System and Couplings
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
<br> Voltage Stabilizer


# Methodology
  <b> Building the Chassis </b>
<br> 1. Disassembled the Hoverboard to extract the Hoverboard wheels , hub motors and the hoverboard programmer.
2. Cut aluminum extrusions according to the required size to build the chassis.
<br> ![image](https://user-images.githubusercontent.com/9202531/217937371-fb2738ce-dc58-4fe0-b3d1-f3ccf253733b.png)

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
  <br> ![image](https://user-images.githubusercontent.com/9202531/217938062-2f36036d-86cf-4129-803a-0b269e3c3fe0.png)

  <br> 2. A simulated map was also created in Fusion 360 and Blender where the Robot model was spawned and mapped.
  <br> ![image](https://user-images.githubusercontent.com/9202531/217937860-42d47ba9-7084-4dc5-9cd1-c15563917f93.png)
  <br> ![image](https://user-images.githubusercontent.com/9202531/217937992-39ab57e4-315a-4e8e-bf58-ec2e5bc21b5e.png)


  <br> 3. The Xacro file in the URDF was changed and plug-ins for the LiDARs and Differential Drive were added.
  <br> 4. The designed map was reopened in Gazebo and was saved as the .world file and  changed in gazebo.launch file were made.
  <br> 5. The Mobile robot was then spawned in the map and the mapping was performed using the Lidars using Gmapping node , thus implementing SLAM in ROS.
  <br> 6. R-Viz was utilized as the visualization software and the map visualized by the Lidars was seen in the environment.
  <br> 7. We needed to make sure that there was no error in the Robot Mdoel and the topic '/scan' data in R-Viz.
  <br> 8. After the map was saved by scanning the Lidar data in Gazrbo, autonomous navigation was performed using the AMCL package. The AMCL package is Adaptive Monte-Carlo Localization algorithm which uses a probablistic approch for localizing the robot and navigating within a known map.
  <br> ![image](https://user-images.githubusercontent.com/9202531/217937591-729f207b-f4b1-41c7-b142-e57ebf4b3a1e.png)

  
 <br> <b> Integrating the Simulation parameters with the manufactured AMR </b>
<br> 1. After the tele-op operation was performed with the Drive Wheel System of the hoverboard, we planned to integrate the actual lidars to get the real-time data.
<br> 2. The real-time date of the lidars were computed using the YD_lidar ROS package.
<br> 3. Since we used two lidars we merged their data together using the IRA_tools_merger and adjusting their TF-models in RViz.
<br> 4. The Mapping of the robot in the environment was done in R-Viz by utilizing the Gmapping node and Cartographer.Using the Google Cartographer algorithm eliminated the use of encoders to localize the robot in the known map. The /front_scan and /back_scan data from both the Lidars were merged to the topic /scan data by publishing data to virtual lidars.
<br> ![image](https://user-images.githubusercontent.com/9202531/217938184-284cf93d-4c2b-486e-b9bc-df78389dc88f.png)

<br> <b> Autonomous Navigation </b>
<br> 1. The navigation stack was implemented on the robot and paramters according to the size of the actual robot was defined.
<br> 2. The Global and Local cost-map parameters were defined for the AMR to know its restrictions for navigating in the map and move from one point to another.
<br> 3. The value of 'sim-time' was changed for adjusting the speed of the robot to reach its destination.
<br> 4. The target-position was defined and the robot was navigating by detecting the static and dynamic obstcles in the map to the final destination.
<br> ![image](https://user-images.githubusercontent.com/9202531/217938546-6e5ae38e-0499-4773-bfb5-f81b96509aab.png)
  
