---
layout: post
title: "Filtering(1)"
tagline: "DIP HW2 & 3 & 4"
description: "Filtering(spatial and Frequency Domain)"
category: notes
tags: [DIP,python]
excerpt: "Image filtering, in as much as possible to retain the details of image under the target image noise suppression, is indispensable in image preprocessing operation, the treatment effect is good or bad will directly affect the validity and reliability of the subsequent image processing and analysis."
---

## Spatial Domain Filtering

**Tasks:**

Basic:

1. Blur the Image. (Mean filtering)
2. Sharpen the Image. (Laplacian filtering)

Noise Reduction:(with introduction to image restoration)

Noise modles: Gussian, Salt and Pepper.

1. Contraharmonic mean filtering
2. Arthmetic mean filtering
3. Geometric mean filtering
4. Median filtering

## Basic

For a selected filter and a selected pixel, we need to calculate the as we want the convolution result of the image. We defined a function to calculate the gray value for the exactly one pixel.

    Convolution(x, y, filter) → new_gray_value

    def convolution(arguments):
      for i in range(filter_size):
        for j in range(filter_size):
          if out_of_range:
            center_value += 0
          else:
            center_value += filter[i][j] * image_p
      center_value /= filter_size * filter_size

Then we can calculate the center_value for one pixel in the origin image. Then we can have a nested for loop to calculate the whole image.

    def filter2d(arguments):
      for i in range(height):
        for j in range(width):
          new_image[i,j] = convolution(i,j, filter)

Notice that, when I am doing laplacian_filter, the result is very wired. As a result, we need to compelete scaling after operation. What’s more, I did a gama correction as wil to have a better view.

scaling method:

$$
s = |S(x,y)| = |\frac{f(x,y)-min(f(x,y))}{max(f(x,y)) - min(f(x,y))}| \times 255
$$

I use the same method in histogram equalization that to set up a table for gama function.

      for i in range(256):
        val = pow(float(i)/255.0 ,scale) * 255.0
        if val>255:
          val = 255
        elif val<0:
          val = 0
        table[i]= val

I meet a problem when using the laplacian method. The noise of the image is also enhanced. So I adjust the method with:

$$
g = G(x,y) = k\times f(x) + L(x,y)
$$

For high_boost_filtering:

First we should calculate the Fuzzy image:

    for i in range(height):
      for j in range(width):
        sub_img[i ,j] = img[i, j][0] - fuzzy_img[i][j]

And we get the mask sub_img , and then we calculate the output image:

    for i in range(height):
      for j in range(width):
        enhanced_img[i, j] = img[i, j][0] + k * sub_img[i][j]

And then we get the enhanced_img by doing the formula.

$$
g(x,y) = f(x,y) + k\times g_m(x,y)
$$

The function of image sharpening is to make the gray contrast enhanced, so that the fuzzy image becomes more clear. The essence of the image is that the image is subjected to an average operation or an integral operation, so it can be carried out on the inverse operation, such as differential operation can highlight the image details, so that the image becomes more clear.

$$
\nabla f^2 = [f(x+1,y)+f(x-1,y)+f(x,y+1)+f(x, y-1)
$$

$$
+f(x+1, y+1)+f(x+1,y-1)+ f(x-1,x+1)+f(x-1,y-1)] - 8f(x,y)
$$


### Result

original image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/14.png?raw=true">

3*3 Mean Filter:

$$
  \frac{1}{9} \left[
  \begin{matrix}
   1 & 1 & 1 \\
   1 & 1 & 1 \\
   1 & 1 & 1
  \end{matrix} \right]
$$

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/Image1.png?raw=true">

7*7 Mean Filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/Image2.png?raw=true">

11*11 Mean Filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/Image3.png?raw=true">

laplacian sharpen result:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/Image4.png?raw=true">

high_boost_filtering result with k = 0.8:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/Image5.png?raw=true">

<br>

## Bar Picture Testing

### Filter input image with 3 × 3 and 9 × 9 arithmetic mean filters respectively.

Origin Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/task_1.png?raw=true">

3*3 arithmetic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/arithmetic_mean_filter3.png?raw=true">

9*9 arithmetic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/arithmetic_mean_filter9.png?raw=true">

3*3 contraharmonic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/arithmetic_mean_filter3.png?raw=true">

9*9 contraharmonic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/arithmetic_mean_filter9.png?raw=true">

3*3 harmonic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/harmonic_mean_filter3.png?raw=true">

9*9 harmonic mean filter:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task1/harmonic_mean_filter9.png?raw=true">

<br>

## Noise Reduction

Review that:

arithmetic:

$$
f(x,y) = \frac{\sum_{i,j\in S(x,y)} f(i,j)}{m\times n}
$$

geometric:

$$
f(x,y) = \sqrt[nm]{\prod_{i,j \in S(x,y)}{f(i,j)}}
$$

harmonic: Notice that we defined

$$
\frac{1}{0} = max(\frac{1}{f(x,y)}) = 1
$$

    if img[img_x, img_y] == 0:
      centre_gray_value += 1
    else:
      centre_gray_value += 1.0 / (img[img_x, img_y] * img_filter[i][j])

contraharmonic: Calculate two converlosion

      centre_gray_value1 = Convolution(img, img_filter, x, y, method,q+1)
      centre_gray_value2 = Convolution(img, img_filter, x, y, method, q)
      if centre_gray_value2 != 0:
        return int(float(centre_gray_value1) / centre_gray_value2)
      else:
        return 0

max, median and min: Write another function to process the points, store them to new array.

      if method == "median":
        array.sort()
      if filter_size ** 2 % 2 == 0:
        return array[filter_size*filter_size / 2]
      else:
        return int(float(array[filter_size*filter_size / 2] + array[filter_size filter_size / 2 -1])/2)
      elif method == "max":
        return np.amax(array)
      elif method == "min":
        return np.amin(array)

### Reduction Result:

#### Gussian Noise Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/1_GaussianNoise.png?raw=true">

arithmetic mean:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/1_GaussianNoise_arithmetic.png?raw=true">

geometric mean:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/1_GaussianNoise_geometric.png?raw=true">

Arthmetic Mean Filter can really reduce some noise because it’s a low pass filter. But it also blurred the image. It looks OK by using 5x5 filter. 9x9 filter is bad in practical. Geometric Mean Filter seems like that it can reduce more noise. It looks OK overall. But it also bolds the edge of the black elements in the image, because one zero will lead to result 0 when filtering.
Median Filter can have the effect that Arthmetic Mean Filter has. It can denois the picture and also make the picture look better.
In a word, I think that median filter is better when filtering Guassian Noise.

<br>

#### Salt Noise Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/2_SaltNoise.png?raw=true">

contraharmonic(negative)

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/1_SaltNoise_contraharmonic_negative.png?raw=true">

contraharmonic(positive)

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/1_SaltNoise_contraharmonic_positive.png?raw=true">

Setting a wrong Q value can lead to terrible results because the contraharmonic mean filter can not filter both pepper noise and salt noise.
When Q is positive, the filter can reduce pepper noise. Because the most significant value is big value and the pepper value is small numbers, the black points can be removed by using this way.
When Q is negative, the filter can reduce salt noise. Because the most significant value is small value(Reciprocal Sum). The Salt value is small numbers, the white points can be remove by using this way.
However, if you use the wrong method, terrible things will happen like using Q=1.5 to reduce salt noise, because the most significant value is big value, the white noise will become louder in this way which will lead to terrible result.

<br>

#### Salt and Pepper Noise Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise.png?raw=true">

arithmetic mean:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise_arithmetic.png?raw=true">

geometric:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise_geometric.png?raw=true">

max:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise_max.png?raw=true">

min:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise_min.png?raw=true">

median:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task2/3_SaltAndPepperNoise_median.png?raw=true">

Median Filter looks best, and then arithmetic mean. Other filter looks very ugly.
Median Filter can do the best. In image processing, such as edge detection in the further processing of this before, usually need a certain degree of reduction Median filtering is a common step in image processing.
It is especially useful for speckle noise and impulse noise. Save the edge of the feature so that it does not want to appear on the edge of the occasion is also very useful. arithmetic mean filtering looks ok here. It’s the same in Guasian Noise, Arthmetic Mean Filter can really reduce some noise because it’s a low pass filter. But it also blurred the image. It looks OK by using 5x5 filter. 9x9 filter is bad in practical.
Geometric mean filtering is bad, because one zero will lead to result 0 when filtering. The points near one pepper noise will lead to a big area of black, it’s very terrible. max and min is bad, because they can only reduce one kind of noise not both pepper and salt. Max can reduce pepper noise very well and Min can reduce salt noise very well.
