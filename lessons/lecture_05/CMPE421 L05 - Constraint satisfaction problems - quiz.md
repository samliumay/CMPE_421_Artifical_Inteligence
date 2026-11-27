---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L05 - Constraint satisfaction problems
type: quiz
questions: 32
---

# CMPE421 L05 - Constraint satisfaction problems - quiz

Lesson note: [[CMPE421 L05 - Constraint satisfaction problems]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. What is a CSP (slides 1-4)

### Q1 (slide 1)

A student writes: "In a CSP the goal test is a black box, as in standard search, so a CSP solver needs a heuristic written for each problem." Which statement corrects the mistake?

- A) The goal test is still a black box, but the state is a set of assignments, so general methods work.
- B) The goal test is a set of constraints the solver can read, so general methods work for every CSP.
- C) The goal test is a set of readable constraints, but each problem still needs its own heuristic.
- D) A CSP has no goal test, because every complete assignment of the variables is already a solution.

> [!success]- Answer
> **B.** In a CSP the state is a set of variable assignments and the goal test is a set of constraints the algorithm can read. So the solver sees which variables and values cause a violation. This is why general-purpose methods (cross off values, choose a good order) work without problem-specific knowledge.
> - A: The goal test is exactly what is not a black box in a CSP. A black-box test would hide which constraint fails.
> - C: Because the constraints are readable, general heuristics (MRV, LCV, forward checking) work for every CSP.
> - D: A solution must be complete and consistent. A complete assignment can violate constraints.

### Q2 (slide 2)

The slide lists 6 allowed pairs (WA, NT) for $WA\neq NT$ with 3 colours, for example (red, green) and (green, red). With 4 colours, how many pairs (WA, NT) are allowed?

- A) 16
- B) 6
- C) 4
- D) 12

> [!success]- Answer
> **D.** There are $4\cdot4=16$ pairs in total. The forbidden pairs use the same colour twice: 4 pairs. Allowed: $16-4=12$. In general $d(d-1)$; for $d=3$ this gives the slide's 6.
> - A: 16 is the number of all pairs. The 4 equal-colour pairs were not removed.
> - B: 6 counts unordered pairs, $\binom{4}{2}$. The slide counts (red, green) and (green, red) as two pairs.
> - C: 4 is the number of forbidden pairs, not allowed ones.

### Q3 (slide 3)

![[cmpe421_lecture_05_csp_p003.jpg]]

Australia constraints (adjacent regions differ): WA-NT, WA-SA, NT-SA, NT-Q, SA-Q, SA-NSW, SA-V, Q-NSW, NSW-V; T has none. Start from the slide's solution: WA = red, NT = green, Q = red, NSW = green, V = red, SA = blue, T = green. First change only V to blue. Then, instead, change only T to red (V stays red). Which result is right?

- A) V = blue is not a solution. T = red is a solution.
- B) V = blue is a solution. T = red is a solution.
- C) V = blue is not a solution. T = red is not a solution.
- D) V = blue is a solution. T = red is not a solution.

> [!success]- Answer
> **A.** V and SA are adjacent and SA = blue, so V = blue violates SA ≠ V. The assignment is complete but not consistent. T has no neighbour, so no constraint mentions T and any colour works.
> - B: V = blue breaks the constraint SA ≠ V.
> - C: T = red is fine. T is in no constraint, so it cannot violate one.
> - D: Both parts are swapped: V = blue is the change that breaks a constraint.

### Q4 (slide 4)

![[cmpe421_lecture_05_csp_p004.jpg]]

Read the constraint graph. How many binary constraints does the Australia CSP have, and in how many of them is SA?

- A) 18 constraints. SA is in 10 of them.
- B) 9 constraints. SA is in 10 of them.
- C) 9 constraints. SA is in 5 of them.
- D) 18 constraints. SA is in 5 of them.

> [!success]- Answer
> **C.** One edge is one constraint. The graph has 9 edges: WA-NT, WA-SA, NT-SA, NT-Q, SA-Q, SA-NSW, SA-V, Q-NSW, NSW-V. SA has 5 edges, the most. T has none.
> Check: degrees $5+3+3+3+2+2+0=18=2\cdot9$.
> - A: This counts arcs (two per edge) in both parts: 18 arcs, and SA is in 10 arcs.
> - B: The total is right, but SA's 5 edges were counted as arcs, in both directions.
> - D: 18 is the sum of the degrees: every edge was counted at both ends.

## 2. Varieties of CSPs and constraints (slides 5-8)

### Q5 (slide 5)

Job 1 takes 5 days and must finish before job 3 starts: $\mathit{StartJob}_1+5\le\mathit{StartJob}_3$. The start days are integers. Job 1 starts on day 3. What is the earliest start of job 3, and why can this constraint not be given as a list of allowed pairs?

- A) Day 9. The domain is all integers, so the list of allowed pairs would be infinite.
- B) Day 8. A list of allowed pairs can only describe constraints of the form $X\neq Y$.
- C) Day 8. The domain is all integers, so the list of allowed pairs would be infinite.
- D) Day 9. A list of allowed pairs can only describe constraints of the form $X\neq Y$.

> [!success]- Answer
> **C.** $3+5=8\le\mathit{StartJob}_3$, so day 8 is allowed. The allowed pairs $(3,8),(3,9),\dots$ never end, so a list is impossible and a formula is needed.
> - A: Day 9 adds one extra day. The constraint is $\le$, so equality (day 8) is allowed.
> - B: A list works for any constraint on finite domains, not only for $\neq$. The problem is the infinite domain.
> - D: Both parts are wrong: the off-by-one day and the false claim about lists.

### Q6 (slide 6)

Add the unary constraint SA ≠ green to the Australia CSP (18 solutions, $3^{7}=2187$ complete assignments). Before search you delete green from the domain of SA. How many complete assignments and how many solutions are left?

- A) 1458 assignments, 12 solutions.
- B) 2187 assignments, 6 solutions.
- C) 1458 assignments, 6 solutions.
- D) 2187 assignments, 12 solutions.

> [!success]- Answer
> **A.** SA = {red, blue}, so there are $2\cdot3^{6}=1458$ complete assignments. By symmetry each colour of SA has $18/3=6$ solutions. Two colours remain: $2\cdot6=12$.
> - B: 2187 ignores the smaller domain of SA, and 6 is the count for one colour of SA only.
> - C: The assignment count is right, but 6 keeps only one colour of SA. SA can still be red or blue.
> - D: The solution count is right, but 2187 still counts SA with 3 values.

### Q7 (slide 7)

![[cmpe421_lecture_05_csp_p007.jpg]]

TWO + TWO = FOUR with constraints alldiff(F, T, U, W, R, O), $O+O=R+10X_1$, $X_1+W+W=U+10X_2$, $X_2+T+T=O+10X_3$, $X_3=F$, and $F\neq0$. The carries $X_1,X_2,X_3$ are not in the alldiff. Check the candidate $765+765=1530$. Which statement is right?

- A) The units column fails: $O+O=10$ would need $R=10$, which is not a digit.
- B) The tens column fails: $W+W=12$ gives $U=2$, not 3.
- C) Alldiff fails, because $F$ and $X_3$ both have the value 1.
- D) Every constraint holds, with the carries $X_1=X_2=X_3=1$.

> [!success]- Answer
> **D.** T = 7, W = 6, O = 5, F = 1, U = 3, R = 0.
> Units: $5+5=10=0+10\cdot1$, so $X_1=1$. Tens: $1+6+6=13=3+10\cdot1$, so $X_2=1$. Hundreds: $1+7+7=15=5+10\cdot1$, so $X_3=1$. Thousands: $F=X_3=1$.
> The digits 1, 7, 3, 6, 0, 5 are all different, and $F\neq0$.
> - A: This forgets the carry: $10=R+10X_1$ with $R=0$, $X_1=1$.
> - B: This forgets the carry $X_1=1$ from the units column: $1+6+6=13$ gives $U=3$.
> - C: The carries are not letters, so they are not in the alldiff. $F=X_3$ is even a constraint.

### Q8 (slide 8)

Classes C1, C2, C3 each need one slot from {9, 10, 11}. C1 and C2 have the same teacher. C2 and C3 share students. C1 cannot be at 9. What does the constraint graph look like, and how many solutions are there?

- A) 3 edges: C1-C2, C2-C3 and a loop at C1 for C1 ≠ 9. 8 solutions.
- B) 2 edges: C1-C2 and C2-C3, a chain. 8 solutions.
- C) 2 edges: C1-C2 and C2-C3, a chain. 12 solutions.
- D) 3 edges: C1-C2, C2-C3 and a loop at C1 for C1 ≠ 9. 12 solutions.

> [!success]- Answer
> **B.** The binary constraints C1 ≠ C2 and C2 ≠ C3 give a chain C1 - C2 - C3 (no edge C1 - C3). The unary C1 ≠ 9 is not an edge: it shrinks C1 to {10, 11}.
> Solutions: C1 has 2 values, C2 has 2 (not C1's value), C3 has 2 (not C2's value): $2\cdot2\cdot2=8$.
> - A: A unary constraint is not drawn as an edge. It only shrinks a domain.
> - C: 12 forgets C1 ≠ 9: $3\cdot2\cdot2=12$.
> - D: Draws the unary constraint as a loop, then forgets to use it in the count.

## 3. Standard search formulation (slide 9)

### Q9 (slide 9)

$n=4$ variables, $d=2$ values each. In the naive tree any unassigned variable may be assigned at any depth, so the branching factor at depth $i$ is $b_i=(n-i)\,d$. How many leaves does the naive tree have?

- A) 384
- B) 4096
- C) 16
- D) 24

> [!success]- Answer
> **A.** $i=0,1,2,3$: $b_0=8$, $b_1=6$, $b_2=4$, $b_3=2$. Leaves $8\cdot6\cdot4\cdot2=384=n!\,d^{n}=24\cdot16$. There are only $d^{n}=16$ complete assignments, each repeated $4!=24$ times.
> - B: $8^{4}=4096$ keeps the branching factor $nd=8$ at every depth. A variable that is assigned cannot be chosen again.
> - C: 16 is the number of complete assignments ($d^{n}$), the leaves of the one-variable-per-level tree.
> - D: 24 is only $n!$, the factor of repetition. The $d^{n}$ part is missing.

## 4. Backtracking search (slides 10-15)

### Q10 (slide 10)

Why may backtracking fix one variable per level of the tree, and how many leaves does that tree have for the Australia CSP (7 variables, 3 colours)?

- A) Every solution is at depth $n$, so the order does not matter. 5040 leaves.
- B) Assignments are commutative: the order does not change the state. 5040 leaves.
- C) Assignments are commutative: the order does not change the state. 2187 leaves.
- D) Every solution is at depth $n$, so the order does not matter. 2187 leaves.

> [!success]- Answer
> **C.** The state is only the set of pairs variable = value, so the order in which they were made changes nothing. One variable per level gives $3^{7}=2187$ leaves instead of $7!\cdot3^{7}=11\,022\,480$.
> - A: 5040 is $7!$, the factor saved, not the number of leaves. The reason is also wrong (see D).
> - B: The reason is right, but 5040 is $7!$, not $d^{n}$.
> - D: The count is right, but "solutions are at depth $n$" is true for every order. It does not say why the order may be fixed.

### Q11 (slide 14)

Plain backtracking, no inference (a value is only checked against the variables already assigned). Variables A, B, C, every pair must differ (A ≠ B, B ≠ C, A ≠ C). Domains A = {r, g, b}, B = {r, g}, C = {r, g}. Fixed variable order A, B, C. Value order r, g, b. A "value tried" is every value that is checked, consistent or not. Which solution is found, and how many values are tried?

- A) A = b, B = g, C = r, after 14 values tried.
- B) A = b, B = r, C = g, after 14 values tried.
- C) A = b, B = r, C = g, after 7 values tried.
- D) A = b, B = g, C = r, after 7 values tried.

> [!success]- Answer
> **B.** A = r: B = r fails, B = g ok, C = r fails, C = g fails, back (5 tried). A = g: B = r ok, C = r fails, C = g fails, B = g fails, back (5 tried). A = b: B = r ok, C = r fails, C = g ok (4 tried). Total $5+5+4=14$.
> - A: A = b, B = g, C = r is the other solution. It is found first only if values are tried in the order b, g, r.
> - C: 7 counts only the consistent values (the nodes of the tree). The question counts every value checked.
> - D: The wrong solution (value order reversed) and only the consistent values counted.

### Q12 (slide 15)

The slide lists four ways to speed up backtracking: which variable next, which value first, detect failure early, use the problem structure. Which two of these techniques belong to "detect failure early"?

- A) Forward checking and MRV.
- B) Arc consistency and cutset conditioning.
- C) MRV and cutset conditioning.
- D) Forward checking and arc consistency.

> [!success]- Answer
> **D.** Forward checking and arc consistency delete values and stop as soon as a domain is empty, before the search goes deeper.
> - A: MRV is "fail first", but it only chooses the next variable. It deletes nothing.
> - B: Cutset conditioning belongs to "use the problem structure".
> - C: MRV is "which variable next" and cutset conditioning is "problem structure".

## 5. Ordering: MRV, degree, LCV (slides 16-18)

### Q13 (slide 16)

![[cmpe421_lecture_05_csp_p016.jpg]]

In the abstract example, suppose $a=1$ has no consistent value of $b$. Look at the two trees ($a$ first on the left, $b$ first on the right). In how many separate places must the search discover this failure in the left tree and in the right tree?

- A) Left 4, right 1.
- B) Left 1, right 1.
- C) Left 4, right 4.
- D) Left 1, right 4.

> [!success]- Answer
> **D.** Both trees have 4 dead leaves with $a=1$. On the left they sit under the one node $a=1$: the search fails there once and never returns. On the right, $a=1$ appears under each of $b=1,2,3,4$: the failure is found 4 times. This is why MRV assigns the variable with fewer values first.
> - A: This swaps the trees.
> - B: On the right the node $a=1$ is a separate node under every value of $b$, so the failure is met 4 times.
> - C: This counts dead leaves (4 in each tree), not places where the failure is discovered.

### Q14 (slide 17, trees)

![[cmpe421_lecture_05_csp_p017.jpg]]

In the abstract example $a$ and $c$ both have 2 values, but the tree with $c$ first has fewer leaves. Why?

- A) $c$ is in a constraint with $b$, so each value of $c$ removes one value of $b$.
- B) $c$ has fewer remaining values than $a$, so MRV puts it at the top of the tree.
- C) $c$ is in more constraints with assigned variables, so it is the most constrained.
- D) $a$ is in a constraint with $b$, so assigning $a$ first adds an extra level.

> [!success]- Answer
> **A.** $c=1$ deletes $b=4$ and $c=2$ deletes $b=3$. So $b$ has 3 values instead of 4 under each value of $c$. $a$ has no constraint and shrinks nothing. This is the degree heuristic: many constraints on unassigned variables.
> - B: Both have 2 values, so MRV ties. The degree heuristic breaks the tie.
> - C: The degree heuristic counts constraints with unassigned variables, not assigned ones.
> - D: $a$ has no constraint with $b$. Both trees have two levels.

### Q15 (slide 17, Australia)

Australia constraints (adjacent regions differ): WA-NT, WA-SA, NT-SA, NT-Q, SA-Q, SA-NSW, SA-V, Q-NSW, NSW-V; T has none. WA = red and Q = green are assigned, with forward checking. The next variable is chosen by MRV; ties are broken by the degree heuristic (number of constraints with unassigned variables), and remaining ties alphabetically. Which option gives the chosen variable with the correct reason and counts?

- A) NT: MRV ties NT and SA at 1 value, and NT comes first in alphabetical order.
- B) NT: MRV ties NT and SA at 1 value, both have 2 assigned neighbours, and NT comes first alphabetically.
- C) SA: MRV ties NT and SA at 1 value, and SA has 3 unassigned neighbours, NT has 1.
- D) SA: MRV ties NT and SA at 1 value, and SA has 5 neighbours, NT has 3.

> [!success]- Answer
> **C.** Forward checking: NT = {blue}, SA = {blue}, NSW = {red, blue}, V and T full. MRV ties NT and SA. Unassigned neighbours: NT has SA (1). SA has NT, NSW, V (3). Pick SA.
> - A: The alphabetical rule is only used after the degree tie-break, and the degree tie-break is not tied here.
> - B: This counts constraints with assigned variables (WA and Q for both). The degree heuristic counts constraints with unassigned variables.
> - D: The choice happens to be the same, but the counts are over all neighbours. WA and Q are assigned and do not count.

### Q16 (slide 18, computation)

Only WA = red is assigned, with forward checking. The next variable is Q. Its unassigned neighbours are NT, SA, NSW. For each colour of Q, count the values it rules out in the current domains of NT, SA and NSW. In which order does LCV try the colours (ties in the order red, green, blue)?

- A) Red rules out 0, green 1, blue 1. Order: red, green, blue.
- B) Red rules out 1, green 3, blue 3. Order: red, green, blue.
- C) Red rules out 1, green 3, blue 3. Order: green, blue, red.
- D) Red rules out 0, green 1, blue 1. Order: green, blue, red.

> [!success]- Answer
> **B.** After WA = red: NT = {green, blue}, SA = {green, blue}, NSW = {red, green, blue}. Q = red deletes red from NSW only (1). Q = green deletes green from NT, SA, NSW (3). Q = blue deletes blue from all three (3). LCV takes the fewest first: red, then green and blue.
> - A: The order is right, but these counts look only at SA, as in the slide's picture. NT and NSW also lose values.
> - C: The counts are right, but the order is reversed: LCV tries the value that rules out the fewest values first.
> - D: Counts only SA, and the order is reversed.

### Q17 (slide 18, why)

MRV picks the variable that is most likely to fail. LCV picks the value that is least likely to fail. Why is this not a contradiction?

- A) Every variable needs a value, so a hard one is met anyway; but only one good value is needed.
- B) MRV and LCV are never used in the same search, so their aims cannot conflict.
- C) LCV is only used when all solutions are wanted, and then failing late is cheaper.
- D) Both pick what fails first: the value that rules out the fewest values also fails soonest.

> [!success]- Answer
> **A.** All variables must be assigned. A variable that causes a dead end will cause it anyway, so we meet it early (fail first). For a value we need only one that works, so we try the one most likely to lead to a solution (fail last).
> - B: The slide combines them: MRV, degree and LCV together make 1000 queens feasible.
> - C: If all solutions are wanted, every value must be tried, so the value order does not matter.
> - D: The least constraining value keeps the most options open. It is the least likely to fail.

## 6. Forward checking (slides 19-23)

### Q18 (slides 19-21)

Australia constraints (adjacent regions differ): WA-NT, WA-SA, NT-SA, NT-Q, SA-Q, SA-NSW, SA-V, Q-NSW, NSW-V; T has none. Start with full domains {red, green, blue}. Assign SA = blue, then NSW = red, with forward checking (after each assignment delete the conflicting value from the unassigned neighbours). What are the domains after the two steps?

- A) Q = V = {green}. WA = NT = {green}.
- B) Q = V = {red, green}. WA = NT = {red, green}.
- C) Q = V = {red, green}. WA = NT = {green}.
- D) Q = V = {green}. WA = NT = {red, green}.

> [!success]- Answer
> **D.** SA = blue deletes blue from WA, NT, Q, NSW, V: each is {red, green}. NSW = red deletes red from its unassigned neighbours Q and V: Q = V = {green}. WA and NT are not neighbours of NSW and stay {red, green}. T is full. No domain is empty.
> - A: Q and V are right, but red was also deleted from WA and NT, which are not neighbours of NSW.
> - B: This stops after SA = blue: the forward check after NSW = red is missing.
> - C: Red was deleted from the wrong variables: from WA and NT instead of from Q and V.

### Q19 (slide 22)

![[cmpe421_lecture_05_csp_p022.jpg]]

After WA = red and Q = green, forward checking gives NT = {blue}, SA = {blue}, NSW = {red, blue}. On the slide V = blue empties SA. Backtracking now tries V = red, with forward checking (the neighbours of V are SA and NSW; NT and SA are neighbours). What is the domain of NSW after this step, and is there a solution below this node?

- A) NSW = {blue}. Yes, a solution exists, because no domain is empty.
- B) NSW = {red, blue}. No, there is no solution below this node.
- C) NSW = {blue}. No, there is no solution below this node.
- D) NSW = {red, blue}. Yes, a solution exists, because no domain is empty.

> [!success]- Answer
> **C.** V = red deletes red from its unassigned neighbours NSW and SA. NSW = {blue}. SA has no red and stays {blue}. No domain is empty, so forward checking reports nothing. But NT and SA are neighbours and both have only blue: there is no solution, whatever V is.
> - A: "No empty domain" does not mean "a solution exists". Forward checking misses the NT-SA conflict.
> - B: NSW is a neighbour of V, so V = red deletes red from NSW.
> - D: Red was not deleted from NSW, and an empty-free state was taken as proof of a solution.

### Q20 (slide 23)

Slide 23, row 3: the state after WA = red and Q = green with forward checking: NT = {blue}, Q = {green}, NSW = {red, blue}, SA = {blue}. Which single arc check shows that this branch is dead?

- A) NSW → SA: blue in NSW has no partner in SA = {blue}, so NSW becomes empty.
- B) NT → SA: blue in NT has no partner in SA = {blue}, so NT becomes empty.
- C) SA → NSW: blue in SA has no partner in NSW, so SA becomes empty.
- D) Q → NT: green in Q has no partner in NT = {blue}, so Q becomes empty.

> [!success]- Answer
> **B.** NT and SA are neighbours and both are {blue}. Blue in NT has no allowed partner in SA, so it is deleted and NT is empty. Forward checking missed it because it only checks arcs into the variable just assigned (Q), and NT and SA are both unassigned.
> - A: Blue is deleted from NSW, but red stays. NSW = {red} is not empty.
> - C: Blue in SA has the partner red in NSW. Nothing is deleted.
> - D: Green in Q has the partner blue in NT. The arc is consistent.

## 7. Arc consistency and AC-3 (slides 24-28)

### Q21 (slides 24-25)

$X=\{1,2,3\}$, $Y=\{2,3\}$, constraint $X>Y$. To make an arc consistent, delete values from its tail. Make $X\to Y$ consistent, then $Y\to X$. Which values are deleted, and from which variable?

- A) $X\to Y$ deletes 3 from $Y$. Then $Y\to X$ deletes 1 and 2 from $X$.
- B) $X\to Y$ deletes 3 from $X$. Then $Y\to X$ deletes nothing.
- C) $X\to Y$ deletes 1 and 2 from $X$. Then $Y\to X$ deletes 3 from $Y$.
- D) $X\to Y$ deletes nothing. Then $Y\to X$ deletes 3 from $X$.

> [!success]- Answer
> **C.** $X\to Y$, tail $X$: $x=1$ and $x=2$ have no $y<x$ in $\{2,3\}$, delete. $x=3$ has $y=2$. $X=\{3\}$. $Y\to X$, tail $Y$: $y=2$ has $x=3$. $y=3$ needs $x>3$, delete. $Y=\{2\}$.
> - A: Each arc deletes from its head instead of its tail. The final domains are the same here, but each step is wrong.
> - B: This reads the constraint as $X<Y$: $x=3$ has no larger $y$, and then every $y$ has a smaller $x$.
> - D: Constraint read as $X<Y$ and each arc revised from the head.

### Q22 (slides 26-27)

![[cmpe421_lecture_05_csp_p025.jpg]]

The state is NT = {blue}, Q = {green}, V = {red, green, blue}, SA = {blue}. On slide 25 NSW loses blue, so NSW = {red}. The neighbours of NSW are Q, SA and V. Which arcs must AC-3 check again because of this, and what do they delete?

- A) Q → NSW, V → NSW, SA → NSW. Only V → NSW deletes: red from V.
- B) NSW → Q, NSW → V, NSW → SA. Only one deletion: red from V.
- C) Q → NSW, V → NSW, SA → NSW. None of them deletes anything.
- D) NSW → Q, NSW → V, NSW → SA. None of them deletes anything.

> [!success]- Answer
> **A.** When NSW loses a value, the arcs that point into NSW may lose support: Q → NSW, V → NSW, SA → NSW. Red in V has no partner in NSW = {red}, so V = {green, blue}. Q = green and SA = blue both have the partner red: nothing.
> - B: The arcs out of NSW delete from NSW, and NSW's change cannot remove support for NSW itself. These arcs cannot delete red from V.
> - C: Right arcs, but checked against the old NSW = {red, blue}. With NSW = {red}, red in V has no partner.
> - D: The arcs out of NSW are the wrong arcs. They do delete nothing, but they are not the ones to recheck.

### Q23 (slide 28)

AC-3 on A ≠ B, B ≠ C, C ≠ D, B ≠ D with A = {r}, B = {r, g}, C = {r, g}, D = {r, g, b}. Queue (first in, first out): A→B, B→A, B→C, C→B, C→D, D→C, B→D, D→B. When a variable loses a value, all arcs into it are added at the end, but only if they are not already in the queue. An arc check is one arc taken from the queue, whether or not it deletes a value. How many arc checks does the run make, and which deletions would forward checking after A = r alone have missed?

- A) 8 arc checks. It misses C = g, D = r and D = g.
- B) 12 arc checks. It misses C = g, D = r and D = g.
- C) 12 arc checks. It misses B = r, C = g, D = r and D = g.
- D) 8 arc checks. It misses B = r, C = g, D = r and D = g.

> [!success]- Answer
> **B.** B→A deletes r from B (add A→B). C→B deletes g from C (add B→C). D→C deletes r from D (add C→D). D→B deletes g from D (add B→D). The 8 first arcs plus the 4 re-added arcs, which delete nothing: 12 checks. Result A = {r}, B = {g}, C = {r}, D = {b}.
> Forward checking after A = r deletes r from B (a neighbour of A) and stops.
> - A: 8 forgets to re-queue arcs after a deletion. The deletions are right.
> - C: Forward checking does delete r from B, because B is a neighbour of A.
> - D: Forgot to re-queue arcs, and B = r is counted as missed.

## 8. Independent subproblems (slides 29-31)

### Q24 (slide 29)

A CSP has 9 variables with 3 values each. Its constraint graph has three connected components of 3 variables. What is the worst-case number of leaves when you use the split, and why?

- A) 19 683, because every solution of one part must be combined with every solution of the others.
- B) 27, because the three components look the same, so one search covers all three of them.
- C) 19 683, because the split only changes the order of the search, not its worst case.
- D) 81, because no constraint links two components, so each one is searched once, alone.

> [!success]- Answer
> **D.** Each component has at most $3^{3}=27$ leaves. The three searches are separate, so the work is added: $3\cdot27=81$. As one problem: $3^{9}=19\,683$, 243 times more.
> - A: Combining solutions needs no search. The leaves are added, not multiplied ($27^{3}=19\,683$).
> - B: The components have the same size but different variables and constraints. Each must be searched.
> - C: The split removes the combinations across components, so the worst case drops.

### Q25 (slide 30)

Six binary variables $a,\dots,f$ (2 values each). Worst case as one problem: $2^{6}=64$ leaves. (1) How many leaves for two independent subproblems of 3 variables? (2) The slide's graph has the edges $a$-$b$, $c$-$d$, $e$-$f$. Add one edge $b$-$c$. How many leaves now?

- A) (1) 16 leaves. (2) 20 leaves.
- B) (1) 64 leaves. (2) 20 leaves.
- C) (1) 16 leaves. (2) 64 leaves.
- D) (1) 64 leaves. (2) 64 leaves.

> [!success]- Answer
> **A.** (1) $2^{3}+2^{3}=16$, 4 times fewer than 64. (2) The components are $\{a,b,c,d\}$ and $\{e,f\}$: $2^{4}+2^{2}=16+4=20$.
> - B: Part (1) multiplies $8\cdot8=64$ instead of adding.
> - C: Part (2) multiplies $16\cdot4=64$ instead of adding.
> - D: Both parts multiply the component sizes, which gives back the unsplit 64.

### Q26 (slide 31)

Repeat the slide's example with $n=60$ binary variables ($d=2$), subproblems of $c=15$ variables, and 10 million nodes per second. One year is about $3.16\times10^{7}$ s. How long does the search take as one problem, and how long with the split?

- A) About 1.3 million years. With the split about 0.013 s.
- B) About 3650 years. With the split about 0.013 s.
- C) About 3650 years. With the split about 0.0033 s.
- D) About 1.3 million years. With the split about 0.0033 s.

> [!success]- Answer
> **B.** One problem: $2^{60}\approx1.15\times10^{18}$ nodes, $1.15\times10^{18}/10^{7}=1.15\times10^{11}$ s, $/3.16\times10^{7}\approx3650$ years. Split: $60/15=4$ subproblems, $4\cdot2^{15}=131\,072$ nodes, $131\,072/10^{7}\approx0.013$ s.
> - A: 1.3 million is the time in days ($1.15\times10^{11}/86\,400$), not in years.
> - C: 0.0033 s counts only one subproblem, $2^{15}$ nodes. There are 4.
> - D: Days taken as years, and only one subproblem counted.

## 9. Tree-structured CSPs (slides 32-33)

### Q27 (slide 32)

![[cmpe421_lecture_05_csp_p032.jpg]]

How many edges does the graph on the slide have? Add the edge $X_1$-$X_3$. Does the $O(nd^{2})$ theorem still apply?

- A) 6 edges. Yes, it still applies, because the graph is still connected.
- B) 5 edges. Yes, it still applies, because the graph is still connected.
- C) 6 edges. No, the new edge closes a loop, so the graph is no tree.
- D) 5 edges. No, the new edge closes a loop, so the graph is no tree.

> [!success]- Answer
> **D.** Edges $X_1$-$X_2$, $X_3$-$X_2$, $X_2$-$X_4$, $X_4$-$X_5$, $X_4$-$X_6$: 5. Connected with $n-1$ edges, so a tree. With $X_1$-$X_3$ there is the loop $X_1$-$X_2$-$X_3$, and the theorem needs a graph with no loops.
> - A: 6 is the number of nodes, and connected is not enough: a tree must have no loop.
> - B: The count is right, but connectivity is not the condition. The condition is "no loops".
> - C: The verdict is right, but 6 counts nodes, not edges.

### Q28 (slide 33)

Tree algorithm. Root $X_1$; $X_2$ is the child of $X_1$; $X_3$ and $X_4$ are children of $X_2$. Constraints $X_1>X_2$, $X_2<X_3$, $X_2\neq X_4$. Domains $\{1,2,3\}$ for $X_1,X_2,X_3$ and $\{2\}$ for $X_4$. Order $X_1,X_2,X_3,X_4$. Backward pass: for $j=4$ down to 2, make $\text{Parent}(X_j)\to X_j$ consistent (delete from the parent). What are the domains of $X_1$ and $X_2$ after the backward pass?

- A) $X_1=\{1,2,3\}$, $X_2=\{1,2\}$.
- B) $X_1=\{2,3\}$, $X_2=\{1,2\}$.
- C) $X_1=\{2,3\}$, $X_2=\{1\}$.
- D) $X_1=\{1,2,3\}$, $X_2=\{1\}$.

> [!success]- Answer
> **C.** $j=4$: $X_2=2$ has no partner $\neq2$ in $\{2\}$, so $X_2=\{1,3\}$. $j=3$: $X_2=3$ needs $X_3>3$, delete, $X_2=\{1\}$. $j=2$: $X_1=1$ needs $X_2<1$, delete, $X_1=\{2,3\}$. Forward pass (smallest value): $X_1=2$, $X_2=1$, $X_3=2$, $X_4=2$, no backtracking.
> - A: This deletes from the child instead of the parent: $X_2$ loses 3 (no $X_1>3$) and $X_3$ loses 1. Then the forward pass sets $X_1=1$ and gets stuck at $X_2$.
> - B: The step $j=4$ (arc $X_2\to X_4$) is missing, so $X_2=2$ survives.
> - D: The loop stops at $j=3$: the arc $X_1\to X_2$ is never made consistent.

## 10. Nearly tree-structured CSPs (slide 34)

### Q29 (slide 34)

Eight variables $X_1,\dots,X_8$ form a ring: each is constrained with the next one, and $X_8$ with $X_1$. $d=4$. Cutset conditioning costs $d^{\,c}\,(n-c)\,d^{2}$. What is the smallest cutset size $c$, and what is the bound?

- A) $c=2$, bound 1536.
- B) $c=1$, bound 448.
- C) $c=1$, bound 512.
- D) $c=2$, bound 2048.

> [!success]- Answer
> **B.** A ring has one loop. Removing one variable leaves a chain of 7, a tree. $4^{1}\cdot7\cdot4^{2}=448$, against $4^{8}=65\,536$ (about 146 times smaller).
> - A: $c=2$ also works, but it is not the smallest: $4^{2}\cdot6\cdot16=1536$.
> - C: $4\cdot8\cdot16=512$ uses $n$ instead of $n-c$ for the size of the tree.
> - D: $c=2$ and $n$ instead of $n-c$: $16\cdot8\cdot16=2048$.

## 11. Iterative improvement: min-conflicts (slides 35-37)

### Q30 (slide 35)

![[cmpe421_lecture_05_csp_p035.jpg]]

The left board is the state $(3,2,3,2)$: the row of the queen in columns 1 to 4, row 1 at the top. Two queens conflict if they share a row or a diagonal. Count the conflicts of the queen in column 2 for each of its rows 1, 2, 3, 4 (the other queens stay).

- A) 0, 3, 2, 3
- B) 0, 1, 2, 0
- C) 2, 5, 4, 5
- D) 0, 2, 0, 3

> [!success]- Answer
> **A.** The others are at rows 3, 3, 2 in columns 1, 3, 4. Row 1: nobody, 0. Row 2: diagonal with columns 1 and 3, same row as column 4, so 3. Row 3: same row as columns 1 and 3, so 2. Row 4: diagonal with columns 1, 3, 4, so 3. Min-conflicts moves it to row 1.
> - B: Only rows are counted, the diagonals are forgotten.
> - C: This is $h$ of the whole board after each move. It includes the pairs 1-3 and 3-4 that do not involve column 2.
> - D: Only diagonals are counted, the same-row conflicts are forgotten.

### Q31 (slide 36)

![[cmpe421_lecture_05_csp_p036.jpg]]

The middle board is $(3,1,3,2)$ with $h=2$ (row of the queen in columns 1 to 4, row 1 at the top). The conflicted queens are in columns 1, 3, 4. The slide moves column 3. What is $h$ after one min-conflicts step if the random choice is column 1 instead? And if it is column 4?

- A) Column 1: $h=0$. Column 4: $h=2$.
- B) Column 1: $h=1$. Column 4: $h=1$.
- C) Column 1: $h=0$. Column 4: $h=1$.
- D) Column 1: $h=1$. Column 4: $h=2$.

> [!success]- Answer
> **D.** Column 1, conflicts per row 1 to 4: 2, 2, 1, 0. Move to row 4: $(4,1,3,2)$. Columns 3-4 still attack: $h=1$. Column 4, conflicts per row: 1, 1, 3, 1. Every best row keeps 1 conflict, so $h$ stays 2 (a plateau step).
> - A: For column 1 this gives the conflicts of the moved queen (0), not $h$ of the whole board. The 3-4 diagonal remains.
> - B: For column 4 this gives the conflicts of the moved queen (1), not $h$. The pair 1-3 (same row) remains too.
> - C: Both values are the conflicts of the moved queen only (0 and 1), not $h$ of the whole board.

### Q32 (slide 37)

![[cmpe421_lecture_05_csp_p037.jpg]]

A random CSP has $R$ (constraints per variable) far above the critical ratio. Min-conflicts runs for a long time without success. What can you conclude?

- A) Min-conflicts is stuck on a plateau, and a restart will show whether a solution exists.
- B) The CSP has no solution, because min-conflicts tries every state before it stops.
- C) The CSP very probably has no solution, but min-conflicts cannot prove it.
- D) The CSP has a solution that is hard to find, because it sits at the peak of the curve.

> [!success]- Answer
> **C.** Far above the critical ratio there are so many constraints that almost no solution exists. But min-conflicts is incomplete: it stops only at a solution or at its step limit. The only conclusion is "no solution found". A proof needs a complete method such as backtracking.
> - A: A restart is still local search. It can find a solution but never prove that none exists.
> - B: Min-conflicts does not try every state. It is incomplete.
> - D: The hard problems are near the critical ratio, not far above it.
