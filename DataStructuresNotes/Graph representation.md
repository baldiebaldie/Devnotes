Data Structures
04-21-2025

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
		v0     v1    v2.     v3      v4