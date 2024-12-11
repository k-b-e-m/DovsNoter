
# Garbage collection
The collection/cleanup of dead/unreachable memory. This means any variables that no longer has a pointer pointing to it, should be removed as the program has no way of getting access to it again. 

## Mark and Sweep Collection

### The General Approach
Here the idea it to start with some root variables, and then do a graph traversal to find all of the reachable nodes with something like a DFS, and then ***mark*** them as reachable. This now leaves all of the nodes which cannot be reached. Therefore the heap can now be ***Swept*** for any dead nodes, when one is found it is added to a freelist, all of the marked alive nodes are unmarked to prepare for next iteration. Then the freelist can be freed.

### Cost of garbage collection with Mark And Sweep
The time complexity of the DFS algorithm is proportional to the number reachable nodes. The Sweep is proportional to the size of the heap. Therefor the cost can be described as below. 

$$
\begin{align*}
\text{Heap} &= H\\
\text{Reachable Words} &=  R\\
\text{Amortized cost of DFS} &= c_1\\
\text{Amortized cost of Sweep} &= c_2\\
\text{Cost of one iteration} &= c_{1}R + c_{2}H\\\\
\text{Amortized cost of Garbage collection} &= \frac{c_{1}R + c_{2}H}{H-R}
\end{align*}
$$
This is the cost it takes pr word to delete, with the garbage collection. It can be seen that when the size of R gets close to H, the cost pr work gets very large. Else if H is much larger then R then the cost can get very low. 

### Stack instead of recursion
In the worst case the size of the longest path could be the size of H, which would cause the algorithm store H activation records. Therefore instead of recursion there could be used an explicit stack, so it wont take up so much heap memory by only allocating H words.

### Pointer reversal
When a field of x has been seen, i would never be looked at again. therefore $x.f_{i}$ could be used to store one element of the stack. Here $x.f_{i}$ could be made to point back to the value from which it was reached. 


## Reference counts
Here each record keeps track of how many pointers are pointing to it. The compiler emits extra instructions to keep track of these counters. When a counter hits zero the related record is then added to the freelist. 

Generally it is better to do the increment of the counters when a record is removed from the freelist, because of the below 2 reasons. 
- [ ] Breaks up the workload into shorter pieces, so the program runs smoothly. 
- [ ] The compiler must emit code to check whether the count has reached zero and put the record on the freelist, but the recursive decrementing will be done only in one place, in the allocator. 

But while this seems simple and attractive there are 2 major problems
- [ ] Cycles of garbage cannot be reclaimed. If for example there are 2 records point in a loop, then there counter would never become zero, and would therefore not be collected, even though they are unreachable. 
- [ ] The incrementing of the counters gets very expensive when a lot of them are done. 


## Copying collection

### The General approach
Here the heap us divided up into 2 equal parts *From-space* and *To-space*, Then first the from space is just added onto as normally when the program is running. Then eventually when the from space gets filled up, it then copies all of the alive memory to the to space and frees all of the from space, this now leaves only the alive memory in the to space, and fries all of the from space. 

### Cheney's algorithm
To find all of the alive data, BFS is used to go from the root out into the chains. The problem here is it would require some memory space to store a stack, or use pointer reversal. Therefore the approach as below is used. 
![[Chapter 13 - Garbage collection.png]]
### Locality of reference
Because of the way caching and virtual memory works, it is important to have good locality of reference. Because records that are related would be in addresses close to each other, which makes the cach miss less. Therefore it is best to have depth first search to find the element to copy, but it requires pointer reversal. This then introduced the hybrid approach below. 

![[Chapter 13 -Algo 13.11 semi-depth-first forwarding.png]]

### Cost of garbage collection

The cost can be described as below with the same notation from before. 

$$
\text{Ammortized cost of collection} = \frac{c_{3}R}{\frac{H}{2}-R}
$$
The main difference here is that there is no sweep phase, and the heap is divided up into 2 equal parts.

## Generational Collection

### The general approach
This approach comes from the observation that old data tends to live longer then newly create data. Therefore the data is separated into *Generations* based on how long it has been alive. The groups are named $G_{0}, G_{1},...$ where the new objects are in the smallest groups. To find all of the objects, both *Depth-first marking* and *breadth-first copying* could be used to find the elements of the $G_{0}$. 

### Remembered list
It is also necessary to keep track of a list of objects from the older generations which has a pointer to the young objects. to make sure they don't get freed while still in use. For this a remembered list is used. There are 4 general approaches.
- Remembered List - The compiler generated code after each update store to add them to a vector with *updated objects*, which then will at clean up time get checked to see if they still has a pointer into $G_{0}$
- Remembered Set - The compiler still adds the object to a vector after update to some field in the object, but now it also stores a bit to say if it has already been added, this removes the need to add duplicates to the vector. 
- Card Marking - divide memory up into logical cards of size $2^{k}$ bytes, and mark the card if there has been an update in that part of memory. 
- Page Marking - Very weird and complicated method, kind of like card marking but uses some specific OS operations to restrict access. (Highly unlikely they will ask this deep on the exam.)

Then the program is made to only cleanup $G_{0}$ and the free list often, and the older a generations is, the more iterations it can wait to be cleaned up. When one of the older generations is cleaned up it would also be smart to clean up all from the generation selected down to the youngest generation, because they would typically point to each other. 

### Cost of garbage collection
With this approach there are 2 many parameters to be able to make a ammortized cost, but generally this is the cheapest option out of all of the discussed solutions. 


