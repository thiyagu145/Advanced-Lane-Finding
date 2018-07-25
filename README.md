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
After the image is undistorted, the next step is to identify the edges in the images. Lane markings are considered to be thick edges, and by proper thresholding the lane markings can be easily extracted. There are 4 different thresholdings used in the lane finding algorithm used. 
1. Absolute Sobel Magnitude threshold </br>
The sobel thresholded image can be obtained with the function **cv2.Sobel**. The images have to be obtained separately for x axis and y axis. We can threshold the image based on the absolute sobel edge values and we can combine both the x and y axes to get a single binary image. 
2. Gradient Threshold</br>
The previous threshold was based completely on the absolute values, but in gradient threshold we can threshold the image based on the gradient of the edge detected image. 
3. Direction threshold</br>
We want to detect only the lane markings and not any other information from the image. Since we know that the edges containing information about the lane markings will definitely belong to a range of particular angles. We need edges that oriented towards the y axis and this can be obtained by thresholding the image based on the edge angle using the sobel x and y images.
4. Color thresholding</br>
A lot of information can be obtained by changing the color spaces of the images. It is found that the lane markings are more easily extractable when the image is represented in the HLS color space as this color space contains the lightness as one of its parameters. The S channel is extracted and thresholded to get a identification of the lane markings. 

These are the four threhsolds that were part of the pre-processing techniques for lane finding algorithm. The different thresholds can be combined with different logics. The logic used here is to </br>
[((gradx == 1) & (grady == 1)) | ((mag_binary == 1) & (dir_binary == 1))]</br>
An example of the thresholded image is given below:
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.20.06%20PM.png)

## Perspective Transformation
With the help of the thresholded image, we can find out the lane markings distinctively but we cannot identify whether the lanes are straight or curved. A straight lane marking appears curved due to the conversion from world space to image space. To correct this issue, we need to have a bird's eye view of the lane to identify whether the lane is curved or not. This is called as warping and can be easily performed with the help of the **cv2.warpPerspective** which takes in the input arguments as the image and the transform matrix. The transform matrix is obtained by passing the source and the destination points to the **cv2.getPerspectiveTransform** function. The inverse transform matrix is needed to put the lane markings back on to the original image and it can be obtained easily by just swapping the source and destination points.
Example of the Perspective transform applied to the test images is given below:
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.19.57%20PM.png)
</br>
A combination of the pre-processing techniques along with the perspective transformation applied is given below:
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-24%20at%207.20.18%20PM.png)

## Detecting Lane Pixels
After the pre-processing techniques are applied, we have the warped thresholded image which give us a clear visualization of the lanes. The next step is to identify the lane pixels from the warped images and fit them with the polynomial to draw the lane lines over these pixels. The histogram with respect to the number of pixels and x-axis location are obtained and where the peaks are high, those regions are considered to be lines. Once the peaks are identified, all the pixels corresponding to these peaks are taken and their coordinates are used to find the best polynomial corresponding these lane markings. Once the polynomial functions for the left and the right lane are identified, the lines can be drawn over the lane pixels to show them on the original images. The following example shows the warped-thresholded image and the polynomial fitted on the thresholded image.
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-25%20at%2012.36.58%20PM.png)

## Determining the curvature and the vehicle position
Once the polynomials are fitted to the lane pixels, the next step is to find the curvature of the lane. The curvature of the lane can be determined by finding the radius of the circle which can approximate the curvature of the lane. This can be determinded with the help of the polynomial function fitted for both the left and the right lanes. The following function is used to measure the curvature of the lane. 
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/other/Screen%20Shot%202018-07-25%20at%201.34.14%20PM.png)

Once the curvature is found out, we can find the vehicle position corresponding to the centre of the lane by finding the starting pixels of the lane for the left and right lines with respect to the x-axis. The mid-point of these two starting lane points will give us the vehicle position with respect to the centre of the lane. 

## Warping lane lines back to the original image
Once the lane lines and the curvature measurements are obtained, the next step is to warp the lane lines back onto the original image so that we can visualize the image with the lane markings. The inverse perspective transform matrix is required for this step. Once the polynomial functions for the lane lines are obtained, we can use the **cv2.follpoly** function to indicate the lane regions with a specific color. Once the image containing the lane markings and the indications is obtained, inverse perspective transform can be applied and the imposed on the original image using the **cv2.addweighted** function. This is how the lane lines are marked on the original image. The lane detection algorithm applied on two of the test images is given below: 
![alt text](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/output_images/Screen%20Shot%202018-07-25%20at%201.47.16%20PM.png)

## Applying the lane finding algorithm on the given video
The result of applying the advanced lane finding algorithm applied on the given video is given below:
![Watch the video](https://github.com/thiyagu145/Advanced-Lane-Finding/blob/master/project_video.mp4)
