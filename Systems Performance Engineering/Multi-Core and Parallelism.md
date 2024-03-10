## Moore's Law and Parallelism
- We all know the old "number of transistors in a chip doubles every two years"
- Moore's law is **not** about making processors faster
- We have to make transistors smaller in order to do this
- Dennard Scaling: *as transistors shrink, they become faster, consume less power and are cheaper*; in essence we get better CPUs for "free"
- 2004: Power consumption and dissipation increases, we lose out on Dennard Scaling
- Jacking up frequency was no longer sustainable: ![[Pasted image 20240213112049.png]]
- In order to address this, we move to multi-core:
	- This increases parallelism as a result of core count
	- By contrast, trying to continue with one core increases core complexity and on chip memory (though this is still high on multi-core)
### Parallelism Has Always Existed
- Multiple forms of parallelism: 
	- Data-level parallelism (DLP) 
		- Example: vector instructions (MMX, SSE, AVX, etc) 
	- Instruction-level parallelism (ILP) 
		- Example: superscalar out-of-order processor 
	- Task-level parallelism 
		- Example: GPUs, POSIX threads, Spark processes, …
		- Programmer has to manually extract parallelism 
		- Lecure focus: task parallelism using threads and processes
### Parallelism Vs Concurrency
Parallelism (or parallel computing): *decomposing a problem into smaller tasks that can be executed at the same time using multiple compute resources* 
Concurrency (or concurrent computing): *several computations are executed concurrently—during overlapping time periods—instead of sequentially—with one completing before the next starts*
![[Pasted image 20240213112937.png]]
### What Can Parallelism Be Good For?
- Performance
	- If we can divide computation then this can lead to better performance
- Cheaper
	- Sharing everything other than cores (RAM, disk, NIC, …)
- We want to appropriately size hardware to serve the workload
#### Parallelism for Performance
```cpp
std::vector nums(…); 
auto middle = nums.size() / 2; 

auto left = std::async( std::accumulate, nums.begin(), nums.begin() + middle, 0); 
auto right = std::async( std::accumulate, nums.begin() + middle, nums.end(), 0);
auto total = left.get() + right.get(); 

std::cout << "total sum: " << total;
```
- Above code parallelises summing of vector
- Note Amdahl's law:
	- $Speedup(p, s) = 1 / ((1 - p) + p/s)$
#### Parallelism for Cost-efficiency
- A single core core is too slow to process incoming data 
	- Example: incoming 100 req/sec, core processes for 2 sec/req
- Solutions:
	- We could double the number of machines, going to 1 sec/req **(horizontal scaling)**
	- We could have multiple cores in a single machine **(vertical scaling)**
	- We could do both **(vertical and horizontal scaling)**
		- Increasing cores has diminishing returns as the bottleneck may be elsewhere
##### Costs in Data Centres Today
- DCs are optimised for **Total Cost of Ownership**
	- This accounts for the entire lifecycle:
		- Purchasing and upkeep until failure
	- This is not just hardware but also
		- Consumption
		- Cooling and power distribution
		- Land costs
### Summary: Key Topics
We will look at: 
- Expressing different forms of parallelism/concurrency 
- Expressing communication in them 
- Performance implications (software & hardware) 
- Using simple modelling to reason about designs
### Example: Simple CPU Provisioning
- Imagine a network server, where: 
	- 1 Gbps network [2^30/8 Bytes/sec] 
	- 1 request is 1KB long [2^10 Bytes] 
	- 1 request takes 50µsec to process [50 * 10^-6 sec]

- Lets use network speed to calculate max no of requests:
	- Network speed: 134,217,728
	- divide by request size: 131,072
- The processing throughput (req/sec for 1 cpu)
	- 1 / 50µsec = 20,000
	- Thus we need 131,072 / 20,000 = 7 cores
## Multi-threading
- Simple conceptual model 
	- Threads are concurrent and share all memory (within a process) 
	- If mapped to separate cores, we have parallelism
- Reality is more complex 
	- Low-level operations, complex to program 
	- Thread management and communication is expensive
### Naive Multi-threading
1. Start with sequential application 
2. Apply existing tools for performance analysis 
3. Use Amdahl’s law to identify what to parallelise $Speedup(p, s) = 1 / ((1 - p) + p/s)$
4. Use multiple threads to execute in parallel
### Critical Path Analysis
![[Pasted image 20240213121703.png]]
- Optimisation is not free but equally **will not necessarily improve runtime**
- Measure, think, plan!
### Cache Coherency
- Cache coherency keeps data consistent across cores 
- Example: MSI coherency protocol – Cache line is in a single state: Modified / Shared / Invalid – Hardware implements per-cache line state machine
![[Pasted image 20240213122437.png]]
- This is not sufficient for parallel computing
### Adding Atomics to Coherency
Coherency is not sufficient to write parallel programs 
- Example: foo++:
		![[Pasted image 20240213122850.png]]
- Need hardware support for atomic read/modify/write to guarantee consistency
![[Pasted image 20240213123002.png]]
#### Compare and Swap
Compare-and-swap - CAS(address, old_value, new_value) 
- Atomically: if `*address == old_value) → *address = new_value`
- Also known as “compare-and-exchange” 
- Formally proven sufficient to implement any atomic operation – Hardware provides more than CAS for performance
EG atomic increment
```cpp
void atomic_inc(uint64_t* addr) { 
bool swapped = false; 								 
while (not swapped) { 
	auto old = *addr; 
	swapped = CAS(addr, old, old+1); 
}
```
### Example with atomic counters
![[Pasted image 20240213124308.png]]
- We instead want entire operations to be atomic
![[Pasted image 20240213124358.png]]
## Synchronisation
- Threads need to synchronize to access critical sections (CS) 
- Problems with concurrency
	- Thread can be rescheduled when inside the CS 
		- T0 enters CS 
		- Scheduler switches to T1 
		- T1 enters CS 
		- CS error!!! 
- Problem with parallelism 
	- Two threads can enter the CS 
		- T0 enters CS 
		- T1 enters CS 
		- CS error!!!
### Primitives:
![[Pasted image 20240213124947.png]]
![[Pasted image 20240304214229.png]]
![[Pasted image 20240304214306.png]]
![[Pasted image 20240304214620.png]]
### False sharing
![[Pasted image 20240304214649.png]]
- Fix using padding to prevent from being on same cache line
![[Pasted image 20240304214857.png]]
![[Pasted image 20240304214907.png]]
![[Pasted image 20240304214914.png]]
![[Pasted image 20240304214924.png]]
![[Pasted image 20240304214934.png]]
### fork/join
- "fork" off threads at a certain point to distribute work, with one new thread per unit work
- "join" all previously forked threads and continue sequential execution
### work dispatching
- Producer cycles through the worker threads, giving each thread one unit of "work" before moving on.
- No effort to maximise utilisation as allocation is arbitrary
### work stealing
- Shared job queue that consumers pull from, no producer to allocate work
- Better utilisation, though in a very massively multithreaded environment contention cost may be quite significant
![[Pasted image 20240304215546.png]]
## Multi-processing
![[Pasted image 20240304215703.png]]
![[Pasted image 20240304220149.png]]
![[Pasted image 20240304220203.png]]
![[Pasted image 20240304220214.png]]
