![[Pasted image 20240207100437.png]]
- Currently Ruby higher order functions such as map and row only cover homogeneous designs – all the blocks are identical. We can define higher order functions to capture the same design pattern, but with heterogeneous blocks. Most laws for homogeneous designs can be extended to cover heterogeneous designs.
![[Pasted image 20240207100900.png]]
- introduce a loop construct to help reason about state machines
![[Pasted image 20240207101030.png]]
- $P \cdot  Q^{n} \cdot R (x) = (P \cdot  Q \cdot R)^n (x)$ ???
![[Pasted image 20240207101732.png]]
![[Pasted image 20240207102233.png]]
- `loop [Q, R] = Q; R` 
![[Pasted image 20240207103518.png]]
![[Pasted image 20240207103538.png]]
![[Pasted image 20240207103822.png]]
![[Pasted image 20240207104220.png]]
![[Pasted image 20240207104226.png]]
![[Pasted image 20240207104350.png]]
