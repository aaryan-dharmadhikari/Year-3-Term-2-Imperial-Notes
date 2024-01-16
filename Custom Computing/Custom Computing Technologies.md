## Motivation
Everything thus far has been involving general purpose computing; is always efficient?
![[Pasted image 20240115182654.png]]
The same program has quite a wide range of characteristics; illustrate the trade-off between flexibility and performance. There is a wide range of characteristics across different technologies. Note performance difference of 3,000,000x across best and worst solutions.

**Learning outcomes**
- develop parametric descriptions of custom computers
- develop alternative designs for custom computers that meet specified requirements
- analyse the performance of a custom computer in terms of time and space
- evaluate space/time trade-offs between competing custom computing designs in order to determine optimal solutions
- use simulation to compare the intended and actual behaviour of custom computers

## Now to the content…
**Generalisation and Specialisation**
	Start with some design $f_0$; once it proves to be inadequate *eg by having to serve a new application or fully exploit a new technology* we can try various ways to generalise $f_0$ to become a generic $f(x)$ that can then be specialised such that $f_{0}= f(x_0)$.
	Then $f(x)$ forms a *design space* that can be specialised to cover any design $f_{k}= f(x_k)$.
	![[Pasted image 20240115184321.png]]

**Benefits of customisation**
- **Accuracy** can use smaller/bigger data structures as required
- **Throughput** rate of producing results
- **Latency** time between first input and output
- **Reconfiguration Time** speed of adapting to changes
- **Size** area, vol, weight
- **Energy/Power Consumption** mobile and remote applications
- **Dev Time** design and validation
- **Cost** minimise fabrication, post-delivery fixes and enhancement costs
In order to do this we must *prioritise design objectives*: for example, **smallest design at given speed consuming given energy**

Customisation could be:
- **application oriented** eg run-time conditions
- **implementation oriented** eg technology used

### Examples

**Application-specific integrated circuits (ASIC)** provide the best performance at the expense of flexibility. Moreover, they are costly since both correctness and efficiency are paramount; if they don't work as expected then they are useless.
**Moore's second law** says the cost of fabrication increases exponentially so making current gen circuits is very expensive.

**Field Programmable Gate Arrays (FPGA)** have multiple resources at varying levels of granularity, coarse-grained arithmetic and memory blocks are programmable to support different i/o standards. They also have fine-grained logic resources for computation and fine-grained memory resources for storage
![[Pasted image 20240115185744.png]]
FGPAs are getting more heterogeneous, with hardware specific to specific use cases, such as Video + AI; with MSFT and AMZN both now hosting FPGA resources for hire.

FPGAs do not involve fabrication and are thus much faster to develop on, lower risk and more flexible though at the expense of execution efficiency. This is why they are being adopted more widely that ASICs.

### When to specialise?
- **Fabrication time** for ASICs we optimise before fabrication, by specialising the physical circuit we lose out on any post-fabrication options. This is naturally inflexible but provides near optimal efficiency for compilation and execution.
- **Compile time** pre-execution optimisation like FPGAs, which improves execution efficiency at the expense of slow compilation.
- **Instruction time** interpret during execution, providing tremendous flexibility at the expense of efficiency. This is what our CPUs do; with some specialisation during execution like instruction pipelining.
![[Pasted image 20240115190620.png]]

**Makoimoto's Wave** shift between standardisation and customisation every 10 years. Currently we are in a Standardisation period with FPGA adoption increasing rapidly.

### Metrics for design
- **Non-recurring engineering (NRE) costs** one time cost of designing the system
- **Total cost**: $total\,cost=NRE\,cost + unit\,cost * \,number \,ofunits$ 
- **Size, power, performance**
- **Flexibility** ie changes w/o NRE cost
- **Time-to-prototype, time-to-market**
- **maintainability**
- **correctness, safety, robustness**
![[Pasted image 20240115191308.png]]

As we progress, we can expect more SoC style FPGAs, that focus on how chips can interface to course-grained computational and memory efficient resources. These resources are less flexible but more efficient.

NEXT [[Systems and Customisation]]