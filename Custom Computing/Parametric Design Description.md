PREV [[Systems and Customisation]]
## Motivation
FPGAs etc. offer customisation and reuse:
- some data may remain constant: e.g. algebraic simplification 
- adopt different data structures: e.g. number representation  
- transform: e.g. enhance parallelism, pipelining, serialisation
As such, we want a way of reprogramming them. We can describe repeating units and parameters, transformations using patterns; as well as optimising to improve efficiency, parallelism and regularity (allows to fit more tightly onto chip). Industrially, VHDL and Verilog are most used; for the course we will use Ruby: a declarative block description language we are using for it's simple and concise syntax for verification of designs and transformations.

## Ruby: what has this been used for?
NOT THE SAME AS RUBY ON RAILS!
- self-adaptive designs that change based on run-time conditions
    - collaboration with Airbus monitoring statistical and temporal situations
- signal processing architectures, especially regular ones
    - linear and non-linear filters, butterfly circuits for FFT, DCT
- non-numercial architectures airspeed sensor
	- sorters, DNA sequence matchers, priority queues, LRU designs
- arithmetic building blocks
    - add, multiply, divide, square root  
    - retarget for various number representations
- data parallel and data serial designs
    - derive implementations with different trade-offs
- Intel supports related work for next-generation hardware
- novel designs which are non-obvious but correct…

**Ruby is similar to FP languages like Haskell**
- Polymorphic types; higher order functions

Consider $f = inc\! where\! inc(x) = x+ 1$:
In Haskell:
```haskell
map inc
```
In Ruby:
![[Pasted image 20240117101934.png]]
For more complex data flows:
![[Pasted image 20240117101737.png]]
Ruby is RELATIONAL, not functional

![[Pasted image 20240117101843.png]]
**Note**: 
- Ruby is often using mathematical notation; Rebecca is a tool used to implement it.
- Also, \` will likely not be required (TODO: UPDATE)
- `.` is a delimiter between statements

**Composite data**
![[Pasted image 20240117102645.png]]
- No type checking at compile time (boo!)
- Inputs and Outputs **are not** Domain and Range; this is what allows for counter-flowing data!
- Rational behind direction convention: If you rotate 45 degrees clockwise, domain on top, range on bottom

**Wire block: polymorphic**
![[Pasted image 20240117103348.png]]
Polymorphism and the fork abstraction allow for one input and multiple outputs; any line may be a bus.
Fork can be used in multiple ways; input could go in multiple ways, behaviour defined by what it connects to.
$**wire** is just a convenient way of defining wiring blocks – you could always use $**rel**, as long as you use VAR to make explicit the input data.

**Series Composition**
![[Pasted image 20240117103740.png]]
There exists is similar to Haskell `where`

**Parallel Composition**
![[Pasted image 20240117103930.png]]
Independent operations of two blocks without communication between them
Law: parallel composition of series composition $\xleftrightarrow$ series composition of parallel composition

**Useful Abbreviations**
![[Pasted image 20240117104240.png]]

https://www.doc.ic.ac.uk/~wl/teachlocal/cuscomp/reprim.html
**Example**
![[Pasted image 20240117104823.png]]

