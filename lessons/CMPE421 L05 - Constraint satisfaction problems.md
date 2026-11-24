---
course: CMPE 421 Artificial Intelligence
lecture: 5
title: Constraint satisfaction problems
tags: [cmpe421, lecture]
slides: lectures/lecture_05/csp.pdf
full_notes: lecture_notes/lecture_05/CMPE421_Lecture_05_CSP.pdf
---
# CMPE 421 · Lecture 5 · Constraint satisfaction problems

> [!summary] In short
> - A CSP = variables, domains, constraints. A solution is a complete and consistent assignment. The path does not matter.
> - Backtracking = DFS that assigns one variable per level and skips values that break a constraint: $d^{n}$ leaves instead of $n!\,d^{n}$.
> - Filtering: forward checking, then arc consistency (AC-3, $O(n^{2}d^{3})$). Delete from the **tail** of an arc.
> - Ordering: MRV for the variable (degree breaks ties), LCV for the value.
> - Structure: independent subproblems, tree CSPs in $O(nd^{2})$, cutset conditioning.
> - Min-conflicts is hill climbing on the number of violated constraints: fast, but incomplete.

Our PDF notes follow the instructor's decks `Ch.5.pptx` (deck A) and `Ch.5-contd.pptx` (deck B). These two exist only as pptx, so there are no slide images of them. The slides embedded here are the handout `csp.pdf` (38 pages), which covers the same material. Slide numbers are PDF page numbers. The number printed on each slide is two higher. Topics that are only in decks A and B are in section 12, without slides.

## 1. What is a CSP (slides 1-4)
![[cmpe421_lecture_05_csp_p001.jpg]]
![[cmpe421_lecture_05_csp_p002.jpg]]
![[cmpe421_lecture_05_csp_p003.jpg]]
![[cmpe421_lecture_05_csp_p004.jpg]]

**Notes.** A **CSP** has variables $X_1,\dots,X_n$, a **domain** $D_i$ for each variable, and **constraints** that say which combinations of values are allowed. An assignment is **complete** if every variable has a value and **consistent** if it violates no constraint. A **solution** is complete and consistent.

Because the state has a known structure, general-purpose algorithms can do more than standard search: cross off values, choose a clever order, split the problem.

Map colouring of Australia: variables WA, NT, Q, NSW, V, SA, T. Domain {red, green, blue}. Adjacent regions must differ. There are $3^{7}=2187$ complete assignments and 18 solutions.

A **binary CSP** has constraints on at most two variables. Its **constraint graph** has one node per variable and one edge per constraint. Tasmania has no edge: it is an independent subproblem.

> [!question]- Check (slide 1): Find the mistake: "In a CSP the goal test is a black box, as in standard search, so a CSP solver needs a heuristic written for each problem."
> In standard search the state is any data structure and the goal test is a black box. In a CSP the state is a set of variable assignments and the goal test is a set of constraints that the algorithm can read.
> So the solver can see which variables and values cause a violation. This is why general-purpose methods (cross off values, choose a good order) work without problem-specific knowledge.

> [!question]- Check (slide 2): The slide lists 6 allowed pairs for $WA\neq NT$ with 3 colours. With 4 colours, how many pairs exist in total and how many are allowed?
> Total: $4\cdot4=16$ pairs. Forbidden pairs use the same colour twice: 4 pairs. Allowed: $16-4=12$.
> In general $d(d-1)$ pairs are allowed. For $d=3$: $3\cdot2=6$, as on the slide.

> [!question]- Check (slide 3): Take the slide's solution and change V to blue. Is it still a solution? Now change T to red instead (V stays red). Is that a solution?
> V = blue: no. V and SA are adjacent and SA = blue, so the constraint SA ≠ V is violated. The assignment is complete but not consistent.
> T = red: yes. T has no neighbour, so no constraint mentions T. Any colour works.

> [!question]- Check (slide 4): Read from the graph: how many binary constraints does the Australia CSP have, which variable is in the most constraints, and which variable is in none?
> 9 edges, so 9 constraints: WA-NT, WA-SA, NT-SA, NT-Q, SA-Q, SA-NSW, SA-V, Q-NSW, NSW-V.
> SA has 5 edges, the most. NT, Q, NSW have 3 each. WA and V have 2 each. T has 0 edges, so it can be solved alone.
> Check: $5+3+3+3+2+2+0=18=2\cdot9$. Every edge is counted at both ends.

## 2. Varieties of CSPs and constraints (slides 5-8)
![[cmpe421_lecture_05_csp_p005.jpg]]
![[cmpe421_lecture_05_csp_p006.jpg]]
![[cmpe421_lecture_05_csp_p007.jpg]]
![[cmpe421_lecture_05_csp_p008.jpg]]

**Notes.**
- **Discrete, finite domains**: $O(d^{n})$ complete assignments. Includes Boolean satisfiability (NP-complete).
- **Discrete, infinite domains** (integers): need a constraint language, for example $\mathit{StartJob}_1+5\le\mathit{StartJob}_3$. Linear constraints are solvable but NP-hard. Nonlinear constraints are undecidable.
- **Continuous variables**: linear constraints are solved by linear programming.

Constraints are **unary** (SA ≠ green, the same as a smaller domain), **binary** (SA ≠ WA), **higher-order** (3 or more variables, as in cryptarithmetic), or **soft** (preferences with a cost, which give constrained optimisation problems).

Cryptarithmetic TWO + TWO = FOUR: variables F, T, U, W, R, O and carries $X_1,X_2,X_3$. Constraints: alldiff(F, T, U, W, R, O), $O+O=R+10X_1$, and so on for each column.

> [!warning] Source issue (slide 7)
> The handout's solution $469+469=0938$ has $F=0$, a leading zero. With the usual rule $F\neq0$, one solution is $734+734=1468$.

> [!info] Reading the figure on slide 7
> The circles are variables and each **box is one constraint**. A box is joined to all variables of that constraint. This is needed because a higher-order constraint cannot be drawn as one edge.
> - The top box joins F, T, U, W, R, O: alldiff.
> - One box per column of the addition, from right to left. $X_1,X_2,X_3$ are the carries, each 0 or 1 (beyond the slides: the slide gives them the domain 0 to 9, but the largest column sum is $1+9+9=19$, so a carry is at most 1).
>
> $$O+O=R+10X_1,\qquad X_1+W+W=U+10X_2,$$
> $$X_2+T+T=O+10X_3,\qquad X_3=F.$$
>
> The last two are not written on the slide ("etc.").

> [!question]- Check (slide 5): Job 1 takes 5 days and must finish before job 3 starts: $\mathit{StartJob}_1+5\le\mathit{StartJob}_3$. If job 1 starts on day 3, what is the earliest start of job 3? Why can this constraint not be given as a list of allowed pairs?
> $3+5=8$, so job 3 starts on day 8 or later.
> The domain is all integers, so there are infinitely many allowed pairs $(3,8),(3,9),\dots$. A list is impossible. A formula (a constraint language) is needed. With a finite domain a list of allowed pairs is always possible, as on slide 2.

> [!question]- Check (slide 6): Add the unary constraint SA ≠ green to the Australia CSP. How do you handle it before search starts, and how many complete assignments and solutions are left?
> Delete green from the domain of SA. SA = {red, blue}. The constraint is then gone.
> Complete assignments: $2\cdot3^{6}=2\cdot729=1458$ instead of 2187.
> Solutions: by symmetry of the colours, each colour of SA has $18/3=6$ solutions. Two colours remain: $2\cdot6=12$.

> [!question]- Check (slide 7): Check the candidate $765+765=1530$ column by column: give $X_1,X_2,X_3$ and say whether every constraint holds.
> T = 7, W = 6, O = 5, F = 1, U = 3, R = 0.
> Units: $O+O=10=R+10X_1$ with $R=0$, $X_1=1$.
> Tens: $X_1+W+W=1+12=13=U+10X_2$ with $U=3$, $X_2=1$.
> Hundreds: $X_2+T+T=1+14=15=O+10X_3$ with $O=5$, $X_3=1$.
> Thousands: $F=X_3=1$.
> Alldiff: 1, 7, 3, 6, 0, 5 are six different digits. All constraints hold, and $F\neq0$.

> [!question]- Check (slide 8): Formulate as a CSP: classes C1, C2, C3 each need one slot from {9, 10, 11}. C1 and C2 have the same teacher. C2 and C3 share students. C1 cannot be at 9. Describe the constraint graph and count the complete assignments and the solutions.
> Variables: C1, C2, C3. Domains: {9, 10, 11}. Constraints: C1 ≠ C2 (binary), C2 ≠ C3 (binary), C1 ≠ 9 (unary).
> Constraint graph: three nodes in a chain C1 - C2 - C3. There is no edge C1 - C3. The unary constraint is not an edge. It only shrinks the domain of C1 to {10, 11}.
> Complete assignments: $3^{3}=27$.
> Solutions: C1 has 2 values, then C2 has 2 values (not the value of C1), then C3 has 2 values (not the value of C2). $2\cdot2\cdot2=8$.

## 3. Standard search formulation (slide 9)
![[cmpe421_lecture_05_csp_p009.jpg]]

**Notes.** States are partial assignments. The initial state is the empty assignment. A successor assigns a value to any unassigned variable. The goal test is "complete and consistent". Every solution is at depth $n$, so depth-first search fits.

If any variable may be assigned at any depth, the branching factor at depth $i$ is $b_i=(n-i)\,d$ and the tree has $n!\,d^{n}$ leaves. There are only $d^{n}$ complete assignments. The factor $n!$ is pure repetition.

> [!warning] Source error (slide 9)
> The slide writes "for $i=1,\dots,n$, $b_i=(n-i)d$, so $b_0b_1\dots b_n=n!\,d^{n}$". The indices do not match: $b_n=0$. Correct: $i$ runs from $0$ to $n-1$ and $b_0b_1\cdots b_{n-1}=n!\,d^{n}$.

> [!question]- Check (slide 9): Take $n=4$ variables with $d=2$ values each. Compute the branching factor at each depth of the naive tree and the number of leaves. How many complete assignments are there really?
> $b_i=(n-i)\,d$ for $i=0,1,2,3$: $b_0=4\cdot2=8$, $b_1=3\cdot2=6$, $b_2=2\cdot2=4$, $b_3=1\cdot2=2$.
> Leaves: $8\cdot6\cdot4\cdot2=384$. Check: $n!\,d^{n}=24\cdot16=384$.
> Complete assignments: $d^{n}=2^{4}=16$. Each one appears $4!=24$ times, once for every order in which the variables can be assigned.

## 4. Backtracking search (slides 10-15)
![[cmpe421_lecture_05_csp_p010.jpg]]
![[cmpe421_lecture_05_csp_p014.jpg]]
![[cmpe421_lecture_05_csp_p015.jpg]]

**Notes.** Assignments are **commutative**: WA = red then NT = green gives the same state as the other order. So assign **one variable per level**: $b=d$ and $d^{n}$ leaves. **Backtracking search** is this depth-first search with a consistency check before each recursive call. It is the basic uninformed algorithm for CSPs and solves $n$-queens for $n\approx25$.

```python
def backtrack(assignment, csp):
    if len(assignment) == len(csp.variables):
        return assignment
    var = select_unassigned_variable(assignment, csp)   # choice point 1
    for v in order_domain_values(var, assignment, csp): # choice point 2
        if csp.consistent(var, v, assignment):
            result = backtrack({**assignment, var: v}, csp)
            if result is not None:
                return result
    return None
```

Four ways to speed it up (slide 15): which variable next, which value first, detect failure early, use the problem structure.

> [!info] Reading the tree on slide 14
> Level 1 gives WA its 3 colours. Level 2 gives NT a colour under WA = red. It has only 2 children (green, blue), because NT = red violates WA ≠ NT and is never expanded. Level 3 gives Q a colour under NT = green: red and blue, because green violates NT ≠ Q.
> The tree is explored depth-first and left to right. A branch is left ("backtrack") when a variable has no consistent value. Then the most recent variable gets its next value.

> [!question]- Check (slide 10): Why may we fix one variable per level in a CSP? Then compare for Australia the number of leaves of the naive tree and of the one-variable-per-level tree.
> Assignments are commutative: the state is only the set of pairs variable = value. The order in which they were made changes neither the state nor its consistency. So trying the same assignments in another order can only repeat work.
> Naive tree: $n!\,d^{n}=7!\cdot3^{7}=5040\cdot2187=11\,022\,480$ leaves.
> One variable per level: $3^{7}=2187$ leaves. The factor is $7!=5040$.

> [!question]- Check (slide 14): Trace BACKTRACK on this CSP. Variables A, B, C, all pairs must differ (A ≠ B, B ≠ C, A ≠ C). Domains A = {r, g, b}, B = {r, g}, C = {r, g}. Variable order A, B, C. Value order r, g, b. Which solution is found, and how many values are tried?
> 1. A = r. B = r violates A ≠ B. B = g is fine. C = r violates A ≠ C. C = g violates B ≠ C. C fails. B has no value left, so B fails. Back to A.
> 2. A = g. B = r is fine. C = r violates B ≠ C. C = g violates A ≠ C. C fails. B = g violates A ≠ B. B fails. Back to A.
> 3. A = b. B = r is fine. C = r violates B ≠ C. C = g is fine. Complete.
>
> Solution: A = b, B = r, C = g.
> Values tried: 5 in step 1, 5 in step 2, 4 in step 3. Total 14. Of these, 7 were consistent and became nodes of the tree.
> There are $3\cdot2\cdot2=12$ complete assignments and 2 solutions (the other one is A = b, B = g, C = r). Backtracking stops at the first one.

> [!question]- Check (slide 15): Put each technique under one of the four bullet points of the slide: forward checking, MRV, cutset conditioning, least constraining value, arc consistency, degree heuristic.
> Which variable next: MRV, degree heuristic.
> Which value first: least constraining value.
> Detect failure early: forward checking, arc consistency.
> Use the problem structure: cutset conditioning (and also independent subproblems and tree CSPs).
> The first two only change the order of the search. The last two remove work.

## 5. Ordering: MRV, degree, LCV (slides 16-18)
![[cmpe421_lecture_05_csp_p016.jpg]]
![[cmpe421_lecture_05_csp_p017.jpg]]
![[cmpe421_lecture_05_csp_p018.jpg]]

**Notes.**
- **MRV (minimum remaining values)**: choose the variable with the **fewest** legal values left. Also called "most constrained variable" or fail-fast.
- **Degree heuristic**: tie-breaker for MRV. Choose the variable with the most constraints on the remaining unassigned variables.
- **LCV (least constraining value)**: for the chosen variable, try first the value that rules out the **fewest** values in the remaining variables.

Combined, these make 1000 queens feasible.

> [!example] After SA = red
> Forward checking leaves WA, NT, Q, NSW, V with {green, blue} and T with 3 values. MRV ties the five. Degree (unassigned neighbours): WA 1, NT 2, Q 2, NSW 2, V 1. Pick NT, Q or NSW.

> [!warning] Watch out
> Heuristics are not guarantees. In our measurements 16 queens needed 44 assignments with FC + MRV but 332 with FC + MRV + LCV.

> [!info] Reading the pictures on slides 16-18
> **Abstract trees (slide 16).** Variable $a$ has 2 values and $b$ has 4. Both orders give $2\cdot4=8$ leaves. With $a$ first there are 2 nodes below the root. With $b$ first there are 4. MRV puts the small branching factor at the top. If one value of $a$ is hopeless, the left tree drops it once. The right tree meets it again under every value of $b$.
>
> **Australia, slide 16.** WA = red, then NT = green, then SA = blue. After WA = red, NT and SA have 2 values left and all others 3. After NT = green, SA has only blue left, so MRV takes SA.
>
> **Abstract trees, slide 17.** $a$ and $c$ both have 2 values, so MRV ties. But $c$ has a constraint with $b$ and $a$ has none. With $c$ first, each value of $c$ deletes one value of $b$: $2\cdot3=6$ leaves instead of $2\cdot4=8$.
>
> **Australia, slide 18.** WA = red and NT = green are fixed, and Q gets a value. Q = red leaves blue for SA. Q = blue leaves nothing for SA. LCV tries red first.
>
> **Fail-first and fail-last (beyond the slides).** Every variable must get a value at some time. So a hard variable cannot be avoided, and it is cheaper to meet it early, high in the tree: fail first. For a value this is different. We need only **one** value that works. So we try the value that is most likely to lead to a solution: fail last. If we wanted all solutions, the value order would not matter.

> [!info] The first step on slide 17
> The Australia pictures on slide 17 start from WA = red, the same first step as on slide 16. The slide does not say that a heuristic chose WA, so read it as "WA = red is given". The pictures show the choices after it: SA is next (NT and SA tie with 2 values, SA has 4 unassigned neighbours and NT has 2), and then NT, which has only green left.
> If the heuristics also chose the first variable: every variable has 3 values, so MRV ties, and the degree heuristic would start with **SA** (5 constraints), not WA (2).

> [!question]- Check (slide 16): In the abstract example, value $a=1$ has no consistent value of $b$. In each of the two trees, how many dead leaves are there, and how many separate places must the search discover the problem?
> Both trees have 4 dead leaves, the leaves with $a=1$.
> Left tree ($a$ first): they all sit under the one node $a=1$. The search fails there once and never returns.
> Right tree ($b$ first): the leaf $a=1$ appears once under each of $b=1,2,3,4$. The same failure is found 4 times.
> This is the reason to assign the variable with the fewest values first.

> [!question]- Check (slide 17, trees): Read the number of leaves from the two abstract trees. Why does choosing $c$ before $a$ help, although both have 2 values?
> Left ($a$ first): $2\cdot4=8$ leaves. Right ($c$ first): $2\cdot3=6$ leaves.
> $c$ is in a constraint with $b$: $c=1$ deletes $b=4$ and $c=2$ deletes $b=3$. Assigning $c$ shrinks the domain of $b$ from 4 to 3 values. $a$ has no constraint, so it shrinks nothing.
> A variable with many constraints on unassigned variables reduces the branching factor of later levels.

> [!question]- Check (slide 17, Australia): WA = red and Q = green are assigned, with forward checking. Give the domains and say which variable MRV with the degree tie-breaker picks next.
> WA = red deletes red from NT and SA. Q = green deletes green from NT, SA and NSW.
> NT = {blue}, SA = {blue}, NSW = {red, blue}, V = {red, green, blue}, T = {red, green, blue}.
> MRV: NT and SA tie with 1 value.
> Degree, counting unassigned neighbours only: NT has 1 (SA). SA has 3 (NT, NSW, V).
> Pick SA.

> [!question]- Check (slide 18, computation): Only WA = red is assigned, with forward checking. The next variable is Q. For each colour of Q, count the values it rules out in the neighbours of Q. In which order does LCV try the colours?
> Domains before: NT = {green, blue}, SA = {green, blue}, NSW = {red, green, blue}. These are the neighbours of Q.
> Q = red: deletes red from NSW only. 1 value.
> Q = green: deletes green from NT, SA and NSW. 3 values.
> Q = blue: deletes blue from NT, SA and NSW. 3 values.
> LCV tries red first, then green and blue (a tie).

> [!question]- Check (slide 18, why): The slide asks "Why?". MRV picks the variable that is most likely to fail. LCV picks the value that is least likely to fail. Why is this not a contradiction?
> All variables must be assigned. A variable that causes a dead end will cause it anyway, so we want to see it early, before work is spent below it.
> Only one value per variable is needed. So for the value we choose the one that keeps the most options open and is most likely to be part of a solution.
> Variable order: fail first. Value order: fail last.

## 6. Forward checking (slides 19-23)
![[cmpe421_lecture_05_csp_p019.jpg]]
![[cmpe421_lecture_05_csp_p020.jpg]]
![[cmpe421_lecture_05_csp_p021.jpg]]
![[cmpe421_lecture_05_csp_p022.jpg]]
![[cmpe421_lecture_05_csp_p023.jpg]]

**Notes.** **Forward checking**: after each assignment $X=x$, delete from the unassigned neighbours of $X$ the values that conflict with $x$. Backtrack as soon as a domain is empty.

| After | NT | NSW | SA |
|---|---|---|---|
| WA = red | g, b | r, g, b | g, b |
| Q = green | b | r, b | b |
| V = blue | b | r | empty |

After V = blue, SA has no value left: backtrack.

Forward checking only compares an assigned variable with its unassigned neighbours. After Q = green, NT and SA are both {blue} and are neighbours, so the branch is already dead. Forward checking does not see it. **Constraint propagation** does.

> [!question]- Check (slides 19-21): Start again with full domains. Assign SA = blue, then NSW = red, with forward checking. Give the domain of every unassigned variable after each step. Is any domain empty?
> SA = blue: delete blue from its neighbours WA, NT, Q, NSW, V. Each is {red, green}. T = {red, green, blue}.
> NSW = red: delete red from its unassigned neighbours Q and V. Q = {green}, V = {green}. WA and NT are not neighbours of NSW and stay {red, green}.
> No domain is empty, so the search goes on.

> [!question]- Check (slide 22): On the slide, V = blue empties SA. Backtracking then tries V = red. Give the domains after forward checking. Does forward checking report a failure? Is there a solution below this node?
> Before V: NT = {blue}, SA = {blue}, NSW = {red, blue}.
> V = red deletes red from NSW. NSW = {blue}. SA has no red, so it stays {blue}. T is full.
> No domain is empty. Forward checking reports nothing.
> There is still no solution: NT and SA are neighbours and both have only blue. With WA = red and Q = green the CSP has 0 solutions, whatever V is.

> [!question]- Check (slide 23): In row 3 of the slide (after Q = green), forward checking has done its work. Which single arc check would show the failure, and why did forward checking not make it?
> The arc NT → SA (or SA → NT). NT = {blue} and SA = {blue}. The value blue in NT has no allowed partner in SA, so it is deleted and NT is empty.
> Forward checking only checks arcs that point to the variable that was just assigned (here Q). NT and SA are both unassigned, so the constraint between them is not looked at.

## 7. Arc consistency and AC-3 (slides 24-28)
![[cmpe421_lecture_05_csp_p024.jpg]]
![[cmpe421_lecture_05_csp_p025.jpg]]
![[cmpe421_lecture_05_csp_p026.jpg]]
![[cmpe421_lecture_05_csp_p027.jpg]]
![[cmpe421_lecture_05_csp_p028.jpg]]

**Notes.** Each constraint between $X$ and $Y$ gives two arcs, $X\to Y$ and $Y\to X$. The arc $X\to Y$ is **consistent** if for *every* value $x$ of the tail $X$ there is *some* allowed value $y$ in the head $Y$. To make it consistent, **delete from the tail**.

If $X$ loses a value, every arc $W\to X$ must be rechecked. **AC-3** keeps a queue of all arcs and re-adds the arcs into a variable that lost a value. Time $O(n^{2}d^{3})$, reducible to $O(n^{2}d^{2})$. It can run as a preprocessor or after each assignment.

Arc consistency finds every failure forward checking finds, plus more. It does not find all failures: that is NP-hard. Forward checking is the special case that enforces only the arcs pointing to the newly assigned variable.

> [!example] AC-3 on $X<Y$, $Y<Z$, domains {1, 2, 3}
> 1. $X\to Y$: delete $X=3$. $X=\{1,2\}$.
> 2. $Y\to X$: delete $Y=1$. $Y=\{2,3\}$.
> 3. $Y\to Z$: delete $Y=3$. $Y=\{2\}$. Add $X\to Y$ again.
> 4. $Z\to Y$: delete $Z=1,2$. $Z=\{3\}$.
> 5. $X\to Y$: delete $X=2$. $X=\{1\}$. Queue empty.
>
> Result $X=1$, $Y=2$, $Z=3$.

> [!warning] Watch out
> If AC empties a domain there is no solution. If AC empties no domain there may still be no solution. Example: a triangle of three mutually different variables with two colours is arc consistent and unsolvable. So arc consistency runs *inside* backtracking.

> [!info] The four arrows on slides 24-27
> The state is the one after WA = red and Q = green with forward checking: NT = {blue}, NSW = {red, blue}, V = {red, green, blue}, SA = {blue}.
> 1. Slide 24, SA → NSW: blue in SA has the partner red in NSW. Consistent, nothing is deleted.
> 2. Slide 25, NSW → SA: blue in NSW has no partner in SA = {blue}. Delete blue. NSW = {red}.
> 3. Slide 26, V → NSW: NSW lost a value, so the arcs into NSW are checked again. Red in V has no partner in NSW = {red}. Delete red. V = {green, blue}.
> 4. Slide 27, SA → NT: blue in SA has no partner in NT = {blue}. Delete blue. SA is empty: failure. This is found after 2 assignments (WA, Q). Forward checking needs a third one (V = blue, slide 22), so arc consistency is one assignment earlier.

> [!info] Where $O(n^{2}d^{3})$ comes from (beyond the slides)
> - A binary CSP with $n$ variables has at most $n^{2}$ arcs.
> - The arc $X_k\to X_i$ is put back in the queue only when $X_i$ loses a value. $X_i$ has $d$ values, so this happens at most $d$ times.
> - One call of REMOVE-INCONSISTENT-VALUES compares $d$ values of the tail with $d$ values of the head: $d^{2}$ checks.
>
> Total: $n^{2}\cdot d\cdot d^{2}=n^{2}d^{3}$. This is a worst case. For $n=4$, $d=3$ the bound is $16\cdot27=432$ value checks, and real runs need far fewer.

> [!question]- Check (slides 24-25): $X=\{1,2,3\}$, $Y=\{2,3\}$, constraint $X>Y$. Make $X\to Y$ consistent, then $Y\to X$. Which values are deleted, and from which variable?
> $X\to Y$: delete from the tail $X$. $x=1$: no $y<1$, delete. $x=2$: no $y<2$, delete. $x=3$: $y=2$ works. $X=\{3\}$.
> $Y\to X$: delete from the tail $Y$. $y=2$: $x=3$ works. $y=3$: needs $x>3$, none, delete. $Y=\{2\}$.
> Result $X=\{3\}$, $Y=\{2\}$.

> [!question]- Check (slides 26-27): On slide 25 NSW loses blue. Which arcs must be checked again because of this, and which of them deletes something? Why is the arc SA → NSW the least useful of them here?
> All arcs that point **into** NSW: Q → NSW, V → NSW, SA → NSW.
> V → NSW deletes red from V (slide 26), because NSW = {red} leaves no partner for red.
> Q → NSW: Q = green has the partner red. Nothing.
> SA → NSW: SA = {blue} has the partner red. Nothing. SA was the head of the arc that caused the deletion. Blue was deleted from NSW because it had no partner in SA. So it was not the partner of any value of SA, and SA cannot lose support. The slide's algorithm adds this arc anyway. It costs one check and changes nothing.

> [!question]- Check (slide 28): Run AC-3 by hand. Variables A, B, C, D with A ≠ B, B ≠ C, C ≠ D, B ≠ D. Domains A = {r}, B = {r, g}, C = {r, g}, D = {r, g, b}. Queue (first in, first out): A→B, B→A, B→C, C→B, C→D, D→C, B→D, D→B. When a variable loses a value, all arcs into it are added at the end of the queue, but an arc is added only if it is not already in the queue. List each removal. What would forward checking after the assignment A = r have missed?
> 1. A→B: r in A has the partner g. Nothing.
> 2. B→A: r in B has no partner in A = {r}. Delete. B = {g}. Add A→B (C→B and D→B are already in the queue).
> 3. B→C: g has the partner r. Nothing.
> 4. C→B: g in C has no partner in B = {g}. Delete. C = {r}. Add B→C (D→C is already in the queue).
> 5. C→D: nothing.
> 6. D→C: r in D has no partner in C = {r}. Delete. D = {g, b}. Add C→D (B→D is already in the queue).
> 7. B→D: nothing.
> 8. D→B: g in D has no partner in B = {g}. Delete. D = {b}. Add B→D (C→D is already in the queue).
> 9. A→B, B→C, C→D, B→D: nothing. The queue is empty after 12 arc checks.
>
> Result: A = {r}, B = {g}, C = {r}, D = {b}. Every domain has one value, and this is the only solution.
> Forward checking after A = r only deletes r from B, the neighbour of A. It stops with B = {g}, C = {r, g}, D = {r, g, b}. It misses the three deletions C = g, D = r, D = g, because they come from constraints between unassigned variables.

## 8. Independent subproblems (slides 29-31)
![[cmpe421_lecture_05_csp_p029.jpg]]
![[cmpe421_lecture_05_csp_p030.jpg]]
![[cmpe421_lecture_05_csp_p031.jpg]]

**Notes.** Independent subproblems are the **connected components** of the constraint graph. If $n$ variables split into subproblems of $c$ variables each, the worst case is

$$O\!\left(\frac{n}{c}\,d^{\,c}\right)\ \text{instead of}\ O(d^{\,n}),$$

which is linear in $n$. Example: $n=80$, $d=2$, $c=20$ at 10 million nodes per second. $2^{80}$ nodes take 4 billion years. $4\cdot2^{20}$ nodes take 0.4 seconds.

Australia: $3^{7}$ leaves as one problem. Mainland and Tasmania separately: at most $3^{6}+3$ leaves.

> [!question]- Check (slide 29): A CSP has 9 variables with 3 values each. Its constraint graph has three connected components of 3 variables. Compare the worst-case number of leaves with and without the split. Why are the three numbers added and not multiplied?
> As one problem: $3^{9}=19\,683$ leaves.
> Split: each component has at most $3^{3}=27$ leaves. $3\cdot27=81$ leaves. This is 243 times fewer.
> No constraint links two components. So any solution of one component can be combined with any solution of the others. Each component is searched once, alone. The work is the sum.
> For Australia the same rule gives $3^{6}+3=732$ instead of 2187, a factor of about 2.99 ("nearly 3 times").

> [!question]- Check (slide 30): The slide gets 12 leaves for three subproblems of 2 binary variables. What do you get for two subproblems of 3 variables? And what if one constraint between $b$ and $c$ is added to the slide's graph?
> Two subproblems of 3: $2\cdot2^{3}=16$ leaves. $64/16=4$ times as fast (the slide's split: $64/12\approx5.3$).
> Extra edge $b$-$c$: the components are $\{a,b,c,d\}$ and $\{e,f\}$. $2^{4}+2^{2}=16+4=20$ leaves. $64/20=3.2$ times as fast.
> Smaller components give a larger gain.

> [!question]- Check (slide 31): Repeat the slide's example with $n=60$, $d=2$, $c=15$ at 10 million nodes per second.
> As one problem: $2^{60}\approx1.15\times10^{18}$ nodes. Time $1.15\times10^{18}/10^{7}=1.15\times10^{11}$ s. One year is about $3.16\times10^{7}$ s, so this is about 3650 years.
> Split: $n/c=4$ subproblems. $4\cdot2^{15}=4\cdot32\,768=131\,072$ nodes. Time $131\,072/10^{7}\approx0.013$ s.

## 9. Tree-structured CSPs (slides 32-33)
![[cmpe421_lecture_05_csp_p032.jpg]]
![[cmpe421_lecture_05_csp_p033.jpg]]

**Notes.** If the constraint graph has no loops, the CSP is solved in $O(n\,d^{2})$ time. The algorithm:

1. **Order.** Choose a root and order the variables so that every node's parent comes before it.
2. **Remove backward.** For $j=n$ down to 2, make the arc $\text{Parent}(X_j)\to X_j$ consistent.
3. **Assign forward.** For $j=1$ to $n$, give $X_j$ a value consistent with its parent.

There are $n-1$ arcs and $d^{2}$ value pairs per arc. After the backward pass every value of a node has a consistent value in each child, so the forward pass never backtracks.

> [!warning] Watch out
> The backward pass must go from the leaves up. Going from $j=2$ upward, a later step can still delete values of $X_j$ and break an arc that was already checked.

> [!info] Reading the figure on slide 33
> The left graph is the tree. The right picture is the same tree after step 1 with root $X_1$: the order is $X_1,X_2,X_3,X_4,X_5,X_6$ and every arrow goes from a parent to its child. The arrows are $X_1\to X_2$, $X_2\to X_3$, $X_2\to X_4$, $X_4\to X_5$, $X_4\to X_6$. So the parent of $X_2$ is $X_1$, the parent of $X_3$ and $X_4$ is $X_2$, and the parent of $X_5$ and $X_6$ is $X_4$. Every variable except the root has exactly one parent, so there are $n-1=5$ arrows.
> "Like a topological sort" means: write the nodes in a line so that every arrow points to the right.

> [!question]- Check (slide 32): The graph on the slide has 6 nodes. How many edges does it have, and why is it a tree? Add the edge $X_1$-$X_3$. Does the theorem still apply?
> 5 edges: $X_1$-$X_2$, $X_3$-$X_2$, $X_2$-$X_4$, $X_4$-$X_5$, $X_4$-$X_6$. It is connected and has $n-1=5$ edges, so it has no loop.
> With $X_1$-$X_3$ there are 6 edges and the loop $X_1$-$X_2$-$X_3$. The theorem does not apply any more.
> Fixing one variable of the loop (for example $X_1$) makes the rest a tree again. This is the idea of the next section.

> [!question]- Check (slide 33): Run the tree algorithm. Root $X_1$, $X_2$ is the child of $X_1$, and $X_3$ and $X_4$ are children of $X_2$. Constraints $X_1>X_2$, $X_2<X_3$, $X_2\neq X_4$. Domains $\{1,2,3\}$ for $X_1,X_2,X_3$ and $\{2\}$ for $X_4$. In the forward pass take the smallest consistent value.
> Backward pass, delete from the parent:
> - $j=4$, arc $X_2\to X_4$: $X_2=2$ has no partner in $\{2\}$. $X_2=\{1,3\}$.
> - $j=3$, arc $X_2\to X_3$: $X_2=3$ needs $X_3>3$. Delete. $X_2=\{1\}$.
> - $j=2$, arc $X_1\to X_2$: $X_1=1$ needs $X_2<1$. Delete. $X_1=\{2,3\}$.
>
> Forward pass: $X_1=2$. $X_2=1$. $X_3=2$ (smallest value above 1). $X_4=2$.
> Check: $2>1$, $1<2$, $1\neq2$. No backtracking was needed.
> Cost: $n-1=3$ arcs with at most $d^{2}=9$ pair checks each, so at most 27 checks in the backward pass.

## 10. Nearly tree-structured CSPs (slide 34)
![[cmpe421_lecture_05_csp_p034.jpg]]

**Notes.** **Conditioning**: instantiate a variable in all possible ways and prune its neighbours' domains. **Cutset conditioning**: instantiate a set of variables (the **cutset**) so that the remaining graph is a tree, then solve each tree with the tree algorithm. With cutset size $c$:

$$O\!\left(d^{\,c}\,(n-c)\,d^{2}\right).$$

Australia: the cutset {SA} leaves a tree. For SA = red, remove red from WA, NT, Q, NSW, V and solve the rest as a tree.

> [!example] $n=100$, $d=3$, $c=4$
> $3^{4}=81$ tree problems, each about $96\cdot9=864$ pair checks. Total $81\cdot864=69\,984$.

> [!question]- Check (slide 34): Eight variables $X_1,\dots,X_8$ form a ring: each is constrained with the next one, and $X_8$ with $X_1$. $d=4$. What is the smallest cutset? Compute the bound of cutset conditioning and compare it with $d^{n}$.
> A ring has one loop. Removing any one variable leaves a chain of 7 variables, which is a tree. So $c=1$.
> $d^{c}\,(n-c)\,d^{2}=4^{1}\cdot7\cdot16=448$.
> $d^{n}=4^{8}=65\,536$. The bound is about 146 times smaller.
> Meaning of the factors: 4 values of the cutset variable, and for each one a tree CSP with 7 variables at cost $7\cdot4^{2}=112$.

## 11. Iterative improvement: min-conflicts (slides 35-37)
![[cmpe421_lecture_05_csp_p035.jpg]]
![[cmpe421_lecture_05_csp_p036.jpg]]
![[cmpe421_lecture_05_csp_p037.jpg]]

**Notes.** Local search works with **complete** states and allows unsatisfied constraints. Operators reassign one variable.

**Min-conflicts**: pick a conflicted variable **at random**, give it the value that violates the fewest constraints. This is hill climbing with $h(x)$ = total number of violated constraints. $h=0$ is a solution.

From a random start, min-conflicts solves $n$-queens in almost constant time for any $n$ (for example $n=10\,000\,000$). The same seems true for random CSPs except in a narrow range of the ratio $R=\dfrac{\text{number of constraints}}{\text{number of variables}}$.

> [!warning] Watch out
> Min-conflicts is incomplete. It can get stuck on a plateau or in a local minimum, and it can never prove that a CSP has no solution.

> [!info] The boards on slides 35-36 and the curve on slide 37
> **Boards.** One variable per column, its value is the row of the queen (row 1 at the top). The left board is the state $(3,2,3,2)$. A line between two queens is one violated constraint (same row or same diagonal). The slide moves the queen of column 2 to row 1, state $(3,1,3,2)$ with $h=2$, and then the queen of column 3 to row 4, state $(3,1,4,2)$ with $h=0$.
>
> **Curve (beyond the slides).** $R$ is small: few constraints, many solutions, a solution is found quickly. $R$ is large: so many constraints that almost no solution exists. Near the **critical ratio** a problem has very few solutions that are hard to find, and the CPU time has a sharp peak. For Australia $R=9/7\approx1.29$.

> [!question]- Check (slide 35): The left board is the state $(3,2,3,2)$ (row of the queen in columns 1 to 4, row 1 at the top). List the attacking pairs. Then count the conflicts of the queen in column 2 for each of its 4 possible rows.
> Attacking pairs (columns): 1-2 diagonal, 2-3 diagonal, 3-4 diagonal, 1-3 same row, 2-4 same row. So $h=5$, the five lines on the board.
> Queen of column 2, the others stay at rows 3, 3, 2 in columns 1, 3, 4:
> - row 1: attacks nobody. 0 conflicts.
> - row 2: diagonal with columns 1 and 3, same row as column 4. 3 conflicts.
> - row 3: same row as columns 1 and 3. 2 conflicts.
> - row 4: diagonal with columns 1, 3 and 4. 3 conflicts.
>
> Min-conflicts moves it to row 1, as on the slide.

> [!question]- Check (slide 36): The middle board is $(3,1,3,2)$ with $h=2$. Which queens are conflicted? The slide moves column 3. What happens if the random choice is column 1 instead? And column 4?
> Violated constraints: columns 1-3 (same row) and columns 3-4 (diagonal). The conflicted queens are in columns 1, 3 and 4.
> Column 3 (slide): conflicts per row 1 to 4 are 3, 2, 2, 0. Move to row 4: $(3,1,4,2)$, $h=0$.
> Column 1: conflicts per row are 2, 2, 1, 0. Move to row 4: $(4,1,3,2)$. Only columns 3-4 still attack, so $h=1$. Not solved yet.
> Column 4: conflicts per row are 1, 1, 3, 1. Rows 1, 2 and 4 tie at the minimum 1, and the queen is in row 2 now. Whichever of them is taken, the queen keeps 1 conflict, so $h$ stays 2 (a plateau step).
> The result of one step depends on the random choice of the variable.

> [!question]- Check (slide 37): Read the curve: for which problems is min-conflicts slow? A random CSP has $R$ far above the critical ratio and min-conflicts runs for a long time without success. What can you conclude?
> It is slow only in a narrow range of $R$ around the critical ratio. On both sides of it the CPU time is low.
> Far above the critical ratio the problem very probably has no solution. But min-conflicts cannot prove this. It is incomplete: it only stops when it finds a solution or reaches its step limit.
> So the conclusion is only "no solution found". A proof needs a complete method such as backtracking.

## 12. Only in the instructor's decks (no slide images)

**Notes.** These topics are in `Ch.5.pptx` and `Ch.5-contd.pptx` and in our PDF notes.

- **Planning or identification.** In planning the path matters. In a CSP only the final assignment matters (identification).
- **$N$-queens, two formulations.** One 0/1 variable per square gives $2^{64}$ assignments for 8 queens. One variable per queen with its position as the value gives $8^{8}$.
- **$k$-consistency.** 1-consistency is node consistency. 2-consistency is arc consistency. $k$-consistency: any consistent assignment to $k-1$ variables can be extended to a $k$-th. **Strong $k$-consistency** means also $(k-1)$, ..., 1-consistent. A strongly $n$-consistent CSP is solved without backtracking, but enforcing it costs as much as solving the CSP.
- **After arc consistency** a CSP can have one solution, several, or none without knowing it.
- **Tree decomposition.** Group variables into overlapping mega-variables that form a tree and must agree on shared variables.
- **Local search recap.** Hill climbing, simulated annealing and genetic algorithms, see [[CMPE421 L04 - Informed and local search]].

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| CSP | variables, domains, constraints. Solution = complete consistent assignment |
| Constraint graph | nodes = variables, edges = binary constraints |
| Sizes | $d^{n}$ complete assignments. Naive tree: $n!\,d^{n}$ leaves |
| Backtracking | DFS, one variable per level, check constraints as you go |
| Forward checking | after $X=x$, delete conflicting values from unassigned neighbours of $X$ |
| Arc $X\to Y$ consistent | every $x\in D_X$ has an allowed $y\in D_Y$. Delete from the tail |
| AC-3 | queue of arcs, recheck arcs into a variable that lost a value. $O(n^{2}d^{3})$ |
| $k$-consistency | any consistent $(k-1)$-assignment extends to a $k$-th variable |
| MRV / degree / LCV | fewest values left / most constraints on unassigned / rules out fewest values |
| Subproblems | $O\!\left(\frac{n}{c}d^{\,c}\right)$ |
| Tree CSP | order, remove backward, assign forward. $O(nd^{2})$ |
| Cutset conditioning | $O\!\left(d^{\,c}(n-c)d^{2}\right)$ |
| Min-conflicts | random conflicted variable, value with fewest conflicts |

## Check yourself
> [!question]- 1. How many complete assignments and how many solutions does the Australia CSP have?
> $3^{7}=2187$ assignments. SA: 3 choices. Its five neighbours form a path and use the 2 other colours alternately: 2 ways. T: 3 choices. $3\cdot2\cdot3=18$ solutions.

> [!question]- 2. For 8-queens with one variable per column ($n=8$, $d=8$), compute $d^{n}$ and $n!\,d^{n}$.
> $8^{8}=16\,777\,216$. $8!=40\,320$. Product: $676\,457\,349\,120\approx6.8\times10^{11}$.

> [!question]- 3. NT = {green, blue}, SA = {blue}. Is NT → SA consistent? Is SA → NT?
> NT → SA: blue in NT has no partner in SA. Not consistent, delete blue from NT. SA → NT: blue has the partner green. Consistent.

> [!question]- 4. Classify: (a) $X_1+W+W=U+10X_2$, (b) NSW ≠ V, (c) "Prof. A prefers mornings", (d) Q ≠ blue.
> (a) higher-order (four variables), (b) binary, (c) soft, (d) unary.

> [!question]- 5. A chain A-B-C-D with $d=10$: compare $d^{n}$ with $n\,d^{2}$.
> $10^{4}=10\,000$ against $4\cdot100=400$. The tree algorithm is 25 times cheaper.

> [!question]- 6. Why does min-conflicts choose the conflicted variable at random?
> Always taking the first conflicted variable can cycle: the same variable is changed back and forth. A random choice lets the search leave such loops and plateaus.

## Links
- Course index: [[CMPE421 Home]]
- Previous: [[CMPE421 L04 - Informed and local search]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_05_CSP.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_05/CMPE421_Lecture_05_CSP.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_05/CMPE421_Lecture_05_CSP.pdf`
- Handout slides (PDF): [csp.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/csp.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/csp.pdf`
- Instructor's deck A (pptx): [Ch.5.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/Ch.5.pptx) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/Ch.5.pptx`
- Instructor's deck B (pptx): [Ch.5-contd.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/Ch.5-contd.pptx) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/Ch.5-contd.pptx`

Slides not embedded (csp.pdf): 11-13 (earlier builds of slide 14), 38 (summary, same as the box at the top).
