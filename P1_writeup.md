# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteCurve.jpg "Hough Line Segments"
[image3]: ./test_images_output/avg_lines_solidWhiteCurve.jpg "Hough Average Lines"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I run a Gaussian smoothing filter with a kernel of size 7 over the image. I then run the Canny Edge detector ('low_threshold' = 65, 'high_threshold' = 135), which filters for edges with pixel values between the thresholds set. A mask is then used to crop the section that the lane lines will be identified, using four vertices to demarcate the 'region_of_interest'. A Hough Transform is then run to identify and label line segments, where then a 'weighted_image' is created by overlaying these hough lines on top of the image's lane lines.

From initial grayscale image:

![Grayscale Image][image1]

To Overlayed Hough Line Segments:

![Hough Line Segements Image][image2]

This constitutes the initial lane finding implementation.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by creating a new function 'draw_lines_avg'. The line segments were comprised of 'x1','y1','x2','y2' which are the endpoints of the line segment. By taking (('y2'-'y1')/('x2'-'x1')), I calculate the m value and take the array of line segments output by the Hough Transform and to separate them into left and right lane line segments based on the slope of the line segment. Afterwards I average the position of each of the lines and extrapolate them to be at the top and bottom of the lane. To avoid small line segments biasing the extrapolated line, I do not add to the final array to be averaged any lines that are less than a length of 2. These lines are averaged to calculate a resulting single left and right lane line and transformed back to the image, where it is then overlaid.

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the image has varying brightness, which would then affect the Canny Edge detection since it uses absolute brightness value thresholds.

Another shortcoming could be it tends to detect false positive short line segments and also assumes that the lane is in a fixed region relative to the camera which it isn't necessarily.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to filter out background brightness and use this value to calculate relative thresholds for the Canny Edge detection.

Another potential improvement could be to filter out lines that exceed slopes of typical lane lines/curvatures.
