# Autonomous-Mobile-Robot
The Autonomous Mobile Robot (AMR) was designed and developed as a multipurpose robot for warehouse applications with a payload capacity of 100 to 120kgs.


![AMR](https://user-images.githubusercontent.com/9202531/217922332-2037ee0f-a8ad-46fb-b295-517001a14436.png)

| **Software Requirements**          | **Hardware Requirements**                      | **Electrical Requirements**               |
|-----------------------------------|-----------------------------------------------|------------------------------------------|
| Robot Operating System : Melodic  | Aluminum extrusions                            | Jetson Nano                              |
| Python                            | Suspension System and Couplings                | STM 32 Flash drive                       |
|                                   | Hoverboard Wheels                             | LiDars                                   |
|                                   | 3D printed Castor wheels                       | Hoverboard hub motors                    |
|                                   |                                               | Hoverboard programmer                    |
|                                   |                                               | 32V Li-ion battery                       |
|                                   |                                               | 12V Li-ion battery                       |
|                                   |                                               | Voltage Stabilizer                       |




# Methodology
## Building the Chassis 
1. Disassembled the Hoverboard to extract the Hoverboard wheels , hub motors and the hoverboard programmer.
2. Cut aluminum extrusions according to the required size to build the chassis.
![image](https://user-images.githubusercontent.com/9202531/217937371-fb2738ce-dc58-4fe0-b3d1-f3ccf253733b.png)

## Building the Drive Wheel System </b>
1. Flashed the hoverboard programmer using the STM Link 32.
This was done so that , we could tailor the programmer according to our AMR application.
A hex file was generated using the Platform IO by flashing the Hoverboard Firmware Hack FOC where a USART communication was established between the hoverboard         programmer and the Jetson Nano.
 ![Programmer](https://user-images.githubusercontent.com/9202531/217925351-5c5310a1-10e2-46dd-bf68-82f517d15d3b.png)
    
 2. Connection beteween the Jetson nano and the programmer and running ROS on the microcomputer
     The TX-RX and the GND was connected to the USART pins and the GPIO pins of the Jetson. ROS -melodic was installed and tele-op operations were run using the Hoverboard-driver ROS package.
     Refered : https://github.com/alex-makarov/hoverboard-driver
     The ROS package was installed on the Jetson and the parameters were changed to the pins connected.
     The Teleop keyboard package was launched and the hoverboard wheels were controlled using the keyboard. The max_jerk_limit was defined to zero to stop the jerking effect while controlling the wheels.
     
 ## Simulation in Gazebo </b>
 1. Designed the Robot model in Autdesk Fusion 360 and converted it to the URDF (Universal Robot Description Format ) file which was to be used in Gazebo for simulation.
    
    ![image](https://user-images.githubusercontent.com/9202531/217938062-2f36036d-86cf-4129-803a-0b269e3c3fe0.png)

   2. A simulated map was also created in Fusion 360 and Blender where the Robot model was spawned and mapped.
  ![image](https://user-images.githubusercontent.com/9202531/217937860-42d47ba9-7084-4dc5-9cd1-c15563917f93.png)
   ![image](https://user-images.githubusercontent.com/9202531/217937992-39ab57e4-315a-4e8e-bf58-ec2e5bc21b5e.png)

  3. The Xacro file in the URDF was changed and plug-ins for the LiDARs and Differential Drive were added.
  4. The designed map was reopened in Gazebo and was saved as the .world file and  changed in gazebo.launch file were made.
  5. The Mobile robot was then spawned in the map and the mapping was performed using the Lidars using Gmapping node , thus implementing SLAM in ROS.
  6. R-Viz was utilized as the visualization software and the map visualized by the Lidars was seen in the environment.
  7. We needed to make sure that there was no error in the Robot Mdoel and the topic `/scan` data in R-Viz.
  8. After the map was saved by scanning the Lidar data in Gazrbo, autonomous navigation was performed using the AMCL package. The AMCL package is Adaptive Monte-Carlo Localization algorithm which uses a probablistic approch for localizing the robot and navigating within a known map.

  <br> ![image](https://user-images.githubusercontent.com/9202531/217937591-729f207b-f4b1-41c7-b142-e57ebf4b3a1e.png)

  
 ## Integrating the Simulation parameters with the manufactured AMR </b>
1. We needed to first provide permission to each of the ports on the Jetson for the 2-Lidars and the hoverboard driver using the following cmd:
   ```bash
   sudo chmod 666 /dev/ttyTHS1
   sudo chmod 777 /dev/ttyUSB0
   sudo chmod 777 /dev/ttyUSB1
   ```

2. After the tele-op operation was performed with the Drive Wheel System of the hoverboard, we planned to integrate the actual lidars to get the real-time data.
  ```bash
  rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=/hoverboard_velocity_controller/cmd_vel
  ```

3. The real-time data of the lidars were computed using the YD_lidar ROS package. To run the front and the back lidar we used the following cmd:
   ```bash
   roslaunch tortoisebot_firmware fl.launch
   roslaunch tortoisebot_firmware bl.launch
   ```
4. Since we used two lidars we merged their data together using the IRA_tools_merger and adjusting their TF-models in RViz.The `/front_scan` and `/back_scan` data from both the Lidars were merged to the topic `/scan` data by publishing data to virtual lidars.
```bash
roslaunch ira_laser_tools merge.launch
```
5. The Mapping of the robot in the environment was done in R-Viz by utilizing the Gmapping node and Cartographer.Using the Google Cartographer algorithm eliminated the use of encoders to localize the robot in the known map.
   ```bash
   roslaunch tortoisebot_firmware bringup.launch
   roslaunch tortoisebot_firmware server_bringup.launch
   roslaunch tortoisebot_slam tortoisebot_slam.launch
   ```
![image](https://user-images.githubusercontent.com/9202531/217938184-284cf93d-4c2b-486e-b9bc-df78389dc88f.png)

## Autonomous Navigation </b>
1. The navigation stack was implemented on the robot and paramters according to the size of the actual robot was defined.
2. The Global and Local cost-map parameters were defined for the AMR to know its restrictions for navigating in the map and move from one point to another.
3. The value of `sim-time` was changed for adjusting the speed of the robot to reach its destination.
4. The target-position was defined and the robot was navigating by detecting the static and dynamic obstcles in the map to the final destination.
5. We launch the scanned map and then launch the navigation stack :
```bash
rosrun map_server map_saver -f home_1
roslaunch tortoisebot_navigation tortoisebot_navigation.launch
```
<br> ![image](https://user-images.githubusercontent.com/9202531/217938546-6e5ae38e-0499-4773-bfb5-f81b96509aab.png)
  
