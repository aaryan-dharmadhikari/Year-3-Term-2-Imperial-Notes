- What are interest points?  
- Why are we interested in them? 
- How do we detect them?
## Interest Points
![[Pasted image 20240129090153.png]]
## Facial Analysis
![[Pasted image 20240129090208.png]]
- though the face is the same, there is other information informing which character is being shown
- These are interest points
![[Pasted image 20240129090250.png]]
- each of these "landmarks" can be considered interest points
- The locations can be used to analyse mood, identification
## Image Matching
![[Pasted image 20240129090429.png]]
- Can we infer that these are the same?
- Could match all pixels
	- ![[Pasted image 20240129090556.png]]
	- We can match similarity after some transformation; after formulating this loss function we can optimise with regards to this function.
- Maybe we can extract landmarks (interest points) to find that they match
	- **Much** cheaper than matching pixels
	- ![[Pasted image 20240129090657.png]]
	- Using matching up interest points we may try to find a transformation between them
	- ![[Pasted image 20240129090853.png]]
		- Panorama mode is reliant on interest points in order to stitch multiple pictures together and form the desired panorama image
		- ![[Pasted image 20240129090934.png]]
## Interest Point Detection
![[Pasted image 20240129091053.png]]
## Harris Detector
![[Pasted image 20240129091115.png]]
![[Pasted image 20240129091258.png]]
- Challenge with edges is hard to map because many pixels; by contrast identifying corners is much easier
- [[Edge Detection]] can be used to find corners
## Finding Corners
![[Pasted image 20240129091617.png]]
![[Pasted image 20240129091629.png]]
- We look for changes in intensity along both directions
![[Pasted image 20240129091742.png]]
![[Pasted image 20240129092120.png]]
- Ignore higher order terms, rearrange taylor expansion to get the approximation shown
- TODO: Expand
	- Fewer Intensity matrix calculations (?)
![[Pasted image 20240129092848.png]]
![[Pasted image 20240129092904.png]]
**What if it's not diagonal?**
![[Pasted image 20240129093330.png]]
![[Pasted image 20240129093513.png]]
![[Pasted image 20240129093943.png]]
- We can interpret eigenvalues to infer the intensity change
![[Pasted image 20240129094139.png]]
- For flat/edge, response is low; for large $\lambda_{1}\lambda_2$ response is large
![[Pasted image 20240129094301.png]]
- Alternatives, follow same characteristics
## Cornerness
![[Pasted image 20240129094402.png]]
## Algorithm
![[Pasted image 20240129094851.png]]
**Detection**
![[Pasted image 20240129100212.png]]
- Things other than corners also illicit changes
![[Pasted image 20240129100553.png]]
- Is invariant to rotation
![[Pasted image 20240129100615.png]]
- Is not invariant to scaling; will not detect as well
**Summary**
- Harris detector finds interest points with strong responses at corners and blobs.
- It calculates the change of intensities within a small window when it is shifted in the image, which can be quantified by local image derivatives.
- It is rotation-invariant, but not scale-invariant.