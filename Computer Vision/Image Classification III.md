## Convolutional Neural Networks
![[Pasted image 20240226095408.png]]
### Limitations with MLP
![[Pasted image 20240226095430.png]]
- When we look at images, we look at each local region, process information and combine the local information from different regions to form a global understanding.
- In CNNs, each neuron only see a small local region in the layer before it. The region it sees is called the receptive field. 
- The local connectivity substantially reduces the number of parameters.
![[Pasted image 20240226095517.png]]
![[Pasted image 20240226095538.png]]
![[Pasted image 20240226095600.png]]
- takes a small subset of prev layer as opposed to the whole thing
![[Pasted image 20240226095712.png]]
![[Pasted image 20240226095740.png]]
![[Pasted image 20240226095811.png]]
![[Pasted image 20240226095929.png]]
![[Pasted image 20240226095959.png]]
- Some operations during convolution 
	- Padding 
	- Stride 
	- Dilation
### Padding
![[Pasted image 20240226101109.png]]
![[Pasted image 20240226101120.png]]
### Stride
![[Pasted image 20240226101149.png]]
![[Pasted image 20240226101202.png]]
![[Pasted image 20240226101212.png]]
### Dilation
![[Pasted image 20240226101241.png]]
![[Pasted image 20240226101318.png]]
![[Pasted image 20240226101328.png]]
## Pooling
![[Pasted image 20240226101438.png]]
![[Pasted image 20240226101506.png]]
### Max Pooling
![[Pasted image 20240226101520.png]]
#### Example of a Basic CNN
![[Pasted image 20240226101548.png]]
![[Pasted image 20240226101628.png]]
#### Deep Neural Networks
![[Pasted image 20240226101741.png]]
![[Pasted image 20240226101750.png]]
#### Improvement in Optimisation
![[Pasted image 20240226101945.png]]
![[Pasted image 20240226102001.png]]
![[Pasted image 20240226102029.png]]
![[Pasted image 20240226102117.png]]
![[Pasted image 20240226102136.png]]
- as $z \to 1/ 0, f(z)f(1 - z) \to 0$ 
![[Pasted image 20240226102531.png]]
![[Pasted image 20240226102542.png]]
![[Pasted image 20240226102557.png]]
