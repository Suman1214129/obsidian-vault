Some dotes (nodes) and lines (edges) connecting those nodes.

***Why we study graph (dots and lines)?***
Graph is simple representation (abstraction) of reality.
Graphs are tools to *solve problem*.

- **Directed graphs:** Self loop considered.
- **Undirected graphs:** Self loop not considered.

*99% of the time we will only study* ***simple graphs***

	Simple graphs are undirected, have no self loop and no multiple edges.


**Degree of a vertex:** 
The degree of a vertex in a undirected graph is number of edges incident with it.

Isolated degree: degree number is *zero*

**Order of graph:** number of vertex:  |  |

**Sum of Total degree** = 2 |E|

**Connected vertices:** There is a path between them.
**Adjacent vertices:** There is a edge between them.

**Length of graph:** Number of edges.

**Connected graph:** In which we can travel to one vertices  to another with path.
**Cyclic graph:** Completes a cycle.


***Note:*** 
If we delete any vertices (b) from graph then
Number of edges |E'| = |E| - Degree(b)

***An important result:*** In every undirected graph with |V| >= 2, At least 2 vertices have *same degree*.

	It is impossible for every vertex of a graph to have a different degree.


#### Special type of simple graph:
A **Regular graph** is a simple graph in which each vertices have same number of degrees
<img src="../Images/Pasted%20image%2020260812180712.png" width="614" alt="">

**Number of edges** in complex graph:
 d = 2|E|
 n(n-1)= 2E
 *E = n(n-1) / 2*


***Path graph*** is nothing but a line sequence of vertices connected to exactly one edges and form no cycle.
*Number of edge = n-1*

***Cycle graph*** is simple graph which makes a cycle.
*Number of edge = n*


#### Hypercube Graph Q(n):
**Vertices (V):** Set of all n-length binary strings.
**Edges (E):** Two vertices are adjacent if and only if they differ in exactly one bit position.

<img src="../Images/Pasted%20image%2020260812183236.png" width="420" alt="">

Number of vertices in hypercube graph = $$2^n$$Number of degree in hypercube graph = **n**
Number of edges in hypercube graph = n * 2^n-1

<img src="../Images/Pasted%20image%2020260812184944.png" width="429" alt="">


***Note:***
For any two graphs G,H:
If some graph invariant property *mismatch* -> Not isomorphic
If some graph invariant property *match* -> May or may not be isomorphic

***Complement of graph*** is the jealous graph which doesn't like the currently edges and inverse them.

#### Self complimentary graph:
A self-complimentary graph is a graph G that is* isomorphic* to its complimentary graph.

Graph G is self-complimentary graph if *G = G'*
<img src="../Images/Pasted%20image%2020260812193127.png" width="358" alt="">


Number of edges in **self complimentary** graph will be *n(n-1) / 4 *

<img src="../Images/Pasted%20image%2020260812195211.png" width="476" alt="">

**Good question:**
![](../Images/Pasted%20image%2020260816134029.png)
Answer: c


#### Bipartite graph
A graph is bipartite if and only if it doesn't have *odd cycle.*
<img src="../Images/Pasted%20image%2020260816135324.png" width="393" alt="">

In **complete bipartite graph**, every element of first part is adjacent to second part. *K(m,n)*

In **Star bipartite graph**, first part has one element and second part has n element that forms a star like structure.

**Parity of bitstring** is even or odd determined by *number of 1's* in the string.

***NOTE:*** We create parts like even parity and odd parity for *hypercube bipartite.*


#### Cyclic & Acyclic graphs
**Cyclic graph** which consists at least one cycle and similarly **acyclic** doesn't consist any graph.

**Trees** are connected acyclic graphs. *(connected forest)*.

If there is tree of *n vertices* then it will have **(n-1) Edges.**

	A graph with n vertices and m edges at least **n-m** connected components.


#### Planer Graph:
A graph is a planer graph if there is some way to draw it in 2-D plane without any of the *edges crossing.*

Non planer graphs: *K(3,3), k5*

**Faces** in planer graphs are which are surrounded by edges with one outer face.

	Every tree is planer graph and has exactly one face.

*Degree of faces* are how many side of edges they are touching.

*Summation* of degree of faces = 2|E|

<img src="../Images/Pasted%20image%2020260817110849.png" width="469" alt="">

#### Euler's Formula for Planer graph
Euler said that all planer representation of graphs will have *same number of faces.*

***Let G be a planer graph:***
**C** = number of connected components
**F** = number of faces
**E** = number of edges
**V** = number of vertices

**Euler's formula:** V+F = E+C+1

	If the graph isn't in planer reprsenation then we don't have to make it planer instead we can use euler's formula.


#### Graph colouring
**Vertex colouring:** Color the vertices of G in such a way that *no two adjacent vertices receive the same color*. A graph is k-colourable if it admits a vertex coloring with at most k colors.

***Chromatic number (X-chi):*** *Minimum* number of colours needed to colour graph G.

In **complete graph** can't use *re-use* any colour.
X: **Kn** = n

***Note:*** Every graph is *d+1* colourable, where d is maximum degree.

In **Cycle graph**, chromatic number pattern:
	X:  **C(even)** = 2
	X:  **C(odd)** = 3

In **Bipartite graph**, X will be **<=2** or at-most 2.
If there is at least one edge then the *X = 2.*
And these graphs are *2 colourable.*


**Greedy algorithm:** This algorithm used to colour the vertices but doesn't necessarily tell the minimum colours required (chromatic number).

	At stage k: look at v(k), and colour it the smallest colour in N not yet used on any of v(k)'s neighbors.

Chromatic number in greedy algorithm: *<=K*

**Theorem (Four-colour Theorem):**  Every planar graph is 4-colourable.

***Guidelines to find Chromatic number:***
1. A graph G is Bipartite if and only if *X(G) <= 2*
2. If graph G does not have any odd length cycle then G is Bipartite, So *X(G) <=2*
3. *X(G) <= d + 1*, where d is maximum degree

***Edge colouring:*** 
Every adjacent edges have different colours.

**Vizing's theorem**
For any simple graph, *X'(G) = d(G)* or *d(G) + 1.*

	In general X'(G) >= d(G), since all edges on a max degree vertex must have different color

#### Graph realization problem: 
Given a List of non-negative integers, determine if there exists a simple undirected graph whose *degree sequence is L.*

***Havel-Hakimi*** is a graph theory method used to decide if a given sequence of numbers can form a *simple graph.*

***Note:*** Maximum number of edges in planer graph: **3n - 6**

	where each face have degree = 3, connected graph = 1