# Using Deep Learning model for detecting intravasation doorways called TMEM
## What is TMEM - Tumor MicroEnvironemnt of Metastasis  
few sentences about TMEM and importance

## Challenge
So far, pathologists have identified TMEMs manully in fixed tissues. The goal of this project is to train a deep learning model based on pathologists' identified TMEM sites and then use this model for inference in to predict to To detect TMEM doorway sites in FFPE tissues triple stained with anti-Mena (ligh pink, cancer cell marker), anti-Iba1 (brown, marrophage marker) and anti-CD31 (blue, endothelial cell marker)

This is a Google Colab notebook for training a Deep Learning model and predicting TMEM doorways (blood vessel intravasation sites for tumor cells) in fixed tissues.


Recently, a machine learning method called Bayesian classification was used to detect and quantify TMEMs. This method requires training based on pixel classes and careful pre and post processing to classify each pixel belonging to either TC, Mac or EC classes. We were interested in trying a Deep Learning model to detect TMEMs in fixed tissues. The advantage of such methods is minimal to no pre/post processing of images and no user input on setting theshold for detecting the TMEM. Unlike Bayseian classifier (ref), we are not trying to identify individual 
