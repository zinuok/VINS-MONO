# VINS-MONO

#### ● rovio-application and installation
#### ● Used Intel D435i - stereo setup
#### ● ROVIO official [homepage](https://github.com/ethz-asl/rovio)
## ● Result
+ Real world on Xavier [clip1](https://youtu.be/_o2KwT8jJN0) : Mono, 30fps, 25 features, 6x6 pathches, 4 NLevel -> big error (scaled)
+ ROVIO on flightgoggles [clip2](https://youtu.be/3Xgwi7k6css)
+ ROVIO vs VINS-Mono [clip3](https://youtu.be/n0N2qDcNcBQ)
+ VINS-Mono vs ROVIO vs ORB-SLAM2 [clip4](https://youtu.be/XMyiNlIbDXU)

<br><br>

## Requirements and installation

### ● [IntelD435i setup and calibration](https://github.com/engcang/VINS-application/tree/Intel-D435i)
  + Used [Kalibr](https://github.com/ethz-asl/kalibr) as [here](https://github.com/engcang/vins-application#-calibration--kalibr---synchronization-time-offset-extrinsic-parameter) and referred [here](https://support.stereolabs.com/hc/en-us/articles/360012749113-How-can-I-use-Kalibr-with-the-ZED-Mini-camera-in-ROS-)
  ~~~shell
  $ rosrun kalibr kalibr_rovio_config --cam <cam-chain.yaml filename>
  ~~~

### ● Install [kindr](https://github.com/ANYbotics/kindr)
 ~~~shell
  $ cd ~/catkin_ws/src && git clone https://github.com/ANYbotics/kindr
  $ cd .. && catkin build -j8
 ~~~

### ● Install lightweight_filtering and ROVIO
 ~~~shell
  $ cd ~/catkin_ws/src && git clone https://github.com/ethz-asl/rovio
  $ cd rovio && git submodule update --init --recursive

  $ cd ..
  $ catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release

  # With with opengl scene (optional)
  $ sudo apt-get install freeglut3-dev libglew-dev
  $ catkin build rovio --cmake-args -DCMAKE_BUILD_TYPE=Release -DMAKE_SCENE=ON
 ~~~
 
 ### ● on [Flightgoggles](http://flightgoggles.mit.edu), need gray scale image
 + used this [file](https://github.com/engcang/rovio-application/blob/master/flightgoggles-rovio/scripts/rgb2gray.py)
