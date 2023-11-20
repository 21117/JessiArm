# JessiArm
rf. https://zeta7.notion.site/zeta7/JessiArm-be431f54912b472fb7f8977e5499612d
## Hardware

Nvidia Jetson 2G board

PCA9685 PWM driver

DFROBOT FIT07071 USB Camera

DIY 5DOF Robot Arm + Gripper

18650 4 battery shield v9

## Software on Jetson

- Jetpack4.5.x or Jetpack4.6.x(Ubuntu 18.04) → OpenCV downgrade to 3.4.6 for darknet_ros
- ROS Melodic
- MoveIt 1
  
# Install jessiarm code
 jessiarm install on Jetson
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/zeta0707/jessiarm.git
$ cd ~/catkin_ws
```
cv_bridge build
```
cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws
cma
```
# Test servo motor
adafruit_pca9685 servo motor check
```
sudo apt install python-pip -y
pip2 install adafruit_pca9685
```
```/home/jetson/catkin_ws/src/jessiarm/jessiarm_control/src/servokit_test.py ```
```
cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py
```
# Teleop by keyboard
teleop_twist_keyboard install
```
jetson@jp4612GCv346Py37:~/catkin_ws$ git clone https://github.com/ros-teleop/teleop_twist_keyboard.git
jetson@jp4612GCv346Py37:~/catkin_ws$ cd ..
jetson@jp4612GCv346Py37:~/catkin_ws$ cma
```
```
$ roslaunch jessiarm_control teleop_keyboard.launch
```
j l: robot arm left, right
i , : motor1 incline
o . : motor2 incline
u m : motor3 incline
t b: gripper open, close
U, M: motor4 revolution
#  Verify USB camera
rf. https://developer.nvidia.com/embedded/learn/tutorials/first-picture-csi-usb-camera
```
jetson@jp4612GCv346Py37:~$ ls /dev/video*
/dev/video0
jetson@jp4612GCv346Py37:~$ nvgstcapture-1.0 --camsrc=0 --cap-dev-node=/dev/video0
```
# blob Pick and Place
```
$ roslaunch jessiarm_control blob_control.launch
```
The contents of "find_ball.yaml"
```
define: &blue_min [55,40,0]
define: &blue_max [150, 255, 255]
define: &white_min [16, 0, 66]
define: &white_max [133, 251, 255]
define: &pink_min [135, 41, 95]
define: &pink_max [255, 196, 255]
define: &green_min [39, 81, 71]
define: &green_max [75, 255, 255]
define: &orange_min [7, 109, 50]
define: &orange_max [76, 218, 234]
define: &red_min [0, 94, 92]
define: &red_max [255, 255, 255]
```
Chose the beneath two filters
```
blob_detector:
  blob_min: *green_min
  blob_max: *green_max
```
You can decide the scaning speed
```
jessiarm_speed:
  SCAN_SPEED: 0.02
```
# Yolo4 Pick and Place
Now camera recognizes object then picks and places it.
use the "darknet_ros" to recognize object.
1. "YOLO" stands for "You Only Look Once," referring to a deep learning model used for object detection and recognition tasks. YOLO is known for its efficiency in real-time object detection, capable of detecting and classifying various objects in images or videos.
"YOLO4-tiny" denotes a lightweight version of YOLOv4.
2. "darknet_ros" is a package for using YOLO with ROS (Robot Operating System). ROS is an open-source robotics platform used for developing and running robotics applications. The "darknet_ros" package facilitates the integration of YOLO into the ROS environment.
Above things allow you to implement object detection in robotics applications.

1.1 Install darknet_ros
```
sudo apt-get install -y ros-melodic-image-pipeline

cd ~/catkin_ws/src
git clone --recursive https://github.com/Tossy0423/yolov4-for-darknet_ros.git
```
1.2 Darknet_ros modification: yolov4 → yolov4-tiny
```
cd ~/catkin_ws/src/yolov4-for-darknet_ros/darknet_ros
git clone https://github.com/zeta0707/darknet_ros_custom.git
cp -rf darknet_ros_custom/* darknet_ros/
```
1.3 Run "darknet_ros"

1.3.4 Pick and place the objects learned by "yolo_v4"
```
#terminal #1, object detect using Yolo_v4
jetson@jp4612GCv346Py37:~$ roslaunch darknet_ros yolo_v4.launch

#terminal #2, camera publish, object x/y -> robot move
jetson@jp4612GCv346Py37:~$ roslaunch jessiarm_control yolo_chase.launch
```
1.4 Write the object to PicknPlace in "yolo_jessiarm.yaml"
```
jessiarm_target:
  DETECT_CLASS1 : "cup"
  DETECT_CLASS2 : "mouse"
:
```
