 ![[Pasted image 20240131100508.png]]
 - More throughput; lower latency
![[Pasted image 20240131100523.png]]
- Block distributes $x_t$ with triangular registers to skew successive copies to produce $x_{t-1}$ and a left reduction of `mac` blocks (*multiply-accumulation* defined in slide) will multiply $<x_{t}, w_{i}>$  and sum the results
![[Pasted image 20240131101115.png]]
- Triangular arrays introduce inefficiency, introduction of anti-delays cancel them
- Bottom is optimised; removing no of delay registers:
	- This removes delay registers and the antidelay registers $w_{t} \leftrightarrow w_{t-k}$  and $w_{t} \leftrightarrow w_{t-k}$ 
- The path at the bottom has the longest propagation path; is the hot path. As such, we want to optimise by increasing throughput here:
![[Pasted image 20240131102419.png]]
- This does the same thing as the previous one, but we exploit commutativity and associativity to optimise design.
- We now instead form pairs $<x_{t-3}, w_3>, <x_{t-2}, w_2>,$...
![[Pasted image 20240131102600.png]]
- We can then simplify this; however there is now delay in input $w$; We could then obtain Cu2, a row of CuCell2 with delays at the bottom between the mac blocks. So the cycle time of Cu2 would just be $T_{mult} + T_{add}$ – ignoring wiring delays especially those at the top. It would, however, be useful to develop a design without the long wiring delay at the top.
- The longest path is now that with $x$ and $w_0$ 
	- Longest path is path from either $output \rightarrow input$ or the longest wire path
- Better than Cu1 due to shorter cycle time
![[Pasted image 20240131103342.png]]
- Why 2 registers? 
	- 2 stages of delay on top?
![[Pasted image 20240131103819.png]]
![[Pasted image 20240131103827.png]]
- Can we introduce just the right degree of pipelining for a design?
![[Pasted image 20240131103902.png]]
- R is timeless: $R = R \backslash D$
![[Pasted image 20240131104501.png]]
- Goes from single list to list of lists; quite tricky
![[Pasted image 20240131104533.png]]
- No delay at the bottom so it is not fully pipelined
- We can fix this with clustering
![[Pasted image 20240131105122.png]]
- Which is better?
![[Pasted image 20240131105718.png]]
- think of sampler as a filter: take every 3rd value for example
- Lets introduce slowdown, a way to introduce delays or anti-delays. All one needs to do is to replace a delay or an anti-delay by n copies in series.
- An interesting observation is that an n-slow design can process n interleaved data streams concurrently – we can think of the function of the sampler is to extract one of them, to relate an n-slow design to the corresponding design without slowdown.
![[Pasted image 20240131105937.png]]
![[Pasted image 20240131110133.png]]
- Apply $slow_2$ and use horner's rule to simplify.