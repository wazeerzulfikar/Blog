+++
categories = ["y"]
date = "2017-07-04T11:10:13+05:30"
description = "Getting closer to version 0.0.1"
tags = []
title = "Chapter 3 - Refactor, Test and Refactor"
draft=false

+++

Great News! Version 0.0.1 is closer to being ready!

Having established that importing graphs and building them is now possible within R, it was time to refactor the code, to make it convenient for the users. Atomics functions need to be exposed so the User can import any graph in a systematic and understandable way and also be able to build any custom graph. The semantics need to be clear such that it is close to the convenient Python API of Tensorflow.

Before breaking down the big functions, I added extensive unit tests. This will allow me to add a checkpoint after every stage of enhancement added to the code. Many functions such as setting the input nodes, feeding input to them, indentifying the output nodes and fetching output tensors are common for graph importing and building. Adding all of them to a shared `utils.h` led to an efficient implementation. Global pointers allowed for shared variables. But global pointers need extra care as its shared memory. All the input vectors need to be wrapped in tensors, while new tensors are allocated where outputs will be stored. 

Currently, I made an individual wrapper for the basic ops one would need to build neural nets. I wish to, in the future, make a more generic version where I can have a simple wrapper over the Rcpp routine for a more efficient and lesser code implementation.

Also, I used templates to extend the datatype compatibility. The package now accords for `int32` and `double`. I extended it to only these two as R only uses these in most cases, occurences of `int16` and `float` are rare in R. This is a bit of a messy implementation, but it works. I will need to get back to this later.

The next step to get it closer to a CRAN package release was to have a configure script. I used autoconf to generate the macros. The script when run checks for the dependencies, mainly `libtensorflow.so` (Obviously :P), and updates the compiler flags suitably to enable the linker to the tensorflow library.

My plan next, is to handle the ops better through a unique name generator, update ReadMe for guidelines for easy usage and link gnulib havelib to the autoconf to enable searching of the Tensorflow library from any location in the host system. Excited about the first release :D