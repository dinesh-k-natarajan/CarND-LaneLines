# **Finding Lane Lines on the Road** 

## Writeup for Project 1

Submitted by Dinesh K. Natarajan

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_output/solidWhiteRight.jpg 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The Lane Detection pipeline using the basic principles of Computer Vision was implemented using helper functions based on the OpenCV library as follows: 

1. The image is first converted to Grayscale. 

![alt text][image1]


2. Gaussian smoothening is done using Gaussian Blur and parameterized kernel size.

3. Canny Edge Detection is applied on the smooothened grayscale image using tunable threshold parameters.

4. A quadrilateral is used to apply Region Masking to the image with edges to identify the region of interest.

5. Hough Transform is performed with the aid of the parameters to identify the lane markings and to display the identified line segments.

6. The draw_lines() function is modified to extrapolate the line segments into single left and right lane lines. The following modifications were made to add this functionality: 

* The line segments identified by the Hough Transform are split into left and right lane lines based on the slope of the line segments. 
* The slopes (m's) and the intercepts(b's) from the equation of a straight line y = mx + c are calculated for each line segment, in order to find the average slope and intercept of the left and right lane group respectively. Thus, an "average" of all the line segments on each lane are found. 
* Now, using the equation for each lane, the vertices for the lanes can be extrapolated using the known values of y (from the vertices of the Region of Interest). 
* Using the newly found coordinates of the lane extremities, the final solid lane markings are superimposed and highlighted on the raw image. 

7. The final image after passing through the pipeline looks like: 

![alt text][image2]



### 2. Identify potential shortcomings with your current pipeline

1. The current implementation of my pipeline cannot extrapolate higher order polynomials, which would be observed in curved roads. As the pipeline only handles linear extrapolation, it fails to properly detect and mark the lanes in the Challenge video with turns and curves. 

2. The line segments are grouped into left and right solely based on the nature of their slopes. This is not robust enough to handle any sharp turns or higher order curves which might be wrongly classified as the other lane. 


### 3. Suggest possible improvements to your pipeline

1. In the lectures, it was discussed that Hough Transform is used to transform straight lines and points from Image Space to the Hough Space. The effectiveness of the Hough Transform for non-linear lanes is to be studied.  

2. Parameter Tuning is cumbersome and a better solution to optimize the parameters involved in the pipeline would greatly improve this simple Computer Vision based Lane Detection Algorithm. 
