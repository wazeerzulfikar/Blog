+++
categories = ["y"]
date = "2017-05-30T22:02:44+05:30"
description = "The inspiration and a brief overview of the project."
tags = []
title = "The Spark"
draft=false

+++

Google Summer of Code! The summer just got better!

Having worked with Tensorflow for quite some time, I was impressed by how amazing the library was, well not a surprise
as it is being backed by Google! Thats when I noticed that though the core of the Tensorflow architecture is written in C++ and exposed through a C API, only Python has a complete wrapper which  users could easily use to design neural nets. And there lies R, most commonly used for handling Big Data. How would it be if the Data Scientists could easily use Tensorflow to directly implement machine learning algorithms on the Data they handle! Wouldn’t that be awesome?

Well, that is exactly what I proposed to the R Project for Statistical Computing. Considering that I did get accepted, I guess they also liked the idea :)

RStudio has actually wrapped Tensorflow already, but it is over the Python API. What makes my project different is that I am planning to wrap the C API, the core, directly. This is how Tensorflow recommends that language bindings be implemented. Interfacing directly with the core code has a lot of advantages such as removing the Python middleman improving stability, being easier to deploy and the fact that there is lesser overhead for calling any function.

Rcpp, a foreign function interface, allows for seamless integration of R and C++. Now thats a tool that will come in handy. According to the Tensorflow folks, a complete client language, like Python, must provide the following functionalities:

* Run a Predefined Graph
* Graph Construction
* Gradients (Automatic Differentiation)
* Subgraphs as Functions
* A Neural Network Library


The minimum requirement for a language binding is to support running a predefined graph and graph construction. The C API currently supports the above two minimum functionalities along with a recently added untested version of Gradients. My plan is to wrap whatever is available in the C API (satisfies minimum requirements) and then write the missing bits from Python to R, in the next phase.

As you might have guessed, this is neither a one summer project nor a one man project. What I plan to do is provide a basic working prototype, by the end of this Summer, which can act as a foundation for enthusiastic developers. It can even be continued as future GSoC projects.

Hopefully, one day, an R developer can use Tensorflow with the same ease as a Python one :)
