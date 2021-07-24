# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/output_solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images/output_solidWhiteRight.jpg "solidWhiteRight"
[image3]: ./test_images/output_solidYellowCurve.jpg "solidYellowCurve"
[image4]: ./test_images/output_solidYellowCurve2.jpg "solidYellowCurve2"
[image5]: ./test_images/output_solidYellowLeft.jpg "solidYellowLeft"
[image6]: ./test_images/output_whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"
---

### Reflection

### 1. My Lane Detection Pipeline Steps.
1.  Read each image from the test_images folder.
2.  Convert the image to gray scale.
3.  Smoothen the noise in the gray scale image using Gaussian Filter.
4.  Detect the edges in the image using Canny Algorithm. Where low_threshold:high_threshold = 50:150 = 1:3.
5.  Defined the region of interest (polygon) for the masking. Based on the percentage width and percentage height defined the following as the polygon vertices. 
			**[[(int(0.49*width), int(0.59*height)), (int(0.10*width), height), (int(0.94*width), height), (int(0.51*width), int(0.59*height))]]**
6.  Once we have the image with the edges detected in the region of interest (polygon), applied probabilistic hough lines function with the arguments rho : 1 pixel, theta: pi/180, threshold: 10, minLineLength: 15 and maxLineGap: 5, to detect the lines.
7.  In order to draw a single line on the left and right lanes, I added a new function find_single_solid_lines(), which does the following.      
			1. 		For each line detected above, the given end points are (x1, y1) and (x2, y2) and the image size is given.
			2. 		The x-coordinate value of the Left line points should be less than the polygon top left vertex. Similarly the x-coordinate value of the Right line points should be greater than the polygon top right vertex.
			3. 		Find the slope of the line using formula m = ((y2-y1)/(x2-x1))
			4. 		Find the center point of the line using the formula c = [(x1+x2)/2, (y1+y2)/2].
			5. 		The line with infinite slope (vertical line) and near horizontal lines (slope ~= 0) is ignored.
			6. 		Average Slope of the Right Lines and Left Lines is calculated. Average center point of the right and left lines is calculated.
			7. 		Finally Right line and Left line end points are calculated by using corresponding slope and y as img.shape[0], int(0.65*img.shape[0]).
8. draw_lines() - Draw these solid left and right lines on a blank image of the same size of original image. 
9. weighted_img() - Super impose the lines detected on the original image
10. If the processing is done by process_image() function for the video frames, then before draw_lines() function call, added a new function for the sanity check. This function compares each frame left and right line end points lie with in the a circle of the radius 18 pixels and the center point as previous frame corresponding end points. If they are not in the rage of the circle returns the previous end points of the line. The processing is performed for each line end point separately.
			
The final output images are
    
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be when the deviation between the slope values of the right or left lines is huge then the average slope value might not be accurate. Especially in the curvy roads. 
