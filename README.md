# Training a Deep Learning model for detecting metastasis doorways called TMEM
## What is TMEM - Tumor MicroEnvironemnt of Metastasis  
TMEM is an immunohistochemical biomarker for doorways on blood vessels that support tumor cell dissemination and is prognostic for metastatic outcome in breast cancer patients (PMIDs: [24895374](https://pubmed.ncbi.nlm.nih.gov/24895374/), [19318480](https://pubmed.ncbi.nlm.nih.gov/19318480/), [29138761](https://pubmed.ncbi.nlm.nih.gov/29138761/), [26269515](https://pubmed.ncbi.nlm.nih.gov/26269515/), [28838996](https://pubmed.ncbi.nlm.nih.gov/28838996/)). TMEM is composed of three cell types - an invasive tumor cell (TC), a macrophage (M) and an endothelial cell (EC) - in stable physical contact with each other.

Following image from [Robinson et al. 2009](https://pubmed.ncbi.nlm.nih.gov/19318480/), shows two examples of TMEM structures in triple IHC immunostained human breast cancer tissue. TMEMs, marked in black squares, were visually identified and drawn by pathologists.

![](https://github.com/ved-sharma/ZeroCostDL4Mic_Detecting_Metastasis_Doorways_in_Cancer/blob/24be5d5067de8a3f3ffc010c1b43f20d1a8b6efc/Files/TMEM_examples_github_v6.png)



## TMEM detection - Background
TMEM identification in formalin-fixed paraffin-embedded (FFPE) tumor tissue involves looking for direct contact of the three cell types that compose TMEM doorway within the tumor. This is done **manually** by pathologists and is a labor-intensive task.

Recently, our lab published a [machine learning method to detect TMEMs](https://pubmed.ncbi.nlm.nih.gov/32244564/) in fixed tissues. This supervised learning method utilized pixel classifcation followed by segmentaion to identify the three cell types of TMEM and then exapnding the size of TMEM-associated vessel to generate the TMEM ROI. This method involves careful pre and post processing of images such as smoothening, filtering and boundary separation of different cell types.

## Goal
We wanted to bypass all of the above described time-consuming and labor-intesive pre/post processing steps and remove all the user inputs which are required for pixel based classification and segmentation of objects. Therefore, we were interested in using a deep learning object detection model trained with just the pathologists drawn TMEM annotations and then use this trained model to detect TMEM doorway sites in images not seen by the model.

<!--
in FFPE tissues triple stained with anti-Mena (ligh pink, cancer cell marker), anti-Iba1 (brown, marrophage marker) and anti-CD31 (blue, endothelial cell marker)

Bayesian classification was used to detect and quantify TMEMs. This method requires training based on pixel classes and careful pre and post processing to classify each pixel belonging to either TC, Mac or EC classes. We were interested in trying a Deep Learning model to detect TMEMs in fixed tissues. The advantage of such methods is minimal to no pre/post processing of images and no user input on setting theshold for detecting the TMEM. Unlike Bayseian classifier (ref), we are not trying to identify individual 

Fixed PyMT tumor tissues were stained with pan-Mena (light pink, Tumor cells), Iba-1 (brown, macrophages) and CD31 (blue, endothelial cells) antibodies and whole-slide images were acquired on HISTECH P250 scanner. Following are examples of some 512x512 fields with TMEM identified and manually drawn by a pathologist in red bounding boxes. 
-->
## Training images and annotations
Following are two examples images from a PyMT tumor tissue stained with pan-Mena (light pink, identifies invasive Tumor cells, TC), Iba-1 (brown, macrophages) and CD31 (blue, endothelial cells) antibodies. Whole-slide imaging was performed on a Histech P250 slide scanner and 14, 512x512 areas were cropped. Ten out of 14 images were used for model training and rest 4 images were used for checking the model prediction.  

![](https://github.com/ved-sharma/Detecting_Metastasis_Doorways_in_Breast_Cancer_with_ZeroCostDL4Mic/blob/17d116a038972642d334a89ae01307a8aa772ac0/Files/train_2_images.jpg)

**Note 1:** Please note that anti-Mena staining (pink) is not that strong and highly variable from one image to the other. We are expecting our deep model to learn this variability during training and make correct predictions.

**Note 2:** Since annotations need to be in the Pascal VOC format for model training, I wrote a script to convert ImageJ ROIs to [Pascal VOC .xml annotations](https://github.com/ved-sharma/PASCAL_VOC_xml_generator_ImageJ_Fiji).

## Code
Here is the Google Colab notebook based on [ZeroCostDL4Mic](https://github.com/HenriquesLab/DeepLearning_Collab/wiki) framework.  

[![Open in colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/ved-sharma/Detecting_Metastasis_Doorways_in_Breast_Cancer_with_ZeroCostDL4Mic/blob/74f9499b89b6e1998f452f379cbf3b7c7ef3db97/TMEM_detection_YOLOv2_ZeroCostDL4Mic.ipynb)

## Model Training summary
- Object detection model YOLOv2 was trained for different epochs from 10-50 and 30 epochs were found to be sufficient for optimum model training.
- Augmentation (flip, rotation) was used to create additional training data - 50 augmented training images were generated from 10 original images. 
- Ten percent training data was used for validation. 
- Penalty values for false-positive (TMEM detected where there is none!) and false-negative (TMEM not detected where there is one!) detections were optimized and values of 1 and 10 respectively, gave the best model performace.

## Model performace
Left plots: Both training and validation losses dropped dramtically after epoch 3 and continued the downward trend till epoch 30.   

Right plot: More importantly mean average precision (mAP) for TMEM detection, which was close to zero in the beginning started climbing beginning epoch 6 and reached 0.92 (closer to 1 indicates good model fit) by epoch 25.

![](https://github.com/ved-sharma/Detecting_Metastasis_Doorways_in_Breast_Cancer_with_ZeroCostDL4Mic/blob/51506ba78aa9151191b700fef43ce0d6819026d2/Files/training_val_loss_mAP_vs_epoch.png)

## TMEM prediction on unseen images
I had four test images on which model inference was run. Two of these images are shown below with TMEM predictions, which looks great :)  

**Note:** you could see TMEM prediction results for all four images in the Google Colab notebook in the Code section above

![](https://github.com/ved-sharma/Detecting_Metastasis_Doorways_in_Breast_Cancer_with_ZeroCostDL4Mic/blob/f264972a45695c16e21942d31f7e0943d9923e85/Files/input_predicted_images_v2.jpg)
## Future plans
This work is a proof-of-principle to demonstrate that a Deep Learning model could be trained to detect TMEM doorways in fixed tissues. Training was successful, since the trained model was able to correctly detect TMEMs on unseen images. Here, I used a small dataset of 10, 512x512 images with minimal staining variation across the images in the training and test datasets. To generalize TMEM detection in tissues with wide variations in staining and to generalize to other conditions, need to train this model on more such images.

