+++
categories = ["y"]
date = "2017-05-30T22:02:44+05:30"
description = "The inspiration and a brief overview of the project."
tags = []
title = "The Spark"
draft=false

+++

<img src="https://developers.google.com/open-source/gsoc/images/gsoc2016-sun-373x373.png" alt="Drawing" style="width: 50px;height: 50px;"/>

Google Summer of Code! The summer just got better!

Having worked with Tensorflow for quite some time, I was impressed by how amazing the library was, well it's not a surprise
as it is being backed by Google! Thats when I noticed that though the core of the Tensorflow architecture is written in C++ and exposed through a C API, only Python has a complete wrapper which  users could easily use to design neural nets. And there lies R, most commonly used for handling large amounts of data. How would it be if the Data Scientists could easily use Tensorflow to not only directly implement efficient machine learning algorithms on the data they handle, but also train them on GPU's! Wouldn’t that be awesome?

Well, that is exactly what I discussed with my mentor, Tomasz Melcer, and then proposed to the R Project for Statistical Computing. Considering that I did get accepted, I guess they also liked the idea :)

RStudio has actually wrapped Tensorflow already, but it is over the Python API. What makes my project different is that I am planning to wrap the C API, the core, directly. This is how Tensorflow recommends that language bindings be implemented. Interfacing directly with the core code has a lot of advantages such as removing the Python middleman improving stability, being easier to deploy and the fact that there is lesser overhead for calling any function.

*Rcpp*, a foreign function interface, allows for seamless integration of R and C++. Now that is a tool that will come in handy. According to the Tensorflow folks, a complete client language, like Python, must provide the following functionalities:

**1. Run a Predefined Graph** : Given a GraphDef (or MetaGraphDef) protocol message, be able to create a session, run queries, and get tensor results. This is sufficient for a mobile app or server that wants to run inference on a pre-trained model.

**2. Graph Construction** : At least one function per defined TensorFlow op that adds an operation to the graph. Ideally these functions would be automatically generated so they stay in sync as the op definitions are modified.

**3. Gradients (Automatic Differentiation)** : Given a graph and a list of input and output operations, add operations to the graph that compute the partial deriviatives (gradients) of the inputs with respect to the outputs. Allows for customization of the gradient function for a particular operation in the graph.

**4. Subgraphs as Functions** : Define a subgraph that may be called in multiple places in the main GraphDef. Defines a FunctionDefin the FunctionDefLibrary included in a GraphDef.

**5. Control Flow** : Construct "If" and "While" with user-specified subgraphs. Ideally these work with gradients.

**6. A Neural Network Library** : A number of components that together support the creation of neural network models and training them (possibly in a distributed setting).


The minimum requirement for a language binding is to support running a predefined graph and graph construction. The C API currently supports the above two minimum functionalities along with a recently added untested version of Gradients. My plan is to wrap whatever is available in the C API (satisfies minimum requirements) and then write the missing bits from Python to R, in the next phase.

| Feature        | Python           | C  |
| ------------- |:-------------:| -----:|
| Run a predefined graph     | tf.import_graph_def, tf.Session | TF_GraphImportGraphDef, TF_NewSession |
| Graph construction with generated op functions      | Yes     |   Yes (The C API supports client languages that do this) |
| Gradients | tf.gradients     |    Yes (Untested) |
| Functions     | tf.python.framework.function.Defun |  |
| Control Flow      | tf.cond, tf.while_loop      |    |
| Neural Network Library | tf.train, tf.nn, tf.contrib.layers, tf.contrib.slim      |     |

As you might have guessed, this is neither a one summer project nor a one man project. What I plan to do is provide a basic working prototype, by the end of this Summer, which can act as a foundation for enthusiastic developers. It can even be continued as future GSoC projects.

Hopefully, one day, an R user can use Tensorflow with the same ease as a Python one :)
