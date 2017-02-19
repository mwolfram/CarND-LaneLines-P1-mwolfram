#**Finding Lane Lines on the Road** 

##Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

First, I converted the images to grayscale, then I applied a gaussian blur with a kernel size of 5. On that image I ran canny edge detection with the thresholds from the course and chose a polygon-shaped mask to concentrate only on the relevant parts of the image. I then ran a hough line detection and combined the resulting lines with the original image to get a first rudimentary lane detection result.

I then spent some time finding better parameters for the HoughLinesP function. I tried closing the gaps in the dashed lane lines, that worked out except for a few gaps that were just too long and probably would have rendered the rest of the detection unstable. As sometimes the algorithm wouldn't detect any line, I increased rho and reduced the number of necessary votes in order to be less strict.

This is what I got:

### TODO images

#### Modifying the draw_lines() function
In order to draw a single line on the left and right lanes, I modified the draw_lines() function by first converting each line to m and b parameters for slope and offset (ignoring the ones that would cause calculation errors). Then I looked at the resulting m values and defined thresholds to either classify them as the left or right lane line. The corresponding m and b values were averaged, so in the end I had exactly two pairs of m and b. For each one of them, I calculated the points where they would intersect the horizontal bottom line of the image and the horizontal (almost-)center of the image. These were the points I passed to the original line-drawing function.


### TODO images
Here are the resulting images: 

![alt text][image1]


###2. Identify potential shortcomings with your current pipeline

The pipeline yields good results on the first two videos, but the more complex challenge showed me that it has some shortcomings.

* The area of interest is hardcoded. Even though I use imshape, I have some hardcoded offsets in there. This is a disadvantage when using images of various resolution for example. Also, if the camera is not in the center of the vehicle, those offsets would have to change.
* I don't think that a car driving in front of me can currently be handled without issues
* The pipeline is optimized for straight or not too curvy lanes.
* Changing lighting conditions / mixing up bright and dark are not accounted for

###3. Suggest possible improvements to your pipeline

* The area of interest could be detected automatically... HOW?
* One could try to detect curves in the image
