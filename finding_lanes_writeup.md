# **Finding Lane Lines on the Road** 


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./test_images_roi_output/solidWhiteCurve.jpg "roi1" 
[image3]: ./test_images_roi_output/solidWhiteRight.jpg "roi2"
[image4]: ./test_images_roi_output/solidYellowCurve.jpg "roi3"
[image5]: ./test_images_roi_output/solidYellowCurve2.jpg "roi4"
[image6]: ./test_images_roi_output/solidYellowLeft.jpg "roi5"
[image7]: ./test_images_output/solidWhiteCurve.jpg "lm1"
[image8]: ./test_images_output/solidWhiteRight.jpg "lm2"
[image9]: ./test_images_output/solidYellowCurve.jpg "lm3"
[image10]: ./test_images_output/solidYellowCurve2.jpg "lm4"
[image11]: ./test_images_output/solidYellowLeft.jpg "lm5"
[image12]: ./test_images_output/whiteCarLaneSwitch.jpg "lm5"


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline used following image processing steps to identify the lanes on the images:
* Gaussian Blurring
* Canny Edge Detection
* Hough line transformation
* Linear Interpolation
* Filtering slope based on collected image data

**Gaussian Blurring**
    Canny edge relies on intensity gradient of the image to detect edges and it is prone to detect false edges in a 
    noisy image. To reduce the noise Gaussian smoothing is performed on the gray scale image with kernel size of 3.
    
**Canny Edge detection**
    The basic principle of finding an edge is to compute intensity gradients and identify edges satisfying the input
    based input higher and lower threshold parameters. Pixels with intensities greater than higher threshold are picked
    and also with intensities in between the given threshold provided it is connected to the higher threshold
    pixel. In order to detect lane marking which are either yellow or white color in given images higher gray scale
    values **60 : 180** are preferred. The detected image is processed further to extract the desired lanes using a hard
    coded polygon
    
```
    edgeimg = canny(blurredimg, 60, 180)
 ```

 ![image2] | ![image6]
  --------- | ----------
![image2] | ![image6] 

**Hough Transformation**
    Transforms the image into (rho, theta) parameter space and accumulates the count for number of lines passing thorough
    each pixel and returns the lines that exceeds the input min_number_of_votes. Also to find the smaller lines on
    the road these number and min_line_length has to be small in this project. These 2 parameters are modified to detect as much
    line sgements of lanes for the given input images
    
```
    lines = hough_lines(finalimg, 1, np.pi/180, 35, 5, 2, img_name)
```
 
 ![image7] | ![image8]
 --------- | ------------
 ![image9] | ![image10] 
![image11] | ![image12]

**Linear Interpolation**
    In order to detect the left and right lanes slopes for each of these lanes hough line segments are calucated
    individually. The computed slopes for right lane are positive and for left lanes are negative as the height for
    them increase and decrease respectively. By computing the average slopes(m) and intercept(c) for these lanes the
    hough line segments for the current frame is interpolated using the equation
    
```
    y = mx + c
```    

   While playing back the yellow left and challenge video on few frames which had horizonal lane lines the drawn lines
   went far off from the actual lanes. To correct these lines slope points for those frames were collected and using
   them as inputs lower and upper slope threshold values **( 0.4 > m < 1.0  or -0.4 > m < -1.0 )** those points were
   removed from the average slope computation.
    
**Refer solidYellowLeft.mp4, solidWhiteRight.mp4**

### 2. Identify potential shortcomings with your current pipeline

* With the existing implementation drawing of lane lines between certain frames isn't a smooth transition especially in **challenge video**

* The curvier the road the harder is to tune the hough transform and to user linear interpolation to draw lanes
on them
* This solution depends heavily on the slope values collected from the input images and videos to reject
 some hough tranformed lines with steeper slopes or outliers and may not be a generalized solution 
* Lane section of the image could be dynamically rather than using a hard coded polygon

### 3. Suggest possible improvements to your pipeline

* Consider last n frames to draw stable lane lines between frames
* Collect more data based on the real world scenario to come up with generalize solution
* Learn few more image processing techniques to handle road curvature better.
