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
+ ~Eigen from [here](http://eigen.tuxfamily.org/index.php?title=Main_Page)
```
$ wget https://gitlab.com/libeigen/eigen/-/archive/3.3.7/eigen-3.3.7.zip
$ unzip eigen-3.3.7.zip
$ cd ~/eigen-3.3.7 && mkdir build && cd build
$ cmake ../ && sudo make install -j $(nproc)
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
        -DCMAKE_INSTALL_PREFIX=/usr/local \
        ../
$ make -j $(nproc) # number of cores
$ make test -j $(nproc)
$ sudo make install -j $(nproc)
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
$ cd ../ && catkin build -DCMAKE_BUILDTYPE=Release -j $(nproc)
$ source ~/catkin_ws/devel/setup.bash
```

## 3. Jetson Boards
#### ● Actually, no installation difference among TX2, Xavier, and NX
<br><br>

## 4. Run
#### ● Uploaded folders for following setup: D435i, pixhawk4 mini 
#### ● for using your own sensor setup, you have to get a calibration data using [kalibr](https://github.com/zinuok/kalibr)
```
$ roslaunch realsense2_camera rs_camera.launch
$ roslaunch mavros px4.launch
$ roslaunch vins_estimator realsense_color.launch
$ roslaunch vins_estimator vins_rviz.launch
```

