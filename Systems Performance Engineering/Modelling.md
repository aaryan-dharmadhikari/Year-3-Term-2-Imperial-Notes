## Motivation
![[Pasted image 20240128215240.png]]
![[Pasted image 20240128215321.png]]
- eg if you intend on using ASIC; pretty expensive to trial
- Predicting cloud compute costs (eg azure)
- Provisioning costs for systems
![[Pasted image 20240128215432.png]]
## Aspects of System
![[Pasted image 20240128215634.png]]
- Models require at least an understanding of Machine, Code and Data
## Assumptions
![[Pasted image 20240128215700.png]]
- IE data is random and most inputs occur using known distribution
- Modelling noise is very difficult; hard to do predictably and valiuably
## Approaches
![[Pasted image 20240128215821.png]]
- **Numerical**
	- Easy to interpret, limited effectiveness
- **Analytical**
	- Usually an equation; hard to interpret
	- Systems can interpret by themselves using (eg) mathematical techniques
## Numerical Models
**We gather data through Microbenchmarks**
![[Pasted image 20240128220032.png]]
**EG**
![[Pasted image 20240128220042.png]]
- Array of integers we use some large constant for; some constant stride
- The experiment is in the loop:
	- We always access a value in the stride
	- We sum arbitrarily to use the value in some capacity 
	- Addition is very cheap <1 cycle so we use it 
- This is trying to benchmark memory access
**This gives**
![[Pasted image 20240128220226.png]]
We can then interpret this:
![[Pasted image 20240128220241.png]]
- We could interpolate values by joining the dots
- This is clearly not the most accurate way of getting information
**If we actually run the experiment**
![[Pasted image 20240128220357.png]]
- Cost increases -> flattens -> increases -> flattens
	- This is to do with memory access. Larger strides mean more cache misses
- We could interpolate points here:
	- +
		- Easy to get data (assumes availability of system)
		- Based on ground truth; actually measured
		- Easy to interpret
	- -
		- Does not generalise well; what if we use different hardware or change something? Does not apply there.
		- Need lots of experimental data if many system parameters (high no of dimensions); very expensive
		- Limited confidence in predictions, measurements could be wrong or missing, inaccurate, etc.
		- Whilst easy to interpret, limited in what we can interpret; there is nothing in the model motivating the data, we happen to have an understand of eg cache so we can get value
		- Limited insight into system so interpretability bounded
## Analytical Models
![[Pasted image 20240128220826.png]]
- We can already see from this (scary) example, they are quite intimidating and hard to see if the accuracy is that useful (cycle accuracy is too granular for example)
![[Pasted image 20240128220937.png]]
- Models are hard to develop 
- Hard to figure out which parameters have little effect on performance; whether they are actually worth tuning
- We want a model to be as simple as possible without losing predictive power; hence the need for extensive validation: we may have misunderstood a system and this could become important if we change contexts, making the model worthless
![[Pasted image 20240128221136.png]]
- This model, as you can see, is very complex
- This model tries to understand the cost model and its impact on query optimisation by varying some parameters and seeing which plans the optimiser selects
- Project names Picasso (lol)
- Behaviour of the optimiser, as we can see is very hard to understand and interpret
	- This demonstrated the complexity involved in analytical modelling
- Systems are driven by user needs; when performance needs are unmet we might optimise very specific use-cases based on needs- this can lead to extremely specific changes that skew performance in a particular way for a small case (think a small coloured polygon above) -> this leads to messy unpredictability as seen in the model; very hard to mathematically predict, analyse and formalise.
## How Do We Get These Analytical Models?
## Model Fitting
*Go from this:*
![[Pasted image 20240128221638.png]]
 *To this:*
![[Pasted image 20240128221659.png]]
 - Literal linear regression makes this analytical?
	 - The line is blurry, we arbitrarily decide that a regression model is analytical where joining the dots is not. 
**What about**
![[Pasted image 20240128221842.png]]
![[Pasted image 20240128221828.png]]
Apply **A**ctual **I**ntelligence! (*And simplifying assumptions*)
![[Pasted image 20240128221917.png]]
## Example for Memory Access
*As seen in [Manegold et al., Generic database cost models for hierarchical memory systems]*
**Recall CPU structure**
![[Pasted image 20240128222040.png]]
**Such a system could (roughly) have parameters:**
![[Pasted image 20240128222102.png]]
**An example equation could then be**
![[Pasted image 20240128222327.png]]
- Every time we access a block from a lower level, we pay the latency cost
- Then we just have to estimate the probability that such a thing happens; this is capped at $1$
	- This is really the result of the stride; hence $min(1, \frac{s}{B})$
![[Pasted image 20240128222601.png]]
- Base on the relationship between size of cache and stride we can model average $T_{mem}$
*Model implemented [here](https://www.wolframcloud.com/obj/pirk0/Published/CPUModel.nb)*
![[Pasted image 20240128223334.png]]
- Using stats or code we can optimise these parameters
![[Pasted image 20240128223402.png]]
- Some architectures well documented, others less so
- We ideally want a generalisable model which can optimise itself
	- Eg model could tune itself based on no cores sharing L3 cache
	- resilience: system can adapt to changes in design eg inclusive vs exclusive cache
## How Do We Get to Describing Behaviour of Code Using Equations?
**Let's try model this:**
![[Pasted image 20240128223602.png]]
- Value of `input1` is offset of `input2`
- Will have more random access patterns
**Parameters**
![[Pasted image 20240128223740.png]]
**Modelling sequential access**
![[Pasted image 20240128223848.png]]
- Read, skip, read, skip - as if $stride = 2$
![[Pasted image 20240128223927.png]]
- If gap between two accesses is < block size, every cache line holds at least one value that needs to be read; so size of mem region divided by block size gives an accurate no of cache misses
- Otherwise, one cache miss per access; for every access we read as many cache lines as needed to access value (words in access / block size)
- Cache miss may be due to misalignment; which we don't take into account here:
	- ![[Pasted image 20240128224304.png]]
## Random Access Modelling
![[Pasted image 20240128224509.png]]
- As stated earlier we assume uniform distribution of accesses
![[Pasted image 20240128224627.png]]
- We define this algebra to model complex access patterns
- The actual composition and evaluation of this is "outside the scope of the module"
![[Pasted image 20240128224654.png]]
- Uniform access to `input1`, repetitive random access to `input2`
	- This is why we must consider both in our cost function; we sequentially access the randomised array (`input1`) and then randomly access `input2`
![[Pasted image 20240128225151.png]]
We could do this instead; giving
![[Pasted image 20240128225206.png]]
## Random, Non-repetitive Access
![[Pasted image 20240128225230.png]]
- IE we don't access same value twice
- Useful to model data intensive designs
![[Pasted image 20240128225258.png]]
- We could use this model in practice in a DB system
## Modelling Stateful Systems
**Good ol' stochastic methods would work here**
![[Pasted image 20240128225553.png]]
## (Discrete) Markov Chains
![[Pasted image 20240128225655.png]]
**Modelling code**
![[Pasted image 20240128225848.png]]
**Results**
![[Pasted image 20240128225934.png]]
**Let us modify to include branch prediction**
![[Pasted image 20240128230023.png]]
## How Does Branch Prediction Work?
![[Pasted image 20240128230058.png]]
- Table stores, for a large no of code locations, branches taken. They will predict behaviour with a feedback loop to improve this.
![[Pasted image 20240128230236.png]]
- We try to model this via a markov chain
- models the likelihood of branch being taken
- Saturated counter can encode this information in 2 bits
![[Pasted image 20240128230318.png]]
![[Pasted image 20240128230457.png]]
- We can compare the model with actual performance of an ivy-lake chip
- Significant divergence between model and performance, suggesting a 6 stage model instead!
- Intel might have fucked it
## Modelling Summary
**Modelling an entire system:**
![[Pasted image 20240128230631.png]]
