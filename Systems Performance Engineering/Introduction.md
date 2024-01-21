## According to Wikipedia: *Performance engineering encompasses the techniques applied during a systems development life cycle to ensure the non-functional requirements for performance will be met.*
## Measuring performance
How do you measure performance:
- We assume code is functional (ie no bugs)
- Beyond this, we usually look at execution speed
	- let's arbitrarily say end-to-end execution time

This is distinct from **high performance computing** as the focus is on:
- A **system** vs a **single problem**
- Not necessarily as simple as HPC (though the solutions might be complex)
- HPC might be supported by special purpose hardware (see [[Custom Computing Technologies]])

### As such, Systems Performance Engineering (SPE) is..
- Concerned with entire systems
- Made up of components that interact to achieve a greater goal
- Generic and apply to different problems or domains
- This differs to a well-designed application, which aims to be:
	- domain-agnostic
- Systems aim to be flexible at runtime

**A system should be widely applicable, handling conditions which are unknown at development time**
	for example, a data management system does not know the format or schema of a dataset.

**Systems are also**
- complex and developed over years
- need to be maintainable
- need to be fast

There is thus often the following trade-off:
![[Pasted image 20240116085242.png]]

**Example: finding instances of " FAUST:" in the book Faust**
- Initially, we can use OOP practices to write "clean code", which uses interfaces, cleanly parses, etc.
- By abandoning these practices in favour of doing everything inline, via a for loop and a\[i\] = "-" etc, we see significant speedups.
- We can further optimise by observing " FAUST:\\n" is 8 bytes, and thus we can convert this to a long and compare that way to go even better performance (long comparison is more CPU efficient as it can use specific instructions)

In essence:
![[Pasted image 20240116091332.png]]

**How do you stop optimising? How fast is fast enough?**
1. Define a target metric; this could be
	- throughput  
	- latency  
	- scalability  
	- memory usage  
	- energy consumption 
	- TCO
	- elasticity 
	- efficiency 
	- etc.
2. Decide when the requirements are met:
	- Easier option is to set a developer time budget
	- Harder option is setting an optimisation target/requirement/threshold
		- These are often referred to as **Quality of Service (QoS)** thresholds.
			- Defined as properties of a metric that will hold for the system
			- They can include preconditions
				For example: *The framerate of the game will, on average, be higher than 60 frames per second if run on a GPU with 50 GFlops or more.*
			- They can also conflict between functional requirements (eg tradeoff between AI realism and render speed)
**Why is setting an optimisation target hard?**
	It has to be agreed upon with a client; has to be achievable as you are committing to it

**Service Level Agreements (SLA)** are formal, legacy contracts which specify QoS objectives and consequences of violations. 
	Example: *Trading orders shall not exceed 1ms response time. In case of violation, the user is eligible for a 10% credit towards fees.*
These are non-functional, so enforcing them is quite difficult, particularly for a user. As such, SLAs be:
	**Specific** State exactly what is acceptable in numeric terms 
	**Measurable** Make sure what is stated can actually be measured
	**Acceptable** rigorous enough to guarantee success in reality 
	**Realisable** lenient enough to allow implementation
	**Thorough** ensure that all necessary aspect of the system are specified

**We will mainly focus on Measurable**

- Measuring  
	- Can be performed on actual system or prototype
	- Accurate if done properly
	- Can be costly and expensive
	- Based on instrumentation: intrusive techniques that modify binary (eg perf)
	- Monitoring:
		- Done constantly to enforce SLAs
		- Observe performance, collect stats, analyse data and report violations
		- can incur costs and is therefore seldom done continuously
		- ![[Pasted image 20240116093810.png]]
	- Benchmarking 
		- Get the system into a predefined or steady state
		- Perform operations while collecting relevant metrics
		- Example: *Database workloads usually have a data generator to make the system load a dataset (i.e., predefined state) and a query set (the series of operations).*
		- ![[Pasted image 20240116094043.png]]
		- Can be:
			- **Batch:** program given entire batch from the start, usually measure throughout, simpler as no generator
			- **Interactive:** Generator creates work piece by piece, often randomly. Metric usually is latency. Generator **must** be at least as fast as system under evaluation, as otherwise system has loophole to blag performance
			- Hybrids which generate queries from a work set is quite common
- Analytical Modeling
- Simulation 
- Hybrids:
	- measure, then model
	- model & simulate
	- …

**Interpreting results**
- Single datapoints are usually meaningless, too much noise (multiple users, cold cache etc)
- we usually want to aggregate runs and report variance (to give confidence to results or get an idea for performance characteristics)

**The optimising loop**
![[Pasted image 20240116094642.png]]

**How do we find these optimisation opportunities?**
Systems and workload characteristics that affect performance are **parameters**
There are two types:
- System - Those that are the same while the system runs (CPU instruction cost etc)
- Workload - Might change during execution (users, memory available etc)

**Resources will refer to Resource Parameters of underlying platform**
**There are numeric parameters, like CPU frequency and nominal parameters, like has a GPU**

**Utilisation** - The percentage of a specific resource being used to perform a service. 
- for CPU/memory, there is a total amount available, how much are we using?

**Bottlenecks** are resources with max utilisation eg CPU-bound, CPU is what is most utilised and needs to be optimised (new CPU or more efficient code)

**Note about bottlenecks** 
	Some factors that bound performance which are not strictly resources, such as **latency bound** which means we wait for system operation to complete.

**Identifying bottlenecks for an entire complex software system is practically infeasible**
	Instead we focus on the **performance-dominating** code path which we use to limit optimisation complexity
		- **Critical path** must be sequential (Amdahl's law)
		- **Hot path** is path which takes the most time

**Improving performance**
we want to:
	- Quickly compare alternative designs
	- Quickly select a close to optimal value for the platform parameter
	- This is known as parameter tuning
**Workload parameters are seldom under our control**
**System parameters can be**
**Parameter Tuning**
Finding the vector in the parameter space that either 
- minimises the resource consumption or
- maximises a performance metric
- Parameter tuning means exploring the parameter space 
- Even with (non-linear) optimisation, this is expensive  
- Analytical models accelerate that process immensely
![[Pasted image 20240116100319.png]]

**Trading off**
- Sometimes consumption of an expensive/non-scalable resource can be reduced by using more of a cheap one
- This often requires changes to the system  
	- new ram, cpu etc
- There can be good, bad and ambiguous trade-offs

**Analytical Performance Models**
Definition: *A formal characterisation of the relationship between system parameters (hardware, software, data) and performance metrics.*
- Often quite complicated
- can be modelled using static systems
	- these can be stateless (characteristic equations) or stateful (Markov chains)
- They are also very fast, simplify tuning via modelling system and workload parameters
- **If you can build an accurate analytical model, you truly understand the system**

**Example 1** *An I/O bound application, that needs to read 40MB per request is limited to 10 requests per second when running on a hardware platform with a single disk providing 400MB/s bandwidth.*
**Example 2** *A compute-bound application that needs 3 cycles to process one byte is limited to 20 requests per second if it needs to process 40MB per request and the CPU runs at 2.4GHz.*

**Simulation**
*A single observed run of a stateful model*
- These can be very difficult and expensive to calculate, this only gets worse as detail increases
- Holger rarely uses it as a result!