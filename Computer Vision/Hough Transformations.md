PREV [[Edge Detection]]
![[Pasted image 20240125090410.png]]

![[Pasted image 20240125090417.png]]

![[Pasted image 20240125090427.png]]
## Basically GCSE Maths
![[Pasted image 20240125090437.png]]

![[Pasted image 20240125090447.png]]

![[Pasted image 20240125090454.png]]
- Uses double intercept form 
## Hough Transform
![[Pasted image 20240125090606.png]]
## Model Fitting
![[Pasted image 20240125090710.png]]
- Similar to LSM
## Hough Transform
![[Pasted image 20240125090816.png]]
![[Pasted image 20240125090933.png]]
- in $m$, All 3 lines intersect at $m = 1$ and $b = 0$; this gives us parameters to use. 
	- If two points with same no of votes, the output of the transform may be more than 1 line.

![[Pasted image 20240125091430.png]]
![[Pasted image 20240125091452.png]]
- we know from normal form we can express any 2D line; $\theta$ is bounded by $[0, \pi)$
**For the same space:**
![[Pasted image 20240125091638.png]]
## Line Detection by Hough Transformation
![[Pasted image 20240125091759.png]]
- Pretty expensive operation as all possible lines to be explored per edge point
![[Pasted image 20240125092220.png]]
![[Pasted image 20240125092237.png]]
![[Pasted image 20240125092432.png]]
![[Pasted image 20240125092441.png]]
![[Pasted image 20240125092456.png]]
![[Pasted image 20240125092708.png]]
![[Pasted image 20240125092848.png]]
By fixing $r$, we can reduce set of solutions so the answer is not ridiculously slow; not doing this keeps 3 free variables to explore.
![[Pasted image 20240125092912.png]]
![[Pasted image 20240125093417.png]]
![[Pasted image 20240125093423.png]]
![[Pasted image 20240125093430.png]]
![[Pasted image 20240125093440.png]]
![[Pasted image 20240125093504.png]]
![[Pasted image 20240125093513.png]]
![[Pasted image 20240125093527.png]]

INCOMPLETE

NEXT [[Interest Point Detection I]]