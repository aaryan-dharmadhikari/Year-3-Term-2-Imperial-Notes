## Tools
### Correctness Tools
![[Pasted image 20240304222254.png]]
![[Pasted image 20240304222340.png]]
![[Pasted image 20240304222356.png]]
![[Pasted image 20240304222411.png]]
### Performance tools
![[Pasted image 20240304222425.png]]
![[Pasted image 20240304222439.png]]
![[Pasted image 20240304222451.png]]
## Patterns
![[Pasted image 20240304223634.png]]
![[Pasted image 20240304223658.png]]
![[Pasted image 20240304223739.png]]
![[Pasted image 20240304223758.png]]

## Algorithms
### Non-blocking algorithms
![[Pasted image 20240220122206.png]]
- Very complicated to write effective non-blocking with atomic RMW without locking
#### Lock-free: stack
![[Pasted image 20240220122340.png]]
- if it is current, current points to current->next
- `push`
	-  `CAS(&head, node->next, node)` attempts to set the head of the stack to the new node. It only succeeds if the head of the stack is still the same as `node->next`. If another thread has modified the head of the stack in the meantime, the CAS operation will fail, and the loop will retry the operation.
- `pop`
	- `CAS(&head, current, current->next)` attempts to set the head of the stack to the next node. It only succeeds if the head of the stack is still the same as `current`. If another thread has modified the head of the stack in the meantime, the CAS operation will fail, and the loop will retry the operation.
	- If the CAS operation succeeds, the loop breaks and the function returns the popped node.
#### The ABA problem
![[Pasted image 20240220122842.png]]
