Computer Logic and Design
03-04-2025

## Sequential logic circuit block design


![block circuit](/images/sequentialBlockCircuit.png)




The diagram illustrates the fundamental structure of a sequential logic circuit with two main components:

Combinational Logic (blue box): Receives inputs labeled x₁ through xₙ and produces outputs z₁ through zₘ.
Memory (gray box): Stores the current state of the system.

The key feature of sequential circuits is the feedback loop, where:

Current state variables (y₁ through yᵣ) feed into the combinational logic
The combinational logic produces next state variables (Y₁ through Yᵣ)
These next state variables feed back into the memory element
The memory element then updates the current state for the next cycle

This feedback mechanism is what gives sequential circuits their "memory" capability, allowing them to make decisions based not just on current inputs (like combinational circuits) but also on previous inputs and states.

#
Here is a simple example problem here:
The example input is 011010
![clock example](/images/clockExample.png)

Note that the 1/1 means a few things in this state diagram. the first number indicates which number will cause a transition and the second number is what the output state will change to. For example if state z starts at 0 and starts at state B. If x = 1 then B transitions to A as well as changing the state of Z to 1.

Also note that the state can only change depending on the falling edge of the clock cycle. For example look at T3-T4, although it seems that the state from A will go to B although it goes to C.
	

## Latches

![flip flop](/images/FlipFlop.png)
This indicates a S-R latch as well as the timing diagram. What this does is it flips the output each and every time. In the table below all of the circled states are considered stable.
A stable state can run the circuit again and again and the inputs as well as the outputs remain the exact same.

if we change the value but no change in output the state is stable. if we change in input value and the output value changes the state is unstable.

![truth table](/images/TruthTable.png)

A gated S-R latch is a S-R latch with an additional enable input. If enable is 0 then the states stay the same, if 1 then they are allowed to change.

The next state equation becomes
Q^+ = SG + Q(R' + G')
Where G is the enable input

The way to include this "Gate" is to use 2 more NAND gates initially as well as the enable input.
![Gate](/images/Gate.png)


There are such latches called a D latch which essentially makes sure that if S=1 then R=0 and if S=0 Then R=1. The will never be the same

![d latch](/images/dLatch.png)

Therefore, the condition S=R=1 will never happen.

Note: if G=0 the next state will always be the same. 
if G=1 then the output will always flip

This is also reffered to as the **"transparent latch"**.

 
