## Systolic Arrays
![[Pasted image 20240129150644.png]]
- **Systolic Arrays** are characterised by an efficient architecture facilitating data streaming, like blood pumping through organs in our body. They have been used in many high performance applications such as the Google TPU, and they suit FPGA technology which offers effective implementation based on the array of fine- grained and coarse-grained computing resources. Although systolic arrays have many benefits, their systematic and parametric development and verification have been challenging.
- **Two challenges.** First, can we verify such designs parametrically? Second, how efficient is this design, and are there better ones? Can we quantify the design trade-offs? Initial designs verified via simulations; this is not exhaustive.
## Simple Systolic Matrix Multiplier
![[Pasted image 20240129151548.png]]
### Example
![[Pasted image 20240129151708.png]]
![[Pasted image 20240129151842.png]]
![[Pasted image 20240129151850.png]]
![[Pasted image 20240129151905.png]]
…
![[Pasted image 20240129151919.png]]
- results stored in 9 processors $P$
## Bit level systolic designs
![[Pasted image 20240129152015.png]]
- The above shows a bit-level systolic correlator
## Sequential Design
![[Pasted image 20240129152110.png]]
- To support systolic array development, we need to extend Ruby to cover sequential designs. The extension is simple: instead of relating variables with the appropriate shape in the domain and range, a sequential Ruby design relates a time sequence of such variables in the domain to one in its range. It is important to note that, for an adder, its domain is a single time sequence of pairs, rather than a pair of time sequences.
- Combinational designs would just relate the corresponding elements in the domain and range at a given cycle. A delay D is a component that models state: the range is one cycle behind the domain. If input is in the domain and output in the range, then D describes an implementable register or a D latch or flipflop. However if input is in the range and the output in the domain, then D models an anti-register that predicts its output, so cannot be implemented. Rebecca can simulate both registers and anti-registers, and those initialised to a given value.
## Pipelining
![[Pasted image 20240129153607.png]]
## Graphical Method
![[Pasted image 20240129153720.png]]
### Pipelining a chain
![[Pasted image 20240129154801.png]]
- Pipelining a chain is straightforward – it can be seen as a special case of Horner’s Rule. Note that the anti-delays are introduced at the boundary so this is fine.
### Pipelining a row
![[Pasted image 20240129154832.png]]
- Involves triangular arrays; a bit more involved
#### Example: polynomial evaluation and optimisation
![[Pasted image 20240129154926.png]]
- Use horner's rule
	- ![[Pasted image 20240129154955.png]]
### Pipelining a grid
![[Pasted image 20240129155023.png]]
**How can we pipeline this???**
![[Pasted image 20240129155040.png]]
- We could do it like this; depends on how much we want to be able to undo:
![[Pasted image 20240129155111.png]]
- The above is fully pipelined
![[Pasted image 20240129155126.png]]
- Could also fully pipeline via diagonal contours; this is less cluttered than above
