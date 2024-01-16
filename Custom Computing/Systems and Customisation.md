PREV [[Custom Computing Technologies]]

![[Pasted image 20240115202020.png]]
As we can see, there is a clear and significant tradeoff between dedicated HW and microprocessors.
![[Pasted image 20240115202141.png]]
As we can see here, the bulk of area is used for memory rather than computation. Thus there is a natural tradeoff between dealing with instructions and efficiency, with large memory aiding execution-time optimisations by exploiting locality at the expense of computational power.

By contrast, a **special purpose computer** is customised for a given function; no instructions are needed. If data can be streamed in or out, no cache is needed; only buffering, if at all.
- a chip customised for a specific application
- no instructions -> no instruction decode logic
- no branches -> no branch prediction
- explicit parallelism -> no out-of-order scheduling
- data streamed onto-chip -> no multi-level caches

Naturally, this will only work for some specific application, which in the general case is infeasibly costly as ASICs are expensive to design and fabricate. 

This is where **reconfigurable chips** like FPGAs fit in. They can be reprogrammed at runtime to cover different applications or versions of the same application. For example, if a communication channel becomes more noisy, one can adopt error correction techniques to enable effective operation under noisy conditions.

**Instruction processors** work by fetching instructions and data from memory and storing results back to memory. Frequent fetchers are a large overhead during execution,
![[Pasted image 20240115205954.png]]

Many applications can instead use **stream/dataflow processors** which can be arranged by streaming data through to functional blocks; which is both space and time efficient when done properly.
![[Pasted image 20240115210114.png]]

## In summary…
1. CPUs are good for non-repetitive, control intensive code.
2. FPGAs/ASICs are good for repetitive processing on large data volumes.
Thus both are often used, and used effectively together to overcome performance bottlenecks.

For example, Bing doubled throughput and reduced latency by 30% by using FPGAs to enhance performance. Costs were also reduced by 30% as server costs fell (could also be because nobody uses bing lol).
![[Pasted image 20240115210436.png]]

## How to program custom computing systems?
Programming FPGAs/ASICS involves hardware design, but a whole range of languages can be used:
- schematic entry of circuits
- hardware description languages
    - VHDL, Verilog, SystemC
- object-oriented languages
    - C/C++, Python, Java, and related languages
- dataflow languages: e.g. MaxJ, OpenSPL
- functional languages: e.g. Haskell, Ruby
- high level interface: e.g. Mathematica, MatLab
- schematic block diagram e.g. Simulink
- domain specific languages (DSLs)

![[Pasted image 20240115211717.png]]
Generally, we will identify parts of application that can benefit from acceleration, developing and improving the acceleration code until it meets the functional and performance goals

## Performance estimation
For the purposes of this course , we will focus on digital designs that:
- use clocked circuits
- disallow combinational loops:
	![[Pasted image 20240115212137.png]]
- gates have delay, speed limited by propagation delay through slowest path
	- this is usually carry path in adder
- clock rate is approx 1/(delay of slowest path)
- don't worry about transistors, logic gates are as granular as we go
### Techniques
- customisation opportunities
	- some data may remain constant: e.g. algebraic simplification - adopt effective data structures: e.g. number representation  
	- transform: e.g. enhance parallelism, pipelining, serialisation
- reuse possibilities (more next lecture)
    - description: repeating unit, parametrisation
	- transforms: patterns, laws, proofs
- example: polynomial evaluation for numbers ai, x y=a0 +a1 x+a2 x2 +a3 x3
- repeat many times with different values for ai and x
## A first polynomial evaluator
Compute: $y = a_{0}+ a_{1}x + a_{2}x^{2}+ a_{3}x^3$
Simplification, assume x is constant.
First attempt: 
![[Pasted image 20240115213918.png]]
This uses 3 adders and 3 multipliers; there are also repeating adders and multipliers

We can improve this by:
1. exploiting algebraic properties
2. enhancing parallelism
3. pipelining

Other opportunities that are not going to be covered include serialisation and custom data representation.
### 1. Algebraic Property
Horner's rule:
![[Pasted image 20240115214214.png]]
There are now fewer multipliers.

### 2. Enhance parallelism
We can shorten the critical path by converting to a balanced tree array of blocks:
![[Pasted image 20240115214355.png]]

## 3. Pipelining
We can add **pipeline registers** which allow for shorter cycle times, assembly-line parallelism and lower power; as data need not be propagated for as long. In below example, the critical delay path reduces from the sum of the delays of the three blocks to the max of the three.
![[Pasted image 20240115214754.png]]
It is important that all the paths from any input to any output are added the same number of registers. It would be preferable to balance the delay in different stages, and to preserve regularity when adding pipeline registers.

## In the next lecture we will look at a concise parametric representation of circuits, rather than verilog.
![[Pasted image 20240115215639.png]]

Here square brackets denotes independent parallel composition and semi-colon denotes series composition. $\Delta _{n}P$ denotes a triangular array of $P$ blocks with $n$ inputs and $n$ outputs, and $rdr_n$
R denotes a reduce array (similar to $Q^n$, a linear array of n blocks of Q, while R has two inputs while Q only has one input). If you have heard of “map- reduce”, this is essentially the “reduce” in “map-reduce”.