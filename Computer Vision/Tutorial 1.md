Linked to [[Image Filtering I]]

## Q1
### Example 1
![[Pasted image 20240118101251.png]]
$x' = x + 10$: So there is a 10 pixel shift
$y' = y + 5$: So there is a 5 pixel shift right
### Example 2
![[Pasted image 20240118101433.png]]
$x' = 5x$: So there is a x5 increase in value.
$y' = 5y$: So there is a x5 increase in value.

## Q2
### 2.1
In order to store RGB, we require: 
- 1 byte per colour
- 3 colours -> 3 bytes
- 3 x 1280 x 960 contiguous bytes; if non-contiguous, additional metadata would be required.
### 2.2
Red converts to: 0.299 x 255
Blue converts to: 0.587 x 255
Green converts to: 0.114 x 255
## Q3
Y can be modelled as $Y_i \sim N(I, \sigma ^2)$
Since $Y = I + n$, we can get that $μ_n = 0$ and $\sigma _n ^{2} = \frac{1}{N}\sigma ^2$
## Q4
Let us assume that less then half of region's pixels are salt and pepper pixels. Let us assume salt is 0 and pepper is 255 for 8 bit colours. Then, for a 3 x 3 matrix, we know that salt and pepper pixels represent <= 4 pixels.
As such, salt and pepper pixels will never be in the fifth position and will therefore never be taken as median. 
## Q5
Let us find:
	$a * b$ for some arbitrary $n$:
		$\sum_{m=- \infty}^{\infty}a[m]b[n-m]$






