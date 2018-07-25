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

## Camera Calibration and Undistortion
Camera calibration is done with the help of openCV functions. The first step is to create an empty array for image points(2d points in image space) and object points(3d points in world space). The camera is calibrated by using the **cv2.findchessboardcorners** function. Chess boards are taken at different angles using the camera that is to be calibrated and once the corners are obtained, we can use the **cv2.calibratecamera** function to get the distortion coefficients. Once the coefficients are obtained, we can use them to undistort the images using the **cv2.undistort** function. An example of the chess board image before and after undistortion is given below:

![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.19.42%20PM.png)

An example of the undistortion applied on the test images is also given below:
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.19.49%20PM.png)

## Color and Gradient Thresholding
After the image is undistorted, the next step is to identify the edges in the images. Lane markings are considered be thick edges, and by proper thresholding the lane markings can be easily extracted. There are 4 different thresholdings used in the lane finding algorithm used. 
1. Absolute Sobel Magnitude threshold
The sobel thresholded image can be obtained with the function **cv2.Sobel**. The images have to be obtained separately for x axis and y axis. We can threshold the image based on the absolute sobel edge values and we can combine both the x and y axes to get a single binary image. 
2. Gradient Threshold
The previous threshold was based completely on the absolute values, but in gradient threshold we can threshold the image based on the gradient of the edge detected image. 
3. Direction threshold
We want to detect only the lane markings and not any other information from the image. Since we know that the edges containing information about the lane markings will definitely belong to a range of particular angles. We need edges that oriented towards the y axis and this can be obtained by thresholding the image based on the edge angle using the sobel x and y images.
4. Color thresholding
A lot of information can be obtained by changing the color spaces of the images. It is found that the lane markings are more easily extractable when the image is represented in the HLS color space as this color space contains the lightness as one of its parameters. The S channel is extracted and thresholded to get a identification of the lane markings. 

These are the four threhsolds that were part of the pre-processing techniques for lane finding algorithm. An example of the thresholded image is given below:
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.20.06%20PM.png)
