
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
			**[[(int(0.49*width), int(0.59*height)), (int(0.15*width), height), (int(0.94*width), height), (int(0.51*width), int(0.59*height))]]**
6.  Once we have the image with the edges detected in the region of interest (polygon), applied probabilistic hough lines function with the arguments rho : 1 pixel, theta: pi/180, threshold: 10, minLineLength: 15 and maxLineGap: 5, to detect the lines.
7.  In order to draw a single line on the left and right lanes, I modified the draw_lines() function by doing the following
		* 			  For each line detected above, the given end points are (x1, y1) and (x2, y2)  and the image size is given.
		* 			  The x-cordinate value of the Left line points should be less than the polygon top left vertex. Similary the x-cordinate value of the Right line points should be greater than the polygon top right vertex.
		* 			  Find the slope of the line using formula m = ((y2-y1)/(x2-x1))
		* 			  Find the center point of the line using the formula c = [(x1+x2)/2, (y1+y2)/2].
		* 			  The line with infinite slope (vertical line) and near horizontal lines (slope ~= 0) is ignored.
		* 			  Average Slope of the Right Lines and Left Lines is calculated. Average center point of the right and left lines is calculated.
		* 			  Finally Right line and Left line end points are calculated by using corresponding slope and y as img.shape[0], int(0.65*img.shape[0]).
		* 			  With these end points solid line is super imposed on the original image.

The final output
    
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...