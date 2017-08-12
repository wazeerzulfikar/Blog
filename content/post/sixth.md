+++
categories = ["y"]
date = "2017-08-04T12:18:43+05:30"
description = "Easy fixes and making it Contributor Friendly"
tags = []
title = "Chapter 6 - Open Source"
draft=false

+++

This phase of the project involved lesser work and definitely a breather compared to the previous 5 phases. I focused on another aspect of open source development. Open Source is not only meant for sharing code but allowing others to contribute as well. To make my package contributor friendly, I sat to follow the Google C++ style guidlines and the R guidlines by Hadley Wikham. This also provided better readibility for new users, making it easier for them to get started. Infact the package already has another contributor now.

I was also delighted to discover that the [HomeBrew PR](https://github.com/Homebrew/homebrew-core/pull/10273) has been merged. Now a homebrew formula for installing `libtensorflow` exists, making it easier for macOS users (Earlier the install name needed to be changed using otool). So thats good news for macOS users.

Having implemented a basic version of `tf.Variable`, I am currently trying out the experimental gradients provided by the Tensorflow C API.

For the ops, I am planning to write a R code generator to parse the protobuf text file residing in the Tensorflow repository. If implemented, all ops ranging from `Adam` to `Add` should have a usable and efficient wrapper implementation to use with rtensorflow.

Another major hinderance I will need to solve in the next phase, is accessing the array-like pointer of the input Rcpp vector while creating new tensors. Right now, a native C++ array is initialized with the Rcpp vector contents, but this might possibly slow down computation, especially around placeholders.