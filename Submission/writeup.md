# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/visualization.jpg "Visualization"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/udacity/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the python to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32 , 32 , 3
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data is distributed in the no of classes

![Histogram](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_2.png)

Followinhg you can see some random images printed with their respective labels in order to have an idea of the dataset we are dealing with.


![Random Images with Label](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_6.png)

From the dataset we can clearly see that the images given in the dataset have 
* Different Brightness
* Different Contrast 
* Different Angle of the traffic sign.
* Some images are also jittered 
* unusual Background Objects.
* and also sometiumes Multiple Signs in one image.

This makes the classification very different and a lot more optimization might be required in order to achieve the optimal results


### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because it would enable my model to differentiate between pixels fast and without much power required

Here is an example of a traffic sign image before and after grayscaling.

![Conversion into grayscale](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_3.png)

As a last step, I normalized the image data because of the two reasons. It was suggested in the lessons and secondly this site (http://stats.stackexchange.com/questions/185853/why-do-we-need-to-normalize-the-images-before-weput-them-into-cnn) has an explanation. it says that it would be very difficult for the cnns to train using a single learning rate if the data has a much wider distribution. This made sense as well and in the end helped me improving the accuracy as well.

I decided to generate additional data because data augmentation is said to be the best technique used in order to improve the accuracy. The reason for it is that in the data set, if the number of images in particular class is way less then the model training will be more biased to the classes which have more data. This made perfect sense to me. 

To add more data to the the data set, I used the techniques in such a way that the data is more and is different but still recognizable. This is the reason i did some small random translations and rotations in order to copy the images and having more data.  

Here is an example of an original image and an augmented image:

![Random Rotation](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_4.png)

![Random Translation](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_5.png)

The difference between the original data set and the augmented data set is the following ... 
![Original Dataset](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/Image_2.png)

![Augmented Dataset](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Writeup/image_1.png)


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Preprocessing    		| 32x32x1 Grayscale Normalized image 			| 
| Convolution 5x5     	| 1x1 stride, same padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6  				|
| Convolution 5x5     	| 1x1 stride, same padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16   				|
| Flattening            | Output 400                                    |
| Fully connected		| Input 400 ; Output : 120						|
| RELU					|												|
| Dropout       		|                       						|
| Fully connected		| Input 120 ; Output : 84						|
| RELU					|												|
| Fully connected		| Input 84 ; Output : 43						|
| RELU					|												|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I trained my model by checking on different optimizer. I started off with GradientdescentOptimizer but there the error loss rate was not so good. I changed it to AdamOptimizer (as used in the lessons). This improved the accuracy very well and the loss rate improved. I also varied the different parameters in order to get a better accuracy. The parameters used for batch size, number of epochs , learning rate, keep_probability.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* Training Set Accuracy of 99.9 %
* validation set accuracy of 99 % 
* test set accuracy of 86.4 %

and the Hyperparameters used were as following

* Batch Size        : 100
* Number of epochs  : 60
* Learning Rate     : 0.0009
* Keep Probability  : 0.55


If an iterative approach was chosen:
* Initially GradientDescentOptimzer was used and wihouth the dropout layer.
* What were some problems with the initial architecture?
    * That architecture required a large number of epochs and very high processing power. It was not really optimal to use that as a model when there are other possibilties.
* How was the architecture adjusted and why was it adjusted? Typical adjustments could include choosing a different model architecture, adding or taking away layers (pooling, dropout, convolution, etc), using an activation function or changing the activation function. One common justification for adjusting an architecture would be due to overfitting or underfitting. A high accuracy on the training set but low accuracy on the validation set indicates over fitting; a low accuracy on both sets indicates under fitting.
    * The model was adjusted by augmenting the data. As the accuracy on the validation dataset was not increasing at a significant rate. It made sense to augment some data in order to have the equal distribution of the data over the classes. This would train the model much better and in return results in much better accuracy.
* Which parameters were tuned? How were they adjusted and why?
    * The parameters that were tuned were as following: Batch size, Number of Epochs, Learning rate and Keep_probability.
* What are some of the important design choices and why were they chosen? For example, why might a convolution layer work well with this problem? How might a dropout layer help with creating a successful model?
    * lenet architecture is tested and well accpeted architecture. It made sense to use this architevture in order to achiev the required results. Dropout layer helped in reducing the processing time and power.
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on the web:

![Sign 1](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_1.png) ![Sign 2](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_2.png) ![Sign 3](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_3.png) ![Sign 4](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_4.png) ![Sign 5](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_5.png) ![Sign 6](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_6.png) ![Sign 7](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_7.png) ![Sign 8](https://github.com/gaurav2205/Traffic-Sign-Classifier/blob/master/Internet%20traffic%20signs/Image_8.png)

Some of the images were not able to be classified correctly were because the images taken from internet are too perfect and the model was trained on images with much lesser brightness and contrast. Hence the model could not identify the images on internetwith 100 % accuracy which i believe is acceptable because in the practical scenario the images are taken by a camera installed in the car which might not give as perfect image as obtained from internet

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 50 speed limit     	| Turn Left ahead								| 
| Stop Sign      		| Priority road									| 
| Priority road     	| Priority road									|
| 30 km / h				| STOP    										|
| General Caution 		| Round about ahead                                 |
| Priority next junction| Priority next junction           							|
| turn right            | turn right           							|
| No Entry              | Turn Left Ahead          							|


The model was able to correctly guess 4 of the 8 traffic signs, which gives an accuracy of 50%. This compares favorably to the accuracy on the test set of 

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

See the HTML file

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


