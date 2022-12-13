# ECE 148 Team 3

- [ECE 148 Team 3](#ece-148-team-3)
  - [Team Members](#team-members)
  - [Final project](#final-project)
    - [Overview](#overview)
    - [Hardware design](#hardware-design)
    - [Wiring Diagram](#wiring-diagram)
    - [Software design](#software-design)
    - [BEV calibration and real time projection](#bev-calibration-and-real-time-projection)
    - [Image stitching](#image-stitching)
    - [Obstacle avoidance using disparity map](#obstacle-avoidance-using-disparity-map)
    - [Future work](#future-work)
  - [Other projects](#other-projects)
    - [OpenCV Line Follower](#opencv-line-follower)
    - [ROS2 Line Follower](#ros2-line-follower)


## Team Members

- Ben Chiang (ECE)
- Xiran Wang (MAE)

## Final project

### Overview

What we have done:
1.Created a software processing pipeline that allows BEV image collection
2.Achieved a working prototype of image stitching using OpenCV stitcher
3.Calibrated the camera intrinsics and extrinsics
4.Collected data on campus at different scenes
5.TCP socket prototype completed, but not integrated into the real time loop
6.Deployed BEV projection code on Jetson with Cuda
7.Partially implemented vision (depth map) only obstacle avoidance algorithm

### Hardware design

### Wiring Diagram

### Software design

### BEV calibration and real time projection
We initially use chessboard to calibrate the camera. By doing so, we can use the fixed calibration to determine the geometric parameters of the image formation process. By using algorithm, we can obtain both intrinsic and extrinsic matrix. In short (The extrinsic matrix is a transformation matrix from the world coordinate system to the camera coordinate system, and the intrinsic matrix is a transformation matrix from the camera coordinate system to the pixel coordinate system.) After we obtain these matrix, we can use OpenCV libraries and algorithms to apply BEV Transformation accordingly.

### Image stitching

### Obstacle avoidance using disparity map

### Future work


## Other projects

### OpenCV Line Follower

### ROS2 Line Follower
