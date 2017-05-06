### README

**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction, binned color features extraction, and histograms of color feature extraction  on a labeled training set of images and train a classifier Linear SVM classifier
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream  and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/HOG/car1-hog.jpg
[image2]: ./output_images/HOG/car1-org.jpg
[image3]: ./output_images/HOG/non-car1-hog.jpg
[image4]: ./output_images/HOG/non-car1-org.jpg
[image5]: ./output_images/windows/Sliding-Window1.png
[image6]: ./output_images/windows/Sliding-Window2.png
[image7]: ./output_images/windows/Sliding-Window3.png
[image8]: ./output_images/windows/Sliding-Window4.png
[image9]: ./output_images/windows/Sliding-Window3.png
[image10]: ./output_images/Heatmaps/window-with-heatmap1.png
[image11]: ./output_images/Heatmaps/window-with-heatmap2.png
[image12]: ./output_images/Heatmaps/window-with-heatmap3.png
[image13]: ./output_images/Heatmaps/window-with-heatmap4.png
[image14]: ./output_images/Heatmaps/window-with-heatmap5.png
[image15]: ./output_images/Heatmaps/window-with-heatmap6.png


---
### Data

The data needed for this project can be found here at these links to the labeled data [vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/vehicles.zip) and [non-vehicle](https://s3.amazonaws.com/udacity-sdc/Vehicle_Tracking/non-vehicles.zip) examples that is needed to train the classifier. **Extract the contents of the zip files into a folder called "data" in the root of the project directory.**


### Histogram of Oriented Gradients (HOG)

### #1. HOG features extraction and choice of parameters.
First I loaded in the all of the image paths to both the "car" and "non-car" classes. Then in the project when we consider the HOG feature vector we first conver an image from a RGB colour space to a YUV colour space. Next we partition our images such that each block consists of a 2x2 cells, each cell consists of 16x16 pixels. Then we have 11 diferent orientations for the histograms. I choose theses parameters after I tried different parameters on random samples images from each of the two classes and displayed them to get a feel for what output look the 'best'. The code for this step is contained in the "Gradient feature selection (HOG)" and the "HOG Visualization" code cells of the IPython notebook.

Here is an example of the original YUV image and the HOG image produced by this stage of the pipline applied to each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]


### #2. Training the classifier.

I trained a linear SVM by first building a feature vector for each image in both of the "car" and "non-car" classes. Then I randomly shuffle the data into a training set and a testing set with a 80%-20% split.  The testing accuracy of the classifier was 99.46%. The classifier could be improved by training on a lager data set that includes motorcycles and semi-trucks as well as using a combination of classifiers, something like adaboosting the SVM's. The code for this step is contained in the "SVM" code cell of the IPython notebook.

### Sliding Window Search

For this I would crop roughly the bottom half of the image since there are no flying cars(yet). Then I would compute the HOG of this whole region and then sub-sample from it to avoid having to recompute. I could not decide on a single scale so did the search at 6 different scales, .85, 1.0, 1.25, 1.5, 1.75, 2.0. Each of I would crop the image indifferent sizes as to not make the pipline super slow. The code for this step is contained in the "Sliding Window Test", "Enhanced Sliding Window", and the "Sliding Window With Heat" code cells of the IPython notebook. Here is some example of the sliding window search at multiple scales and the merging of the found bounding boxes

![alt text][image5]
![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]
---

### Video Implementation

Here is a link to a  youtube video of my pipline in action

[![IMAGE ALT TEXT](http://img.youtube.com/vi/4G4XtjyrnoU/0.jpg)](https://youtu.be/4G4XtjyrnoU)

Here's a [link to my video file](./output_video_v15.mp4)



### Filter false positives and combining overlapping bounding boxes.

I would generate a heat map of detected positions on the image and would add heat to them. The idea being we can eliminate false positives by only considering positions that are hot enough, that is we thresholded the heatmap to identify vehicle positions. I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  We also let the heat decay over time to add a smoothing effect for future frames. Here are some examples of the heat map in action

![alt text][image10]
![alt text][image11]
![alt text][image12]
![alt text][image13]
![alt text][image14]
![alt text][image15]
---

### Discussion
There were not real issues faced in this project other then the slow time to process the large video file.

The pipline could be made more robust by keeping track of the number of cars on screen. Since currenly now when two car are close there bounding boxes merge togeather into one. If we could keep two seperate bounding boxes that would be a great improvement.  The pipline is likelly to fail in urban environment where there is a large density of cars on screen, was it would probably draw one large bounding box. The pipline also does not work if there are motorcycles or semi-trucks on the road.

