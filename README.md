# tb3_slam_simple
For the final project, the plan was to build a new world in gazebo that shape like a maze. Thus, this project is a simulation using turtlebot3 in gazebo and rviz. Using one of the robot model in turtlebot3, burger, it will scan the maze using lidar and try to recreate the map.

The world and the rviz will be on a different launch file. Therefore, the world can be changed with any different world sample `turtlebot3_world`, `turtlebot3 _house` or using self made world. Also, the world can be used for other different purposes.

For starters, the required packages are : 
    
* ROS2  
    In this project, ros2 foxy will be used. The steps for how to install can be followed on ROS2 documentation (https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html).
      
* Turtlebot3    
    The installation of the turtlebot3 can be installed using
    ```
    $ sudo apt install ros-foxy-turtlebot3* 
    ```
    in the terminal. However, it is recommended to use source build for the ease of add, edit, and modify. The steps of source building can be followed in e-manual ROBOTIS.
      Getting started : https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/
      Turtlebot3 installation : https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/#gazebo-simulation.
* Gazebo, Nav2 Bring-Up, and SLAM Toolbox   
    Gazebo is usually automatically installed if using the steps in the ROS2 documentation. However, it can be installed using
    ```
    $ sudo apt-get install ros-foxy- gazebo-*
    ```
    For this project, the gazebo is gazebo-11.
      
    Nav2 is also usually already installed on the installing process of Turtlebot3. For manual installation, 
    ```
    $ sudo apt install ros-foxy-navigation2 && sudo apt install ros-foxy-nav2-bringup
    ```
    
    SLAM Toolbox is also automatically installed if using the steps in the ROS2 documentation. However, it can be installed using 
    ```
    $ sudo apt install ros-foxy-slam-toolbox
    ```

After all the requirements have been fulfilled, then the project can be compiled finely. For the package can be download in my GitHub repository (https://github.com/MarcAxel/tb3_slam_simple). 

The steps to install:
1. Open terminal.
2. If turtlebot3 are installed using source, then the folder `turtlebot3_ws` will be existed. If not, type 
    ```
    $ mkdir turtlebot3_ws/src
    ```
    in terminal to create one.
3. Open the `turtlebot3_ws` folder and add the `tb3_slam_simple` to the `src` folder. 
	```
    $ cd ~/turtlebot3_ws/src/ && git clone https://github.com/MarcAxel/tb3_slam_simple.git
    ```
4. Turn back to ‘turtlebot3’ and build the file.
	```
    $ cd ~/turtlebot3_ws && colcon build --symlink-install
    ```
5. After the build finished, source the file to the bashrc.
	```
    $ echo "source ~/turtlebot3_ws/install/setup.bash\" >> ~/.bashrc
    ```
6. Because in this project, the model will be only burger robot, then the model can be input to bashrc as a default model
	```
    $ echo 'export TURTLEBOT3_MODEL=burger' >> ~/.bashrc
    ```
7. If there are error while compiling turtlebot3, type :
    ```
    $ echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
    ```
>REMINDER : It will work for simulation in turtlebot3 using gazebo. If the robot is real, delete the line export ROS_DOMAIN_ID=30 #TURTLEBOT3'  from bashrc.
8. Make sure to resourcing the bashrc.    
    ```
    $ cd ~ && source ./.bashrc
    ```
9. Now the packages are ready to use.

The steps to use :
1. Open terminal or terminator
2. Open three `terminal` or split `terminator` to three windows.
3. First window will be use to launch the maze world.   
    ```
    $ ros2 launch tb3_slam axel_mazemap.launch.py
    ```
4. Second window will be used to open `rviz` slam.
    ```
    $ ros2 launch tb3_slam slam_rviz.launch.py
    ```
5. The third one will be used for moving command of the robot. It can be done manually using keyboard 
    ```
    $ ros2 run turtlebot3_teleop teleop_keyboard
    ```
    , using cmd_vel on terminal
    ```
    $ ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "linear: \  x: 10.0\angular:\  z: 5.0\"
    ```
    , or auto driving.
    ```
    $ ros2 run turtlebot3_gazebo turtlebot3_drive’
    ```
9. If auto driving, it will automatically go forward until it reach certain distance to the wall and then it will change direction. Thus, waiting several minutes, it will automatically create map in the rviz.

The thing that can be improved from this project is the auto driving of the robot. Because using the basic auto driving, the map scanning sometimes takes a longer time because the robot should go around a few time for the wrong turn.

However, this project is a fun project to work with. It can be developed more to the information part, make into application, broadcast it live, use to deliver things from one point to other point, or other functions.
