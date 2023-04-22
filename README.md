All files in this repository came from https://automaticaddison.com/how-to-create-a-simulated-mobile-robot-in-ros-2-using-urdf/  
The purpose of this repository is to write down notes while learning ROS2.

Required packages
- Nav2
- joint-state-publisher-gui
- xacro

# Background
A .urdf file represents a robot model using joints and links.
- joint: the skeleton of the robot model
- links: the link that connects the joings

(xml has a html-style format)

# Creating a ROS2 package
Make a folder and a folder named source in it.  
***mkdir ~/dev_ws/src***  
***cd ~/dev_ws/src***  
***ros2 pkg create --build-type ament_cmake basic_mobile_robot***  
- this will make a folder named basic_mobile_robot inside the src folder
- this folder will have
  - CMakeLists.txt
  - package.xml
  - include folder
  - src folder

Then make launch, meshes, models, and rviz folders inside the package.  
***cd ~/dev_ws/src/basic_mobile_robot***  
***mkdir launch meshes rviz***  
- launch folder will have the launch script in Python
- meshes will have the .stl folders, which are like texture files
- rviz will have the rviz launch file
- models will have the .urdf file of the robot model

Go to the root directory of the package and build.  
***cd ~/dev_ws***  
***colcon build***  

# Make the .urdf file
Go to the models folder and make a basic_mobile_bot_v1.urdf file  
***cd ~/dev_ws/src/basic_mobile_robot/models***  
***gedit basic_mobile_bot_v1.urdf***  
In this tutorial, the urdf file is copy-pasted in text format, but it is usually made using visual tools such as Solidworks.  
(the urdf file can be made using a text editor, but only for simple robot models)

Also add the .stl files into the meshes folder.  
This is done to display custom textures for the robot model and is not necessary.  
You can use SolidWorks or Blender to generate the .stl or .dae files to add custom texture.  

Also add the .rviz file into the rviz folder.  
I don't know how you generate this file yet.  

# Add dependencies
Add these lines to the package.xml file

# Make the launch file
***cd ~/dev_ws/src/basic_mobile_robot/launch***  
***gedit basic_mobile_bot_v1.launch.py***  
- launch files in Python end with .launch.py

In short, the launch file sets launch configurations.  
Ex.  

Then adds all the launch configurations to the launch description.

# Building the package
Add these lines to the CMakeLists.txt

***cd ~/dev_ws***  
***colcon build***  

After the building, add these lines to the ~/.bashrc file
***source ~/dev_ws/install/setup.bash***  
- this will source the package everytime the terminal is opened
- sourcing sets the appropriate environment variables

# Displaying the robot in RViz
Open a new terminal and type  
***ros2 launch basic_mobile_robot basic_mobile_bot_v1.launch.py***  
This will bring up the RViz window and the joint_state_publisher_gui. 


The joint_state_publisher_gui is used to change the angle of the motor.  


Checking the "Collision Enabled" checkbox will make a rectangular overlay on the robot's body.  This is like the hitbox of the robot. This box is used to determine the collision instead of the actual robot model to simply the collision simulation in Gazebo.


# Viewing the coordinate transform
***ros2 run tf2_ros tf2_echo base_link front_caster***  
Syntax is: ***ros2 run tf2_ros tf2_echo <parent frame> <child frame>***  


The transform from base_link to front_caster is a static transform because front_caster is fixed in the base_link's coordinate frame. 


The "Translation" says that front_caster is located at point [0.217, 0, -0.1] in base_link's frame. The "Rotation", which is represented in a Quaternion (a way of numerically representing orientation), is all [0, 0, 0, 1], which means that base_link and front_caster have the same orientation.


