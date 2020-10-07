# Extract time from image of a clock
Function **extract_time** extracts the minutes and hours.<br />
Function **extract_time_bonus** extracts seconds if the needle exists, if not, outputs -1.

# IMPLEMENTATION
Input images are converted from RGB to grayscale, because knowledge of the color is redundant. Than the images are filtered with Gaussian filter in order to remove some artifacts and noise and to make gradients of the seconds needle smaller than the gradients of hours needle. 
The main problem is how to determine of all of the edges, extracted by [Canny edge detection algorithm](https://en.wikipedia.org/wiki/Canny_edge_detector)(you can cehck out my Matlab implemenation from scratch [here](https://en.wikipedia.org/wiki/Canny_edge_detector)), which edges belong to needles. To make this possible we will only observe the middle part of the images. That is the part where we possibly only have needles edges.
So the Canny edge detection algorithm is applied only on the cropped part that contains 25% of the dimensions around the middle pixel of the images:<br />
![img1](https://github.com/Digital-Image-Processing-kosta/Extract-time-from-image-of-a-clock/blob/master/garbage/1.png)<br />
Then the Hough transformation is applied on the extracted edges. The transformation is tuned so that it extracts 2 longest lines:<br />
![img1](https://github.com/Digital-Image-Processing-kosta/Extract-time-from-image-of-a-clock/blob/master/garbage/2.png)<br />
Now we have positions of the 2 needles, but we can not determine which one is for hours and which one is for minutes, because we maybe have cropped both needles and now they have the same length. The idea is to extract edges on the original image and to apply Hough transform to get some number (hyperparameter) of longest lines (edges). Then from the longest lines find 2 lines that match the lines extracted from the cropped part. The longer line will be hours needle and the shorter one will be minutes needle.

# TEST 
Run the **main.m** to test the functions on 12 images.

# NOTE
The hyperparameters in Hough's transformation are tuned for Matlab 2017.
