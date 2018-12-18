# **Finding Lane Lines on the Road** 

[//]: # (Image References)

[grayscale]: ./examples/grayscale.jpg "Grayscale"
[solidWhiteCurve]: ./test_images/solidWhiteCurve.jpg "solidWhiteCurve"
[solidWhiteCurveOut]: ./test_images_output/solidWhiteCurve.jpg "solidWhiteCurveOut"
[solidWhiteRight]: ./test_images/solidWhiteRight.jpg "solidWhiteRight"
[solidWhiteRightOut]: ./test_images_output/solidWhiteRight.jpg "solidWhiteRightOut"
[solidYellowCurve]: ./test_images/solidYellowCurve.jpg "solidYellowCurve"
[solidYellowCurveOut]: ./test_images_output/solidYellowCurve.jpg "solidYellowCurveOut"
[solidYellowCurve2]: ./test_images/solidYellowCurve2.jpg "solidYellowCurve2"
[solidYellowCurve2Out]: ./test_images_output/solidYellowCurve2.jpg "solidYellowCurve2Out"
[solidYellowLeft]: ./test_images/solidYellowLeft.jpg "solidYellowLeft"
[solidYellowLeftOut]: ./test_images_output/solidYellowLeft.jpg "solidYellowLeftOut"
[solidYellowLeftWithShadow]: ./test_images/solidYellowLeftWithShadow.jpg "solidYellowLeftWithShadow"
[solidYellowLeftWithShadowOut]: ./test_images_output/solidYellowLeftWithShadow.jpg "solidYellowLeftWithShadowOut"
[solidYellowWithShadowAndWhiteRoad1]: ./test_images/solidYellowWithShadowAndWhiteRoad1.jpg "solidYellowWithShadowAndWhiteRoad1"
[solidYellowWithShadowAndWhiteRoad1Out]: ./test_images_output/solidYellowWithShadowAndWhiteRoad1.jpg "solidYellowWithShadowAndWhiteRoad1Out"
[solidYellowWithShadowAndWhiteRoad2]: ./test_images/solidYellowWithShadowAndWhiteRoad2.jpg "solidYellowWithShadowAndWhiteRoad2"
[solidYellowWithShadowAndWhiteRoad2Out]: ./test_images_output/solidYellowWithShadowAndWhiteRoad2.jpg "solidYellowWithShadowAndWhiteRoad2Out"
[whiteCarLaneSwitch]: ./test_images/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"
[whiteCarLaneSwitchOut]: ./test_images_output/whiteCarLaneSwitch.jpg "whiteCarLaneSwitchOut"

---


## Overview

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.
* First selected region of interest, so that to reduce unecessary image processing. Region of interest is lower half traingle of the rectangle.
* Then I selected white & yellow colors from selected region. Image is converted to HLS color scheme which helps in better color selection.
* Converted this to grayscale for further processing
* Applied Canny algorithm to detect edges from grayscale image
* Then detected hough lines using edges
* Then I fused above found edges & the original image to form the final output

In order to draw a single line on the left and right lanes, I modified the draw_lines() function.
* Seperated left & right lines by slope. If slope is greater than 0 then its left line or else its right line.
* Detected y2 by identifying minimum of them all for each right & left line.
* while processing, ignored horizontal lines all together to reduce noise/errors.
* By using slope formula i.e.
m = (y1 - y2) / (x1 - x2)
I calculated x1 where y1 = image.shape[0]. This will be lower edge of the line.
& used y2 as min of y on line and corresponding x as x2. This will be upper edge of the line.
where image.shape[0] is y height of the image
* Then drawn line for each left & right using x1, y1 & x2, y2

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][solidWhiteCurve]        ![alt text][solidWhiteCurveOut]
![alt text][solidWhiteRight]        ![alt text][solidWhiteRightOut]
![alt text][solidYellowCurve]        ![alt text][solidYellowCurveOut]
![alt text][solidYellowCurve2]        ![alt text][solidYellowCurve2Out]
![alt text][solidYellowLeft]        ![alt text][solidYellowLeftOut]
![alt text][solidYellowLeftWithShadow]        ![alt text][solidYellowLeftWithShadowOut]
![alt text][solidYellowWithShadowAndWhiteRoad1]        ![alt text][solidYellowWithShadowAndWhiteRoad1Out]
![alt text][solidYellowWithShadowAndWhiteRoad2]        ![alt text][solidYellowWithShadowAndWhiteRoad2Out]
![alt text][whiteCarLaneSwitch]        ![alt text][whiteCarLaneSwitchOut]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when tree is prominant in region of interest. This will cause lines to be detected out of actual lane lines. Thus affecting mean slope of lines in one set.

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to use gaussian blur, erosion & dilate functions. Currently without using these, I was able to get good edge detection. 

Another potential improvement could be to use mode of slopes instead of mean, for each set of lines (left & right). Which will reduce errors/noise of lines. Also, I can use KMeans to cluster slopes to remove noise.
Also, I have taken x2, y2 from existing lines. But in case of curves, it goes wrong. Needs improvement to detect lowest y2 amongst y points which are more commonly on single line. This will reduce the noise on the curve.
