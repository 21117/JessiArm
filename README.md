# JessiArm
# Install jessiarm code
 jessiarm install on Jetson
```$ cd ~/catkin_ws/src
$ git clone https://github.com/zeta0707/jessiarm.git
$ cd ~/catkin_ws
```
cv_bridge build
```cd ~/Downloads/opencvDownTo34
sudo patch -p1 /opt/ros/melodic/share/cv_bridge/cmake/cv_bridgeConfig.cmake -p1 < cv_brige.patch
cd ~/catkin_ws
cma
```
# Test servo motor
adafruit_pca9685 servo motor check
```sudo apt install python-pip -y
pip2 install adafruit_pca9685
```
```/home/jetson/catkin_ws/src/jessiarm/jessiarm_control/src/servokit_test.py ```
```cd ~/catkin_ws
python src/jessiarm/jessiarm_control/src/servokit_test.py
```
# Teleop by keyboard
teleop_twist_keyboard install
```jetson@jp4612GCv346Py37:~/catkin_ws$ git clone https://github.com/ros-teleop/teleop_twist_keyboard.git
jetson@jp4612GCv346Py37:~/catkin_ws$ cd ..
jetson@jp4612GCv346Py37:~/catkin_ws$ cma
```
```$ roslaunch jessiarm_control teleop_keyboard.launch```
j l: robot arm left, right
i , : motor1 incline
o . : motor2 incline
u m : motor3 incline
t b: gripper open, close
U, M: motor4 revolution
