## Types of Descriptors
We want a way of detecting points of interest in images; how do we identify and describe these points to ultimately compare and match images?
There are various types of descriptors:
- Simple descriptors 
	- Pixel intensity
	- Patch intensities  
	- Gradient orientation 
	- Histogram
- SIFT descriptor
### Pixel Intensity
![[Pasted image 20240205004223.png]]
- eg imagine the same landmark being photographed at different times of day; hard to see the parallels
### Patch Intensities
![[Pasted image 20240205004320.png]]
- Fixes the fact that we don't consider pixel relative to other pixels
- Still not that good with rotation; intensity
### Gradient Orientation
![[Pasted image 20240205004545.png]]
- Intensity still an issue; separates orientation and magnitude but is not rotation invariant
### Histogram
![[Pasted image 20240205005208.png]]
- By changing the way data is represented we can find representations that are robust to both rotation and scaling by instead representing grids in a way that identifies trends in values across pixels
- Still sensitive to intensity changes
### The Problems
We want something that is:
- Gradient orientation  
	- Robust to change of absolute intensity values
- Histogram  
	- Robust to rotation and scaling
- Can we combine the advantages of the two? 
	- Yes.
	- Scale-invariant feature transform (SIFT) algorithm.
### Scale-invariant Feature Transformation (SIFT)
![[Pasted image 20240205010110.png]]
#### Detection of Scale-space Extrema
![[Pasted image 20240205010147.png]]
![[Pasted image 20240205010212.png]]
![[Pasted image 20240205010224.png]]
![[Pasted image 20240205010722.png]]
![[Pasted image 20240205010733.png]]
#### Keypoint Localisation
![[Pasted image 20240205011150.png]]
![[Pasted image 20240205011219.png]]
![[Pasted image 20240205011229.png]]
![[Pasted image 20240205011340.png]]
![[Pasted image 20240205011415.png]]
![[Pasted image 20240205011438.png]]
![[Pasted image 20240205011446.png]]
#### Local Image Descriptor
![[Pasted image 20240205011517.png]]
![[Pasted image 20240205011557.png]]
![[Pasted image 20240205011603.png]]
![[Pasted image 20240205011612.png]]
#### Scale and Rotation Invariance
![[Pasted image 20240205011632.png]]
![[Pasted image 20240205011638.png]]
##### Orientation Assignment
![[Pasted image 20240205011722.png]]
![[Pasted image 20240205011853.png]]
![[Pasted image 20240205011915.png]]
**In summary**
![[Pasted image 20240205012059.png]]
## Keypoint Matching
![[Pasted image 20240205012136.png]]
![[Pasted image 20240205012144.png]]
![[Pasted image 20240205012207.png]]
![[Pasted image 20240205090622.png]]
### Outliers in Matching
![[Pasted image 20240205090649.png]]
![[Pasted image 20240205090811.png]]
![[Pasted image 20240205090820.png]]
![[Pasted image 20240205090837.png]]
![[Pasted image 20240205090844.png]]
![[Pasted image 20240205090850.png]]
![[Pasted image 20240205090857.png]]
### The RANSAC Algorithm
![[Pasted image 20240205090941.png]]
- Random algorithm so needs some arbitrary termination
![[Pasted image 20240205091104.png]]
**ASIDE**: RANSAC vs LSM: LSM is sensitive to outliers, not really suitable for actual IO of inputs as there are seldom images etc with no noise
#### Applications of Keypoint Matching
![[Pasted image 20240205091447.png]]
![[Pasted image 20240205091506.png]]
## Summary
- Feature descriptors extract local features near an interest point.
- Robustness to rotation, scaling and intensity changes are the three main aspects to consider in designing feature descriptors.
- We have introduced the SIFT descriptor.
- It can be used for matching keypoints between two images and estimating the spatial correspondence.