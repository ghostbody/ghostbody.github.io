
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

<img class="img-responsive"  src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/outImage.png?raw=true">

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project2/code/newHist.png?raw=true">

The result in fact looks better than the origin one.By this method, the brightness can be better distributed on the histogram. This can be used to enhance the contrast of the contrast without affecting the overall contrast, histogram equalization by effectively expanding the common brightness to achieve this function.

Let’s put the origin image and the output image together.

It strongly shows that the first image is much better than the second one since after the histogram equalization processing, the original relatively small pixel gray level will be allocated to other gray, the pixel is relatively concentrated, after processing gray range, contrast, large, clarity, so can effectively enhance the image.

Histogram equalization is a method to adjust the contrast in the field of image processing. This method is usually used to increase the local contrast of many images, especially when the contrast of the useful data of the image is very close. By this method, the brightness can be better distributed on the histogram. This can be used to enhance the contrast of the contrast without affecting the overall contrast, histogram equalization by effectively expanding the common brightness to achieve this function.

**Implement Details**

The target to get a histogram equalization is the same to get a corresponding relation between gray_value_old and gray_value_new .
The core algorithm of histogram equalization is to calculate such a table(array) for the image.

$$
s = T(r) = \sum_0^r p_r(i),0\leq r \leq 255
$$

      fx[i] = 255 * sum([hist[j] for j in range(i+1)]) / (width * height)

Note that fx indicates the corresponding relation table(array) for the origin image. And sum is function to calculate the formula above.
And also note that I implicitly do a scaling in the calculating.
Next it’s very easy, just traverse the array and then assign the table value for the new image.

      new_img[x, y] = fx[old_img[x, y]]

Happily , we got the histogram equalization result and then we can draw the histogram again and the result.

<br>

## Histogram Equalization in Color Domain:
Color histogram equalization is actually the image of one or more color channels for gray histogram equalization operations, common in the following ways:

Statistics of all RGB color channels of the histogram of the data and do a balanced operation, and then according to the balance of the mapping table to replace the G, B, R channel color value.

R, G, B color channels of the data and do a balanced operation, and then according to the R, B, G mapping table to replace the R, G, B channel color value.

Calculate the luminance channel by using the average value of the or the RGB, then calculate the histogram of the luminance channel and do a balanced operation, then replace the G, B, R channel color values by the mapping table.

Histogram equalization is a non-linear process. Channel splitting and equalizing each channel separately is not the proper way for equalization of contrast. Equalization involves Intensity values of the image not the color components. So for a simple RGB color image, HE should not be applied individually on each channel. Rather, it should be applied such that intensity values are equalized without disturbing the color balance of the image. So, the first step is to convert the color space of the image from RGB into one of the color space which separates intensity values from color components.

We can learn from it from another point.
If process the R, G, B channels separately, it will only have the information from R,G,B separately. If we usethe whole average, it use all the information from R,G,B.

**Reference:**

*From Wikipedia*

> The above describes histogram equalization on a grayscale image. However it can also be used on color images by applying the same method separately to the Red, Green and Blue components of the RGB color values of the image. However, applying the same method on the Red, Green, and Blue components of an RGB image may yield dramatic changes in the image’s color balance since therelative distributions of the color channels change as a result of applying the algorithm. However, if the image is first converted to another color space, Lab color space, or HSL/HSV color space in particular, then the algorithm can be applied to the luminance or value channel without resulting in changes to the hue and saturation of the image.[4] There are several histogram equalization methods in 3D space. Trahanias and Venetsanopoulos applied histogram equalization in 3D color space[5]

> However, it results in “whitening” where the probability of bright pixels are higher than that of dark ones.[6] Han et al. proposed to use a new cdf defined by the iso-luminance plane, which results in uniform gray distribution.[7]


Original Image:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task3/me.jpg?raw=true">

Histogram Equalization using independent histogram:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task3/output1.png?raw=true">

You can see, the image doesn't be enhanced. However, it become ugly.

Histogram Equalization using average histogram:

<img class="img-responsive"
src="https://github.com/ghostbody/Image-Processing/blob/master/project4/task3/output2.png?raw=true">

You can also use HSI to do Histogram Equalization, which is a proper way.

<br>

**Suppose that you have performed histogram equalization on a digital image. Will a second pass of histogram equalization (on the histogram-equalized image) produce exactly the same result as the first pass?Prove your answer.**

It is equivalent to all of the same number of pixels on the gray level, no matter how many times you have a number of histogram equalization, he has not changed.

Prove:

Assume that the first pass of histogram equalization result in gray level s.

First Pass:

$$
s = T_1(r) = \int_0^r p_r(\omega)d\omega, 0\leq r \leq 1
$$

According to probability theory:

$$
P_s(s) = P_r(r) \frac{dr}{ds}
$$

Second Pass:

$$
t = \int_0^s P_s(\omega)d\omega, 0\leq s \leq 1
$$

$$
t = \int_0^r P_r(\omega)\frac{d\omega}{dr}dr = \int_0^r P_r(\omega)d\omega, 0\leq r \leq 1
$$

$$\therefore$$ it's no sense to have a second pass histogram equalization

<br>

## Github:
[https://github.com/ghostbody/Image-Processing](https://github.com/ghostbody/Image-Processing)

## Reference:

- Acharya and Ray, Image Processing: Principles and Applications, Wiley-Interscience 2005 ISBN 0-471-71998-6
- Russ, The Image Processing Handbook: Fourth Edition, CRC 2002 ISBN 0-8493-2532-3
