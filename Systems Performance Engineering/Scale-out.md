![[Pasted image 20240304210630.png]]
![[Pasted image 20240304210649.png]]
#### Design Considerations
![[Pasted image 20240304210706.png]]
![[Pasted image 20240304210720.png]]
![[Pasted image 20240304210817.png]]
![[Pasted image 20240304210928.png]]
![[Pasted image 20240304211415.png]]
![[Pasted image 20240304211439.png]]
- In essence, improbable levels of overhead are more and more likely the number of servers are involved: hence obsession on nines
![[Pasted image 20240304211603.png]]
![[Pasted image 20240304211850.png]]
- Incentive to maximise resource utilisation is strong, but naturally the greater utilisation is the less excess capacity exists (hence "pay as you use" solutions like k8s catch on)
![[Pasted image 20240304211906.png]]
![[Pasted image 20240304212455.png]]
![[Pasted image 20240304213022.png]]
### Scale-out architectures
![[Pasted image 20240304213830.png]]
![[Pasted image 20240304213837.png]]
- Productivity is gained by virtue of the division of components leading to:L
	- Performance overheads: eg from comms across APIs, which will always be slower than a monolithic process - This is also harder to optimise in turn
	- Complexity management: clearly defined APIs and services are easier to maintain in the long run, owing to superior productivity for devs overall
	- Scaling: eg having a dynamic nodepool in k9s is easier than vertical scaling of a single server, leads to better utilisation and probably more cost effective in a corporate environment
![[Pasted image 20240307111736.png]]
- Consolidation in this case means:
	- Virtualisation allowing for partitioning of physical servers into VMs, better utilisation vs no VMs because resources can be divided based on demand
	- Resources are pooled (eg CPU, memory, storage), so cheaper (but obviously more contention and overhead from VMs)
	- Some cloud systems may offer dynamic resource allocation too
	- Policy based management may also be applicable, helps optimise resource usage
![[Pasted image 20240307113121.png]]
![[Pasted image 20240307113200.png]]
- Modularity gives easier iteration as all microservices are independent, requiring only compliance to existing API and otherwise being standalone
- Overhead of complexity in system management:
	- Slow VM initialisation times, booting up is expensive and slow
	- Monitoring VMs can be quite challenging, keeping image up to date across VMs is hard
	- Overhead of keeping multiple OS instances on memory also expensive
![[Pasted image 20240307114220.png]]
- We can also use containers which are far more lightweight and may require fewer VMs (*used to isolate tenants*) to run a similar workflow, leading to less duplicate memory use and other drawbacks presented for VMs
![[Pasted image 20240307114431.png]]
- Abstracts running AWAY from the tenant
- This is what k8s does
![[Pasted image 20240307204920.png]]
![[Pasted image 20240307205224.png]]
![[Pasted image 20240307205133.png]]
- Excess capacity is worth the cost, 60% is an approximate heuristic for utilisation that allows for a bit of slack so it works as much as possible
![[Pasted image 20240307205348.png]]
![[Pasted image 20240307205435.png]]
- IE the cost of creating a k8s pod, initialising a Docker container
![[Pasted image 20240307205516.png]]
- very limited in languages supported, utilisation is "perfect" as tenant is blind to runtime costs, pays per function in essence
- fork from running processes, ezpz
![[Pasted image 20240307210311.png]]
![[Pasted image 20240307205746.png]]
- Keep sending reqs/fork from a hot system
## Communication Methods
![[Pasted image 20240307210759.png]]
![[Pasted image 20240307210827.png]]
![[Pasted image 20240307211700.png]]
![[Pasted image 20240307211709.png]]
![[Pasted image 20240307211922.png]]
![[Pasted image 20240307212118.png]]
- This process is resource-intensive because each time data is sent or received, the operating system has to copy data from user space to kernel space (and vice versa), which is an expensive operation in terms of CPU cycles.
![[Pasted image 20240307212628.png]]
- Remote Direct Memory Access
- Access server memory DIRECTLY: no allocation required
- All done in userspace: no kernel-space context switches
- Before the actual data transfer, a setup phase is required. During this phase, the applications on both sides communicate using standard networking protocols to exchange information about the memory regions that will be used for RDMA operations. This involves setting up memory buffers and registering them with the RNICs, which involves pinning the memory to prevent it from being swapped out to disk and providing virtual-to-physical memory translations.
- ZERO-COPY: Usually Application->Kernel->NIC, RDMA: NICs have special hardware such that reading and writing can be done without buffering or CPU intervention
![[Pasted image 20240307213302.png]]
![[Pasted image 20240307213315.png]]
![[Pasted image 20240307213454.png]]
## Taming latency and cost at scale
![[Pasted image 20240307214033.png]]
![[Pasted image 20240307214050.png]]
![[Pasted image 20240307214058.png]]
![[Pasted image 20240307214118.png]]
- Poisson makes sense for arrival, models frequency
- **Arrival Assignment (who and when is assigned to process)**: This parameter determines how incoming tasks are assigned to the resources available to process them. The policies for this can vary widely, from random assignment to load balancing strategies aiming to keep all servers equally busy.
- **Service Time Distribution (time to process a request)**: This refers to the distribution of the time it takes to serve a request or complete a job. It is crucial because it affects how quickly the queue moves and, therefore, the overall system latency. Different types of distributions can be used:
	- **Constant**: An ideal situation where each request takes exactly the same amount of time to process. This is rarely the case in real-world systems.
	- **Exponential**: In many real-life systems, the service time is exponentially distributed, due to contention (memoryless Markov processes)
	- **Bimodal (heterogeneous)**: This represents scenarios where there are two distinct types of service times, which could be indicative of two types of requests or jobs. For example, a server might handle some quick, simple requests and some that are more time-consuming, leading to a service time distribution with two peaks.
![[Pasted image 20240307214325.png]]
- work conservation is the idea that all resources should be kept busy if there are submitted jobs!
![[Pasted image 20240307214907.png]]
- 32 queues, Markovian, General time distribution, 1 server per queue, FCFS
- Provide predictability with exponential service times (by distributing work)
![[Pasted image 20240307215057.png]]
- in bimodal, long tasks will block short tasks, which is undesirable and would lead to far worse throughput than a 2 channel output. Particularly bad if short tasks are latency sensitive
![[Pasted image 20240307215312.png]]
- ZygOS is described as an almost-single-queue system that supports large service dispersion and avoids head-of-line blocking. This system would allow for scaling up by handling large variances in service times more gracefully and ensuring that slower-processing tasks do not block the entire system.
- It is work-conserving, which means it doesn't waste computational resources. When a core is free, it can "steal" work from other cores to maximise utilisation.
- The shuffle queue is a key component of ZygOS. It is used to reorder tasks to prevent head-of-line blocking and to efficiently manage tasks across different cores.
![[Pasted image 20240307215323.png]]
- R2P2 aims to implement a global queue for all service instances when establishing a connection. This means that instead of each service instance managing its own queue, there is a single queue from which all instances pull requests. This can be beneficial because it allows for better load balancing and can prevent any single instance from becoming a bottleneck.
- The system maintains regular client/server connections afterward and does not rebalance long-lived connections. This approach can help reduce tail latency due to overloaded nodes because no single node will continue to receive new requests if it is already heavily loaded.
![[Pasted image 20240307215331.png]]

![[Pasted image 20240307215337.png]]
