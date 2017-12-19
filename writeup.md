## Self-Driving Car Engineer Nanodegree

### Advanced Lane Finding Project

---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[undistorted]: ./output_images/undistorted.jpg "Undistorted"
[distortion-corrected]: ./output_images/distortion-corrected.jpg "Distortion Corrected"
[binary]: ./output_images/binary.jpg "Binary"
[warp1]: ./output_images/warp1.jpg "Warp1"
[warp2]: ./output_images/warp2.jpg "Warp2"
[boxes]: ./output_images/boxes.jpg "Boxes"
[final]: ./output_images/final.jpg "Final"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.
[Link](https://github.com/anilmuthineni/CarND-Advanced-Lane-Lines/blob/master/writeup.md) is the link to this writeup.


### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the code cells 4-6 in the IPython notebook located in "./solution.ipynb"

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `object_point` is just a replicated array of coordinates, and `object_points` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `image_points` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `object_points` and `object_points` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![undistorted][undistorted]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![distortion-corrected][distortion-corrected]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image.  Here's an example of my output for this step.

![binary][binary]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `unwarp()`, which appears in the IPython notebook solution.ipynb. The `unwarp()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![warp1][warp1]
![warp2][warp2]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I identified lane line pixels in the method `polyfit()`, which appears in the IPython notebook solution.ipynb.
I used a histogram of pixels along x axis and identifies the starting positions of two lanes. After that I used sliding wondows to trace the path of the lane lines from the bottom of the image to the top of the image.

![boxes][boxes]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I identified lane line pixels in the method `curvature_radius_and_center_distance()`, which appears in the IPython notebook solution.ipynb. The curvature and centre position are calculated using the forlulae in the self driving car nano degree course.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

Here is an example of my result on a test image:

![final][final]


---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I spent significant time trying to figure out which color channel to use. I looked at RGB, HSV and HLS color spaces. After significant experimentation, I decided to use S channel in HLS color space. I also spent some time trying to understand how to use the gradients and which gradient to use to identify lane line pixels. I finally decided to use sobel_x since lane lines usually have high gradient across x axis.
