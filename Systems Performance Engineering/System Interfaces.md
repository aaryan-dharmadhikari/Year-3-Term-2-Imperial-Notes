![[Pasted image 20240220123843.png]]
![[Pasted image 20240220124133.png]]
## Keys to an efficient system
![[Pasted image 20240220124147.png]]
![[Pasted image 20240220124511.png]]
![[Pasted image 20240220124749.png]]
TODO to 37
## IO interfaces
###  Device as memory
![[Pasted image 20240227111324.png]]
![[Pasted image 20240227111450.png]]
- Device may not actually exist at this address in memory, instead may just be pointing to some data structure
- Writing to memory is recording head and tail location; akin to registers in CPU
- Device operates independently to work asynchronously
- If the queue is full, CPU stops outputting
- Obviously unbuffered IO works differently
- Buffer size is device dependent
![[Pasted image 20240227112651.png]]
![[Pasted image 20240227112615.png]]
#### Multi-homed Devices
![[Pasted image 20240227113238.png]]
- everything always looks local, assign addresses in memory to some PCIe ports
- Performant but extremely expensive
### Device and driver models
![[Pasted image 20240227113619.png]]
- Polling: CPU checks if device is ready and available and ready for a read/write. Expensive on CPU, which has to wait and check on status, wasting cycles
	- Unbuffered is different: IO is directly executed without buffering
- Interrupt Driven: Uses the circular buffer detailed above
- Hybrid involves speculating on polling latency over time and dynamically switching
![[Pasted image 20240227114016.png]]
- Cache Coherency -> Read Only state -> Update to memory invalidates cache -> CPU now aware of new state
![[Pasted image 20240227114424.png]]
- Trapping is the intercepting and handling of MMIO to ensure isolation, security and management.
- Passthrough: essentially moving some of the hypervisor logic into the device; this is naturally more complicated but reduces overhead from Hypervisor emulation by allowing direct communication between VM and 
![[Pasted image 20240227120809.png]]
![[Pasted image 20240227121027.png]]
- Use more threads!
	- Thousands of threads: lots of overhead, no bueno
	- Cannot control scheduler
![[Pasted image 20240227121137.png]]
![[Pasted image 20240227121430.png]]
![[Pasted image 20240227121516.png]]
![[Pasted image 20240227121811.png]]
**Blocking Handling**:
- When a thread is blocked in an event-driven system, it typically relinquishes control back to the event loop or scheduler. This allows other tasks or events to be processed while waiting for the conditions to change and the blocking situation to be resolved.
- The event loop or scheduler continues to process other events and tasks, ensuring that the system remains responsive and can make progress despite individual threads being blocked.
![[Pasted image 20240227122457.png]]
- Contention for any and all tasks; this is quite stupid, especially in a multicore setting
![[Pasted image 20240227122629.png]]
- IO in usermode, kernel bypass
![[Pasted image 20240227123108.png]]
- communication between user and kernel