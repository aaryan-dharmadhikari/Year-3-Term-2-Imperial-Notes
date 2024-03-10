Image segmentation is a process of partitioning an image into multiple regions, each region consisting of pixels with similar properties (e.g. intensity, colour, texture) or semantics (e.g. person, car, building).
![[Pasted image 20240226104503.png]]
![[Pasted image 20240226104515.png]]
![[Pasted image 20240226104523.png]]
![[Pasted image 20240226104539.png]]
- No training involved, simply splits intensities into two clusters and separates
## K-means Clustering
![[Pasted image 20240226104623.png]]
![[Pasted image 20240226104640.png]]
![[Pasted image 20240226104657.png]]
![[Pasted image 20240226104705.png]]
![[Pasted image 20240226104805.png]]
![[Pasted image 20240226104835.png]]
### Gaussian Mixture Models
![[Pasted image 20240226104943.png]]
![[Pasted image 20240226105026.png]]
![[Pasted image 20240226105130.png]]
- $\pi_k$​: The sum of the membership probabilities for cluster �k divided by the total number of data points.
- $\mu_k$​: The weighted sum of the data points, with the weights being the membership probabilities, divided by the sum of the membership probabilities.
- $\sigma^2_k$​: The weighted sum of the squared differences between the data points and the mean of cluster $k$, divided by the sum of the membership probabilities.
![[Pasted image 20240226110528.png]]
![[Pasted image 20240226110757.png]]
## CNNs
![[Pasted image 20240226111003.png]]
![[Pasted image 20240226111011.png]]
![[Pasted image 20240226111024.png]]
![[Pasted image 20240226111410.png]]
- recall that fully connected layers are better suited to classification; as opposed to convolutional layers which are favoured for feature extraction.
![[Pasted image 20240226111434.png]]
![[Pasted image 20240226111712.png]]
### Upsampling
![[Pasted image 20240226111733.png]]
![[Pasted image 20240226111811.png]]
![[Pasted image 20240226111822.png]]
![[Pasted image 20240226111839.png]]
![[Pasted image 20240226111850.png]]
![[Pasted image 20240226112035.png]]
![[Pasted image 20240226112053.png]]
![[Pasted image 20240226112106.png]]
![[Pasted image 20240226112126.png]]
