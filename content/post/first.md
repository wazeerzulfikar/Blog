+++
categories = ["y"]
date = "2017-06-10T12:02:44+05:30"
description = "The First Steps of the Project."
tags = []
title = "Week 1 - The Scaffolding"
draft=false

+++

A week of the coding period has been completed. As any package which is made from scratch would require, the first step for this project was to build a scaffolding on which the crux of the package can be built on. The scaffolding of the package contains all the imports needed, namely:

- Rcpp, for seamless integration of C++ and R
- roxygen2, for generation of documentation in the form of Rd files
- testthat, to support unit tests for the package

</br>
In addition to all the necessary components for a complete R package, namely /R, /man, /tests, Description and Namespace, a /src directory is needed where all the C++ files will reside, allowing the C++ functions to be exported. 

To actually wrap Tensorflow, we ran through multiple approaches. One way was to use the ProtoBuf files of Tensorflow and use a code generator to automatically generate headers and functions. After discussing with the lead developer of Rust wrapper for Tensorflow, Adam Crume, it was decided that protobufs may not be the best approach as generated proto code only helps marshal and unmarshal protos. So we decided to manually export Rcpp functions, and if possible later write a generator for automatically exporting them. 

After the scaffolding was built, I hard-coded a C++ function for a feed forward network to check the working of the Rcpp exports and its implementation. Once, the tests for these passed, I moved on to building the same feed forward network but now using Tensorflow graphs through the C API.

Firsty, Tensorflow needs to installed from source so that the C API is exposed and can be #included in C++ files. Since the Tensorflow C API is built as a dynamically linked library and distributed as a shared object (.so) file, certain flags are needed to be set for the g++ compliler to find the library in the system. 

In the R package, a Makevars file needs to be written for setting the flags to customize the g++ compiler. For now, I have hardcoded the flags to accomodate the paths to the Tensorflow library on my machine specifically, something to generalise later on.

Then, I moved onto the first major port of the C API, loading and running predefined graphs (Step 1 in the <a href="https://wazeerzulfikar.github.io/gsoc-blog/post/spark/">previous post </a>). I am working on first actually loading and running a predefined feed-forward network graph (made using Python) using the Tensorflow C API directly. The C API, I have noticed, is very low level. It is not made for user convenience but as a foundation for other languages to build wrappers on. As it is directly connected to the Distribution Master and DataFlow Executor, which in turn resides over the kernel, most functions like Session Running requires various specific parameters to be passed. Currently, I have successfully imported a graph and initialised a session associated with it. Now I am working on running the session, feeding inputs to the graph and fetching outputs from it. This would create a model around which I can build the interface for importing and running predefined graphs.