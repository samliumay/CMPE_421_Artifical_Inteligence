---
course: CMPE 421 Artificial Intelligence
lecture: 9
title: Predicate logic and planning
tags: [cmpe421, lecture]
slides: lectures/lecture_07/PredicateLogic.pdf
full_notes: lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf
---
# CMPE 421 · Lecture 9 · Predicate logic and deterministic planning

> [!summary] In short
> - First-order logic (FOL) talks about objects, relations and functions, not only facts. Sentences use terms, predicates, connectives, $=$, $\forall$ and $\exists$.
> - Truth is defined in a world $M=(D,I)$: a domain plus an interpretation.
> - $\forall$ goes with $\Rightarrow$, $\exists$ goes with $\land$. Quantifier order matters when the two differ.
> - A classical planning state is a set of ground atoms. An operator has preconditions and effects. Result: $\gamma(s,a)=(s-\text{effects}^-(a))\cup\text{effects}^+(a)$.
> - Graphplan builds a planning graph (a relaxed problem) with mutex links, then extracts a plan backwards.
> - FastForward uses planning-graph heuristics in a greedy forward search.

Three decks: `PredicateLogic.pdf` (part A), `DeterministicPlanning.pdf` (part B), and the extra deck `Inference-PredicateLogic.pdf` (part C, not covered by our lecture notes).

---
## Part A. Predicate logic (deck PredicateLogic)

## 1. What FOL talks about (slides 1-2)
![[cmpe421_lecture_07_predicatelogic_p001.jpg]]
![[cmpe421_lecture_07_predicatelogic_p002.jpg]]

**Notes.** Propositional logic assumes the world contains only facts. FOL assumes the world contains **objects** (people, numbers), **relations** (x is red, x is y's brother: true or false of its objects) and **functions** (the sister of x: gives back one object). Probability theory keeps facts but adds a degree of belief. Fuzzy logic gives facts a degree of truth.

## 2. Syntax (slides 3-5)
![[cmpe421_lecture_07_predicatelogic_p003.jpg]]
![[cmpe421_lecture_07_predicatelogic_p004.jpg]]
![[cmpe421_lecture_07_predicatelogic_p005.jpg]]

**Notes.** Symbols: constants ($\mathit{KingJohn}$, $2$), predicates ($\mathit{Brother}$, $>$), functions ($\mathit{Sqrt}$, $\mathit{LeftLegOf}$), variables ($x,y$), connectives, equality, quantifiers.

- **Term** = constant, variable, or $\mathit{function}(\mathit{term}_1,\dots,\mathit{term}_n)$. A term names an object.
- **Atomic sentence** = $\mathit{predicate}(\mathit{term}_1,\dots,\mathit{term}_n)$ or $\mathit{term}_1=\mathit{term}_2$. It is true or false.
- **Complex sentence**: atomic sentences joined by $\neg,\land,\lor,\Rightarrow,\Leftrightarrow$.

> [!warning] Watch out
> A predicate may contain terms, but a term never contains a predicate. $\mathit{LeftLegOf}(\mathit{Brother}(\mathit{Richard},\mathit{John}))$ is not allowed.

## 3. Truth: worlds and models (slides 6-8)
![[cmpe421_lecture_07_predicatelogic_p006.jpg]]
![[cmpe421_lecture_07_predicatelogic_p007.jpg]]
![[cmpe421_lecture_07_predicatelogic_p008.jpg]]

**Notes.** A world is $M=(D,I)$. $D$ is the **domain**: a nonempty set of objects. $I$ is the **interpretation**: it maps constant symbols to objects, predicate symbols to relations, function symbols to functions. An atomic sentence is true in $M$ if the objects its terms refer to satisfy the relation its predicate refers to. $M$ is a model of $\alpha$ if $\alpha$ is true in $M$.

Example (slide 7): $\mathit{Brother}(\mathit{Richard},\mathit{John})$ is true if the pair (Richard the Lionheart, the evil King John) is in the brotherhood relation.

There are far too many worlds to enumerate (slide 8), so model checking by enumeration is not practical in FOL.

## 4. Quantifiers (slides 9-11)
![[cmpe421_lecture_07_predicatelogic_p009.jpg]]
![[cmpe421_lecture_07_predicatelogic_p010.jpg]]
![[cmpe421_lecture_07_predicatelogic_p011.jpg]]

**Notes.** $\forall x\ P(x)$ is true in a world if $P$ holds for every object. $\exists x\ P(x)$ is true if $P$ holds for at least one object.

Order: $\forall x\,\forall y$ equals $\forall y\,\forall x$, and the same for two $\exists$. But $\exists x\,\forall y\ \mathit{Loves}(x,y)$ (one person loves everyone) is not $\forall y\,\exists x\ \mathit{Loves}(x,y)$ (everyone is loved by someone).

Duality:

$$\forall x\,P\equiv\neg\exists x\,\neg P,\qquad \exists x\,P\equiv\neg\forall x\,\neg P$$

> [!warning] Watch out
> $\forall$ goes with $\Rightarrow$, $\exists$ goes with $\land$. $\forall x\ \mathit{At}(x,\mathit{UMD})\land\mathit{Smart}(x)$ says everything is at UMD and smart (too strong). $\exists x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$ is true as soon as one object is not at UMD (too weak).

## 5. Translating English, equality (slides 12-13)
![[cmpe421_lecture_07_predicatelogic_p012.jpg]]
![[cmpe421_lecture_07_predicatelogic_p013.jpg]]

**Notes.** Examples from slide 12:
- Brothers are siblings: $\forall x,y\ \mathit{Brother}(x,y)\Rightarrow\mathit{Sibling}(x,y)$
- Sibling is symmetric: $\forall x,y\ \mathit{Sibling}(x,y)\Leftrightarrow\mathit{Sibling}(y,x)$
- One's mother is one's female parent: $\forall x,y\ \mathit{Mother}(x,y)\Leftrightarrow(\mathit{Female}(x)\land\mathit{Parent}(x,y))$

$\mathit{term}_1=\mathit{term}_2$ is true if both terms refer to the same object. Equality is needed to say "two different": $\exists x\,\exists y\ \mathit{Brother}(x,\mathit{Richard})\land\mathit{Brother}(y,\mathit{Richard})\land\neg(x=y)$.

## 6. Substitutions, numbers, sets, lists (slides 14-16)
![[cmpe421_lecture_07_predicatelogic_p014.jpg]]
![[cmpe421_lecture_07_predicatelogic_p015.jpg]]
![[cmpe421_lecture_07_predicatelogic_p016.jpg]]

**Notes.** A **substitution** is a set of variable bindings. These slides write $\theta=\{x=1,\ y=f(z)\}$ (Russell and Norvig write $\{x/1,\ y/f(z)\}$). $S\theta$ is the result of applying $\theta$ to sentence $S$. Bindings are applied simultaneously.

A **unifier** of $p$ and $q$ is a substitution with $p\theta=q\theta$ (our notes add this, the deck covers it in part C).

Natural numbers (slide 15): $\mathit{NatNum}(0)$, and $\forall n\ \mathit{NatNum}(n)\Rightarrow\mathit{NatNum}(S(n))$, where $S$ is the successor function. Addition: $+(0,m)=m$ and $+(S(m),n)=S(+(m,n))$.

## 7. Using a FOL knowledge base (slides 17-20)
![[cmpe421_lecture_07_predicatelogic_p017.jpg]]
![[cmpe421_lecture_07_predicatelogic_p018.jpg]]
![[cmpe421_lecture_07_predicatelogic_p019.jpg]]
![[cmpe421_lecture_07_predicatelogic_p020.jpg]]

**Notes.** `Tell(KB, S)` adds sentence $S$. `Ask(KB, S)` returns some or all substitutions $\sigma$ with $KB\models S\sigma$.

Wumpus KB (slide 18): perception rule $\forall t,s,g,m,c\ \mathit{Percept}([s,\mathit{Breeze},g,m,c],t)\Rightarrow\mathit{Breeze}(t)$, reflex rule $\forall t\ \mathit{Glitter}(t)\Rightarrow\mathit{BestAction}(\mathit{Grab},t)$, and a successor state axiom

$$\forall t\ \mathit{HaveArrow}(t+1)\Leftrightarrow[\mathit{HaveArrow}(t)\land\neg\mathit{Action}(\mathit{Shoot},t)]$$

One quantified sentence replaces one propositional sentence per square or per time step.

> [!warning] Errors on the slides
> - Slide 19: the brackets of the adjacency axiom do not match, and the pit axiom is printed without $\exists r$.
> - Slide 17 asks for $\mathit{Action}(a,5)$, but the rules on slide 18 conclude $\mathit{BestAction}(\mathit{Grab},t)$.

---
## Part B. Deterministic planning (deck DeterministicPlanning)

## 8. Blocks world and states (slides 1-5)
![[cmpe421_lecture_07_deterministicplanning_p001.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p002.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p003.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p004.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p005.jpg]]

**Notes.** The planning language is FOL without function symbols, plus operator names. Blocks world predicates: `ontable(x)`, `on(x,y)`, `clear(x)`, `holding(x)`, `handempty`.

A **state** is a set of ground atoms that are currently true. An atom not in the set is false. With $c$ constants and $p$ predicates of $k$ arguments there are $p\,c^k$ ground atoms and $2^{p\,c^k}$ possible states: finite.

## 9. Operators and actions (slides 6-11)
![[cmpe421_lecture_07_deterministicplanning_p006.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p007.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p008.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p009.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p010.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p011.jpg]]

**Notes.** An **operator** is (head, preconditions, effects). An **action** is a ground instance of an operator.

| Operator | Precond | Effects |
|---|---|---|
| `unstack(x,y)` | `on(x,y)`, `clear(x)`, `handempty` | $\neg$`on(x,y)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)`, `clear(y)` |
| `stack(x,y)` | `holding(x)`, `clear(y)` | $\neg$`holding(x)`, $\neg$`clear(y)`, `on(x,y)`, `clear(x)`, `handempty` |
| `pickup(x)` | `ontable(x)`, `clear(x)`, `handempty` | $\neg$`ontable(x)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)` |
| `putdown(x)` | `holding(x)` | $\neg$`holding(x)`, `ontable(x)`, `clear(x)`, `handempty` |

For a set of literals $S$, $S^+$ holds the atoms that appear positively and $S^-$ those that appear negatively. Action $a$ is **executable** in $s$ if

$$\text{precond}^+(a)\subseteq s\quad\text{and}\quad\text{precond}^-(a)\cap s=\emptyset$$

Result of performing it:

$$\gamma(s,a)=\big(s-\text{effects}^-(a)\big)\cup\text{effects}^+(a)$$

> [!warning] Watch out
> - Test executability first, then delete, then add.
> - Slide 11 prints the result with an extra `clear(b)` that is missing from the state printed above it. The state has lost `clear(b)` (slides 13 and 15 list it).

## 10. Plans, problems, complexity (slides 12-16)
![[cmpe421_lecture_07_deterministicplanning_p012.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p013.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p014.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p015.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p016.jpg]]

**Notes.** A **plan** $\pi=(a_1,\dots,a_n)$ is executable in $s_0$ if each action is executable in the state produced by the previous one. A **planning problem** is $P=(O,s_0,g)$: operators, initial state, goal (a set of literals). $\pi$ is a **solution** if it is executable in $s_0$ and $\gamma(s_0,\pi)$ satisfies $g$.

> [!example] Slide 15
> $s_0=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `handempty`$\}$, $g=\{$`on(a,b)`$\}$.
> One solution: `unstack(c,a)`, `putdown(c)`, `pickup(a)`, `stack(a,b)`.

Complexity (slide 16): deciding whether a classical (function-free) problem has a solution is PSPACE-complete. With function symbols it is undecidable.

## 11. Forward search (slide 17)
![[cmpe421_lecture_07_deterministicplanning_p017.jpg]]

**Notes.** Search forward from $s_0$, applying executable actions, until a state satisfies $g$. Sound: a returned plan is a solution. Breadth-first and best-first are complete but need memory exponential in the solution length. Depth-first and greedy need linear memory but are not complete, unless loop-checking is added (the number of states is finite).

Backward search (our notes add this): regress the goal through a relevant action, $\gamma^{-1}(g,a)=(g-\text{effects}^+(a))\cup\text{precond}(a)$, until the goal is a subset of $s_0$.

## 12. Graphplan and the planning graph (slides 18-32)
![[cmpe421_lecture_07_deterministicplanning_p018.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p019.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p020.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p021.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p022.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p023.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p024.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p025.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p026.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p027.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p028.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p029.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p030.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p031.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p032.jpg]]

**Notes.** Idea: solve a **relaxed problem** first (easy, polynomial time, its solutions include all real solutions), then search only among the actions that appear in it.

**Planning graph**: alternating layers. State-level $i$ holds the literals that might be true at time $i$. Action-level $i$ holds the actions that might be executable at time $i$. Edges are preconditions and effects. A **maintenance action** per literal carries it forward unchanged.

**Mutex** (mutually exclusive), slide 23. Two actions at one level are mutex if:
- inconsistent effects: an effect of one negates an effect of the other,
- interference: one deletes a precondition of the other,
- competing needs: their preconditions are mutex.

Two literals are mutex if one negates the other, or all ways of achieving them are pairwise mutex (inconsistent support).

**Graphplan**: for $k=0,1,2,\dots$ expand the graph to $k$ levels. If all goals are present and pairwise non-mutex (necessary, not sufficient), run **solution extraction**: a backward search that picks, for each goal literal, an action at the previous level, backtracks on mutex pairs, and recurses on the preconditions.

> [!example] Dinner date (slides 21-32)
> $s_0=\{$garbage, cleanHands, quiet$\}$, $g=\{$dinner, present, $\neg$garbage$\}$.
> `cook`: needs cleanHands, gives dinner. `wrap`: needs quiet, gives present. `carry`: gives $\neg$garbage, $\neg$cleanHands. `dolly`: gives $\neg$garbage, $\neg$quiet.
> With one level the goals cannot be achieved without mutex actions (`carry` deletes the precondition of `cook`, `dolly` that of `wrap`). With two levels a plan exists, for example level 1 `cook`, `wrap`, level 2 `carry`.

> [!warning] Watch out
> - Slide 30 says "twelve combinations at level 4". The graph has only levels 0 to 2, and the combinations are at level 2.
> - Parallel length is not the number of actions: the plan above has 2 levels but 3 actions.

## 13. Heuristics and FastForward (slides 33-38)
![[cmpe421_lecture_07_deterministicplanning_p033.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p034.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p035.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p036.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p037.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p038.jpg]]

**Notes.** Forward search needs a good heuristic. Graph expansion is polynomial, solution extraction is exponential, so use only the expansion: $\Delta^g(s,g)$ = level of the first layer of a planning graph from $s$ that "possibly achieves" the goal.

**FastForward (FF)**: no A*-style search (too much memory). Greedy: expand the current state and move to the child with the smallest $h$. In a local minimum FF does a breadth-first search until it finds a node with lower $h$. In a dead end FF fails. No guarantee, but it works well on many classical problems.

---
## Part C. Extra deck: inference in predicate logic

> [!info] Not in our lecture notes
> The deck `lectures/Lecture 8/Inference-PredicateLogic.pdf` (48 slides, CMSC 421 Chapter 9) has no easybook notes yet. The notes below are taken from the slide text only. Our Lecture 9 notes cover unification and Generalized Modus Ponens briefly as additions.

## 14. Instantiation and propositionalization (inference deck, slides 1-8)
![[cmpe421_lecture_8_inference_predicatelogic_p001.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p002.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p003.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p004.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p005.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p006.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p007.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p008.jpg]]

**Notes.** **Universal instantiation (UI)**: from $\forall v\ \alpha$ infer $\alpha\{v=g\}$ for any ground term $g$. **Existential instantiation (EI)**: from $\exists v\ \alpha$ infer $\alpha\{v=k\}$ for a new constant $k$ (a Skolem constant) that appears nowhere else. UI can be used many times, EI once.

Instantiating every universal sentence in all possible ways turns a FOL KB into a propositional one. Problem: with function symbols there are infinitely many ground terms ($\mathit{father}(\mathit{father}(\mathit{John}))$, ...). Herbrand's theorem: an entailed sentence is entailed by a finite subset of the propositionalized KB.

## 15. Unification and Generalized Modus Ponens (inference deck, slides 9-20)
![[cmpe421_lecture_8_inference_predicatelogic_p009.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p010.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p011.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p012.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p013.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p014.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p015.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p016.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p017.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p018.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p019.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p020.jpg]]

**Notes.** A **unifier** of $\alpha$ and $\beta$ is a substitution $\theta$ with $\alpha\theta=\beta\theta$.

| $p$ | $q$ | $\theta$ |
|---|---|---|
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(\mathit{John},\mathit{Jane})$ | $\{x=\mathit{Jane}\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(y,\mathit{Joanna})$ | $\{x=\mathit{Joanna},\ y=\mathit{John}\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(y,\mathit{mother}(y))$ | $\{y=\mathit{John},\ x=\mathit{mother}(\mathit{John})\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(x,\mathit{Joanna})$ | fail (rename one $x$ first: standardizing apart) |

A **most general unifier (mgu)** binds no more than needed.

**Generalized Modus Ponens**: from facts $p_1',\dots,p_n'$ and a rule $p_1\land\dots\land p_n\Rightarrow q$, with $p_i'\theta=p_i\theta$ for all $i$, infer $q\theta$. Example: $\mathit{King}(\mathit{John})$, $\mathit{Greedy}(y)$ and $\mathit{King}(x)\land\mathit{Greedy}(x)\Rightarrow\mathit{Evil}(x)$ give $\mathit{Evil}(\mathit{John})$ with $\theta=\{x=\mathit{John},\ y=\mathit{John}\}$.

## 16. Forward chaining in FOL (inference deck, slides 21-31)
![[cmpe421_lecture_8_inference_predicatelogic_p021.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p022.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p023.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p024.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p025.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p026.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p027.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p028.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p029.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p030.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p031.jpg]]

**Notes.** Colonel West KB: $\mathit{American}(x)\land\mathit{Weapon}(y)\land\mathit{Sells}(x,y,z)\land\mathit{Hostile}(z)\Rightarrow\mathit{Criminal}(x)$, $\mathit{Missile}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Sells}(\mathit{West},x,\mathit{Nono})$, $\mathit{Missile}(x)\Rightarrow\mathit{Weapon}(x)$, $\mathit{Enemy}(x,\mathit{America})\Rightarrow\mathit{Hostile}(x)$, plus the facts $\mathit{American}(\mathit{West})$, $\mathit{Missile}(M_1)$, $\mathit{Owns}(\mathit{Nono},M_1)$, $\mathit{Enemy}(\mathit{Nono},\mathit{America})$.

Forward chaining fires every rule whose premises unify with known facts and adds the conclusions, round after round: first $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$, $\mathit{Hostile}(\mathit{Nono})$, then $\mathit{Criminal}(\mathit{West})$.

It is sound and complete for first-order Horn clauses. If the query is not entailed it may not terminate (entailment is semidecidable). It always terminates for Datalog (Horn clauses without functions).

## 17. Backward chaining and Prolog (inference deck, slides 32-44)
![[cmpe421_lecture_8_inference_predicatelogic_p032.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p033.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p034.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p035.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p036.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p037.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p038.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p039.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p040.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p041.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p042.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p043.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p044.jpg]]

**Notes.** Backward chaining starts from the goal $\mathit{Criminal}(\mathit{West})$, unifies it with a rule head and proves the premises one by one, carrying the substitution along. It is a depth-first proof search: space is linear in the size of the proof. It is incomplete because of infinite loops (partly fixed by checking the goal stack) and inefficient because of repeated subgoals (fixed by caching).

**Prolog** = backward chaining with Horn clauses plus extras. A program is a set of clauses `head :- literal1, ..., literaln.` Capitalized words are variables. Negation is "negation as failure": `alive(joe)` succeeds if `dead(joe)` fails.

## 18. Resolution in FOL (inference deck, slides 45-48)
![[cmpe421_lecture_8_inference_predicatelogic_p045.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p046.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p047.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p048.jpg]]

**Notes.** Resolution works as in propositional logic, with a substitution $\theta$ that unifies the two complementary literals. Example: $\neg\mathit{Rich}(x)\lor\mathit{Unhappy}(x)$ and $\mathit{Rich}(\mathit{Ken})$ give $\mathit{Unhappy}(\mathit{Ken})$ with $\theta=\{x=\mathit{Ken}\}$. To prove $\alpha$, convert $KB\land\neg\alpha$ to CNF and resolve.

Conversion to CNF: (1) remove $\Leftrightarrow$ and $\Rightarrow$, (2) move $\neg$ inwards, (3) standardize variables, (4) Skolemize: replace each existential variable by a Skolem function of the enclosing universal variables, (5) drop the universal quantifiers, (6) distribute $\lor$ over $\land$.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Term | constant, variable, or function applied to terms. Names an object |
| Atomic sentence | predicate applied to terms, or $t_1=t_2$. True or false |
| World | $M=(D,I)$: domain and interpretation |
| Quantifier duality | $\neg\forall x\,P\equiv\exists x\,\neg P$, $\neg\exists x\,P\equiv\forall x\,\neg P$ |
| Substitution | $S\theta$: apply the bindings of $\theta$ to $S$, simultaneously |
| Unifier | $\theta$ with $p\theta=q\theta$ |
| GMP | facts $p_i'$, rule $p_1\land\dots\land p_n\Rightarrow q$, $p_i'\theta=p_i\theta$: infer $q\theta$ |
| State | set of ground atoms. $p\,c^k$ atoms, $2^{p\,c^k}$ states |
| Executable | $\text{precond}^+(a)\subseteq s$ and $\text{precond}^-(a)\cap s=\emptyset$ |
| Result | $\gamma(s,a)=(s-\text{effects}^-(a))\cup\text{effects}^+(a)$ |
| Solution | plan executable in $s_0$ whose result satisfies $g$ |
| Planning graph | alternating literal and action layers of a relaxed problem |
| Mutex actions | inconsistent effects, interference, competing needs |
| FF | greedy forward search on a planning-graph heuristic |

## Check yourself
> [!question]- 1. Term, atomic sentence, complex sentence or not allowed: (a) $\mathit{Sqrt}(2)$, (b) $\mathit{Brother}(x,\mathit{Richard})$, (c) $\mathit{Sqrt}({>}(1,2))$?
> (a) term. (b) atomic sentence. (c) not allowed: a function takes terms, not sentences.

> [!question]- 2. Rewrite $\neg\exists x\ (\mathit{Student}(x)\land\mathit{Lazy}(x))$ with $\forall$ and an implication.
> Flip the quantifier: $\forall x\ \neg(\mathit{Student}(x)\land\mathit{Lazy}(x))$, which is $\forall x\ \mathit{Student}(x)\Rightarrow\neg\mathit{Lazy}(x)$.

> [!question]- 3. Unify or say why it fails: (a) $P(x,x)$ and $P(A,B)$, (b) $Q(F(x),B)$ and $Q(y,x)$.
> (a) $x=A$, then $A$ against $B$: two different constants, fail. (b) $y=F(x)$, then $x=B$, applied to the first binding: $\{y=F(B),\ x=B\}$.

> [!question]- 4. How many ground atoms and states does the blocks world with 3 blocks have?
> `ontable` 3, `on` $3^2=9$, `clear` 3, `holding` 3, `handempty` 1: 19 atoms. $2^{19}=524\,288$ states.

> [!question]- 5. $s=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `holding(d)`, `ontable(e)`, `clear(e)`$\}$. Compute $\gamma(s,$`stack(d,c)`$)$.
> Executable: `holding(d)` and `clear(c)` are in $s$. Delete `holding(d)`, `clear(c)`. Add `on(d,c)`, `clear(d)`, `handempty`. Result: `ontable(a)`, `on(c,a)`, `on(d,c)`, `clear(d)`, `ontable(b)`, `clear(b)`, `ontable(e)`, `clear(e)`, `handempty` (9 atoms).

> [!question]- 6. Dinner date: at action-level 1, is `cook` mutex with `wrap`? Is `carry` mutex with `cook`?
> `cook` and `wrap`: no effect negates another, neither deletes a precondition, preconditions are not mutex. Not mutex. `carry` deletes cleanHands, the precondition of `cook`: interference, mutex.

## Links
- Course index: [[CMPE421 Home]]
- Previous lecture: [[CMPE421 L08 - Logical agents]]
- Next lecture: [[CMPE421 L10 - Quantifying uncertainty]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf`
- Original slides (PDF, stored in the lecture_07 folder): [PredicateLogic.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/PredicateLogic.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/PredicateLogic.pdf`
- Original slides (PDF): [DeterministicPlanning.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/DeterministicPlanning.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/DeterministicPlanning.pdf`
- Extra deck (PDF, no lecture notes yet): [Inference-PredicateLogic.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/Lecture%208/Inference-PredicateLogic.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/Lecture 8/Inference-PredicateLogic.pdf`
