# Advanced-Lane-Finding
This is the fourth project in Udacity Self Driving Car Nanodegree. The objective of the project is to find the lane markings using advanced computer vision techniques, from a video recorded using a camera placed on the hood of the car. The main code is contained in the jupyter notebook and the output images folder contains the images obtained after different stages of pre-preocessing namely gradient thresholding, color thresholding, undistorting and warping. The test_images folder contains the images using which the advanced lane finding algorithm is checked. **final1.mp4** contains the output of the lane finding algorithm applied on the **project_video.mp4**. 

The advanced lane finding algorithm is mentioned below:
* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

## Camera Calibration
Camera calibration is done with the help of openCV functions. The first step is to create an empty array for image points(2d points in image space) and object points(3d points in world space). The camera is calibrated by using the **cv2.findchessboardcorners** function. Chess boards are taken at different angles using the camera that is to be calibrated and once the corners are obtained, we can use the **cv2.calibratecamera** function to get the distortion coefficients. Once the coefficients are obtained, we can use them to undistort the images using the **cv2.undistort** function. An example of the chess board image before and after undistortion is given below:

![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.19.42%20PM.png)

An example of the undistortion applied on the test images is also given below:
![alt text] (https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.19.49%20PM.png)
