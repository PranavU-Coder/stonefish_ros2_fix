# stonefish_ros2_fix

Tutorial on how to make stonefish (uuv simulator) compatible with ros2 software particularly jazzy jalisco distro

pre-requisites:

- (1) OS: Ubuntu 24.04 LTS
- (2) ROS2: Jazzy Jalisco 
- (3) GPU: OpenGL 4.3+ support (verify with glxinfo | grep "OpenGL version") 

install the following dependencies if you haven't already

```bash
sudo apt install -y \  
  libglm-dev \  
  libsdl2-dev \  
  libfreetype6-dev \  
  libbullet-dev \  
  build-essential
 
```

for main installation :

## Cloning and building the stonefish core


replace name "auv_ws" with anyname you want to , just an example snippet

```bash
mkdir -p ~/auv_ws/src  
cd ~/auv_ws/src  
git clone https://github.com/patrykcieslak/stonefish.git  
cd stonefish  
mkdir build && cd build  
cmake .. -DCMAKE_INSTALL_PREFIX=~/auv_ws/install/stonefish  
make -j$(nproc)  
make install  
```

## install ROS2 interface

```bash
cd ~/auv_ws/src  
git clone https://github.com/patrykcieslak/stonefish_ros2.git  
cd ~/auv_ws  
colcon build --cmake-args -DCMAKE_PREFIX_PATH=~/auv_ws/install/stonefish  
```

## environment setup 


```bash
source /opt/ros/jazzy/setup.bash  
source ~/auv_ws/install/setup.bash  
export LD_LIBRARY_PATH=~/auv_ws/install/stonefish/lib:$LD_LIBRARY_PATH
```

if you are a fellow 'fish' user like me and don't use bash terminal 

```bash
set -gx LD_LIBRARY_PATH ~/auv_ws/install/stonefish/lib $LD_LIBRARY_PATH  
bass source ~/auv_ws/install/setup.bash
```

now there's a problem that occurs during colcon build of the repository , which is the CMakesPolicy stderr warnings
please refer to the file I uploaded in this very repo which contains the lines to add at very top of the CMakeLists.txt file which is under stonefish_ros2 directory


## rebuild

```bash
colcon build --cmake-args -DCMAKE_PREFIX_PATH=~/auv_ws/install/stonefish  
```

if you still get some sort of error it is most probably non-fatal and it doesn't affect functionality.

just check at the end if it says that packages have been built successfully or not

alternatively if you don't wish to see these annoying messages

```bash
colcon build --cmake-args -Wno-dev  
```


## if it shows pkgs been completed then the build has been successful

now launch the file to check if it works

run either one of these commands :

```bash
ros2 launch stonefish_ros2 stonefish_simulator.launch.py
# or for non-GPU version:
ros2 launch stonefish_ros2 stonefish_simulator_nogpu.launch.py
```

star this tutorial if it helped solve problem of installing and building the stonefish-uuv simulator
