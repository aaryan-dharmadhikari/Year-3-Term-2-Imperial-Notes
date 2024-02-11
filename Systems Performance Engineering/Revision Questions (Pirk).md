## General pointers
- Read the question:
	- Discuss is not asking for definition or examples
### Q1: How would you instrument this piece of code
- TODO: Add code
- Naive: print every line with more granularity (`f2(input2 + i, i)`)
	- Too much perturbation
- Sample: also quite dense and could involve perturbation
- Indirect tracing: figure out what dominates what
	- After tracing we know hot path
	- f3 is unknown
#### Q1: Answer
- Minimise perturbation
- Since metric is unspecified we assume that we collect no of times each instruction is executed
- Idea is block counting: exploit limited no of control-flow-divergence
- GET REST FROM SLIDES

### Q2: What metrics would you collect to optimise this
- Cache misses
	- Having the amount of cache misses would indicate whether this is memory bound and could be optimised
- selectivity 
	- Branch misprediction would be useful
- Memory accesses
	- We don't know if accesses will hit or miss cache
### Q3: Predict branch predictor state

### Q4: Describe  mem access pattern
- Input has unique values, randomly distributed
- non-repeating; random access to `tmp`
- linear traversal of `input`
- first loop has sequential traversal and non-repeating random access in parallel
- 