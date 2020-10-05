# Custom-Vision-Crack-Detector

## Table of contents
* [Overview](#overview)
* [Challenge](#challenge)
* [Learning Objectives](#learning-objectives)
* [Pre-Requisites](#pre-requisites)
* [Step-by-Step Guide](#step-by-step-guide)

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

* Log in to [Azure Portal](https://portal.azure.com)
* Select **+ Create a resource**
* In search box, type **Custom Vision** and click Enter
* Select **Create**
* Your new service should have the following settings:
  * Create Options: Both
  * Project Details:
    * Subscription: select your preferred subscription for this exercise
    * Resource group: select your preferred resource group for this exercise
    * Name: custvision<your initials>(please note this name must be unique, try different options if the one you selected is already in use. You can create multiple projects within the same service, we will learn about it in the next steps)
  * Training Resource
    * Training Location: select your preferred region
    * Training pricing tier: if available select **Free**, otherwise **Standard**
  * Prediction Resource
    * Prediction location: select your preferred region, ideally same as selected for Training Resource
    * Prediction pricing tier: if available select **Free**, otherwise **Standard**
 * Select **Review + create**
 * Select **Create**
 


## Step 2 - Create a custom model project with Custom Vision

* Sign into [Custom Vision](https://customvision.ai)
* Select **+ new project** 
* Your new project should have the following settings:
  * Name: Crack detector
  * Description: crack detector for predictive maintenance
  * Resource: select the custom vision service created in [step 1](##step-1---create-custom-vision-service)
  * Project Type: classification
  * Classification Types: Multiclass
  * Domain: General
 * Select **Create Project**

 ![Step 2 Screenshot](/../main/Snapshots/Step%202%20-%20Create%20a%20Custom%20Vision%20Project.PNG?raw=true "Step 2")

## Step 3 - Upload Training Images

We will now add images for each tag from the corresponding folder in "Crack Detector Training Images". 

* Click **+Add Images** in the top row
* Browse to the folder **Concrete Cracks Dataset\Train\Positive**
* Select all files: click one file, then press Control-A
* Click **Open**
* In the **Image Upload** dialog that appears next, type **positive** under **My Tags** and then click **Upload 50 Files**
* Click **Done**
* Repeat this process for the Negative tag **Concrete Cracks Dataset\Train\Negative**

 ![Step 3 Screenshot](/../main/Snapshots/Step%203%20-%20Upload%20Training%20Images.PNG?raw=true "Step 3")
 
## Step 4 - Train Classification Model

* Select **Train** in top row, towards the right
* Choose **Quick Training**
* Click **Train** (please note the training should take a few minutes)

 ![Step 4 Screenshot](/../main/Snapshots/Step%204%20-%20Train%20Classification%20Model.PNG?raw=true "Step 4")

## Step 5 - Test Classification Model

* Spend a few minutes getting familiar with the performance tab and different performance metrics
* Select **Quick Test** on the top row
* Select **Browse local files**
* Navigate to the folder **Concrete Cracks Dataset\Test\Positive** and select one of the images
* Click **Open**
* Confirm the prediction is correct

 ![Step 5 Screenshot](/../main/Snapshots/Snapshots/Step%205%20-%20Test%20Classification%20Model.PNG?raw=true "Step 5")

## Step 6 - Publish Classification Model

* Select **Publish** in second top row
* Type **Crack-Detector-Model** as the **Model Name**
* Select the **prediction resource** created on Step 1
* Click **Publish**

![Step 6 Screenshot](/../main/Snapshots/Snapshots/Step%206%20-%20Publish%20Classification%20Model.PNG?raw=true "Step 6")

**Now that we have trained and published a custom machine learning model to detect concrete cracks, we will learn how to create an integration workflow that is triggered every time a new image is uploaded to azure storage.**

## Step 7 - Create Storage Account

* Login to [Azure Portal](https://portal.azure.com)
* Select **+ Create a resource**
* In search box, type **Storage account** and click Enter
* Select **Create**
* Your new service should have the following settings:
 * Project Details:
    * Subscription: select your preferred subscription for this exercise
    * Resource group: select your preferred resource group for this exercise
  * Instance details
    * Storage account name: crackdetectorstorage<your initials>(please note this name must be unique, try different options if the one you selected is already in use. You can create multiple projects within the same service, we will learn about it in the next steps)
    * Location: select your preferred region
    * Performance: Standard
    * Account kind: StorageV2 (general purpose v2)
    * Replication: Read-access geo-reducdant storage (RA-GRS)
    * Blob access tier (default): Hot
 * Select **Review + create**
 * Select **Create**

![Step 7 Screenshot](/../main/Snapshots/Snapshots/Step%207%20-%20Create%20Storage%20Account.PNG?raw=true "Step 7")


## Step 8 - Create container to upload images

* In azure portal, select the storage account created in previous step
* Select **Storage Explorer (preview)** on the left side menu
* Right-click **BLOB CONTAINERS** and select **Create blob container**
* In the **New Container**, specify the following settings:
  * Name: **crack-inspection-files**
  * Public access level: container
* Select **Create**
* Upload sample image from **Concrete Cracks Dataset\Train\Positive**

![Step 8 Screenshot](/../main/Snapshots/Snapshots/Step%208%20-%20Create%20container%20to%20upload%20images.PNG?raw=true "Step 8")

## Step 9 - Create Logic App

* Login to [Azure Portal](https://portal.azure.com)
* Select **+ Create a resource**
* In search box, type **Logic App** and click Enter
* Select **Create**
* Your new service should have the following settings:
 * Project Details:
    * Subscription: select your preferred subscription for this exercise
    * Resource group: select your preferred resource group for this exercise
  * Instance details
    * Logic app name: crackdetectorlogapp<your initials>(please note this name must be unique, try different options if the one you selected is already in use. You can create multiple projects within the same service, we will learn about it in the next steps)
    * select the location: Region
    * Location: select your preferred region
    * Log Analytics: Off
* Select **Review + create**
* Select **Create**
* When the deployment is complete, select **Go to resource**
 

 
## Step 10 - Build Logic App

### Create Trigger when Image is uploaded
* Select **Blank Logic App +**
* In search box type **Blob**, select from **Triggers** the option **When a blob is addedd or modified (properties only)**
* Your new blob trigger should have the following properties:
  * Connection Name: crackdetectorstorage
  * Storage Account: select crackdetectorstorage (account created in [step 7](##step-7---create-storage-account)
  * Container: /crack-inspection-files
  * Number of blobs to return from the trigger: 1
  * How often do you want to check for items? 1 minute
  
  ![Step 10 Screenshot](/../main/Snapshots/Snapshots/Step%2010%20-%20Build%20Logic%20App%20-%20Blob%20Trigger.PNG?raw=true "Step 10")

### Create Action to call Custom Vision Prediction URL
* Select **+ New step**
* In search box type **Custom Vision**, select from **Actions** the option **Classify an image url (V2) (preview)**
* Your new action should have the following properties:
  * Connection Name: custvision<your initials>
  * Prediction Key:
     * Navigate to https://customvision.ai, open crack detector project and in the top row select **Performance**
     * Select **Prediction URL**
     * Copy the value for **Prediction-Key**
     * Navigate back to Logic App Designer and past it as the Prediction Key
  * Site URL: 
     * Navigate to https://customvision.ai, open crack detector project and in the top right corner select **Settings**
     * Copy the URL in **Endpoint**
     * Navigate back to Logic App Designer and past it as the Site URL
 * Select **Create**
 * Project ID: 
     * Navigate to https://customvision.ai, open crack detector project and in the top right corner select **Settings**
     * Copy the id in **Project Id**
     * Navigate back to Logic App Designer and past it as the Project ID
 * Publish Name: Crack-Detector-Model
     * Navigate to https://customvision.ai, open crack detector project and in the top row select **Performance**
     * Select **Prediction URL**
     * you can find the model name as part of prediction URL
 * Image URL: https://<REPLACE WITH NAME OF YOUR STORAGE ACCOUNT.blob.core.windows.net[List of Files Path] 
     * please note you need to select list of files path from the dynamic content

 ![Step 10 Screenshot](/../main/Snapshots/Snapshots/Step%2010%20-%20Build%20Logic%20App%20-%20Custom%20Vision%20Action.PNG?raw=true "Step 10")

### Create Conditional Statement
* Select **+ New step**
* In search box type **Control**, select from **Actions** the option **Condition**
* In **Choose a Value**, please select **Predictions Probabilities**

    ![Step 10 Screenshot](/../main/Snapshots/Snapshots/Step%2010%20-%20Build%20Logic%20App%20-%20Conditional.PNG?raw=true "Step 10")

* Expand Condition and select
   * Predictions Probabilities is greater than or equal to 0.75
   * Select **+ Add** and **Add row**
   * Predcitions Tag is equal to Positive
* In **True**, select **Add an action**
* In search box type **Outlook**, select from **Actions** the option **Send an Email (V2)**
* Sign in with a valid email, i.e. with access to outlook
* Your new action should have the following properties:
   * To: your email address
   * Subject: Crack Detector Alert on [created date]
   * Body: Please review this image [List of Files Path] we are [Prediction Probability] confident, there is a crack on it!!
   
    ![Step 10 Screenshot](/../main/Snapshots/Snapshots/Step%2010%20-%20Build%20Logic%20App%20-%20Email%20Action.PNG?raw=true "Step 10")
 
## Step 11 - Run and Test Logic App

* Select **Save** and then **Run** in logic app designer
* Navigate to storage account eplorer preview in the azure portal
* Navigate to **crack-inspection-files** container.
* Upload a new image which contains a crack from **Concrete Cracks Dataset\Test\Positive**
* Confirm if you have received an email in your inbox.
* In [Azure Portal](https://portal.azure.com), navigate to your logic app and check status and output of each step.
* Repeat the steps with an image with no cracks.


