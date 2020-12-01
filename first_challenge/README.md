# Description


We wanted to classify images in 3 categories depending on the fact that all the people in the images are wearing masks, some people are wearing masks or 
nobody is wearing a mask. We use Convolutional Neural Networks that are the state of the art in the image classification field. The CNN acts like an encoder that tries 
to capture the different patterns of the images in an automatic way. Traditionally after the encoder we have a final fully connected classifier that takes as input the flatten feature vector and classifies the images. Different models can be used and tried based on the different datasets.\
We explain the various steps we took while developing the mask image classification on the final report.\
The final report can be downloaded [here](https://github.com/calde97/Deep_Learning_Challenge/blob/main/first_challenge/Report.pdf). Notice that the most important architectures are the the Inception model from scratch and the Xception model with Transfer Learning.
Remember that for more details you can have a look at the various jupyter notebooks. You can open them
here on Github or directly on Google Colab. If you want to try them yourself I suggest you to open them
on Colab

#### Accuracy results from kaggle ####

| Model from scratch | Transfer learning model |
|------------|----------|
|0.895|0.955|
