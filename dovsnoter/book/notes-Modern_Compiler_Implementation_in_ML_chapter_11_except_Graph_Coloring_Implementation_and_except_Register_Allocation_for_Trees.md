When doing register allocation One derrives an *interference graph*. In such a graph each node represents a temporary value and each edge, indicates a pair of tempoaries that cannot be assigned to the same register.
After the interference graph is made one can start to color the graph. Here one wants to use as few colors as possible, but not have the same color on two nodes that share an edge.

When coloring the graph one say that one have K-colors. Where K is the number of registers in the machine. If one can color the graph with K-colors, then the program can be run using the registers.
If K-Coloring is not possible, then that means that some of the temporary must be stored in the memory instead of registers. This is called *spilling*.

## Coloring by Simplification
The problems of coloring a graph this way is NP-complete. However there is an algorithm that can help solve the problem. This algorithm consists of the following steps:
**Build**:
Construct the interference graph

**Simplify**
Remove nodes that have fewer than k neighbors. Call this graph G'. If you can color G' you can color G, since m's neighbors at maximum uses K-1 colors.

**Spill**

If one at one point end up with a graph G with nodes only having edges where $e \geq K$, we pick some node at mark it for spilling.
One can afterwards start the simplification steps again

**Select**
When one end with the empty graph, one starts coloring. The coloring is done by slowly building the graph once again one node at the time. If one ends up in a situation where one cant color a node, we end up with an actual spill.

**Start Over**
If the select phase fails the program must be rewritten and the algorithm is then done on the rewritten program.

## Coalescing
![[Briggs and George Coalesced - page 232.png]]

## Caller-save and Callee-save registers.
Caller-save:
The one calling the function have it in a safe register.

Callee-save:
The one called stores the values safely.


