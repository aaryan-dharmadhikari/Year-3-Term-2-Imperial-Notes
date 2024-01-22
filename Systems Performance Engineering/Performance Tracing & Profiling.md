PREV [[Systems Performance Engineering/Introduction|Introduction]]
## Motivation
![[Pasted image 20240120171752.png]]

After evaluating performance and concluding it is insufficient, we must identify optimisation opportunities; that is where **profiling** comes in. After identifying the hot path (code that takes most time) we need to identify where the bottle neck is: CPU, Memory etc. Both are functions of system behaviour.
## What is system behaviour?
**Events**
- any change in system state
- Simple/atomic events
	- clock has ticked
	- instruction executed
	- package sent
- Complex events
	- cache line evicted from L1 to L2
	- instruction aborted due to misspeculation
	- Evants have an optional payload
- Eventas have an accuracy: its payload/attributes may be numeric and thus subject to measurement

**What can you do with events**
- Event sources have:
	- Generator: observes changes of the system state
	- Comnsumer:
They can be:
- online: while process is running
- offline: dumps events without analysing

## Tracing
- Definition: A complete log of every state the system has ever been in (in the period of interest)
	- Comprised of events
	- Events are ordered (usually totally ordered)
- Accuracy is "inherited" from the events 
- Event collection overhead may be high

**Example: call stack tracing**
![[Pasted image 20240120172519.png]]
Stack going top to bottom with addresses on it

More abtstractly:
![[Pasted image 20240120172554.png]]
Bottom to top 
From EBP and their return addresses we can turn the call stack into something we can use for debugging: using debug symbols we can resolve the "variables and stuff"

**Problems**
- Recording the entrie callstack is quite expensive
	- Stack needs to be walked and pointers chased
	- Callstacks can be deep
	- Frame pointers resolved during tracing need to be written to memory
- As a resulllt, small function processing can be more expensive than the function
this is known as:
**Perturbation**
*The degree to which the performance of a system changes when it is being analyzed.*
- negatively affects accuracy if it is non-determinisitc
- just by looking at the ysstem, you influence state

**reducing perturbation**
- reduce fidelity; or *exactness to which something is copied or reprocued*
- in doing so, not every event is recorded and thus we can reduce perturbation
## How do we do that
**Sampling**
- do not collect all events
- either:
	- Sample in regukar intervals
	- sample randomly
- Idea: Skip some events
	- chance some functions wont be sampled
	- Fortunately, more expensive functinos will likely be sampled more often, since they are bigger
- However:
	- less perturbation
	- good performance: low impact
*clear tradeoff between perturbation/performance and fidelity*

**Sampling intervals**
- Time based
	- Most common
	- Set a recurrent timer and sample whenever it runs out
	- We use CPU reference cycles as a proxy metric
	- Unfortunately, computer clocks are not well defined and synchronisation is unreliable so data can be inaccurate, non-determinsitc and noisy
	- But easy to interpret due to clear inverse relationship between performance and time
- Event based
	- Generalisation of time-based intervals as computer time is discrete
	- Define some interval in terms of occurrence of an invent
		- Every fifth function call
		- Every fifth time a jump is executed
	- More accurate as you define the semantics; less noisy as a result, semantics are determinsitic
	- Tricky to interpret as we are more interested in time than function calls
**Quantization errors**
- Interval resolution is limited (usually to single clock cycles but sometimes more). Fro example:
	- CPU can only record jump instructions every fifth CPU cycle
	- It may be that an instruction is misattributed to a problem as recorded later than executed
- Time is (practically) continuous  
- This introduces "quantisation errors/biases"
	- E.g., costs being attributed to the wrong state
	- VTune tries to estimate and correct bias but not always successfully

**Indirect tracing**
- Idea: Some trace events are "dominated" by others (executed depending on their outcome)
	- Think of it as intervals defined by the execution flow
	- For example, control-flow instructions (if, else, for, while) dominate non-control-flow instructions (jumps are expensive!)
	- By figuring out control flow path taken, we may gain insight into performance and let it direct what events to sample (eg where a jump went) and reverse engineer trace from that
	- reduce overhead (sometimes)
	- Fidelity and accuracy usually good (depending on the event and the indirection)

## Profiling
*A characterization of a system in terms of the resources it spends in certain states.*
- Basically taking a trace and performing some aggregation over it
	- This can be global- how long did th eprogram take end to end?
		- usually total cache misses, total CPU cyles, …
	- Or broken down by some other event
		- Cycles per instruction, cache misses per line of code
- **Information is lost** and it takes though as to what metrics we care about
- Provides post-mortem for easier interpretation, congitive load too high for tracing
- lighter on the runtime in reduce perturbation (assuming aggregating is cheaper than dumping)
	- Better to spend CPU cycles aggregating event and writing it to memory, memory access usually more expensive than CPU cycles

**Example: Flame Graphs**
![[Pasted image 20240120175859.png]]
- X-axis shows the stack profile population, sorted alphabetically (not by time),
- Y-axis shows stack depth of a frame
- Each rectangle represents a stack frame
- Width of a box is proportional to the number of collected samples
- Colors are usually not significant
- Boxes collapsed, not necessarily one singular, contiguous function call

## Event Sources
- Detailed: as much info as we need
- Accurate: Close to real world 
- Little perturbation

**Where to get events?**
- Software
	- Library: Manual Instrumentation/Logging
	- Compiler: Automatic Instrumentation
	- OS: Kernel Counters
- Hardware:
	- Performance counter
- Emulator:  
	- a funky hybrid, minimal perturbation but usually not scalable

**Instrumnetation**
- Augment program with event logging code
- Advantages
	- No need for Hhardware sipport
	- very flexible
- Disadvantages
	- Overhead is high: vfunc calls
	- Perturbation is high

- Three approaches:
	- Manual
		- basically printf logging (or using a logging library) 
		- Advantages
			- Fine control over instrumentation
			- Needs no support from hardware or compiler
		- Disadvantages
			- high overhead for implementation & runtime
			- usually disabled for release build
				- needs recompilation for selective enabling
	- Automatic source-level
		- Usually compiler-supported  
		- Source-to-source rewriting is possible 
		- Disadvantages
			- Less control
			- Need for compiler support
		- Advantages
			- Let’s discuss this!
	- Automatic binary level
		- Static
			- Simple, portable
			- Overhead can be assessed from binary, perturbation predictable
			- Cannot be disabled at runtime
		- Dynamic
			- No recompilation
			- Can be performed on running code
			- Works very well with JiT like JVM
			- On native code, leave some slack space to call into code: Interactive
		- Hybrid

## Hardware Support
**Performance Counters**

**Software Performance Counters (OS)**
- Network Packages sent
- Virtual Memory Operations
- …
- Let’s say we want to write code that is efficient at the microarchitectural level; these are not enough as they are extraneous to hardware

**Hardware Performance Counters (OS)**
- Special registers that can be configured to count low-level events without software support
	- Fixed number can be active at runtime, is dedicated register
- Can be used to define collected events as well as intervals: 
	- sample this performance counts (elapsed clock cycles) every 1000 cache misses
- Unfortunately:
	- Often buggy or unmaintained
	- Sometimes poorly documented
	- Accuracy can be poor
		- The common ones are usually okay

## Example: Microarchitectural bottleneck analysis

**How does a CPU work**
Start with empty pipeline work:
![[Pasted image 20240121195653.png]]
Some instructions skip stages but principally they go through them.
As CPU beings working, Pipeline begins to fill:
![[Pasted image 20240121195722.png]]
A bubble is formed if the CPU cannot fill pipeline (eg unexpected jump):
![[Pasted image 20240121195908.png]]
**Branch prediction**
CPU tries to guess where next instruction is coming from (to avoid bubbles)
![[Pasted image 20240121200017.png]]
In the case of a if, we could arbitrarily pick any of the two branches. If we pick branch 1, we can then populate the pipeline with instructions for it.
![[Pasted image 20240121200120.png]]
This could of course be wrong; in that case, the pipeline must be flushed, rolling back to the branch with correct instructions. This is REALLY expensive and we want to avoid them.
![[Pasted image 20240121200217.png]]
**CPUs can thus stall on dependencies**

**Resource Stalls**
**ALU stall:** MUL takes more than 1 cycle, may have multiplication stuck in execution, stalling all subsequent instructions and creating bubbles after execution.
![[Pasted image 20240121200252.png]]
**CPU can also stall due to resources (predominantly ALU)**

**How does the memory subsystem work?**
![[Pasted image 20240121200457.png]]
When we read a value into register, we ask L1 -> L2 -> L3 -> Memory.
![[Pasted image 20240121200546.png]]
In the above, we access values in some data structure with variable padding between elements (quantified by stride). For short strides we see good performance, whereas for wide strides we take 100s of cycles.

![[Pasted image 20240121200705.png]]
With fixed size array, we find if array fits in L1, takes much less time than L2, Same for memory (no L3).
**CPUs can also stall on data access**
Cache hit rate is a very important factor.

Bottlenecks:
- Data Stalls
- ALU Stalls
- Branch Mispredictions
- Control-flow dependencies: CPU cannot speculate eg JUMPs
![[Pasted image 20240121200907.png]]
When instructions enter, CPU will first read from cache, then process via ALU, then be either written to memory or abandoned due to mispredict.

![[Pasted image 20240121201016.png]]
We can sample, for every cycle, whether the CPU did an operation. 
- If No
	- We can ask why and if there was allocation stall (were there resources available to execute). 
		- If there weren't any instructions in pipeline but resources available then frontend bound
		- If Backend bound: Stalling due to cache miss else something else (usually ALU related)
- If Yes:
	- Did the op retire?
		- No: Bad speculation, result discarded
		- Yes: In process of being retired, this cannot be optimised at this level (though maybe at higher level)