***
# VINS-MONO
+ **hardware setup**
    + jetson TX2 - Jetpack 4.2
    + jetson AGX Xavier
    + jetson Xavier NX - Jetpack 4.4
    + realsense D435i (color, infra1, infra2)
    + pixhawk4 mini
    <br>
+ **software setup**
    + Ubuntu: 18.04 
    + ROS: Melodic 
    + OpenCV 3.4.1
    <br>
+ **github link**: [HKUST-Aerial-Robotics](https://github.com/HKUST-Aerial-Robotics/VINS-Mono)
***
<br>

# Index
### 1. Prerequisites
####    &nbsp;&nbsp;&nbsp;&nbsp;● Eigen
####    &nbsp;&nbsp;&nbsp;&nbsp;● Ceres solver
### 2. Install
### 3. Jetson Boards
####    &nbsp;&nbsp;&nbsp;&nbsp;● Actually, there is no installation difference among TX2, Xavier, and NX
### 4. Run
<br><br>

## 1. Prerequisites
### ● Eigen
+ ~Eigen from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)~ 
=> This link is not avaiable (2020.09.03). Download eigen-3.3.7.zip from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)
```
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip # <= do not download from this. 
$ unzip eigen-3.3.7.zip
$ cd ~/eigen-3.3.7 && mkdir build && build
$ cmake ../ && sudo make install
```
### ● Ceres solver
+ Ceres solver from [here](http://ceres-solver.org/installation.html)
```
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ cd ceres-solver-1.14.0
$ mkdir build && cd build
$ cmake -DEXPORT_BUILD_DIR=ON \
        -DCMAKE_INSTALL_PREFIX=/usr/local/include \
        ../
$ make -j8 # 8 : number of cores
$ make test
$ make install
```

### ● cv_bridge
+ if you built OpenCV manually(refer [here](https://github.com/zinuok/Xavier_NX#1-opencv-ver-341-install)), you should also build 'cv_bridge' manually from source.
+ Download from proper branch in [here](https://github.com/ros-perception/vision_opencv/tree/melodic) according to your ROS version. For example,
```
$ cd ~/catkin_ws/src
$ git clone -b melodic https://github.com/ros-perception/vision_opencv.git
```
<br><br>

## 2. Install
+ git clone and build from source
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/HKUST-Aerial-Robotics/VINS-Mono.git
$ cd ../ && catkin build -DCMAKE_BUILDTYPE=Release -j3
$ source ~/catkin_ws/devel/setup.bash
```

+ Trouble shooting
++ ceres/rotation.h: No such file or directory
```
$ sudo apt install -y libceres-dev
```
~or add followings into ~/VINS-Mono/feature_tracker/CMakeLists.txt~
<br>
~find_package(Ceres REQUIRED)~<br>
~include_directories(${CERES_INCLUDE_DIRS})~<br>
<br><br>

## 3. Jetson Boards
#### ● Actually, no installation difference among TX2, Xavier, and NX
<br><br>

## 4. Run
#### ● you have to get a calibration data using [kalibr](https://github.com/zinuok/kalibr)
```
$ roslaunch realsense2_camera rs_camera.launch
$ roslaunch mavros px4.launch
$ roslaunch vins_estimator realsense_color.launch
$ roslaunch vins_estimator vins_rviz.launch
```

