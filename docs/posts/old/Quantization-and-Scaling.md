---
layout: post
title: "Scaling and Quantization"
tagline: "DIP HW1"
description: "Scaling a picture to different size"
category: notes
tags: [DIP,python]
excerpt: "Quantization and Scaling are basic operations in digital image processing. In this article I will discuss it detailedly."
---

## Scaling

First, we create an image object which has the desired input size. The calculat the Scaling
Rate(size_new/size_origin) for both height( m ) and width(‘n’). Then we will have a nested ‘for’ loop to
calculate the gray value for each pixel for the new image.
For down scaling, Isometric sampling and Mean sampling will be applied (respectively) to the image.
For Isometric sampling, there is a corresponding relationship, and we can easily calculate the gray
value of the pixel.

      x = int(i/m)
      y = int(j/n)
      new_image[i, j] = old_img[x,y]

For mean sampling,we will split the image into serveral small pieces which equals to the new down
scaling image and then we will calculate a area of pixels and then sum up then, and then calculate the average of the new pixel.

      (x1,y1)...(x1,yn)
      .
      .
      .
      .
      (xn,y1)...(xn,yn)

      average = sum(xi,yi) / n^2

Well, it’s very surprised to find that mean scaling is not always better than the Isometric sampling.
For up scaling , I use the Isometric sampling, for example, if we scale width = 2*width, the matrix of the
image will become:

        11 12 13
        14 15 16
        17 18 19

        11 11 12 12 13 13
        14 14 15 15 16 16
        17 17 18 18 19 19

Down-Scaling and Up-Scaling Formula(Isometric sampling):

$$
  G(x,y) = F(x/m, y/n);
$$

Up-Scaling Formula:

Up-Scaling Formula (Interpolation scaling)

$$
f(i,j)=w1 \cdot p1+w2 \cdot p2+w3 \cdot p3+w4 \cdot p4
$$

Origin Picture:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/14.png?raw=true">

96 * 64:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/scale_96*64.png?raw=true">

450 * 300

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/scale_450*300.png?raw=true">

<br>

## Quantization

For quantization, we similarily create a new image firstly. Then, we will calculat two values level and
segment. For a given gray level for example 4. It will divided the rgb set into 4 segemnts (0~64, 64~128,
128~172, 172~256), numbering with 0, 1, 2, 3 respctively. The variable segement represent the number.
Then we will have 4 gray values for 4-l level(0, 85, 170, 255). Then for each numbered position, there is
a solution for the new image:

        segement = 256 / level
        level = 255.0 / float(level - 1)
        new_image[i,j]
        = int(old_image[i.j] / segement) * level

Then we can calculate all the gray values for other pixels.

Origin Picture:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/14.png?raw=true">

Note that, in practice computers always represent “white” via the pixel value of 255, so you should also follow this rule. For example, when the gray level resolution is reduced to 4 levels,the resulting image should contain pixels of 0, 85, 170, 255, instead of 0, 1, 2, 3.

To 32 gray levels:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/quantization32.png?raw=true">

To 2 gray levels:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project1/code/quantization2.png?raw=true">

## Github:

[https://github.com/ghostbody/Image-Processing](https://github.com/ghostbody/Image-Processing)
