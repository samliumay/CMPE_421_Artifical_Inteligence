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

## 3. Standard search formulation (slide 9)
![[cmpe421_lecture_05_csp_p009.jpg]]

**Notes.** States are partial assignments. The initial state is the empty assignment. A successor assigns a value to any unassigned variable. The goal test is "complete and consistent". Every solution is at depth $n$, so depth-first search fits.

If any variable may be assigned at any depth, the branching factor at depth $i$ is $b_i=(n-i)\,d$ and the tree has $n!\,d^{n}$ leaves. There are only $d^{n}$ complete assignments. The factor $n!$ is pure repetition.

> [!warning] Source error (slide 9)
> The slide writes "for $i=1,\dots,n$, $b_i=(n-i)d$, so $b_0b_1\dots b_n=n!\,d^{n}$". The indices do not match: $b_n=0$. Correct: $i$ runs from $0$ to $n-1$ and $b_0b_1\cdots b_{n-1}=n!\,d^{n}$.

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

## 8. Independent subproblems (slides 29-31)
![[cmpe421_lecture_05_csp_p029.jpg]]
![[cmpe421_lecture_05_csp_p030.jpg]]
![[cmpe421_lecture_05_csp_p031.jpg]]

**Notes.** Independent subproblems are the **connected components** of the constraint graph. If $n$ variables split into subproblems of $c$ variables each, the worst case is

$$O\!\left(\frac{n}{c}\,d^{\,c}\right)\ \text{instead of}\ O(d^{\,n}),$$

which is linear in $n$. Example: $n=80$, $d=2$, $c=20$ at 10 million nodes per second. $2^{80}$ nodes take 4 billion years. $4\cdot2^{20}$ nodes take 0.4 seconds.

Australia: $3^{7}$ leaves as one problem. Mainland and Tasmania separately: at most $3^{6}+3$ leaves.

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

## 10. Nearly tree-structured CSPs (slide 34)
![[cmpe421_lecture_05_csp_p034.jpg]]

**Notes.** **Conditioning**: instantiate a variable in all possible ways and prune its neighbours' domains. **Cutset conditioning**: instantiate a set of variables (the **cutset**) so that the remaining graph is a tree, then solve each tree with the tree algorithm. With cutset size $c$:

$$O\!\left(d^{\,c}\,(n-c)\,d^{2}\right).$$

Australia: the cutset {SA} leaves a tree. For SA = red, remove red from WA, NT, Q, NSW, V and solve the rest as a tree.

> [!example] $n=100$, $d=3$, $c=4$
> $3^{4}=81$ tree problems, each about $96\cdot9=864$ pair checks. Total $81\cdot864=69\,984$.

## 11. Iterative improvement: min-conflicts (slides 35-37)
![[cmpe421_lecture_05_csp_p035.jpg]]
![[cmpe421_lecture_05_csp_p036.jpg]]
![[cmpe421_lecture_05_csp_p037.jpg]]

**Notes.** Local search works with **complete** states and allows unsatisfied constraints. Operators reassign one variable.

**Min-conflicts**: pick a conflicted variable **at random**, give it the value that violates the fewest constraints. This is hill climbing with $h(x)$ = total number of violated constraints. $h=0$ is a solution.

From a random start, min-conflicts solves $n$-queens in almost constant time for any $n$ (for example $n=10\,000\,000$). The same seems true for random CSPs except in a narrow range of the ratio $R=\dfrac{\text{number of constraints}}{\text{number of variables}}$.

> [!warning] Watch out
> Min-conflicts is incomplete. It can get stuck on a plateau or in a local minimum, and it can never prove that a CSP has no solution.

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
