PREV [[Image Filtering II]]

- What is an edge?
	- Lines where brightness changes sharply, allowing us to notice discontinuities.
	- Happens due to:
		- change of colour
		- change of depth
		- surface normal discontinuity: sudden change or break in the orientation of surface normals within an image 

Expected output of edge detection:
![[Pasted image 20240122093726.png]]

**Why do we care?**
- Edges capture important properties of what we see
- They are important to finding and features for analysing images
- They provide low level features for both animal and computer vision for analysing and understanding images.

**How do we detect edges**
- An image can be interpreted as a function of pixel position; as such we can use derivatives to characterise the discontinuities of a function, helping us characterise edges
![[Pasted image 20240122094451.png]]
![[Pasted image 20240122094531.png]]
![[Pasted image 20240122100243.png]]
![[Pasted image 20240122100508.png]]
We can use the above trick to implement central difference via convolution 

![[Pasted image 20240122100640.png]]
From the large differences, we can denote discontinuties
![[Pasted image 20240122100812.png]]
## How about 2D

We want to find difference between neighbouring pixels; these are naturally a bit more involved
## Prewitt Filter
![[Pasted image 20240122101009.png]]
Essentially same as 1D filter; requires us to pick a direction.
Remember that we flip, so the above is in fact left to right; this is just by definition, it should not matter too much which direction we go with.

We usually use 3x3 due to the $[1, 0, -1]$ seen before

This can be separated:
![[Pasted image 20240122101050.png]]

## Sobel Filter
![[Pasted image 20240122101413.png]]
This filter oversamples the central x or y axis line

It can also be separated:
![[Pasted image 20240122101500.png]]

**Example**
![[Pasted image 20240122101614.png]]

At each pixel, we have 2 outputs form x and y Sobel filters; this is called the gradient:
![[Pasted image 20240122101834.png]]
We can describe for the gradient the magnitude and orientation.
![[Pasted image 20240122101912.png]]

**Why do filters have a moving average?**
- Derivatives are sensitive to noise
- Moving average or another smoothing kernel can be used to suppress noise
- We could, for example, also use a Gaussian kernel to smooth the signal.
Consider:
**Derivative with no smoothing**
![[Pasted image 20240122102400.png]]
**Derivative with Gaussian smoothing**
![[Pasted image 20240122102455.png]]
![[Pasted image 20240122102541.png]]

**We want to know generate a binary map from a gradient magnitude map**
**Canny Edge detection:**
- Perform Gaussian filtering to suppress noise.
- Calculate the gradient magnitude and direction.  
- Apply non-maximum suppression (NMS) to get a single response for each edge. 
- Perform hysteresis thresholding to find potential edges.

**Non-maximum suppression**
- Aim: To get a single response for an edge.
- Idea: Edge occurs where the gradient magnitude reaches the maximum.
- Let us zoom in a small region of the magnitude map in the next slide.
![[Pasted image 20240122103056.png]]
We can see which pixel has max value on a given gradient and set the max to be 1 and all others to be 0 (or equiv). This then leads to well defined edges.
![[Pasted image 20240122103322.png]]

This gives us results like:
![[Pasted image 20240122103337.png]]

**Thresholding**
In NMS:
- Some pixels may be local maxima but they may have low magnitudes.
- We want detect edges that have high magnitudes. To this end, we perform thresholding.
We want to convert magnitude map into binary map;
As such we either have: 
![[Pasted image 20240122103801.png]]

**Hysteresis thresholding**
![[Pasted image 20240122103828.png]]
Or graphically:
![[Pasted image 20240122103855.png]]
Then, after inspecting the neighbouring pixels:
![[Pasted image 20240122104007.png]]

This gives us output like:
![[Pasted image 20240122104029.png]]
**Effect of changing $\sigma$**
![[Pasted image 20240122104042.png]]
As we can see, for lower $\sigma$ there is less smoothing and hence more well defined edges. 
- Large $\sigma$ detects large scale-edges
- Small $\sigma$ detects finer features

**The merits of Canny Edge detection**
It carefully designs the image features (e.g. Gaussian filtering + image gradient) and procedures (e.g. NMS, hysteresis thresholding) for the task.
- Gaussian smoothing to suppress noise.  
- Gradient orientation and non-maximum suppression to find the exact location of edges.  
- Hysteresis thresholding to find weak edges.

## Learning-based Edge Detection
![[Pasted image 20240122104657.png]]
Results using approach shown in above image:
![[Pasted image 20240122104726.png]]
There are many other approaches, eg using CNNs

## Summary
- Edge detection is a fundamental problem in image processing and computer vision.
- Edges provide important low-level features, where discontinuities occur, for both human vision system and computer vision for analysing and understanding images.
- In the following lectures, we will explore other types of features, such as describing the features for interest points, for regions or for an entire image.

NEXT [[Hough Transformations]]