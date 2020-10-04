# Custom-Vision-Crack-Detector

## Table of contents
* [Overview](#overview)
* [Challenge](#challenge)
* [Learning Objectives](#Learning-Objectives)
* [Pre-Requisites](#pre-requisites)

# Overview	
This lab is intended to serve as an introduction to creating and consuming Azure Cognitive Services.
The lab will walk you through accessing the Custom Vision frontend and how to create and consume a simple classification model.

# Challenge 
Welcome to our Predictive Maintenance team! You have been challenged with creating a solution that is capable of receiving concrete images from a building/site/platform, analysing them and sending an alert email if there is a crack in the a building/site/platform

# Learning Objectives

Upon completing this lab, you will have hands-on experience with the following functions and concepts related to Azure Cognitive Services:

* Logging in to Azure Portal and creating cognitive services 
* Creating, modifying, and saving a project with Custom Vision.
  * Creating a new project
  * Uploading and tagging images
  *	Training and testing an image classification model
  * Publish an endpoint for the image classification model
* Creating an integration workflow in Logic Apps

# Pre-Requisites
* Azure Subscription

# Step-by-Step Guide

## Step 1 - Create Custom Vision Service

* Login to [Azure Portal](https://portal.azure.com)
* Select **+**
* In search box, type **Custom Vision**
*


## Step 2 - Create a custom model project with Custom Vision

* Sign into [Custom Vision](https://customvision.ai)
* Select **+ new project** 
* Your new project should have the following settings:
  * Name: Crack detector
  * Description: crack detector for predictive maintenance
  * Resource: select the custom vision service created in [step 1](##  Step 1 - Create Custom Vision Service)
  * Project Type: classification
  * Classification Types: Multiclass
  * Domain: General


## Upload Training Images

We will now add images for each tag from the corresponding folder in "Crack Detector Training Images". 

* Click "Add Images" in the top row
* Browse to Crack Detector Training Images / Positive
* Select all files: click one file, then press Control-A
* Click Open
* In the "Image Upload" dialog that appears next, type "positive"and then click "Upload XX Files"
* repeat this process for the Negative tag
* Select train a model

