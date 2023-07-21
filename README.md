# Autonomous-Wheelchair-using-a-camera-on-Gazebo
This is a project of an autonomous wheelchair using a camera to recognize the objects in the environment. The project is still in the beginning. I am planning to use ultrasound sensor and Lidar sensor to help to detect the obstacles in the environment and avoid collisions. I will be updating the code as soon as possible.


#How to Run the Project
############################################# VIRTUAL ENVIRONMENT CREATION ##############################################################
# Install virtualenv package using pip
python3 -m pip install --user virtualenv
# Create a virutal Environment
python3 -m virtualenv ROS2SDC_VENV
# Activate Virtaul Environment
source ROS2SDC_VENV/bin/activate
# Install neccesary python modules
pip3 install -r Repo_resources/installation_requirements_python.txt

############################ Installing Dependencies 

mkdir -p ~/.gazebo/models
sudo apt install -y python3-colcon-common-extensions
sudo apt install -y ros-foxy-gazebo-ros-pkgs

############################################# BUILDING THE PROJECT

# Bring all models into your .gazebo/models
cp -r self_driving_car_pkg/models/* ~/.gazebo/models
# Notify Colcon to ignore ROS2SDC_VENV while build
touch ROS2SDC_VENV/COLCON_IGNORE
#Build repo
colcon build

############################################# RUNNING THE PROJECT

>>>>>>> Open A new Terminal <<<<<<<<

#Activate Environment
source ROS2SDC_VENV/bin/activate
# Source *your Workspace* in any terminal you open to Run files from this workspace
source /home/carlos/Development/ROS2-Self-Driving-Car-AI-using-OpenCV-main/install/setup.bash
source /opt/ros/humble/setup.bash

## once build you can run the simulation e.g [ ros2 launch (package_name) world(launch file) ] 
ros2 launch self_driving_car_pkg world_gazebo.launch.py

## To activate the SelfDriving Car
ros2 run self_driving_car_pkg computer_vision_node

############################################# PROBLEMS & SOLUTION  ########################################3
## TypeError: Descriptors cannot not be created directly.
pip install protobuf==3.20.0

######################################## Checking gazebo pluggins/world
cd /opt/ros/humble/share/gazebo_plugins/worlds
####################################### Kill gazebo service
killall gzserver
killall -9 gazebo & killall -9 gzserver  & killall -9 gzclient

####################################### open gazebo simulation for the wheelchair
gazebo --verbose /home/carlos/Development/ROS2-Self-Driving-Car-AI-using-OpenCV-main/self_driving_car_pkg/worlds/wheelchair_self_driving.world

####################################### Run the node for controlling the wheelchair
ros2 run self_driving_car_pkg driver_node

####################################### set up bashrc
gedit ~/.bashrc

###################################### check the list of topics activated
ros2 topic list

##################################### controlling using keyboard
ros2 run teleop_twist_keyboard teleop_twist_keyboard

##################################### installing teleop
sudo apt-get install ros-humble-teleop-twist-keyboard
sudo apt-get install ros-humble-teleop-twist-keyboard

ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/model/vehicle_blue/cmd_vel

##################################### check camera image
ros2 run rqt_image_view rqt_image_view

#################################### check image size
ros2 topic bw /camera/image_raw

################################### record simulation on gazebo
ros2 run self_driving_car_pkg video_recording_node
