#**Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[challenge1_original]: ./challenge1_original.png "Original Image"
[challenge1_highlight]: ./challenge1_highlight.jpg "Left lane line drifts toward center"

---

### Reflection

###1. Pipeline Description

My pipeline consisted of 6 steps, derived from the "Finding Lane Line" tutorial.

1) Convert image to grayscale
2) Apply Gaussian Smoothing
3) Find edges in the image using Canny Edge Detector
4) Crop the image to focus on the region of interest
5) Find the Hough Tranform to find lines
6) Draw lane lines on the image

###2. draw_line() function Description:
1) Iterate through all lines generated by Hough Transform

2) Ignore horizontal lines using slope threshold.
This step is vital for the challenge video because the shadow of the trees, clutter on the road, and the change in road color create near-horizonal lines.

3) Split lines into left or right lanes using the line's slope.
Left Side - Negative Slope | Right Side - Positive Slope

4) Ignore lines whose slope is drastically different from median slope for the lane line
Track median slope of left and right side using static variable
The lane lines should not change drastically between frames

5) For remaining set of points, fit line using numpy.polyfit

6) Generate new coefficents using a moving average
current_coefficients = (1 - epsilon) * new_coefficients + epsilon * prev_coefficients 
new_epsilon = min(old_epsilon + delta, maximum) where delta = 0.1 and maximum = 0.90

7) If the number of remaining points is zero, use previous lane line

8) Using the coefficients from numpy.polyfit fit a line from the top and bottom of the region of interest window

9) Generate lane lines using the left and right set of lines

Improvements for Challenge Video:
1) Near-Horizontal line filtering
2) Median Slope filtering
3) Moving Average of lane line parameters

###2. Identify potential shortcomings with your current pipeline

In the challenge video, the left lane line does not follow the yellow line drifts a bit towards the right. During the section of the video where the road color changes from black to white, the yellow line is difficult to see, so my model relies on the previous lane line. A solution is to use a different color space where white and yellow do not appear similarly such that the edge detector can find the lane line more easily.

![alt text][challenge1_original]
![alt text][challenge1_highlight]

###3. Suggest possible improvements to your pipeline

1) Improve the line detection of the left yellow line in the challenge video

2) The lane line should follow the curve of the road instead of being two parallel lines

