#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./solidYellowLeft.jpg "Color image"
[image2]: ./grayscale.png "Grayscale image"
[image3]: ./gaussian.png "Grayscale image after Gaussian blur"
[image4]: ./canny.png "Canny edge detection"
[image5]: ./roi.png "Region of interest mask"
[image6]: ./hough.png "Hough lines"
[image7]: ./color_hough.png "Color image with Hough lines"
[image8]: ./line_extend.png "Extrapolated lines"



---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

Let's take a sample image. 

![Color image][image1]

My pipeline consisted of the following steps:

1. Conversion of the image to grayscale 

  ![Grayscale image][image2]
2. Gaussian blurring 

  ![Grayscale image after Gaussian blur][image3]
3. Canny edge detection 

  ![Canny edge detection][image4]
4. The application of a triangular region-of-interest mask 

  ![Region of interest mask][image5]
6. The computation of Hough lines

  ![Hough lines][image6]
5. The annotation of the color image with Hough lines

  ![Color image with Hough lines][image7]

In addition, while refining the model, I calculated the slopes of individual lines and grouped them into 2 clusters of slopes using k-means clustering. In the existing images, one slope was positive and one negative, so the clusters were quite intuitive. From these clusters, I was able to extrapolate the lines to form continuous lanes when the lane markers were formed by a broken line.

![Extrapolated lines][image8]

###2. Identify potential shortcomings with your current pipeline

There are several shortcomings with this implementation. First, the extrapolation is done in a linear way, but the road can be bending and then linear extrapolation will be inappropriate. One may instead try quadratic or cubic extrapolation, and interpolation between the points defining the line segments. Another issue has to do with the Canny edge detection being based on gradients. It may be the case that hard-coding specific gradient strengths will do poorly if there is a cloudy day, or a shadow is being cast on the road by an object such as a tree. In general, heuristic-based approaches that rely on hard-coded values (e.g. Hough transform parameters, Canny edge detection gradient thresnholds, the size of the Gaussian filter, the size of the region of interest masks) tend to be brittle. 


###3. Suggest possible improvements to your pipeline

A possible improvement would be to use non-linear extrapolation, and doing contrast adjustment to address weaker gradients on cloudy days or with shadows covering parts of the road.

Another improvement would be to learn parameters that define the image processing steps from data. For example, Canny edge detection and Gaussian blurring are heuristics that ultimately define a convolution over the original image. Machine learning models such as convolutional neural networks also have parameters (number of filter, size of filter, etc.), but they learn the weights that ultimately decide how to best filter images seen in the training set, which can go into millions of images. The problem with CNNs is that they are a supervised learning algorithm, and hence need a lot of labeled training data. 
