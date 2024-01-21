An introduction to key ideas and results in distributed algorithms.

We'll cover some of the following topics and ideas:  
- TIMING: synchronous, asynchronous, partially synchronous  
- COMMUNICATION: message passing, broadcast (reliable, causal, total), flooding … 
- FAILURES: fail-silent, fail-stop, fail-recovery, …  
- BUILDING BLOCKS: processes, events, failure detectors, logical clocks, quorums, … 
- ALGORITHMS: broadcast, consensus, leader election, …  
- PROPERTIES: safety, liveness, fairness, termination, …  
- CONSENSUS CASE STUDY: Raft  
- LIMITS: FLP impossibility proof,…
- Programming => Elixir - Modelling => $TLA+$

## Why a Distributed System?

- Distribution of computation and/or data resources  
- Performance (through parallelisation and localisation - *closer to data*)  
- Reliability and availability (through replication and redundancy, fault-tolerance, security)  
- Scalability, modularity, evolvability  
	- More nodes, more data etc. Can also introduce problems.
- Heterogeneity of hardware (CPU, networks, storage) and software (OS's, implementations, versions) 
- Legal/Business reasons
	- most have client-server designs with replication, geo-caching; could involve intellectual property or privacy laws. 

## But what is a Distributed System?

A set of processes connected by a (logical) network
- execute on machines (that can be located across the planet) 
- communicate via message passing, not shared memory
 No common physical clock
- no total order on events by time, events and actions in a system can happen simultaneously.

**Distributed Systems**
- Distributed systems are very much concerned with uncertainty – working in the presence of delays, failures, dynamic changes etc. Also attacks by adversaries, mobility of devices, energy management, …
- But what concepts, models, algorithms, reasoning and programming techniques/tools are needed?
- What guarantees do distributed systems aim to provide?
- How can they be designed, deployed, managed and evolved?

**Distributed Algorithms**

This course is about algorithms that are critical to the reliability of distributed systems. In particular, we’re interested in an algorithm’s:

1. Specification  
	- What's the problem? What are its safety and liveness properties?  
	- Ideally the specification should be formal and provide an abstract version of the algorithm.
2. Assumptions
		- What are the algorithm's timing, failure and communication assumptions?
3. Algorithm
	- What’s the solution (less abstract more refined version => implementation)? 
	- How do we know it works? Can we provide a proof?  
	- What is its complexity?
this concerns processes, messages, logical clocks etc.

**Specification**

Configuration: global state of algorithm, states of all processes and messages

Safety and Liveness: Assertions and what may and must happen (invariants on all states and invariants on a sequence of states)

Fairness: Infinitely often, all processes can execute an action.

Example: Mutual exclusion
- Safety: Two processes cannot access critical region simultaneously
- Liveness: Every request must eventually be granted
- Fairness: Different requests must be granted in the **order they are made**
![[Pasted image 20240116163649.png]]
This specification does not in fact work, as fairness cannot be guaranteed: **the orders would instead be processed in the order they are received** 
Failure to guarantee properties can lead to disastrous consequences; this is why we must rigorously prove and check properties; involving logic 

In practice it would be written more like this:
![[Pasted image 20240116163831.png]]
So first order logic is our tool of choice

Here is an example TLA+ specification:
![[Pasted image 20240116163919.png]]

**Assumptions - Timing**
Our algorithms are tailored around certain assumptions; systems are
- Asynchronous: 
	- steps and IPC can take arbitrary time
	- no assumptions of physical clocks, can be useful to have logical clocks
- Synchronous:
	- upper bounds on delays, easier to reason about
	- performance may be constrained by bounds
- Partially synchronous systems:
	- real world systems do both, assume system is eventually synchronous

**Failures**
- Process failures
	- Process stops sending messages it is supposed to or does not send expected messages
- Link failures:
	- IPC fail due to issues in communications, could lead to partitioned networks
- Process failures:
	- Process stops, fail-stop if crash can be reliably detected by other processes, fail-silent if not, fail-noisy if detection can take time, fail-recovery where crashed processes can recover
	- A process that is not faulty is called a CORRECT PROCESS
- Omission failure:
	- Process fails because it does not properly send all the messages required by the algorithm (send omission) or properly receive all the messages required (receive omission)
- Byzantine failure:
	- Process exhibits arbitrary, potentially malicious behaviour
**Assumptions - Communication**
Asynchronous message parsing:
- For the most part we assume a process does not wait on a response, continues running
- When required we will build synchronous message passing
Reliable message communication:
 - Assume reliable passing where no messages dropped, duplicated etc.
 - Communications can fail for other reasons, like network failure
 - TCP will be our protocol of choice
Message delays are bounded:
- Timeout if delayed too long

**Complexity Analysis**
- by the number of messages exchanged
- by the size of messages exchanged – when messages are big  
- by the time taken – asynchronous systems need an external observer, by the number of rounds in a synchronous system  
- by the memory used by a process n by the energy consumed