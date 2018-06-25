# Traffic Sign Recognition

[//]: # (Image References)

[image1]: ./1.png "no entry"
[image2]: ./2.png "fifty"
[image3]: ./3.png "stop"
[image4]: ./4.png "thirty"
[image5]: ./5.png "left turn ahead"
[image6]: ./dataset1.png "data analysis"
[image7]: ./lowlight.png "low light image "
[image8]: ./tilted.png "image with camera tilt"
[image9]: ./multiple.png "image with multiple signs"


### Build a Traffic Sign Recognition Project

The goals / steps of this project are the following:

* Load the data set (see below for links to the project data set).
* Explore, summarize and visualize the data set
Design, train and test a model architecture.
* Use the model to make predictions on new images.
* Analyze the softmax probabilities of the new images.
* Summarize the results with a written report.

## Rubric Points

## Here I will consider the rubric points individually and describe how I addressed each point in my implementation.

## Writeup / README

### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my project code

## Data Set Summary & Exploration

### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic signs data set:

* The size of training set is : 34799
* The size of test set is : 12630
* The shape of a traffic sign image is: (32, 32, 3)
* The number of unique classes/labels in the data set is : 43

### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how much train and test data is available for all the different classes. Here an important thing to note is the disparity of the amount of data for different classes which means that for some common classes we will be having lot more images than for some rare class for which a lot of data weren't available. Now this might skew the end results in favor of the more common images which means that the network's ability of spot any image with that common class is much better than for the less common ones. 

![][image6]

Apart from that I have included images of some random images of signs from multiple classes and this is to show how different conditions are incorporated to make the training robnust. By different conditions, I means various things like low lighting, tilt in the image, background objects in the image etc. These conditions are important as they are present in the real time conditions thus making the network adaptable for the real life conditions.

Some of the images of low lighting, camera tilt and multiple objects below in the respective order:

![][image7]

![][image8]

![][image9]

## Design and Test a Model Architecture

### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)


 I did three simple things here:
 1. Generated a lot more data (70000 to be specific) as jittered data  which will allow us to gather sufficient data set for even testing and validation (which are in turn 22 and 20 percent of the training set respectively.)

 2. I normalized the data by scaling them in between 0-1 scale from 0-255 which has shown to improve the accuracy of the training in many papers.

 3. Converted image to grayscale as only one dimension or channel will be utilized saving the space.

 Now to generate extra data I used the resample function from sklearn.utils. Also I have put an image of the details of this new augmented data set below...

### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

The first phrase includes convolution, ReLu activation and maximum pooling:

First Conv layer: Input dimension(32x32x3) -> output dimension(28x28x8) using 8 filters of size(5x5) with Valid padding and 1 stride. Relu applied after that which puts all negative values to 0.

Then max pool layer of valid padding and strides=2 will return an output size of 14x14x8.

Second Conv layer is same as the first one with only change of filters which here is 18. Output size is 10x10x18.

One more max pool layer which returns output size of 5x5x18.

Relu applied again.

The second phrase :ReLu and Drop-out

The output layer coming from previous layer (5x5x18) is flattened out as a 1D array of size 450.

Now linear Regression is applied with 450x150 weights and 150 biases.

Relu is applied.

Dropout of 0.5 is applied.

Second layer is used which converts number of values from 150 to 86 by using 150x86 weights and 1x86 biases.

Relu and 0.5 dropout is applied again.

Finally, the output layer returns 43 classes by using weights = 86x43 and baises = 43.


### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

The following parameters describes how I trained my model:
1. Optimizer : Adam optimizer.
2. Batch Size: 130
3. Epochs =25
4. Hyperparameters: mean=0, standard deviation = 0.5.
5. Dropout: training=0.5, evaluation=1.

Now for choosing the hyperparameters the job was very interesting and has a lot of insights. Here an important aspect of the model was to keep the mean 0 and and equal variance thus these hyperparameters were chosen at first. Then I tried to play with the standart deviation which for smaller values did not work well and the larger values took a lot of time to train so I optimized and settelled at a value of 0.5.

Using all these parameters the model achieved the following results:
1. Maximum validation accuracy : 99.6% at 18th epoch
2. Final accuracy on the test set is 94.4%


### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

I used trial an error on a similar model as the one from the LeNet lab to tune the hyperparameters. On the first trail the results were pretty impressive with the training accuracy of 98.8 %. Anyway, I added three things after this which were a relu layer, a dropout at the two fully connected layers and incresed the number of epochs from 10 to 20. This brought the accuracy down to 97%.
So a final change of changing the dropout to 1 was made which got the accuracy to 99.6 %. This seemed like a good accuracy to stop tuning the parameters.


## Test a Model on New Images

### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on the web:


![][image1]
![][image2]
![][image3]
![][image4]
![][image5]


1. First sign is a no entry sign which I think would be tricky to classify as there is a tilt in the image which might cause problems.

2. Second one is a speed limit of 50 but again the 50 is not readable and has a curved textual font which is very deceptive.

3. The third one is a stop sign which is comparatively easier but still some tilt in it can cause problems.

4. The fourth is a speed limit sign of 30 which is very different from the usual 30 signs as here the fond is a different and the image has become overtly broad due to compression done for fitting in the required size.

5. The last one is a left turn ahead which also is difficult as there is a white background which is not the case for the photos on which the classifier has been trained on as they usually have a background in it.


## 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

Images: No entry, 50 speed limit, stop, 30 speed limit, left turn ahead

predictions: No entry, yield, stop, 30 speed limit, left turn ahead

So as 4 out of 5 images were correctly classified, accuracy was 80%.

## 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

For the first image, the model is relatively sure that this is a no entry sign (probability of 100%). The top five soft max probabilities were:

No entry : 100.0%
Bumpy road : 2.112447577973242e-08%
Stop : 3.6358679955661444e-09%
Bicycles crossing : 2.7259972403025686e-13%
Children crossing : 2.1470417769945614e-14%


Image 2 has been wrongly classified as 60 km/hr. Its top 5 softmax probabilities are:

Speed limit (60km/h) : 70.11036276817322%
Speed limit (50km/h) : 28.676587343215942%
No vehicles : 0.8481469005346298%
Yield : 0.16287651378661394%
Stop : 0.11768729891628027%


Image 3 has been rightly classified as stop. Its top 5 softmax probabilities are:

Stop : 99.99973773956299%
No entry : 0.000243884232986602%
Yield : 2.0220899443756934e-05%
Go straight or right : 2.3429723938761526e-06%
Traffic signals : 1.4621592825392327e-06%

Image 4 has been rightly classified as 30 km/hr. Its top 5 softmax probabilities are:

Speed limit (30km/h) : 99.99996423721313%
Speed limit (50km/h) : 3.458345645412919e-05%
Speed limit (20km/h) : 6.209240899224255e-08%
Speed limit (60km/h) : 2.1442728148635126e-08%
Speed limit (80km/h) : 6.720077272426295e-09%

Image 5 has been rightly classified as left turn ahead. Its top 5 softmax probabilities are:

Turn left ahead : 85.64892411231995%
Ahead only : 14.267198741436005%
Roundabout mandatory : 0.0638321740552783%
Children crossing : 0.014127448957879096%
Beware of ice/snow : 0.003844018283416517%
