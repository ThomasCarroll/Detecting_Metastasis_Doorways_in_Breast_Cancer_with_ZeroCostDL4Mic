# Using Deep Learning model for detecting intravasation doorways called TMEM
## What is TMEM - Tumor MicroEnvironemnt of Metastasis  
few sentences about TMEM and importance
| ![](https://github.com/ved-sharma/ZeroCostDL4Mic_Detecting_Metastasis_Doorways_in_Cancer/blob/24be5d5067de8a3f3ffc010c1b43f20d1a8b6efc/Files/TMEM_examples_github_v6.png) |
|:--:|
| Two examples from [Robinson et al. 2009](https://pubmed.ncbi.nlm.nih.gov/19318480/) showing triple-cell TMEM structure  |


## Goal
So far, pathologists have identified TMEMs manully in fixed tissues. The goal of this project is to train a deep learning model based on pathologists' identified TMEM sites and then use this model for inference in to predict to To detect TMEM doorway sites in FFPE tissues triple stained with anti-Mena (ligh pink, cancer cell marker), anti-Iba1 (brown, marrophage marker) and anti-CD31 (blue, endothelial cell marker)

This is a Google Colab notebook for training a Deep Learning model and predicting TMEM doorways (blood vessel intravasation sites for tumor cells) in fixed tissues.


Recently, a machine learning method called Bayesian classification was used to detect and quantify TMEMs. This method requires training based on pixel classes and careful pre and post processing to classify each pixel belonging to either TC, Mac or EC classes. We were interested in trying a Deep Learning model to detect TMEMs in fixed tissues. The advantage of such methods is minimal to no pre/post processing of images and no user input on setting theshold for detecting the TMEM. Unlike Bayseian classifier (ref), we are not trying to identify individual 

Following are some examples of PyMT tumor tissues with TMEM annotations manually marked by a pathologist as red bounding boxes. There bounding boxes were drawn in Fiji and then converted to Pascal VOC format

## Model Training parameters
Object detection model YOLOv2 was trained for 30 epochs. Augmentation (flip, rotation) was used to create additional training data - 50 images were generated from 10 original training images. Ten percent training data was used for validation. Penalty values for false-positive (TMEM detected where there is none!) and false-negative (TMEM not detected where there is one!) detections were optimized and values of 1 and 10 respectively, gave the best model performace.

## Model performace
Both training and validation losses dropped dramtically at epoch 3 and slowly thereafter till epoch 30. More importantly mean average precision (mAP) for TMEM detection, which was close to zero in the beginning started climbing beginning epoch 6 and reached the max value of 1 by around epoch 20.

## TMEM prediction on unseen images
There were four images on which model inference was run. TMEM predictions look great :)
images for all 4 cases

## Future improvements
1. This model was trained on only 10 images of 512x512 size. To detect TMEMs in tissues with a wide variation in staining and to generalize to other conditions, need to train this model on more such images.
2. Since Google Drive only support 15 GB of free space, which fills out quickly with more data, I'd need to figure out how to run the Python code on a local machine with GPU.
