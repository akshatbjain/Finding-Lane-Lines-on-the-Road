# **Finding Lane Lines on the Road** 

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./pipeline_steps/0.Original_image.png "Original Image"
[image2]: ./pipeline_steps/1.Grayscale_image.png "Image converted to Grayscale"
[image3]: ./pipeline_steps/2.After_Gaussian_Blur.png "Image after applying Gaussian Blur"
[image4]: ./pipeline_steps/3.After_Canny_Edge_Detection.png "Image after applying Canny Edge Detection"
[image5]: ./pipeline_steps/4.Region_of_Interest.png "Defining Region of Interest"
[image6]: ./pipeline_steps/5.After_Hough_Transform.png "Image after applying Hough Transform"
[image7]: ./pipeline_steps/6.Superimposed_image.png "Superimposing detected lane lines on Original Image"

---

### Reflection

My pipeline consisted of 6 steps. 

First, I converted the images to grayscale, then I applied a gaussian blur to supress any kind of noise and/or any spurious gradients by averaging. I then proceed with Canny edge detection, a multi-stage edge detection algorithm, to detect all the edges in the image. In simple words, whenever there is a big change in the intensity of neighbouring pixels, canny considers that to be an edge. Now that I have the edges, I go on to define a region of interest (ROI), which is basically, as the name suggests, that region of the image that we are most interested in. Since we are trying to detect lanes, I define my ROI to be only that part of the image where our lane is present. But there is a possiblilty that some other part present in the ROI may have been detected as an edge by Canny. Thus we need to find only those edges that correspond to the lane lines. This is acheived by applying Hough Transform. Finally, I superimpose the joined lane lines on to my original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by defining a range of slope. Most lane lines appear to be between an angle range of 30 to 40 degrees (by trial and error, may change if the camera position is changed). For the right and the left lane lines, I defined the slope to be in the range (0.55, 0.85) and (-0.55, -0.85) respectively which correspond to the angle range mentioned earlier. I looped through all the detected lines to check if any of the lines satisfy this slope criteria and if they did, I added them to a list. After looping through all the lines, I found the most suitable points that would define the left and right lane lines by finding the max and min values through the stored list. Once I had the most suitable points, I went on to find the slope and intercepts of the left and right lane lines. This was required because although these points are the most suitable ones, we still need to extrapolate the line segments to map out the full extent of the lane. Now, from the ROI we know the y-range in which the lane lines should be. Thus, using the y-coordinate, slope and intercepts of both left and right lines, we can very easily compute the x-coordinate. This way I extrapolated the lane lines to their full extent. I also fill color in the lane that we are following using these computed points.

Additionally, I also added a memory feature. There might be times when the algorithm might fail due to some change in the environment. But the car should not lose it's sense of direction and should use the previous information to move forward until it can find more information and the algorithm starts working again. In the scenario, where the algorithm is not able to detect new lane lines, it will use the previous lane line positions to maintain direction.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]
![alt text][image7]


Potential Shortcomings:
1. One potential short coming may be steep turns. I don't really have enough data to test my algorithm there.
2. Also, since this is an ROI based algorithm, it would fail if the camera position is changed.


Possible Improvements:
Getting rid of the ROI and making the algorithm more dynamic to different lighting conditions.
