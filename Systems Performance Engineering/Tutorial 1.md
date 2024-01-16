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