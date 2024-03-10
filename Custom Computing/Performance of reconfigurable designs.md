![[Pasted image 20240221100702.png]]
![[Pasted image 20240221100756.png]]
### What determines performance?
![[Pasted image 20240221100820.png]]
- **Cycle time is usually fixed:** most designs just use the maximum frequency and hence min cycle time.
- Host: PC -> Bus: PCIe 
- No dynamic events, interrupts etc: these are handled far better by modern processors than by FPGAs.
![[Pasted image 20240221101310.png]]
![[Pasted image 20240221101711.png]]
- network usually QSFP: much faster as point to point
- $T_{stream}$ is max as all operations running in parallel
- Wormhole routing:
	- GPT: 
		- Wormhole routing is a technique used in FPGA (Field-Programmable Gate Array) architectures to efficiently route data between different functional blocks or processing elements within the FPGA fabric. In wormhole routing, data is divided into smaller units called flits (short for flow control digits). These flits are then routed through the FPGA fabric using a packet-switching mechanism.
		- Here's how wormhole routing works in FPGA:
			1. **Flit-based Communication**: Instead of transmitting an entire packet at once, data is broken down into smaller units called flits. Each flit typically consists of a fixed number of bits and includes both data and control information.
			2. **Routing Logic**: The FPGA fabric contains a network of interconnected routing resources, such as switches and interconnects. These routing resources are responsible for directing the flits from their source to their destination.
			3. **Virtual Channels**: To avoid contention and improve throughput, wormhole routing often employs virtual channels. Virtual channels are logical lanes within the routing network that allow multiple flits to be transmitted concurrently without interfering with each other. This helps in reducing congestion and improving overall performance.
			4. **Flow Control**: Flow control mechanisms are used to manage the transmission of flits and prevent deadlock situations. Techniques such as credit-based flow control or token-based flow control ensure that flits are only sent when there is available buffer space and routing resources.
			5. **Adaptive Routing**: In some implementations, wormhole routing may incorporate adaptive routing algorithms. These algorithms dynamically select the most efficient path for each flit based on factors such as current network congestion and latency. Adaptive routing can help optimize performance in varying traffic conditions.
			6. **Buffering**: At each routing node, temporary buffers are used to store incoming flits until they can be forwarded to the next hop in the network. Proper buffering management is crucial for maintaining low latency and preventing packet loss.
		- Overall, wormhole routing in FPGA architectures offers a balance between low latency, high throughput, and efficient resource utilization. By breaking data into smaller units and dynamically routing them through the FPGA fabric, it enables efficient communication between different components of the FPGA design
![[Pasted image 20240221102257.png]]
$T_{compute}$ is easiest: simply number of cycles times cycle time (recall cycle time = 1/frequency). You control the number of cycles; the cycle time is reported to you by the FPGA compile tools
![[Pasted image 20240221102444.png]]
![[Pasted image 20240221102555.png]]
- Memory performance modelling: unlike the bus, read and write bandwidth is not shared, so the total time is the read time plus the write time. Note that memory bandwidth is typically much larger than bus bandwidth 
- If you can move data to the FPGA on-card memory, it’s often worth doing so.
![[Pasted image 20240221102920.png]]
- Efficiency favours linear, monotonically increasing accesses (somewhat obviously)
![[Pasted image 20240221103452.png]]
- External memory is accessed in bursts – a sequence of N contiguous bytes, where N depends on the memory architecture, e.g. N=384 for some Maxeler systems. It’s still random access, but unlike software you can’t just access 1 byte at a time.
- The graph shows how efficiency varies with transfer size. Note the log scale on the graph – you only get close to 100% efficiency with long blocks of adjacent requests.
![[Pasted image 20240221103906.png]]
![[Pasted image 20240221104041.png]]
- Important: for long streams (millions or billions), stream time dominates initialization time. For small streams, initialization time becomes important.
- It takes time to setup, config, fill pipeline, run and drain; this is why we want accesses to be as long as possible
- Streaming multiple times in a call is hence very expensive
![[Pasted image 20240221104918.png]]
- You can improve the effective bandwidth of both the bus and memory by using compression, but note the disadvantage: you need to spend both software time and FPGA area in implementing the compression and decompression. Also, not all data can be easily compressed.
![[Pasted image 20240221105200.png]]
- You can improve parallelism by running multiple pipelines at once – typically each pipeline is a kernel. This is still limited by the total area available.
- The area is the number of resources used. There’s a fixed amount of each type of resource: DSPs (Digital Signal Processing elements, typically multiply-add); BRAMs (Block RAMs, small on-chip memories); LUTs (lookup tables, used to implement arbitrary logical functions).
![[Pasted image 20240221105324.png]]
![[Pasted image 20240221105554.png]]
- Compression trades compute for bandwidth: reduce the data transfer but uses extra software time, and hardware area, for the compression and decompression.
- The above numbers don't account for decompression and compression: Worsens latency but throughput remains constant
![[Pasted image 20240221105844.png]]
![[Pasted image 20240221105956.png]]
![[Pasted image 20240221110155.png]]
![[Pasted image 20240221110734.png]]
1. Answer (mine)
		1. $T_{write}$: (20 + 25 + 8) = 53; 53/30 = 1.76
		2. $T_{compute}$: (100,000,000 /  100,000,000) = 1s
		3. $max(T_{write}, T_{compute})$ = 1.76
2. Answer (mine)
```java
DFEVectorType vectorType = new DFEVectorType(DFEFloat(8, 24), N);
DFEVar p = io.input("p", vectorType)
DFEVar M = io.input("M", vectorType)
DFEVar q = io.input("q", vectorType)

for (int i = 1; i < 1_000_000; i ++) {
	q[i] <== p[i] * ((M[i - 1] + M[i] + ))
}

```

