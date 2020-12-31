# **Finding Lane Lines on the Road** 

## Writeup 

## Alexis Canizares, December 2020

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

### Initial Setup

Additional helper functions were created to facilitate development and testing of the pipeline. Two functions were added to load all the images in the source folder. The function `load_images_from_folder` loads all the images in a folder and send them to an array of images. The function `load_images_from_folder_w_name` loads the same into a dictionary. This allows to load the filenames as well into the dictionary and use them to save processed files into an output directory. Files in the output directory are named with a postfix of '<_processed>'.

Loading all the files allows me to show all the images while processing steps in the pipeline. All images are displayed vertically.

### Pipeline Steps

The pipeline has seven steps described below. All parameters used in the pipeline functions and code lineas are in one block which makes easy to apply different values to see the results. In summary the help functions provided have been used and optimzied with different parameters. I was not able to use the hough_lines function so I made some changes in that step.

1. Grayscale

Images are converted into grayscale.

2. Gaussian Smoothing & Bluring, the helper function is used with the default value of kernel_size = 5, which seems to perform best.

3. Apply Canny to the blurred picture to find the edges. The parameters are 50 for low and 150 for high thresholds. This produces an images with the edges.
    
4. Mask image to a region of interest. A four vertices shape is used to mask the area of interest. Using the helper function produced a shape with parameters:
'<vertices = np.array([[(0,imshape[0]),(450, 330), (550, 330), (imshape[1],imshape[0])]], dtype=np.int32)>'

5. For the Hough lines I applied the same techniques on the lesson to run the lines in the edge dtected image in step 4.  The parameters are rho = 1, theta = np.pi/180, threshold = 14, min_line_length = 40, max_line_gap = 20.

![lines](test_images_output/solidWhiteRight_lines.jpg?raw=true "Lines"")

6. Draw straight lines using averages. I created a new function called '<draw_lines_average>' exaplined in the next section that produces extended lines from the maximum points in the area of interest. The parameters that worked better were thickness of 10. The color was left to [255, 0, 0].

7. Finally ythe oupput lines are combined with the original images into one to produce the intended output.

![processed](test_images_output/solidWhiteCurve_processed.jpg?raw=true "Straighg Lines"")

### Single lines 

To draw the single lines I created a new function calle draw_lines average. This function draws the lines on to a mutated image. 

1. Defined arrays to hold values for m, b, lines for each side, left and right.

2. Left and right lines are separated using the sign of the calculated slope, m. If negative goes to left, if positive goes to right.

3. Calculate averages for the slope m and the value of b. Then we have averages m and b for each side.

4. Calculate the end points from the passed image. This calculates x, y for each point for the left and righ '<x1_left, x1_right, y1_left, x2_left, y2_left, y1_right, x2_right, y2_right>'

5. Finally, use cv2.line to combine the lines in the original image.

### 2. Identify potential shortcomings with your current pipeline

1. When building applying to the video there were some instances of NaN values being passed to the line builder which causes the process to break. I modified the '<mean>' function to avoid problems. However when applying just as a try on the challenge video I see many instances of the NaN values passed. Some work could be done here to handle this situation.

### 3. Suggest possible improvements to your pipeline

1. The pipeline can be improved perhaps adding some clean up for outliers when building the vertical lines. I am not sure there is much difference however. I tried a few techniques but did not see much difference in the end result. Maybe it can be applied and optimzied when other videos are used.

2. Another point would be to re build the straight lines continously to adpt to changed in the view. The function I created initially does that, but it loads on the function because if builds a line for every point that exist. I commented out this for performance so lines are built only once.

Thanks


