Secure Coding
01-21-2026

Sound enforces
- cannot have any false negatives
- may have false positives
- at its extreme, mechanism that flags everything as an error

Complete enforces:
- cant have false positives
- may have false negatives
- at its extreme, allowing everything and flag no errors

Ideally we look for a spot in the middle for our security. If we had to choose one, we would choose soundness


### What is a program (in the context of computer security)
A program is (lower case p for program): 
- **_Program_**: A set of traces
- **_Traces_**/execution/run: A sequence of security relevant actions/events
- i.e 
	- what does a trace of the 'echo' program looks like?
	- NOTE: semicolons separate actions and commas separate traces.
		- 2 traces might look like: {input(a) ; output(a), input(a) ; output(a) ; input(a) ; output(a)}
	- input(a) ; output(a) <---------------- this is one trace of echo
	- input(a) ; output(a) ; input('ctrl-c') ; exit <----------- another example of a trace
	- for an empty trace (program started but no input or maybe some non security relevant thing happens). In this case, we use epsilon ( ε )
	- what if a program feeds infinite characters into the program?
		- input(a) ; input(a) ;input(a)... <----------it would run forever, this is an infinite trace
		- We say the trace **_diverges_** (non terminating)
		- Conversely a program that terminates is a **_converging_** program

### What is a policy
**_Policy_**: a set of programs (Upper case P)
- The output is a predicate (either true or false)
- If the program doesn't fall in the policy its a bad program
- "These are all of the good programs"
Breaking it down
- A policy is a set of a sets of traces

### Properties
If your program sits in the set of good programs then your program is good.

Formal definition
- In English
	- A policy P is a property if and only if there exists a set of good traces G such that for all programs p: program exists in a policy if and only if a program in in the subset of being good.
- In notation
	- A $policy$ P is a $property$ **⟺** $∃$ a set of good $traces$ G s.t $∀$ $programs$ p: $p ∈ P$ **⟺** $p ⊆ G$


A policy P is a property if and only if there exists a group G, for all p: p is a subset of P if and only if p is a subset of G A policy $P$ is a property $\iff$ $\exists G \ \forall p: p\in P \iff p \subseteq G$