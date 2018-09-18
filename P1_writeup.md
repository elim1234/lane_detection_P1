# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:
1. Convert image to grayscale.
2. Apply a Gaussian filter to reduce noise.
3. Apply a Canny filter to detect edges.
4. Apply a mask to ignore detections outside of the region of interest.
5. Apply a Hough transform to detect lines.

In order to draw a single line on the left and right lanes, I computed the average slope and intercept (essentially a Hough transform) on each side, identifying left and right segments based on the sign of the slope. Then I extended the left and right line equations to the bottom of the image and the top of the road.

### 2. Identify potential shortcomings with your current pipeline

There are shortcomings evident in the resulting videos. The detections blink off during frames when longer lines roll off the screen leaving only short lines that were not detected. The detections are jittery because there is no frame-to-frame information transfer. When the lines and the surface have comparable pixel intensity, the lines are not detected. The implementation does not handle the case when more than two lines are detected.

### 3. Suggest possible improvements to your pipeline

There are a number of enhancements I could add to improve the result:
1. I could compute a weighted average of slope and intercept instead, weighted by the length of the line segments.
2. I could apply a low-pass filter such that the current estimate is a weighted average of the current result and the previous result. This would reduce the jumpiness of the solution.
3. I could add logic to return the previous solution if no segments were detected in the current frame. This would eliminate frames with no lane detection.
4. Color-based filtering can be used to get better contrast when the lane line and the road surface have comparable gray-scale intensity.
5. When more than two lines are detected, logic can be applied to select only the two closest to the center.
6. And obviously there is a lot to be gained by further tuning the parameters.
