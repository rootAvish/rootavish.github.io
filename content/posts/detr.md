---
title: "DETR: An end to end object detection pipeline"
date: 2024-04-21T01:05:28+05:30
draft: false
categories:
    - computer vision
    - deep learning
    - object detection
---

I recently started a project on object detection where I chose DETR as my object detector of choice
because the formulation looked simpler, and because it's in Huggingface which is the library
I'm the most comfortable with in the deep learning ecosystem.

As of this writing however, I've hit a roadblock where I'm getting a high false positive rate for a
certain class in my project and I'm kind of out of ideas for what's going to work for me. So I'm
going through the theory now, to figure out how to do one final run that solves my problem for me so
that I may take my work live.

So now that the context on this is set, let's get into the theory and at the end of this post I'll
detail what I'd been trying thus far, if my insights into this have changed and the next experiment
I'll try based on the theory. I hope to god this works out.


##  High Level

* Uses first a CNN, and then a transformer to detect objects.
* A "bipartite matching" objective.
* Much simpler architecture than previous approaches that had hand engineered stuff


## What is object detection?

Given an image, find all the objects - what they are and where they are (my current task only cares
about the what mostly).

Number of very difficult things:

* Detect the objects
* Detect how many there are
* overlaps
* small/big
* partial occlusion etc.
* Used to require very complicated heuristics (classify two pixels check proximity and what not)

## DETR architecture

* Put the image through a CNN encoder
* High level, in a CNN you go from a high res 3 channel thing to a low res many channel thing
* Important: This retains information about the location of the object in the image.
* Goes into transformer encoder-decoder
* Outputs a set of box predictions
* Each box prediction is a tuple - (class, bounding box)
* The box prediction can be a "nothing" class - &Phi;
* The number of predictions *is always the same.*


## Formulation nuances

* Ordering shouldn't matter - just all bounding boxes are to be found
* Regressor can output slightly off bounding boxes, classifier can even output multiple objects in the same bounding box
* Number of predictions will always be the same - need to predict nothing class wherever objects are not present.

This is all fine and dandy but the money question here is how would you train such a thing and why should it work?

How does paper suggest we deal with all the nuances of object detection? **A bipartite matching loss.**

Important: The number of ground truth labels *per image* also becomes N because you have to compare to it. The classifier component's supposed to correctly predict everything else as &Phi;.

In order to not predict the same object with a slightly different bounding box, we calculate a maximum matching when we calculate the loss. Specifically, we want a one-to-one assignment such that the total
loss in minimized. So we need to align the two to be maximally favourable to the model's predictions. This solves the sequence problem because the matching will be calculated irrespective of what the order of the predictions is (loss of a prediction is only calculated using its partner).

## Object queries

While everything else is relatively straightforward adaptations of the transformer architecture to the task
of attending to pixel information, a key detail is what goes as input to the decoder since that's required for transformer prediction.

Answer- start with N random vectors and learn feature vectors as "object queries".

# Detr Variants


## Deformable DETR

* There's a thing called a "deformable convolution" - essentially you deform the conv kernel to remove the simplification that a conv kernel that always looks at a certain number of pixels can learn good features. For small objects you'd have more dense points and for large points we scatter.

* So that gives rise to an idea of learable offsets. Intead of performing convolution with the neighbors, you use the neighbors to predict an offset and the deformable convolution computes the feature map value using a convolution of the convolution kernel and those offset pixels replacing the neighbors.

* Selecting a pixel is a non-differentiable operation. We use bilinear interpolation to predict closest full pixel value after a real valued offset prediction is returned. In practice, this works quite well.

* A similar idea is tha that of deformable attention.
