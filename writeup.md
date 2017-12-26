
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

[image_cam_bef]: ./camera_cal/calibration4.jpg "Distorted"
[image_cam_now]: ./camera_cal/calibration4_undist.jpg "Undistorted"
[image_road]: ./test_images/straight_lines1_rgb_undist.jpg "Road Transformed"
[image_binary]: ./test_images/straight_lines1_binary.jpg "Binary Example"
[warped]: ./test_images/warped.png "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

In order to correct for camera lens distortion, we learn the camera calibration parameters using 20 checkerboard images taken at different perspectives. Specifically, we first pick out checkerboard corners by applying `cv2.findChessboardCorners` on each image, and using `cv2.calibrateCamera` to learn the map between image corner points and object points, which would be the ground truth points in real space.

Here is an example of image without calibration.
![alt text][image_cam_bef]

After applying the `cv2.calibrateCamera` on the above image with calibrated parameters (also grayscaled now), we can get rid of lens distortion artifacts such as the curvatures of the board on its edges.
![alt text][image_cam_now]


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

Applying the distortion-correcting transform on a test image (straight_lines1.jpg), we get the following result:
![alt text][image_road]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

To extract lane gradients robustly, we combine the x-directional sobel gradient-thresholded binary map, which picks up the near vertical property of lanes, and the s-channel (in HLS channel domain) thresholded binary map, which has good illumination-invariance, using the pixelwise OR operation. Shown below is our binary map of the distortion-corrected straight_lines1 image.

![alt text][image_binary]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The perspective transform computations are done in the cell under the markdown title "test images".
The source points were chosen to be the corners of trapezoid containing the two lanes and destination points were chosen to contain a sizable rectangle:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 200, 700      | 300, 720        | 
| 1080, 700      | 980, 720      |
| 590, 450     | 300, 0      |
| 690, 450      | 980, 0 |

The image below shows the perspective transform in action -- on the left is the raw image, and on the right is the result of the transform, parallelizing the lanes in the image domain.

![alt text][warped]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_test.mp4), which for each frame shows estimated lanes, radius of curvature, and relative shift of car from center of road.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
