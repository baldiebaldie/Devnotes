Based on the whiteboard images provided, here are the structured markdown notes for your Relational Algebra class.

https://dbis-uibk.github.io/relax/landing
### **Relational Algebra**

#### **I. Domain**
- **Definition:** All relational instances.

#### **II. Operators**
The operators are categorized into **Unary** (operating on one relation) and **Binary** (operating on two relations).
**A. Unary Operators**
1. **Selection** ($\sigma$)
2. **Projection** ($\pi$)
3. **Rename** ($\rho$)

**B. Binary Operators**
4. **Set Operations:**
* **Intersect** ($\cap$)
* **Union** ($\cup$)
* **Set Difference** ($-$)
4. **Product** ($\times$)
* _Also known as Cartesian Product._
4. **Join** ($\bowtie$)
5. **Division** ($\div$)

---
### **Deep Dive: 1. Selection ($\sigma$)**
- **Notation:** $$\sigma_C(T) \rightarrow B$$
    - **$T$**: The input table (Relation).
    - **$C$**: The condition (predicate) used to filter rows.
    - **$B$**: The resulting table (Relation).
    - **$\sigma$**: The selection operator symbol.
- **Logic (Visualized):**
    - The operator looks at every row in Table $T$.
    - It checks the condition $C$ against the row's attributes.
        - If the row satisfies the condition (Checkmark $\checkmark$), it is included in Table $B$.
        - If the row does not satisfy the condition (Cross $X$), it is discarded.
    - _Note:_ Selection filters **rows** (horizontal), not columns.
- **Condition Types:**
    - Comparisons can be made between an attribute and a constant, or between two attributes.
    - Examples shown:
        - $A \text{ op } c$ (Attribute compared to a constant)
        - $A_1 \text{ op } A_2$ (Attribute compared to another attribute)
Cardinality: Number of rows
- C(T1) \* C(T2) = C(T3)
Degree: Number of columns
- D(T1) + D(T2) = D(T3)