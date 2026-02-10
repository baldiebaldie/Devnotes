	 

#### Review from Last lecture
- A policy is a set of programs
- A policy is a set of sets of traces
- A policy is a set of all sets of traces
$$P = \{ \{t_1, t_2, \ldots\}\}$$
---
#### Lecture Notes (0)

A policy is a property when there exists a set of good traces (G): A program is good for the policy if and only if its traces are in set G.

A policy must satisfy:
1) $P \in P$ => $P \subseteq G$ 
2) $P \subseteq G$ => $P \in G$

---
#### Unclear Lecture Notes (1)
Proof Let $G_1 = \{t | read(0) \notin t\}$
Consider any $p \in P$, By definition of $P$, $\forall t \in p$ read(0) $\notin t$

...

$P_2 = \{ \{ t^{1}, \text{output}(k_1), t^{2}, \text{output}(k_2), \ldots \} \}$ 
 $\{ k_1, k_2, \ldots \} = \underbrace{\{00..00, 00..01, \ldots, 11..11\}}_{\text{The set of all 64-bit keys}}$
...

If a program is good if and only if 

case 1: t_0 \in G_2
Let some program P = {t_0} then $P \subseteq G_2$, but $P \subsetneq P_2$
case 2: ...

---
#### AI Proof Reconstruction (1)


$$P_2 = \{ \{ t^{1}, \text{output}(k_1), t^{2}, \text{output}(k_2), \ldots \} \}$$
 $$\{ k_1, k_2, \ldots \} = \underbrace{\{00..00, 00..01, \ldots, 11..11\}}_{\text{The set of all 64-bit keys}}$$

**Proof: $P_2$ is NOT a property**
Assume for the sake of contradiction that $P_2$ is a property.
By definition, there must exist a set of good traces $G_2$ such that for any program $P$:
$$P \in P_2 \iff P \subseteq G_2$$
Let $t_0$ be an arbitrary single trace (e.g., one that outputs key $0$). We analyze two cases:

**Case 1:** Assume $t_0 \in G_2$
	Let program $P_{small} = \{t_0\}$.
	Since $t_0 \in G_2$, then $P_{small} \subseteq G_2$.
	If $P_2$ is a property, this implies $P_{small} \in P_2$.
	**Contradiction:** $P_{small} \notin P_2$ because it does not output _all_ keys.

**Case 2:** Assume $t_0 \notin G_2$
	Let program $P_{all}$ be a program that outputs all possible keys. We know $P_{all} \in P_2$.
	However, since $P_{all}$ outputs every key, it must contain the trace $t_0$.
	Since $t_0 \in P_{all}$ and $t_0 \notin G_2$, then $P_{all} \not\subseteq G_2$.
	If $P_2$ is a property, this implies $P_{all} \notin P_2$.

**Contradiction:** $P_{all} \in P_2$.

**Conclusion:** Since both cases lead to a contradiction, no such set $G_2$ exists. Therefore, $P_2$ is not a property.

---
#### Unclear Lecture Notes (2)
$$P_2 = \{ \{ t^{1}, \text{output}(k_1), t^{2}, \text{output}(k_2), \ldots \} \}$$
 $$\{ k_1, k_2, \ldots \} = \underbrace{\{00..00, 00..01, \ldots, 11..11\}}_{\text{The set of all 64-bit keys}}$$

What does it mean for a program to have no traces. Not even an input trace.
A program that can't even run. (Symbol: $\emptyset$)
A program that can run but does nothing (Symbol: $\epsilon$)

$P_2$ is not a prop

Proof. BWOC, ... $P_2$ is a prop w/ good traces $G_2$ such that 
	$\forall p$: $p\in P_2 \iff p \subseteq G_2$
Let $p \ne 0$. Then $p \subseteq G_w$, but $p \notin P_2$ $\bot$

---
#### AI Reconstruction (2)
$$P_2 = \{ \{ t^{1}, \text{output}(k_1), t^{2}, \text{output}(k_2), \ldots \} \}$$
 $$\{ k_1, k_2, \ldots \} = \underbrace{\{00..00, 00..01, \ldots, 11..11\}}_{\text{The set of all 64-bit keys}}$$
##### **Definitions: Empty Program vs. Empty Trace**
- **$\emptyset$ (Empty Set of Traces):** A program that never generates an execution (e.g., fails to compile or link).
- **$\{\epsilon\}$ (Set containing Empty Trace):** A program that runs successfully but performs no observable actions (length 0 trace).
##### **Proof: $P_2$ is NOT a Property**

**Theorem:** $P_2$ (the policy requiring output of _all_ 64-bit keys) is not a property.

**Proof:**
By Way of Contradiction (BWOC), assume $P_2$ is a property.
Then there exists a set of valid traces $G_2$ such that $\forall p: p \in P_2 \iff p \subseteq G_2$.
1. Let $p_{all}$ be a program that outputs **all** possible keys.
    - $p_{all} \in P_2 \implies p_{all} \subseteq G_2$.
2. Let $p_{sub} \subset p_{all}$ be a program containing only **one** trace from $p_{all}$.
    - Since $p_{sub} \subset p_{all}$ and $p_{all} \subseteq G_2$, then $p_{sub} \subseteq G_2$.
3. By definition of a property, $p_{sub} \subseteq G_2 \implies p_{sub} \in P_2$.
4. **Contradiction ($\bot$):** $p_{sub}$ only outputs one key, but $P_2$ requires _all_ keys. Thus $p_{sub} \notin P_2$.
**Conclusion:** $P_2$ is not a property.

---
#### ...

##### CIA classification of properties is used in information security (and other areas of computer security)
> [!NOTE] Note
> - Subject/Principals:
> 	 - actions, e.g. users, processes, threads, etc.
> - Objects/Resources
> 	- passive

- Confidentiality
	- Some subjects may not **read** some objects. 
- Integrity
	- Some subjects may not **write** some objects.
- Accessibility
	- Some subjects must be available or be able to accessed.

Confidentiality and integrity polices are completely independent of each other

epsilon is a prefix of any trace

### Safety properties 
- If you want a policy in place, you want it to be a safety properties
- We know how to enforce these well
- 
- Bad traces are irremediable
- 
### Liveness
- any problem can be fixed