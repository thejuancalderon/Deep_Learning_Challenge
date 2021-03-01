## We will go through all the choices made during this challenge. 

## Baseline
We started from the model seen in the exercise class(Class activation map), but the performances were really poor. We think that this was due to the lack of skip connections and learnable weights. The result obtained is a Global IoU of 0.3499. 

## U-Net Notebook
After that, we moved to a standard U-Net architecture, with an Encoding/Decoding depth of 5. Given the computing and memory limits, we opted for an input size of 768x1024 pixels, a trade-off between image resolution and network complexity. The output of the network is a 3 channels image, where each channel defines the probability of each pixel to belong to one of the 3 possible classes, which are for instance: background, crop and weed.  We tried different image augmentations, ending up with the ones in the notebook. 
Moreover, we performed different training over different combinations of the four datasets. We obtained the best result using all the datasets (BipBip, Pead, Roeau, Weedelec), with a Global IoU of 0.6627. 

## CED-Net Notebook
After U-Net, we started searching for other solutions which could perform better, taking inspiration from available state-of-the-art papers. The 2 main papers we ended up with were: 
* Double_segmentation : in this paper a first segmentation between background and vegetation is performed. Then we perform “blob extraction” to extract the vegetation blobs. Finally a second segmentation is performed on the blobs, between crop and weed. 
* CED-Net : Two segmentations at different resolution levels are performed. Weed and crop are segmented individually.
After some reasoning on the possible implementation, we opted for the second solution. 
In the CED-Net architecture, two segmentations are performed in parallel, one between background and crop, the other between background and weed. 
Moreover each segmentation is divided into two segmentations at different resolutions. We will refer to the first 2 lower resolution segmentations with Level 1 segmentations, and the last 2 higher resolution segmentations with Level 2 segmentations. 
The output of the Level 1 segmentations is upsampled, from 448x448 up to 896x896 pixels, and it is concatenated with the images at higher resolution. In this way the input of the level 2 segmentations are images with 4 channels (RGB + previous prediction). 
The outputs of the Level 2 segmentations are images with one channel, representing the probability for each pixel to belong to crop vs background or weed vs background.
Finally, these two outputs are merged together in a 3-channel image, in which the first channel represents background, the second crop and the third weed. Since the final evaluation is done with an argmax operation on the different channels, the background channel is created with a value of 0.45 for each pixel, while the others are the results of the output of Level 2 models (which present sigmoid activation functions). The argmax operator will choose background when the other two channels have values around zero, while it will choose the max between weed or crop when at least one of the two is near one (larger than background level 0.45). 
Searching for the best performance, we did different tests, exploiting image augmentations, hyperparameter tuning and different combinations of the given datasets. Since CED-Net is composed of 4 different models, we have been able to optimize each one of them, independently. The result obtained is a Global IoU of 0.5731. 


Baseline
0.3499
U-Net
0.6627
CED-Net
0.5731



Final Considerations 
The performance with the CED-Net architecture was not good as expected. Since the model did not overfit during the training, a possible improvement could be increasing its complexity. Another possibility would be to perform tailing on the images, for training at higher resolution. If we had more time, we could have tried to implement the first paper, Double_segmentation. 
