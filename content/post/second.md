+++
categories = ["y"]
date = "2017-06-20T12:45:44+05:30"
description = "Working with the low-level C API of Tensorflow to load and build graphs from scratch"
tags = []
title = "Week 2 - Diving into the C API"
draft=false

+++

In the previous update, I had successfully loaded a predefined graph (feed forward network) and instantiated a session with the loaded graph. Well, now I have run the graph, fed inputs and fetched outputs from the predefined network. The input was hardcoded so as to allow no room for error from that side. Running a session for the first time using the C API, essentially almost directly on the underlying kernel, was indeed a tedious task. A separate vector was needed to be initialised for each of input, output and target operations. Another vector for each of them was also needed to store the corresponding operations as pointers to pointers. Even the length of each vector is passed as an argument to the function. Aside from the vectors the essential graph and status pointers need to be passed as well. Along with the above buffers for run options and metadata is needed. So you can see, lots of arguments!

I realised, upon fetching the output, that the datatypes used and space allocated for every variable needs to be kept track of to ensure accurate results. Luckily, the tensorflow folks embedded a lot of accessors in the API, so that was convenient. I worked only with Int32 for now. 

For the first iteration of improved functionality, I added custom input. Since all the tensorfow operations work with Tensors defined using their interfaces, TF_NewTensor was handy to convert C++ arrays to Tensorflow Tensors. Here I faces an issue with the deallocator. While fixing that, is when I learnt the usages of malloc/free and new/delete. I converted all the malloc/free to new/delete for better memory allocation of the tensor objects, the only pitfall being needing to know the datatype beforehand.

With the import-run function working and after writing suitable tests, I started work with building graphs directly on the C API, from scratch. Again I was working on building the same feed forward network to maintain uniformity and easier testing. Every operation included a wrapper of its own. To initialise values into the constant operation, again the memory allocation needed to be taken care of. Building the graph worked successfully reproducing the results of the graph imported earlier, which had been built in Python.

Next, I will be working on refactoring the code, breaking down the big graph-loading and graph-building functions by defining C++ interfaces for small functions with atomic functionalities. This will make it closer to the actual functions which will be exposed to the R users later on. Additionaly, I will be adding more test cases, to ensure I am on the right track.