Based on the handwritten and typed notes you uploaded, here is a focused, high-yield review sheet for your Secure Coding exam.

This covers the formal definitions, proofs, property distinctions (Safety vs. Liveness), and threat models found in your materials.

### **Part 1: Core Definitions & Foundations**

#### **1. Policy vs. Mechanism**

- **Policy (The Law):** Defines the "what." Constraints on behavior.
    
    - _Abstract:_ A predicate $P$ returns `true` (good) or `false` (bad) for a program.
        
    - Formally: A policy is a set of **programs** (where a program is a set of traces).
        
- **Mechanism (The Enforcer):** The concrete tool or implementation that ensures the software adheres to the policy.
    
    - **Static Mechanism:** Checks code _before_ execution (e.g., compiler, linting). Always imprecise (undecidable).
        
    - **Dynamic Mechanism:** Checks code _during_ execution (e.g., Firewall, IDS).
        
    - **Relationship:** Many-to-Many. Multiple mechanisms can enforce one policy; one mechanism can enforce parts of many policies.
        

#### **2. Evaluation Metrics (The Matrix)**

When a tool scans for vulnerabilities, there are four outcomes:

- **True Positive (+):** Tool says "Vulnerability exists," and it **does**. (Correct)
    
- **True Negative (-):** Tool says "Safe," and it **is**. (Correct)
    
- **False Positive (Type I Error):** Tool says "Vulnerability exists," but code is **safe**. (False Alarm).
    
- **False Negative (Type II Error):** Tool says "Safe," but a **vulnerability exists**. (Missed Danger).
    

#### **3. Soundness vs. Completeness**

- **Soundness:** "No False Negatives."
    
    - If it says the code is safe, it is **guaranteed** to be safe.
        
    - _Trade-off:_ It will have many False Positives (flags things that might be safe just to be sure).
        
    - _Extreme:_ A tool that flags **everything** as an error is sound (it never misses an error).
        
- **Completeness:** "No False Positives."
    
    - If it reports an error, it is **guaranteed** to be a real error.
        
    - _Trade-off:_ It will have False Negatives (it will miss bugs it isn't 100% sure about).
        
    - _Extreme:_ A tool that approves **everything** is complete (it never raises a false alarm).
        

> **Exam Tip:** Ideally, we want both, but that is impossible (undecidable). In security, we usually prefer **Soundness** (we'd rather fix false alarms than miss a hack).

---

### **Part 2: Formal Definitions (Traces & Properties)**

#### **1. Program & Traces**

- **Trace ($t$):** A sequence of security-relevant events (e.g., `input(a); output(b)`).
    
    - $\epsilon$: Empty trace (program started, nothing happened).
        
    - **Divergence:** An infinite trace (program never stops).
        
- **Program ($p$):** A set of traces ($p = \{t_1, t_2, ...\}$).
    
- **Policy ($P$):** A set of valid programs.
    

#### **2. What is a "Property"?**

Not all policies are properties.

- **Definition:** A policy $P$ is a property **if and only if** we can determine if a program is "good" just by looking at its individual traces.
    
- **Formal:** $P$ is a property $\iff \exists$ a set of good traces $G$ such that $\forall p: p \in P \iff p \subseteq G$.
    
    - _Translation:_ A program is good if and only if **every single one** of its traces is in the "Good Trace Set" ($G$).
        

#### **3. Example of a Non-Property (The $P_2$ Proof)**

- **Scenario:** Policy $P_2$ requires a program to output **all** 64-bit keys.
    
- **Why it fails:**
    
    - Trace $t_1$ outputs Key A. Is $t_1$ good? You don't know. It's good _if_ the program also outputs Key B, C, etc.
        
    - Because the "goodness" of a trace depends on the existence of _other_ traces, $P_2$ is **not** a property.
        
    - _Proof Summary:_ If you assume $P_2$ is a property, you reach a contradiction where a subset program (outputting only 1 key) would have to be accepted, but the policy rejects it.
        

---

### **Part 3: Safety vs. Liveness (The Big Two)**

Every property can be decomposed into: **Property = Safety $\cap$ Liveness**

|**Feature**|**Safety Property (GS​)**|**Liveness Property (GL​)**|
|---|---|---|
|**Motto**|"Bad things **never** happen."|"Good things **eventually** happen."|
|**Detection**|Can be detected in finite time (counterexample).|Cannot be violated by a finite trace (violation requires infinite time).|
|**Remediation**|**Irremediable.** Once a "bad prefix" occurs, the trace is dead. You cannot append anything to fix it.|**Fixable.** Any finite trace can technically be appended with more events to eventually satisfy the goal.|
|**Example**|"Read(0) must never occur."|"If `req(i)` occurs, `send(i)` must eventually occur."|
|**Logic**|If trace $t$ is valid, all its prefixes must be valid.|If trace $t$ is finite, it is always considered "potentially valid" (prefix).|

#### **The Decomposition Theorem**

For any property $G$, there exist $G_S$ (Safety) and $G_L$ (Liveness) such that $G = G_S \cap G_L$.

- **$G_S$ (Safety Part):** Includes traces that are "fixable" according to $G$ (even if not currently in $G$) + valid infinite traces.
    
- **$G_L$ (Liveness Part):** Includes all valid traces in $G$ + all "unfixable" traces (essentially assuming the good thing _might_ happen unless proven otherwise by infinity).
    

---

### **Part 4: Threats & Attacks**

#### **1. The CIA Triad (Security Goals)**

- **Confidentiality:** Subjects cannot **read** objects they shouldn't.
    
- **Integrity:** Subjects cannot **write/modify** objects they shouldn't.
    
- **Accessibility (Availability):** Resources must be available to legitimate users.
    

#### **2. "Trust" Issues**

The root cause of many vulnerabilities is software **trusting** user input or the environment too much.

- **Buffer Overflow:** Attacker provides input longer than the allocated memory space, overwriting adjacent memory (code injection).
    
- **SQL Injection:** Attacker provides input that alters the database query logic (e.g., `' OR 1=1 --`).
    
- **Confused Deputy:** A program with high privileges (the deputy) is tricked by a low-privilege user into misusing its authority (e.g., a compiler overwriting a system file because the user asked it to output there).
    

#### **3. Specific Attack Types**

- **Social Engineering:**
    
    - _Phishing:_ Fake emails (bank).
        
    - _Smishing:_ SMS phishing.
        
    - _Spear Phishing:_ Targeted attacks (using specific info about the victim).
        
- **Denial of Service (DoS):** Exhausting resources (bandwidth, battery, processing) so legitimate users cannot access the system.
    
    - _Mitigation:_ Auditing, logging, and rate-limiting.
        

---

### **Part 5: Logic & Trade-offs**

#### **Logic Laws for Policy**

- **Implication:** $A \implies B \equiv \neg A \lor B$.
    
- **Vacuous Truth:** A statement "All X are Y" is automatically true if there are no X's. (e.g., "All infinite traces are good" is true for a program that only halts).
    

#### **The Security Trade-offs**

Security is never free. It trades off against:

1. **Usability:** (e.g., annoying pop-ups vs. silent security; complex passwords vs. user memory).
    
2. **Performance:** (e.g., runtime overhead of bounds checking).
    
3. **Resources:** Bandwidth, Data size, Battery power (encryption consumes energy).
    

> **Next Step:** Would you like me to generate a specific practice problem for the "$P_2$ is not a property" proof or the Safety/Liveness distinction to test your readiness?


i + x = target
target - x = i {
	ans.push()
}