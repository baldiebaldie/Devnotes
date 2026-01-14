Secure Coding
01-12-2026

## What does it mean for code/software to be secure?
- Abstractly break down your service and related into a policy
	- Then change the policy whenever security concerns might arise
### Policy vs Mechanism:
Policy (the law):
- **Secure software adheres to constraints specified by a policy**
- A policies job is to specify constraints
- Specification the **what** of a "good" or "bad program" / program behavior (policies are concerned with how a program behaves)
Mechanism (law enforcement):
- Concrete implementation ensuring software adheres to a policy
- It is what "enforces" the policy
- Ensures software obeys
Formally written
When a mechanism "M" ensures software obeys a policy "P", **"M _enforces_ P"**

Big organization often work in SOCs
SOC (Security operation centers)

---
### Potential security tool outcomes
False alarms take a lot of work and time.
Here are four scenarios where we must consider and are ranked off of favorability 
``` 
2 True+: Mechanism signals policy violation & is correct
3 False+ Mechanism signals policy violation & is incorrect (Type I Error)
1 True -: Mechanism signals satisfaction & is correct
4 False +: Mechanism signals satisfaction & is incorrect (Type II Error)
```
In other words:
- False Positive - False alarms. Incorrect scans that there is a security vulnerability
- False Negatives - When a security risk is present but NOT detected
- True Positive -  When security tools correctly identify a vulnerability
- True Negative - When security tools correctly identify safe as safe
---
### Dynamic vs Static mechanisms
There are many types in programming
- int
- bool
- char
and there can be constrictions via policy 
- int allowed operations: +, -, \*, /
- bool allowed operations: true, false
- char: construct, push, pop, access array in bounds
For security reasons, we would want a type checker to make sure our program follows the type policy:
Type safety policy - time of memory access control policy

``` c
int main () {
	if alwaysTrue() {
		exit
	}
	else {
		f();
	}
}

void f() {
	5(); //these are type errors
	f+f; //these are type errors
}

bool alwaysTrue() {return true}
```
Dynamic mechanisms have false positives
- static mechanism - mechanisms that run before the code executers
	- ALL STATIC MECHANISMS WILL ALWAYS BE IMPRECISE (they will can have false positives or false negatives)
	- No static mechanism can tell dynamically if a program will be well behaved
- dynamic mechanisms - mechanisms that check things during runtime
	- Example of dynamic mechanisms:
		- Firewall
		- Intrusion detection systems
- With this in mind, its undecidable for a computer to work out what an arbitrary program will do when it runs
- 
Note: You can have an infinite amount of mechanisms that can enforce any specific policy
- **There exists a many to many relationship between policies and mechanism**
#### Intrusion detection systems (dynamic mechanisms)
Lets say machine A is speaking to machine B
- Intrusion detection systems sit between the two as a mechanism to ensure Alice(A) is not sending malicious packets to Bob(B).
Now what if we enforce a policy Bob is not allowed to send anything malicious to Alice (specifically a port scan. A type of malicious reconnaissance)
- "We want IDS to blank any host performing malicious reconnaissance" (we don't want A scouting info on B)
- This introduces false positives (perfectly normal, non-malicious network traffic can be flagged as malicious)
THIS IS THE LARGEST PROBLEM IN COMPUTER SECURTRITY
- The gap of what we want on a human level (we want our systems to be secure)
- How do you bridge the gap of the low level policy 