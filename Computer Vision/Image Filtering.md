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
**Complexity**:
	Image size: $N * N$
	Kernel Size: $K * K$
	$K^2$ multiplications then $K^2 -1$ summations for $N^2$ pixels
	Hence $O(N^2 * K^2)$
	*Can we do better?*
	