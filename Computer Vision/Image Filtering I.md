PREV [[Image Formation]]

Images are comprised of RGB(A) which assemble a colour image.
Greyscale images have one channel, easier to deal with
## Moving Average
Moves a window across the image, takes an average of the values to help identify trends in data.
Example:
	![[Pasted image 20240115104100.png]]
There is an issue of padding above; this is circumvented using Padding by:
- selecting a constant value
- mirroring values
Larger window size leads to blurrier image.
**Complexity**:
	Image size: $N * N$
	Kernel Size: $K * K$
	$K^2$ multiplications then $K^2 -1$ summations for $N^2$ pixels
	Hence $O(N^2 * K^2)$
	*Can we do better?*
		parallelism also possible but we are concerned first with algorithmic improvements

## Separable Filter
A big filter can be separated into two smaller filters:
![[Pasted image 20240118090542.png]]
Which in practice gives:
![[Pasted image 20240118090642.png]]

Consider: 
![[Pasted image 20240118090658.png]]
Then:
![[Pasted image 20240118090708.png]]

**Complexity**:
	Image size: $N * N$
	Kernel Size: $K * 1$
	$2*K$ multiplications then $2 * (K -1)$ summations for $N^2$ pixels
	Hence $O(N^2 * K)$
This can be quite significant for larger K.

**In conclusion, moving average filters**
- Remove high-frequency signal (reduce noise)
- Produce smooth but blurrier image as a result

## Types of Filters
- Identity filter
- Low-pass or smoothing filters 
	- Moving average filter  
	- Gaussian filter
- High-pass or sharpening filters 
- Denoising filters
	- Median filter 
- …
## Identity Filter
![[Pasted image 20240118091225.png]]
Useful later on…
## Gaussian Filter
![[Pasted image 20240118091258.png]]
The 2D Gaussian filter is a separable filter, equivalent to two 1D Gaussian filters with the same 𝜎, one along x-axis and the other along y-axis.
![[Pasted image 20240118091403.png]]
Output:
![[Pasted image 20240118091458.png]]

## Low-pass Vs High-pass
- The moving average filter and Gaussian filter smooth or blur the image, keeping the low-frequency signals.
- They are called low-pass filters or smoothing filters.
    
- There are some filters that sharpen the image and highlight the high-frequency signals.
- They are called high-pass filters or sharpening filters.

## High Frequency Signals
![[Pasted image 20240118091644.png]]
We have Identity, which is high frequency; and uniform matrix, which is low filter: by using them, we have a high-pass filter.

## Median Filter
- non-linear: no longer calculating some weighted sum
- used for denoising
- Done by moving sliding window, taking median from all values in current window
![[Pasted image 20240118091958.png]]
For padding, just use smaller window (eg by having default `inf` padding rather than zero padding).

![[Pasted image 20240118092200.png]]
In the above, a linear filter would not work as well; by contrast the median filter successfully removes noise by ignoring it to some extent.

**There are other denoising filters, such as non-local means or block-matching filters, but we won't look at these for now**

**Filters are used by:**
- Photographers and designers
- Denoising on camera
- Zoom/Teams filters

### Covered:
- Low-pass filters  
	- Moving average filter
	- Gaussian filter  
	- Separable filtering
- High-pass filters
- Denoising filters 
	- Median filter

NEXT [[Image Filtering II]]
