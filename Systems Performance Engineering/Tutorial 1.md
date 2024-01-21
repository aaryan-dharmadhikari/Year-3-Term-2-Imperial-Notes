TUTORIAL FOR [[Systems Performance Engineering/Introduction|Introduction]]

**What is a QoS objective? What are its properties? How is it different from an SLA?**
SLA sets hard constraints on performance, failure to deliver etc.
**SMART**
- Specific
- Measurable
- Acceptable
- Realisable
- Thorough

**What is utilization? Why is utilization important in practice?**
For a specific resource, % used.
Used to identify bottlenecks

**Why does PE-Grep outperform regular grep?** 
8 byte string, can utilise CPU instructions to reduce no instructions.
We have sacrificed generality for performance.
ALU probably 64 bit, underutilising resources.

**Where is the line between performance engineering and high-performance computing?**
HPC sacrifices generality and maintainability for performance; SPE does not look to do that.

**What modelling approaches exist, what are their advantages/disadvantages?**
Analytical:
	Stateful
	Stateless:
		- Usually this one; equation to describe relationship b/w parameters
		- Save time, solve model to get best possible answer easily
Numerical:
	Not very useful, seldom used

**What is the Performance Engineering Tradeoff-triangle? Provide an example!**
Eg variables instead of explicit constants -> less maintainable and general but avoids lookup table interaction so faster

**Provide three examples for Performance Engineering target metrics**
- throughput  
- latency  
- scalability  
- memory usage  
- energy consumption 
- TCO - Total cost of operation
- elasticity - *the degree to which a system is able to adapt to workload changes by provisioning and de-provisioning resources in an autonomic manner, such that at each point in time the available resources match the current demand as closely as possible*
- efficiency

**Compare Monitoring and Benchmarking?**
Monitoring constant, benchmarking infrequent
Monitoring should be less intrusive which may be challenging
Benchmarking involves finding parameters that represent the workload which may be a challenge. For example data production, throughput >= real workload.

**What kinds of performance-relevant parameters exist?**
**2 axes**:
- Workload (fixed at execution, **can be changed by you**) and System (**outside of your control**)
- Numerical and Nominal

**What is the difference between hot and critical path? why are both important?**
Critical: Must be sequential
Hot: Path that takes the most time

**Illustrate the requirements of SMART (Specific, Measurable, Acceptable, Realizable & Thorough)**
- Specific: 
	- Executes an operation in 2 seconds - specific
	- Executes an operation fast - unspecific
- Measurable
	- Fast enough for a user - subjective, not really measurable
- Acceptable
- Realisable
- Thorough