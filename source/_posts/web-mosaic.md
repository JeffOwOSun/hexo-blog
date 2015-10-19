title: Web Mosaic
id: 228
categories:
  - 'Coding & Geek Stuff'
date: 2015-10-14 08:04:17
tags:
---

## Background

In the first project of COMP4411: Computer Graphics course there was an extra requirement about a web mosaic effect.

Here is the example given in the text: [Web Gothic](http://www.cs.princeton.edu/~af/cool/webgothic.html)

[![WebGothic](http://wordpress.jowos.moe/wp-content/uploads/2015/10/WebGothic-812x1024.jpg)](http://wordpress.jowos.moe/wp-content/uploads/2015/10/WebGothic.jpg) Web Gothic

<!--more-->

## The idea

My idea was straight forward:

*   Given a source image and a bunch of alternatives, the first step is to slice the image into patches and
*   then compare with alternatives and
*   finally replace the original patches with closest alternatives.

## The Dataset

The problem was, how to find the alternatives.

The author of Web Gothic obviously didn't offer any clue: _made it by collecting a bunch of images off the Web._ I got to figure out some way else to get these images.

Fortunately, I had experience with CIFAR dataset during my UROP this summer. Back then I used CIFAR-10 to train an Convolutional Neural Network model using MatConvNet with MATLAB. This time the scenario is a little bit different, of course.

[![CIFAR-10 Dataset](http://wordpress.jowos.moe/wp-content/uploads/2015/10/Screen-Shot-2015-10-17-at-16.11.33-300x208.png)](http://wordpress.jowos.moe/wp-content/uploads/2015/10/Screen-Shot-2015-10-17-at-16.11.33.png) A brief idea of CIFAR-10 Dataset

## The Performance

So we downloaded CIFAR-10 binary format and fiddled with it in C++. Apparently C++'s merit with direct binary manipulation eased our job. Our metric is simple: treat the 32x32x3 bitmap as 3072-dimentional vector, and calculate the norm of the differences. This constitutes an $ O(n^2)$ algorithm. Not too bad. After turning on /O2 switch in Visual Studio 2013, we could achieve 1000 pics/s performance in my 2-core virtual machine running on my MBPR 13\. The whole dataset, 60000 32x32x3 bitmaps in total, took 1 minute to process.

[![mosaic for babyraptor.bmp](http://wordpress.jowos.moe/wp-content/uploads/2015/10/rapter-mosaic.bmp)](http://wordpress.jowos.moe/wp-content/uploads/2015/10/rapter-mosaic.bmp) mosaic for babyraptor.bmp

I'd say the result is pretty good. One short coming is the dataset only consists of objects, so you could see patches of birds over blue sky being substitute into the solid blue areas of the original image, and the grayish birdie really annoys when such patches comes in flocks.

[![Web mosaic for my favorite image of Hatsune Miku by Achunchun.](http://wordpress.jowos.moe/wp-content/uploads/2015/10/the-stars-in-your-eyes.bmp)](http://wordpress.jowos.moe/wp-content/uploads/2015/10/the-stars-in-your-eyes.bmp) Web mosaic for my favorite image of Hatsune Miku by Achunchun.

So I set out for another dataset, ImageNet. It's at the back of the famous yearly ILSVRC competition. I immediately gave it up when I saw the 1.2TB size of the dataset.

## Review

The professor later suggested in our presentation of the algorithm that preprocessing the datasets to extract a scalar hash for comparison would be much faster. That's right. If we are going to implement something similar in the future we would definitely consider that. But not this time.
