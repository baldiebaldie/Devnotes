Computer networks
03-25-2025
### 5.2 Routing algorithms
The goal of any routing algo is to find "good paths" from the sending host to the receiver. But what does it mean to be good?
>Path: sequence of router packets going from source to destination

>Good: Could mean different things. Could mean the least "cost" path, "fastest" path, or even "least congested" path.

Graphs are used to show paths. This is the notation here. ![[Graph.png]]

The link cost between two nodes that aren't connecting is infinity.
Routing algorithms can be defined by 4 things on a spectrum.
Whether or not they are global/decentralized or whether or not static/dynamic.

>Global: All routers have complete topology. They know what their costs are through link state algorithms.

>Decentralized: The calculation of link path is calculated iterative through each router 

---
>Static: Algorithm changes slowly over time

>Dynamic: Algorithm changes quickly over time

Dijkstras link state algo:
- Centralized: Link cost known to ALL nodes and all nodes have the same info
- Lowest cost nodes are connected to all other nodes via forwarding table
- The algorithm complexity:
	- O(n^2) average
	- O(nlogn) best
- The message complexity
	- O(n)