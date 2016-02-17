---
layout: post
title: "Histogram Equalization"
tagline: "DIP HW2"
description: "Filter an image"
category: notes
tags: [DIP,python]
excerpt: "Histogram equalization is a method in image processing of contrast adjustment using the image's histogram."
---

Image filtering, in as much as possible to retain the details of image under the target image noise suppression, is indispensable in image preprocessing operation, the treatment effect is good or bad will directly affect the validity and reliability of the subsequent image processing and analysis.

Histogram equalization is a method in image processing of contrast adjustment using the image's histogram.

Suppose that we have a image with histogram H1, our goal is to output a histogram of uniform distribution. In the process of uniforming, two conditions must be satisfied:

1. The output function should be Non-Decreasing.
2. The output function F' should satisfy  0 <= F' <= L-1 (L is the gray level of F)

For continues function:

$$
  s = T(r) \  (0 \leq r \leq 1)
$$

According to the two conditions, we have:

$$
  r = T^{-1}(s) \  (0 \leq s \leq 1)
$$

According to probability theory:

$$
P_s(s) = P_r(r) |\frac{dr}{ds}|
$$

We need to have a uniform distribution, so we let:

$$
  P_s(s) = 1
$$

$$
  \therefore |\frac {dr}{ds}| = |\frac {1}{P_r(r)}|
$$

$$
  P_r(r)dr = dT(r)
$$

For T(r), there can be a possible answer:

$$
T(r) = \int_0^r P_r(\omega) d \omega
$$

Now, we can get T(r), and T(r) is exactly the cdf(cumulative distribution function).

**Note that, in the condition of discrete, it may not work exactly.**

Origin Image:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/14.png?raw=true">

Origin Histogram:

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/originHist.png?raw=true">

After Equalization:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/newHist.png?raw=true">
