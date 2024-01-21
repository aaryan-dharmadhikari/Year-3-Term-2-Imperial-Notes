## How to describe filtering mathematically?
- Signal processing  
- Filtering as convolution

**For 1D**
![[Pasted image 20240118093223.png]]
Or more abstractly:
![[Pasted image 20240118093256.png]]

## Filtering in signal processing
- Filtering is studies in depth in signal processing, which:
	- removes unwanted components/features from signals
	- enhances desired features
It can be done on a hardware device (eg guitar pedals) or via software (eg Garageband)

**Filtering can be done on arbitrary dimension signals; from audio to radar imagery**

## Impulse response
Impulse response is the output of a filter when the input is an impulse signal

For continuous signal, an impulse is a Dirac delta function $\delta (x)$
![[Pasted image 20240118093752.png]]

For discrete signal, it is a Kronecker delta function $\delta[x]$
![[Pasted image 20240118093835.png]]
![[Pasted image 20240118093901.png]]
- The impulse response h completely characterises a linear time-invariant filter. As along as we know h, we can calculate the output signal 𝑦, given any input 𝑥.
- In signal processing, we often denote a filter by its impulse response function h.

## Time-invariant system
Most filters we've seen thus far are **linear** and **time-invariant**.
![[Pasted image 20240118094051.png]]
![[Pasted image 20240118094059.png]]
As we can see; if the same input is given at different times, the output is the same, just offset by time elapsed. *Time elapsed offset must be the same; time offsets MUST be propagated from input to output.*
- $g[n] = 10 * f[n]$ is time-invariant, amplifies input by constant step
- $g[n] = n * f[n]$ is not time-invariant, output depends on time step n

## Linear systems
![[Pasted image 20240118094619.png]]
- if a system is linear, combining two inputs linearly leads to the output being combined linearly:
	$output(\alpha f_{1[n]} + \beta f_{2[n]}= \alpha g_{1[n]} + \beta g_2[n] )$
## Linear, time-invariant systems
- Theory and methods are well developed for linear time-invariant systems.
- For such systems, impulse response $h$ completely characterises how the system works.
- Thus given input $f$ and impulse response $h$; we can derive $g$ and define this output as a *convolution.*