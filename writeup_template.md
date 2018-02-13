## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

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

[image1]: ./examples/undistortion.png "Undistorted image"
[image2]: ./examples/undistorted_test_image.png "Undistorted test image"
[image3]: ./examples/binary_image.png "Binary image"
[image4]: ./examples/perspective_transform.png "Perspective transform"
[image5]: ./examples/polyfit.png "Polyfit"
[image6]: ./examples/line_area.png "Line area"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. 

You're reading it.

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the second and third code cell of the IPython notebook located in "./advanced-lane-lines.ipynb".

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Undistorted image][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

I used the output from the previous step and the `cvs.undistort()` function to undistord the images:
![Undistorted test image][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

In the section "create binary image" I created the binary image.
I tried different methods. First I converted the image into a grayscale image, into the hls color space and into hsvl color space. Then I applied a sobel in x and y direction on the grayscale image. I also tried the direction and the magnitude of the gradient, but I was not convinced by the result. I also use a threshold on the s-channel of the hsl-picture and a threshold on the v-channel of the hsv-picture. I saw the convertion into the hsv color space in the following video https://www.youtube.com/watch?v=vWY8YUayf9Q&list=PLAwxTw4SYaPkz3HerxrHlu1Seq8ZA7-5P&index=4.
So in the end I used a combination to generate a binary image. Here's an example of my output for this step:

![Binary image][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warp_image()`, which appears in the section "perspective transform".  The `warp_image()` function takes as inputs only an image (`img`). The source and destination points are defined in this function.  I chose the source and destination points in the following manner:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 220, 720      | 220, 720      | 
| 570, 470      | 220, 0        |
| 720, 470      | 1110, 0       |
| 1110, 720     | 1110, 720     |

The points are hard coded. They shall be a function of the image size because this could be lead to problems if the image size is not the same.

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![Perspective transform][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I took the code that is defined in the classroom and fitted a 2D polynomial on the lines. That can be found in the sections "Sliding window and polyfit" and "Use fit from previous frame":

![Polyfit][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in the sections "Radius of curvature" and "Center offset".

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in the section "Show lane area". Here is an example of my result on a test image:

![Line area][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
