# VINS-MONO
+ VINS-Mono setup for different nvidia board: jetson TX2, jetson Xavier NX
+ version info
    + Ubuntu: 18.04 
    + ROS: Melodic 
+ from [here](https://github.com/HKUST-Aerial-Robotics/VINS-Mono)
<br>

# Index
### 1. Prerequisites
#### ● [Ceres solver and Eigen](#-ceres-solver-and-eigen--mandatory-for-vins) : Mandatory for VINS (build Eigen first)
#### ● [Installation](#-installation-1)

<br><br><br>

# 1. Prerequisites
### ● Ceres solver and Eigen : Mandatory for VINS
+ Eigen [home](http://eigen.tuxfamily.org/index.php?title=Main_Page)
~~~shell
$ wget -O eigen.zip http://bitbucket.org/eigen/eigen/get/3.3.7.zip #check version
$ unzip eigen.zip
$ mkdir eigen-build && cd eigen-build
$ cmake ../eigen_source_folder_name && sudo make install
~~~

+ Ceres solver [home](http://ceres-solver.org/installation.html)
~~~shell
$ sudo apt-get install -y cmake libgoogle-glog-dev libatlas-base-dev libsuitesparse-dev
$ wget http://ceres-solver.org/ceres-solver-1.14.0.tar.gz
$ tar zxf ceres-solver-1.14.0.tar.gz
$ mkdir ceres-bin
$ mkdir solver && cd ceres-bin
$ cmake ../ceres-solver-1.14.0 -DEXPORT_BUILD_DIR=ON -DCMAKE_INSTALL_PREFIX="../solver"  #good for build without being root privileged and at wanted directory
$ make -j8 # 8 : number of cores
$ make test
$ make install
~~~
<br><br>

### ● Installation
+ git clone and build from source
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/HKUST-Aerial-Robotics/VINS-Mono.git
$ cd ../ && catkin build -DCMAKE_BUILDTYPE=Release -j3
$ source ~/catkin_ws/devel/setup.bash
```
**Before build VINS-Fusion, process below could be required.**
***
<br>

+ For GPU version, if OpenCV with CUDA was built manually, build cv_bridge manually also
~~~shell
$ cd ~/catkin_ws/src && git clone https://github.com/ros-perception/vision_opencv
# since ROS Noetic is added, we have to checkout to melodic tree
$ cd vision_opencv && git checkout origin/melodic
$ gedit vision_opencv/cv_bridge/CMakeLists.txt
~~~
Edit OpenCV PATHS in CMakeLists and include cmake file
~~~txt
#when error, try both lines
#find_package(OpenCV 3 REQUIRED PATHS /usr/local/share/OpenCV NO_DEFAULT_PATH
find_package(OpenCV 3 HINTS /usr/local/share/OpenCV NO_DEFAULT_PATH
  COMPONENTS
    opencv_core
    opencv_imgproc
    opencv_imgcodecs
  CONFIG
)
include(/usr/local/share/OpenCV/OpenCVConfig.cmake) #under catkin_python_setup()
~~~
~~~shell
$ cd .. && catkin build cv_bridge
~~~
<br>

+ For GPU version, Edit CMakeLists.txt for loop_fusion and vins_estimator
~~~shell
$ cd ~/catkin_ws/src/VINS-Fusion-gpu/loop_fusion && gedit CMakeLists.txt
or
$ cd ~/catkin_ws/src/VINS-Fusion-gpu/vins_estimator && gedit CMakeLists.txt
~~~
~~~txt
##For loop_fusion : line 19
#find_package(OpenCV)
include(/usr/local/share/OpenCV/OpenCVConfig.cmake)

##For vins_estimator : line 20
#find_package(OpenCV REQUIRED)
include(/usr/local/share/OpenCV/OpenCVConfig.cmake)
~~~
<br>

### ● Trouble shooting
+ Aborted error when running **vins_node** : 
~~~shell
 $ echo "export MALLOC_CHECK_=0" >> ~/.bashrc
 $ source ~/.bashrc
~~~
