![[Pasted image 20240219140311.png]]
![[Pasted image 20240219141332.png]]
![[Pasted image 20240219141342.png]]
![[Pasted image 20240219141347.png]]
![[Pasted image 20240219141355.png]]
![[Pasted image 20240219141401.png]]
![[Pasted image 20240219141407.png]]
![[Pasted image 20240219141419.png]]
![[Pasted image 20240219141430.png]]
![[Pasted image 20240219141435.png]]
![[Pasted image 20240219141440.png]]
![[Pasted image 20240219141452.png]]
![[Pasted image 20240219141458.png]]
## ASAP
![[Pasted image 20240219141513.png]]
![[Pasted image 20240219141520.png]]
![[Pasted image 20240219141525.png]]
![[Pasted image 20240219141531.png]]
![[Pasted image 20240219141536.png]]
## ALAP
![[Pasted image 20240219141547.png]]
![[Pasted image 20240219141552.png]]
![[Pasted image 20240219141605.png]]
![[Pasted image 20240219141912.png]]
![[Pasted image 20240219141929.png]]
- ASAP needed one cycle of buffering to schedule the graph, but ALAP requires none. Since there’s only a finite number of buffers on the FPGA, this is a better result.
### What if we add another output?
![[Pasted image 20240219142040.png]]
 ![[Pasted image 20240219142151.png]]
 - neither can schedule optimally.
- Note that in this design, the optimal schedule would have no buffering, since we can push the buffer next to output zero into the output (stream inputs and outputs are implemented by memory buffers, so this is equivalent to changing the address to that buffer by a small, constant amount).
![[Pasted image 20240219142400.png]]
![[Pasted image 20240219142414.png]]
- positive latency for positive offsets
![[Pasted image 20240219142445.png]]
![[Pasted image 20240219142754.png]]
![[Pasted image 20240219142844.png]]
![[Pasted image 20240219142952.png]]
![[Pasted image 20240219142958.png]]
![[Pasted image 20240219143006.png]]
Ruby pipelining does resemble Maxeler scheduling, with some differences: 
- Ruby is top-down (add parameters to design description), whereas Maxeler is bottom-up (annotate the graph after building). 
- Ruby operators are all combinatorial, whereas Maxeler operators are pipelined by default (see Maxeler loop lecture). 
- Ruby allows you to control pipelining using clustering, whereas Maxeler uses another API call (see loop lecture).
