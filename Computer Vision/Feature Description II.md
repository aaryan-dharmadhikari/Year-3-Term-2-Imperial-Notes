*We will now look at feature descriptors relevant to SIFT and extending the idea of describing features for interest points to features for images*
## SIFT
![[Pasted image 20240205091725.png]]
### Strategies for Faster Feature Description
![[Pasted image 20240205091830.png]]
![[Pasted image 20240205091857.png]]
![[Pasted image 20240205091910.png]]
## Speeded-up Robust Features (SURF)
![[Pasted image 20240205092227.png]]
![[Pasted image 20240205092401.png]]
![[Pasted image 20240205092547.png]]
![[Pasted image 20240205092555.png]]
- Exploit the fact that it is very quick to calculate $dx$ and $dy$ to increase speed
![[Pasted image 20240205092706.png]]
- Take sum of $dx$ and $dy$ and absolute values; these are then our descriptors
![[Pasted image 20240205093301.png]]
- these 4 values can be used as above to demonstrate how they are valuable
![[Pasted image 20240205093523.png]]
### Can We Do even Better
![[Pasted image 20240205093606.png]]
![[Pasted image 20240205093612.png]]
## Binary Robust Independent Elementary Features (BRIEF)
![[Pasted image 20240205093909.png]]
![[Pasted image 20240205094106.png]]
![[Pasted image 20240205094521.png]]
![[Pasted image 20240205094606.png]]
- This is used ACROSS images to compare distances
![[Pasted image 20240205095008.png]]
![[Pasted image 20240205095204.png]]![[Pasted image 20240205095208.png]]
### Other Strategies for Speedup
![[Pasted image 20240205095216.png]]
### Feature Descriptor
![[Pasted image 20240205100525.png]]
## Histograms of Oriented Gradient (HOG)
![[Pasted image 20240205100659.png]]
![[Pasted image 20240205100830.png]]
![[Pasted image 20240205100956.png]]
![[Pasted image 20240205101035.png]]
## Image Classification
![[Pasted image 20240205101253.png]]

