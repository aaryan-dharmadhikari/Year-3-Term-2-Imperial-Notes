## VGG
![[Pasted image 20240228164020.png]]
![[Pasted image 20240228164050.png]]
- The way I think of this is every $3 * 3$ convolution will lead to one layer lost (assuming zero padding); this then gives us $-3$ on all sides, equivalent to one $7 * 7$ pass
- Each $3 * 3$ layer has 9 nodes, so we have 27 nodes vs 49
![[Pasted image 20240228164947.png]]
- VGG is able to outperform AlexNet, this is largely explained by:
	- Additional Depth: Captures more diverse and complex patterns
	- Nonlinear Combination: More Activation Functions, more complex exploration
	- This leads to better performance, more expressive network, can better discriminate between features
![[Pasted image 20240228165406.png]]
## ResNet
![[Pasted image 20240228165424.png]]

![[Pasted image 20240228165438.png]]
- In a residual unit, the input to a convolutional layer is added to the output of that layer, rather than being passed through the layer directly.
- This means that the network is learning the residual, or the difference between the input and output of the layer, rather than the complete transformation from input to output.
![[Pasted image 20240228165516.png]]
![[Pasted image 20240228165548.png]]
- So the introduction of Residual units means that, by virtue of both the input and output being retained, it is easier to reach earlier layers with less attenuation; this means gradients may carry useful information about loss to earlier layers, rather than being reliant on previous layers for information, which is susceptible to the 'vanishing gradient' problem seen earlier
![[Pasted image 20240228171218.png]]
![[Pasted image 20240228171235.png]]
## Vision Transformers
![[Pasted image 20240228171343.png]]
![[Pasted image 20240228171354.png]]
![[Pasted image 20240228171447.png]]
![[Pasted image 20240228171502.png]]
![[Pasted image 20240228171547.png]]
![[Pasted image 20240228171621.png]]
![[Pasted image 20240228171627.png]]
![[Pasted image 20240228171811.png]]
![[Pasted image 20240228171852.png]]
![[Pasted image 20240228171909.png]]
- In other words, dot product between query (part of the input that model wants to attend on) and key (another part of the input sequence against which the query is compared) - this allows use to quantify similarity between query and key. We then use softmax to get attention weights
- The value is the information associated with the key - by multiplying by weight we get a metric for how much focus should be given to each value in the input sequence
![[Pasted image 20240228171950.png]]
![[Pasted image 20240228172839.png]]
![[Pasted image 20240228172848.png]]
![[Pasted image 20240228172857.png]]
![[Pasted image 20240228172910.png]]
![[Pasted image 20240228173051.png]]
![[Pasted image 20240228173041.png]]
## Self-supervised Learning
![[Pasted image 20240228173112.png]]
![[Pasted image 20240228173138.png]]
### Contrastive Learning
![[Pasted image 20240228173214.png]]
- Augment and try to make sure the outputs are as close as possible
- Also find an example of a different sample, try and minimise agreement
### Masked Auto-encoding
![[Pasted image 20240228173403.png]]
![[Pasted image 20240228173426.png]]
![[Pasted image 20240228173436.png]]
![[Pasted image 20240228173446.png]]
![[Pasted image 20240228173542.png]]
![[Pasted image 20240228174003.png]]
