# P4_Line_Detection

## Objective

The objective of this project is the detection of lane lines in a video

## The procedure is the following

* Calibrate the camera, and compute the calibration matrix
* Undistort the images using the calibration information
* Compute a bird eye view
* Use color thresholdings to compute a binary image
* Detect line pixels, and fit a a curve to it
* Determine the curvature of the lines
* Project the results in the original image
* Display the results

[//]: # (Image References)

[Calibration_Image]: ./img/calibration.png "Camera calibration image"
[Undistorted]: ./img/undistort.png "Undistorted image"

[threshold]: ./img/warped_images_th.png "Threshold examples"

[bird_eye]: ./img/bird_eye.png "Bird eye view"


[line_detection]: ./img/line_detection_his.png "Line detection"
[line_tracking]: ./img/line_tracking.png "Line tracking"
[detected_lines]: ./img/detected_lines.png "Detected lines"
[annotated]: ./img/annotated_image.png "Result images"


# Project rubric

## 1. Writeup / README

This file represents the writeup report

## 2. Camera Calibration

#### Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

To calibrate the camera we do the following tasks:

a) Read the chessboard images
b) Find the corners for each image
c) Compute the calibration matrix using the object points and the image corners
d) Save the calibration data into a file

![alt text][Calibration_Image]


## 3. Pipeline (test images)

### 3.a Provide an example of a distorted-corrected image

Using the calibration matrix we can compute an undistorted image.

An example of the undistorted image is the following:
![alt text][Undistorted]

### 3.b Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image. Provide an example of a binary image result.

The thresholhing used in this project is based on a color threshold, which is computed using the def color_thresholds_BGR(img) or 
def color_thresholds_RGB(img), depending on the image color space BGR or RGB.

![alt text][threshold]

### 3.c Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

To perform the bird eye view we need two sets of points and then use the getPerspectiveTransform function.

In this project I implemented the function bird_eye_view_matrix(src,des), that returns the tranformation matrix (A) and its inverse (Ainv).

![alt text][bird_eye]

### 3.d Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

The line detection is performed using the histogram of the bottom half of the image, which is implemented in the find_lines function.

![alt text][line_detection]

Once that the lines are detected then we perfom a search based on the previous frames.
![alt text][line_tracking]

The algorithm takes into account previous results, these results were saved in Left and Right objects of class Line. The algorithm stores a maximum of 10 results, and uses the mean of the parameters to estimate the line.


### 3.e Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

The curvature and the position of the vehicle with respect to the center was computed at the end of the line_detection and line_tracking functions.

![alt text][annotated]

### 3.f Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

The results were plotted back using the inverse perspective matrix (Ainv).

![alt text][detected_lines]

## 3. Pipeline (video)

Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!)

Here is a [link to my video result](https://github.com/CarlosLF/P4_Line_Detection/blob/master/video_result.mp4)

Here is a dropbox link [dropbox link to my video result](https://www.dropbox.com/s/ggzlxfobum4f6fm/video_result.mp4?dl=0)

## 4. Discussion

Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?

The main problem of this project was the creation of a thresholded binary image. To create such binary image have to test with different color spaces (HLS,LUV, Lab). 

The current algorithm is able to detect lane lines in the project_video.mp4, however is not able to deal with the challenge_video.mp4 nor the harder_challenge_video.mp4. The main reason of the fail is that no lane line points are found in the image.

One of the main problems are the illumination changes, in addition to the shwadows of bridges, trees, etc. This illuminations changes are a challenge for the lane lines detection, for example if the image is to dark (night), then the color and gradient is almost lost.

Improvements

The video processing image can be improved by using an histogram equalization, a combination of gradient methods with color transforms.

The algorithm can be improved if the detection of dashed lines is more robust, these type of lines are problematic since they are projected with a small number of pixels.

The algorithm should detect and reject outliers.

