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
    - [Demo!](#demo)
    - [Future work](#future-work)
  - [Other projects](#other-projects)
    - [DonkeyCar behavioral cloning](#donkeycar-behavioral-cloning)
    - [OpenCV Line Follower](#opencv-line-follower)
    - [ROS2 Line Follower](#ros2-line-follower)

## Team Members

- Ben Chiang (ECE)
- Xiran Wang (MAE)

## Final project

Our final project aims to create a autonomously driven car that can create a massive bird's eye view (BEV) image of a region. This can unlock many post processing in robotics, such as path-planning, model training, and (hazardous) environment evaluation. Typically, such task would be done with a drone, but drones are easily occluded by taller objects, such as trees, buildings, and tunnels overheads, not to mention the lost in environmental details when flying high up in the air. 

We were able to achieve ~20 FPS real-time BEV projection on Nvidia Jetson Nano and ~25 planar images stitched in post-processing. Furthermore, using OAK-D lite camera's built in stereo pair camera, we were able to extract disparity info from the scene for obstacle avoiding.

### Overview

Here's a list of things that we were able to achieve:

1. Created a software processing pipeline that allows BEV image collection
2. Successfully stitched together ~25 BEV images using OpenCV stitcher and Signal-to-noise-ratio (SNR)
3. Achieved accurate BEV project with programmatically obtained intrinsic and extrinsic using checkerboard calibration and OpenCV
4. Collected 20k images on campus at different scenes
5. Completed TCP socket prototype for transferring BEV images from Jetson to host server, but haven't implemented into real-time processing
6. Deployed BEV projection code on Jetson with CUDA at ~20 FPS / 720P
7. Extracted disparity information from the scene using OAK-D lite stereo pair for obstacle avoidance algorithm

### Hardware design

### Wiring Diagram

Diagram Legend:

- Green: Signal
- Red: Power
- Dotted: Wireless

![ECE 148 Electrical Schematic_no_bg](/assets/ECE%20148%20Electrical%20Schematic_no_bg.svg)


### Software design

![ECE 148 Software Pipeline](/assets/ECE%20148%20Software%20Pipeline.svg)

### BEV calibration and real time projection

We initially use chessboard to calibrate the camera. By doing so, we can use the fixed calibration to determine the geometric parameters of the image formation process. By using algorithm, we can obtain both intrinsic and extrinsic matrix. In short (The extrinsic matrix is a transformation matrix from the world coordinate system to the camera coordinate system, and the intrinsic matrix is a transformation matrix from the camera coordinate system to the pixel coordinate system.) After we obtain these matrix, we can use OpenCV libraries and algorithms to apply BEV Transformation accordingly.

### Image stitching

### Obstacle avoidance using disparity map

### Demo!

### Future work

Unfortunately, we ran out of time to implement the following features:

- [ ] Implement TCP socket into real-time processing to allow real-time BEV image transfer and stitching
- [ ] Modify stitching algorithm to allow images to be stitched along different car trajectory
- [ ] Complete obstacle avoidance algorithm using disparity map, and possibly incorporate lidar and GPS to achieve better motion planning when mapping environment
- [ ] Allow for points of interest to be set and path planning to be done
- [ ] Obtain more accurate distance measurement
- [ ] Measure real time performance of the entire pipeline (TCP connection latency, disparity mapping, motion planning, etc)
- [ ] Create a OCR word recognition algorithm to allow for text recognition on the road surface
- [ ] Deploy the entire pipeline on ROS for better scalability and modularity


## Other projects

The following are some of the projects that we have worked on during the course of the semester:

### DonkeyCar behavioral cloning


### OpenCV Line Follower





### ROS2 Line Follower
