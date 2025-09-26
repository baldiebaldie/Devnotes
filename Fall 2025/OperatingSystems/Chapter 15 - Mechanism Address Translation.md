Operating systems
09-22-2025

### What is address translation?
- Address translation is like having a mail forwarding service. When a program thinks it's sending mail to "123 main street," the post office secretly forwards it to the real address "456 Oak avenue"
- All programs think they live at address 0, but actually live somewhere else in physical memory. Address translation converts the program's "virtual addresses" to the real "physical addresses" automatically.
- ---
### Why do we need address translation?
- Efficiency: Making address translation happens fast