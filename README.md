# A-LOAM
## Advanced implementation of LOAM (ubuntu 20.04 noetic supported)

A-LOAM is an Advanced implementation of LOAM (J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time), which uses Eigen and Ceres Solver to simplify code structure. This code is modified from LOAM and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED). This code is clean and simple without complicated mathematical derivation and redundant operations. It is a good learning material for SLAM beginners.

<img src="https://github.com/HKUST-Aerial-Robotics/A-LOAM/blob/devel/picture/kitti.png" width = 55% height = 55%/>

**Modifier:** [Tong Qin](http://www.qintonguav.com), [Shaozu Cao](https://github.com/shaozu)

**Beware:**
only supports 16/32/64 scans lidar

does not rewrite markOcculdePoints function of original LOAM (see featureExtraction.cpp::markOccludePoints() in LIO-SAM)

## 1. Prerequisites
### 1.1 **Ubuntu** and **ROS**
Ubuntu 64-bit 16.04 or 18.04.
ROS Kinetic or Melodic. [ROS Installation](http://wiki.ros.org/ROS/Installation)

Ubuntu 20.04 Noetic.
summmarized from [Installation Guide](https://blog.csdn.net/weixin_43910370/article/details/120736760c.)



### 1.2. **Ceres Solver** (not verified)
Follow [Ceres Installation](http://ceres-solver.org/installation.html).

### 1.3. **PCL** (not verified)
Follow [PCL Installation](http://www.pointclouds.org/downloads/linux.html).


## 2. Build A-LOAM
Clone the repository and catkin_make:

```
    cd
    git clone https://github.com/countsp/A-LOAM.git ~/aloam_noted_ws/src
```

Modify the '/camera_init' in the four .cpp files to 'camera_init'.

Change '#include <opencv/cv.h>' in scanRegistration.cpp to '#include <opencv2/imgproc.hpp>'.

Replace 'CV_LOAD_IMAGE_GRAYSCALE' in kittiHelper.cpp with 'cv::IMREAD_GRAYSCALE'.

In aloam_noted_ws->src->CMakeLists.txt, add

```
set( CMAKE_CXX_STANDARD 14)
```



```
    cd aloam_noted_ws
    catkin_make
    source ~/aloam_noted_ws/devel/setup.bash
```




## 3. Velodyne VLP-16 Example
Download [NSH indoor outdoor](https://drive.google.com/file/d/1s05tBQOLNEDDurlg48KiUWxCp-YqYyGH/view) to YOUR_DATASET_FOLDER. 

```
    cd aloam_noted_ws/src/launch
    roslaunch aloam_velodyne aloam_velodyne_VLP_16.launch
    rosbag play ~/bagfiles/nsh_indoor_outdoor.bag
```


## 4. KITTI Example (Velodyne HDL-64)
Download [KITTI Odometry dataset](http://www.cvlibs.net/datasets/kitti/eval_odometry.php) to YOUR_DATASET_FOLDER and set the `dataset_folder` and `sequence_number` parameters in `kitti_helper.launch` file. Note you also convert KITTI dataset to bag file for easy use by setting proper parameters in `kitti_helper.launch`. 

```
    roslaunch aloam_velodyne aloam_velodyne_HDL_64.launch
    roslaunch aloam_velodyne kitti_helper.launch
```
<img src="https://github.com/HKUST-Aerial-Robotics/A-LOAM/blob/devel/picture/kitti_gif.gif" width = 720 height = 351 />

## 5. Docker Support
To further facilitate the building process, we add docker in our code. Docker environment is like a sandbox, thus makes our code environment-independent. To run with docker, first make sure [ros](http://wiki.ros.org/ROS/Installation) and [docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/) are installed on your machine. Then add your account to `docker` group by `sudo usermod -aG docker $YOUR_USER_NAME`. **Relaunch the terminal or logout and re-login if you get `Permission denied` error**, type:
```
cd ~/catkin_ws/src/A-LOAM/docker
make build
```
The build process may take a while depends on your machine. After that, run `./run.sh 16` or `./run.sh 64` to launch A-LOAM, then you should be able to see the result.


## 6.Acknowledgements
Thanks for LOAM(J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time) and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED).



---

# 录制pcd
在即将跑完的时候录制，在想要保存的文件夹位置，打开终端，运行

rosrun pcl_ros pointcloud_to_pcd input:=/laser_cloud_map

使用pcl_viewer查看（需要install pcl-tools）
