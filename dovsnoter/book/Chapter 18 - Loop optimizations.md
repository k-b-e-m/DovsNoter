
Loops are a huge part of the runtime of almost all programs. Therefore it would be a be a good spot to optimize. 

They can be technically described like 
"A loop in a control-flow graph is a set of nodes S including a header node h with the following properties:
- From any node in S there is a path of directed edges leading to h
- There is a path of directed edges from h to any node in S
- There is no edge from any node outside S to any node in S other than h
"

# Reducible f£low graphs

The control-flow for a program can typically be reduced down to a more simple control flow as with the example below. 
![[Chapter 18 - Reduce control flow.png]]

Where it can be seen a reduction from 18.2 c to 18.2 a. 

Where figure a is a reduced flow graph, this is because it cannot be reduced any more with any of the concepts used to create it. 

Generally speaking all of the typical programing constructs as *If-then*, *If-then-else*, *while-do*, *repeat-until*, *for* and *break* all create a control flow that can be reduced. As long as there is no *go-to*

# Dominators

Dominators are typically used find the loops in flow graphs. All control flows start in some state $s_{0}$, 

Definition of Dominator
A node *d* dominates a node *n* if every path of directed edges from $s_{0}$ to *n* must go through *d*. Every node dominates itself. 

![[Chapter 18 - Definition Dominantor.png]]


## Immediate Dominators
![[Chapter 18 - Immediate dominators.png]]

## Dominator tree
![[Chapter 18 - Dominator tree.png]]

Dominator trees can be created from a Flow Graph.
## Loops

Loops can be found in the Flow graphs, because a loop is every place where there is a back edge in the flow graph. 

The natural loop of a back edge n -> h where h dominates n, is the set of nodes x such that h dominates x and there is a path from x to n not containing h. The header of this loop would be h.

## Hoisting
The process of taking something that always gives the same out of a loop body, to only do one computation. This could be if 2 constants are getting added together in the body, to be used for something, then instead they could just be computed before the loop. 

Definition is here
![[Chapter 18 - Hoisting-1.png]]


## Loop Unrolling

![[Chapter 18 - Loop unrolling.png]]


