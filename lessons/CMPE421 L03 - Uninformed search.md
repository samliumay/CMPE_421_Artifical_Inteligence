---
course: CMPE 421 Artificial Intelligence
lecture: 3
title: Uninformed search
tags: [cmpe421, lecture]
slides: lectures/lecture_03/Ch.3.pdf
full_notes: lecture_notes/lecture_03/CMPE421_Lecture_03_Uninformed_Search.pdf
---
# CMPE 421 · Lecture 3 · Uninformed search

> [!summary] In short
> - A search problem = state space + successor function (actions, costs) + start state + goal test. A solution is a plan.
> - A search-tree node is a whole **path**, not a state. A finite graph with a cycle gives an infinite tree.
> - DFS, BFS and UCS are one loop. Only the fringe differs: stack, queue, priority queue on path cost $g$.
> - DFS: linear memory, not complete, not optimal. BFS: complete, fewest actions, exponential memory.
> - UCS is the only one that is optimal for general positive costs. It must test the goal when a node is removed.
> - Iterative deepening gives BFS's answer with DFS's memory. Graph search adds an explored set.

Main deck: `Ch.3.pdf` (55 pages). Slide numbers are the page numbers of this PDF. Sections 12 to 15 add slides from the handout `uninformed-search.pdf`.

## 1. Reflex agents and planning agents (slides 2-10)
![[cmpe421_lecture_03_ch_3_p002.jpg]]
![[cmpe421_lecture_03_ch_3_p004.jpg]]
![[cmpe421_lecture_03_ch_3_p005.jpg]]
![[cmpe421_lecture_03_ch_3_p008.jpg]]

**Notes.** A **reflex agent** chooses from the current percept (and maybe memory). It does not consider the future consequences of its actions. It considers how the world **is**. A **planning agent** asks "what if?". It needs a **model** of how the world reacts to actions and a **goal test**. It considers how the world **would be**.

Complete planning finds some plan. Optimal planning finds the cheapest one. Replanning plans again after each step.

> [!warning] Watch out
> Having a model does not make an agent a planning agent. A model-based reflex agent models the *current* state. Only a planning agent uses the model to predict *future* states.

## 2. Search problems (slides 12-14)
![[cmpe421_lecture_03_ch_3_p012.jpg]]
![[cmpe421_lecture_03_ch_3_p013.jpg]]
![[cmpe421_lecture_03_ch_3_p014.jpg]]

**Notes.** A **search problem** has four parts:
- a **state space** (all states),
- a **successor function**: for each state, the actions, the resulting states and their costs,
- a **start state**,
- a **goal test**.

A **solution** is a sequence of actions (a plan) from the start state to a goal state.

Romania: states are cities, successors are roads with cost = distance, start is Arad, goal test is "state == Bucharest". Two solutions: via Fagaras, $140+99+211=450$ km (3 roads). Via Rimnicu Vilcea and Pitesti, $140+80+97+101=418$ km (4 roads). Fewest actions and least cost are different questions.

## 3. What is in a state space (slides 15-17)
![[cmpe421_lecture_03_ch_3_p015.jpg]]
![[cmpe421_lecture_03_ch_3_p016.jpg]]
![[cmpe421_lecture_03_ch_3_p017.jpg]]

**Notes.** The **world state** has every detail. A **search state** keeps only what planning needs (abstraction). Pathing needs only $(x,y)$. Eat-all-dots needs $(x,y)$ plus one boolean per dot.

Counting rule: independent parts **multiply**. $k$ yes/no flags give $2^{k}$.

> [!example] Pac-Man sizes (slide 16)
> 120 positions, 30 dots, 2 ghosts with 12 positions each, 4 facings.
> World states: $120\cdot2^{30}\cdot12^{2}\cdot4\approx7.4\times10^{13}$. Pathing: 120. Eat-all-dots: $120\cdot2^{30}\approx1.3\times10^{11}$.

> [!warning] Slide slip
> In the PDF export the exponents are flattened: "230" means $2^{30}$ and "122" means $12^{2}$.

## 4. State space graphs and search trees (slides 19-24)
![[cmpe421_lecture_03_ch_3_p019.jpg]]
![[cmpe421_lecture_03_ch_3_p020.jpg]]
![[cmpe421_lecture_03_ch_3_p021.jpg]]
![[cmpe421_lecture_03_ch_3_p022.jpg]]
![[cmpe421_lecture_03_ch_3_p023.jpg]]
![[cmpe421_lecture_03_ch_3_p024.jpg]]

**Notes.** In a **state space graph** nodes are states, arcs are successors, and **each state occurs only once**. A **search tree** is a "what if" tree of plans. The start state is the root, children are successors. Each tree **node is an entire path** in the graph, so one state can appear in many nodes. Both are built on demand, as little as possible.

The 4-state graph (S, a, b, G with a and b connected both ways) has an infinite search tree: the cycle a, b, a, b never ends.

> [!warning] Watch out
> A finite graph does not give a finite tree. One cycle is enough to make the tree infinite.

## 5. Tree search (slides 26-30)
![[cmpe421_lecture_03_ch_3_p026.jpg]]
![[cmpe421_lecture_03_ch_3_p027.jpg]]
![[cmpe421_lecture_03_ch_3_p028.jpg]]
![[cmpe421_lecture_03_ch_3_p029.jpg]]
![[cmpe421_lecture_03_ch_3_p030.jpg]]

**Notes.**
- The **fringe** (frontier) is the set of tree nodes generated but not yet expanded.
- To **expand** a node is to generate all its children.
- The **strategy** decides which fringe node to expand next.

Tree search is one loop: remove a node from the fringe, test it, expand it, add the children. Try to expand as few nodes as possible. Slide 30 shows the fringe growing on the tiny graph: s, then s-d, s-e, s-p, and so on until s-d-e-r-f-G.

## 6. Depth-first search (slides 32-35)
![[cmpe421_lecture_03_ch_3_p032.jpg]]
![[cmpe421_lecture_03_ch_3_p034.jpg]]
![[cmpe421_lecture_03_ch_3_p035.jpg]]

**Notes.** Four questions judge a strategy: **complete** (finds a solution if one exists), **optimal** (finds the least-cost path), **time**, **space**. Symbols: $b$ = branching factor, $m$ = maximum depth, $s$ = depth of the shallowest solution. The whole tree has $1+b+b^{2}+\dots+b^{m}=O(b^{m})$ nodes. The bottom tier dominates.

**DFS**: expand a deepest node first. The fringe is a **LIFO stack**.

| DFS | |
|---|---|
| Time | $O(b^{m})$ |
| Space | $O(bm)$, only the siblings on the path to the root |
| Complete | no ($m$ can be infinite). Yes in finite spaces if cycles are prevented |
| Optimal | no, it returns the "leftmost" solution |

> [!warning] Watch out
> Do not mix up $b^{m}$ and $bm$. For $b=10$, $m=5$ they are $100\,000$ and $50$. In the PDF export "O(bm)" stands for both: time is $b^{m}$, space is $b\cdot m$.
> On the tiny graph DFS returns S-d-e-r-f-G (cost 10) with the leftmost-first rule. With the opposite tie-breaking it returns S-e-r-f-G (cost 14). One lucky run does not make DFS optimal.

## 7. Breadth-first search (slides 37-40)
![[cmpe421_lecture_03_ch_3_p037.jpg]]
![[cmpe421_lecture_03_ch_3_p038.jpg]]
![[cmpe421_lecture_03_ch_3_p040.jpg]]

**Notes.** **BFS**: expand a shallowest node first. The fringe is a **FIFO queue**.

| BFS | |
|---|---|
| Time | $O(b^{s})$, all nodes above the shallowest solution |
| Space | $O(b^{s})$, roughly the last tier |
| Complete | yes ($s$ is finite if a solution exists) |
| Optimal | only if all step costs are equal |

On the tiny graph BFS returns S-e-r-f-G, cost 14 (4 actions). In Romania it returns the 450 km route.

BFS wins when solutions are shallow. DFS wins when memory is tight and solutions are deep or plentiful.

## 8. Iterative deepening (slide 43)
![[cmpe421_lecture_03_ch_3_p043.jpg]]

**Notes.** Run DFS with depth limit 1, then 2, then 3, until a solution appears. This gives DFS's memory, $O(bs)$, with BFS's completeness and shallowest answer. The repeated work is small because most nodes are in the lowest tier searched. Details and the cost formula are in section 14.

## 9. Uniform-cost search (slides 44-48)
![[cmpe421_lecture_03_ch_3_p044.jpg]]
![[cmpe421_lecture_03_ch_3_p046.jpg]]
![[cmpe421_lecture_03_ch_3_p047.jpg]]
![[cmpe421_lecture_03_ch_3_p048.jpg]]

**Notes.** BFS finds the fewest actions, not the least cost. **UCS**: expand a cheapest node first. The fringe is a **priority queue** on the cumulative path cost $g$. Nodes come out in order of increasing cost (cost contours). With equal step costs UCS behaves like BFS.

$C^{*}$ = cost of the optimal solution, $\varepsilon$ = smallest step cost. The effective depth is about $C^{*}/\varepsilon$.

| UCS | |
|---|---|
| Time | $O(b^{C^{*}/\varepsilon})$ |
| Space | $O(b^{C^{*}/\varepsilon})$ |
| Complete | yes, if the best solution has finite cost and $\varepsilon>0$ |
| Optimal | yes (with the goal test on removal) |

The bad side: UCS explores in every direction and knows nothing about where the goal is. Informed search fixes that ([[CMPE421 L04 - Informed and local search]]).

> [!example] Results
> Tiny graph: UCS returns S-d-e-r-f-G, cost $3+2+2+1+2=10$. Romania: UCS returns Arad, Sibiu, Rimnicu Vilcea, Pitesti, Bucharest, 418 km. Bucharest enters the fringe at 450 (via Fagaras) but the cheaper copy at 418 comes out first.

## 10. The one queue (slide 53)
![[cmpe421_lecture_03_ch_3_p053.jpg]]

**Notes.** All these algorithms are the same loop. Conceptually every fringe is a priority queue. For DFS and BFS a stack and a queue avoid the $\log n$ overhead.

Tie-breaking in our traces: successors in alphabetical order, DFS explores the alphabetically first child first, equal $g$ comes out first-in first-out.

## 11. Search and models (slides 54-55)
![[cmpe421_lecture_03_ch_3_p054.jpg]]
![[cmpe421_lecture_03_ch_3_p055.jpg]]

**Notes.** Search operates over **models** of the world. The agent does not try the plans in the real world. Planning is all in simulation, so the search is only as good as the model. An optimal plan for a wrong model can fail.

## 12. Handout: problem types (handout pages 2-5, 10)
![[cmpe421_lecture_03_uninformed_search_p002.jpg]]
![[cmpe421_lecture_03_uninformed_search_p003.jpg]]
![[cmpe421_lecture_03_uninformed_search_p004.jpg]]
![[cmpe421_lecture_03_uninformed_search_p005.jpg]]
![[cmpe421_lecture_03_uninformed_search_p010.jpg]]

**Notes.** These slides are only in the handout. What the agent knows decides the kind of solution.

| Problem type | When | Solution | Vacuum example |
|---|---|---|---|
| Classical | deterministic, fully observable | a fixed sequence | start in state 1: [Suck, Right, Suck] |
| Conformant | no sensors | a sequence that works from every state | [Right, Suck, Left, Suck] |
| Contingency | nondeterministic or partially observable | an if-then plan (policy) | [Right, if dirt then Suck] |
| Exploration | unknown state space | learn by acting (online) | agent does not know the other rooms |

**Offline** problem solving builds the whole solution before acting. **Online** gathers knowledge while acting. This lecture is offline search for classical problems. Handout page 10 shows the 8 states of the vacuum world as a graph.

## 13. Handout: nodes, eager and cautious goal test (handout pages 17, 19)
![[cmpe421_lecture_03_uninformed_search_p017.jpg]]
![[cmpe421_lecture_03_uninformed_search_p019.jpg]]

**Notes.** A **state** is a configuration of the world. A **node** is a data structure in the search tree: it has a state, a parent, an action, a depth and a path cost $g$. A state has none of these.

- **Eager** tree search tests a node when it is **generated**. It generates fewer nodes.
- **Cautious** tree search tests a node when it is **removed** from the frontier. This is necessary for an optimal solution with UCS.

BFS should be eager. UCS must be cautious: an eager UCS would stop when it first generates Bucharest via Fagaras (450) and return the worse route.

## 14. Handout: depth-limited search and IDS (handout pages 57, 58, 67)
![[cmpe421_lecture_03_uninformed_search_p057.jpg]]
![[cmpe421_lecture_03_uninformed_search_p058.jpg]]
![[cmpe421_lecture_03_uninformed_search_p067.jpg]]

**Notes.** **Depth-limited search** is DFS that backtracks at depth = limit $l$. It returns a solution, **Failure** (no solution at all) or **Cutoff** (the limit cut something off). **Iterative deepening (IDS)** runs it with $l=0,1,2,\dots$.

With the solution at depth $d$, a node at depth $i$ is generated in $d-i+1$ rounds:

$$(d+1)\,b^{0}+d\,b^{1}+(d-1)\,b^{2}+\dots+1\cdot b^{d}=O(b^{d}).$$

IDS: complete, time $O(b^{d})$, space $O(bd)$, optimal if every step costs 1. For $b=10$, $d=5$ IDS generates $123\,456$ nodes against $111\,111$ for eager BFS, only about 11% more.

The deck starts with limit 1, the handout with limit 0. The answer is the same.

## 15. Handout: summary, repeated states, graph search (handout pages 30, 68-70)
![[cmpe421_lecture_03_uninformed_search_p030.jpg]]
![[cmpe421_lecture_03_uninformed_search_p068.jpg]]
![[cmpe421_lecture_03_uninformed_search_p069.jpg]]
![[cmpe421_lecture_03_uninformed_search_p070.jpg]]

**Notes.**

| Criterion | BFS | UCS | DFS | Depth-limited | IDS |
|---|---|---|---|---|---|
| Complete | yes | yes, if $\varepsilon>0$ | no | yes, if $l\ge d$ | yes |
| Time | $b^{d}$ (eager) | $b^{\lceil C^{*}/\varepsilon\rceil}$ | $b^{m}$ | $b^{l}$ | $b^{d}$ |
| Space | $b^{d}$ | $b^{\lceil C^{*}/\varepsilon\rceil}$ | $bm$ | $bl$ | $bd$ |
| Optimal | if costs equal | yes | no | no | if costs equal |
| Fringe | queue | priority queue on $g$ | stack | stack | stack |

The handout's $d$ is the deck's $s$ (depth of the shallowest solution).

Failing to detect repeated states can turn a linear problem into an exponential one. **Graph search** is tree search plus an **explored set** of states already expanded. A removed node whose state is in the explored set is skipped. It can take exponentially less time when many paths reach the same state. It can take exponentially more space when the space is tree-like, and it removes the linear-memory advantage of DFS and IDS.

> [!warning] Source error (handout page 30)
> The handout writes the BFS time as $1+b+\dots+b^{d}+b(b^{d}-1)=O(b^{d})$. The last term is $b^{d+1}-b$, so the cautious count is $O(b^{d+1})$. $O(b^{d})$ is right for eager BFS. For $b=10$, $d=5$: eager about $111\,111$ nodes, cautious up to $1\,111\,101$. The same page calls $d$ the depth of the "least-cost" solution. For BFS it is the shallowest.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Search problem | state space, successor function, start state, goal test |
| Search state | only the details planning needs (abstraction) |
| Tree node | a path from the root, with state, parent, depth, path cost $g$ |
| Fringe | generated but not yet expanded nodes |
| $b$, $m$, $s$ (or $d$) | branching factor, maximum depth, depth of the shallowest solution |
| $C^{*}$, $\varepsilon$ | optimal solution cost, smallest step cost |
| Tree size | $1+b+\dots+b^{m}=O(b^{m})$ |
| DFS | stack. Time $O(b^{m})$, space $O(bm)$. Not complete, not optimal |
| BFS | queue. Time and space $O(b^{s})$. Complete. Optimal for equal costs |
| UCS | priority queue on $g$. $O(b^{C^{*}/\varepsilon})$. Complete and optimal |
| IDS | limits $0,1,2,\dots$. Time $O(b^{d})$, space $O(bd)$ |
| Eager / cautious | goal test on generation / on removal |
| Graph search | tree search + explored set |

## Check yourself
> [!question]- 1. A $10\times10$ grid world has 20 dots. How many eat-all-dots states?
> Position: 100. Dots: $2^{20}=1\,048\,576$. Product: $104\,857\,600\approx1.0\times10^{8}$.

> [!question]- 2. Without sensors, does [Suck, Right, Suck] work from every start state of the vacuum world?
> No. Start in B with both rooms dirty: Suck cleans B, Right bumps the wall, Suck does nothing. A stays dirty. One failing start state is enough, so the plan is not conformant.

> [!question]- 3. For $b=2$ and a solution at depth $d=3$, how many nodes do IDS and eager BFS generate?
> IDS: $4\cdot1+3\cdot2+2\cdot4+1\cdot8=26$. BFS: $1+2+4+8=15$.

> [!question]- 4. Why do eager and cautious BFS return the same path, while eager UCS can return a worse one?
> BFS removes nodes in arrival order, so the first goal generated is also the first goal removed. UCS removes by $g$, so a goal generated first can be more expensive than a goal generated later.

> [!question]- 5. If every road in Romania had cost 1, which route would UCS return?
> Costs become counts of roads, so UCS behaves like BFS: Arad, Sibiu, Fagaras, Bucharest (3 roads).

> [!question]- 6. UCS with graph search on the tiny graph: what changes?
> The node S-e (cost 9) is skipped when removed, because e was already expanded through S-d-e (cost 5). Same answer S-d-e-r-f-G, cost 10, with 8 expansions instead of 9.

## Links
- Course index: [[CMPE421 Home]]
- Previous: [[CMPE421 L02 - Intelligent agents]] · Next: [[CMPE421 L04 - Informed and local search]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_03_Uninformed_Search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_03/CMPE421_Lecture_03_Uninformed_Search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_03/CMPE421_Lecture_03_Uninformed_Search.pdf`
- Original slides (PDF): [Ch.3.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/Ch.3.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/Ch.3.pdf`
- Handout (PDF, 77 pages): [uninformed-search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/uninformed-search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/uninformed-search.pdf`
- The deck `Ch.4.pdf` in the same folder ("Search continued") belongs to the Lecture 4 topic.

Slides not embedded (Ch.3): 1, 3, 11, 18, 25, 31, 33, 36, 39, 45 (titles), 6, 7, 9, 10, 41, 42, 49-52 (video placeholders). Handout: only pages 2-5, 10, 17, 19, 30, 57, 58, 67-70 are embedded. The rest are step-by-step traces of BFS, DFS, UCS and IDS, and repeats of the deck.
