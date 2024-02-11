![[Pasted image 20240205140228.png]]
- Homomorphism in the context of parametric circuit specification refers to the property where the mapping between input and output circuits preserves the structure and relationships, allowing the parametric specifications to be consistently transformed across different instances of the circuit.
![[Pasted image 20240205140910.png]]
![[Pasted image 20240205141809.png]]
- because $w$ is 1 bit, we can use `and` instead
- where is the inefficiency?
	- wires may have longer delay than adders
	- lots of wires and blocks; not doing very much
	- solution:
![[Pasted image 20240205142615.png]]
- wiring between the two columns can be optimised away
- do not need to "demultiplex" new range from the circuit; no need to add additional wiring at the end
- wiring is therefore optimised for composition
![[Pasted image 20240205143438.png]]
![[Pasted image 20240205143617.png]]
![[Pasted image 20240205144307.png]]
![[Pasted image 20240205144317.png]]
![[Pasted image 20240205144331.png]]
![[Pasted image 20240205144341.png]]
![[Pasted image 20240205144349.png]]
- Here there is delay equating to $3 * R$ which is inefficient
![[Pasted image 20240205145008.png]]
- Now only 2R
![[Pasted image 20240205145023.png]]
![[Pasted image 20240205145428.png]]
![[Pasted image 20240205145530.png]]
![[Pasted image 20240205145539.png]]
![[Pasted image 20240205145548.png]]
![[Pasted image 20240205145557.png]]
![[Pasted image 20240205151427.png]]