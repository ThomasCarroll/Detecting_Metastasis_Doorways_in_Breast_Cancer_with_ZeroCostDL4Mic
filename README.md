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

Fixed PyMT tumor tissues were stained with pan-Mena (light pink, Tumor cells), Iba-1 (brown, macrophages) and CD31 (blue, endothelial cells) antibodies and whole-slide images were acquired on HISTECH P250 scanner. Following are examples of some 512x512 fields with TMEM identified and manually drawn by a pathologist in red bounding boxes. 

> some rep images  

TMEM ROIs were then converted to [Pascal VOC annotations](https://github.com/ved-sharma/PASCAL_VOC_xml_generator_ImageJ_Fiji) format in ImageJ/Fiji

## Model Training summary
- Object detection model YOLOv2 was trained for 30 epochs.
- Augmentation (flip, rotation) was used to create additional training data - 50 images were generated from 10 original training images. 
- Ten percent training data was used for validation. 
- Penalty values for false-positive (TMEM detected where there is none!) and false-negative (TMEM not detected where there is one!) detections were optimized and values of 1 and 10 respectively, gave the best model performace.

## Model performace
Both training and validation losses dropped dramtically after epoch 3 and continued the downward trend till epoch 30. More importantly mean average precision (mAP) for TMEM detection, which was close to zero in the beginning started climbing beginning epoch 6 and reached the max value of 1 by around epoch 20.

>three plots side by side

## TMEM prediction on unseen images
I had four test images on which model inference was run. TMEM predictions look great :)

>images for all 4 cases

## Future work
This project was the proof-of-principle to see if a Deep Learning model could be trained to detect TMEM doorways in fixed tissues. Training was successful, since the trained model was able to correctly detect TMEMs on unseen images. Here, I used a small dataset of 10, 512x512 images with minimal staining variation across the images in the training and test datasets. To generalize TMEM detection in tissues with wide variations in staining and to generalize to other conditions, need to train this model on more such images.

