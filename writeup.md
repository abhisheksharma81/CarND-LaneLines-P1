# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the Process_Image function.

My pipeline consisted of 6 steps. 

1. Grayscaling
2. Gaussian smoothing
3. Canny Edge detection
4. Masking
5. Hough Lines
6. Drawing the lane lines

The openCV calls are used for the grayscaling, gaussian smoothing, Canny edge detection and Hough Lines.

In order to draw a single line on the left and right lanes, I modified the Process_image function for the pipeline creation.

To draw the lines, first I seperated the left and right lines by their respective slopes. If a line has positive slope the line segment
belongs to a right line and if a line segment has negative slope it is part of the left lane.

After storing the left and right line coordiantes in the seperate lists, I applied "np.polyfit" to get the slope and intersect for each line.

Now I needed the coorinates to draw the lines which intesect with the X axis.

Right lane:

I extracted the Xmax value from the list of right line segments and calculate the Ymax using the 
fit_line = np.polyfit(xr_points, yr_points, 1)
max_y = fit_line[0]*max_x + fit_line[1]

so one set of coordinates were 
point_1 = (max_x.astype(int), max_y.astype(int))

For the second set of coordinates I took a calibrated value of y=320 on the line and calulated the corresponding
x coordinate using

xpoint = (ypoint - fit_line[1])/fit_line[0]

Now that I had two points for right lane I used the below function for drawing the right line.

cv2.line(image, point_1, point_2, color=(255,0,0),thickness=5)


Left Line:

Similar to right line , I calculate the slope and intersect of the left line using np.polyfit function

In this case I chose Xmin as one coordinate and calulated the Ymin using the function mentioned below

min_yl = fit_linel[0]*min_xl + fit_linel[1]

So the one set of coordinates for left line were 

point_3 = (min_xl.astype(int), min_yl.astype(int))

Again for the second coordinate I chose the ylpoint=320 aand calculated the X value as mentioned below

xlpoint = (ylpoint - fit_linel[1])/fit_linel[0]


It gave me another coordinate for left line as

point_4 = (xl.point, ylpoint)

Once I had both the coordinates I  drew left line using 

cv2.line(image, point_3, point_4, color=(255,0,0),thickness=5)
I had to put some checks and condition to avoid drawing some lines which intersect on Y axis and were wrongly detected by hough lines.

I could see that my pipeline could detect and draw the lines on the both the solidewhiteright and solidyellowleft videos.

If you'd like to include images to show how the pipeline works, here is how to include an image: 


### 2. Identify potential shortcomings with your current pipeline

There are some shortcomings like the pipeline may not work for the curved lanes. I believe
there is need to put more boundary conditions. Also the min and max coordinates can be the point of
intesection from the x axis.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to avoid flickering and provide smoothing effect on the lines drawn.


