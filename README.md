# PyOCVLaneDetect
PythonOpenCV Based Lane Detect for Self-Driving Car

**The Goals**

* Compute Camera Calibration
* Get Distortion Coefficients
* Apply Distortion Correction
* Apply Color Transform and Gradients
* Apply Perspective Transform
* Detect Lane Area
* Detect Left & Right Lane
* Determine the Curvature of the Lane & Vehicle Position
* Find Disadvantage to make more Good
* How to improve it

[//]: # (Image References)

[test]: ./output_images/test-undist.png "Dist to Undist"
[origin]: ./output_images/origin.jpg "Original Image"
[undistort]: ./output_images/after_undistort.jpg "Undistortion Image"
[perspective]: ./output_images/after_pt.jpg "Perspective Image"
[extract_red]: ./output_images/extract_red_after_pt.jpg "Extract Red"
[histo_equ]: ./output_images/histogram_equalization.jpg "Histogram Equalization"
[get_strongest]: ./output_images/get_strongest_at_equ.jpg "Get Strongest"
[sobel_x_filter]: ./output_images/sobel_x_filter_with_red.jpg "Sobel X Filter"
[combine]: ./output_images/sobel_n_equ.jpg "Combine Sobel & Hist Equalization"
[get_white]: ./output_images/get_white.jpg "Get White"
[left_lane_rect0]: ./output_images/make_left_lane_rect0.jpg "Left Lane Rect 0"
[right_lane_rect0]: ./output_images/make_right_lane_rect0.jpg "Right Lane Rect 0"
[left_lane_rect1]: ./output_images/make_left_lane_rect1.jpg "Left Lane Rect 1"
[right_lane_rect1]: ./output_images/make_right_lane_rect1.jpg "Right Lane Rect 1"
[left_lane_rect2]: ./output_images/make_left_lane_rect2.jpg "Left Lane Rect 2"
[right_lane_rect2]: ./output_images/make_right_lane_rect2.jpg "Right Lane Rect 2"
[left_lane_rect3]: ./output_images/make_left_lane_rect3.jpg "Left Lane Rect 3"
[right_lane_rect3]: ./output_images/make_right_lane_rect3.jpg "Right Lane Rect 3"
[left_lane_rect4]: ./output_images/make_left_lane_rect4.jpg "Left Lane Rect 4"
[right_lane_rect4]: ./output_images/make_right_lane_rect4.jpg "Right Lane Rect 4"
[left_lane_rect5]: ./output_images/make_left_lane_rect5.jpg "Left Lane Rect 5"
[right_lane_rect5]: ./output_images/make_right_lane_rect5.jpg "Right Lane Rect 5"
[left_lane_rect6]: ./output_images/make_left_lane_rect6.jpg "Left Lane Rect 6"
[right_lane_rect6]: ./output_images/make_right_lane_rect6.jpg "Right Lane Rect 6"
[left_lane_rect7]: ./output_images/make_left_lane_rect7.jpg "Left Lane Rect 7"
[right_lane_rect7]: ./output_images/make_right_lane_rect7.jpg "Right Lane Rect 7"
[left_lane_rect8]: ./output_images/make_left_lane_rect8.jpg "Left Lane Rect 8"
[right_lane_rect8]: ./output_images/make_right_lane_rect8.jpg "Right Lane Rect 8"
[need_perspective_rect]: ./output_images/need_perspective_rect.jpg "Need Perspective Rect"
[perspective_rect]: ./output_images/perspective_rect.jpg "Perspective Rect"
[render_lane_area]: ./output_images/rendering_green_lane_area.jpg "Lane Area"
[need_perspective_left]: ./output_images/need_perspective_left_rect.jpg "Need Perspective Left"
[perspective_left]: ./output_images/perspective_left_rect.jpg "Perspective Left"
[display_left]: ./output_images/display_left_rect.jpg "Display Left"
[need_perspective_right]: ./output_images/need_perspective_right_rect.jpg "Need Perspective Right"
[perspective_right]: ./output_images/perspective_right_rect.jpg "Perspective Right"
[display_left_right]: ./output_images/rendering_left_right_lane.jpg "Display Left & Right"
[final_out]: ./output_images/final_output.jpg "Final Output"
[solid_white]: ./output_images/solidWhite.gif "Solid White Right"
[solid_yellow]: ./output_images/solidYellow.gif "Solid Yellow Left"
[challenge]: ./output_images/challenge.gif "Challenge"
[superior]: ./output_images/superior.gif "Superior Challenge"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
### Here I explained each steps, references, disadvantage and how to improve it.

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.

This is the README file.

### Camera Calibration

#### First I can found Camera Calibration reference. It explained how to compute Camera Matrix, Distortion Coefficients and how to apply it.

<p>http://docs.opencv.org/3.1.0/dc/dbb/tutorial_py_calibration.html</p>
<p>https://github.com/paramaggarwal/CarND-Advanced-Lane-Lines</p>

This code use the mathematical model of Pinhole Camera. So if I use the other then have to change the model. This is the first disadvantage. There are very many kinds of Camera Model.

The code start by preparing Object Points which will be represent the 3D coordinates(x, y, z). 3D world in computer will be represent at 2D Monitor. So I can fixed z = 0. I know the Real World coordinates of Object. And I know the Virtual World coordinates of Object. So I can calibrate it. cv2.calibrateCamera() support it. If I use this function then we can get the Camera Matrix and Distortion Coefficients. And I can use cv2.undistort() to make Distortion Image to Undistortion Image.

![alt_text][test]

However we can have one more problem that is the how to represent Perspective View. I'll explain it to below.

### Pipeline

#### 1. Distortion Image to Undistortion Image

It's already explained above stage. And if we apply it to the video image then it looks like below.

![alt_text][origin]  ![alt_txt][undistort]

Left is origin Image and right is Undistortion Image.

#### 2. Find the ROI and Orthogonal Transform(Inverse Perspective Transform)

I have interest at the curved lane. So I have to know where I make the lane. I decided the region and apply Inverse Perspective Transform for after transform.

![alt_text][perspective]

#### 3. Extract Red Channel

I extract the red channel with image[:,:,0]. Below is the result. This is the second disadvantage. It has the color dependency.

![alt_text][extract_red]

#### 4. Histogram Equalization

I can found the reference of Histogram Equalization.

<p>http://docs.opencv.org/3.1.0/d5/daf/tutorial_py_histogram_equalization.html</p>
<p>http://docs.opencv.org/3.1.0/d4/d1b/tutorial_histogram_equalization.html</p>

It makes histogram of image more smooth.

![alt_text][histo_equ]

#### 5. Get Strongest Red Channel.

Use the threshold value to 250 ~ 255

![alt_text][get_strongest]

#### 6. Apply Sobel X Filter

Sobel X Filter can detect change of horizontal.

![alt_text][sobel_x_filter]

#### 7. Mix Sobel X Filter & Strongest Red Channel

![alt_text][combine]

#### 8. Get White

![alt_text][get_white]

#### 9. Fitting Curve with Regression Analysis

I use quadratic function to fitting curve. Maybe Hyperbolic function, spline, or bezier curve is much greater than it. This is the third disadvantage.

![alt_text][left_lane_rect0]  ![alt_text][right_lane_rect0]
![alt_text][left_lane_rect1]  ![alt_text][right_lane_rect1]
![alt_text][left_lane_rect2]  ![alt_text][right_lane_rect2]
![alt_text][left_lane_rect3]  ![alt_text][right_lane_rect3]
![alt_text][left_lane_rect4]  ![alt_text][right_lane_rect4]
![alt_text][left_lane_rect5]  ![alt_text][right_lane_rect5]
![alt_text][left_lane_rect6]  ![alt_text][right_lane_rect6]
![alt_text][left_lane_rect7]  ![alt_text][right_lane_rect7]
![alt_text][left_lane_rect8]  ![alt_text][right_lane_rect8]

#### 10. Rendering Lane Area

Below Image Area means the Lane Area

![alt_text][need_perspective_rect]

We need to change it to Perspective View

![alt_text][perspective_rect]

Mix with Undistortion Image

![alt_text][render_lane_area]

#### 11. Rendering Left Lane

Same as Lane Area. This is just for Left Lane

![alt_text][need_perspective_left]

![alt_text][perspective_left]

![alt_text][display_left]

#### 12. Rendering Right Lane too

It's same as Lane Area. Just for Right Lane

![alt_text][need_perspective_right]

![alt_text][perspective_right]

![alt_text][display_left_right]

#### 13. Add curvature

![alt_text][final_out]

#### 14. Summary of Disadvantage

I found the disadvantage of this pipeline.

1. It has the dependency of Camera Model.
2. Dependency of Color
3. Curve Fitting Model(Quadratic Function)

#### 15. Results

![alt_text][solid_white]
![alt_text][solid_yellow]
![alt_text][challenge]

Final Image is my superior challenge. However the result was so terrible.

![alt_text][superior]

#### 16. How to improve the pipeline ? (More Advanced)

I think if we improve the algorithm then we have to use RANSAC algorithm. And we have to change the curve fitting model to hyperbolic function, spline or bezier curve. And one more we need to detect advanced edges. That is Laplacian Filter. If we implement like it then we can remove all the above disadvantage. Anyway if we do that then the logic will be like below.

1. Setting ROI
2. Apply Gaussian Filter
3. Apply Canny Edge Filter or Laplacian Filter
4. Apply Hough Transform
5. Get Slope
6. Apply RANSAC Algorithm
7. Remove Outliers
8. Fitting Lane

I have no enough time to make it. So I'll study about the algorithm and do it by C++ or VHDL with enough time. After few times later I'll add it to my repo.

#### 17. References

[1] Juneja, M., & Sandhu, P. S. (2009). Performance evaluation of edge detection techniques for images in spatial domain. International Journal of Computer Theory and Engineering, 1(5), 614.

http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.457.535&rep=rep1&type=pdf

[2] Yoo, H., Yang, U., & Sohn, K. (2013). Gradient-enhancing conversion for illumination-robust lane detection. IEEE Transactions on Intelligent Transportation Systems, 14(3), 1083-1094.

https://pdfs.semanticscholar.org/2bce/94e1f0d921d6876cf346103f5f3e121bfdd8.pdf

[3] McCall, J. C., & Trivedi, M. M. (2006). Video-based lane estimation and tracking for driver assistance: survey, system, and evaluation. IEEE transactions on intelligent transportation systems, 7(1), 20-37.

http://escholarship.org/uc/item/1bg5f8qd

[4] Macro Zuliani(2008 - 2010). RANSAC for Dummies With examples using the RANSAC toolbox for Matlab & Octave and more ...

http://www.ic.unicamp.br/~rocha/teaching/2012s1/mc949/aulas/ransac-4-dummies.pdf

[5] Amol Borkar, Monson Hayes, & Mark T. Smith. (2009). ROBUST LANE DETECTION AND TRACKING WITH RANSAC AND KALMAN FILTER

https://pdfs.semanticscholar.org/8469/c6bc70c7314a69c0736ac59b84baa402d088.pdf

[6] Lan-Rong Dung, Chang-Min Huang, Yin-Yi Wu. (2013). Implementation of RANSAC Algorithm for Feature-Based Image Registration

https://file.scirp.org/pdf/JCC_2013112614150696.pdf

[7] https://campushippo.com/lessons/detect-highway-lane-lines-with-opencv-and-python-21438a3e2

[8] https://medium.com/@mrhwick/simple-lane-detection-with-opencv-bfeb6ae54ec0

[9] https://medium.com/@ldesegur/a-lane-detection-approach-for-self-driving-vehicles-c5ae1679f7ee

[10] http://www.bogotobogo.com/python/OpenCV_Python/python_opencv3_Image_Gradient_Sobel_Laplacian_Derivatives_Edge_Detection.php

[11] https://github.com/paramaggarwal/CarND-Advanced-Lane-Lines

[12] http://docs.opencv.org/3.1.0/dc/dbb/tutorial_py_calibration.html

[13] http://docs.opencv.org/3.1.0/d5/daf/tutorial_py_histogram_equalization.html

[14] http://docs.opencv.org/3.1.0/d4/d1b/tutorial_histogram_equalization.html
