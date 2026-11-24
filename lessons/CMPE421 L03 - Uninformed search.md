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

> [!question]- Check (Ch.3, slides 4-5): The slide asks "Can a reflex agent be rational?". Answer it, and say what goes wrong for the robot in the picture.
> Yes, in some environments. A reflex agent is rational when the best action depends only on the current percept, for example "if this room is dirty, suck".
> In the picture it fails. The rule "apple ahead, move forward" ignores the gap between the two blocks. The agent never asks what happens after the move, so it falls.
> So a reflex agent is rational only where no look-ahead is needed.

> [!question]- Check (Ch.3, slide 8): A maze has three routes to the exit, with 9, 12 and 15 steps. Planner X returns the 15-step route. Can X be a complete planner? Can it be an optimal planner? When is replanning a good idea?
> Complete: yes, it can be. Complete only means "finds some plan when one exists", and X found one.
> Optimal: no. An optimal planner must return the cheapest plan, the 9-step route.
> Replanning means: make a plan, execute a part of it, then plan again from the new situation. It helps when a full plan is too expensive to compute, or when the model can be wrong and the world may not follow the plan.

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

> [!info] The textbook names for the same parts (beyond the slides)
> Russell and Norvig list six items: **states**, **initial state**, **actions** (what can be done in state $s$), **transition model** (the state that results from doing action $a$ in $s$), **goal test**, **path cost**.
> The deck's **successor function** packs three of them into one: for a state it returns triples (action, next state, step cost). On slide 12 these are the arrows labelled "N", 1.0 and "E", 1.0.
> The **path cost** of a plan is the sum of its step costs. It is written $g$.

> [!question]- Check (Ch.3, slide 12): A robot is in a corridor of 5 cells, numbered 1 to 5. It starts in cell 2 and must reach cell 5. Right costs 2, Left costs 1. Write the four parts of the search problem and the cheapest solution.
> State space: the robot's cell, $\{1,2,3,4,5\}$, so 5 states.
> Successor function: from cell $i$, Right gives cell $i+1$ with cost 2 (if $i<5$). Left gives cell $i-1$ with cost 1 (if $i>1$).
> Start state: cell 2. Goal test: "cell == 5".
> Cheapest solution: [Right, Right, Right], path cost $2+2+2=6$. Every Left must be undone by one more Right, so any plan with a Left costs at least $6+1+2=9$.

> [!question]- Check (Ch.3, slide 13): The picture shows a robot that builds a blocky copy of the Earth next to the real Earth. What does it say about a search problem? Name a detail that the Romania problem leaves out, and say when this is harmful.
> A search problem is a simplified model of the world, not the world itself.
> Romania keeps only the cities and the road lengths. It leaves out traffic, closed roads, fuel, speed limits and everything inside a city.
> Leaving details out is what makes the problem small enough to solve (20 cities). It is harmful when the missing detail changes which plan is possible or best, for example a closed road on the planned route.

> [!question]- Check (Ch.3, slide 14): Read from the map. What does the successor function return for Sibiu? Is Arad, Zerind, Oradea, Sibiu, Fagaras, Bucharest a solution, and what does it cost?
> Sibiu has four roads: to Arad (140), Oradea (151), Fagaras (99) and Rimnicu Vilcea (80). The successor function returns these four (action, city, cost) triples.
> The route is a solution, because it starts in Arad, follows roads only and ends in a state that passes the goal test.
> Cost: $75+71+151+99+211=607$ km. A solution does not have to be the cheapest one. The cheapest costs 418 km.

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

> [!question]- Check (Ch.3, slide 15): New problem: Pac-Man must touch all four corners of the maze. What must the search state hold? How many states are there with 120 positions?
> The state needs the position $(x,y)$ and four booleans, one per corner ("already touched or not").
> The position alone is not enough: the goal test "all four corners touched" cannot be answered from $(x,y)$.
> Size: $120\cdot2^{4}=120\cdot16=1920$ states. Dots and ghosts are not needed, so they stay out.

> [!question]- Check (Ch.3, slide 16): A student writes the eat-all-dots size as $120\cdot30$. What is wrong? A third ghost is added: which of the three counts on the slide change, and by what factor?
> Each of the 30 dots is eaten or not eaten, independently of the others. That gives $2^{30}=1\,073\,741\,824$ dot patterns, not 30. The number of dots left is not enough, because the successor function must know which dots are left.
> A third ghost has 12 positions, so the world-state count is multiplied by 12: $120\cdot2^{30}\cdot12^{3}\cdot4\approx8.9\times10^{14}$.
> Pathing (120) and eat-all-dots ($120\cdot2^{30}$) do not change, because ghosts are not part of these search states.

> [!question]- Check (Ch.3, slide 17): In "Safe Passage", why must the state hold the remaining scared time? Why does the slide's answer have no ghost positions?
> The state must hold everything that changes what can happen next. Take two situations with the same position, dots and pellets, but with 1 step and with 20 steps of scared time left. With 20 steps Pac-Man can still walk to a far pellet. With 1 step he cannot. The valid plans differ, so these must be two different states.
> The power pellet booleans say which pellets can still reset the timer. The dot booleans are needed for the goal test.
> Ghost positions are left out because the plan keeps the ghosts scared all the time, so they never block or hurt Pac-Man.

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

> [!question]- Check (Ch.3, slide 19): Pathing in a $3\times3$ grid without walls, actions N, S, E, W. How many nodes and how many arcs does the state space graph have?
> Nodes: one per position, so 9. Each state occurs only once.
> Arcs: there are 6 horizontal and 6 vertical pairs of neighbour cells, 12 pairs. Each pair gives two arcs (one per direction), so $12\cdot2=24$ arcs. On the slide these are the double arrows between neighbour pictures.

> [!question]- Check (Ch.3, slide 20): The arcs of the tiny graph are S→d, S→e, S→p, d→b, d→c, d→e, b→a, c→a, e→h, e→r, h→p, h→q, p→q, r→f, f→c, f→G. What are the successors of e? Which states have no successor? Does the graph have a cycle?
> Successors of e: h and r.
> No successor: a, q and G. No arc starts at them.
> No cycle. Write the states in the order S, d, b, e, h, p, q, r, f, c, a, G. Every arc goes from an earlier state to a later one, so no path can come back. This is why the search tree of this graph is finite.

> [!question]- Check (Ch.3, slide 21): In the pathing problem, the plan "N then E" and the plan "E then N" end on the same square. Are they one node or two nodes of the search tree? What is the cost of each?
> Two nodes. A tree node stands for a plan (a path from the root), and these are two different plans.
> Both nodes show the same state. In the state space graph this state exists only once.
> Each plan has two steps of cost 1.0, so both nodes have path cost 2.0.

> [!question]- Check (Ch.3, slide 22): The tiny graph has 12 states, but the search tree on the slide has 28 nodes. Why? Which tree nodes hold state a, and which hold G?
> The tree has one node for every path that starts in S. A state that can be reached in several ways appears several times.
> State a appears 4 times: S-d-b-a, S-d-c-a, S-d-e-r-f-c-a and S-e-r-f-c-a.
> G appears 2 times: S-d-e-r-f-G and S-e-r-f-G. These are the two solutions.

> [!question]- Check (Ch.3, slides 23-24): The 4-state graph has the arcs S→a, S→b, a→b, a→G, b→a, b→G. The start is S, the goal is G. How many tree nodes are at depth 0, 1, 2 and 3? How many nodes down to depth 5, and how many of them are solutions?
> Depth 0: S (1 node). Depth 1: a, b (2 nodes). Depth 2: under a come b and G, under b come a and G (4 nodes). Depth 3: again 4 nodes, because the two G nodes have no children and the a and b nodes have 2 children each.
> Every deeper level also has 4 nodes: one a, one b and two G.
> Down to depth 5: $1+2+4\cdot4=19$ nodes. Solutions: 2 at each of the depths 2, 3, 4, 5, so 8.
> The levels never stop, so the full tree is infinite.

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

> [!info] How to read the list on slide 30 (beyond the slides)
> Each line of the list on the right is one tree node, written as its path (s-d-e means the path S, d, e). The lines appear in the order in which the nodes are generated.
> A line that is crossed out has been removed from the fringe and expanded. The last line, the goal node, is also crossed out: it was removed and returned, not expanded. The lines that are not crossed out are the fringe.
> The red path in the graph and in the tree is the returned solution.

> [!question]- Check (Ch.3, slides 26-27): On slide 27 Arad and Sibiu are expanded (grey). Which nodes are on the fringe? Why is there a node "Arad" below Sibiu?
> The fringe holds the generated nodes that are not expanded yet: Timisoara, Zerind, and the four children of Sibiu: Arad, Fagaras, Oradea, Rimnicu Vilcea. That is 6 nodes.
> The dashed green nodes are not generated yet.
> The road Sibiu to Arad exists, and tree search does not remember visited states. The lower Arad node is the plan Arad, Sibiu, Arad with cost $140+140=280$. It is a different node from the root, with the same state.

> [!question]- Check (Ch.3, slide 28): Which single line of TREE-SEARCH is different for DFS, BFS and UCS? What does the first `if` do, and at which moment is a node tested for the goal?
> Only the line "choose a leaf node for expansion according to strategy" differs. Everything else is the same loop.
> The first `if` returns failure when the fringe is empty. Then every plan was tried and none reached a goal.
> The goal test is applied to the node that was chosen (removed from the fringe), not at the moment a node is generated.

> [!question]- Check (Ch.3, slides 29-30): On slide 30 the search stops with s-d-e-r-f-G. How many nodes were generated, how many were expanded, and which nodes are still on the fringe?
> Generated: 12, one per line of the list.
> Expanded (crossed out): s, s-d, s-d-e, s-d-e-r, s-d-e-r-f. That is 5 nodes. Their child counts are $3+3+2+1+2=11$, plus the root gives 12.
> s-d-e-r-f-G is removed, passes the goal test and is returned.
> Still on the fringe: s-e, s-p, s-d-b, s-d-c, s-d-e-h, s-d-e-r-f-c. That is 6 nodes ($12-5-1=6$).

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

> [!info] Where $O(b^{m})$ and $O(bm)$ come from (beyond the slides)
> Tree size: the tiers have $1, b, b^{2},\dots,b^{m}$ nodes. The sum is a geometric series:
> $$1+b+b^{2}+\dots+b^{m}=\frac{b^{m+1}-1}{b-1}.$$
> For $b\ge2$ this is less than $2\,b^{m}$, so it is $O(b^{m})$. The last tier alone holds more than half of the nodes.
> DFS fringe: when DFS expands a node, it pushes $b$ children and continues with one of them. The other $b-1$ wait on the stack. At depth $m$ the stack holds $b-1$ waiting siblings for each of the $m$ levels, plus the current node:
> $$(b-1)\,m+1=O(bm).$$

> [!question]- Check (Ch.3, slide 32): Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Trace DFS tree search. Rule: the alphabetically first child is expanded first, goal test on removal. Give the order of removal, the fringe after each step, the path and its cost.
> 1. Remove S. Fringe (top of the stack first): S-A, S-B.
> 2. Remove S-A. Fringe: S-A-C, S-A-D, S-B.
> 3. Remove S-A-C. Fringe: S-A-C-G, S-A-D, S-B.
> 4. Remove S-A-C-G. It is a goal, stop.
> 
> Path S-A-C-G, cost $1+2+7=10$, after 3 expansions.
> It is not the cheapest path (S-A-D-G costs 6) and not the shortest (S-B-G has 2 actions). DFS returns the leftmost solution.

> [!question]- Check (Ch.3, slide 34): A tree has $b=3$ and $m=4$. How many nodes does it have? What share of them is in the bottom tier?
> Tiers: $1+3+9+27+81=121$ nodes.
> With the formula: $\frac{3^{5}-1}{3-1}=\frac{242}{2}=121$.
> Bottom tier: $81/121\approx0.67$, so about 67% of all nodes. This is why the bottom tier dominates and the total is $O(b^{m})$.

> [!question]- Check (Ch.3, slide 35, space): For $b=3$ and $m=4$, what is the largest number of nodes on the DFS fringe? Compare with the size of the bottom tier.
> At each of the 4 levels on the current path, $b-1=2$ siblings wait. Add the current node:
> $(b-1)\,m+1=2\cdot4+1=9$ nodes.
> The bottom tier has $3^{4}=81$ nodes. DFS memory grows like $b\cdot m$, not like $b^{m}$.

> [!question]- Check (Ch.3, slide 35, completeness): Take the 4-state graph of slide 23: S→a, S→b, a→b, a→G, b→a, b→G. Start S, goal G. Run DFS tree search with the alphabetically first child first (a before b before G), goal test on removal. What happens?
> Removal order: S, S-a, S-a-b, S-a-b-a, S-a-b-a-b, and so on.
> Below a the child b comes before G, and below b the child a comes before G. DFS always goes deeper along a, b, a, b and never removes a G node.
> A solution of 2 actions exists (S-a-G), but DFS does not find it. The node S-a-G is generated when S-a is expanded, but with the goal test on removal it is never tested, because it is never removed. The tree has infinite depth ($m=\infty$), so DFS is not complete.

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

> [!question]- Check (Ch.3, slide 37): Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Trace BFS tree search. Rule: children enter the queue in alphabetical order, goal test on removal. Give the order of removal, the fringe after each step, the path and its cost.
> 1. Remove S. Fringe (front first): S-A, S-B.
> 2. Remove S-A. Fringe: S-B, S-A-C, S-A-D.
> 3. Remove S-B. Fringe: S-A-C, S-A-D, S-B-G.
> 4. Remove S-A-C. Fringe: S-A-D, S-B-G, S-A-C-G.
> 5. Remove S-A-D. Fringe: S-B-G, S-A-C-G, S-A-D-G.
> 6. Remove S-B-G. It is a goal, stop.
> 
> Path S-B-G, 2 actions, cost $5+6=11$, after 5 expansions.
> It is the shallowest solution, but not the cheapest: S-A-D-G costs $1+4+1=6$ with 3 actions.

> [!question]- Check (Ch.3, slide 38): Let $b=10$ and let the shallowest solution be at depth $s=6$. How many nodes are in the tiers 0 to 6? How much memory does the last tier need at 1 kB per node? What happens if $s$ grows to 7?
> Tiers 0 to 6: $1+10+10^{2}+\dots+10^{6}=1\,111\,111$ nodes.
> Last tier: $10^{6}$ nodes, so $10^{6}$ kB $=1$ GB.
> With $s=7$ everything is multiplied by about $b=10$: $11\,111\,111$ nodes and 10 GB. Time and space of BFS both grow like $b^{s}$.

> [!question]- Check (Ch.3, slide 40): A binary tree ($b=2$) has maximum depth $m=20$. Case 1: all solutions are at depth 20, and there are many of them. Case 2: the only solution is at depth 2, below the right child of the root. Which of DFS and BFS is better in each case?
> Case 1: DFS. It dives to depth 20 with about 20 expansions and keeps about $(b-1)m+1=21$ nodes in memory. BFS must first process all tiers above depth 20: $2^{20}-1=1\,048\,575$ nodes, and it holds about a whole tier in memory.
> Case 2: BFS. It processes at most the tiers 0, 1, 2: $1+2+4=7$ nodes. DFS goes left first and searches the whole left subtree, $2^{20}-1=1\,048\,575$ nodes, before it tries the right child.
> BFS is better for shallow solutions. DFS is better for deep and plentiful solutions and for small memory.

## 8. Iterative deepening (slide 43)
![[cmpe421_lecture_03_ch_3_p043.jpg]]

**Notes.** Run DFS with depth limit 1, then 2, then 3, until a solution appears. This gives DFS's memory, $O(bs)$, with BFS's completeness and shallowest answer. The repeated work is small because most nodes are in the lowest tier searched. Details and the cost formula are in section 14.

> [!question]- Check (Ch.3, slide 43): Practice graph, directed: S→A, S→B, A→C, A→D, B→G, C→G, D→G. Start S, goal G. Run iterative deepening as on the slide: tree search, DFS with depth limit 1, then 2. Children in alphabetical order. A node at the limit is tested but not expanded. List the nodes visited in each round and the path found.
> Limit 1: S, S-A, S-B. No goal. The limit cut off A and B, so continue.
> Limit 2: S, S-A, S-A-C, S-A-D, S-B, S-B-G. S-B-G is a goal, stop.
> Path S-B-G, 2 actions. This is the answer of BFS, not of plain DFS (S-A-C-G).
> Total visits: $3+6=9$. The first round is repeated work, but it is the small part.
> Memory: only the current path and the waiting siblings, as in DFS.

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

> [!warning] Slide error
> Slide 44 labels the arc r→f with cost 2. Slide 46 labels the same arc with 1, and the numbers in its tree (r 7, f 8, G 10) use 1. This note uses 1 everywhere.
> With cost 2 the paths would cost 11 (S-d-e-r-f-G) and 15 (S-e-r-f-G). The order of the two paths stays the same.

> [!info] Why the effective depth is $C^{*}/\varepsilon$ (beyond the slides)
> Every arc costs at least $\varepsilon$. A path with $k$ arcs costs at least $k\,\varepsilon$.
> UCS removes nodes in order of $g$, so before it returns the best solution it only expands nodes with $g\le C^{*}$. For such a node $k\,\varepsilon\le C^{*}$, so $k\le C^{*}/\varepsilon$.
> All expanded nodes are in the first $C^{*}/\varepsilon$ tiers of the tree. These tiers hold $O(b^{C^{*}/\varepsilon})$ nodes. This gives the time bound. The fringe is about the last of these tiers, which gives the same space bound.
> If $\varepsilon=0$ is allowed, a path can have infinitely many arcs with a finite cost, and the bound is lost.
> The textbook counts one tier more, because the children of the last expanded tier are also generated: $O(b^{1+\lfloor C^{*}/\varepsilon\rfloor})$. The handout writes $O(b^{\lceil C^{*}/\varepsilon\rceil})$. All three mean "about $C^{*}/\varepsilon$ tiers".

> [!question]- Check (Ch.3, slide 44): Use the costs S→d 3, d→e 2, S→e 9, e→r 2, r→f 1, f→G 2. BFS returns S-e-r-f-G. For which cost $x$ of the arc S→e would this BFS path also be a least-cost path?
> S-e-r-f-G costs $x+2+1+2=x+5$.
> S-d-e-r-f-G costs $3+2+2+1+2=10$.
> The BFS path is a least-cost path when $x+5\le10$, so when $x\le5$. With $x=9$ it costs 14 and is not optimal.
> BFS counts actions only. Its path is optimal only when the direct arc is not more expensive than the detour S-d-e (cost 5).

> [!question]- Check (Ch.3, slide 46): Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Trace UCS tree search. Rule: smallest $g$ first, equal $g$ first-in first-out, goal test on removal. Give the order of removal, the fringe with $g$ after each step, the path and its cost.
> 1. Remove S (0). Fringe: S-A (1), S-B (5).
> 2. Remove S-A (1). Fringe: S-A-C (3), S-B (5), S-A-D (5).
> 3. Remove S-A-C (3). Fringe: S-B (5), S-A-D (5), S-A-C-G (10).
> 4. Remove S-B (5), it entered before S-A-D. Fringe: S-A-D (5), S-A-C-G (10), S-B-G (11).
> 5. Remove S-A-D (5). Fringe: S-A-D-G (6), S-A-C-G (10), S-B-G (11).
> 6. Remove S-A-D-G (6). It is a goal, stop.
> 
> Path S-A-D-G, cost $1+4+1=6$, after 5 expansions.
> Two goal nodes (cost 10 and 11) were on the fringe earlier. UCS did not return them, because it tests a node only when it is removed. This is why the cheapest one wins.

> [!question]- Check (Ch.3, slide 47): Let $b=3$, $C^{*}=20$ and $\varepsilon=2$. What is the effective depth and the bound on the number of nodes? What changes if one arc gets cost 0.5? What goes wrong with an arc of cost 0?
> Effective depth: $C^{*}/\varepsilon=20/2=10$. Bound: of the order of $3^{10}=59\,049$ nodes (the $O$ hides a constant factor).
> With an arc of cost 0.5: $\varepsilon=0.5$, effective depth $20/0.5=40$, bound of the order of $3^{40}\approx1.2\times10^{19}$. One cheap arc makes the worst case much larger.
> With an arc of cost 0, for example a loop, UCS can follow it forever. The cost never grows, so these nodes always come out before the solution. UCS is then not complete. This is the reason for the condition $\varepsilon>0$.

> [!question]- Check (Ch.3, slide 48): Open grid, moves N, S, E, W, each of cost 1. The goal is 10 steps east of the start. About how many different states have cost less than 10 from the start? What does this show?
> States with cost at most $k$ form a diamond around the start with $2k(k+1)+1$ cells.
> For $k=9$: $2\cdot9\cdot10+1=181$ states.
> UCS expands all of them before it reaches the cost-10 contour with the goal. The straight plan needs only 10 steps. UCS spreads in every direction (the circles on the slide), because it has no information about where the goal is.

## 10. The one queue (slide 53)
![[cmpe421_lecture_03_ch_3_p053.jpg]]

**Notes.** All these algorithms are the same loop. Conceptually every fringe is a priority queue. For DFS and BFS a stack and a queue avoid the $\log n$ overhead.

Tie-breaking in our traces: successors in alphabetical order, DFS explores the alphabetically first child first, equal $g$ comes out first-in first-out.

> [!question]- Check (Ch.3, slide 53): You have only a priority queue that removes the smallest priority first. Which priority makes it behave like UCS, like BFS and like DFS? Why do real programs still use a stack and a queue?
> UCS: priority = path cost $g$.
> BFS: priority = depth of the node. Shallow nodes come out first.
> DFS: priority = minus the depth (or minus a counter of insertion time). The deepest, newest node comes out first.
> A priority queue with $n$ nodes needs about $\log n$ work for each insert and remove. A stack and a queue need constant time, so for DFS and BFS they are cheaper.

## 11. Search and models (slides 54-55)
![[cmpe421_lecture_03_ch_3_p054.jpg]]
![[cmpe421_lecture_03_ch_3_p055.jpg]]

**Notes.** Search operates over **models** of the world. The agent does not try the plans in the real world. Planning is all in simulation, so the search is only as good as the model. An optimal plan for a wrong model can fail.

> [!question]- Check (Ch.3, slide 54): A route planner runs UCS on a map in which one bridge is still open. In reality the bridge is closed. Is the returned plan optimal? What can the agent do?
> The plan is optimal for the model, because UCS is optimal on the graph it is given. It is not guaranteed to work in the world: if it uses the bridge, it fails.
> The search algorithm made no mistake. The model was wrong.
> The agent can correct the model (remove the arc) and plan again from where it is now. This is replanning.

> [!question]- Check (Ch.3, slide 55): The right map on the slide shows a route from Haugesund to Trondheim, both in Norway, with 2713.2 km and 47 hours 31 minutes. The line runs far south through other countries. Is this more likely an error of the search algorithm or of the model?
> Of the model. The algorithm found the best route in the graph it had.
> A possible reason (this is an interpretation, the slide only shows the picture): the map data missed some road or ferry links inside Norway. Then the cheapest path in the model is a very long detour.
> "Your search is only as good as your models."

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

> [!info] Handout numbering
> The handout numbers in this note are the page numbers of the PDF file. The number printed in the footer of each handout slide is one higher (PDF page 2 shows "3" in the footer).
> The state numbers 1 to 8 of the vacuum world are on handout slides 3 and 4. Odd numbers: the agent is in room A. Even numbers: in room B. States 1, 2: both rooms dirty. 3, 4: only A dirty. 5, 6: only B dirty. 7, 8: both clean.

> [!question]- Check (handout, slide 2): The agent knows that it starts in room B and that both rooms are dirty (state 2). Give a solution. Why is a fixed sequence enough here, without looking at any percept during execution?
> Solution: [Suck, Left, Suck]. State 2, Suck gives 4 (B clean), Left gives 3 (agent in A), Suck gives 7 (all clean).
> The start state is known and every action has exactly one result. The agent can compute all future states before it acts, so the percepts tell it nothing new.

> [!question]- Check (handout, slide 3): No sensors: the start state can be any of 1 to 8. Follow the set of possible states through [Right, Suck, Left, Suck]. Is there a conformant plan with only 3 actions?
> Start: $\{1,2,3,4,5,6,7,8\}$.
> After Right the agent is surely in B: $\{2,4,6,8\}$.
> After Suck room B is surely clean: $\{4,8\}$.
> After Left the agent is surely in A: $\{3,7\}$.
> After Suck room A is surely clean: $\{7\}$. State 7 is a goal, so the plan works from every start state.
> No plan with 3 actions works. Both rooms can be dirty, so the plan needs a Suck that is surely in A and a Suck that is surely in B. The agent is surely in A only after a Left, and surely in B only after a Right. That is at least 2 Sucks, 1 Left and 1 Right: 4 actions.

> [!question]- Check (handout, slide 4): The start state is one of 5, 6, 7, 8 and the agent senses its room and the dirt in it. Follow the plan [Right, if dirt then Suck]. Would the fixed sequence [Right, Suck] also reach the goal?
> In states 5 to 8 room A is clean. After Right: 5 and 6 become 6, 7 and 8 become 8. Possible states: $\{6,8\}$.
> Now the percept decides. Dirt seen: the state is 6, Suck gives 8. No dirt: the state is 8, nothing to do. Both branches end in state 8, a goal.
> [Right, Suck] also ends in 8 here, because Suck in a clean room changes nothing. The contingent plan only saves a useless action.
> A conditional plan becomes necessary when the outcome of an action is uncertain, for example if Suck could sometimes drop dirt on a clean floor (beyond the slides).

> [!question]- Check (handout, slide 5): In the exploration problem the agent does not know which rooms exist. Why can it not run BFS or UCS before acting? How is this different from the no-sensor problem of handout slide 3?
> Offline search needs the successor function: for each state, the actions and their results. Here the agent does not have it, so it cannot build any search tree in its head.
> It must act, observe the result and learn the state space step by step. This is online problem solving.
> In the no-sensor problem the agent knows the whole model and only does not know which state it is in. In the exploration problem the model itself is unknown.

> [!question]- Check (handout, slide 10): The vacuum world on the slide has 2 rooms and 8 states. How many states and how many goal states are there with 3 rooms in a row? The slide gives NoOp the cost 0: why is that dangerous for uniform-cost search?
> States: 3 agent positions times $2^{3}$ dirt patterns, $3\cdot8=24$. (Check with 2 rooms: $2\cdot2^{2}=8$.)
> Goal states: all rooms clean, agent anywhere, so 3.
> NoOp keeps the state and costs 0. The smallest step cost is then $\varepsilon=0$. UCS tree search can remove the NoOp child (still cost 0), then its NoOp child, and so on forever. It never reaches the nodes of cost 1. Completeness of UCS needs $\varepsilon>0$.

## 13. Handout: nodes, eager and cautious goal test (handout pages 17, 19)
![[cmpe421_lecture_03_uninformed_search_p017.jpg]]
![[cmpe421_lecture_03_uninformed_search_p019.jpg]]

**Notes.** A **state** is a configuration of the world. A **node** is a data structure in the search tree: it has a state, a parent, an action, a depth and a path cost $g$. A state has none of these.

- **Eager** tree search tests a node when it is **generated**. It generates fewer nodes.
- **Cautious** tree search tests a node when it is **removed** from the frontier. This is necessary for an optimal solution with UCS.

BFS should be eager. UCS must be cautious: an eager UCS would stop when it first generates Bucharest via Fagaras (450) and return the worse route.

> [!question]- Check (handout, slide 17): Two nodes hold the same 8-puzzle state, one with depth 6 and one with depth 8. Is that possible? The node on the slide has depth 6 and $g=6$: what does this say about the step costs? How do you get the solution from a goal node?
> Yes. The same board can be reached by different move sequences. Each sequence is its own node. Depth and path cost belong to the node, not to the state.
> Depth 6 with $g=6$ fits a cost of 1 per move, as in the 8-puzzle.
> Solution: follow the parent pointers from the goal node up to the root, collect the actions, and reverse the list.

> [!question]- Check (handout, slide 19): BFS on a tree with $b=3$. The only goal is the last node of depth 2. How many nodes does eager BFS generate, and how many does cautious BFS generate?
> Eager: the goal is tested when it is generated. Generated: $1+3+9=13$ nodes.
> Cautious: the goal is tested when it is removed. It is the last node of depth 2, so the other $9-1=8$ nodes of depth 2 are removed and expanded first. They generate $8\cdot3=24$ nodes of depth 3.
> Cautious total: $13+24=37$ nodes.
> In general cautious BFS can generate about one extra tier, $b(b^{d}-1)$ nodes.

## 14. Handout: depth-limited search and IDS (handout pages 57, 58, 67)
![[cmpe421_lecture_03_uninformed_search_p057.jpg]]
![[cmpe421_lecture_03_uninformed_search_p058.jpg]]
![[cmpe421_lecture_03_uninformed_search_p067.jpg]]

**Notes.** **Depth-limited search** is DFS that backtracks at depth = limit $l$. It returns a solution, **Failure** (no solution at all) or **Cutoff** (the limit cut something off). **Iterative deepening (IDS)** runs it with $l=0,1,2,\dots$.

With the solution at depth $d$, a node at depth $i$ is generated in $d-i+1$ rounds:

$$(d+1)\,b^{0}+d\,b^{1}+(d-1)\,b^{2}+\dots+1\cdot b^{d}=O(b^{d}).$$

IDS: complete, time $O(b^{d})$, space $O(bd)$, optimal if every step costs 1. For $b=10$, $d=5$ IDS generates $123\,456$ nodes against $111\,111$ for eager BFS, only about 11% more.

The deck starts with limit 1, the handout with limit 0. The answer is the same.

> [!question]- Check (handout, slide 57): A tree without any goal: R has the children X and Y, X has the child Z, and Y and Z have no children. What does LIMITED-DEPTH-SEARCH return for limit 1, 2 and 3? Why does iterative deepening need this difference?
> In the code, a node that is reached with limit 0 returns Cutoff, even if it has no children. A node with no children that is reached with limit above 0 returns Failure.
> Limit 1: X and Y are reached with limit 0. Both return Cutoff. Result: Cutoff.
> Limit 2: Y is reached with limit 1 and has no children: Failure. Z is reached with limit 0: Cutoff, so X returns Cutoff. Result: Cutoff.
> Limit 3: Z is reached with limit 1 and has no children: Failure. X and Y return Failure. Result: Failure.
> Cutoff means "something may be below the limit, try a larger limit". Failure means "the whole tree was searched, there is no solution". Iterative deepening stops on Failure. Without it, it would raise the limit forever.

> [!question]- Check (handout, slide 58): Iterative deepening with limits 0, 1, 2, ... on a tree with $b=3$ and the solution at depth $d=4$. How many nodes are generated in total? Compare with one search of the tiers 0 to 4.
> A node of depth $i$ is generated in every round with limit $i, i+1,\dots,d$. That is $d-i+1$ rounds.
> Total: $5\cdot1+4\cdot3+3\cdot9+2\cdot27+1\cdot81=5+12+27+54+81=179$.
> One search of all tiers: $1+3+9+27+81=121$.
> Ratio: $179/121\approx1.48$. The repeated rounds cost about 48% more, not several times more.

> [!question]- Check (handout, slide 67): For $b=2$ and $d=10$, compute the IDS node count and compare it with $1+b+\dots+b^{d}$. How many nodes does IDS keep in memory, compared with the last tier? Why is the overhead larger here than for $b=10$?
> IDS: $\sum_{i=0}^{10}(11-i)\,2^{i}=4083$. All tiers once: $2^{11}-1=2047$. Ratio $\approx1.99$.
> Memory: about $b\cdot d=20$ nodes for IDS. The last tier has $2^{10}=1024$ nodes.
> For large $d$ the ratio is close to $b/(b-1)$ (beyond the slides). This is 2 for $b=2$ and 1.11 for $b=10$. With a small $b$ the upper tiers are not small compared with the last tier, so repeating them costs more. It is still only a constant factor, so the time stays $O(b^{d})$.

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

> [!question]- Check (handout, slide 30): The slide says: 12 hours at 200 MB per second need 8.64 TB. Check this number, then do the same for 1 hour at 100 MB per second. Why is BFS optimal when every step costs the same constant $k$?
> 12 hours $=12\cdot3600=43\,200$ s. $43\,200\cdot200$ MB $=8\,640\,000$ MB $=8.64$ TB. The slide is right.
> 1 hour at 100 MB/s: $3600\cdot100$ MB $=360\,000$ MB $=360$ GB.
> BFS keeps every node in memory, so memory runs out long before time does.
> With cost $k$ per step, a path with $n$ actions costs $n\,k$. The path with the fewest actions is then also the cheapest, and BFS returns the path with the fewest actions.

> [!question]- Check (handout, slide 68): A problem has $b=10$, the shallowest solution at depth $d=6$, every step cost 1, an infinite state space, and memory for 10 000 nodes. Go through the columns of the table and choose an algorithm.
> BFS: complete and optimal, but the space is $b^{d}=10^{6}$ nodes. Too much.
> UCS: with equal costs it behaves like BFS. Same memory problem.
> DFS: space $bm$ is small, but $m=\infty$, so it may never return. Not complete, not optimal.
> Depth-limited: complete only if $l\ge d$, and $d$ is not known in advance. Not optimal.
> IDS: complete, optimal for equal step costs, space $b\,d=60$ nodes. Time: $7+60+500+4000+30\,000+200\,000+1\,000\,000=1\,234\,567$ generated nodes, against $1\,111\,111$ for the tiers 0 to 6 once.
> Choose iterative deepening.

> [!question]- Check (handout, slide 69): In the graph on the slide every state has two arcs to the next state (A to B, B to C, and so on). For a chain with states down to depth 20, how many states are there, and how many nodes does the search tree have at depth 20 and in total?
> States: one per level, so 21.
> Tree: every node has 2 children, so depth $k$ has $2^{k}$ nodes. Depth 20: $2^{20}=1\,048\,576$ nodes, all with the same state.
> Total: $2^{21}-1=2\,097\,151$ nodes.
> The number of states is linear in the depth, the tree is exponential. A search that detects repeated states expands each of the 21 states once.

> [!question]- Check (handout, slide 70): Take the 4-state graph S→a, S→b, a→b, a→G, b→a, b→G. Start S, goal G. Run GRAPH-SEARCH as on the slide (goal test on removal, then the explored test) with the depth-first rule: new nodes go to the front of the queue, in the order a before b before G. Give each step with the explored set and the frontier. Compare with the same search without the explored set.
> 1. Remove S. Explored: {S}. Frontier: S-a, S-b.
> 2. Remove S-a. Explored: {S, a}. Frontier: S-a-b, S-a-G, S-b.
> 3. Remove S-a-b. Explored: {S, a, b}. Frontier: S-a-b-a, S-a-b-G, S-a-G, S-b.
> 4. Remove S-a-b-a. State a is already explored, so the node is not expanded. Frontier: S-a-b-G, S-a-G, S-b.
> 5. Remove S-a-b-G. It is a goal, stop.
> 
> Result: S-a-b-G, 3 actions.
> Without the explored set, step 4 expands S-a-b-a and the search runs a, b, a, b forever. The explored set makes depth-first search complete on this finite graph.
> It does not make it optimal: S-a-G has only 2 actions.

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
