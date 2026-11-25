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

> [!question]- Check (informed-search, slide 1): The slide says heuristics can make search much faster on average. Which two guarantees can you lose, depending on the heuristic?
> You can lose optimality: the solution found may not be the cheapest one.
> You can lose completeness: the search may find no solution at all, even if one exists.
> Greedy search later in this lecture shows both problems. A* with an admissible heuristic keeps both guarantees.

> [!question]- Check (informed-search, slide 2): The frontier holds three nodes: P with $g=3$, $h=9$; Q with $g=8$, $h=2$; R with $g=5$, $h=4$. Which node is removed next by uniform-cost search, by greedy search and by A*?
> Each algorithm removes the node with the lowest $f$.
> - Uniform-cost, $f=g$: P 3, Q 8, R 5. It removes **P**.
> - Greedy, $f=h$: P 9, Q 2, R 4. It removes **Q**.
> - A*, $f=g+h$: P $3+9=12$, Q $8+2=10$, R $5+4=9$. It removes **R**.
>
> The code is the same for all three. Only the evaluation function changes.

> [!question]- Check (informed-search, slide 3): A node $x$ is removed from the frontier. Its state is not a goal and is already in `explored`. What does `Graph-Search` do with $x$? What would `Cautious-Tree-Search` (slide 2) do?
> `Graph-Search` throws $x$ away: it does not expand it and adds no new nodes.
> `Cautious-Tree-Search` has no `explored` set, so it expands $x$ again and repeats the work below it.
> Note the order of the tests on the slide: the goal test comes first, and it happens when the node is removed, not when it is generated.

> [!question]- Check (informed-search, slide 4): Use the map and the table for Hirsova and Urziceni. (a) Is $h_{\mathrm{SLD}}$ admissible (never larger than the true road distance to Bucharest) at these two cities? (b) Does the edge Hirsova to Urziceni satisfy $h(x)\le c(x,y)+h(y)$?
> (a) True road distances to Bucharest: Urziceni $h^{*}=85$. Hirsova $h^{*}=98+85=183$.
> Table: $h(\text{Urziceni})=80\le 85$ and $h(\text{Hirsova})=151\le 183$. Both are admissible.
> (b) $h(\text{Hirsova})=151$ and $c+h(\text{Urziceni})=98+80=178$. $151\le 178$, so the edge satisfies the inequality.
> This inequality is called consistency. It is defined in section 5.

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

> [!question]- Check (informed-search, slide 5): Run greedy search on this small graph and say if the result is optimal. Directed edges with costs: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3. Heuristic: $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Start S, goal G, tree search, goal test when a node is removed, ties in alphabetical order.
> Greedy ranks by $f=h$ only.
> 1. Remove S ($h=6$) and expand it. Frontier: A ($h=5$), B ($h=3$).
> 2. Remove B (lowest $h$) and expand it. Frontier: A (5), C (2), G (0).
> 3. Remove G ($h=0$). It is the goal, stop.
>
> Order of expansion: S, B. Path found: S, B, G with cost $5+5=10$.
> The cheapest path is S, A, B, C, G with cost $1+2+1+3=7$. Greedy is not optimal here. It never looks at the edge costs, so the expensive edge S→B (5) does not stop it.

> [!question]- Check (informed-search, slides 6-7): Run greedy search from **Lugoj** to Bucharest with the map and the $h_{\mathrm{SLD}}$ table on the slide. Tree search, goal test when a node is removed. Give the order of expansion, the route and its length.
> Greedy always removes the frontier node with the lowest $h$.
> 1. Expand Lugoj (244). Frontier: Mehadia 241, Timisoara 329.
> 2. Expand Mehadia (241). Frontier: Dobreta 242, Lugoj 244, Timisoara 329.
> 3. Expand Dobreta (242). Frontier: Craiova 160, Mehadia 241, Lugoj 244, Timisoara 329.
> 4. Expand Craiova (160). New nodes: Pitesti 98, Rimnicu Vilcea 193, Dobreta 242.
> 5. Expand Pitesti (98). New nodes: Bucharest 0, Craiova 160, Rimnicu Vilcea 193.
> 6. Remove Bucharest (0): goal.
>
> Route: Lugoj, Mehadia, Dobreta, Craiova, Pitesti, Bucharest. Length $70+75+120+138+101=504$ km. 5 expansions.
> Here greedy is lucky: 504 km is also the shortest route from Lugoj. In step 3, $h$ goes up from 241 to 242. That is allowed: greedy takes the lowest value **on the frontier**, not a value lower than the last one.

> [!question]- Check (informed-search, slides 8-9): On slide 8 greedy chooses Fagaras (176) and not Rimnicu Vilcea (193). Compute the road distance from Sibiu to Bucharest through each of them. What does greedy not see?
> Through Fagaras: $99+211=310$ km. Through Rimnicu Vilcea and Pitesti: $80+97+101=278$ km.
> The Rimnicu Vilcea way is $310-278=32$ km shorter. This is exactly the difference between the greedy route (450 km) and the optimal route (418 km).
> Greedy compares only $h$: $176<193$. It does not see $g$, the cost already paid, and it does not see that the last edge from Fagaras is very long (211 km).

> [!question]- Check (informed-search, slide 14): Find the mistake: "Greedy search and a greedy algorithm are the same thing. Both become complete in a finite space if we add loop checking."
> They are different. Greedy **search** keeps the whole frontier. If the path it follows ends in a dead end, it goes back to another frontier node. With loop checking in a finite space it is complete.
> A greedy **algorithm** keeps only the current path and never goes back. If its choices lead to a dead end, it fails, and loop checking cannot repair that. It runs in time $O(l)$ for a solution of length $l$, and it is normally used where every path leads to a solution.

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

> [!question]- Check (informed-search, slide 15): Run A* on this graph. Directed edges with costs: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3. Heuristic: $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Start S, goal G, tree search, goal test when a node is removed, ties in alphabetical order. List $f=g+h$ for the frontier after every expansion.
> 1. Expand S ($f=0+6=6$). Frontier: A $1+5=6$, B $5+3=8$.
> 2. Expand A (6). New: B via A $3+3=6$, C via A $6+2=8$. Frontier: B 6, B 8, C 8.
> 3. Expand B with $g=3$ (6). New: C $4+2=6$, G $8+0=8$. Frontier: C 6, B 8, C 8, G 8.
> 4. Expand C with $g=4$ (6). New: G $7+0=7$. Frontier: G 7, B 8, C 8, G 8.
> 5. Remove G with $f=7$: goal.
>
> Order of expansion: S, A, B, C. Path: S, A, B, C, G with cost $1+2+1+3=7$, the optimal cost.
> Greedy search on the same graph returns S, B, G with cost 10. A* avoids it because the node B reached directly from S has $g=5$, so $f=8$.
> G was generated in step 3 with $f=8$. A* did not stop there, because the goal test happens on removal.

> [!question]- Check (informed-search, slides 16-17): Slide 17 shows the node Zerind with $449=75+374$. Compute $f$ for its child Oradea (path Arad, Zerind, Oradea). On the next slide Oradea appears below Sibiu with $f=671$. Are these the same node?
> Via Zerind: $g=75+71=146$, $h=380$, $f=146+380=526$.
> Via Sibiu (slide 18): $g=140+151=291$, $f=291+380=671$.
> They are two different **nodes** for the same **state**. $h$ depends only on the state, so it is 380 in both. $g$ depends on the path, so $f$ differs. In tree search both nodes can be in the frontier at the same time.

> [!question]- Check (informed-search, slides 18-19): Suppose $h(\text{Rimnicu Vilcea})$ were 196 instead of 193 (the true road distance to Bucharest is $97+101=198$). How do the order of expansion and the result of A* from Arad change?
> 196 is still admissible because $196\le 198$.
> New value: $f(\text{Rimnicu Vilcea})=220+196=416$. Fagaras has $f=239+176=415$, so now Fagaras is expanded **before** Rimnicu Vilcea.
> New order: Arad (366), Sibiu (393), Fagaras (415), Rimnicu Vilcea (416), Pitesti (417), then Bucharest (418) is removed.
> The result is the same: Arad, Sibiu, Rimnicu Vilcea, Pitesti, Bucharest, 418 km, 5 expansions. Only the order of two expansions changed.

> [!question]- Check (informed-search, slides 20-21): On slide 21 the frontier holds two Bucharest nodes, $f=418$ and $f=450$. A* removes the one with 418 and stops. Read the other frontier values on the slide. Why can no route shorter than 418 km be hidden behind them?
> Other frontier nodes: Timisoara 447, Zerind 449, Bucharest 450, Craiova 526, Sibiu 553, Sibiu 591, Rimnicu Vilcea 607, Craiova 615, Arad 646, Oradea 671.
> The smallest of them is 447, which is larger than 418.
> With an admissible $h$, $f(x)=g(x)+h(x)$ is never more than the true cost of the best solution through $x$. So every solution through one of these nodes costs at least 447 km.

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

> [!info] The optimality proof in words (slide 23)
> Suppose a suboptimal goal node $G_2$ is in the frontier. Let $G$ be an optimal goal node. Some node $x$ on an optimal path to $G$ is also in the frontier: the first node of that path that is not expanded yet.
> - $f(G_2)=g(G_2)$, because $h=0$ at a goal.
> - $g(G_2)>g(G)$, because $G_2$ is not optimal.
> - $g(G)=g(x)+h^{*}(x)\ge g(x)+h(x)=f(x)$, because $x$ is on an optimal path and $h$ never overestimates.
>
> Together: $f(G_2)>f(x)$. A* always removes the lowest $f$, so it removes $x$ before $G_2$. This is true as long as $G_2$ waits in the frontier, so $G_2$ is never returned.

> [!question]- Check (informed-search, slide 22): Directed edges with costs: S→A 3, S→B 1, B→A 1, A→G 4, B→G 7. Goal G. A heuristic gives $h(S)=6$, $h(A)=4$, $h(B)=6$, $h(G)=0$. Compute $h^{*}$ for every node and decide if $h$ is admissible.
> $h^{*}(G)=0$.
> $h^{*}(A)=4$ (only A→G).
> $h^{*}(B)=\min(1+h^{*}(A),\,7)=\min(5,7)=5$.
> $h^{*}(S)=\min(3+h^{*}(A),\,1+h^{*}(B))=\min(7,6)=6$.
> Check $h\le h^{*}$: S $6\le 6$ yes. A $4\le 4$ yes. B $6\le 5$ **no**. G $0\le 0$ yes.
> $h$ is not admissible, because it overestimates at B. The largest admissible value at B is 5.

> [!question]- Check (informed-search, slide 23): In the proof, which line uses admissibility, and why is $g(G)\ge g(x)+h(x)$ true? Then check the chain with numbers: $g(G_2)=12$, optimal cost $g(G)=10$, and $x$ on the optimal path with $g(x)=4$, $h(x)=5$.
> The line "$\ge g(x)+h(x)$" uses admissibility.
> $x$ is on an optimal path to $G$, so $g(G)=g(x)+h^{*}(x)$. Admissible means $h(x)\le h^{*}(x)$, so $g(G)\ge g(x)+h(x)=f(x)$.
> Numbers: $f(G_2)=12+0=12$. $h^{*}(x)=10-4=6$, and $h(x)=5\le 6$. $f(x)=4+5=9$.
> $f(x)=9<12=f(G_2)$, so A* removes $x$ before $G_2$.

> [!question]- Check (informed-search, slide 24): The step costs along one infinite path are $\tfrac12,\tfrac14,\tfrac18,\dots$ Why does this break the completeness requirement, and why does "every step costs at least $\varepsilon>0$" repair it?
> The total cost of the infinite path is $\tfrac12+\tfrac14+\tfrac18+\dots=1$, a finite number. This is an infinite path with a finite cost.
> Suppose a solution costs 5 and $h=0$ on this path. Then every node of the path has $f<1<5$. A* expands these nodes forever and never removes the goal.
> With step costs of at least $\varepsilon$, a path with $n$ steps costs at least $n\varepsilon$, which grows without limit. Every infinite path then becomes more expensive than the solution, and A* must leave it.

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

> [!question]- Check (informed-search, slide 25): Directed edges with costs: S→A 2, S→B 3, A→C 4, B→C 1, C→G 3. Heuristic: $h(S)=7$, $h(A)=6$, $h(B)=3$, $h(C)=3$, $h(G)=0$. Is $h$ admissible? Is it consistent? Check edge by edge.
> **Admissible.** $h^{*}(C)=3$, $h^{*}(B)=1+3=4$, $h^{*}(A)=4+3=7$, $h^{*}(S)=\min(2+7,\,3+4)=7$.
> $7\le 7$, $6\le 7$, $3\le 4$, $3\le 3$, $0\le 0$: admissible.
> **Consistent.** Test $h(x)\le c(x,y)+h(y)$ on every edge:
> - S→A: $7\le 2+6=8$ yes.
> - S→B: $7\le 3+3=6$ **no**.
> - A→C: $6\le 4+3=7$ yes.
> - B→C: $3\le 1+3=4$ yes.
> - C→G: $3\le 3+0=3$ yes.
>
> $h$ is admissible but not consistent. On the edge S→B the value of $f$ drops: $f(S)=0+7=7$, $f(B)=3+3=6$.

> [!question]- Check (informed-search, slide 30): In the figure the left node has $h=5$. Its edge to the bottom node costs 1, and the bottom node has $h=1$. For which values of $h$ at the left node is this edge consistent? The slide does not show the cost from the bottom node to the goal (it shows only "..."). Assume this cost is 4, as in the example above. With this assumption, is $h=5$ admissible at the left node?
> Consistent on this edge: $h(\text{left})\le 1+1=2$.
> Admissible (with the assumed cost 4): the only way from the left node to the goal goes through the bottom node, so $h^{*}(\text{left})=1+4=5$, and $h=5\le 5$ is admissible.
> Every value with $2<h\le 5$ is admissible but inconsistent here. With $h=5$, $f$ goes from 7 at the left node down to 4 at the bottom node.

> [!question]- Check (informed-search, slide 31): `A*-Graph-Search` removes a node $x$ with state C and $f(x)=4$. `explored` holds a node with state C and $f=5$. Is $x$ expanded? What if the node in `explored` had $f=4$?
> The test is: expand $x$ only if there is **no** node $y$ in `explored` with the same state and $f(y)\le f(x)$.
> First case: $5\le 4$ is false. No such $y$ exists, so $x$ is added to `explored` and expanded again. This is the re-expansion that plain `Graph-Search` does not do.
> Second case: $4\le 4$ is true. The old node is at least as good, so $x$ is thrown away.

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

> [!question]- Check (informed-search, slide 36): The slide gives time $O(d)$ in the best case. With which heuristic does this happen, and why does A* then expand only about $d$ nodes?
> It happens with the perfect heuristic $h=h^{*}$ (and ties broken toward the deeper node).
> Then every node on an optimal path has $f=g+h^{*}=C^{*}$, and every node off the optimal paths has $f>C^{*}$. A* expands no node with $f>C^{*}$, so it walks straight down one optimal path: $d$ expansions for a solution at depth $d$.
> The other extreme is $h=0$: A* is uniform-cost search and can expand the whole state space.

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

> [!question]- Check (informed-search, slide 37): A student makes the problem **harder** by adding a constraint (for the 8-puzzle: tile 1 may never move), solves it optimally and uses the cost as $h$. Is this $h$ admissible? Compare with a relaxation.
> No. A relaxation removes constraints: every real solution is still allowed, so the best relaxed cost is $\le h^{*}$.
> Adding a constraint forbids some real solutions. The best remaining solution costs $\ge h^{*}$, or does not exist at all. This $h$ can overestimate, so it is not admissible.

> [!question]- Check (informed-search, slide 38): Which constraint of the route problem is removed to get $h_{\mathrm{SLD}}$? Read the true road distance from Pitesti to Bucharest off the map and compare it with the table value on the slide.
> Removed constraint: "you must drive on the roads of the map". In the relaxed problem you may go in a straight line from any city to Bucharest.
> Pitesti: road distance $h^{*}=101$ km (the direct road). Table: $h_{\mathrm{SLD}}=98$. $98\le 101$, as a relaxation promises.

> [!question]- Check (informed-search, slide 39): Four cities with distances AB 2, AC 3, AD 4, BC 4, BD 5, CD 3. Compute the cost of the minimum spanning tree and the cost of the best tour. Does the MST give a lower bound?
> MST (take the cheapest edges that make no cycle): AB 2, AC 3, CD 3. Cost $2+3+3=8$.
> There are three different tours:
> - A, B, C, D, A: $2+4+3+4=13$
> - A, B, D, C, A: $2+5+3+3=13$
> - A, C, B, D, A: $3+4+5+4=16$
>
> Best tour: 13. $8\le 13$, so the MST cost is a lower bound.
> Reason: remove one edge from a tour and you get a path through all cities. A path is a tree, and no tree is cheaper than the minimum spanning tree.

> [!question]- Check (informed-search, slide 41): Compute $h_1$ for the board 8 1 3 / 4 _ 2 / 7 6 5 with the goal 1 2 3 / 4 5 6 / 7 8 _ of the slide.
> Compare each square with the goal and do not count the blank.
> Row 1: 8 (goal 1) wrong, 1 (goal 2) wrong, 3 right.
> Row 2: 4 right, blank, 2 (goal 6) wrong.
> Row 3: 7 right, 6 (goal 8) wrong, 5 (goal: the blank square) wrong.
> Misplaced tiles: 8, 1, 2, 6, 5. $h_1=5$.
> In relaxation 1 each misplaced tile needs exactly one jump, so the optimal relaxed cost is the number of misplaced tiles.

> [!question]- Check (informed-search, slide 43): Compute $h_2$ for the same board 8 1 3 / 4 _ 2 / 7 6 5 (goal 1 2 3 / 4 5 6 / 7 8 _). The shortest real solution has 14 moves. Check $h_1\le h_2\le h^{*}$.
> Manhattan distance = rows moved + columns moved, for each tile.
> - Tile 8: from row 1, column 1 to row 3, column 2: $2+1=3$.
> - Tile 1: from row 1, column 2 to row 1, column 1: 1.
> - Tile 2: from row 2, column 3 to row 1, column 2: $1+1=2$.
> - Tile 6: from row 3, column 2 to row 2, column 3: $1+1=2$.
> - Tile 5: from row 3, column 3 to row 2, column 2: $1+1=2$.
> - Tiles 3, 4, 7: 0.
>
> $h_2=3+1+2+2+2=10$. With $h_1=5$ and $h^{*}=14$: $5\le 10\le 14$.

## 8. Dominance (slides 44-45)
![[cmpe421_lecture_04_informed_search_p044.jpg]]
![[cmpe421_lecture_04_informed_search_p045.jpg]]

**Notes.** If $h_a$ and $h_b$ are admissible and $h_a(x)\le h_b(x)$ for all $x$, then $h_b$ **dominates** $h_a$ and is better for search. For the 8-puzzle $h_1\le h_2\le h^{*}$.

$h(x)=\max(h_a(x),h_b(x))$ is admissible and dominates both. Faking dominance: $c\cdot h$ with $c>1$ is probably not admissible, so the solution may not be optimal, but the search is faster. A larger $c$ is closer to greedy search.

> [!warning] Watch out
> The maximum of admissible heuristics is admissible. The sum is not in general. The blank does not count as a misplaced tile.

> [!info] Effective branching factor (beyond the slides)
> The table on slide 44 compares node counts. One number can summarise such a count: the **effective branching factor** $b^{*}$. If a search produces $N$ nodes and the solution is at depth $d$, then $b^{*}$ is the branching factor of a uniform tree of depth $d$ that has $N+1$ nodes:
>
> $$N+1=1+b^{*}+(b^{*})^{2}+\dots+(b^{*})^{d}.$$
>
> A good heuristic has $b^{*}$ close to 1.
> The slide gives only the node counts $N$: 3,473,941 (IDS), 539 ($h_1$), 113 ($h_2$) at $d=14$, and 39,135 ($h_1$), 1,641 ($h_2$) at $d=24$. The slide gives no $b^{*}$ values. The values below are computed here: put the slide's $N$ and $d$ into the equation and solve it for $b^{*}$ numerically.
> - $d=14$: IDS about 2.84, A* with $h_1$ about 1.44, A* with $h_2$ about 1.25.
> - $d=24$: A* with $h_1$ about 1.48, A* with $h_2$ about 1.28.
>

> [!question]- Check (informed-search, slide 44): (a) From the table: how many times fewer nodes does A* with $h_2$ need than A* with $h_1$ at $d=14$ and at $d=24$? (b) A search finds a solution at depth $d=5$ and generates $N=62$ nodes. Show that its effective branching factor is $b^{*}=2$.
> (a) $d=14$: $539/113\approx 4.8$ times. $d=24$: $39135/1641\approx 23.8$ times. The advantage of the dominating heuristic grows with the depth.
> (b) $b^{*}$ solves $N+1=1+b^{*}+(b^{*})^{2}+\dots+(b^{*})^{d}$.
> With $b^{*}=2$ and $d=5$: $1+2+4+8+16+32=63=62+1$. So $b^{*}=2$.

> [!question]- Check (informed-search, slide 45): Three states have $h^{*}=6,\ 5,\ 7$. Two admissible heuristics give $h_a=3,\ 5,\ 2$ and $h_b=4,\ 1,\ 6$. Compute $\max(h_a,h_b)$, $h_a+h_b$ and $2h_a$ for each state. Which of the three are admissible?
> $\max(h_a,h_b)=4,\ 5,\ 6$. Compare with $6,\ 5,\ 7$: never larger, so it is admissible. It is $\ge h_a$ and $\ge h_b$ everywhere, so it dominates both.
> $h_a+h_b=7,\ 6,\ 8$. All three are larger than $h^{*}$: not admissible.
> $2h_a=6,\ 10,\ 4$. The second state has $10>5$: not admissible. This is "faked" dominance with $c=2$: the search can be faster, but the solution may not be optimal.

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

> [!question]- Check (informed-search, slide 46): Run IDA* on this graph. Directed edges with costs: S→A 1, S→B 5, A→B 2, A→C 5, B→C 1, B→G 5, C→G 3. Heuristic: $h(S)=6$, $h(A)=5$, $h(B)=3$, $h(C)=2$, $h(G)=0$. Start S, goal G, tree search, children in alphabetical order. Give the limit of each round and the $f$ values where the search backtracks.
> First limit: $f_{\max}=h(S)=6$.
> **Round 1 (limit 6).** Depth-first: S ($f=6$), A ($1+5=6$), B ($3+3=6$), C ($4+2=6$) are expanded.
> Backtrack at G below C ($f=7+0=7$), at G below B ($f=8+0=8$), at C below A ($f=6+2=8$), at B below S ($f=5+3=8$).
> No solution. The new limit is the smallest of these values: $\min(7,8,8,8)=7$.
> **Round 2 (limit 7).** S, A, B, C are expanded again, then G with $f=7\le 7$ is reached.
> Solution: S, A, B, C, G with cost 7. Limits used: 6, 7. Expansions: $4+4=8$, where A* needs 4.

> [!question]- Check (informed-search, slide 51): The time bound needs many nodes with the same $f$. Take the bad case: 100 nodes have $f\le C^{*}$ and all their $f$ values are different. About how many expansions does IDA* make, and how many does A* make?
> Each round raises the limit to the next $f$ value, so each round reaches only one new node.
> Round $k$ expands $k$ nodes. Total: $1+2+\dots+100=\dfrac{100\cdot 101}{2}=5050$ expansions.
> A* expands each node once: about 100.
> This is why the slide asks for integer $f$ values and exponential growth: then the last round dominates and the repeated work is small. The gain of IDA* is memory: $O(bd)$, only the current path and the siblings along it.

## 10. Summary of informed search (slide 52)
![[cmpe421_lecture_04_informed_search_p052.jpg]]

**Notes.** Greedy: lowest $h$, fast, incomplete, not optimal. A*: lowest $g+h$, optimal with an admissible (tree) or consistent (graph) heuristic, memory-hungry. IDA*: A*'s optimality in $O(bd)$ memory, paid for with repeated rounds.

> [!question]- Check (informed-search, slide 52): Choose greedy, A* or IDA* for each case. (a) The optimal solution is needed and memory is very small. (b) Any solution is fine and it must come fast. (c) The optimal solution is needed with as few expansions as possible, and memory is large.
> (a) IDA*: optimal with an admissible $h$, and it stores only one path, $O(bd)$.
> (b) Greedy: it follows the lowest $h$ and often reaches a goal quickly, but the solution may be poor (and in tree search it can loop).
> (c) A*: with an admissible (tree) or consistent (graph) heuristic it is optimal and does not repeat rounds, but it keeps all nodes in memory.

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

> [!question]- Check (Ch.4, slide 10): Use the graph on the slide (S→a 1, a→b 1, b→c 1, a→d 3, a→e 8, e→d 1, d→G 2, $h$ values as printed). Start S, goal G, tree search, goal test when a node is removed. (a) Give the A* order of expansion with the $f$ values. (b) Which nodes does uniform-cost search expand that A* does not? (c) Which node does greedy expand after a? After that expansion two d nodes with the same $h=2$ are in the frontier: one reached from a and one reached from e. The slide's red path takes the d below e. Which path and cost does greedy return with this choice, and which with the other d?
> (a) Expand S ($0+6=6$). Frontier: a $1+5=6$.
> Expand a (6). Frontier: b $2+6=8$, d $4+2=6$, e $9+1=10$.
> Expand d (6). Frontier: G $6+0=6$, b 8, e 10.
> Remove G (6): goal. Order S, a, d. Path S, a, d, G with cost 6.
> (b) UCS ranks by $g$: S (0), a (1), b (2), c (3), d (4), then G (6) is removed. It also expands b and c, which lead nowhere. A* skips them because their $h$ is high.
> (c) Greedy ranks by $h$: after a the frontier is b 6, d 2, e 1. It expands e (lowest $h$), although the edge a→e costs 8.
> Expanding e generates d again. Frontier: d below a ($h=2$, $g=4$), d below e ($h=2$, $g=1+8+1=10$), b 6. The two d nodes tie, because greedy looks only at $h$.
> Slide's choice, d below e: expand it, G ($h=0$) is generated and removed. Path S, a, e, d, G with cost $1+8+1+2=12$. This is the red path and the right branch of the search tree on the slide.
> Other choice, d below a: path S, a, d, G with cost $1+3+2=6$, which is the optimal path.
> So the result of greedy depends on the tie-breaking rule here. A* has no such tie: the two d nodes have $f=4+2=6$ and $f=10+2=12$.

> [!question]- Check (Ch.4, slide 11): New numbers: S→A 3, A→G 3, S→B 1, B→G 6, $h(S)=5$, $h(A)=3$, $h(B)=4$, $h(G)=0$. Start S, goal G, tree search. What does A* return if it stops when a goal is **enqueued**? What if it stops when a goal is **dequeued**?
> Expand S ($f=5$). Frontier: A $3+3=6$, B $1+4=5$.
> Expand B (5). It generates G with $g=7$, $f=7$.
> Stop on enqueue: return S, B, G with cost 7. This is wrong.
> Stop on dequeue: the frontier is A 6, G 7. Expand A (6): it generates G with $g=6$, $f=6$. Now G (6) is dequeued before G (7). Return S, A, G with cost 6, the optimal path.

> [!question]- Check (Ch.4, slide 12): On the slide $h(A)=6$ and A* returns the path of cost 5. Change only $h(A)$ to 3 and trace A* again (start S, goal G, tree search, stop when a goal is dequeued). Is $h(A)=3$ admissible?
> True cost from A to G: $h^{*}(A)=3$. So $h(A)=3\le 3$ is admissible. The slide's $h(A)=6>3$ is not.
> Expand S. Frontier: A $1+3=4$, G $5+0=5$.
> Expand A (4): it generates G with $g=1+3=4$, $f=4$.
> Dequeue G (4): return S, A, G with cost 4, the optimal path.
> The slide's $h(S)=7$ also overestimates ($h^{*}(S)=4$). It does not change the trace, because S is expanded first anyway.

> [!question]- Check (Ch.4, slide 22): Why is the UCS contour a circle around the start, and the A* contour a narrow shape stretched toward the goal?
> UCS expands every node with $g<C^{*}$. The cost $g$ grows equally in all directions, so the expanded region is a circle around the start.
> A* expands nodes with $g+h<C^{*}$. A node on the far side of the start has a small $g$ but a large $h$, so its $f$ is too large and it is not expanded. Nodes between start and goal keep a small $f$.
> "Hedges its bets": A* still expands some nodes that are not on the best path, because their $f$ is below $C^{*}$. That is the price of the optimality guarantee.

> [!question]- Check (Ch.4, slide 40): Check the heuristic on the slide ($h(S)=2$, $h(A)=4$, $h(B)=1$, $h(C)=1$, $h(G)=0$) for admissibility and for consistency, edge by edge. Edges: S→A 1, S→B 1, A→C 1, B→C 2, C→G 3.
> **Admissible.** $h^{*}(C)=3$, $h^{*}(A)=1+3=4$, $h^{*}(B)=2+3=5$, $h^{*}(S)=\min(1+4,\,1+5)=5$.
> $2\le 5$, $4\le 4$, $1\le 5$, $1\le 3$: admissible.
> **Consistent.**
> - S→A: $2\le 1+4=5$ yes.
> - S→B: $2\le 1+1=2$ yes.
> - A→C: $4\le 1+1=2$ **no**.
> - B→C: $1\le 2+1=3$ yes.
> - C→G: $1\le 3+0=3$ yes.
>
> Admissible but not consistent. That is why graph search fails here: C is closed with $g=3$ (via B, $f=4$) before A ($f=5$) is expanded. The better C ($g=2$) is then skipped, and the result is S, B, C, G with cost 6. The optimal cost is 5.

> [!question]- Check (Ch.4, slide 41): The slide repairs the heuristic with $h(A)=2$. Run A* **graph** search again on the graph of slide 40 (S→A 1, S→B 1, A→C 1, B→C 2, C→G 3, $h(S)=2$, $h(B)=1$, $h(C)=1$, $h(G)=0$). Start S, goal G, goal test on removal, a state is expanded at most once.
> Edge A→C is now consistent: $2\le 1+1$.
> 1. Expand S ($f=2$). Frontier: A $1+2=3$, B $1+1=2$.
> 2. Expand B (2). Frontier: A 3, C via B $3+1=4$.
> 3. Expand A (3). Frontier: C via A $2+1=3$, C via B 4.
> 4. Expand C via A (3). Frontier: C via B 4, G $5+0=5$.
> 5. Remove C via B (4): state C is already closed, skip it.
> 6. Remove G (5): goal.
>
> Result: S, A, C, G with cost 5, the optimal path. $f$ never decreased along a path, so the first time C was expanded it already had its best $g$.

## 12. Local search: iterative improvement (local-search slides 1-3)
![[cmpe421_lecture_04_local_search_p001.jpg]]
![[cmpe421_lecture_04_local_search_p002.jpg]]
![[cmpe421_lecture_04_local_search_p003.jpg]]

**Notes.** In many optimisation problems the path does not matter. The state is a complete configuration and we want one that satisfies the constraints or has the best value. **Iterative improvement** keeps a single current state and tries to improve it. It uses constant space.

$n$-queens: one queen per column, $h$ = number of attacking pairs, goal $h=0$. Move a queen within its column to reduce $h$.

> [!info] TSP by pairwise exchange (slide 3)
> A state is a complete tour. A neighbour is made by a **pairwise exchange**: remove two edges of the tour and reconnect the two pieces the other way. If the new tour is shorter, keep it. Two edges that cross can always be replaced by two shorter edges that do not cross. The slide says variants of this get within 1% of the optimal tour very quickly, even with thousands of cities.

> [!question]- Check (local-search, slide 1): Which of the two problems fits iterative improvement, and why: (a) place 8 queens with no attacks, (b) find the cheapest road route from Arad to Bucharest?
> (a) fits. Only the final board matters, not the order in which the queens were moved. Every board is a complete state that can be improved.
> (b) does not fit directly. The solution **is** the path, and a local search keeps only the current state, not the way it got there.
> The price of constant space: there is no frontier, so there is no guarantee of completeness or optimality.

> [!question]- Check (local-search, slide 2): An 8-queens state has the queens of columns 1 to 8 in rows 3, 5, 2, 5, 1, 4, 7, 7. Count the attacking pairs $h$.
> Two queens in columns $i<j$ attack each other if they are in the same row, or if the row difference equals the column difference $j-i$ (same diagonal).
> - Columns 2 and 4: rows 5 and 5, same row.
> - Columns 7 and 8: rows 7 and 7, same row.
> - Columns 3 and 8: rows 2 and 7, difference 5, and the columns differ by 5: same diagonal.
>
> No other pair attacks. $h=3$. There are $\binom{8}{2}=28$ pairs in total, so 25 pairs do not attack.

> [!question]- Check (local-search, slide 3): Four cities at A $(0,0)$, B $(4,0)$, C $(4,3)$, D $(0,3)$. The tour A, C, B, D, A crosses itself. Compute its length, do the pairwise exchange that removes the crossing, and compute the new length.
> Lengths: AB $=4$, BC $=3$, CD $=4$, DA $=3$, diagonals AC $=$ BD $=\sqrt{4^{2}+3^{2}}=5$.
> Old tour A, C, B, D, A: $5+3+5+3=16$.
> Exchange: remove the two crossing edges AC and BD, add AB and CD.
> New tour A, B, C, D, A: $4+3+4+3=14$.
> The move changes the cost by $(4+4)-(5+5)=-2$, so iterative improvement accepts it.

## 13. Hill climbing (local-search slides 5-6)
![[cmpe421_lecture_04_local_search_p005.jpg]]
![[cmpe421_lecture_04_local_search_p006.jpg]]

**Notes.** **Hill climbing** moves to the best neighbour and stops when no neighbour is **strictly** better. It gets stuck at:
- a **local maximum** (higher than its neighbours, lower than the global maximum),
- a **plateau**: a "flat" local maximum, or a **shoulder** from which progress is still possible.

**Random-restart hill climbing** repeats from random starting points. With finitely many local maxima, $P(\text{complete})\to1$ as the number of restarts grows. If one climb succeeds with probability $p$, the expected number of climbs is $1/p$.

8-queens in our run: plain hill climbing solves 14% of random starts, 95% with sideways moves.

> [!question]- Check (local-search, slide 5): Positions 0 to 9 have the values 1, 3, 4, 4, 2, 5, 8, 6, 7, 3. The neighbours of a position are the positions directly left and right of it. Run `Hill-Climbing` from the slide (maximise) from the starts 1, 4 and 9. Where does each run stop?
> The code stops when the best neighbour is **not strictly better** ($\le$).
> - Start 1 (value 3): the best neighbour is position 2 (value 4), move. At 2 the neighbours have 3 and 4. $4\le 4$, stop. Result: position 2, value 4. It is stuck on the flat part 4, 4.
> - Start 4 (value 2): go to 5 (value 5), then to 6 (value 8). The neighbours have 5 and 6, stop. Result: position 6, value 8, the global maximum.
> - Start 9 (value 3): go to 8 (value 7). The neighbours have 6 and 3, stop. Result: position 8, value 7, a local maximum.

> [!question]- Check (local-search, slide 6): (a) In the figure, where does hill climbing end from the marked "current state"? (b) One climb from a random start reaches the global maximum with probability $p=0.25$. What is the expected number of climbs with random restarts, and what is the probability that at least one of 5 climbs succeeds?
> (a) The current state is on the left slope of the hill labelled "local maximum". Hill climbing goes up this slope and stops at the local maximum. It never reaches the global maximum on the left, because it would have to go down first.
> (b) Expected number of climbs: $1/p=1/0.25=4$ (3 failures and 1 success on average).
> At least one success in 5 climbs: $1-(1-p)^{5}=1-0.75^{5}\approx 1-0.237=0.763$.

## 14. Simulated annealing (local-search slides 7-10)
![[cmpe421_lecture_04_local_search_p007.jpg]]
![[cmpe421_lecture_04_local_search_p008.jpg]]
![[cmpe421_lecture_04_local_search_p009.jpg]]
![[cmpe421_lecture_04_local_search_p010.jpg]]

**Notes.** **Simulated annealing** picks a *random* neighbour. $\Delta E$ is the change in value. An uphill move ($\Delta E>0$) is always taken. A downhill move is taken with probability $e^{\Delta E/T}$. The **temperature** $T$ decreases over time. High $T$ is a random walk. Low $T$ is hill climbing.

At fixed $T$ the state distribution approaches the Boltzmann distribution $p(x)=\alpha\,e^{E(x)/kT}$. If $T$ decreases slowly enough, the probability of reaching the best state approaches 1.

> [!warning] Source error (slide 8)
> The printed `value` function is `return -abs(0.5-state)`, one hill at 0.5. The text and the plot on slide 9 use two hills (near 0.3 and 0.8). The line `target = cmath.e ** (DeltaE/T):` has a stray colon.

> [!info] The Python example (slides 8-9)
> A state is a number $s$ in $[0,1]$. Every state is a neighbour of every other state, so `next` is just a new random number. The intended value function has two hills: a low one at $s=0.3$ (value 0) and a higher, narrow one at $s=0.8$ (value 0.025). The temperature starts at 10 and is multiplied by 0.9 in each iteration.
> The code has no separate uphill case. For $\Delta E>0$, `target` $=e^{\Delta E/T}>1$, which is larger than every random number in $[0,1)$, so the move is always taken.
> The plot on slide 9 shows $s$ over 100 iterations, one curve per run.

> [!question]- Check (local-search, slide 7): A random neighbour is worse by 2, so $\Delta E=-2$. Compute the acceptance probability at $T=10$, $T=1$ and $T=0.1$. What happens to a neighbour with $\Delta E=+2$?
> Probability $=e^{\Delta E/T}$.
> - $T=10$: $e^{-2/10}=e^{-0.2}\approx 0.819$.
> - $T=1$: $e^{-2}\approx 0.135$.
> - $T=0.1$: $e^{-20}\approx 2\times 10^{-9}$, practically never.
>
> With $\Delta E=+2$ the move is uphill and is always taken, at every temperature.
> High $T$: almost every move is accepted (random walk). Low $T$: only uphill moves are accepted (hill climbing).

> [!question]- Check (local-search, slide 8): The temperature is $T=10\times 0.9^{i}$. (a) Compute $T$ at $i=20$. (b) From which iteration is $T$ below 0.1? (c) With the intended function $\max\{-\lvert s-0.3\rvert,\ 0.025-\lvert s-0.8\rvert\}$, compute the value at $s=0.3$, $s=0.5$ and $s=0.8$.
> (a) $0.9^{20}\approx 0.1216$, so $T\approx 1.22$.
> (b) $10\times 0.9^{i}<0.1$ means $0.9^{i}<0.01$, so $i>\ln 0.01/\ln 0.9\approx 43.7$. From $i=44$ ($T\approx 0.097$).
> (c) $s=0.3$: $\max(0,\ 0.025-0.5)=0$. $s=0.5$: $\max(-0.2,\ 0.025-0.3)=-0.2$. $s=0.8$: $\max(-0.5,\ 0.025)=0.025$.
> So 0.8 is the global maximum (0.025) and 0.3 is a local maximum (0).

> [!question]- Check (local-search, slide 9): In the plot the curves jump wildly until about $i=40$ and then settle near 0.3 or 0.8. Explain both phases. Two curves sit near 0.3 and still jump to 0.8 after $i=80$, where $T\approx 0.002$. How is that possible?
> Early: $T$ is large compared with the values, which all lie between $-0.3$ (at $s=0$) and 0.025 (at $s=0.8$). At $i=0$ even the worst move, $\Delta E=-0.3-0.025=-0.325$, is accepted with probability $e^{-0.325/10}\approx 0.97$. Almost every random state is accepted.
> After about $i=44$, $T<0.1$: downhill moves are almost never accepted, so a run stays near a maximum.
> The late jump is an **uphill** move. All states are neighbours, and every $s$ with $\lvert s-0.8\rvert<0.025$ has a value above 0, the value at 0.3. Such a state is drawn with probability about 0.05 per iteration, and it is always accepted.

> [!question]- Check (local-search, slide 10): State $x^{*}$ has $E(x^{*})=3$ and state $x$ has $E(x)=2$. Compute $p(x^{*})/p(x)$ for $kT=1$ and for $kT=0.1$. What does this say about low temperatures?
> $p(x^{*})/p(x)=e^{(E(x^{*})-E(x))/kT}$. The constant $\alpha$ cancels.
> $kT=1$: $e^{1/1}=e\approx 2.72$.
> $kT=0.1$: $e^{1/0.1}=e^{10}\approx 22026$.
> At a low temperature the best state is far more likely than any other state. So if $T$ goes down slowly enough for the distribution to follow, the search ends in $x^{*}$ with probability near 1.

## 15. Local beam search (local-search slide 11)
![[cmpe421_lecture_04_local_search_p011.jpg]]

**Notes.** **Local beam search** keeps $k$ states. Each step generates all successors of all $k$ states and keeps the $k$ best of the whole pool. This is not $k$ independent searches: good states recruit the others. The risk is that all $k$ states end on the same hill. Stochastic beam search picks the $k$ successors at random, biased toward good ones.

> [!question]- Check (local-search, slide 11): $k=2$. The current states are P (value 5) and Q (value 9). The successors of P have the values 6 and 4. The successors of Q have the values 12 and 10. Which states does local beam search keep? What would two independent hill climbers keep?
> Local beam search puts all four successors in one pool: 6, 4, 12, 10. It keeps the 2 best: 12 and 10. Both come from Q, so the region of P is given up.
> Two independent hill climbers each keep their own best successor: 6 (from P) and 12 (from Q).
> This is the "recruiting" on the slide, and also the risk: all $k$ states can end on the same hill.

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

> [!info] The second GA example (slides 15-17)
> The slides reuse the two-hill function as fitness. A state is an integer from 0 to 9999, read as a string of digits (`top` = 10000, so 3000 stands for 0.3 and 8000 for 0.8). Each iteration computes the fitness of all 200 individuals, records the best one in `history`, builds the selection wheel (`probs`, `cutoff`), and creates 200 children: crossover of two parents at a random `split`, then with probability 0.001 a mutation of one digit. The plot on slide 17 shows the best individual of each iteration, one curve per run.

> [!question]- Check (local-search, slide 12): Two 8-queens parents are 32752411 and 24748552. Cut both after the 5th character. Write the two possible children. What does one character mean on the board?
> Front of the first + back of the second: 32752 + 552 = **32752552**.
> Front of the second + back of the first: 24748 + 411 = **24748411**.
> The $i$-th character is the row of the queen in column $i$. A child takes the queens of the left columns from one parent and the queens of the right columns from the other.

> [!question]- Check (local-search, slide 13): `Reproduce` returns `x[:c] + y[c:]` with `c` a random integer in `range(len(x))`. For `x = "1234"` and `y = "abcd"`, write the child for every possible `c`. Which child is a plain copy of a parent?
> `range(4)` gives $c=0,1,2,3$.
> - $c=0$: `"" + "abcd"` = `abcd`, a copy of `y`.
> - $c=1$: `1bcd`.
> - $c=2$: `12cd`.
> - $c=3$: `123d`.
>
> $c=4$ is never chosen, so a plain copy of `x` cannot appear. With $c=0$ no crossover happens.

> [!question]- Check (local-search, slide 14): 4-queens: a state is a string of 4 rows, fitness = number of non-attacking pairs (maximum 6). The population is 2424, 2143, 1343, 1234. (a) Compute the fitness values and the selection probabilities. (b) Cross $x=2424$ and $y=2143$ at $c=2$. (c) Mutate the third character of the child to 1. Give the fitness after each step.
> (a) Attacking pairs (same row, or row difference = column difference):
> - 2424: columns 1-3 and 2-4 (same rows). 2 pairs, fitness $6-2=4$.
> - 2143: columns 1-2, 3-4, 1-3 and 2-4 (diagonals). 4 pairs, fitness 2.
> - 1343: columns 2-4 (same row), 2-3 and 3-4 (diagonals). 3 pairs, fitness 3.
> - 1234: all 6 pairs are on one diagonal. Fitness 0.
>
> Sum $=4+2+3+0=9$. Probabilities: $4/9\approx 44\%$, $2/9\approx 22\%$, $3/9\approx 33\%$, $0\%$. The string 1234 is never chosen.
> (b) `x[:2] + y[2:]` = 24 + 43 = **2443**. Attacking pairs: columns 2-3 (same row), 1-3 and 3-4 (diagonals). Fitness $6-3=3$.
> (c) Third character 4 → 1: **2413**. No pair attacks, fitness 6. This is a solution.

> [!question]- Check (local-search, slide 15): Compute the fitness $\max\{-\lvert s-0.3\rvert,\ 0.025-\lvert s-0.8\rvert\}$ for $s=0.3$, $s=0.55$, $s=0.78$ and $s=1.0$. Which of these values can be used directly as selection weights?
> - $s=0.3$: $\max(0,\ 0.025-0.5)=0$.
> - $s=0.55$: $\max(-0.25,\ 0.025-0.25)=\max(-0.25,\,-0.225)=-0.225$.
> - $s=0.78$: $\max(-0.48,\ 0.025-0.02)=0.005$.
> - $s=1.0$: $\max(-0.7,\ 0.025-0.2)=-0.175$.
>
> Only values $\ge 0$ make sense as selection weights: here 0 and 0.005. The fitness is positive only for $\lvert s-0.8\rvert<0.025$ and zero at $s=0.3$. Everywhere else it is negative (see the warning above).

> [!question]- Check (local-search, slide 16): In the program a state is an integer with up to 4 digits. Take `par1 = 3012`, `par2 = 7985`, `split = 2`. What is the child? Then a mutation happens with `where = 0`, `what = "9"`. What is the child now? How many values can `split` take?
> `str(par1)[:2] + str(par2)[2:]` = `"30" + "85"` = `"3085"`, so the child is 3085.
> Mutation: `child[0:0] + "9" + child[1:]` = `"" + "9" + "085"` = `"9085"`, so the child is 9085.
> `split = int(random.random() * (stringsize+1))` gives 0, 1, 2, 3 or 4: five values. With 0 the child is a copy of `par2`, with 4 a copy of `par1`.

> [!question]- Check (local-search, slide 17): Read the plot: near which two values of $s$ do the best individuals sit, and which one is the global maximum? Then compare the work with simulated annealing: how many fitness evaluations does this GA run make, and how many value computations does the annealing example make in 100 iterations?
> The points sit near $s\approx 3000$ and $s\approx 8000$. These are 0.3 and 0.8 scaled by `top` = 10000. The global maximum is at 8000 (fitness 0.025). 3000 has fitness 0.
> GA: 100 iterations × 200 individuals = 20000 fitness evaluations.
> Annealing: one new state per iteration with `value(next)` and `value(state)`: $100\times 2=200$ computations.
> The GA does about 100 times more work. This matches "running time is much worse" on the slide.

> [!question]- Check (local-search, slide 18): True or false, with the reason from the slide: (a) "A genetic algorithm is a faithful model of biological evolution." (b) "Engineers use genetic algorithms widely, which proves that they perform better than other methods."
> (a) False. For example, real genes also encode the replication machinery itself. In a GA the crossover and mutation rules are fixed from outside.
> (b) False. They are widely used (circuit layout, job-shop scheduling), but the slide says it is not clear if this is because of performance or because the idea is attractive. More work is needed to know when they perform well.

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

> [!question]- Check (local-search, slide 19): (a) How many dimensions does the state space have for 4 airports? (b) Small case: one airport and three cities at $(0,0)$, $(4,0)$, $(2,6)$. Compute $f$ = sum of squared distances for the airport at $(1,1)$ and at $(2,2)$. Which place is better?
> (a) Each airport has two coordinates: $4\times 2=8$ dimensions.
> (b) Squared distance to a city at $(x_c,y_c)$: $(x-x_c)^{2}+(y-y_c)^{2}$.
> At $(1,1)$: $(1+1)+(9+1)+(1+25)=2+10+26=38$.
> At $(2,2)$: $(4+4)+(4+4)+(0+16)=8+8+16=32$.
> $(2,2)$ is better, because we minimise $f$.

> [!question]- Check (local-search, slide 20): Take $f(x,y)=x^{2}+y^{2}$ (a bowl like the one on the slide). Do one gradient step from $(1,1)$ with $\alpha=0.1$. Does $f$ go down?
> $\nabla f=(2x,\,2y)$. At $(1,1)$: $\nabla f=(2,2)$.
> $\mathbf x\leftarrow(1,1)-0.1\cdot(2,2)=(0.8,\,0.8)$.
> $f$ before: $1+1=2$. $f$ after: $0.64+0.64=1.28$. It goes down.
> The gradient points uphill, so we subtract it to minimise. To maximise we would add it.

> [!question]- Check (local-search, slide 21): One airport and three cities at $(0,0)$, $(4,0)$, $(2,6)$, with $f(x,y)=\sum_c\big[(x-x_c)^{2}+(y-y_c)^{2}\big]$. Solve $\nabla f=0$ exactly.
> $\partial f/\partial x=\sum_c 2(x-x_c)=2\,(3x-(0+4+2))=2\,(3x-6)$. It is 0 at $x=2$.
> $\partial f/\partial y=\sum_c 2(y-y_c)=2\,(3y-(0+0+6))=2\,(3y-6)$. It is 0 at $y=2$.
> The best place is $(2,2)$, the mean of the city coordinates, with $f=32$.
> With three airports it is not so simple: which city belongs to which airport changes when the airports move.

> [!question]- Check (local-search, slide 22): $f(x,y)=x^{2}+y^{2}$, current point $(1,1)$, we minimise. (a) Do one Newton-Raphson step. (b) Empirical gradient with $\delta=0.1$: compute $f$ at the four points $(1\pm\delta,\,1)$ and $(1,\,1\pm\delta)$. Which moves improve $f$?
> (a) $\nabla f=(2,2)$. $\mathbf H$ has $\partial^{2}f/\partial x^{2}=2$, $\partial^{2}f/\partial y^{2}=2$ and 0 for the mixed derivatives, so $\mathbf H^{-1}$ multiplies by $\tfrac12$.
> $\mathbf x\leftarrow(1,1)-\tfrac12(2,2)=(0,0)$. One step reaches the minimum, because $f$ is quadratic.
> (b) $f(1.1,1)=1.21+1=2.21$. $f(0.9,1)=0.81+1=1.81$. $f(1,1.1)=2.21$. $f(1,0.9)=1.81$.
> The current value is 2. The moves $-\delta$ in $x$ and $-\delta$ in $y$ improve $f$. No derivative was needed, only values of $f$.

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
