---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L03 - Uninformed search
type: quiz
questions: 45
---

# CMPE421 L03 - Uninformed search - quiz

Lesson note: [[CMPE421 L03 - Uninformed search]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Reflex agents and planning agents (slides 2-10)

### Q1 (Ch.3, slides 4-5)

The slide asks "Can a reflex agent be rational?". The robot in the picture uses the rule "apple ahead, move forward". Which answer is right?

![[cmpe421_lecture_03_ch_3_p005.jpg]]

- A) No, never. A reflex agent has no model, and an agent without a model can never be rational.
- B) Yes, always. The rule maps every percept to an action, and that is all a rational agent needs.
- C) Yes, in some environments. Here it fails: the rule ignores the gap, because the agent never asks what happens after the move.
- D) No, never. A reflex agent cannot have memory, so it cannot remember where the gap was.

> [!success]- Answer
> **C.** A reflex agent is rational when the best action depends only on the current percept, for example "if this room is dirty, suck". Here the right action depends on what happens next (the gap), and the agent does not look ahead. So it falls.
> - A: A reflex agent can be rational where no look-ahead is needed. Also, a model-based reflex agent does have a model of the current state.
> - B: A mapping from percepts to actions is not enough. The picture shows a case where the mapping leads to a fall.
> - D: Slide 5 says a reflex agent "may have memory". The problem is not memory, it is that it does not consider future consequences.

### Q2 (Ch.3, slide 8)

A maze has three routes to the exit, with 9, 12 and 15 steps. Planner X returns the 15-step route. Which statement is right?

- A) X cannot be complete and cannot be optimal.
- B) X can be complete, but it cannot be optimal.
- C) X cannot be complete, but it can be optimal.
- D) X can be complete and can be optimal.

> [!success]- Answer
> **B.** Complete only means "finds some plan when one exists". X found one, so it can be complete. An optimal planner must return the cheapest plan, the 9-step route, so X is not optimal.
> - A: Returning a long route does not break completeness. Completeness asks only whether a plan is found.
> - C: This swaps the two words. Optimal is the property that fails here.
> - D: Optimal means the cheapest plan. 15 steps is not the cheapest, because 9 steps exist.

## 2. Search problems (slides 12-14)

### Q3 (Ch.3, slide 12)

A robot is in a corridor of 5 cells, numbered 1 to 5. It starts in cell 2 and must reach cell 5. Right costs 2, Left costs 1. How many states does the state space have, and what is the cost of the cheapest solution?

- A) 4 states, cost 6.
- B) 5 states, cost 6.
- C) 5 states, cost 3.
- D) 4 states, cost 3.

> [!success]- Answer
> **B.** The state is the robot's cell, so the state space is $\{1,2,3,4,5\}$: 5 states. The cheapest plan is [Right, Right, Right], cost $2+2+2=6$. Every Left must be undone by one more Right, so a plan with a Left costs at least $6+1+2=9$.
> - A: Cell 1 is never needed by the best plan, but it is still a state. The robot can reach it with Left from cell 2.
> - C: 3 is the number of actions, not the path cost. Each Right costs 2.
> - D: Two slips: cell 1 left out of the state space, and the number of actions used as the cost.

### Q4 (Ch.3, slide 13)

The picture shows a robot that builds a blocky copy of the Earth next to the real Earth. The Romania problem keeps only the cities and the road lengths. When is leaving out the other details harmful?

- A) Always. A search problem should contain every detail of the world, or its solution is wrong.
- B) Never. Uniform-cost search is optimal, so the plan it returns is always the best one in the world.
- C) When a missing detail changes which plan is possible or best, for example a closed road on the planned route.
- D) Only when the map has more than about 20 cities, because then the search becomes too slow.

> [!success]- Answer
> **C.** A search problem is a simplified model of the world. Leaving details out is what makes it small enough to solve. It is harmful only when the missing detail changes which plan is possible or best.
> - A: With every detail the problem becomes too large to solve. Abstraction is the point of the model.
> - B: The search is optimal for the model, not for the world. A wrong model can make the best model plan fail.
> - D: The number of cities is about the size of the problem, not about which details are missing.

### Q5 (Ch.3, slide 14)

Use the map. The road lengths are Arad-Zerind 75, Zerind-Oradea 71, Oradea-Sibiu 151, Sibiu-Fagaras 99, Fagaras-Bucharest 211. Is the route Arad, Zerind, Oradea, Sibiu, Fagaras, Bucharest a solution, and what does it cost?

![[cmpe421_lecture_03_ch_3_p014.jpg]]

- A) No, because it is not the cheapest route. It costs 607 km.
- B) Yes. It costs 532 km.
- C) No, because it is not the cheapest route. It costs 532 km.
- D) Yes. It costs 607 km.

> [!success]- Answer
> **D.** It starts in Arad, follows roads only and ends in Bucharest, which passes the goal test. So it is a solution. Cost: $75+71+151+99+211=607$ km.
> A solution does not have to be the cheapest one. The cheapest costs 418 km.
> - A: "Solution" means any plan that reaches the goal. "Cheapest" is the extra word "optimal".
> - B: $532=71+151+99+211$ leaves out the first road, Arad-Zerind (75).
> - C: Two slips: a solution need not be cheapest, and the first road (75) is missing from the sum.

## 3. What is in a state space (slides 15-17)

### Q6 (Ch.3, slide 15)

New problem: Pac-Man must touch all four corners of the maze. The maze has 120 positions. The state holds the position and what else is needed. How many states are there?

- A) 120
- B) 480
- C) 1920
- D) 136

> [!success]- Answer
> **C.** The state needs the position and four booleans, one per corner ("already touched or not"). Independent parts multiply, and 4 booleans give $2^{4}=16$ patterns. So $120\cdot16=1920$.
> - A: The position alone cannot answer the goal test "all four corners touched".
> - B: $120\cdot4$ counts the number of corners, not the number of yes/no patterns. 4 booleans give $2^{4}$, not 4.
> - D: $120+16$ adds the parts. Independent parts multiply.

### Q7 (Ch.3, slide 16)

Slide 16 has 120 positions, 30 dots, 2 ghosts with 12 positions each and 4 facings. World states: $120\cdot2^{30}\cdot12^{2}\cdot4$. Pathing: 120. Eat-all-dots: $120\cdot2^{30}$. A third ghost with 12 positions is added. What changes?

- A) All three counts are multiplied by 12.
- B) Only the world-state count changes. It is multiplied by 12.
- C) Only the world-state count changes. It is multiplied by 1.5.
- D) All three counts are multiplied by 1.5.

> [!success]- Answer
> **B.** The third ghost is one more independent part with 12 positions, so the world count becomes $120\cdot2^{30}\cdot12^{3}\cdot4\approx8.9\times10^{14}$, 12 times more. Pathing and eat-all-dots do not hold ghost positions, so they stay the same.
> - A: Ghosts are not part of the pathing or eat-all-dots search states.
> - C: $3/2=1.5$ compares the number of ghosts. Independent parts multiply, so one more ghost multiplies by its 12 positions.
> - D: Two slips: the ghost factor is 12, and the two search states do not contain ghosts.

### Q8 (Ch.3, slide 17)

In "Safe Passage" Pac-Man must eat all dots while the ghosts stay scared. Why must the search state hold the remaining scared time?

- A) Because the goal test "all dots eaten" needs to know the scared time.
- B) Because two situations with a different scared time allow different plans.
- C) Because the path cost of each step depends on how long the ghosts stay scared.
- D) Because the ghost positions depend on it, and the state must hold the ghost positions.

> [!success]- Answer
> **B.** The state must hold everything that changes what can happen next. With 20 steps of scared time Pac-Man can still reach a far pellet, with 1 step he cannot. The valid plans differ, so these are two states.
> - A: The goal test needs the dot booleans, not the timer.
> - C: Each step costs the same. The timer limits which plans are possible, not what a step costs.
> - D: The slide's state has no ghost positions. The plan keeps the ghosts scared, so they never block Pac-Man.

## 4. State space graphs and search trees (slides 19-24)

### Q9 (Ch.3, slide 19)

Pathing in a $3\times3$ grid without walls, actions N, S, E, W. The state space graph has 9 nodes. How many arcs does it have?

- A) 12
- B) 36
- C) 24
- D) 48

> [!success]- Answer
> **C.** There are 6 horizontal and 6 vertical pairs of neighbour cells, 12 pairs. Each pair gives two arcs, one per direction: $12\cdot2=24$.
> Check: 4 corners with 2 moves, 4 edge cells with 3 moves, 1 centre with 4 moves: $8+12+4=24$.
> - A: 12 counts each pair once. A move from left to right and a move from right to left are two arcs.
> - B: $9\cdot4$ assumes every cell has 4 moves. Corner and edge cells have walls on some sides.
> - D: $24\cdot2$ doubles the arcs a second time. The 24 already contain both directions.

### Q10 (Ch.3, slide 20)

The tiny graph has the arcs S→d, S→e, S→p, d→b, d→c, d→e, b→a, c→a, e→h, e→r, h→p, h→q, p→q, r→f, f→c, f→G. What are the successors of e, and does the graph have a cycle?

- A) Successors of e: h and r. No cycle.
- B) Successors of e: S and d. A cycle, for example h, p, q, h.
- C) Successors of e: S and d. No cycle.
- D) Successors of e: h and r. A cycle, for example h, p, q, h.

> [!success]- Answer
> **A.** The successors of e are the ends of the arcs that start at e: e→h and e→r. Order the states S, d, b, e, h, p, q, r, f, c, a, G. Every arc goes from an earlier state to a later one, so no path comes back: no cycle.
> - B: S and d are the states with arcs into e, its predecessors. And h, p, q is not a cycle: there is no arc q→h.
> - C: S and d have arcs into e. Successors use the arcs that leave e.
> - D: This reads the arcs as two-way. With directed arcs h→p→q has no way back to h.

### Q11 (Ch.3, slide 21)

In the pathing problem every move costs 1.0. The plan "N then E" and the plan "E then N" end on the same square. How many search-tree nodes do they give, and what path cost does each node have?

- A) One node, path cost 2.0.
- B) Two nodes, path cost 1.0 each.
- C) One node, path cost 1.0.
- D) Two nodes, path cost 2.0 each.

> [!success]- Answer
> **D.** A tree node stands for a plan, a path from the root. These are two different plans, so two nodes, with the same state. Each plan has two steps of cost 1.0, so $g=2.0$.
> - A: One state, yes. But the tree has one node per path, and the state graph is where each state occurs once.
> - B: 1.0 is the cost of the last step only. The path cost $g$ adds all steps.
> - C: Two slips: the tree has one node per plan, and $g$ is the sum of all steps.

### Q12 (Ch.3, slide 22)

The tiny graph of slide 20 has the arcs S→d, S→e, S→p, d→b, d→c, d→e, b→a, c→a, e→h, e→r, h→p, h→q, p→q, r→f, f→c, f→G. Its search tree from S has 28 nodes. How many tree nodes hold state a, and how many hold G?

- A) a: 4 nodes. G: 2 nodes.
- B) a: 2 nodes. G: 1 node.
- C) a: 4 nodes. G: 1 node.
- D) a: 2 nodes. G: 2 nodes.

> [!success]- Answer
> **A.** The tree has one node for every path from S. Paths to a: S-d-b-a, S-d-c-a, S-d-e-r-f-c-a, S-e-r-f-c-a, so 4. Paths to G: S-d-e-r-f-G, S-e-r-f-G, so 2.
> - B: 2 and 1 are the numbers of arcs that enter a and G. A tree counts paths, and several paths use the same last arc.
> - C: G is one state, but it is reached by two different paths, so it is in two tree nodes.
> - D: For a this counts the arcs into a (b→a, c→a). The arc c→a is the end of three different paths.

### Q13 (Ch.3, slides 23-24)

The 4-state graph has the arcs S→a, S→b, a→b, a→G, b→a, b→G. The start is S, the goal is G. How many search-tree nodes are there from depth 0 down to depth 5, and how many of them are solutions (hold G)?

- A) 63 nodes, 8 solutions.
- B) 19 nodes, 2 solutions.
- C) 63 nodes, 2 solutions.
- D) 19 nodes, 8 solutions.

> [!success]- Answer
> **D.** Depth 0: 1. Depth 1: 2 (a, b). Depth 2: a gives b, G and b gives a, G, so 4. The two G nodes have no children, so every deeper level again has 4 nodes: one a, one b, two G.
> Total: $1+2+4\cdot4=19$. Solutions: 2 at each depth 2, 3, 4, 5, so 8.
> - A: $1+2+4+8+16+32=63$ doubles every level. G has no outgoing arc, so a G node has no children.
> - B: 2 counts only the shallowest solutions, S-a-G and S-b-G. Deeper paths like S-a-b-G are solutions too.
> - C: Two slips: doubling every level, and counting only the depth-2 solutions.

## 5. Tree search (slides 26-30)

### Q14 (Ch.3, slides 26-27)

On slide 27 the grey nodes Arad and Sibiu have been expanded. The dashed green nodes are drawn only to show what could come later. How many nodes are on the fringe?

![[cmpe421_lecture_03_ch_3_p027.jpg]]

- A) 4
- B) 10
- C) 8
- D) 6

> [!success]- Answer
> **D.** The fringe holds the generated nodes that are not expanded yet: Timisoara, Zerind, and the four children of Sibiu (Arad, Fagaras, Oradea, Rimnicu Vilcea). That is 6. The lower Arad is the plan Arad, Sibiu, Arad, a new node with an old state.
> - A: Only the children of Sibiu. Timisoara and Zerind were generated when Arad was expanded, and they are still waiting.
> - B: $6+4$ adds the four dashed green nodes below Timisoara and Zerind. They are not generated yet.
> - C: $6+2$ adds the grey nodes. Expanded nodes have left the fringe.

### Q15 (Ch.3, slide 28)

In the TREE-SEARCH pseudocode, which line is different for DFS, BFS and UCS, and when is a node tested for the goal?

- A) The line "choose a leaf node for expansion according to strategy". The node is tested when it is chosen (removed from the fringe).
- B) The line "if the node contains a goal state". The node is tested when it is chosen (removed from the fringe).
- C) The line "choose a leaf node for expansion according to strategy". The node is tested when it is generated as a child.
- D) The line "if the node contains a goal state". The node is tested when it is generated as a child.

> [!success]- Answer
> **A.** The three algorithms are the same loop. Only the choice of the next fringe node differs: stack, queue, or priority queue on $g$. The goal test is applied to the chosen node, the one just removed from the fringe.
> - B: The goal test is the same in all three. The strategy line is the one that changes.
> - C: Testing at generation is the "eager" variant of the handout. Slide 28 tests the chosen node.
> - D: Two slips: the goal-test line is the same for all, and slide 28 does not test at generation.

### Q16 (Ch.3, slides 29-30)

On slide 30 each line of the list is one tree node, in the order it was generated. A crossed-out line was removed from the fringe. The search stops with s-d-e-r-f-G. How many nodes were generated, and how many were expanded?

![[cmpe421_lecture_03_ch_3_p030.jpg]]

- A) 12 generated, 6 expanded.
- B) 11 generated, 5 expanded.
- C) 11 generated, 6 expanded.
- D) 12 generated, 5 expanded.

> [!success]- Answer
> **D.** Generated: one per line, 12. Expanded: s, s-d, s-d-e, s-d-e-r, s-d-e-r-f, so 5. Their child counts give $3+3+2+1+2=11$, plus the root makes 12.
> The goal line is crossed out because it was removed and returned. It was not expanded. Still on the fringe: $12-5-1=6$ nodes.
> - A: Counts the goal node as expanded. It passes the goal test and is returned, so its children are never generated.
> - B: $3+3+2+1+2=11$ counts only children. The root s is generated too.
> - C: Two slips: the root left out, and the goal counted as expanded.

## 6. Depth-first search (slides 32-35)

### Q17 (Ch.3, slide 32)

Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Run DFS tree search (no check for repeated states). The fringe is a stack, and the alphabetically first child is expanded first. The goal test is done when a node is removed. What is the order of removal, and which path is returned?

- A) S, S-B, S-B-G. Returns S-B-G, cost 11.
- B) S, S-A, S-B, S-A-C, S-A-D, S-B-G. Returns S-B-G, cost 11.
- C) S, S-A, S-A-C, S-B, S-A-D, S-A-D-G. Returns S-A-D-G, cost 6.
- D) S, S-A, S-A-C, S-A-C-G. Returns S-A-C-G, cost 10.

> [!success]- Answer
> **D.** Remove S, push S-A and S-B with S-A on top. Remove S-A, push S-A-C and S-A-D with S-A-C on top. Remove S-A-C, push S-A-C-G. Remove S-A-C-G: goal, stop.
> Cost $1+2+7=10$, after 3 expansions. DFS returns the leftmost solution, not the cheapest (S-A-D-G, 6) and not the shortest (S-B-G).
> - A: Wrong tie-break: the children were pushed so that B is on top. With the stated rule A is expanded first.
> - B: This is a FIFO queue, so it is BFS, not DFS.
> - C: This orders the fringe by path cost $g$, so it is UCS, not DFS.

### Q18 (Ch.3, slide 34)

A tree has branching factor $b=3$ and maximum depth $m=4$ (tiers 0 to 4). How many nodes does it have?

- A) 81
- B) 364
- C) 121
- D) 40

> [!success]- Answer
> **C.** Tiers: $1+3+9+27+81=121$. With the formula: $\frac{3^{5}-1}{3-1}=\frac{242}{2}=121$.
> The bottom tier holds $81/121\approx67\%$ of all nodes. This is why the total is $O(b^{m})$.
> - A: $3^{4}=81$ is only the bottom tier.
> - B: $\frac{3^{6}-1}{2}=364$ uses $b^{m+2}$ in the formula, so it counts tiers 0 to 5.
> - D: $\frac{3^{4}-1}{2}=40$ uses $b^{m}$ in the formula, so it counts only tiers 0 to 3.

### Q19 (Ch.3, slide 35, space)

DFS tree search on a tree with $b=3$ and $m=4$. What is the largest number of nodes on the DFS fringe at any moment?

- A) 12
- B) 9
- C) 81
- D) 121

> [!success]- Answer
> **B.** At each of the 4 levels of the current path, $b-1=2$ siblings wait on the stack. Add the current deepest node: $(b-1)\,m+1=2\cdot4+1=9$.
> - A: $b\cdot m=12$ is the order of growth $O(bm)$. On each level one of the $b$ children has already been taken off the stack, so only $b-1$ wait.
> - C: $3^{4}=81$ is the bottom tier. That is close to the BFS memory, not the DFS memory.
> - D: 121 is the whole tree. DFS forgets a subtree after it has searched it.

### Q20 (Ch.3, slide 35, completeness)

The 4-state graph of slide 23 has the arcs S→a, S→b, a→b, a→G, b→a, b→G. Start S, goal G. Run DFS tree search (no check for repeated states). The fringe is a stack, and children are expanded in the order a before b before G. The goal test is done when a node is removed. What happens?

- A) It returns S-a-G, the shallowest solution, after 2 expansions.
- B) It never stops. It removes S-a, S-a-b, S-a-b-a, S-a-b-a-b, ... and never removes a G node.
- C) It returns S-a-b-G after 3 expansions, because S-a-b-a is skipped.
- D) It returns failure, because it meets the state a a second time on the same path.

> [!success]- Answer
> **B.** Below a the child b comes before G, and below b the child a comes before G. DFS always goes deeper along a, b, a, b. The node S-a-G is generated, but it is never removed, so it is never tested. The tree is infinite ($m=\infty$), so DFS is not complete.
> - A: This tests S-a-G when it is generated. With the goal test on removal it stays on the stack forever.
> - C: Skipping S-a-b-a needs an explored set. That is graph search, not tree search.
> - D: Tree search does not remember states, so it does not notice a repeated a.

## 7. Breadth-first search (slides 37-40)

### Q21 (Ch.3, slide 37)

Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Run BFS tree search (no check for repeated states). The fringe is a FIFO queue, and children enter it in alphabetical order. The goal test is done when a node is removed. What is the order of removal?

- A) S, S-A, S-B, S-A-C, S-A-D, S-B-G. Returns S-B-G after 5 expansions.
- B) S, S-A, S-B. Returns S-B-G after 3 expansions.
- C) S, S-B, S-A, S-B-G. Returns S-B-G after 3 expansions.
- D) S, S-A, S-A-C, S-A-C-G. Returns S-A-C-G after 3 expansions.

> [!success]- Answer
> **A.** Queue after each removal (front first): S-A, S-B. Then S-B, S-A-C, S-A-D. Then S-A-C, S-A-D, S-B-G. Then S-A-D, S-B-G, S-A-C-G. Then S-B-G, S-A-C-G, S-A-D-G. Now S-B-G is removed: goal, stop.
> Path S-B-G, 2 actions, cost $5+6=11$. It is the shallowest solution, not the cheapest (S-A-D-G, 6).
> - B: This stops when S-B-G is generated (eager goal test). The question tests on removal.
> - C: Wrong tie-break: B entered the queue before A.
> - D: This takes the newest node first (LIFO). That is DFS.

### Q22 (Ch.3, slide 38)

Let $b=10$ and let the shallowest solution be at depth $s=6$. How many nodes are in the tiers 0 to 6, and how much memory does the last tier need at 1 kB per node?

- A) 111 111 nodes. The last tier needs 1 GB.
- B) 1 111 111 nodes. The last tier needs 1.1 GB.
- C) 111 111 nodes. The last tier needs 100 MB.
- D) 1 111 111 nodes. The last tier needs 1 GB.

> [!success]- Answer
> **D.** Tiers 0 to 6: $1+10+10^{2}+\dots+10^{6}=1\,111\,111$. Last tier: $10^{6}$ nodes, so $10^{6}$ kB $=1$ GB.
> With $s=7$ both grow about 10 times. BFS time and space both grow like $b^{s}$.
> - A: $111\,111$ is the sum of tiers 0 to 5. One tier is missing.
> - B: 1.1 GB is the memory of all tiers. The question asks for the last tier.
> - C: $10^{5}$ nodes is tier 5, not tier 6. Both numbers are one tier too low.

### Q23 (Ch.3, slide 40)

A binary tree ($b=2$) has maximum depth $m=20$. Case 1: all solutions are at depth 20, and there are many of them. Case 2: the only solution is at depth 2, below the right child of the root. DFS goes left first. Which algorithm is better in each case?

- A) Case 1: BFS. Case 2: DFS.
- B) Case 1: DFS. Case 2: BFS.
- C) Case 1: BFS. Case 2: BFS.
- D) Case 1: DFS. Case 2: DFS.

> [!success]- Answer
> **B.** Case 1: DFS dives to depth 20 in about 20 expansions and keeps about $(b-1)m+1=21$ nodes. BFS must first process all tiers above depth 20, $2^{20}-1=1\,048\,575$ nodes.
> Case 2: BFS processes at most tiers 0 to 2, $1+2+4=7$ nodes. DFS searches the whole left subtree, $2^{20}-1$ nodes, before it tries the right child.
> - A: This swaps the cases. BFS wins for shallow solutions, DFS for deep and plentiful ones.
> - C: In case 1 BFS needs about a million nodes and a whole tier of memory. DFS finds a solution almost at once.
> - D: In case 2 DFS is stuck in the left subtree of about a million nodes.

## 8. Iterative deepening (slide 43)

### Q24 (Ch.3, slide 43)

Practice graph, directed: S→A, S→B, A→C, A→D, B→G, C→G, D→G. Start S, goal G. Run iterative deepening as on the slide: tree search, DFS with depth limit 1, then 2. Children in alphabetical order. A node is tested when it is visited, and a node at the limit is tested but not expanded. Which path is found, and how many node visits happen in total over both rounds?

- A) S-A-C-G, 9 visits.
- B) S-B-G, 6 visits.
- C) S-B-G, 9 visits.
- D) S-A-C-G, 6 visits.

> [!success]- Answer
> **C.** Limit 1: S, S-A, S-B. No goal, 3 visits. Limit 2: S, S-A, S-A-C, S-A-D, S-B, S-B-G. S-B-G is a goal, 6 visits.
> Total $3+6=9$. The path S-B-G is the answer of BFS. The first round is repeated work, but the small part.
> - A: S-A-C-G is the answer of plain DFS. It has depth 3, so the limit 2 never reaches it.
> - B: 6 counts only the last round. The round with limit 1 is work too.
> - D: Two slips: the plain DFS path, and only the last round counted.

## 9. Uniform-cost search (slides 44-48)

### Q25 (Ch.3, slide 44)

Costs on the tiny graph: S→d 3, d→e 2, S→e $x$, e→r 2, r→f 1, f→G 2. BFS returns S-e-r-f-G. The other way to G is S-d-e-r-f-G. For which $x$ is the BFS path also a least-cost path?

- A) $x\le10$
- B) $x\le3$
- C) $x\ge5$
- D) $x\le5$

> [!success]- Answer
> **D.** S-e-r-f-G costs $x+2+1+2=x+5$. S-d-e-r-f-G costs $3+2+2+1+2=10$. The BFS path is least-cost when $x+5\le10$, so $x\le5$. With the slide's $x=9$ it costs 14.
> - A: This compares $x$ alone with 10 and forgets the cost 5 of e-r-f-G.
> - B: This compares $x$ with the arc S→d only. The detour to e is S-d-e, cost $3+2=5$.
> - C: Wrong direction. A larger $x$ makes the BFS path more expensive.

### Q26 (Ch.3, slide 46)

Practice graph, directed, costs in brackets: S→A (1), S→B (5), A→C (2), A→D (4), B→G (6), C→G (7), D→G (1). Start S, goal G. Run UCS tree search (no check for repeated states). The fringe is a priority queue on the path cost $g$, and nodes with equal $g$ come out first-in first-out. The goal test is done when a node is removed. What is the order of removal?

- A) S, S-A, S-A-C, S-B, S-A-D, S-A-D-G
- B) S, S-A, S-A-C, S-A-D, S-B, S-A-D-G
- C) S, S-A, S-A-C, then S-A-C-G is returned
- D) S, S-A, S-A-C, S-A-D, S-A-D-G

> [!success]- Answer
> **A.** Fringe with $g$: S-A (1), S-B (5). Remove S-A: S-A-C (3), S-B (5), S-A-D (5). Remove S-A-C: S-B (5), S-A-D (5), S-A-C-G (10). S-B and S-A-D tie at 5, and S-B entered first. Remove S-B, then S-A-D, then S-A-D-G (6): goal, stop.
> Path S-A-D-G, cost $1+4+1=6$. The goals at 10 and 11 were on the fringe earlier, but UCS tests a node only when it is removed.
> - B: Wrong tie-break: the newer node S-A-D came out before S-B at equal $g=5$.
> - C: This stops when S-A-C-G is generated (eager goal test). It returns cost 10, not the cheapest path.
> - D: The priority here is the cost of the last arc only (S-A-D-G has 1), not the path cost $g$. With $g$, S-B (5) comes out before S-A-D-G (6).

### Q27 (Ch.3, slide 47)

UCS with $b=3$, optimal solution cost $C^{*}=20$ and smallest step cost $\varepsilon=2$. What is the order of the bound $O(b^{C^{*}/\varepsilon})$ on the number of nodes?

- A) 59 049
- B) 1 000
- C) 3 486 784 401
- D) 30

> [!success]- Answer
> **A.** Effective depth $C^{*}/\varepsilon=20/2=10$, bound $3^{10}=59\,049$.
> With one arc of cost 0.5, $\varepsilon=0.5$ and the bound becomes $3^{40}\approx1.2\times10^{19}$. With an arc of cost 0, UCS can follow it forever, so it needs $\varepsilon>0$ to be complete.
> - B: $10^{3}$ swaps base and exponent: depth to the power $b$.
> - C: $3^{20}$ uses $C^{*}$ as the depth and forgets to divide by $\varepsilon$.
> - D: $3\cdot10$ is linear, like the DFS space $b\cdot m$. The tree grows exponentially with depth.

### Q28 (Ch.3, slide 48)

Open grid, moves N, S, E, W, each of cost 1. The goal is 10 steps east of the start. UCS expands every state with cost less than 10 before it reaches the goal. How many states have cost at most 9 from the start?

- A) 221
- B) 19
- C) 181
- D) 100

> [!success]- Answer
> **C.** The states with cost at most $k$ form a diamond around the start with $2k(k+1)+1$ cells. For $k=9$: $2\cdot9\cdot10+1=181$.
> The straight plan needs only 10 steps. UCS spreads in every direction because it knows nothing about where the goal is.
> - A: $k=10$ gives 221. That includes cost 10, the contour of the goal itself.
> - B: $2\cdot9+1$ counts only the cells on the east-west line. UCS also goes north and south.
> - D: $10\cdot10$ is a square. Cells at cost at most 9 form a diamond, not a square.

## 10. The one queue (slide 53)

### Q29 (Ch.3, slide 53)

You have only a priority queue that removes the smallest priority first. The step costs can differ. Which priority makes it behave like DFS?

- A) Minus the depth of the node.
- B) The depth of the node.
- C) The path cost $g$ of the node.
- D) Minus the path cost $g$ of the node.

> [!success]- Answer
> **A.** With priority $-\text{depth}$ the deepest node has the smallest priority and comes out first. That is DFS. In practice a stack does the same in constant time, without the $\log n$ work of a priority queue.
> - B: Depth as priority takes the shallowest node first. That is BFS.
> - C: $g$ as priority takes the cheapest node first. That is UCS.
> - D: This takes the most expensive path first. When step costs differ, the most expensive node is not always the deepest one.

## 11. Search and models (slides 54-55)

### Q30 (Ch.3, slide 54)

A route planner runs UCS on a map in which one bridge is still open. In reality the bridge is closed, and the returned route uses it. Which statement is right?

- A) The plan is optimal for the model but fails in the world. The agent should remove the arc and plan again from where it is.
- B) The plan is not optimal, because UCS made a mistake. The agent should switch to BFS.
- C) The plan is optimal in the world too, because UCS is optimal. The agent should follow it.
- D) The plan is not optimal, because UCS cannot handle a closed bridge. The agent should add a cost of 0 to the bridge.

> [!success]- Answer
> **A.** UCS is optimal on the graph it is given, so the plan is optimal for the model. The model is wrong, so the plan can fail. The fix is to correct the model and replan.
> - B: The algorithm made no mistake. BFS on the same wrong map would use the bridge too.
> - C: Optimal for the model does not mean it works in the world. The bridge is closed.
> - D: A closed bridge must be removed from the graph. Cost 0 would make it even more attractive.

### Q31 (Ch.3, slide 55)

The right map shows a route from Haugesund to Trondheim, both in Norway: 2713.2 km and 47 hours 31 minutes. The line runs far south through other countries. Which explanation is the most likely?

![[cmpe421_lecture_03_ch_3_p055.jpg]]

- A) The search algorithm is not optimal, so it returned a long route although the map has a short one.
- B) The planner used BFS, which returns the route with the fewest roads instead of the shortest one.
- C) The model is wrong: the map data probably missed some road or ferry links inside Norway.
- D) The search ran out of memory and returned the first route it found.

> [!success]- Answer
> **C.** The algorithm found the best route in the graph it had. If links inside Norway are missing from the data, the cheapest path in the model is a long detour. "Your search is only as good as your models." (The missing links are an interpretation; the slide only shows the picture.)
> - A: A route planner uses an optimal search on its graph. The slide's message is about the model.
> - B: A route of 2713 km through several countries would also have many roads. Fewest roads does not explain it.
> - D: A planner out of memory returns no route. The slide does not point to memory at all.

## 12. Handout: problem types (handout pages 2-5, 10)

### Q32 (handout, slide 2)

Vacuum world with rooms A (left) and B (right). The agent knows it starts in room B and both rooms are dirty (state 2). Which plan is a solution, and why is a fixed sequence enough?

- A) [Suck, Left, Suck]. The start state is known and every action has one result, so all future states are known before acting.
- B) [Suck, Right, Suck]. The start state is known and every action has one result, so all future states are known before acting.
- C) [Suck, Left, Suck]. The agent's sensors check for dirt during execution, so it can correct the plan.
- D) [Suck, Right, Suck]. The agent's sensors check for dirt during execution, so it can correct the plan.

> [!success]- Answer
> **A.** State 2, Suck gives 4 (B clean), Left gives 3 (agent in A), Suck gives 7 (both clean). The problem is deterministic and fully observable, so the agent can compute the whole plan offline, and percepts tell it nothing new.
> - B: Room A is on the left. From B, Right bumps the wall, and A stays dirty.
> - C: A classical solution is executed without looking at percepts. The reason is the known start and deterministic actions.
> - D: Two slips: Right goes the wrong way, and a fixed sequence does not use percepts.

### Q33 (handout, slide 3)

No sensors: the start state can be any of 1 to 8. (Odd: agent in A. Even: agent in B. 1, 2: both rooms dirty. 3, 4: only A dirty. 5, 6: only B dirty. 7, 8: both clean.) The agent executes [Right, Suck, Left]. Which set of states is possible now?

- A) $\{1,3,5,7\}$
- B) $\{7\}$
- C) $\{3,7\}$
- D) $\{4,8\}$

> [!success]- Answer
> **C.** Right: surely in B, $\{2,4,6,8\}$. Suck: B surely clean, $\{4,8\}$. Left: surely in A, $\{3,7\}$. A last Suck gives $\{7\}$, so [Right, Suck, Left, Suck] is conformant. No 3-action plan works: it needs a Suck in A, a Suck in B, a Left and a Right.
> - A: This forgets that Suck cleaned B. After Left, B is surely clean, so only 3 and 7 remain.
> - B: Room A may still be dirty. Nothing has cleaned it yet.
> - D: $\{4,8\}$ is the set after Suck. Left moves the agent to A.

### Q34 (handout, slide 4)

The start state is one of 5, 6, 7, 8 (room A clean), and the agent senses its room and the dirt in it. It follows [Right, if dirt then Suck]. Which states are possible after Right, and would the fixed sequence [Right, Suck] also reach the goal?

- A) $\{6,8\}$. No: Suck in a clean room fails the plan.
- B) $\{6,8\}$. Yes: Suck in a clean room changes nothing.
- C) $\{2,4,6,8\}$. Yes: Suck in a clean room changes nothing.
- D) $\{2,4,6,8\}$. No: Suck in a clean room fails the plan.

> [!success]- Answer
> **B.** Room A is clean in 5 to 8. Right: 5 and 6 give 6, 7 and 8 give 8. In 6 Suck gives 8. In 8 Suck changes nothing. So [Right, Suck] also ends in 8. The contingent plan only saves a useless action.
> - A: Suck in a clean room is allowed and leaves the state the same.
> - C: States 2 and 4 have room A dirty. The start states 5 to 8 have A clean, and Right does not change A.
> - D: Two slips: A is known to be clean, and Suck in a clean room does no harm.

### Q35 (handout, slide 5)

In the exploration problem the agent does not know which rooms exist. Why can it not run BFS or UCS before acting?

- A) Because BFS and UCS need sensors during the search, and the agent has none.
- B) Because the state space is infinite, and BFS and UCS never stop on an infinite space.
- C) Because the agent does not know its start state, as in the no-sensor problem.
- D) Because it does not know the successor function, so it cannot build a search tree in its head.

> [!success]- Answer
> **D.** Offline search needs, for each state, the actions and their results. Here the model itself is unknown, so the agent must act, observe and learn the state space step by step (online).
> - A: Offline search uses no sensors at all. It works on the model.
> - B: BFS is complete on infinite spaces when $b$ is finite and a solution exists. The problem is the missing model, not the size.
> - C: That is the no-sensor problem: model known, state unknown. Here the model is unknown.

### Q36 (handout, slide 10)

The vacuum world with 2 rooms has 8 states. Now take 3 rooms in a row. How many states are there, and how many goal states (all rooms clean)?

- A) 24 states, 3 goal states.
- B) 16 states, 3 goal states.
- C) 24 states, 1 goal state.
- D) 16 states, 1 goal state.

> [!success]- Answer
> **A.** 3 agent positions times $2^{3}=8$ dirt patterns: $3\cdot8=24$ (check with 2 rooms: $2\cdot2^{2}=8$). All rooms clean with the agent anywhere: 3 goal states.
> The slide gives NoOp cost 0, so $\varepsilon=0$: UCS tree search could remove NoOp children of cost 0 forever.
> - B: $2\cdot8$ doubles the 2-room count. One more room multiplies the positions by $3/2$ and the dirt patterns by 2.
> - C: The goal fixes the dirt but not the agent's room, so each of the 3 positions gives a goal state.
> - D: Two slips: the state count doubled, and the agent position forgotten in the goal states.

## 13. Handout: nodes, eager and cautious goal test (handout pages 17, 19)

### Q37 (handout, slide 17)

Two nodes of an 8-puzzle search tree hold the same board, one with depth 6 and one with depth 8. Is that possible, and how do you get the solution from a goal node?

- A) Not possible: each state appears only once in a search tree. Follow the parent pointers to the root and reverse the actions.
- B) Possible: depth belongs to the node. Read the solution from the state stored in the goal node.
- C) Not possible: each state appears only once in a search tree. Read the solution from the state stored in the goal node.
- D) Possible: depth belongs to the node. Follow the parent pointers to the root and reverse the actions.

> [!success]- Answer
> **D.** The same board can be reached by different move sequences, and each sequence is its own node. Depth and $g$ belong to the node, not to the state. The plan is found by following parent pointers up to the root, collecting the actions, and reversing them.
> - A: "Each state once" is true of the state space graph, not of the search tree.
> - B: A state is only a board. It has no parent and no action, so it does not contain the plan.
> - C: Two slips: graph and tree mixed up, and the state used as if it held the plan.

### Q38 (handout, slide 19)

BFS tree search on a tree with $b=3$. The only goal is the last node of depth 2. Cautious BFS tests a node when it is removed from the queue. How many nodes does cautious BFS generate?

- A) 40
- B) 13
- C) 36
- D) 37

> [!success]- Answer
> **D.** Tiers 0 to 2: $1+3+9=13$. The goal is the last node of depth 2, so the other $9-1=8$ nodes of depth 2 are removed and expanded first. They generate $8\cdot3=24$ nodes. Total $13+24=37$.
> - A: $13+9\cdot3$ also expands the goal node. The goal is returned when it is removed, not expanded.
> - B: 13 is the eager count, with the goal test at generation.
> - C: $12+24$ leaves out the root.

## 14. Handout: depth-limited search and IDS (handout pages 57, 58, 67)

### Q39 (handout, slide 57)

A tree without any goal: R has the children X and Y, X has the child Z, and Y and Z have no children. In LIMITED-DEPTH-SEARCH a node reached with limit 0 returns Cutoff, even without children. A node with no children reached with a limit above 0 returns Failure. What is returned for limit 1, 2 and 3?

- A) Cutoff, Cutoff, Failure
- B) Cutoff, Failure, Failure
- C) Failure, Failure, Failure
- D) Cutoff, Cutoff, Cutoff

> [!success]- Answer
> **A.** Limit 1: X and Y are reached with limit 0, both Cutoff. Limit 2: Y (limit 1, no children) gives Failure, but Z is reached with limit 0 and gives Cutoff, so X and R give Cutoff. Limit 3: Z has limit 1 and no children, Failure everywhere.
> Iterative deepening stops on Failure. Without it, it would raise the limit forever.
> - B: With limit 2, Z sits exactly at the limit, so it returns Cutoff. The code does not check whether Z has children.
> - C: This treats every childless node as Failure. A node at limit 0 returns Cutoff.
> - D: Limit 3 reaches every node above the limit, so no node is cut off. The result is Failure.

### Q40 (handout, slide 58)

Iterative deepening with limits 0, 1, 2, ... on a tree with $b=3$ and the solution at depth $d=4$. Count every node generated in every round, in the worst case. How many nodes in total?

- A) 121
- B) 179
- C) 605
- D) 178

> [!success]- Answer
> **B.** A node of depth $i$ is generated in the rounds with limit $i,\dots,d$, so $d-i+1$ times. Total: $5\cdot1+4\cdot3+3\cdot9+2\cdot27+1\cdot81=179$. One search of tiers 0 to 4 is 121, so the overhead is about 48%.
> - A: 121 counts every node once, as one BFS of tiers 0 to 4.
> - C: $5\cdot121$ counts every round as a search of the full tree. The early rounds stop at small depths.
> - D: The root is counted 4 times, as if the limits started at 1. With limit 0 it is generated 5 times.

### Q41 (handout, slide 67)

For $b=2$ and $d=10$, the IDS node count is $\sum_{i=0}^{10}(11-i)\,2^{i}$. What is this count, and about how many nodes does IDS keep in memory?

- A) 2047 nodes generated, about 20 in memory.
- B) 4083 nodes generated, about 1024 in memory.
- C) 4083 nodes generated, about 20 in memory.
- D) 2047 nodes generated, about 1024 in memory.

> [!success]- Answer
> **C.** The sum is 4083. All tiers once: $2^{11}-1=2047$. Ratio $\approx1.99$, close to $b/(b-1)=2$. With small $b$ the upper tiers are not small compared with the last tier. Memory: about $b\cdot d=20$ nodes, against $2^{10}=1024$ in the last tier.
> - A: 2047 counts every node once. IDS repeats the upper tiers in every round.
> - B: 1024 is the last tier, the BFS memory. IDS is a DFS and keeps only the path and siblings.
> - D: Both numbers belong to BFS, not to IDS.

## 15. Handout: summary, repeated states, graph search (handout pages 30, 68-70)

### Q42 (handout, slide 30)

The slide says: 12 hours at 200 MB per second need 8.64 TB. How much memory is filled in 1 hour at 100 MB per second?

- A) 6 GB
- B) 360 GB
- C) 720 GB
- D) 4.32 TB

> [!success]- Answer
> **B.** 1 hour $=3600$ s. $3600\cdot100$ MB $=360\,000$ MB $=360$ GB. (Check of the slide: $43\,200\cdot200$ MB $=8.64$ TB.) Memory runs out long before time does.
> - A: $60\cdot100$ MB uses 60 seconds for one hour.
> - C: $8.64$ TB$/12$ changes only the time. The rate is also half as large.
> - D: $8.64$ TB$/2$ changes only the rate. The time is also 12 times shorter.

### Q43 (handout, slide 68)

A problem has $b=10$, the shallowest solution at depth $d=6$, every step cost 1, an infinite state space, and memory for 10 000 nodes. Which algorithm do you choose?

- A) BFS: it is complete and optimal for equal step costs.
- B) Iterative deepening: complete, optimal for equal step costs, and about $b\,d=60$ nodes in memory.
- C) DFS: it needs only about $b\,m$ nodes in memory.
- D) Depth-limited search with limit 5: it needs only about $b\,l=50$ nodes in memory.

> [!success]- Answer
> **B.** IDS is complete, optimal when all steps cost the same, and keeps about 60 nodes. It generates $1\,234\,567$ nodes, against $1\,111\,111$ for one pass over tiers 0 to 6.
> - A: BFS needs about $b^{d}=10^{6}$ nodes in memory, far above 10 000.
> - C: The space is infinite, so $m=\infty$. DFS may never return.
> - D: The solution is at depth 6, so limit 5 never reaches it. Depth-limited search is complete only if $l\ge d$.

### Q44 (handout, slide 69)

In the graph on the slide every state has two arcs to the next state (A to B, B to C, and so on). Take a chain with states down to depth 20 (A at depth 0). How many states are there, and how many nodes does the search tree have in total down to depth 20?

![[cmpe421_lecture_03_uninformed_search_p069.jpg]]

- A) 20 states, 2 097 151 tree nodes.
- B) 21 states, 1 048 575 tree nodes.
- C) 21 states, 2 097 151 tree nodes.
- D) 20 states, 1 048 575 tree nodes.

> [!success]- Answer
> **C.** One state per level, depths 0 to 20: 21 states. Each node has 2 children, so depth $k$ has $2^{k}$ nodes. Total $2^{21}-1=2\,097\,151$. The states grow linearly, the tree exponentially.
> - A: Depths 0 to 20 are 21 levels. The start state A is a state too.
> - B: $2^{20}-1$ is the sum of depths 0 to 19. Depth 20 alone has $2^{20}$ nodes.
> - D: Two slips: the start state left out, and depth 20 left out of the sum.

### Q45 (handout, slide 70)

The 4-state graph S→a, S→b, a→b, a→G, b→a, b→G. Start S, goal G. Run GRAPH-SEARCH as on the slide: a node is goal-tested when it is removed, then skipped if its state is already in the explored set, otherwise expanded and its state added. Depth-first rule: new nodes go to the front of the queue, in the order a before b before G. Which path is returned?

- A) S-a-b-G
- B) S-a-G
- C) None: the search runs a, b, a, b forever.
- D) S-b-G

> [!success]- Answer
> **A.** Remove S, explored {S}, front: S-a, S-b. Remove S-a, explored {S, a}, front: S-a-b, S-a-G, S-b. Remove S-a-b, explored {S, a, b}, front: S-a-b-a, S-a-b-G, ... Remove S-a-b-a: a is explored, skip. Remove S-a-b-G: goal. 3 actions.
> The explored set makes DFS complete on this finite graph, but not optimal: S-a-G has 2 actions.
> - B: This tests S-a-G when it is generated. With the test on removal, S-a-b comes off the front first.
> - C: This is tree search without the explored set. Here S-a-b-a is skipped, so the loop is cut.
> - D: Wrong tie-break: the children were put in front one at a time, so the order is reversed (G before b before a). Then S-b comes off first, and S-b-G is in front of S-b-a.
