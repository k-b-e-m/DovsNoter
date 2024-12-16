Here is a list on optimazationz:
Safety
Constant folding
Algebraic simplification
	-Strength reduction
Constant propagation
Copy Propagation
Dead code elimination
Inlining and specialization
	Recursive function inlining
Tail call elimination
Common subexpression elimination

# Why do we need optimizations?
Thinking about optimal code as a programmer is just adding more details to coding, making it harder. The idea is that programmers dont write the optimal code. Optimazations help the programmers code in high level langauge and having the compiler optimize the code for us.

*Example of optimazations*
code :
``` Python
A[i][j]=A[i][j]+1

```
Optimizations ideas:
Instead of retrieving the same field twice, one can optimize it by instead retrieving the field once.

There are three different kinds of optimizations:
**Time:** Improve execution speed
**Space:** reduce amount of memory needed
**Power:** lower power consumption



# Constant Folding
If operands are known at compile time, perform the operation staticly. '
*Example:*
``` java
int x = (2+3) * y /*->*/ int x = 5*y
```
This improves the:
Time, Power and Space.

This optimizations can be applied in the llvm generation of the code (codegen)

*Note:*
*Most optimizations is done after the semantic analysis*

When is Constant Folding safe?
For Booleans its always safe
For integers nearly always
 Could be overflows or division by zero.
Floats - use with caution
	Example for error could be float.
General notes about 


## Algebraic Simplification
this build upon the **Constant folding**. Here are the rules for the algebraic simplification:
$$
\begin{split}
a \cdot 1 \rightarrow a\\
a \cdot 0 \rightarrow 0\\
a + 0 \rightarrow a\\
a - 0 \rightarrow a\\
b | false \rightarrow b\\
b\ \&\ true \rightarrow b\\
\end{split}
$$

also:
$$
\begin{split}
(a+b)+c \rightarrow a+(b+c)
a+b \rightarrow b+c
\end{split}
$$

# Strength Reduction
Replace expensive operations with cheap operations.
For example
$$
a\cdot 4 \rightarrow a<<2
$$
$$
a\cdot 7 \rightarrow (a<<3) -a
$$
$$
a/32787 \rightarrow (a>>15)+(a>>30)
$$

# Constant Propagation
If the value of a variable is known to be a constant, replace the use of the variable by that consant.
Example:
![[Example for constant propagation.png]]


Constant propagation requires a data-flow analysis.

# Dead Code Elimination
If a side-effect free statement can never be observered, it is safe to eliminate the statement.
![[Example Dead code elimination.png]]

Is safe if code that are remove do not having any side effects. Example of side effects:
Raising an exception, modifying a global variable, going into an infinite loop, printing to standard output, sending a network packet, launching a rocket,....

# Unreachable code Elimination
Basic blocks not reachable by any trace leading from the starting basic block are unreachable and can be deleted.

Improves instruction cache utilization and size of the program.

# Common Sub expression Elimination

Replace an expression with previously stored evaluations of that expression. For example:

```Python
[a+i*4]=[a+i*4]+1
# Can be transformed to
t = a+i*4
[t] = [t]+1
``` 

For safety one must ensure that the expressions always have the same value in both places.

Here is an example of an unsafe transformation:
![[Example of unsafe common subexpression Elimination.png]]
This is unsafe since the value at $a[i]$ is changed. $c[k]$ will therefore have a different result. In the case that a and b can be the same array.


This optimization almost always improves performance, however one will use a register to store the common sub-expression, therefore it is sometimes cheaper to not do this.

# Loop-invariant code motion
hoist invariant code out of a loop.
example:
![[Example of loop-invariant code motion.png]]

It improves instruction cache.

It is not always safe. the instructions are performed whether b is true or not, meaning that it could trigger an exception like division by zero.

# Loop Unrolling
Replace the body of a loop by several copies of body and adjust the loop-control code.
![[Example of loop unrolling.png]]
This gives half the loop jumps. This example does 50 comparisons instead of 100 jumps.
This improves performance but affect space, since program will be larger.

# Inlining
Replace a call to a function with function body.
![[Example for inlining.png]]
Eliminates the stack manipulation and removes the jump.

Needs to be aware of side effects.
This optimization can be done after the semantic analysis.

## Inlining recursive functions.
![[recursive functions inlining 1.png]]
![[recursive function inlining part 2.png]]
![[recursive inlining 3.png]]
![[When to inline.png]]
## Tail call Elimination
![[Tail call elimination.png]]

# When can different Optimizations Apply:
![[When to apply optimizaations.png]]

___
*Other lecture starts*

Definition of a loop:
For any to nodes in S there is a path from one to the other using only nodes in S.
There is a distinguished header $h\in S$ such that there is no edge from a node outside S to S\{h}$

Examples

![[Example identifying loops.png]]
Orange is header, yellow is S. Not that the 3rd have two loops and therefore 2 headers. Purple and orange is headers.

Another example:
![[Loop example 2.png]]

B and C cannot be loop, since there is no clear head.


## Finding loops
**Definition:**
node d dominates node n if every path from the start node to n must go through d.
**Definition:**
an edge from n to a dominator d is called a back-edge-
**Definition:**
a **loop** of a back edge n->d is the set of nodes x such that d dominates x and there is a path from x to n including d.

Therefor one finds loops by figuring out dominators and identify back edges.


![[Example domination.png]]
Dominator trees:
![[Dominator tree.png]]

**Definition:**
If loops A and B have distinct headers and all nodes in B are in A then we say B is **nested** in a.
**Definition:**
An inner loop is a nested loop that doesn't contain any other loops.


## Loop Fusion

![[Loop Fusion.png]]

## Loop Fission

![[Loop fission.png]]

## Loop Unrolling

![[Loop Unrolling.png]]


## Loop Interchange
![[Loop Interchange.png]]

## Loop Tiling
![[Loop Tiling.png]]

## Loop Parallelization
![[Loop parallelization.png]]

