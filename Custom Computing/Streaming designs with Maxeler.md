## Introduction
![[Pasted image 20240212140905.png]]
FPGAs allow you to design your own circuits for your particular application. These circuits can run much faster than CPUs or GPUs for some problems, since you need only include the hardware for your application. On this course, we teach you Ruby and Maxeler. Commercial tools typically use VHDL or Verilog, but there are other design inputs such as OpenCL and C. Maxeler can be thought of as a domain-specific language for streaming systems implemented on FPGAs.
## Acceleration Development Flow
![[Pasted image 20240212141800.png]]
This shows you how you can accelerate a software system using Maxeler. It’s similar to a software flow, but with extra tests to partition into software and hardware parts, plus build and deploy the hardware.
Key steps: 
- profile the code: what takes the longest time? 
- transform to use hardware 
- Simulation: since building for hardware can take hours, it’s important to check correctness first, using a simulator 
- Hardware build
Compilation can be extraordinarily slow, bear that in mind.
## MaxCompiler
![[Pasted image 20240212141921.png]]
- First two lines are input
- Last line is output
- Middle is computation, overloads Java's `x` via a language extension
## Application Components
![[Pasted image 20240212142325.png]]
Dataflow kernels (see example on previous slide) run on the FPGA. Typically these correspond to the bodies of software loops we want to accelerate.
## Programming with MaxCompiler
![[Pasted image 20240212142553.png]]
### Simple Application Example
![[Pasted image 20240212142639.png]]
### Development Process
![[Pasted image 20240212142812.png]]
This is what the code looks like with Maxeler. There are three parts: 
1. The Kernel (right hand side): build a dataflow graph in MaxJ 
2. The Manager (centre): connect the kernel’s inputs and outputs to the host or memory. 
3. The host code. The Maxeler tools generate a function. When you call that function, the hardware runs with the data you supply as parameters. Note that the streams in hardware map to C arrays in software.
#### The full kernel
![[Pasted image 20240212143456.png]]
This shows how the MaxJ kernel maps to hardware.
Note that there is a one-to-one correspondence between each element of the MaxJ kernel and the resulting dataflow graph. This is what is meant by syntax-directed.
### Streaming Data through the kernel
![[Pasted image 20240212143533.png]]
On each cycle, one input is read from input stream and one output is written to output stream.
### Compile,  Build and Run
![[Pasted image 20240212143704.png]]
Steps to build and run the hardware. 
Compile the hardware using Maxeler’s compiler. 
Run it like any other Java program – this builds the dataflow graph and generates the hardware. 
Link the generated .max file with your C software (the host program). Run the host program.
### Meta-programming 
![[Pasted image 20240212143838.png]]
- The value x cannot be evaluated at compile time, as `DFEVar` implies that x is resolved at hardware buildtime (java runtime)
### Dataflow Graph Generation: Simple
![[Pasted image 20240212144442.png]]
### Dataflow Graph Generation: Simple II
![[Pasted image 20240212144521.png]]
- Note, shape is due to Java operators being left associative
- Matters in case:
	- Floating point as affects results
	- Latency for eg `(x + x) + (x + x)` vs `(x + x + x) + x`
- As Maxeler is syntax directed, optimisations in the compiler may not change this
### Data Graph Generation: Variables
![[Pasted image 20240212145143.png]]
- See above on metaprogramming
### Data Graph Generation: Conditionals
![[Pasted image 20240212145237.png]]
### Conditional Choice in Kernels
![[Pasted image 20240212145539.png]]
### Constants in Kernels
![[Pasted image 20240212145554.png]]
### Dataflow Graph Generation: Java Loops
![[Pasted image 20240212145636.png]]
- Again, note metaprogramming slide; loop evaluated at **Hardware buildtime/Java Runtime**
![[Pasted image 20240212145849.png]]
- We can generate arbitrarily large dataflow graphs via loops etc

 