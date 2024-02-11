## Introduction
![[Pasted image 20240123160504.png]]
- We will assume that all nodes are logically connected; we will not concern ourselves with the physical network.
- Broadcasting includes oneself unless specified otherwise.
- Later sent messages may be delivered earlier -> no ordering

## Approach
![[Pasted image 20240123161349.png]]
- Algorithms will be presented as modular, composable algorithms using diagrams like the above; monolithic approaches or other compositions are also valid. Think of such a compositional approach as akin to OOP.
- Above: 
	- 3 Elixir Nodes, abstracted behind some network ("Distributed Process")
	- We will consider these as static and fixed during this course
	- Each green box is one Elixir process
## Assumptions
![[Pasted image 20240123161741.png]]
- We will move to a more hybrid system as we progress, for now everything is **Asynchronous**
- **Reliable** will in reality use TCP and is imperfect; the underlying abstraction should ideally recover but of course, TCP does not do this. A *send primitive* does do this though
## Classes of Broadcast
![[Pasted image 20240123162136.png]]
**This lecture focusses on ONE SHOT**
- As above, we don't care about ordering
- We acknowledge inefficiency here, but this is more to illustrate the concepts
## PL: Perfect point-to-point links
![[Pasted image 20240123162325.png]]
- **Deliver:** message has been received by the process expected to get it; does not mean it has actually been processed in any way -> will trigger a process
	- Postman has put the letter through the letterbox
- Safety can be thought of as an invariant: If expressed formally, could be shown via a Model Checker
- Liveness says something good will eventually happen. 
- A `:bind` may also be involved in the above, often abstracted away by the instance of `PL`.
## PL: Safety and Liveness Properties
![[Pasted image 20240123163055.png]]
- Correct: perfect process which runs as expected (no bugs etc)
	- Reliable Delivery is only point-to-point for now
- No Duplication: sequence ids may be used ie. no specific instance of a message is sent twice; destination will only see one version of this unique message
## BEB: Best effort broadcast
![[Pasted image 20240123163555.png]]
## BEB: Safety and Liveness
![[Pasted image 20240123163750.png]]
- BEB is sufficient if **every** process is reliable
	- "delivers it" -> the corresponding processes will be delivered to
## BEB: Basic broadcast (fail-silent)
![[Pasted image 20240123164329.png]]
**Scenario 1**
![[Pasted image 20240123164424.png]]
![[Pasted image 20240123164540.png]]
- If the sender crashes then it is broken; the correct processes should all have a correct view of the process; this is not something BEB provides
**Implementation**
![[Pasted image 20240123164646.png]]
**Summary**
![[Pasted image 20240123165039.png]]
## RB: Reliable broadcast
![[Pasted image 20240123165324.png]]
- Reliable broadcast essentially ensures a **consistent state** after every message is sent (via an agreement property). This is what makes RB more reliable.
## RB: Safety and Liveness
![[Pasted image 20240123165439.png]]
**Scenario 1**
![[Pasted image 20240123170629.png]]
**Scenario 2**
![[Pasted image 20240123170648.png]]
**Scenario 3**
![[Pasted image 20240123170742.png]]
## RB: Eager Reliable Broadcast (fail-silent)
![[Pasted image 20240123170906.png]]
- In order to ensure consistent state (no scenario 3) , there is re-broadcasting.
![[Pasted image 20240123171332.png]]
**Implementation**
![[Pasted image 20240123171602.png]]
- Maintaining `rb_delivered` is *very* expensive and suboptimal; ideally we want some GC mechanism. Very high memory overhead
**Summary**
![[Pasted image 20240123171930.png]]
## RB: Lazy Reliable broadcast (fail-stop)
![[Pasted image 20240123172049.png]]
- Failure detector via *heartbeat monitor* 
	- Send messages periodically and expect a return
	- This is done via the `PFD` process
	- Then, we only rebroadcast if there is a crash
	- Lazy in the sense we only rebroadcast if need be
**Implementation**
![[Pasted image 20240123172414.png]]
![[Pasted image 20240123172421.png]]
## Failure Detector
![[Pasted image 20240123172819.png]]
## PFD: Exclude on timeout
![[Pasted image 20240123173400.png]]
## Process configuration
![[Pasted image 20240123173452.png]]
## Distributed System configuration
![[Pasted image 20240123173526.png]]
**Reliable broadcast message delivery**
![[Pasted image 20240123173609.png]]
## PFD: Exclude on timeout
![[Pasted image 20240123173649.png]]
![[Pasted image 20240123173659.png]]
