---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L04 - Informed and local search
type: quiz
questions: 56
---

# CMPE421 L04 - Informed and local search - quiz

Lesson note: [[CMPE421 L04 - Informed and local search]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Heuristics and best-first search (slides 1-4)

### Q1 (informed-search, slide 1)

The slide says that a heuristic can make search much faster on average. Which two guarantees of the search can you lose, depending on the heuristic?

- A) Optimality and completeness.
- B) Optimality and the memory bound $O(bd)$.
- C) Completeness and the worst-case time bound $O(b^{d})$.
- D) Admissibility and consistency.

> [!success]- Answer
> **A.** With a bad heuristic the solution found may not be the cheapest one (optimality is lost). The search may also find no solution although one exists (completeness is lost). Greedy search shows both problems.
> - B: A best-first search keeps the whole frontier, so it never had an $O(bd)$ memory bound to lose. That bound belongs to depth-first methods such as IDA*.
> - C: A heuristic is used to make the search faster. Losing a time bound is not the risk the slide talks about.
> - D: Admissibility and consistency are properties of the heuristic $h$ itself, not guarantees of the search.

### Q2 (informed-search, slide 2)

The frontier holds three nodes: P with $g=3$, $h=9$; Q with $g=8$, $h=2$; R with $g=5$, $h=4$. Each algorithm removes the node with the lowest $f$. Which node does uniform-cost search remove next, which node does greedy search remove, and which node does A* remove?

- A) UCS: Q, greedy: P, A*: R.
- B) UCS: Q, greedy: P, A*: P.
- C) UCS: P, greedy: Q, A*: P.
- D) UCS: P, greedy: Q, A*: R.

> [!success]- Answer
> **D.** UCS uses $f=g$: P 3, Q 8, R 5, so P. Greedy uses $f=h$: P 9, Q 2, R 4, so Q. A* uses $f=g+h$: P 12, Q 10, R 9, so R.
> - A: This swaps $g$ and $h$: UCS ranked by $h$ (lowest Q 2) and greedy by $g$ (lowest P 3).
> - B: This removes the node with the largest value each time: $g$ 8 (Q), $h$ 9 (P), $g+h$ 12 (P).
> - C: UCS and greedy are right, but for A* this takes the largest $f$, 12 (P), instead of the smallest, 9 (R).

### Q3 (informed-search, slide 3)

A node $x$ is removed from the frontier. Its state is not a goal, and the state is already in `explored`. What does `Graph-Search` do with $x$, and what does `Cautious-Tree-Search` (slide 2, no `explored` set) do?

- A) `Graph-Search` expands $x$ again. `Cautious-Tree-Search` throws $x$ away.
- B) Both throw $x$ away.
- C) Both expand $x$ again.
- D) `Graph-Search` throws $x$ away. `Cautious-Tree-Search` expands $x$ again.

> [!success]- Answer
> **D.** `Graph-Search` checks `explored`: the state was expanded before, so $x$ is thrown away and no children are added. `Cautious-Tree-Search` has no `explored` set, so it expands $x$ again and repeats the work below it.
> - A: This swaps the two algorithms. The `explored` set is what lets `Graph-Search` skip a state.
> - B: `Cautious-Tree-Search` cannot throw $x$ away, because it keeps no record of expanded states.
> - C: `Graph-Search` does not expand a state twice. That is the purpose of `explored`.

### Q4 (informed-search, slide 4)

Use the map and the table. Hirsova and Urziceni are joined by a road of 98 km, and Urziceni is joined to Bucharest by a road of 85 km. Table: $h(\text{Hirsova})=151$, $h(\text{Urziceni})=80$. (a) Is $h_{\mathrm{SLD}}$ admissible ($h\le h^{*}$, the true road distance to Bucharest) at both cities? (b) Does the edge Hirsova to Urziceni satisfy $h(x)\le c(x,y)+h(y)$?

![[cmpe421_lecture_04_informed_search_p004.jpg]]

- A) (a) No, it overestimates at Hirsova. (b) Yes.
- B) (a) No, it overestimates at Hirsova. (b) No.
- C) (a) Yes at both cities. (b) Yes.
- D) (a) Yes at both cities. (b) No.

> [!success]- Answer
> **C.** (a) $h^{*}(\text{Urziceni})=85$ and $h^{*}(\text{Hirsova})=98+85=183$. $80\le 85$ and $151\le 183$, so it is admissible at both.
> (b) $c+h(\text{Urziceni})=98+80=178$, and $151\le 178$, so the edge satisfies the inequality.
> - A: This takes $h^{*}(\text{Hirsova})=98$, only the edge to Urziceni, and forgets the 85 km from Urziceni to Bucharest. Then $151>98$ looks like an overestimate.
> - B: This makes both slips: $h^{*}(\text{Hirsova})=98$ in (a), and in (b) it compares 151 with $c=98$ alone and forgets $h(\text{Urziceni})=80$.
> - D: (a) is right, but (b) compares $h(\text{Hirsova})=151$ with the step cost 98 only and forgets to add $h(\text{Urziceni})=80$.

## 2. Greedy best-first search (slides 5-14)

### Q5 (informed-search, slide 5)

Directed edges with costs: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3. Heuristic: $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Run greedy best-first search from S to the goal G: tree search, goal test when a node is removed from the frontier, ties in alphabetical order. Which path does it return, and is it optimal?

- A) S, A, B, C, G with cost 7. It is optimal.
- B) S, B, G with cost 10. It is not optimal.
- C) S, A, B, G with cost 8. It is not optimal.
- D) S, A, B, G with cost 8. It is optimal.

> [!success]- Answer
> **B.** Greedy ranks by $h$ only. Remove S (6): frontier A 5, B 3. Remove B (3): frontier A 5, C 2, G 0. Remove G (0): goal. Path S, B, G, cost $5+5=10$. The path S, A, B, C, G costs $1+2+1+3=7$, so greedy is not optimal here.
> - A: This is the output of A* ($f=g+h$), not of greedy ($f=h$).
> - C: This is the output of $f=g$ (uniform-cost) with the goal test at generation: G is generated with cost 8 when B (via A) is expanded.
> - D: This is A* ($f=g+h$) with the goal test at generation: G is generated with $f=8$ when B is expanded. It is not optimal, because S, A, B, C, G costs 7.

### Q6 (informed-search, slides 6-7)

Run greedy search from **Lugoj** to Bucharest on the map. The values you need: $h$ = Lugoj 244, Mehadia 241, Timisoara 329, Dobreta 242, Craiova 160, Rimnicu Vilcea 193, Pitesti 98, Bucharest 0. Tree search, goal test when a node is removed. How many nodes are expanded, and is the route found also the shortest road route from Lugoj?

![[cmpe421_lecture_04_informed_search_p006.jpg]]

- A) 6 expansions. Yes, its 504 km are the shortest.
- B) 5 expansions. No, the route through Rimnicu Vilcea is shorter.
- C) 6 expansions. No, the route through Rimnicu Vilcea is shorter.
- D) 5 expansions. Yes, its 504 km are the shortest.

> [!success]- Answer
> **D.** Expanded: Lugoj (244), Mehadia (241), Dobreta (242), Craiova (160), Pitesti (98). Then Bucharest (0) is removed: goal. That is 5 expansions. Route Lugoj, Mehadia, Dobreta, Craiova, Pitesti, Bucharest: $70+75+120+138+101=504$ km, which is also the shortest route from Lugoj.
> - A: This counts the removal of Bucharest as a sixth expansion. The goal is removed and tested, not expanded.
> - B: This assumes greedy is never optimal. Through Rimnicu Vilcea the route is $70+75+120+146+97+101=609$ km, longer.
> - C: This makes both slips: it counts the goal removal as an expansion and assumes greedy can never be optimal.

### Q7 (informed-search, slides 8-9)

On slide 8 greedy (from Sibiu) chooses Fagaras ($h=176$) and not Rimnicu Vilcea ($h=193$). Roads: Sibiu-Fagaras 99, Fagaras-Bucharest 211, Sibiu-Rimnicu Vilcea 80, Rimnicu Vilcea-Pitesti 97, Pitesti-Bucharest 101. What is the road distance from Sibiu to Bucharest through each city, and what does greedy not see?

- A) Fagaras 310 km, Rimnicu Vilcea 278 km. Greedy does not see $h$, the estimate still to go.
- B) Fagaras 275 km, Rimnicu Vilcea 273 km. Greedy does not see $g$, the cost already paid.
- C) Fagaras 310 km, Rimnicu Vilcea 278 km. Greedy does not see $g$, the cost already paid.
- D) Fagaras 275 km, Rimnicu Vilcea 273 km. Greedy does not see $h$, the estimate still to go.

> [!success]- Answer
> **C.** Through Fagaras: $99+211=310$ km. Through Rimnicu Vilcea and Pitesti: $80+97+101=278$ km. The difference, 32 km, is exactly $450-418$. Greedy compares only $h$ ($176<193$). It ignores $g$ and does not see that the road from Fagaras is very long.
> - A: The distances are right, but greedy uses only $h$. It is $g$ that it ignores.
> - B: These are $99+176$ and $80+193$: the first edge plus the straight-line value, not the road distance.
> - D: This makes both slips: it adds $h$ to the first edge, and it says greedy ignores $h$.

### Q8 (informed-search, slide 14)

Find the mistake: "Greedy search and a greedy algorithm are the same thing. Both become complete in a finite space if we add loop checking."

- A) There is no mistake. Both keep a frontier and both become complete.
- B) The mistake: greedy search is not complete in a finite space, even with loop checking.
- C) The mistake: they differ. A greedy algorithm keeps a whole frontier, but greedy search keeps only the current path.
- D) The mistake: they differ. A greedy algorithm keeps only the current path and never goes back, so loop checking cannot make it complete.

> [!success]- Answer
> **D.** Greedy **search** keeps the whole frontier. From a dead end it goes back to another frontier node, so with loop checking in a finite space it is complete. A greedy **algorithm** keeps only its current path. If its choices lead to a dead end, it fails, and loop checking does not help.
> - A: A greedy algorithm keeps no frontier and never goes back.
> - B: With loop checking in a finite space, greedy search is complete. The slide says so.
> - C: This swaps the two. Greedy search is the one with the frontier.

## 3. A* search (slides 15-21)

### Q9 (informed-search, slide 15)

Same graph as Q5. Directed edges with costs: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3. Heuristic: $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Run A* with $f=g+h$: tree search, goal test when a node is removed, ties in alphabetical order. Which nodes are expanded, and which path is returned?

- A) Expanded S, A, B, C. Path S, A, B, C, G with cost 7.
- B) Expanded S, A, B. Path S, A, B, G with cost 8.
- C) Expanded S, B. Path S, B, G with cost 10.
- D) Expanded S, A, B, C, B, C, C. Path S, A, B, C, G with cost 7.

> [!success]- Answer
> **A.** Expand S (6): A $1+5=6$, B $5+3=8$. Expand A (6): B $3+3=6$, C $6+2=8$. Expand B with $g=3$ (6): C $4+2=6$, G $8+0=8$. Expand C with $g=4$ (6): G $7+0=7$. Remove G (7): goal. Cost $1+2+1+3=7$, optimal.
> - B: This tests the goal at generation. G is generated with $f=8$ when B is expanded, and the search stops too early.
> - C: This ranks by $f=h$ only. That is greedy search, not A*.
> - D: This ranks by $f=g$ only. That is uniform-cost search: same path, but it expands B, C and C again before G (7) is removed.

### Q10 (informed-search, slides 16-17)

Slide 17 shows Zerind with $f=449=75+374$. Roads: Arad-Zerind 75, Zerind-Oradea 71. $h(\text{Oradea})=380$. Compute $f$ for the child Oradea on the path Arad, Zerind, Oradea. On the next slide an Oradea node appears below Sibiu with $f=671$. Is it the same node?

![[cmpe421_lecture_04_informed_search_p017.jpg]]

- A) $f=526$. Yes, it is the same node, because the state is the same.
- B) $f=526$. No, it is a different node for the same state.
- C) $f=455$. No, it is a different node for the same state.
- D) $f=455$. Yes, it is the same node, because the state is the same.

> [!success]- Answer
> **B.** $g=75+71=146$, $h=380$, $f=146+380=526$. Below Sibiu: $g=140+151=291$, $f=291+380=671$. Same state, but a node also stores the path and its $g$. So these are two nodes. $h$ is the same (380), $g$ differs.
> - A: A node is a state plus a path. Two paths give two nodes, even with the same state.
> - C: This forgets the edge Zerind-Oradea: $g=75$, $f=75+380=455$.
> - D: This makes both slips: $g=75$ without the last edge, and node = state.

### Q11 (informed-search, slides 18-19)

In the A* run from Arad: Sibiu has $g=140$; Fagaras has $g=239$, $h=176$; Rimnicu Vilcea has $g=220$; Pitesti has $g=317$, $h=100$ (the search-tree value on slide 19). Suppose $h(\text{Rimnicu Vilcea})$ were 196 instead of 193 (the true road distance from Rimnicu Vilcea to Bucharest is 198). Tree search, goal test when a node is removed. What changes?

- A) Nothing: Rimnicu Vilcea is still expanded before Fagaras, and the route of 418 km is returned.
- B) Rimnicu Vilcea is still expanded first, but A* now returns the 450 km route, because 196 makes $h$ inadmissible.
- C) Fagaras is now expanded before Rimnicu Vilcea, and A* returns Arad, Sibiu, Fagaras, Bucharest, 450 km.
- D) Fagaras is now expanded before Rimnicu Vilcea, and A* still returns the 418 km route.

> [!success]- Answer
> **D.** $f(\text{RV})=220+196=416$ and $f(\text{Fagaras})=239+176=415$, so Fagaras comes first now. Order: Arad, Sibiu, Fagaras, Rimnicu Vilcea, Pitesti, then Bucharest (418) is removed. $196\le 198$ is still admissible, so the result stays optimal: Arad, Sibiu, Rimnicu Vilcea, Pitesti, Bucharest, 418 km.
> - A: With 196, $f(\text{RV})=416>415$. The order of these two expansions does change.
> - B: 196 is admissible because $196\le 198$. Also, the order changes, as shown above.
> - C: This tests the goal at generation: Bucharest (450) is generated when Fagaras is expanded, and the search stops there.

### Q12 (informed-search, slides 20-21)

The search tree on slide 21 is the final A* tree from Arad. A* removes Bucharest with $f=418$ and stops. Read the other frontier nodes on the slide. Why can no route shorter than 418 km be hidden behind them? ($h_{\mathrm{SLD}}$ is admissible.)

![[cmpe421_lecture_04_informed_search_p021.jpg]]

- A) The smallest other frontier value is 447. With an admissible $h$, $f$ is an upper bound, so every route through these nodes costs at most 447 km.
- B) The smallest other value is 413 (Rimnicu Vilcea). With an admissible $h$, $f$ is a lower bound, so a route through it may cost only 413 km, and A* stops only because a goal was found.
- C) The smallest other value is 413 (Rimnicu Vilcea). With an admissible $h$, $f$ is an upper bound, so the route through it costs at most 413 km.
- D) The smallest other frontier value is 447. With an admissible $h$, $f$ is a lower bound, so every route through these nodes costs at least 447 km.

> [!success]- Answer
> **D.** The other frontier nodes are Timisoara 447, Zerind 449, Bucharest 450, Craiova 526, Sibiu 553, Sibiu 591, Rimnicu Vilcea 607, Craiova 615, Arad 646, Oradea 671. The smallest is 447. With $h\le h^{*}$, $f=g+h$ is never more than the true cost of the best route through the node. So every such route costs at least $447>418$.
> - A: The direction is wrong. Admissible $h$ never overestimates, so $f$ is a lower bound, not an upper bound.
> - B: Rimnicu Vilcea (413) is already expanded (grey). It is not on the frontier. Only the leaves are.
> - C: This makes both slips: it counts the expanded Rimnicu Vilcea as a frontier node, and it uses $f$ as an upper bound.

## 4. Admissibility and optimality (slides 22-24)

### Q13 (informed-search, slide 22)

Directed edges with costs: S→A 3, S→B 1, B→A 1, A→G 4, B→G 7. Goal G. A heuristic gives $h(S)=6$, $h(A)=4$, $h(B)=6$, $h(G)=0$. What is $h^{*}(B)$, the true cost from B to the goal, and is $h$ admissible?

- A) $h^{*}(B)=7$. $h$ is admissible.
- B) $h^{*}(B)=5$. $h$ is admissible.
- C) $h^{*}(B)=5$. $h$ is not admissible: it overestimates at B.
- D) $h^{*}(B)=7$. $h$ is not admissible: $h(S)=6$ is larger than the step costs out of S.

> [!success]- Answer
> **C.** $h^{*}(A)=4$. $h^{*}(B)=\min(1+4,\ 7)=5$. $h^{*}(S)=\min(3+4,\ 1+5)=6$. Check $h\le h^{*}$: S $6\le 6$, A $4\le 4$, B $6\le 5$ **fails**. So $h$ is not admissible.
> - A: This uses only the direct edge B→G (7) and misses the cheaper path B, A, G (5).
> - B: $h^{*}(B)=5$ is right, but this checks only the start, $h(S)=6\le 6$. One state that overestimates is enough to break admissibility.
> - D: Admissibility compares $h$ with the whole remaining cost $h^{*}$, not with one step cost. (And $h^{*}(B)$ is 5, not 7.)

### Q14 (informed-search, slide 23)

In the optimality proof, $G_2$ is a suboptimal goal in the frontier, $G$ is an optimal goal, and $x$ is a frontier node on an optimal path. Which line of the proof uses admissibility? Then use the numbers $g(G_2)=12$, $g(G)=10$, $g(x)=4$, $h(x)=5$: what is $f(x)$?

- A) The line $g(G)\ge g(x)+h(x)$. $f(x)=9$.
- B) The line $f(G_2)=g(G_2)$. $f(x)=9$.
- C) The line $g(G)\ge g(x)+h(x)$. $f(x)=10$.
- D) The line $f(G_2)=g(G_2)$. $f(x)=10$.

> [!success]- Answer
> **A.** $x$ is on an optimal path, so $g(G)=g(x)+h^{*}(x)$. Admissible means $h(x)\le h^{*}(x)$, so $g(G)\ge g(x)+h(x)=f(x)$. Numbers: $f(x)=4+5=9$ and $f(G_2)=12+0=12$. $9<12$, so A* removes $x$ before $G_2$.
> - B: $f(G_2)=g(G_2)$ uses only $h=0$ at a goal, not admissibility.
> - C: This uses $h^{*}(x)=10-4=6$ instead of $h(x)=5$: $4+6=10$.
> - D: This makes both slips: the wrong line and $h^{*}(x)$ in place of $h(x)$.

### Q15 (informed-search, slide 24)

The step costs along one infinite path are $\tfrac12,\tfrac14,\tfrac18,\dots$ A solution of cost 5 exists elsewhere, and $h=0$ on this path. Why does this break completeness of A*, and why does "every step costs at least $\varepsilon>0$" repair it?

- A) The infinite path costs infinitely much, so A* never follows it. The rule "at least $\varepsilon$" only makes every step cost positive.
- B) The infinite path costs only 1, so A* follows it forever. The rule "at least $\varepsilon$" repairs this because it makes every step cost positive.
- C) The infinite path costs only 1, so A* follows it forever. With "at least $\varepsilon$", a path of $n$ steps costs at least $n\varepsilon$, which grows without limit.
- D) The infinite path costs infinitely much, so A* never follows it. With "at least $\varepsilon$", a path of $n$ steps costs at least $n\varepsilon$.

> [!success]- Answer
> **C.** $\tfrac12+\tfrac14+\tfrac18+\dots=1$. Every node of the path has $f<1<5$, so A* keeps expanding these nodes and never removes the goal. With steps of at least $\varepsilon$, a path of $n$ steps costs at least $n\varepsilon$, so every infinite path becomes more expensive than 5 at some point.
> - A: This assumes an infinite sum is always infinite. Here the sum is 1.
> - B: The costs $\tfrac12,\tfrac14,\dots$ are already positive. Positive is not enough. What helps is a fixed lower bound $\varepsilon$.
> - D: The repair is right, but this assumes an infinite sum is always infinite. Then there would be nothing to repair.

## 5. A* with graph search: consistency (slides 25-31)

### Q16 (informed-search, slide 25)

Directed edges with costs: S→A 2, S→B 3, A→C 4, B→C 1, C→G 3. Heuristic: $h(S)=7$, $h(A)=6$, $h(B)=3$, $h(C)=3$, $h(G)=0$. Is $h$ admissible ($h\le h^{*}$)? Is it consistent ($h(x)\le c(x,y)+h(y)$ on every edge)?

- A) Admissible and consistent.
- B) Admissible, but not consistent.
- C) Not admissible and not consistent.
- D) Not admissible, but consistent.

> [!success]- Answer
> **B.** $h^{*}(C)=3$, $h^{*}(B)=1+3=4$, $h^{*}(A)=4+3=7$, $h^{*}(S)=\min(2+7,\ 3+4)=7$. All $h\le h^{*}$ ($7\le7$, $6\le7$, $3\le4$, $3\le3$): admissible. Edge S→B: $7\le 3+3=6$ fails, so $h$ is not consistent. $f$ drops there from 7 to 6.
> - A: This assumes admissible implies consistent. Only the other direction is true.
> - C: This takes $h^{*}(B)=1$, the edge B→C only, and forgets C→G. Then $h(B)=3>1$ looks like an overestimate.
> - D: This swaps the two definitions. The edge test, which fails on S→B, is taken as the test for admissibility, and $h\le h^{*}$, which holds everywhere, as the test for consistency.

### Q17 (informed-search, slide 30)

In the figure the left node has $h=5$. Its edge to the bottom node costs 1, and the bottom node has $h=1$. Assume the cost from the bottom node to the goal is 4, and this is the only way to the goal. For which values of $h$ at the left node is this edge consistent? Is $h=5$ admissible at the left node?

![[cmpe421_lecture_04_informed_search_p030.jpg]]

- A) Consistent for $h\le 1$. $h=5$ is admissible.
- B) Consistent for $h\le 2$. $h=5$ is not admissible.
- C) Consistent for $h\le 2$. $h=5$ is admissible.
- D) Consistent for $h\le 1$. $h=5$ is not admissible.

> [!success]- Answer
> **C.** Consistency: $h(\text{left})\le 1+h(\text{bottom})=1+1=2$. Admissibility: $h^{*}(\text{left})=1+4=5$, and $5\le 5$. So $h=5$ is admissible but not consistent on this edge.
> - A: This forgets $h(\text{bottom})$ in the consistency test and compares $h$ with the step cost 1 alone.
> - B: This takes $h^{*}(\text{left})=1$, only the first edge, and forgets the assumed cost 4 below the bottom node.
> - D: This uses only the step cost 1 in both tests and ignores everything after the bottom node.

### Q18 (informed-search, slide 31)

`A*-Graph-Search` expands a removed node $x$ only if `explored` holds **no** node $y$ with the same state and $f(y)\le f(x)$. A node $x$ with state C and $f(x)=4$ is removed. Case 1: `explored` holds a node with state C and $f=5$. Case 2: it holds a node with state C and $f=4$. What happens to $x$?

- A) Case 1: $x$ is expanded. Case 2: $x$ is thrown away.
- B) Case 1: $x$ is thrown away. Case 2: $x$ is thrown away.
- C) Case 1: $x$ is expanded. Case 2: $x$ is expanded.
- D) Case 1: $x$ is thrown away. Case 2: $x$ is expanded.

> [!success]- Answer
> **A.** Case 1: $5\le 4$ is false, so no such $y$ exists, and $x$ is expanded again (re-expansion). Case 2: $4\le 4$ is true, the old node is at least as good, so $x$ is thrown away.
> - B: This is plain `Graph-Search`: any explored node with the same state blocks $x$.
> - C: This uses a strict test $f(y)<f(x)$. Then $4<4$ is false in case 2, and $x$ would be expanded.
> - D: This reads the test the wrong way round and expands $x$ only when $f(x)\ge f(y)$.

## 6. Properties of A* (slides 32-36)

### Q19 (informed-search, slide 36)

The slide gives time $O(d)$ in the best case for A* ($d$ = depth of the solution, $C^{*}$ = optimal cost). With which heuristic does this happen, and why?

- A) With $h=0$: A* then expands nodes in order of $g$, straight down the cheapest path.
- B) With any admissible $h$: A* never expands a node off the optimal path.
- C) With $h=h^{*}$: nodes off the optimal path have $f>C^{*}$, so A* walks down one optimal path.
- D) With $h=2h^{*}$: a larger $h$ always means fewer expansions, so A* goes straight to the goal.

> [!success]- Answer
> **C.** With the perfect heuristic, $f=g+h^{*}=C^{*}$ on an optimal path and $f>C^{*}$ elsewhere. A* never expands a node with $f>C^{*}$, so (with ties broken toward deeper nodes) it makes about $d$ expansions.
> - A: $h=0$ is the other extreme: A* becomes uniform-cost search and can expand the whole state space.
> - B: An admissible $h$ below $h^{*}$ still lets A* expand off-path nodes with $f<C^{*}$ ("hedging its bets").
> - D: $2h^{*}$ overestimates, so it is not admissible and A* may lose optimality. Dominance helps only among admissible heuristics.

## 7. Designing heuristics: relaxed problems (slides 37-43)

### Q20 (informed-search, slide 37)

A student makes the 8-puzzle **harder** by adding a constraint (tile 1 may never move), solves it optimally and uses the cost as $h$. Is this $h$ admissible?

- A) Yes. The optimal cost of any related problem is a lower bound on the real cost.
- B) No. The added constraint forbids some real solutions, so the best remaining cost is $\ge h^{*}$ and can overestimate.
- C) Yes. The harder problem allows fewer moves, so its optimal cost is smaller than $h^{*}$.
- D) No, but only because the harder problem may have no solution. When it has one, its cost is admissible.

> [!success]- Answer
> **B.** A relaxation removes constraints: every real solution is still allowed, so the best relaxed cost is $\le h^{*}$. Adding a constraint does the opposite: the best remaining solution costs $\ge h^{*}$. So $h$ can be larger than $h^{*}$.
> - A: Only a relaxed problem (fewer constraints) gives a lower bound.
> - C: Fewer allowed moves can only make the cheapest solution more expensive, not cheaper.
> - D: Even when a solution exists, its cost can be larger than $h^{*}$, so it can overestimate.

### Q21 (informed-search, slide 38)

The slide relaxes the route problem to allow straight-line paths. Roads: Rimnicu Vilcea-Pitesti 97, Pitesti-Bucharest 101, Rimnicu Vilcea-Craiova 146, Craiova-Pitesti 138. What is the true road distance $h^{*}$ from Rimnicu Vilcea to Bucharest, and how does it compare with the table value $h_{\mathrm{SLD}}=193$?

![[cmpe421_lecture_04_informed_search_p038.jpg]]

- A) $h^{*}=97$. $193>97$, so $h_{\mathrm{SLD}}$ overestimates here.
- B) $h^{*}=198$. $193\le 198$, as a relaxation promises.
- C) $h^{*}=385$. $193\le 385$, as a relaxation promises.
- D) $h^{*}=193$. The table gives the road distance itself.

> [!success]- Answer
> **B.** The shortest road route is Rimnicu Vilcea, Pitesti, Bucharest: $97+101=198$. The relaxed (straight-line) cost is $193\le 198$.
> - A: 97 is only the first road, to Pitesti. The route still has 101 km to go.
> - C: This is the longer route through Craiova: $146+138+101=385$. $h^{*}$ is the cheapest route.
> - D: The table is the straight-line distance $h$, not the road distance $h^{*}$.

### Q22 (informed-search, slide 39)

Four cities with distances AB 2, AC 3, AD 4, BC 4, BD 5, CD 3. What is the cost of the minimum spanning tree (MST), and what is the cost of the best tour (a closed round trip through all four cities)?

- A) MST 9, best tour 13.
- B) MST 8, best tour 8.
- C) MST 8, best tour 13.
- D) MST 9, best tour 8.

> [!success]- Answer
> **C.** MST: take the cheapest edges that make no cycle: AB 2, AC 3, CD 3, cost 8. Tours: A,B,C,D,A $=2+4+3+4=13$; A,B,D,C,A $=2+5+3+3=13$; A,C,B,D,A $=3+4+5+4=16$. Best tour 13, and $8\le 13$: the MST is a lower bound.
> - A: This builds a nearest-neighbour path from A (A-B 2, B-C 4, C-D 3 = 9). That is not the minimum spanning tree.
> - B: This forgets the edge back to the start: the cheapest open path B, A, C, D costs 8, but a tour must close.
> - D: This makes both slips: the nearest-neighbour path as "MST" and the tour without its return edge.

### Q23 (informed-search, slide 41)

Board 8 1 3 / 4 _ 2 / 7 6 5 (rows top to bottom, _ is the blank). Goal 1 2 3 / 4 5 6 / 7 8 _. What is $h_1$, the number of misplaced tiles?

- A) 5
- B) 6
- C) 4
- D) 10

> [!success]- Answer
> **A.** Compare each square with the goal. Wrong: 8 (goal 1), 1 (goal 2), 2 (goal 6), 6 (goal 8), 5 (goal blank). Right: 3, 4, 7. The blank is not counted. $h_1=5$.
> - B: This also counts the blank as a misplaced tile.
> - C: This skips tile 5 because its goal square is the blank square. Tile 5 is still in the wrong place.
> - D: 10 is $h_2$, the total Manhattan distance, not the number of misplaced tiles.

### Q24 (informed-search, slide 43)

Same board 8 1 3 / 4 _ 2 / 7 6 5, goal 1 2 3 / 4 5 6 / 7 8 _. What is $h_2$, the total Manhattan distance (rows moved plus columns moved, summed over the tiles)?

- A) 6
- B) 10
- C) 12
- D) 5

> [!success]- Answer
> **B.** Tile 8: $2+1=3$. Tile 1: 1. Tile 2: $1+1=2$. Tile 6: $1+1=2$. Tile 5: $1+1=2$. Tiles 3, 4, 7: 0. $h_2=10$. Check: $h_1=5\le 10\le 14=h^{*}$.
> - A: This takes the larger of the row and column moves for each tile (diagonal steps count as 1), not their sum.
> - C: This also counts the blank, which is 2 squares from its goal square.
> - D: This counts only the row moves and forgets the column moves.

## 8. Dominance (slides 44-45)

### Q25 (informed-search, slide 44)

The table gives the nodes generated by A*: $h_1$: 539 at $d=14$ and 39135 at $d=24$; $h_2$: 113 at $d=14$ and 1641 at $d=24$. (a) How many times fewer nodes does $h_2$ need than $h_1$? (b) A search finds a solution at depth $d=5$ with $N=62$ nodes. The effective branching factor $b^{*}$ solves $N+1=1+b^{*}+(b^{*})^{2}+\dots+(b^{*})^{d}$. Which is right?

- A) (a) About 4.8 times and 23.8 times. (b) $b^{*}=2$, since $1+2+4+8+16+32=63$.
- B) (a) About 0.21 times and 0.042 times. (b) $b^{*}=2$, since $1+2+4+8+16+32=63$.
- C) (a) About 4.8 times and 23.8 times. (b) $b^{*}=62^{1/5}\approx 2.28$.
- D) (a) About 0.21 times and 0.042 times. (b) $b^{*}=62^{1/5}\approx 2.28$.

> [!success]- Answer
> **A.** (a) $539/113\approx 4.8$ and $39135/1641\approx 23.8$. The gain grows with depth. (b) With $b^{*}=2$, $d=5$: $1+2+4+8+16+32=63=N+1$.
> - B: This divides the wrong way, $113/539$ and $1641/39135$. "How many times fewer" is the larger count over the smaller one.
> - C: This uses $N=(b^{*})^{d}$, only the last level of the tree, instead of the sum of all levels.
> - D: This makes both slips: the inverted ratio and $N=(b^{*})^{d}$.

### Q26 (informed-search, slide 45)

Three states have $h^{*}=6,\ 5,\ 7$. Two admissible heuristics give $h_a=3,\ 5,\ 2$ and $h_b=4,\ 1,\ 6$. Which of $\max(h_a,h_b)$, $h_a+h_b$ and $2h_a$ are admissible?

- A) Only $\max(h_a,h_b)$.
- B) $\max(h_a,h_b)$ and $h_a+h_b$.
- C) $\max(h_a,h_b)$ and $2h_a$.
- D) All three.

> [!success]- Answer
> **A.** $\max=4,\ 5,\ 6\le 6,\ 5,\ 7$: admissible (and it dominates both). $h_a+h_b=7,\ 6,\ 8$: larger than $h^{*}$ in every state. $2h_a=6,\ 10,\ 4$: $10>5$ in the second state.
> - B: The sum of admissible heuristics is not admissible in general. Here it overestimates everywhere.
> - C: This checks only the first state ($6\le 6$). The second state gives $10>5$.
> - D: This assumes anything built from admissible heuristics is admissible. Only the maximum is safe.

## 9. IDA* (slides 46-51)

### Q27 (informed-search, slide 46)

Same graph as Q5: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3, with $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Run IDA*: tree search, children in alphabetical order, first limit $h(S)=6$. Round 1 backtracks at nodes with $f=7, 8, 8, 8$ and finds no solution. What is the limit of round 2, and how many expansions does IDA* make in total?

- A) Limit 8. 8 expansions in total.
- B) Limit 7. 8 expansions in total.
- C) Limit 7. 4 expansions in total.
- D) Limit 8. 4 expansions in total.

> [!success]- Answer
> **B.** The next limit is the smallest cut-off value: $\min(7,8,8,8)=7$. Round 1 expands S, A, B, C (4). Round 2 expands S, A, B, C again (4) and then reaches G with $f=7\le 7$. Total $4+4=8$. Path S, A, B, C, G, cost 7.
> - A: This takes the largest cut-off value, 8. A larger limit could jump over a cheaper solution.
> - C: This counts only the last round. IDA* repeats the earlier rounds, so they count too.
> - D: This makes both slips: the largest cut-off value and only the last round.

### Q28 (informed-search, slide 51)

In a bad case, 100 nodes have $f\le C^{*}$ and all their $f$ values are different, so each IDA* round reaches only one new node. About how many expansions does IDA* make, and how many does A* make?

- A) IDA* about 5050, A* about 100.
- B) IDA* about 10100, A* about 100.
- C) IDA* about 100, A* about 100.
- D) IDA* about 4950, A* about 100.

> [!success]- Answer
> **A.** Round $k$ expands $k$ nodes. Total $1+2+\dots+100=\dfrac{100\cdot 101}{2}=5050$. A* expands each node once: about 100.
> - B: This is $100\cdot 101$ without dividing by 2.
> - C: This assumes the last round dominates. That holds only when many nodes share the same $f$ value, not here.
> - D: This is $1+2+\dots+99$: the last round, with 100 nodes, is left out.

## 10. Summary of informed search (slide 52)

### Q29 (informed-search, slide 52)

Choose greedy, A* or IDA* for each case. (a) The optimal solution is needed and memory is very small. (b) Any solution is fine and it must come fast. (c) The optimal solution is needed with as few expansions as possible, and memory is large.

- A) (a) A*, (b) greedy, (c) IDA*.
- B) (a) IDA*, (b) greedy, (c) A*.
- C) (a) IDA*, (b) A*, (c) A*.
- D) (a) A*, (b) A*, (c) IDA*.

> [!success]- Answer
> **B.** (a) IDA* is optimal with an admissible $h$ and stores one path, $O(bd)$. (b) Greedy follows the lowest $h$ and is usually fast, but the solution may be poor. (c) A* is optimal and does not repeat rounds, but it keeps all nodes in memory.
> - A: This swaps A* and IDA*: it gives A* the small memory and IDA* the few expansions. It is the other way round.
> - C: A* in (b) pays for optimality with extra expansions. When any solution is fine and speed counts, greedy is the choice.
> - D: This swaps A* and IDA* in (a) and (c), and also picks A* where greedy is enough.

## 11. From the instructor's deck `Ch.4.pdf` (Ch.4 pages 10, 11, 12, 22, 40, 41)

### Q30 (Ch.4, slide 10)

Graph of Ch.4 slide 10: S→a 1, a→b 1, b→c 1, a→d 3, a→e 8, e→d 1, d→G 2. $h$: S 6, a 5, b 6, c 7, d 2, e 1, G 0. Start S, goal G, tree search, goal test when a node is removed. Which nodes does A* expand, and which nodes does uniform-cost search expand that A* does not?

- A) A* expands S, a, e, d. UCS also expands b and c.
- B) A* expands S, a, d. UCS also expands e.
- C) A* expands S, a, d. UCS also expands b and c.
- D) A* expands S, a, e, d. UCS also expands e.

> [!success]- Answer
> **C.** A*: S (6); a $1+5=6$; frontier b $2+6=8$, d $4+2=6$, e $9+1=10$; expand d (6); remove G $6+0=6$: goal. UCS ranks by $g$: S 0, a 1, b 2, c 3, d 4, then G 6 is removed. So UCS also expands b and c.
> - A: S, a, e, d is the greedy order ($f=h$: after a it picks e with $h=1$).
> - B: e is expanded by greedy, not by UCS. Its $g=9$ is larger than the solution cost 6.
> - D: This uses $f=h$ for A* and also gives greedy's extra node e to UCS.

### Q31 (Ch.4, slide 11)

S→A 3, A→G 3, S→B 1, B→G 6. $h(S)=5$, $h(A)=3$, $h(B)=4$, $h(G)=0$. A* with $f=g+h$, tree search, ties in alphabetical order. What does A* return if it stops when a goal is **enqueued** (generated), and what if it stops when a goal is **dequeued** (removed)?

- A) Enqueue: S, B, G, cost 7. Dequeue: S, A, G, cost 6.
- B) Enqueue: S, A, G, cost 6. Dequeue: S, A, G, cost 6.
- C) Enqueue: S, B, G, cost 7. Dequeue: S, B, G, cost 7.
- D) Enqueue: S, A, G, cost 6. Dequeue: S, B, G, cost 7.

> [!success]- Answer
> **A.** Expand S (5): A $3+3=6$, B $1+4=5$. Expand B (5): G with $g=7$ is generated. Stop on enqueue: S, B, G, cost 7 (wrong). Stop on dequeue: frontier A 6, G 7. Expand A (6): G with $f=6$. G (6) is removed first: S, A, G, cost 6, optimal.
> - B: This ranks by $h$ only (greedy): A ($h=3$) is expanded before B ($h=4$), so G via A is generated first.
> - C: This uses a stack (last in, first out) instead of the lowest $f$: G (7), pushed last, is popped before A.
> - D: This swaps the two rules. Testing on removal is the safe rule.

### Q32 (Ch.4, slide 12)

S→A 1, A→G 3, S→G 5. $h(S)=7$, $h(A)=3$, $h(G)=0$ (the slide has $h(A)=6$). A* tree search, stop when a goal is dequeued. Only the value at A is asked about: is $h(A)=3\le h^{*}(A)$, so that this single value is admissible? And what cost does A* return?

- A) Yes, admissible. A* returns cost 5.
- B) Yes, admissible. A* returns cost 4.
- C) No, not admissible. A* returns cost 4.
- D) No, not admissible. A* returns cost 5.

> [!success]- Answer
> **B.** $h^{*}(A)=3$ (only A→G), and $3\le 3$. Expand S: A $1+3=4$, G $5+0=5$. Expand A (4): G with $g=4$, $f=4$. Dequeue G (4): S, A, G, cost 4, optimal.
> - A: This stops when G (5) is generated in the first step, a goal test at enqueue.
> - C: This uses a strict test $h<h^{*}$. Admissible allows equality.
> - D: This judges the whole heuristic instead of the value at A: $h(S)=7>h^{*}(S)=4$, so it expects A* to miss the optimal path. The question asks only about A, where $3\le 3$, and the trace gives cost 4.

### Q33 (Ch.4, slide 22)

Why is the UCS contour a circle around the start, and the A* contour a narrow shape stretched toward the goal?

![[cmpe421_lecture_03_ch_4_p022.jpg]]

- A) UCS expands nodes with $g<C^{*}$, equally in all directions. A* expands nodes with $g+h<C^{*}$, and nodes behind the start have a large $h$.
- B) UCS has no heuristic, so it expands nodes in random order. A* expands only the nodes on the optimal path.
- C) UCS expands nodes with $g<C^{*}$. A* expands nodes with $h<C^{*}$, so only nodes close to the goal.
- D) UCS uses a FIFO queue, so it expands level by level. A* uses a priority queue, so it goes toward the goal.

> [!success]- Answer
> **A.** UCS expands every node with $g<C^{*}$: a circle in cost around the start. A* uses $f=g+h$: a node on the far side of the start has a small $g$ but a large $h$, so its $f$ is too large. A* still expands some off-path nodes with $f<C^{*}$ ("hedges its bets").
> - B: UCS is not random: it expands in order of $g$. And A* expands more than the optimal path.
> - C: A* uses $g+h$, not $h$ alone. $h<C^{*}$ would describe nodes near the goal regardless of the cost from the start.
> - D: UCS uses a priority queue ordered by $g$, not a FIFO queue. FIFO is breadth-first search.

### Q34 (Ch.4, slide 40)

Check the heuristic on the slide ($h(S)=2$, $h(A)=4$, $h(B)=1$, $h(C)=1$, $h(G)=0$) with the edges S→A 1, S→B 1, A→C 1, B→C 2, C→G 3. Is it admissible, and on which edges does $h(x)\le c(x,y)+h(y)$ fail?

![[cmpe421_lecture_03_ch_4_p040.jpg]]

- A) Admissible. Consistency fails on S→B and A→C.
- B) Not admissible. Consistency fails on A→C only.
- C) Not admissible. Consistency fails on S→B and A→C.
- D) Admissible. Consistency fails on A→C only.

> [!success]- Answer
> **D.** $h^{*}(C)=3$, $h^{*}(A)=4$, $h^{*}(B)=5$, $h^{*}(S)=5$. $2\le5$, $4\le4$, $1\le5$, $1\le3$: admissible. Edges: S→A $2\le5$, S→B $2\le2$, A→C $4\le2$ **fails**, B→C $1\le3$, C→G $1\le3$.
> - A: This uses a strict test $<$. On S→B, $2\le 1+1$ holds with equality.
> - B: This takes $h^{*}(A)=1$, the edge A→C alone, and forgets C→G. Then $4>1$ looks like an overestimate.
> - C: This makes both slips: $h^{*}$ from one edge and the strict test.

### Q35 (Ch.4, slide 41)

The slide repairs the heuristic with $h(A)=2$. Edges S→A 1, S→B 1, A→C 1, B→C 2, C→G 3; $h(S)=2$, $h(A)=2$, $h(B)=1$, $h(C)=1$, $h(G)=0$. Run A* **graph** search: goal test when a node is removed, a state is expanded at most once, no ties occur. In which order are the states expanded, and what is returned?

- A) S, A, B, C. Result S, A, C, G with cost 5.
- B) S, B, C, A. Result S, B, C, G with cost 6.
- C) S, B, C. Result S, B, C, G with cost 6.
- D) S, B, A, C. Result S, A, C, G with cost 5.

> [!success]- Answer
> **D.** Expand S (2): A $1+2=3$, B $1+1=2$. Expand B (2): C via B $3+1=4$. Expand A (3): C via A $2+1=3$. Expand C via A (3): G $5+0=5$. C via B (4) is removed and skipped. G (5) is removed: S, A, C, G, cost 5.
> - A: This ranks by $f=g$ only (uniform-cost graph search). A and B both have $g=1$, the alphabet puts A first, and C ($g=2$) follows. The result is the same, but the order is not the A* order.
> - B: This keeps the old value $h(A)=4$: A has $f=5$, so C is closed with $g=3$ first. That is slide 40's wrong result.
> - C: This ranks by $f=h$ only (greedy graph search): B ($h=1$), then C ($h=1$), then G.

## 12. Local search: iterative improvement (local-search slides 1-3)

### Q36 (local-search, slide 1)

Which of the two problems fits iterative improvement (local search), and why? (a) Place 8 queens with no attacks. (b) Find the cheapest road route from Arad to Bucharest.

- A) (b) fits and (a) does not: a route can be improved city by city, but a board has no cost to improve.
- B) (a) fits: only the final board matters. (b) does not fit directly: the solution is the path, and local search keeps no path.
- C) Both fit: each problem has a cost that a local search can reduce step by step.
- D) Neither fits: local search works only when the states are real numbers.

> [!success]- Answer
> **B.** In (a) every board is a complete state, and the order of the moves does not matter. In (b) the answer **is** the path, but local search keeps only the current state, not how it got there.
> - A: This swaps the two. The board has a cost (the number of attacking pairs) that is easy to improve.
> - C: (b) has a cost, but its answer is a path. Local search does not keep the path, so it does not fit directly.
> - D: Local search works on discrete states too: $n$-queens and TSP tours are the slides' examples.

### Q37 (local-search, slide 2)

An 8-queens state has the queens of columns 1 to 8 in rows 3, 5, 2, 5, 1, 4, 7, 7. Two queens attack each other if they are in the same row or on the same diagonal. How many attacking pairs $h$ are there?

- A) 2
- B) 6
- C) 25
- D) 3

> [!success]- Answer
> **D.** Columns 2 and 4: rows 5 and 5, same row. Columns 7 and 8: rows 7 and 7, same row. Columns 3 and 8: rows 2 and 7, row difference 5 = column difference 5, same diagonal. No other pair attacks: $h=3$.
> - A: This counts only same-row pairs and forgets the diagonals.
> - B: This counts each pair twice, once from each queen.
> - C: 25 is the number of pairs that do **not** attack ($28-3$), which is the GA fitness, not $h$.

### Q38 (local-search, slide 3)

Four cities at A $(0,0)$, B $(4,0)$, C $(4,3)$, D $(0,3)$. The tour A, C, B, D, A crosses itself. A pairwise exchange removes the two crossing edges AC and BD and adds AB and CD. What are the old and the new tour lengths?

- A) Old 20, new 14.
- B) Old 16, new 14.
- C) Old 16, new 11.
- D) Old 20, new 11.

> [!success]- Answer
> **B.** AB $=4$, BC $=3$, CD $=4$, DA $=3$, diagonals AC $=$ BD $=\sqrt{4^{2}+3^{2}}=5$. Old tour A, C, B, D, A: $5+3+5+3=16$. New tour A, B, C, D, A: $4+3+4+3=14$. The change is $(4+4)-(5+5)=-2$, so the move is accepted.
> - A: This measures the diagonals as $4+3=7$ (city-block distance), not the straight-line 5.
> - C: This forgets the edge D→A back to the start in the new tour: $4+3+4=11$.
> - D: This makes both slips: diagonals of 7 and no return edge.

## 13. Hill climbing (local-search slides 5-6)

### Q39 (local-search, slide 5)

Positions 0 to 9 have the values 1, 3, 4, 4, 2, 5, 8, 6, 7, 3. The neighbours of a position are the positions directly left and right of it. `Hill-Climbing` (maximise) moves to the best neighbour and stops when the best neighbour is **not strictly better**. Runs start at positions 1, 4 and 9. Where does the run from start 1 stop, and which runs end at the global maximum (position 6, value 8)?

- A) Start 1 stops at position 2. Starts 4 and 9 reach the global maximum.
- B) Start 1 never stops: it moves between positions 2 and 3. Starts 4 and 9 reach the global maximum.
- C) Start 1 stops at position 2. Only start 4 reaches the global maximum.
- D) Start 1 never stops: it moves between positions 2 and 3. Only start 4 reaches the global maximum.

> [!success]- Answer
> **C.** Start 1 (value 3) moves to position 2 (value 4). Its neighbours have 3 and 4, and $4\le 4$, so it stops on the flat part. Start 4 (value 2) goes to 5 (value 5), then 6 (value 8), and stops: global maximum. Start 9 (value 3) goes to 8 (value 7) and stops: neighbours 6 and 3, a local maximum.
> - A: From 8 the run would have to go down to 6 (value 6) before it could go up to 8. Hill climbing never goes down.
> - B: This makes both slips: it also moves on equal values, and it lets start 9 pass the local maximum.
> - D: This moves when the neighbour is equal ($\ge$). The code stops on "not strictly better", so no endless sideways moves happen.

### Q40 (local-search, slide 6)

From the marked current state, plain hill climbing stops at the local maximum. With random restarts, one climb from a random start reaches the global maximum with probability $p=0.25$. What is the expected number of climbs, and what is the probability that at least one of 5 climbs succeeds?

![[cmpe421_lecture_04_local_search_p006.jpg]]

- A) Expected 3 climbs. At least one success in 5: about 0.763.
- B) Expected 4 climbs. At least one success in 5: about 0.237.
- C) Expected 3 climbs. At least one success in 5: about 0.237.
- D) Expected 4 climbs. At least one success in 5: about 0.763.

> [!success]- Answer
> **D.** Expected number of climbs: $1/p=1/0.25=4$. At least one success: $1-(1-p)^{5}=1-0.75^{5}\approx 1-0.237=0.763$.
> - A: 3 is the expected number of failed climbs. The successful climb also counts: $3+1=4$.
> - B: 0.237 is $0.75^{5}$, the probability that all 5 climbs fail. The answer is 1 minus this.
> - C: This makes both slips: it counts only the failures and gives the probability that all 5 fail.

## 14. Simulated annealing (local-search slides 7-10)

### Q41 (local-search, slide 7)

A random neighbour is worse by 2, so $\Delta E=-2$. A downhill move is accepted with probability $e^{\Delta E/T}$. What are the acceptance probabilities at $T=10$, $T=1$ and $T=0.1$, and what happens to a neighbour with $\Delta E=+2$?

- A) About $2\times10^{-9}$, 0.135, 0.819. The uphill move is always taken.
- B) About 0.819, 0.135, $2\times10^{-9}$. The uphill move is taken with probability $e^{-2/T}$, like the downhill one.
- C) About 0.819, 0.135, $2\times10^{-9}$. The uphill move is always taken.
- D) About $2\times10^{-9}$, 0.135, 0.819. The uphill move is taken with probability $e^{-2/T}$, like the downhill one.

> [!success]- Answer
> **C.** $e^{-2/10}=e^{-0.2}\approx 0.819$, $e^{-2/1}\approx 0.135$, $e^{-2/0.1}=e^{-20}\approx 2\times 10^{-9}$. An uphill move ($\Delta E>0$) is always taken. High $T$: almost a random walk. Low $T$: almost hill climbing.
> - A: This computes $e^{\Delta E\cdot T}$ instead of $e^{\Delta E/T}$, so the trend is reversed.
> - B: This uses $-\lvert\Delta E\rvert$ for every move. Only downhill moves are random.
> - D: This makes both slips: $e^{\Delta E\cdot T}$ and the same rule for uphill moves.

### Q42 (local-search, slide 8)

The temperature is $T=10\times 0.9^{i}$ in iteration $i$. (a) What is $T$ at $i=20$? (b) From which iteration on is $T$ below 0.1?

- A) (a) $T\approx 1.22$. (b) From $i=44$.
- B) (a) $T\approx 0.122$. (b) From $i=44$.
- C) (a) $T\approx 1.22$. (b) From $i=43$.
- D) (a) $T\approx 0.122$. (b) From $i=43$.

> [!success]- Answer
> **A.** (a) $0.9^{20}\approx 0.1216$, so $T\approx 1.22$. (b) $10\times 0.9^{i}<0.1$ means $0.9^{i}<0.01$, so $i>\ln 0.01/\ln 0.9\approx 43.7$. The first integer is 44 ($T\approx 0.097$). At $i=43$, $T\approx 0.108$, still above 0.1.
> - B: This forgets the factor 10: $0.9^{20}\approx 0.122$ is not $T$.
> - C: This rounds 43.7 down. $i$ must be larger than 43.7.
> - D: This makes both slips: no factor 10 and 43.7 rounded down.

### Q43 (local-search, slide 9)

The plot shows $s$ over 100 iterations for several runs. The value function is $\max\{-\lvert s-0.3\rvert,\ 0.025-\lvert s-0.8\rvert\}$: value 0 at $s=0.3$, value 0.025 at $s=0.8$. Every state is a neighbour of every other state (the next state is a new random number in $[0,1]$). Two curves sit near 0.3 and still jump to 0.8 after $i=80$, where $T\approx 0.002$. How is that possible?

![[cmpe421_lecture_04_local_search_p009.jpg]]

- A) It is a downhill move. Its probability $e^{\Delta E/T}$ is small at $T\approx 0.002$, but not zero.
- B) The program raises the temperature again when a run is stuck, so the run can escape.
- C) The values at 0.3 and 0.8 are equal, so the jump has $\Delta E=0$ and is always taken.
- D) It is an uphill move: every $s$ within 0.025 of 0.8 has a value above 0, and such an $s$ is drawn about 5% of the time.

> [!success]- Answer
> **D.** At $s=0.3$ the value is 0. Every new state within 0.025 of 0.8 has a value above 0, so $\Delta E>0$ and the move is always accepted, at every temperature. That interval has width 0.05, so such a state is drawn with probability about 0.05 per iteration.
> - A: The move goes from value 0 to a value above 0. It is uphill, not downhill. A downhill move at $T\approx 0.002$ would be accepted practically never.
> - B: The temperature only goes down: it is multiplied by 0.9 in every iteration.
> - C: The values are not equal: 0 at 0.3 and 0.025 at 0.8. The jump has $\Delta E>0$.

### Q44 (local-search, slide 10)

At a fixed temperature the state distribution approaches $p(x)=\alpha\,e^{E(x)/kT}$. State $x^{*}$ has $E(x^{*})=3$ and state $x$ has $E(x)=2$. What is $p(x^{*})/p(x)$ for $kT=1$ and for $kT=0.1$?

- A) About 0.368 and about $4.5\times 10^{-5}$.
- B) About 2.72 and about 1.105.
- C) About 2.72 and about 22026.
- D) About 20.1 and about $1.07\times 10^{13}$.

> [!success]- Answer
> **C.** $p(x^{*})/p(x)=e^{(E(x^{*})-E(x))/kT}$, and $\alpha$ cancels. $kT=1$: $e^{1}\approx 2.72$. $kT=0.1$: $e^{10}\approx 22026$. At a low temperature the best state is far more likely.
> - A: This computes $p(x)/p(x^{*})$, the inverse ratio: $e^{-1}$ and $e^{-10}$.
> - B: This multiplies by $kT$ instead of dividing: $e^{1\cdot 1}$ and $e^{1\cdot 0.1}$.
> - D: This uses only $E(x^{*})=3$ in the exponent, not the difference: $e^{3}$ and $e^{30}$.

## 15. Local beam search (local-search slide 11)

### Q45 (local-search, slide 11)

$k=2$. The current states are P (value 5) and Q (value 9). The successors of P have the values 6 and 4. The successors of Q have the values 12 and 10. We maximise. Which states does local beam search keep, and which states do two independent hill climbers keep?

- A) Beam search: 12 and 6. Independent climbers: 12 and 10.
- B) Beam search: 12 and 10. Independent climbers: 12 and 10.
- C) Beam search: 12 and 10. Independent climbers: 6 and 12.
- D) Beam search: 12 and 6. Independent climbers: 6 and 12.

> [!success]- Answer
> **C.** Beam search pools all successors (6, 4, 12, 10) and keeps the best 2: 12 and 10, both from Q. Each independent climber keeps its own best successor: 6 from P and 12 from Q.
> - A: This swaps the two methods.
> - B: Independent climbers do not share a pool. The climber at P never sees Q's successors.
> - D: This makes beam search keep one best successor per state. That is $k$ independent searches, not beam search.

## 16. Genetic algorithms (local-search slides 12-18)

### Q46 (local-search, slide 12)

Two 8-queens parents are 32752411 and 24748552. Cut both after the 5th character and join the front of one parent to the back of the other. What are the two children?

- A) 32752552 and 24748411.
- B) 32752452 and 24748511.
- C) 32758552 and 24742411.
- D) 32752411 and 24748552.

> [!success]- Answer
> **A.** Front of the first + back of the second: 32752 + 552 = 32752552. Front of the second + back of the first: 24748 + 411 = 24748411. Each character is the row of the queen in its column.
> - B: This cuts after the 6th character: 327524 + 52 and 247485 + 11.
> - C: This cuts after the 4th character: 3275 + 8552 and 2474 + 2411.
> - D: These are the parents. No crossover happened.

### Q47 (local-search, slide 13)

`Reproduce` returns `x[:c] + y[c:]` with `c` a random integer from `range(len(x))`. For `x = "1234"` and `y = "abcd"`, which children are possible, and which one is a plain copy of a parent?

- A) `abcd`, `1bcd`, `12cd`, `123d`, `1234`. Both `abcd` (copy of `y`) and `1234` (copy of `x`) are copies.
- B) `abcd`, `1bcd`, `12cd`, `123d`. Only `abcd`, a copy of `y`, is a copy.
- C) `1bcd`, `12cd`, `123d`, `1234`. Only `1234`, a copy of `x`, is a copy.
- D) `1234`, `1bcd`, `12cd`, `123d`. Only the child for $c=0$ is a copy, and it is a copy of `x`.

> [!success]- Answer
> **B.** `range(4)` gives $c=0,1,2,3$. $c=0$: `"" + "abcd"` = `abcd`. $c=1$: `1bcd`. $c=2$: `12cd`. $c=3$: `123d`. $c=4$ never occurs, so a copy of `x` is impossible.
> - A: This lets $c$ go up to 4. `range(4)` stops at 3.
> - C: This lets $c$ run from 1 to 4. `range` starts at 0.
> - D: This assumes that $c=0$ means "no crossover, keep `x`". But `x[:0]` is the empty string, so $c=0$ gives all of `y`, not `x`.

### Q48 (local-search, slide 14)

4-queens: a state is a string of 4 rows, fitness = number of non-attacking pairs (maximum 6). The population is 2424, 2143, 1343, 1234. (a) What is the selection probability of 2424 (fitness-proportional)? (b) Cross $x=2424$ and $y=2143$ at $c=2$ (child `x[:2] + y[2:]`), then change the third character of the child to 1. What is the final child and its fitness?

- A) (a) $4/9\approx 44\%$. (b) 2413, fitness 6.
- B) (a) $2/15\approx 13\%$. (b) 2413, fitness 6.
- C) (a) $4/9\approx 44\%$. (b) 2114, fitness 4.
- D) (a) $2/15\approx 13\%$. (b) 2114, fitness 4.

> [!success]- Answer
> **A.** (a) Attacking pairs 2, 4, 3, 6, so fitness $6-2=4$, 2, 3, 0. Sum 9, and $P(2424)=4/9\approx 44\%$. (b) `24` + `43` = 2443 (fitness 3). Third character 4 → 1: 2413. No pair attacks: fitness 6, a solution.
> - B: This uses the number of attacking pairs (2, 4, 3, 6, sum 15) as fitness. Then the worst states are chosen most often.
> - C: This takes the front of $y$ and the back of $x$: `21` + `24` = 2124, then 2114 (2 attacking pairs, fitness 4).
> - D: This makes both slips: attacking pairs as fitness and the parents swapped.

### Q49 (local-search, slide 15)

The fitness is $\max\{-\lvert s-0.3\rvert,\ 0.025-\lvert s-0.8\rvert\}$. Compute it at $s=0.55$ and $s=0.78$. Of the four states $s=0.3$, $0.55$, $0.78$, $1.0$, which fitness values can be used directly as roulette-wheel selection weights?

- A) $-0.25$ and $-0.48$. All four values can be used.
- B) $-0.225$ and $0.005$. Only the values at $s=0.3$ (0) and $s=0.78$ (0.005).
- C) $-0.225$ and $0.005$. All four values can be used.
- D) $-0.25$ and $-0.48$. None of the four values can be used.

> [!success]- Answer
> **B.** $s=0.55$: $\max(-0.25,\ 0.025-0.25)=-0.225$. $s=0.78$: $\max(-0.48,\ 0.025-0.02)=0.005$. Also $s=0.3$: 0, and $s=1.0$: $\max(-0.7,\ -0.175)=-0.175$. Selection weights must be $\ge 0$: only 0 and 0.005.
> - A: This takes the minimum of the two terms, not the maximum, and also accepts negative weights.
> - C: The values are right, but a roulette wheel cannot use negative weights such as $-0.225$.
> - D: This takes the minimum instead of the maximum. Then every value is negative.

### Q50 (local-search, slide 16)

In the program a state is an integer with 4 digits. `par1 = 3012`, `par2 = 7985`, `split = 2`, and the child is `str(par1)[:split] + str(par2)[split:]`. Then a mutation with `where = 0`, `what = "9"` makes `child[0:where] + what + child[where+1:]`. `split = int(random.random() * (stringsize+1))` with `stringsize = 4`. What is the child after the mutation, and how many values can `split` take?

- A) 93085. `split` can take 5 values.
- B) 9085. `split` can take 4 values.
- C) 93085. `split` can take 4 values.
- D) 9085. `split` can take 5 values.

> [!success]- Answer
> **D.** Crossover: `"30" + "85"` = 3085. Mutation: `"" + "9" + "085"` = 9085. `random.random()` is in $[0,1)$, so `split` is 0, 1, 2, 3 or 4: five values.
> - A: This inserts the 9 (`child[0:]` after it) instead of replacing the first digit.
> - B: This forgets the `+1` in `stringsize+1`.
> - C: This makes both slips: insertion and no `+1`.

### Q51 (local-search, slide 17)

The plot shows the best individual of each GA iteration, with `top` = 10000 (so 3000 stands for $s=0.3$ and 8000 for $s=0.8$). The fitness is 0 at $s=0.3$ and 0.025 at $s=0.8$. Which value is the global maximum? The GA runs 100 iterations with 200 individuals. The annealing example computes `value(next)` and `value(state)` once each per iteration for 100 iterations. How many evaluations does each make?

![[cmpe421_lecture_04_local_search_p017.jpg]]

- A) 3000. GA 20000 evaluations, annealing 200.
- B) 8000. GA 200 evaluations, annealing 100.
- C) 8000. GA 20000 evaluations, annealing 200.
- D) 3000. GA 200 evaluations, annealing 100.

> [!success]- Answer
> **C.** 8000 ($s=0.8$) has fitness 0.025, more than 0 at 3000. GA: $100\times 200=20000$ fitness evaluations. Annealing: $100\times 2=200$. The GA does about 100 times more work, as the slide says.
> - A: This picks the hill where many curves sit for a long time. The global maximum is the one with the larger fitness.
> - B: This counts the population once (200) and one value per annealing iteration (100). The GA evaluates all 200 individuals in every iteration.
> - D: This makes both slips: the wrong hill and the counts without the iterations.

### Q52 (local-search, slide 18)

True or false? (a) "A genetic algorithm is a faithful model of biological evolution." (b) "Engineers use genetic algorithms widely, which proves that they perform better than other methods."

- A) (a) true, (b) true.
- B) (a) false: real genes also encode the replication machinery. (b) true: circuit layout and job-shop scheduling show that they perform better.
- C) (a) true: a GA copies selection, crossover and mutation. (b) false: it is not clear if they are used for performance or for the attractive idea.
- D) (a) false: real genes also encode the replication machinery, while a GA's rules are fixed from outside. (b) false: it is not clear if they are used for performance or for the attractive idea.

> [!success]- Answer
> **D.** (a) In nature the genes also encode how they are copied. In a GA the crossover and mutation rules are fixed. (b) The slide says GAs are widely used, but it is not clear whether because of performance or because the idea is attractive.
> - A: Both claims go beyond the slide: a GA is a simplified model, and wide use is not proof of better performance.
> - B: (b) mixes up "widely used" with "proven better". The slide says more work is needed.
> - C: (a) ignores the difference between real genes and the fixed GA rules.

## 17. Continuous state spaces (local-search slides 19-22)

### Q53 (local-search, slide 19)

(a) How many dimensions does the state space have for 4 airports in the plane? (b) One airport and three cities at $(0,0)$, $(4,0)$, $(2,6)$. $f$ is the sum of squared distances from the cities to the airport, and we minimise $f$. Which place is better, $(1,1)$ or $(2,2)$?

- A) (a) 8. (b) $(2,2)$, because $f=32<38$.
- B) (a) 4. (b) $(2,2)$, because $f=32<38$.
- C) (a) 8. (b) $(1,1)$, because $f=38>32$.
- D) (a) 4. (b) $(1,1)$, because $f=38>32$.

> [!success]- Answer
> **A.** (a) Each airport has two coordinates: $4\times 2=8$. (b) At $(1,1)$: $(1+1)+(9+1)+(1+25)=38$. At $(2,2)$: $(4+4)+(4+4)+(0+16)=32$. We minimise, so $(2,2)$ is better.
> - B: This counts one dimension per airport. A position in the plane needs $x$ and $y$.
> - C: This picks the larger $f$, as if we maximised.
> - D: This makes both slips: one dimension per airport and the larger $f$.

### Q54 (local-search, slide 20)

$f(x,y)=x^{2}+y^{2}$, we minimise. Do one gradient step $\mathbf x\leftarrow\mathbf x-\alpha\nabla f(\mathbf x)$ from $(1,1)$ with $\alpha=0.1$. What is the new point and the new value of $f$ (it was 2)?

- A) $(1.2,\ 1.2)$, $f=2.88$.
- B) $(-1,\ -1)$, $f=2$.
- C) $(0.8,\ 0.8)$, $f=1.28$.
- D) $(0.9,\ 0.9)$, $f=1.62$.

> [!success]- Answer
> **C.** $\nabla f=(2x,\,2y)=(2,2)$ at $(1,1)$. $(1,1)-0.1\cdot(2,2)=(0.8,\,0.8)$. $f=0.64+0.64=1.28<2$.
> - A: This adds the gradient. Adding goes uphill: that is for maximising.
> - B: This forgets the step size $\alpha$: $(1,1)-(2,2)$.
> - D: This uses $\partial f/\partial x=x$ instead of $2x$: $(1,1)-0.1\cdot(1,1)$.

### Q55 (local-search, slide 21)

One airport and three cities at $(0,0)$, $(4,0)$, $(2,6)$, with $f(x,y)=\sum_c\big[(x-x_c)^{2}+(y-y_c)^{2}\big]$. Solve $\nabla f=0$. Where is the best place?

- A) $(6,\ 6)$
- B) $(3,\ 3)$
- C) $(2,\ 0)$
- D) $(2,\ 2)$

> [!success]- Answer
> **D.** $\partial f/\partial x=2(3x-(0+4+2))=2(3x-6)=0$ gives $x=2$. $\partial f/\partial y=2(3y-(0+0+6))=2(3y-6)=0$ gives $y=2$. The best place is the mean of the cities, $(2,2)$, with $f=32$.
> - A: This uses the sum of the coordinates (6 and 6), not the mean. $f(6,6)=128$.
> - B: This divides the sums by 2 instead of by the number of cities, 3. $f(3,3)=38$.
> - C: This takes the median of the $y$ values (0) instead of the mean. $f(2,0)=44$.

### Q56 (local-search, slide 22)

$f(x,y)=x^{2}+y^{2}$, current point $(1,1)$, we minimise. (a) Do one Newton-Raphson step $\mathbf x\leftarrow\mathbf x-\mathbf H^{-1}\nabla f$, where $\mathbf H$ is the matrix of second derivatives. (b) Empirical gradient with $\delta=0.1$: which of the moves $\pm\delta$ in $x$ and $\pm\delta$ in $y$ improve $f$?

- A) (a) $(-3,\ -3)$. (b) The moves $-\delta$ in $x$ and $-\delta$ in $y$.
- B) (a) $(0,\ 0)$. (b) The moves $-\delta$ in $x$ and $-\delta$ in $y$.
- C) (a) $(0,\ 0)$. (b) The moves $+\delta$ in $x$ and $+\delta$ in $y$.
- D) (a) $(-3,\ -3)$. (b) The moves $+\delta$ in $x$ and $+\delta$ in $y$.

> [!success]- Answer
> **B.** (a) $\nabla f=(2,2)$, $\mathbf H=\mathrm{diag}(2,2)$, $\mathbf H^{-1}=\mathrm{diag}(\tfrac12,\tfrac12)$. $(1,1)-\tfrac12(2,2)=(0,0)$: one step reaches the minimum. (b) $f(0.9,1)=f(1,0.9)=1.81<2$ and $f(1.1,1)=f(1,1.1)=2.21>2$. So the $-\delta$ moves improve $f$.
> - A: This multiplies by $\mathbf H$ instead of $\mathbf H^{-1}$: $(1,1)-(4,4)=(-3,-3)$.
> - C: The $+\delta$ moves raise $f$ to 2.21. They would be right only if we maximised.
> - D: This makes both slips: $\mathbf H$ instead of $\mathbf H^{-1}$ and the $+\delta$ moves.
