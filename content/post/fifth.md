+++
categories = ["y"]
date = "2017-07-24T21:38:43+05:30"
description = "Added important feature to load saved models"
tags = []
title = "Chapter 5 - Trainable Models"
draft=false

+++

Big feature update! So last week I started working on a new feature to add to tensorflow, loading saved models. This feature will allow an R user to not only load the protobuf graph from python but also load the weight and bias values. First I worked on a simple regressor (Python code available [here](https://github.com/wazeerzulfikar/rtensorflow/blob/master/tests/regress.py)). After successfully loading the graph, I was able to serve the model. Or in other words, feed my data and fetch outputs from it.

But, why stop there? So I pushed on and tried to further train the model as well. It worked. Inspite of not having stable functions to handle gradients in the C API, I was able to use gradients functions through previously saved models and loading them. For faciliting training, the trainable ops are sent to the session as target operations, unlike the usual output operations. Initially, the training feature had to be set explicitly while running the session, but I automated (implicitly checked) this later.

Then I moved to a bigger and more complex model and dataset, namely Convolutional Neural Nets and the MNIST handwritten digits respectively. But before I could delve deeper, I had to take care of unknown dimensions in placeholders (The -1 dimension, for those who use NumPy). On preprocessing the data and feeding it to the model, the output dimensions was slightly off. I figured that it was because in an R multidimensional matrix (also called array), the shallowest dimensions are filled first, unlike in Python (where deepest dimension is filled first). A simple reverse of dimensions and a transposition of the nd matrix solved the problem. Not surprisingly, training of the CNN was feasible. A small snippet of the output is below :

```
> output <- check_mnist("./tests/saved-models/mnist-model/", "../mnist_data/train.csv")
Sucessfully instantiated session variables
2017-07-26 22:09:20.575804: I tensorflow/cc/saved_model/loader.cc:226] Loading SavedModel from: ./tests/saved-models/mnist-model/
2017-07-26 22:09:20.599068: I tensorflow/cc/saved_model/loader.cc:145] Restoring SavedModel bundle.
2017-07-26 22:09:20.735830: I tensorflow/cc/saved_model/loader.cc:180] Running LegacyInitOp on SavedModel bundle.
2017-07-26 22:09:20.738360: I tensorflow/cc/saved_model/loader.cc:274] Loading SavedModel: success. Took 162571 microseconds.

[1] "Data read successful"

Iter  1 ,  Cost= 39661.59,  Training Accuracy= 0.1328125 
Iter  11 ,  Cost= 13616.89,  Training Accuracy= 0.4296875 
Iter  21 ,  Cost= 5783.97,  Training Accuracy= 0.6640625 
Iter  31 ,  Cost= 3996.494,  Training Accuracy= 0.734375 
Iter  41 ,  Cost= 4015.983,  Training Accuracy= 0.7890625 
Iter  51 ,  Cost= 3367.392,  Training Accuracy= 0.765625 
Iter  61 ,  Cost= 3081.917,  Training Accuracy= 0.8359375 
Iter  71 ,  Cost= 2018.533,  Training Accuracy= 0.859375 
Iter  81 ,  Cost= 1984.596,  Training Accuracy= 0.875 
Iter  91 ,  Cost= 3046.545,  Training Accuracy= 0.859375 
Iter  101 ,  Cost= 1464.636,  Training Accuracy= 0.9375 
...
```

I also made a critical feature addition of passing multiple output and target operations to the session to run it. The output is returned in a dictionary format with all the outputs of the output operations contained in it while running the session. The particular outputs in the returned dictionary can be referenced with the op name used a key.

I also added a Vignette, `Introduction.Rmd`. This vignette explains (with an example each) how to load saved models, import and run protobuf graphs and build and run custom graphs in a detailed step by step manner.

Next, I plan to allow multiple sessions to run simultaneously and test out the experimental gradients C API and see if building trainable models from R is possible.

