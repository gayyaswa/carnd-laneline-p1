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

The pipeline used following image processing steps to identify the lanes on the images:
1. Canny Edge detection algorithm is used to identify the edges on the input message. The basic principle in finding an
edge is to compute intensity gradients of the image after applying guassian filter to reduce the image noise.

    * Converted the input image to single channel grayscale and applied guassian filter of kernel size 5 to blur and
     reduce the image noise. Applying kernel size of 3 appear to work as well for this project.
    * Based on the defined upper and lower threshold algorithm filter out pixel which has lesser intensity than
     lower_threshold. The ratio between these values are kept 1:3 (50:150) as suggested and observed the lanes were
     identified properly
    * Defined a hard coded polygon to extract the lane section from the image. Restricting the image properly did help
    tuning the hough line transformation easier. In one of the image lines from vehicle was getting returned after
    transformation as the vehicle wasn't removed by the defined polygon
    [image1]: ./test_images_roi_output/solidWhiteCurve.jpg | [image1]: ./test_images_roi_output/solidWhiteRight.jpg
    ----------------------------------------------------- | -------------------------------------------------------
    [image1]: ./test_images_roi_output/solidWhiteCurve.jpg | [image1]: ./test_images_roi_output/solidWhiteRight.jpg

    * The parameters


In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
