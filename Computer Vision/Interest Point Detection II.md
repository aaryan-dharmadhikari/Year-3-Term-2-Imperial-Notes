PREV [[Interest Point Detection I]]

Recall Harris Detector is not invariant to scale
**How do we deal with this?**
![[Pasted image 20240129100831.png]]
**By convolving using different $\sigma$**
![[Pasted image 20240129101001.png]]
**We may notice different features**
![[Pasted image 20240129101017.png]]
![[Pasted image 20240129101152.png]]
- We want to account for corners found at all scales
	- As $\sigma$ increases, so does our interest in the corner; response higher at larger scales -> more pronounced with larger $\sigma$
- However, direct comparison may not be fair
![[Pasted image 20240129101421.png]]
- **NOTE: There is an inverse relationship between $I$ and $\sigma$**
![[Pasted image 20240129101510.png]]
![[Pasted image 20240129101612.png]]
- Note the differing widths for the same shape
- For larger $\sigma$ wider curve
![[Pasted image 20240129101716.png]]
![[Pasted image 20240129101821.png]]
![[Pasted image 20240129102029.png]]
- After multiplying, the magnitudes are comparable
## Scale Adapted Harris Detector
![[Pasted image 20240129102221.png]]
- we will now normalise so all results are comparable
![[Pasted image 20240129102319.png]]
- For each element we multiply by $\sigma^2$ 
- We can now form some comparison across all $\sigma_i$ 
## Scale-space Extrema
![[Pasted image 20240129102524.png]]
## Algorithm
![[Pasted image 20240129102703.png]]
### Interest point Detectors
![[Pasted image 20240129102941.png]]
#### Laplacian Filter
![[Pasted image 20240129103003.png]]
- We can just use the given filter
- why?
- ![[Pasted image 20240129103117.png]]
	- we approximate $x + \Delta x$ to be equal to $x$ and normalise to 1; giving us the weights $\vec{1, -2, 1}$  
- ![[Pasted image 20240129103310.png]]
**This gives us something like**
![[Pasted image 20240129103345.png]]
- Second derivative is even more sensitive to noise than the first
- ![[Pasted image 20240129103435.png]]
- Note how much more pronounced signals are
- This can be reduced using **gaussian smoothing**
- ![[Pasted image 20240129103511.png]]
#### Laplacian of Gaussian
![[Pasted image 20240129103533.png]]
- We first smooth to make signals clearer
![[Pasted image 20240129103728.png]]
![[Pasted image 20240129104119.png]]
- aka the mexican hat, mexican hat wavelet, Marr's wavelet, np.sombrero (ok not the last one)
![[Pasted image 20240129104242.png]]
- Using $\sigma^2$ as before allows for results to be comparable
![[Pasted image 20240129104417.png]]
- Same idea for finding maximum across x, y, scale
##### Difference of Gaussian
![[Pasted image 20240129104806.png]]
![[Pasted image 20240129124144.png]]
#### DoG Vs LoG
- DoG is a good approximation to the normalised Laplacian of Gaussian (LoG).
- It provides some convenience in calculating the response across different scales.
![[Pasted image 20240129124213.png]]
#### Interest Point Detectors
![[Pasted image 20240129124236.png]]
![[Pasted image 20240129124243.png]]

NEXT [[Feature Description I]]