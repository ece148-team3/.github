# ECE 148 Team 3 
What we have done:
1.Created a software processing pipeline that allows BEV image collection
2.Achieved a working prototype of image stitching using OpenCV stitcher
3.Calibrated the camera intrinsics and extrinsics
4.Collected data on campus at different scenes
5.TCP socket prototype completed, but not integrated into the real time loop
6.Deployed BEV projection code on Jetson with Cuda 
7.Partially implemented vision (depth map) only obstacle avoidance algorithm


# Detailed Description of Each Task:
Birds-Eye-View Transformaion:
We initially use chessboard to calibrate the camera. By doing so, we can use the fixed calibration to determine the geometric parameters of the image formation process. By using algorithm, we can obtain both intrinsic and extrinsic matrix. In short (The extrinsic matrix is a transformation matrix from the world coordinate system to the camera coordinate system, and the intrinsic matrix is a transformation matrix from the camera coordinate system to the pixel coordinate system.) After we obtain these matrix, we can use OpenCV libraries and algorithms to apply BEV Transformation accordingly. 

Image Stitching:
Obstacle Aviodance:
ROS2 Line Follower:


Future Advancement:
OCR:

## Team Members
Xiran Wang(MAE)
