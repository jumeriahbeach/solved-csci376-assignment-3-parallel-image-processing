Download Link: https://assignmentchef.com/product/solved-csci376-assignment-3-parallel-image-processing
<br>
For this assignment, a test image “peppers.bmp” has been provided.

You will find that the examples in Lab 9 on parallel image processing will be very useful for this assignment. You are allowed to use code from the labs in your assignment.

Note that if you use the code from the lab on image processing, the red, green, and blue values range from 0 to 255 (unsigned char) on the host and 0.0 to 1.0 (float) on the device.




Task 1 (Basic image manipulation)

Write a parallel OpenCL program using image objects to flip an image. Your kernel should accept an input image object and three output image objects. For the respective output images, flip the input image as follows:

<ol>

 <li>Flip the image in the horizontal dimension (i.e. left become right, and vice versa)</li>

 <li>Flip the image in the vertical dimension (i.e. top becomes bottom, and vice versa)</li>

 <li>Flip the image in both horizontal and vertical dimensions</li>

</ol>

Output the resulting images into three output image files, named: “Task1a.bmp”, “Task1b.bmp” and “Task1c.bmp”.

Task 2 (Luminance image)

Write a parallel OpenCL program to convert the RGB values (i.e. red, green and blue colour channels) of an image to luminance values (this approach is used to convert a colour image into a greyscale image).

For each pixel, calculate:

luminance = 0.299*R + 0.587*G + 0.114*B

Save the luminance image into a 24-bit BMP file. To do this, set all RGB values of each pixel to the luminance value (i.e. red = luminance, green = luminance, blue = luminance).

Name the output image file “Task2.bmp”.




Task 3 (Gaussian blurring)

Gaussian blurring is a commonly used technique to image processing and graphics to create a smooth blurring effect using a Gaussian function. The weights of the filter depend on the size of the Gaussian filter window. The following are weights for a 7×7 windows:




<table width="640">

 <tbody>

  <tr>

   <td width="96">0.000036</td>

   <td width="96">0.000363</td>

   <td width="96">0.001446</td>

   <td width="96">0.002291</td>

   <td width="96">0.001446</td>

   <td width="96">0.000363</td>

   <td width="64">0.000036</td>

  </tr>

  <tr>

   <td width="96">0.000363</td>

   <td width="96">0.003676</td>

   <td width="96">0.014662</td>

   <td width="96">0.023226</td>

   <td width="96">0.014662</td>

   <td width="96">0.003676</td>

   <td width="64">0.000363</td>

  </tr>

  <tr>

   <td width="96">0.001446</td>

   <td width="96">0.014662</td>

   <td width="96">0.058488</td>

   <td width="96">0.092651</td>

   <td width="96">0.058488</td>

   <td width="96">0.014662</td>

   <td width="64">0.001446</td>

  </tr>

  <tr>

   <td width="96">0.002291</td>

   <td width="96">0.023226</td>

   <td width="96">0.092651</td>

   <td width="96">0.146768</td>

   <td width="96">0.092651</td>

   <td width="96">0.023226</td>

   <td width="64">0.002291</td>

  </tr>

  <tr>

   <td width="96">0.001446</td>

   <td width="96">0.014662</td>

   <td width="96">0.058488</td>

   <td width="96">0.092651</td>

   <td width="96">0.058488</td>

   <td width="96">0.014662</td>

   <td width="64">0.001446</td>

  </tr>

  <tr>

   <td width="96">0.000363</td>

   <td width="96">0.003676</td>

   <td width="96">0.014662</td>

   <td width="96">0.023226</td>

   <td width="96">0.014662</td>

   <td width="96">0.003676</td>

   <td width="64">0.000363</td>

  </tr>

  <tr>

   <td width="96">0.000036</td>

   <td width="96">0.000363</td>

   <td width="96">0.001446</td>

   <td width="96">0.002291</td>

   <td width="96">0.001446</td>

   <td width="96">0.000363</td>

   <td width="64">0.000036</td>

  </tr>

 </tbody>

</table>




<ol>

 <li>a) Write an OpenCL program that accepts a colour image and outputs a filtered image using Gaussian blurring based on the 7×7 window weights provided above.</li>

</ol>

(3 marks) b) Instead of using the 7×7 window (the naïve approach), an alternate approach is to run the filter in 2 passes. The first pass will perform blurring in the horizontal direction; the result will then undergo a second pass to blur it in the vertical direction (enqueue the kernel twice to perform blurring in each direction). The result will be similar to the single pass approach (i.e. in Task3a), but the amount of computation will be different.

For example, using a 7×7 window approach, each pixel will have to perform a weighted sum on 49 pixels. In the 2-pass approach, each pixel will have to perform a weighted sum on 7 pixels in each pass, processing a total of 14 pixels. This is illustrated below:

7×7 window                            horizontal pass            vertical pass




Your task is to implement the parallel 2-pass approach. For this, use the following weights for the horizontal pass as well as the vertical pass:

0.00598             0.060626         0.241843         0.383103         0.241843         0.060626                            0.00598




<ol>

 <li> c) The parallel image processing examples provided in Lab 9 are 2-dimensional solutions (i.e. the kernel is enqueued in 2-dimensions). Write an OpenCL program to perform Gaussian blurring using the 7×7 window previously provided in Task 3a, as a 1-dimensional solution (i.e. enqueue the kernel in 1-dimension) where every pixel is processed by 1 work-item.</li>

 <li>d) Compare the performance of the Gaussian blurring kernels that you wrote in Task 3a, Task 3b and Task 3c by profiling the kernel execution times. You will not need an addition program for this part; simply add code for profiling kernel execution time into your previous programs.</li>

</ol>

Run the programs on the same device and plot a graph/bar chart to compare the kernel execution times of Task 3a, Task 3b and Task 3c. Clearly label your graph/bar chart and provide information about the device type (CPU or GPU) and the device name that you ran the programs on.

To get more reliable results, run the kernel at least 1000 times for each case and calculate the average time. Note that for Task 3b, the total kernel execution time is the time taken for both passes.

Submit your graph/bar chart as a pdf file named “Task3d.pdf”.

Task 4 (Bloom effect)

Bloom effects are commonly used in graphics, movies, video games, etc. This part combines the work from Task 2 and Task 3b. The basic steps to create an image with a bloom effect are illustrated as follows:




Figure 1: Bloom effect steps.

<ol>

 <li>The image in Fig. 1(a) shows the original image</li>

 <li>The image in Fig. 1(b) shows an image where the glowing pixels are kept, while the rest are set to black<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. For this assignment, allow the user to input a valid threshold luminance value. Pixels above the threshold luminance value are kept, while pixels below this luminance value are set to black. This step is related to Task 2.</li>

</ol>




<ol start="3">

 <li>The image in Fig. 1(b) undergoes a horizontal blur pass, then a vertical blur pass to obtain the image depicted in Fig. 1(c). This step is related to Task 3b.</li>

 <li>Finally, the pixel values in the images shown in Fig. 1(a) and Fig. 1(c) are added together to form the final image shown in Fig. 1(d). Note that values above the maximum value should be clamped to the maximum value.</li>

</ol>

Write a parallel program using OpenCL to perform the bloom effect on an input image. For the threshold value (in step 2), allow the user to enter a valid threshold value.

Your program should output the following images:

<ul>

 <li>“Task4a.bmp” – an image after step 2 (i.e. image showing the glowing pixels)</li>

 <li>“Task4b.bmp” – an image after the horizontal blur pass</li>

 <li>“Task4c.bmp” – an image after the vertical blur pass</li>

 <li>“Task4d.bmp” – the final image with the bloom effect</li>

</ul>

<a href="#_ftnref1" name="_ftn1">[1]</a> Note that Figure 1(b) shows a down-sampled image (i.e. the image has been shrunk). It is more efficient to process a down-sampled image during the blurring step because there are fewer pixels to process. This also works well for blurring since referencing the pixels from the smaller image in the final step will also cause blurring (blurring caused by effectively up-sampling back to the original image size. In fact, some approaches simply use down-sampling for blurring). To make things easier, for this assignment you do not have to perform down-sampling/up-sampling.