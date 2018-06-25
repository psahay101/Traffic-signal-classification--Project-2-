# Traffic Sign Recognition

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

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...


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
2. Batch Size: 18
3. Epochs =20
4. Hyperparameters: mean=0, standard deviation = 0.5.
5. Dropout: training=0.5, evaluation=1.

Using all these parameters the model achieved the following results:
1. Maximum validation accuracy : 99.6% at 18th epoch
2. Final accuracy on the test set is 94.4%


### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

I used trial an error on a similar model as the one from the LeNet lab to tune the hyperparameters. On the first trail the results were pretty impressive with the training accuracy of 98.8 %. Anyway, I added three things after this which were a relu layer, a dropout at the two fully connected layers and incresed the number of epochs from 10 to 20. This brought the accuracy down to 97%.
So a final change of changing the dropout to 1 was made which got the accuracy to 99.6 %. This seemed like a good accuracy to stop tuning the parameters.


## Test a Model on New Images

### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are eight German traffic signs that I found on the web:


# Image Here

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
