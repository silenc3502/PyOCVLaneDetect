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
* Determine the Curvature of the Lane & Vehicle Position.
* Find Disadvantage to make more Good.

[//]: # (Image References)

[origin]: ./output_images/origin.jpg "Original Image"
[undistort]: ./output_images/after_undistort.jpg "Undistortion Image"
[perspective]: ./output_images/after_pt.jpg' "Perspective Image"
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
[perspective_right]: ./output_images/need_perspective_right.jpg "Perspective Right"
[display_right]: ./output_images/rendering_left_right_lane.jpg "Display Left & Right"
[final_out]: ./output_images/final_output.jpg "Final Output"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points
### Here I explained each steps, references, disadvantage and how to improve it.

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.

This is the README file.

### Camera Calibration

#### 1. First I can found Camera Calibration reference. It explained how to compute Camera Matrix, Distortion Coefficients and how to apply it.

http://docs.opencv.org/3.1.0/dc/dbb/tutorial_py_calibration.html
https://github.com/paramaggarwal/CarND-Advanced-Lane-Lines
