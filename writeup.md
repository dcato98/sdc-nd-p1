# Udacity Self-Driving Car Nanodegree
## Project #1: Finding Lane Lines on the Road 

The goals of this project are to:
* Make a pipeline that finds lane lines on the road (P1-Submission1.ipynb)
* Reflect on the project (this file)


[//]: # (Image References)


[image0]: ./test_images_output/test_img1-0_original.jpg
[image1]: ./test_images_output/test_img1-1_gray.jpg "Grayscale"
[image2]: ./test_images_output/test_img1-2_blur_gray.jpg "Blurred"
[image3]: ./test_images_output/test_img1-3_edges.jpg "Canny Edges"
[image4]: ./test_images_output/test_img1-4_masked_edges.jpg "Masked Edges"
[image5]: ./test_images_output/test_img1-5_lines.jpg "Hough Lines"
[image6]: ./test_images_output/test_img1-6_images-with-lines.jpg "Final Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. First, I converted the images to grayscale (image 1) and applied a Gaussian blur (image 2).

![Grayscale][image1]
Image 1. Grayscale (above) and Image 2. Gaussian Blur (below)
![Gaussian Blur][image2]

Next, I passed the image through a Canny edge detection algorithm (image 3), tweaking the parameters until satisfied that the edges of the lanes were being detected while minimizing the number of extraneous lines. After this, I masked the image to include only a the lower part of the image where lanes are expected in order to further reduce the extraneous lines (image 4). 
![Canny Edges][image3]
Image 3. Canny Edges (above) and Image 4. Masked Edges (below)
![Masked Edges][image4]

In order to draw a single line on the left and right lanes, I modified the `weighted_hough_lane_lines` function by filtering out lines with a slope less than a tuned value and then using slope to separate left and right lane lines. Next, I weighted each line by its length before averaging the lines to reduce the impact of small lines that were still occasionally incorrectly detected (image 5). Finally, I overlaid these lines on top of the original image to produce the final result (image 6).
![Hough Lines][image5]
Image 5. Hough Lines (above) and Image 6. Final Output (below)
![Final Output][image6]

### 2. Identify potential shortcomings with your current pipeline


One shortcoming is when lane lines are missing or vision is obstructed, for example, on a gravel road or during a heavy rain. In this case, the current pipeline is likely either to detect incorrect lane lines or not to detect any lanes at all.

Another shortcoming is when there are more than two lanes (e.g. when attempting to cross lanes or cross an intersection). The pipeline is currently hardcoded to only detect a single line to the left and a single line to the right.

A third shortcoming is that the pipeline is currently not proven to work under conditions are significantly different from the test cases. Given the wide variety of real-life road conditions (e.g. poor lighting, low line contrast with the road, etc...), the current pipeline is not likely to handle all of these well. 


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to detect all lane lines for crossing lanes, not just the lines in the current lane. To do this, I would need to collect test data that includes crossing lanes and modify the `process_lane_lines` function to detect and return more than two lines.

Another potential improvement could be to average lane lines over multiple frames. This would better handle the conditions when lane lines are temporarily either not detected or are falsely detected. I would implement this by declaring a global variable to store the existing average lines. In `process_lane_lines` I would add the current lane lines detected to the existing average multiplied by a decay rate (which would have to be tuned to determine an appropriate value). This approach is likely to smooth out small errors that cause the detected lines to occasionally jump around. 

A third improvement would be to simply collect more data under a wide variety of conditions. Actively testing the pipeline on more data would enable more confident estimates of the pipeline's robustness. Collecting more data would also be useful for testing other approaches for identifying lane lines, such as by applying deep learning techniques.
