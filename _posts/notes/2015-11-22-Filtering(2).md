---
layout: post
title: "Filtering(2)"
tagline: "DIP HW2 & 3 & 4"
description: "Filtering(spatial and Frequency Domain)"
category: notes
tags: [DIP,python]
excerpt: "Image filtering, in as much as possible to retain the details of image under the target image noise suppression, is indispensable in image preprocessing operation, the treatment effect is good or bad will directly affect the validity and reliability of the subsequent image processing and analysis."
---

## Filtering in Frequency Domain

We can use the following serveral steps to implment the algorithm:

**1. Calculate the suitable size for the image which is suppose to be powers 2 and then padding zeros for both the origin image and the filter simply puting them at the letf top conrner. (Because in frequency domain, it does not matter where you put the image, but will have some side effect for the image’s edge).**

Resize function:

    def calculate_pow2(x):
      res = 2
      while res < x:
        res = res << 1
      return res

**2. Apply Fourier transform for both the images. Centerlize them respetively and then apply fft2d in question2(because it’s fast than using fft2d).**

    FFT_filter = FFT.centralize(FFT_filter)
    adjust_img = FFT.centralize(adjust_img)
    FFT_filter = FFT.fft2d(FFT_filter, 1)
    FFT_image = FFT.fft2d(adjust_img, 1)

**3. Using Convolution theorem:**

$$
f(x,y) * g(x,y) \Leftarrow \Rightarrow F(u,v) \times G(u,v)
$$

Let the two matrix to do array multiplication. And get the result image.

    Result = FFT_image * FFT_filter
    Result = FFT.fft2d(Result, -1).real

**4. Apply Inverse Fourier transform for the image, and get the real part as well as apply centerlize again.**

**5. Return the iamge and then do scaling for display.**

**Deatils:**

1. For laplacian filtering, the formula should be applied like that in project 2.:
Result[x, y] = 5 * img[x, y] - Result[x, y]
2. The output image will become brighter. In this way, I apply histogram equlization in project 2. This time I use gama
correction.
3. Log scaling is more better than linear:

$$
f'(x,y) = \frac{255}{\log_{10}256} \times \log_2(1+|\frac{f(x,y)}{max(f(x,y))}|\times 255)
$$

### Origin Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project3/code/14.png?raw=true">

### Smooth Result:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project3/code/Mean_Filtered_image.png?raw=true">

### Sharpen Result:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project3/code/Laplacian_Filtered_image.png?raw=true">


## Prove Convolution Theory

$$
f(t) * g(t) \Leftarrow \Rightarrow F(u) \times G(u)
$$

prov.

$$
FG(u,v)=Ft(\frac{1}{MN} \sum_i \sum_j f(i,j) \cdot g(x-i,y-j))
$$

$$
= \frac{1}{(MN)^2} \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} \sum_i \sum_j f(i,j) \cdot g(x-i,y-j)) \cdot e^{-2j\pi (\frac{ux}{M} + \frac{vy}{N})}
$$

$$
= \frac{1}{(MN)^2} \sum_i \sum_j f(i,j) \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} \cdot g(x-i,y-j)) \cdot e^{-2j\pi (\frac{ux}{M} + \frac{vy}{N})}
$$

$$
= \frac{1}{(MN)^2} \sum_i \sum_j f(i,j) \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} \cdot g(x-i,y-j)) \cdot e^{-2j\pi (\frac{u(x-i)}{M} + \frac{v(y-j)}{N})} \cdot e^{-2j\pi (\frac{ui}{M} + \frac{vj}{N})}
$$

$$
= \frac{1}{MN} \sum_i \sum_j f(i,j) G(u,v) \cdot e^{-2j\pi (\frac{ui}{M} + \frac{vj}{N})}
$$

$$
= F(u,v) \cdot G(u,v)
$$
