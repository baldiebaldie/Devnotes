Data Structures
04-21-2025

### Overview 
Graphs in the context of data structures are networks of relationships
- contains a set of n objects 
- has V vertices (just like a node!!)
- has E edges which connect two vertices together
Examples of data structure graphs are
- The internet
- Social media 
- Topology networks: roads, computer networks, etc. 
### Essential terminology
- Order: the number of vertices in the graph (n or v)
- Size: number of edges in the graph (m or e)
- Max # of edges: ($^n _2$) = O(n$^2$) 
- Adjacent: Two vertices connected with an edge
- Incident: Vertex and an edge where the vertex is an endpoint
- Degree: The number of vertices adjacent to a given vertex (deg(v))
![](../images/GraphDiagram.png)

How can we represent a graph as a data structure?
Four notable represntations:
1. Edge list
2. Adjacent matrix
3. Adjecent list 
4. Incidence matrix 

### Edge list
>Represent all of the edges in the graph

- May be sorted or unsorted 
- Edges must be undirected meaning that there is no direction of edges between nodes. If there was a direction V$_0$ --> V$_1$  but not V$_1$ --> V$_0$ 
![](../images/EdgeList.png)

#### Edge list operations:
-  AddVertex()  
	- Increment order  
	-  O(1)  
- RemoveVertex(v)  
	- Scan list  
	- Remove all edges with v  
	- O(m)  
- AddEdge(u, v)  
	- Append (u, v)  
	- O(1)  
- RemoveEdge(u, v)  
	- Remove (u, v)  
	- O(m)  
- IsAdjacent(u, v)  
	- Scan list for (u, v)  
	- O(m)  
- GetNeighbors(v)  
	- Scan list for (v, *) or (*, v)  
	- O(m)

### Adjacency Matrix 
- We have an n by n matrix
- If two given vertices are adjacent, then it is denoted by a 1 and 0 otherwise.
![](../images/AdjacentMatrixDiagram.png)
- Note if the matrix is always symmetrical for undirected graphs...
	- Therefore you only need to store the lower triangle of the matrix to optimize the matrix
- Also adjacency matrices can support weight (the "cost" of links). Instead of putting just 1 to indicate adjacency just use the cost of the link instead.
#### Adjacency matrix operations: 
- AddVertex()  
	- Add row and column to matrix  
	- O(n2) or O(n)  
- RemoveVertex(v)  
	- Remove row and column from matrix  
	- Shift elements left or up  
	- O(n2)  
- AddEdge(u, v)  
	- a$_u$$_v$ = a$_u$$_v$ = 1  
	- O(1)  
- RemoveEdge(u, v)  
	- a$_u$$_v$  = a$_u$$_v$  = 0  
	- O(1)  

### Graph traversal 
#### Terminology 
- Walk: Sequence of vertices joined by edges
	- if directed must obey directionality of edge
	- The length of a walk is the number of edges (This could be infinite because can loop over the same vertices and edges)
- Path: walk with no repeated vertices or edges
- Circuit: walk that begins and ends at the same vertex (A.K.A a closed walk)
- Cycle: nontrivial path that begins and end at the same vertex
	- Not the difference from a cycle and ciruit is that a circuit is a wzalk and a cycle is a path
- Connected: two vertices that have a path between them
- Component: Maximal set of connected vertices in a graph
	- Maximal = cannot be enlarged by adding more vertices
	![](../images/componentsExample.png)
#### BFS
Breadth first search (BFS)
- Queue-based traversal  
- Resembles level-wise BinarySearchTree traversal  
- Start at first vertex and add to queue  
- Dequeue vertex and add unvisited neighbors to queue  
- Continue until queue is empty  
- If there is more than one component, repeat on next
The psuedo code works as followed:
1.  Mark all vertices as undiscovered  
2. Add node to queue  
3. Mark node as discovered  
4. While queue is not empty:  
	- Dequeue vertex v  
	- Mark v as explored (optional)  
	- Enqueue undiscovered neighbors of v   
	- Mark those neighbors as discovered  
5. If any vertex still undiscovered, choose one and restart
[Visual representation of BFS](https://visualgo.net/en/dfsbfs)
The idea is you want to explore all of your neighbors before looking at other neighbors neighbors
