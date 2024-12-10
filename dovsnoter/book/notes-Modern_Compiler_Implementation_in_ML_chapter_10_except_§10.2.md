A variable is live if it holds a value that is needed in the future. Therefore one can do a Liveness Analysis.

![[Figure 10.2. Page 212.png]]
Here b is live in 2->3 and 3->4. a is live 1->2 and 4->5->2. C is live the entry of the program. Here one can see that a and b never "live" at the same time, so register 1 can hold a and b and register 2 can hold c. This is because a register can not have 2 variables that are used at the same time.


![[Dynamic vs static liveness page 220.png]]


