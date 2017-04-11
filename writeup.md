#**Behavioral Cloning** 

##Writeup Template

###You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/CNN.jpg "Model Visualization"
[image2]: ./examples/Center_Lane.jpg "Center Lane"
[image3]: ./examples/r1.jpg "Recovery Image"
[image4]: ./examples/r4.jpg "Recovery Image"
[image5]: ./examples/r7.jpg "Recovery Image"
[image6]: ./examples/r10.jpg "Recovery Image"
[image7]: ./examples/r13.jpg "Recovery Image"
[image8]: ./examples/r18.jpg "Recovery Image"

[image10]: ./examples/original.jpg "Normal Image"
[image11]: ./examples/flipped.jpg "Flipped Image"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
###Files Submitted & Code Quality

####1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup_report.md or writeup_report.pdf summarizing the results

####2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```

####3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

###Model Architecture and Training Strategy

####1. An appropriate model architecture has been employed

My model consists of a convolution neural network with 3x3 filter. First Convolution layer gives 3 outputs and the second Convolution layer gives 5 outputs.

The model includes RELU layers to introduce nonlinearity, and the data is normalized in the model using a Keras lambda layer. 

####2. Attempts to reduce overfitting in the model

The model contains dropout layers with drop out probability as 0.5, in order to reduce overfitting. 

The model was trained (75% of data) and validated (25% of data) on different data sets to ensure that the model was not overfitting. The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

####3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually.

####4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road. I collected data in three sessions.

For details about how I created the training data, see the next section. 

###Model Architecture and Training Strategy

####1. Solution Design Approach

The overall strategy for deriving a model architecture was to extract the features from the image as well as keeping the architecture as simmple as possible.

My first thought was to use a simple two layer convolution neural network model. I thought this model might be appropriate because I had control on data collection and data augmentation and addition was not that of an issue. 

Each image was cropped so that each image contained only the road features.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. To counter this, I added two drop out operations after each of the layers. I also collected more data and augmented the collected data by flipping the images and the steering angles for the images which had non-zero steering angles.

Then I ran the CNN for 10 epochs but the training loss seemed not to be reducing after 5 epochs. I then used 5 epochs for all my model building and iterating exercises. For my penultimate model I saw that the loss stabilised at 3 epochs and thus after making a few changes, I ran the model for 3 epochs only.

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track like the turns. To improve the driving behavior in these cases, I collected data for recovering from wrong positions of the car.

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

####2. Final Model Architecture

The final model architecture consisted of a convolution neural network with the following layers and layer sizes:

Here is a visualization of the architecture

![alt text][image1]

####3. Creation of the Training Set & Training Process

To capture good driving behavior, I first recorded two laps on track one using center lane driving. Here is an example image of center lane driving:

![alt text][image2]

I then recorded the vehicle recovering from the left side and right sides of the road back to center so that the vehicle would learn to recover towards the center lane if it goes towards any side of the road. These images show what a recovery looks like starting from left side :

![alt text][image3] ![alt text][image4]![alt text][image5] ![alt text][image6]![alt text][image7]
![alt text][image8]


To augment the data sat, I also flipped images and angles thinking that this would counter overfitting. For example, here is an image that has then been flipped:

![alt text][image10]
![alt text][image11]


After the collection process, I had 18306 number of data points which increased to 32597 after flipping operation. I then preprocessed this data by normalizing and cropping.

I finally randomly shuffled the data set and put 25% of the data into a validation set. 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was 3 as evidenced by the performance of the model on the track in autonomous mode. I used an adam optimizer so that manually training the learning rate wasn't necessary.
