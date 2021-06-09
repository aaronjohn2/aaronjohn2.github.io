---
layout: post
title:  Flower Recognition - Implementing a Caffe Model on Swift
date:   2018-06-18
category: computer_science
---

In this project I made a Flower Recognition app by implementing a pre-trained Convolutional Neural Network to classify with relative accuracy as to what flower is in the image that one has taken.

To start off, I converted a pre-trained Caffe Model into MLModel format which was later used in my Xcode project.
The Caffe Model that I used had already been trained on thousands upon thousands of images. This pre-trained model was converted to a .MLModel so that it can be seen and used by my Swift files in order to build this flower recognition app.

Apple has released open source tools in python that allow you to convert any pre-trained model that had been trained using Caffe, Keras, scikit learn, etc.

Brief Description of app:

* This app allows you to take a picture of any flower and it will try to recognize what that flower is called.

* The data set that our model has been trained on is called "Oxford 102 Flower Dataset."
A bunch of researchers from Oxford did a very time consuming task by labeling a whole bunch of images of flowers with the correct classification of the flower.

* The reason why it is called Oxford 102, is because they have 102 categories for the flowers.
In their dataset they've got anywhere between 40 to 200 images for each and every category.
These images could vary and have been taken at different angles and at different stages of life cycle of the flower.

* Therefore if you train a machine learning model on it, it should be able to classify with relative accuracy as to what flower is in the image that you have taken. [Jimmie Goode](https://github.com/jimgoo/caffe-oxford102), used this dataset to train a Convolutional Neural Network(CNN). He used Caffe to classify the images in the Oxford 102 category flower dataset. 
This Caffe model is what I used in my Swift project to build the Flower Recognition app.

You can get my [Flower Recognition iOS app work](https://github.com/aaronjohn2/FlowerRecog-master) on GitHub. This project works without Internet connection, since the MLModel is pre-loaded onto the device and an API is not required. It's pretty cool to see Caffe finally working on my iPhone to solve a real world problem. 
