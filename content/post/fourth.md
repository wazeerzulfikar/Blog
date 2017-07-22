+++
categories = ["y"]
date = "2017-07-14T10:22:13+05:30"
description = "Emulating the Python API"
tags = []
title = "Chapter 4 - Essential Tweaks"
draft=false

+++

As I was working through, I realised the API still did not look much like the Python API. Lots of enhancements can be made to the existing API. 

One of the important parts of the framework I worked on were the *Ops*, after all they constitute the elements of the graph. The following changes were made to Ops handling by rtensorflow :
- Unique Name Generators : I made a unique string generator for reference to all the ops created in the working session. A permutation of 5 characters allowed for a large number of unique name possibilities. I used the characteristic Op type name as the base for easier debugging for users.
- Optional Name : Using the string generator now allowed for users to give optional custom names for the nodes created. This is useful while exporting graphs.
- Generic Routines : This development made adding new Ops to the package more streamlined. I created four basic deeper level functions for creating a Placeholder, Constant, Unary Ops and Binary Ops. This allowed for minimal wrappers in R to function as entry points to adding new Ops to the package.
- Constant Op Tensor Shape : To emulate the Python API, the constant op now does not require an explicit shape of Tensor value to be provided. 

The output from the Session is now returned to the R workspace itself as a multidimensional matrix. Then I removed the explicit function for fetching outputs (`fetchOutputs()`). The output op is specified while running the session. The output also is returned by the same function to the R workspace. 

I used templates for better data type compatibility, now the package supports `float`, `int32`, `double`, and `boolean`. Adding the remaining types is a low hanging fruit.

I also fixed a bug which did not allow multiple feeds to be passed to placeholders. It was a matter of clearing input values in wrong points of the workflow.

A crucial component for a complete R package was still missing, Documentation! I updated Documentation for all the exposed functions. Still need to add comments for utility functions deep inside the package. I updated README for easy installation and set up for new users.

Then for ease of new contributors, I refactored the whole code base following the [Google C++ style guide](https://google.github.io/styleguide/cppguide.html).

My next plan is to add a Vignette for an introduction to the package, work on adding important ops (especially activation functions), and the interesting enhancement,loading saved models from tensorflow.
