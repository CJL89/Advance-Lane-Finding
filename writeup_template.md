## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./examples/undistort_output.png "Undistorted"
[image2]: ./test_images/test1.jpg "Road Transformed"
[image3]: ./examples/binary_combo_example.jpg "Binary Example"
[image4]: ./examples/warped_straight_lines.jpg "Warp Example"
[image5]: ./examples/color_fit_lines.jpg "Fit Visual"
[image6]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

In order to calibrate the camera, I iterated through all the chessboard images that were provided in the repository and saved them in a variable called 'images'.
![Example of an image can be seen on line ` of the Jupyter Notebook.]

After that, I created the `nx`, `ny`, `objpoints`, `imagepoints` variables in order to save the results that I would have got from my function `find_points()` function. Thanks to this function, I could apply the `undistort_img()` functon that allows me to fix the distorted images found in the sample directory of chessboards; and creating and saving `mtx`, and `dst`.
![Example of undistorted image can be seen on line 9 of the Jupyter Notebook.]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

As I went throught the project I decided to write a function that included every single threshold and show and example as the function was created. At the end of it, I combined them all and applied to both the chessboard image selected and the `hightway_test1.jpg` image.
This process made me realize that spiltting my code in smaller sections was easier to debug since I didn't have to look through various lines. By the time I combined all the thresholded with HLS, the function worked smoothly; which surprised myself since most of the times something goes wrong along the way.
Examples of images can be found in the notebook as each function is created.

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

This portion of the project was more of copying and pasting from the lessons. I used the `img_size`:
```python
src = np.float32(
    [[(img_size[0] / 2) - 55, img_size[1] / 2 + 108],
    [((img_size[0] / 6) - 10), img_size[1]],
    [(img_size[0] * 5 / 6) + 60, img_size[1]],
    [(img_size[0] / 2 + 55), img_size[1] / 2 + 108]])
dst = np.float32(
    [[(img_size[0] / 4), 0],
    [(img_size[0] / 4), img_size[1]],
    [(img_size[0] * 3 / 4), img_size[1]],
    [(img_size[0] * 3 / 4), 0]])
```
I change the value to 108 since I found in the forums that it actually provided with better results.
After grabbing the sizes of the image and submitting the undistorted images through the combined threshold functions, I made the `birds_eye_view` function and passed the images.
[Example of the image can be found on line 37]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

After creating and passing image through the function `birds_eye_view`; this allowed me to make the histogram where the lanes are marked by the histogram. After that I passed the bird eye view image and previewed the image marking the lanes and finding the polynomial.
[Example of the image can be found on line 42]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

After finding the polynomial, I found the radius and curvature of the lane and the position of the vehicle. With this result, I was able to draw the shade on the lanes. The function `draw_shades` iterates through the images and returns the drawn shades on the images.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

All example images are found on the directory `output_images`.

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

The pipeline introduces all the functions that were created throught the entire project and then some checks on the different lanes that pass through the images. My initial pipeline was shorter and simpler, but was having traouble detecting the change of colors, shadows or reflections of the lanes so I had to find a way to be able to save the different results and go based on the previous findings rather than actually analizing each individual image by itself.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The toughest part was figuring out what was wrong with my pipeline. At the beginning I thought my thresholds were not good enough, but then I realized that my pipeline was analizing each frame individually rather than taking the result of the previous frame to help achieve a result. By not saving the results that my pipeline got, whenever the image presented a change of color on the road, shadows or reflections, the drawn shades will break.
This is when I started looking at the Lines class that was suggested in the classroom. Even though it was suggested, I believe it is a crucial part of the project to be able to get good results. I think in the classroom, there should be more time or one section that should be talked about implementing the class since it will help a lot.
