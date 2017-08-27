## Writeup
### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/example_image.png
[image2]: ./output_images/hog_images.jpg
[image4]: ./output_images/Test_Sample_Result.jpg
[image5]: ./output_images/HeatMap_Result.jpg
[video1]: ./output_images/project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. This Writeup / README that includes all the rubric points and how I addressed each one. [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a original template writeup for this project and this is a starting point.  


## **Overview**

For detecting vehicles, I implement these functions below.
- Feature extraction
- Training the model
- Sliding windows seaerch

## Feature Extraction

#### 1. Histogram of Oriented Gradients (HOG)
This section explain how (and identify where in my code) I extracted HOG features from the training images.

The code for this step is contained in the code cell `HOG parameters` of the IPython notebook called `P5.ipynb`.  

In this project, data set for traing is GTI and KITTI.
I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

Based on the images, I visualized some color spaces at `Anaysis for Color Space for detecting car using 3D Plot` in IPython Notebook.

Then, I explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=9`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image2]


### 2. Definition HOG parameters
I used HOG as feature vectore for detecting vehicles.
I tried various combinations of parameters and decide the parameter below.
This is the beset result for my project.

|HOG parameter   |value |
|:---            |:---  |
|color space     |YCrCv |
|orientations    |9     |
|pixels_per_cell |8     |
|cells_per_block |2     |



### 3. Training the model
This describe how I trained a classifier using my HOG features and color features.

I trained linear SVM as classifier with three features with color space YCrCv.

- HOG features vectors
- Color binns
- Color histgram

The accuracy for test data set is 0.9876.
This is my SVM parameter.

|SVM parameter   |value |
|:---            |:---  |
|Kernel          |Linear|
|C               |0.1   |

This is final feature vector parameter.

|Features        |value |
|:---            |:---  |
|color space     |YCrCv |
|HOG             |orient = 8, pix_per_cell = 8, cell_per_block = 2|
|Spatial bin     |Size   = (32,32)     |
|Color histgram  |nbins  = 32     |


## Sliding Window Search

#### 1. HOG Sub-sampling window search
I used a HOG-subsampling sliding window search following Udacity's lecture.
This code is included in `Using HOG Sub-sampling Windows Search section` IPython notebook.

I set region of interest is about a half of image. Overlap window is 75[%].


#### 2. Result for test images
I show some examples of test images to demonstrate how my pipeline is working.

Ultimately, I searched on images with YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here are some example images:

If my pipeline get a lot of false-positive, It's OK to remove the `false-positive` as noise. See below `The result of Video Implementation`.

![alt text][image4]
---

### The result of Video Implementation

#### 1. Video result
I provide a link to my final video output.  My pipeline recognize vehicles reasonably well on the entire project video.
Here's a [link to my video result](./output_images/project_video_out.mp4)


#### 2. Filter for removing False-positive
I describe how I implemented a filter for false positives and a method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.

I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  After that, I assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

#### Here are the result of frame and their corresponding heatmaps:

![alt text][image5]

---

## Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

In this projrct, I use classic method using HOG feature and SVM approach. Near future, I try to implement DNN or RNN approach for detecting vehicles.