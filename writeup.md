## Advanced Lane Finding Project

### The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./camera_cal/calibration1.jpg "Chessboard Image"
[image2]: ./output_images/camera_cal_results/corners_found0.jpg "Corners on Chessboard"
[image3]: ./output_images/undistorted_images/undistorted_0.jpg "Undistorted Image"
[image4]: ./output_images/threshold_binary_images/binary_img5.jpg "Threshold Binary Image"
[image5]: ./output_images/perspective_transform/birds_eye_view_img5.jpg "Perspective transform to bird's eye view"
[image6]: ./output_images/lanes_found/lane_img5.jpg "Applying sliding window and fitting a polynomial"
[image7]: ./output_images/final_output.jpg "Final output"
[video1]: ./project_video_output.mp4 "Video"

---

## Advanced Lane finding Pipeline

My pipeline consists of 8 steps -

1. Calibrating the camera.
   The camera is calibrated using cv2.findChessboard() and cv2.drawChessboard() function for chessboard of 9X6 size.
   ![alt_text][image1]
   ![alt_text][image2]
   
2. Undistorting the image.
   Using object and image points from the above step to calibrate camera.
   Using the matrix obtained from above step to undistort the image using cv2.undistort() function.
   ![alt_text][image3]

3. Applying threshold to obtain a binary image.
   Threshold is applied on HLS converted image. Sobel x is calculated on s_channel.
   Threshold for channel is applied on all h,l and s channel.
   A binary image is obtained.
   ![alt_text][image4]

4. Applying perspective transform to obtain bird's eye view.
   Source and respective destination points are chosen.
   M is obtained by using cv2.perspectiveTransform() from src to dest.
   Minv is also obtained for further use.
   Bird's eye view is obtained by using cv2.warpPerspective().
   ![alt_text][image5]
  
5. Finding lane lines and fitting polynomial.
   Sliding window technique is used to find the lane lines on the warped binary image.
   ![alt_text][image6]

6. Find curvature of the lane.
   Left and right curverads are found and their average is taken as the curvature.
   Position of the vehicle is calculated considering vehicle is in the centre of the lane.

7. Show lane lines on original image.
   Detected lane lines are unwarped using the Minv and shown on the original image.
   ![alt_text][image7]

8. Sanity check.
   To check the validity of lane lines detected, the slope of both the lines is calculated at the mid point and compared.
   
---

## Output Video

Upon applying my pipeline to the given video, the below output video was obtained.
   
Here's a [link to my video result](./project_video_output.mp4)

---

## Challenges faced
   
While implementing the pipeline, one big challenge I faced was my out_img was being created with 9 dimensions.
On debugging and by taking help from the forum, I found out that my binary image was itself of 3/4 dimensions.
This was resolved by adding the below line before calling the fit_polynomial() function.
``` 
binary_warped=binary_warped[:,:,0]
```
  
---

## Possible Improvements

1. Instead of using sliding window for each step, I could've searched for lane lines around the detected lanes of previous frame. 
   After performing sanity check, if they're found to be robust they could've been used otherwise sliding window method could be used for 
   that frame.

2. The sanity check function can be improved further taking into consideration various characteristics of lane lines.
