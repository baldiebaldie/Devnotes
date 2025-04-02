 Computer Logic and Design
03-06-2025

## Falling edge triggered D flip flop

Falling edge D-flip flops "remember" the value between clock transitions
 ![D latch flip flop](/images/dLatchFlipFlop.png)

- If G = 0 then Q^+ = Q (Q will be the same value as long as the latch is inactive)
- If G = 1 then Q^+ = D or P becomes D (P is allowed to change because the latch is active)
 - A D flip flop can be constructed from two d-latches 
 - The invertor on the clock makes sure that only one of the latches are active at one time
- makes more sense when you look at the timing diagram
 ---
    Timing diagram questions will be on exam!! On the exam itself all you have to do is draw out the outuputs on the timing diagram
 ---
 

 ## Setup time
 >Setup time: The amount of time required for the input of a flip flop to be stable before the active clock edge occurs. 

 ![setup time](/images/setupTime.png)
 If setuptime is not satisfied, you might get some unstable behavior from the circuit.

 - In (b) FlipFlop delay + Inverter delay + setup time = 10ns. The total clock lasts for 9ns.
 Total time < total delay so there will be issues.

 ## S-R flip flops timing diagram example

 ![SR example](/images/SRexample.png)

 at t1, S1=S=1; R1=R=0; P = 1, S2 = 0, R2 = 1, so therefore Q = 0 and Q' = 1

 ## J-K flip flop timing diagram example

 ![](/images/JKflipFlopExample.png)

- JK is the same as a S-R flip flpo but the forbidden output is allowed. (Q^+ = Q')

(Q^+) = J Q'+K' Q

![](/images/JKfullCircuit.png)

## T Flip Flop
![](/images/TflipFlop.png)

- The main difference with a T flip flop is it has a toggle input. When toggle is 1 then the clock signal is applied. If toggle is Low then the clock signal is not applied
