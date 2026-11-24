---
course: CMPE 421 Artificial Intelligence
lecture: 4
title: Informed and local search
tags: [cmpe421, lecture]
slides: lectures/lecture_04/informed-search.pdf
full_notes: lecture_notes/lecture_04/CMPE421_Lecture_04_Informed_and_Local_Search.pdf
---
# CMPE 421 · Lecture 4 · Informed and local search

> [!summary] In short
> - A heuristic $h(x)$ estimates the cost still to go. Best-first search expands the frontier node with the lowest $f$.
> - Greedy uses $f=h$: fast, not optimal, not complete as tree search. A* uses $f=g+h$.
> - A* tree search is optimal if $h$ is **admissible** ($h\le h^{*}$). A* graph search needs a **consistent** $h$.
> - Admissible heuristics come from relaxed problems. A larger admissible $h$ (dominance) expands fewer nodes.
> - IDA* is depth-first with a limit on $f$: optimal in $O(bd)$ memory.
> - Local search keeps one complete state and improves it: hill climbing, simulated annealing, beam search, genetic algorithms, gradient steps.

Main decks: `informed-search.pdf` (52 pages, sections 1 to 10) and `local-search.pdf` (22 pages, sections 12 to 17). Slide numbers are PDF page numbers. The number printed on each slide is one higher, and our PDF notes cite that printed number. Section 11 adds six slides from the instructor's `Ch.4.pdf`.

**Local search (sections 12 to 17) is not in the midterm.**

## 1. Heuristics and best-first search (slides 1-4)
![[cmpe421_lecture_04_informed_search_p001.jpg]]
![[cmpe421_lecture_04_informed_search_p002.jpg]]
![[cmpe421_lecture_04_informed_search_p003.jpg]]
![[cmpe421_lecture_04_informed_search_p004.jpg]]

**Notes.** A **heuristic** is a rule of thumb or educated guess. The **heuristic function** $h(x)$ estimates the cost of the cheapest path from node $x$ to a goal. Each node has three numbers: $g(x)$ = cost already paid, $h(x)$ = estimated cost still to go, $f(x)$ = the value the search ranks by. **Best-first search** always removes the frontier node with the lowest $f$.

| Algorithm | $f(x)$ |
|---|---|
| Uniform-cost | $g(x)$ |
| Greedy | $h(x)$ |
| A* | $g(x)+h(x)$ |

Romania: $h_{\mathrm{SLD}}(x)$ is the straight-line distance from city $x$ to Bucharest.

> [!warning] The slides disagree with themselves
> The $h_{\mathrm{SLD}}$ table next to the map says Fagaras 178 and Pitesti 98. The search trees on the same slides use Fagaras 176 and Pitesti 100 (the AIMA values). Our notes use 176 and 100. The routes found are the same with either pair.

## 2. Greedy best-first search (slides 5-14)
![[cmpe421_lecture_04_informed_search_p005.jpg]]
![[cmpe421_lecture_04_informed_search_p006.jpg]]
![[cmpe421_lecture_04_informed_search_p007.jpg]]
![[cmpe421_lecture_04_informed_search_p008.jpg]]
![[cmpe421_lecture_04_informed_search_p009.jpg]]
![[cmpe421_lecture_04_informed_search_p014.jpg]]

**Notes.** **Greedy search** uses $f=h$. It expands the node that *appears* closest to the goal and ignores the cost already paid.

From Arad: expand Arad (366), then Sibiu (253), then Fagaras (176), then remove Bucharest. Route Arad, Sibiu, Fagaras, Bucharest: $140+99+211=450$ km with 3 expansions. The optimal route is 418 km.

| Greedy | |
|---|---|
| Complete | no, it can loop (Iasi, Neamt, Iasi, ...). Yes in finite spaces with loop checking or graph search |
| Time | $O(b^{m})$ worst case, a good heuristic helps a lot |
| Space | $O(b^{m})$, keeps all nodes |
| Optimal | no |

## 3. A* search (slides 15-21)
![[cmpe421_lecture_04_informed_search_p015.jpg]]
![[cmpe421_lecture_04_informed_search_p016.jpg]]
![[cmpe421_lecture_04_informed_search_p017.jpg]]
![[cmpe421_lecture_04_informed_search_p018.jpg]]
![[cmpe421_lecture_04_informed_search_p019.jpg]]
![[cmpe421_lecture_04_informed_search_p020.jpg]]
![[cmpe421_lecture_04_informed_search_p021.jpg]]

**Notes.** **A\*** ranks a node by

$$f(x)=g(x)+h(x),$$

the estimated cost of the cheapest path to a goal through $x$. With $h=0$ it is uniform-cost search. Without $g$ it is greedy search.

> [!example] A* from Arad (5 expansions)
> 1. Arad ($f=366$): Sibiu $140+253=393$, Timisoara $118+329=447$, Zerind $75+374=449$.
> 2. Sibiu (393): Fagaras $239+176=415$, Rimnicu Vilcea $220+193=413$, Arad 646, Oradea 671.
> 3. Rimnicu Vilcea (413): Pitesti $317+100=417$, Craiova 526, Sibiu 553.
> 4. Fagaras (415): Bucharest $450+0=450$, Sibiu 591.
> 5. Pitesti (417): Bucharest $418+0=418$, Craiova 615, Rimnicu Vilcea 607.
> 6. Remove Bucharest (418): goal. Route Arad, Sibiu, Rimnicu Vilcea, Pitesti, Bucharest, **418 km**.
>
> Uniform-cost search needs 52 expansions for the same answer.

> [!warning] Watch out
> Bucharest (450) was *generated* in step 4. A* must test the goal when a node is **removed**, like UCS. Testing on generation would return the 450 km route.

## 4. Admissibility and optimality (slides 22-24)
![[cmpe421_lecture_04_informed_search_p022.jpg]]
![[cmpe421_lecture_04_informed_search_p023.jpg]]
![[cmpe421_lecture_04_informed_search_p024.jpg]]

**Notes.** Let $h^{*}(x)$ be the true cost of the best path from $x$ to a goal. $h$ is **admissible** if it never overestimates:

$$0\le h(x)\le h^{*}(x)\quad\text{for every }x.$$

Then $h(\text{goal})=0$. $h_{\mathrm{SLD}}$ is admissible because a road is never shorter than the straight line.

**Theorem.** If $h$ is admissible, A* **tree** search never returns a non-optimal solution.

**Completeness.** If no infinite path has a finite cost (for example every step costs at least $\varepsilon>0$ and $b$ is finite), A* tree search returns a solution on any solvable problem.

> [!warning] Watch out
> Admissible is about the true remaining cost $h^{*}$, not about single step costs. One state that overestimates is enough to break it: $h_{\mathrm{SLD}}+10$ gives $110>101=h^{*}$ at Pitesti.

## 5. A* with graph search: consistency (slides 25-31)
![[cmpe421_lecture_04_informed_search_p025.jpg]]
![[cmpe421_lecture_04_informed_search_p030.jpg]]
![[cmpe421_lecture_04_informed_search_p031.jpg]]

**Notes.** $h$ is **consistent** if for every node $x$ and child $y$ with step cost $c(x,y)$:

$$h(x)\le c(x,y)+h(y).$$

This is a triangle inequality. Then $f(y)=g(x)+c(x,y)+h(y)\ge g(x)+h(x)=f(x)$: **$f$ never decreases along a path**, A* expands nodes in order of increasing $f$, and A* **graph** search is optimal. Consistent implies admissible (with $h(\text{goal})=0$). The converse is false.

> [!example] Inconsistent heuristic (slide 30)
> Edges S-A 2, S-B 2, A-C 1, B-C 2, C-G 4. $h$: S 2, A 5, B 2, C 1. Admissible, but A to C: $5>1+1$.
> A* expands S, then B ($f=4$), then C via B ($f=5$), then A ($f=7$), which gives C via A with $f=3+1=4$. $f$ dropped.
> Tree search expands C again and returns S-A-C-G, cost **7** (optimal). Plain graph search skips C (already explored) and returns S-B-C-G, cost **8**.

`A*-Graph-Search` (slide 31) fixes this: it re-expands a state when a path with lower $f$ is found.

## 6. Properties of A* (slides 32-36)
![[cmpe421_lecture_04_informed_search_p036.jpg]]

**Notes.** $C^{*}$ = cost of the optimal solution, $d$ = depth of the solution.

| A* | |
|---|---|
| Complete | yes, unless there are infinitely many nodes with $f\le f(G)$ |
| Time | whole state space in the worst case, $O(d)$ in the best case |
| Space | keeps all nodes in memory (this runs out first) |
| Optimal | yes on trees with admissible $h$. On graphs with consistent $h$ or with `A*-Graph-Search` |

A* expands all nodes with $f<C^{*}$, some with $f=C^{*}$, and (consistent $h$) none with $f>C^{*}$.

> [!warning] Slide slip
> The slide says "if $f$ is consistent". Consistency is a property of the heuristic $h$.

## 7. Designing heuristics: relaxed problems (slides 37-43)
![[cmpe421_lecture_04_informed_search_p037.jpg]]
![[cmpe421_lecture_04_informed_search_p038.jpg]]
![[cmpe421_lecture_04_informed_search_p039.jpg]]
![[cmpe421_lecture_04_informed_search_p041.jpg]]
![[cmpe421_lecture_04_informed_search_p043.jpg]]

**Notes.** A **relaxation** removes some constraints of the problem. Every real solution is still a solution of the relaxed problem, so the optimal relaxed cost is never larger than $h^{*}$: it is an **admissible heuristic**.

- Romania: allow driving off-road, so $h$ = straight-line distance.
- TSP: allow any connected tree instead of a tour, so $h$ = cost of a minimum spanning tree, computed in $O(n^{2})$.
- 8-puzzle, relaxation 1 (a tile may jump anywhere): $h_1$ = number of misplaced tiles.
- 8-puzzle, relaxation 2 (a tile may move to any adjacent square): $h_2$ = total Manhattan distance.

For the start state on the slides: $h_1=6$, $h_2=14$, $h^{*}=20$. A* expansions in our run: 3666 with $h_1$, 282 with $h_2$, 48389 with no heuristic.

## 8. Dominance (slides 44-45)
![[cmpe421_lecture_04_informed_search_p044.jpg]]
![[cmpe421_lecture_04_informed_search_p045.jpg]]

**Notes.** If $h_a$ and $h_b$ are admissible and $h_a(x)\le h_b(x)$ for all $x$, then $h_b$ **dominates** $h_a$ and is better for search. For the 8-puzzle $h_1\le h_2\le h^{*}$.

$h(x)=\max(h_a(x),h_b(x))$ is admissible and dominates both. Faking dominance: $c\cdot h$ with $c>1$ is probably not admissible, so the solution may not be optimal, but the search is faster. A larger $c$ is closer to greedy search.

> [!warning] Watch out
> The maximum of admissible heuristics is admissible. The sum is not in general. The blank does not count as a misplaced tile.

## 9. IDA* (slides 46-51)
![[cmpe421_lecture_04_informed_search_p046.jpg]]
![[cmpe421_lecture_04_informed_search_p051.jpg]]

**Notes.** **IDA\*** is iterative deepening on $f$. Each round is a depth-first search that backtracks at every node with $f>f_{\max}$. The first limit is $h(\text{start})$. The next limit is the **smallest** $f$ that was cut off.

Romania: the limits are 366, 393, 413, 415, 417, 418. IDA* needs 20 expansions where A* needs 5, because $f$ takes many different values and each round adds little.

| IDA* | |
|---|---|
| Complete | yes, unless infinitely many nodes have $f\le f(G)$ |
| Time | like A* if $f$ is an integer and the number of nodes with $f\le k$ grows exponentially in $k$ |
| Space | $O(bd)$ |
| Optimal | yes, with admissible $h$ |

> [!warning] Source error (slide 51)
> The slide says "IDA* expands no nodes with $f(x)\ge C^{*}$". That is too strong. In the last round the limit is $C^{*}$ and nodes with $f=C^{*}$ can be expanded. Correct: no nodes with $f(x)>C^{*}$.

## 10. Summary of informed search (slide 52)
![[cmpe421_lecture_04_informed_search_p052.jpg]]

**Notes.** Greedy: lowest $h$, fast, incomplete, not optimal. A*: lowest $g+h$, optimal with an admissible (tree) or consistent (graph) heuristic, memory-hungry. IDA*: A*'s optimality in $O(bd)$ memory, paid for with repeated rounds.

## 11. From the instructor's deck `Ch.4.pdf` (Ch.4 pages 10, 11, 12, 22, 40, 41)
![[cmpe421_lecture_03_ch_4_p010.jpg]]
![[cmpe421_lecture_03_ch_4_p011.jpg]]
![[cmpe421_lecture_03_ch_4_p012.jpg]]
![[cmpe421_lecture_03_ch_4_p022.jpg]]
![[cmpe421_lecture_03_ch_4_p040.jpg]]
![[cmpe421_lecture_03_ch_4_p041.jpg]]

**Notes.** The Ch.4 deck covers the same A* material with different small examples.

- **Page 10**: UCS orders by backward cost $g$, greedy by forward cost $h$, A* by the sum.
- **Page 11, when to stop**: S-A 2, A-G 2, S-B 2, B-G 3, $h(A)=2$, $h(B)=1$. S-B-G ($f=5$) is enqueued before S-A-G ($f=4$). Stop only when a goal is **dequeued**.
- **Page 12, is A\* optimal**: S-A 1, A-G 3, S-G 5, $h(A)=6$. S-G ($f=5$) comes out before S-A ($f=7$), so A* returns cost 5 instead of 4. The heuristic overestimated: it is not admissible.
- **Page 22**: UCS expands equally in all directions. A* expands mainly toward the goal.
- **Pages 40-41**: graph search with an inconsistent heuristic returns a worse path. Consistency: $h(A)-h(C)\le \text{cost}(A\text{ to }C)$ on every arc.

## 12. Local search: iterative improvement (local-search slides 1-3)
![[cmpe421_lecture_04_local_search_p001.jpg]]
![[cmpe421_lecture_04_local_search_p002.jpg]]
![[cmpe421_lecture_04_local_search_p003.jpg]]

**Notes.** In many optimisation problems the path does not matter. The state is a complete configuration and we want one that satisfies the constraints or has the best value. **Iterative improvement** keeps a single current state and tries to improve it. It uses constant space.

$n$-queens: one queen per column, $h$ = number of attacking pairs, goal $h=0$. Move a queen within its column to reduce $h$.

## 13. Hill climbing (local-search slides 5-6)
![[cmpe421_lecture_04_local_search_p005.jpg]]
![[cmpe421_lecture_04_local_search_p006.jpg]]

**Notes.** **Hill climbing** moves to the best neighbour and stops when no neighbour is **strictly** better. It gets stuck at:
- a **local maximum** (higher than its neighbours, lower than the global maximum),
- a **plateau**: a "flat" local maximum, or a **shoulder** from which progress is still possible.

**Random-restart hill climbing** repeats from random starting points. With finitely many local maxima, $P(\text{complete})\to1$ as the number of restarts grows. If one climb succeeds with probability $p$, the expected number of climbs is $1/p$.

8-queens in our run: plain hill climbing solves 14% of random starts, 95% with sideways moves.

## 14. Simulated annealing (local-search slides 7-10)
![[cmpe421_lecture_04_local_search_p007.jpg]]
![[cmpe421_lecture_04_local_search_p008.jpg]]
![[cmpe421_lecture_04_local_search_p009.jpg]]
![[cmpe421_lecture_04_local_search_p010.jpg]]

**Notes.** **Simulated annealing** picks a *random* neighbour. $\Delta E$ is the change in value. An uphill move ($\Delta E>0$) is always taken. A downhill move is taken with probability $e^{\Delta E/T}$. The **temperature** $T$ decreases over time. High $T$ is a random walk. Low $T$ is hill climbing.

At fixed $T$ the state distribution approaches the Boltzmann distribution $p(x)=\alpha\,e^{E(x)/kT}$. If $T$ decreases slowly enough, the probability of reaching the best state approaches 1.

> [!warning] Source error (slide 8)
> The printed `value` function is `return -abs(0.5-state)`, one hill at 0.5. The text and the plot on slide 9 use two hills (near 0.3 and 0.8). The line `target = cmath.e ** (DeltaE/T):` has a stray colon.

## 15. Local beam search (local-search slide 11)
![[cmpe421_lecture_04_local_search_p011.jpg]]

**Notes.** **Local beam search** keeps $k$ states. Each step generates all successors of all $k$ states and keeps the $k$ best of the whole pool. This is not $k$ independent searches: good states recruit the others. The risk is that all $k$ states end on the same hill. Stochastic beam search picks the $k$ successors at random, biased toward good ones.

## 16. Genetic algorithms (local-search slides 12-18)
![[cmpe421_lecture_04_local_search_p012.jpg]]
![[cmpe421_lecture_04_local_search_p013.jpg]]
![[cmpe421_lecture_04_local_search_p014.jpg]]
![[cmpe421_lecture_04_local_search_p015.jpg]]
![[cmpe421_lecture_04_local_search_p016.jpg]]
![[cmpe421_lecture_04_local_search_p017.jpg]]
![[cmpe421_lecture_04_local_search_p018.jpg]]

**Notes.** A **genetic algorithm** is stochastic beam search where a successor comes from two parents. Each state (an individual) is a string.
- **Selection**: choose parents with probability proportional to **fitness**.
- **Crossover**: cut both parents at the same random point and join the front of one to the back of the other.
- **Mutation**: with small probability change one random character.

8-queens: fitness = number of non-attacking pairs = $28-h$. With fitnesses 24, 23, 20, 11 the selection probabilities are $24/78=31\%$, 29%, 26%, 14%.

> [!warning] Source error (slide 16)
> The GA program on the slide has negative fitness for almost every state. Roulette-wheel selection needs non-negative fitness. With a negative sum the probabilities are proportional to $|\text{fitness}|$, so the worst individuals are chosen most often.

## 17. Continuous state spaces (local-search slides 19-22)
![[cmpe421_lecture_04_local_search_p019.jpg]]
![[cmpe421_lecture_04_local_search_p020.jpg]]
![[cmpe421_lecture_04_local_search_p021.jpg]]
![[cmpe421_lecture_04_local_search_p022.jpg]]

**Notes.** Example: place three airports in Romania, state $(x_1,y_1,x_2,y_2,x_3,y_3)$, minimise the sum of squared distances from each city to its nearest airport. The **gradient** $\nabla f$ is the vector of partial derivatives. It points in the direction of steepest slope.

$$\mathbf x\leftarrow\mathbf x-\alpha\,\nabla f(\mathbf x)\quad(\text{minimise, step size }\alpha>0).$$

**Newton-Raphson** iterates $\mathbf x\leftarrow\mathbf x-\mathbf H^{-1}(\mathbf x)\,\nabla f(\mathbf x)$ with $H_{ij}=\partial^{2}f/\partial x_i\partial x_j$. The **empirical gradient** tries a $\pm\delta$ change in each coordinate.

> [!warning] Watch out
> $\nabla f=\mathbf 0$ holds at local minima, local maxima and saddle points. It does not prove a global minimum.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| $g$, $h$, $f$ | cost paid, estimated cost to go, ranking value |
| Greedy | $f=h$ |
| A* | $f=g+h$, goal test on removal |
| Admissible | $0\le h(x)\le h^{*}(x)$. A* tree search optimal |
| Consistent | $h(x)\le c(x,y)+h(y)$. $f$ nondecreasing, A* graph search optimal |
| Relaxed problem | optimal cost of an easier problem = admissible $h$ |
| $h_1$, $h_2$ (8-puzzle) | misplaced tiles, total Manhattan distance |
| Dominance | $h_a\le h_b\le h^{*}$: $h_b$ is better. $\max(h_a,h_b)$ is admissible |
| IDA* | DFS with limit on $f$, next limit = smallest cut-off $f$, space $O(bd)$ |
| Hill climbing | best neighbour while strictly better |
| Simulated annealing | downhill with probability $e^{\Delta E/T}$, $T$ decreasing |
| Local beam search | keep the $k$ best of all successors |
| Genetic algorithm | selection by fitness, crossover, mutation |
| Gradient step | $\mathbf x\leftarrow\mathbf x-\alpha\nabla f(\mathbf x)$ |

## Check yourself
> [!question]- 1. The node Arad, Sibiu, Fagaras is in the frontier. Compute $f$ for UCS, greedy and A*.
> $g=140+99=239$, $h=176$. UCS: 239. Greedy: 176. A*: $239+176=415$.

> [!question]- 2. Is $h=h_{\mathrm{SLD}}+10$ (and 0 at Bucharest) admissible?
> No. Pitesti: $100+10=110>h^{*}=101$. One overestimating state is enough.

> [!question]- 3. Show that $h(x)=0$ is consistent. Which algorithm is A* graph search then?
> $0\le c(x,y)+0$ because step costs are non-negative. $f=g$: uniform-cost graph search.

> [!question]- 4. 8-puzzle state 1 2 3 / 4 5 6 / 7 _ 8. Compute $h_1$, $h_2$, $h^{*}$. Is "misplaced tiles counting the blank" admissible?
> Only tile 8 is off by one square: $h_1=h_2=h^{*}=1$. Counting the blank gives $2>1$, so it is not admissible.

> [!question]- 5. A* with a consistent $h$, $C^{*}=50$. Nodes $u$ with $f=48$ and $v$ with $f=53$ are generated. Which is expanded?
> $u$ is certainly expanded ($f<C^{*}$). $v$ is certainly not ($f>C^{*}$).

> [!question]- 6. In IDA* on Romania, the cut-off values of round 2 are 646, 415, 671, 413, 447, 449. What is the next limit and why?
> 413, the smallest. A larger limit could jump over a cheaper solution.

## Links
- Course index: [[CMPE421 Home]]
- Previous: [[CMPE421 L03 - Uninformed search]] · Next: [[CMPE421 L05 - Constraint satisfaction problems]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_04_Informed_and_Local_Search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_04/CMPE421_Lecture_04_Informed_and_Local_Search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_04/CMPE421_Lecture_04_Informed_and_Local_Search.pdf`
- Original slides (PDF): [informed-search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_04/informed-search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_04/informed-search.pdf`
- Original slides (PDF): [local-search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_04/local-search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_04/local-search.pdf`
- Instructor's deck "Search continued" (PDF, 50 pages): [Ch.4.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/Ch.4.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/Ch.4.pdf`

Slides not embedded (informed-search): 10-13, 26-29, 32-35, 40, 42, 47-50 (earlier builds of the next embedded slide). Local-search: 4 (outline). Ch.4: only pages 10, 11, 12, 22, 40, 41 are embedded. The rest repeats the material above or is a video placeholder.
