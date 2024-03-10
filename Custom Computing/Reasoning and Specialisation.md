PREV [[Grid components and Combinators]]
![[Pasted image 20240129140934.png]]
![[Pasted image 20240129141110.png]]
- Useful for proving behaviour for reuse; important for library design
- **POINTWISE FOR BASE CASE, POINTFREE FOR INDUCTIVE**
## Pointwise Proof
![[Pasted image 20240129141709.png]]
- easier to use pointwise (ie involving a, b, c, d)
## Algebraic laws for row and column
![[Pasted image 20240129142101.png]]
![[Pasted image 20240129142701.png]]
![[Pasted image 20240129142830.png]]
### Showing row identity shown above
![[Pasted image 20240129142931.png]]
- Note that the justification of transforming one expression to the next is enclosed by curly brackets “{“ and “}”. This is a style of proof due to Edsger W. Dijkstra, a renowned computer scientist
![[Pasted image 20240129143421.png]]
![[Pasted image 20240129143739.png]]
![[Pasted image 20240129144028.png]]
#### Formally:
![[Pasted image 20240129144150.png]]
### Specialisation of row and column
![[Pasted image 20240129144412.png]]
### Inner product reduction
![[Pasted image 20240129144700.png]]
![[Pasted image 20240129144740.png]]
- Wayne used this in practice to speedup a process from $O(N)$ to $O(\log{N})$
NEXT [[Sequential Design and Pipelining]]