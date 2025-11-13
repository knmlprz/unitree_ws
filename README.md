# unitree_ws

### install ros2 humble and follow instructions
- [ros2_humble_installation](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
echo "export ROS_DOMAIN_ID=1" >> ~/.bashrc
```

go to: 
- [unitree_mujoco](https://github.com/unitreerobotics/unitree_mujoco)

### simulation and unitree_sdk2_python test
```bash
cd ~
sudo apt install python3-pip
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
cd unitree_sdk2_python
sudo pip3 install -e .

pip3 install mujoco

pip3 install pygame

cd ~
git clone https://github.com/unitreerobotics/unitree_mujoco.git
cd unitree_mujoco
cd ./simulate_python
python3 ./unitree_mujoco.py

```

### simulation moving test
change config.py
```bash
nano ~/unitree_mujoco/simulate_python/config.py

# set in this file: USE_JOYSTICK = 0
```

```bash
cd ~/unitree_mujoco/example/python
python3 ./stand_go2.py
```

### unitree_ros2 test

go to: 
[unitree_ros2](https://github.com/unitreerobotics/unitree_ros2)

```bash
cd ~
git clone https://github.com/unitreerobotics/unitree_ros2

sudo apt install ros-humble-rmw-cyclonedds-cpp
sudo apt install ros-humble-rosidl-generator-dds-idl
sudo apt install libyaml-cpp-dev

cd ~/unitree_ros2
colcon build
```

### setup.sh configuration
```bash
sudo gedit ~/unitree_ros2/setup_local.sh
```

```bash
#!/bin/bash
echo "Setup unitree ros2 simulation environment"
source /opt/ros/humble/setup.bash
#source $HOME/unitree_ros2/cyclonedds_ws/install/setup.bash
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI='<CycloneDDS><Domain><General><Interfaces>
                            <NetworkInterface name="lo" priority="default" multicast="default" />
                        </Interfaces></General></Domain></CycloneDDS>'
```

# running ros2 moving test

```bash
cd ~/unitree_mujoco/simulate_python
source ~/unitree_ros2/setup_local.sh # use "lo" as the network interface
python3 ./unitree_mujoco.py

```

```bash
source ~/unitree_ros2/setup_local.sh 
ros2 topic list

cd ~/unitree_ros2

source install/setup.bash
./install/unitree_ros2_example/bin/read_low_state_hg 

```

change config.py
```bash
nano ~/unitree_mujoco/simulate_python/config.py

# set in this file: ROBOT = "g1"
# and             : ROBOT_SCENE = "../unitree_robots/" + ROBOT + "/scene_29dof.xml"
```

```bash
cd ~/unitree_ros2
source ~/unitree_ros2/setup_local.sh 
source install/setup.bash
./install/unitree_ros2_example/bin/g1_ankle_swing_example 

```

