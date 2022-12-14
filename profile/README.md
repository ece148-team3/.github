# ECE 148 Team 3

- [ECE 148 Team 3](#ece-148-team-3)
  - [Team Members](#team-members)
  - [Final project](#final-project)
    - [Overview](#overview)
    - [Wiring Diagram](#wiring-diagram)
    - [Software design](#software-design)
    - [BEV calibration and real time projection](#bev-calibration-and-real-time-projection)
    - [Image stitching](#image-stitching)
    - [Obstacle avoidance using disparity map](#obstacle-avoidance-using-disparity-map)
    - [Demo](#demo)
    - [Future work](#future-work)
  - [Other projects](#other-projects)
    - [OpenCV Line Follower](#opencv-line-follower)
    - [ROS2 Line Follower](#ros2-line-follower)

## Team Members

- Ben Chiang (ECE)
- Ling Yu Chen (ECE)
- Xiran Wang (MAE)
- Yaosen Zhang (ECE)

## Final project

Our final project aims to create a autonomously driven car that can create a massive bird's eye view (BEV) image of a region. This can unlock many post processing in robotics, such as path-planning, model training, and (hazardous) environment evaluation. Typically, such task would be done with a drone, but drones are easily occluded by taller objects, such as trees, buildings, and tunnels overheads, not to mention the lost in environmental details when flying high up in the air.

We were able to achieve ~20 FPS real-time BEV projection on Nvidia Jetson Nano and ~25 planar images stitched in post-processing. Furthermore, using OAK-D lite camera's built in stereo pair camera, we were able to extract disparity info from the scene for obstacle avoiding.

### Overview

Here's a list of things that we were able to achieve:

1. Created a software processing pipeline that allows BEV image collection
2. Successfully stitched together ~25 BEV images using OpenCV stitcher and Peak-Signal-to-noise-ratio (PSNR)
3. Achieved accurate BEV project with programmatically obtained intrinsic and extrinsic using checkerboard calibration and OpenCV
4. Collected 20k images on campus at different scenes
5. Completed TCP socket prototype for transferring BEV images from Jetson to host server, but haven't implemented into real-time processing
6. Deployed BEV projection code on Jetson with CUDA at ~20 FPS / 720P
7. Extracted disparity information from the scene using OAK-D lite stereo pair for obstacle avoidance algorithm

### Wiring Diagram

Diagram Legend:

- Green: Signal
- Red: Power
- Dotted: Wireless

<!-- ![ECE 148 Electrical Schematic_no_bg](/assets/ECE%20148%20Electrical%20Schematic.svg) -->
![ECE 148 Electrical Schematic_no_bg](/assets/ECE%20148%20Electrical%20Schematic_no_bg.svg)

### Software design

<!-- ![ECE 148 Software Pipeline](/assets/ECE%20148%20Software%20Pipeline.svg) -->
![ECE 148 Software Pipeline_no_bg](/assets/ECE%20148%20Software%20Pipeline_no_bg.svg)

### BEV calibration and real time projection

To obtain accurate BEV projection, we need to calibrate the camera. We used a checkerboard calibration pattern to calibrate the camera.  In short, the extrinsic matrix is a transformation matrix from the world coordinate system to the camera coordinate system, and the intrinsic matrix is a transformation matrix from the camera coordinate system to the screen coordinate system. After we obtain the extrinsic and intrinsic matrix, we can use OpenCV libraries and algorithms to apply BEV Transformation accordingly. Here's an example of the checkerboard calibration pattern being applied for transformation:


![Result of checkerboard calibration](/assets/calibration_result.png)

After the transformation, we deployed the code on Jetson with CUDA to achieve ~20 FPS / 720P. Here's an example of the BEV projection in action. We then measured the performance hit of the BEV projection and found that it took up significant amount of CPU processing power. Thus, we decided to move the project onto GPU to achieve real-time BEV projection.

The following is a screenshot of the CPU and GPU usage during BEV projection, you can see that with the GPU helping out, the CPU usage dropped significantly.

![CPU and GPU usage during BEV projection](/assets/bev_perf.png)

### Image stitching

Utilizing the OpenCV stitcher class and its methods we were able to successfully stitch around 25 BEV image of various scenes on campus, including the snake path, bubble road, and the warren logo near Price Center. At first we tried stitching BEV collected on track, but because the suboptimal lighting condition and lack of key features we weren't successful. For the successful attempts, we implemented two different sampling methods to filter out images that were too similar. This helps us reduce the number of images to stitch and processing complexity, since stitching over thousands of images collected in a single run together is infeasible. The first method uses Structural Similarity Index as a metric of similarity between the two images. The output is a value from 0 to 1, with 1 being exactly the same. However, we quickly discovered how computationally intensive the process is. Thus, we tried Peak signal-to-noise ratio. It compares the difference between the two images as noise. The higher the noise, the more different the two pictures are. PSNR was less computationally intensive but Jetson still struggles to stitch images in real time. So we decided to stitch the images in post-processing on a more powerful machines.

Here is an example of what the stitched image looks like:

<img src="https://github.com/ece148-team3/.github/blob/main/assets/stitched_img.png" width="160" height="400">


### Obstacle avoidance using disparity map

We modify the depth-sensing functionality from DepthAI API in our OKA-D Lite camera to aid our desieogn of obstacle avoiding. Although there are pre-trained models in DepthAI that can detect objects which may be used to tell our car to change its direction when the object is near, these models have a limited scope on objects like human or vehicle but not universal obstacles that should unbiasedly deteted by our car to avoid collision. Therefore, we decide to use the disparity map captured by our stereo camera which uses both a left and a right mono camera to calculate the disparity of the two image view. Theoretically, the disparity distribution is inversely related to the depth distribution and hence a large disparity value should suggest a "danger zone" for our car so that an alternate direction should be chosen. However, we are faced several limitations on our device and cannot calculate depth from disparity using the theoretical formula at both a close and a distant point. Moreover, the noise on the dispartiy map add more unwanted factors that we need to be careful of to get an accurate sense of the depth distribution at the middel arrange. Here is some illustration of the disparity map with the numbers showing as the disparity value of the middle point of each region of interest. 

<img src="https://github.com/ece148-team3/.github/blob/main/assets/disparity_map_1.png" width="320" height="200" style="border-left:160px solid white;border-right:50px solid white;"> <img src="https://github.com/ece148-team3/.github/blob/main/assets/disparity_map_2.png" width="320" height="200">

### Demo

Here's a few example of the BEV projection in action:

The first example is a video of the car driving through one of the mockup intersections used by another team. You can see the BEV projection in action and the disparity visualization.

<https://youtu.be/k5y6Xw4DUZQ>

Here's another showcases the accuracy of the BEV projection. In the video, you can see the characters of the Jacob's Engineering Logo very clearly, as opposed to the original car POV.

<https://youtu.be/KqpQ_7KYDQc>

### Future work

Unfortunately, we ran out of time to implement the following features:

- [ ] Implement TCP socket into real-time processing to allow real-time BEV image transfer and stitching
- [ ] Modify stitching algorithm to allow images to be stitched along different car trajectory
- [ ] Complete obstacle avoidance algorithm using disparity map, and possibly incorporate lidar and GPS to achieve better motion planning when mapping environment
- [ ] Allow for points of interest to be set and path planning to be done
- [ ] Obtain more accurate distance measurement
- [ ] Measure real time performance of the entire pipeline (TCP connection latency, disparity mapping, motion planning, etc)
- [ ] Create a OCR word recognition algorithm to allow for text recognition on the road surface(We have successflly established the code for OCR using PyTesseract, but we have not implement the code into Jetson.)
- [ ] Deploy the entire pipeline on ROS for better scalability and modularity

## Other projects

The following are some of the other projects that we have worked on this quarter:

### OpenCV Line Follower

We utilized OpenCV to create a line following algorithm. The processing steps are as follows:

1. Crop the image to only include the region of interest, which is roughly the horizon line to the bottom of the image
2. Apply Gaussian blur to the image to reduce noise
3. Convert RGB image to HSV color space
4. Apply color segmentation to the image to isolate the yellow lane dividers
5. Find the contours of the yellow lane dividers
6. Find the center of the largest contour
7. Steer the car based on the center of the largest contour. If the center of the contour on the right side of the image, then the car should steer to the right. If the center of the contour on the left side of the image, then the car should steer to the left. If the center of the contour is in the middle of the image, then the car should go straight. If there is no contour, then the car should continue it's last command.

Here's a video of the robocar completing 3 laps around the track: <https://youtu.be/Zr8NgKoxbow>

### ROS2 Line Follower

We deployed the same algorithm that successfully completed 3 autonomous laps into the ROS framework by using the `ucsd_robocar` ROS package provided in the docker container. In the config files of `ucsd_robocar_nav2_pkg` we turned on all the required components, which includes OAK-D lite, VESC, and the subpub_camera_actuator_launch from the `ucsd_robocar_basics2_pkg` package. The main file that we edited is the `subpub_camera_actuator_launch.py` that can be found the `ucsd_robocar_basics2_pkg` package. It subscribes to the camera node and publishes to the `cmd_vel` topic, which then controls the VESC a through an Ackermann steering node. However, we soon found that the performance of the pipeline was not as good as the OpenCV counterpart. In particular, the control signals have high latency where the turning commands would only be actuated after 1-2 seconds. To solve this issue, we first reduced the resolution of the images being published from the OAK-D lite camera node down to. Through experimentation, we found that the Ackermann node is another bottleneck, so we directly publish the steering angle to the VESC node. This significantly reduced the latency of the control signals. With all these modifications, our robot successfully completed 9.5 laps around the track.

Here's a video of the robocar completing 3 laps: <https://youtu.be/5b5-0zkLHsc>
