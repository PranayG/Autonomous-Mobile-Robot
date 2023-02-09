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
<br> LiDars
<br> Hoverboard hub motors - BLDC
<br> Hoverboard programmer
<br> STM 32 Flash drive

# Methodology
  <b> Building the Chassis </b>
1. Disassembled the Hoverboard to extract the Hoverboard wheels , hub motors and the hoverboard programmer.
2. Cut aluminum extrusion according to the required size to build the chassis.

  <b>Building the Drive Wheel System </b>
 <br> 1.Flashed the hoverboard programmer using the STM Link 32.
    This was done so that , we could tailor the programmer according to our AMR application.
    A hex file was generated using the Platform IO by flashing the Hoverboard Firmware Hack FOC where a USART communication was established between the hoverboard         programmer and the Jetson Nano.
    ![Programmer](https://user-images.githubusercontent.com/9202531/217925351-5c5310a1-10e2-46dd-bf68-82f517d15d3b.png)
    
 <br> 2.



