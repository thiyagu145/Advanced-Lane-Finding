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
