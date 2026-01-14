40: beq x1, x2, 16    # Branch to address 72 (40 + 16*2)
44: add x3, x4, x5    # Incorrectly fetched
48: or  x6, x7, x8    # Incorrectly fetched  
52: sub x9, x10, x11  # Incorrectly fetched
...
72: ld  x12, 0(x13)   # Actual target (correct)


### Scenario: Branch Taken (with flush in MEM stage)

| Cycle | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------|---|---|---|---|---|---|---|---|
| `beq x1,x2,16` | IF | ID | EX | **MEM** | WB | | | |
| `add x3,x4,x5` | | IF | ID | **[FLUSH]** | | | | |
| `or x6,x7,x8` | | | IF | **[FLUSH]** | | | | |
| `sub x9,x10,x11` | | | | **[FLUSH]** | | | | |
| `ld x12,0(x13)` | | | | | IF | ID | EX | MEM |

### What Happens During the Flush:

**Cycle 4**: 
- Branch outcome determined in MEM stage → **BRANCH TAKEN**
- Control unit detects misprediction
- **FLUSH ACTION**: Set control signals to 0 for instructions in IF, ID, and EX stages
- PC updated to branch target (address 72)

**Cycle 5**:
- Correct target instruction (`ld`) begins IF stage
- Previously fetched instructions (`add`, `or`, `sub`) are converted to NOPs

## Visual Representation of the Flush:
```
Before Flush (Cycle 3):
Pipeline: [beq|add|or |sub|...]
          EX  ID  IF   -

After Flush (Cycle 4):
Pipeline: [beq|NOP|NOP|NOP|...]
          MEM  -   -   -

New Path (Cycle 5):
Pipeline: [beq| ld|...]
          WB  IF