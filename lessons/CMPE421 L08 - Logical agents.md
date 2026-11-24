---
course: CMPE 421 Artificial Intelligence
lecture: 8
title: Logical agents
tags: [cmpe421, lecture]
slides: lectures/lecture_07/logical-agents.pdf
full_notes: lecture_notes/lecture_08/CMPE421_Lecture_08_Logical_Agents.pdf
---
# CMPE 421 · Lecture 8 · Logical agents

> [!summary] In short
> - A knowledge-based agent keeps a knowledge base (KB) of sentences and an inference engine. Each step: Tell the percept, Ask for an action, Tell the action.
> - $KB\models\alpha$ (entails) means $\alpha$ is true in every world where the KB is true: $M(KB)\subseteq M(\alpha)$.
> - Model checking with a truth table is sound and complete, but needs $2^n$ rows.
> - Horn clauses allow forward and backward chaining with Modus Ponens, in linear time.
> - Resolution on CNF clauses is sound and complete for all of propositional logic. Prove $\alpha$ by refuting $KB\land\neg\alpha$.
> - A propositional wumpus agent needs time-indexed fluents and successor state axioms.

## 1. Knowledge-based agents (slides 1-2)
![[cmpe421_lecture_07_logical_agents_p001.jpg]]
![[cmpe421_lecture_07_logical_agents_p002.jpg]]

**Notes.** A **knowledge base** is a set of sentences in a formal language. It holds the domain-specific content. The **inference engine** is a set of domain-independent algorithms that derive new sentences. The agent loop: `Tell(KB, percept)`, `Ask(KB, action?)`, `Tell(KB, action)`, then $t\leftarrow t+1$. So each call makes 2 Tells and 1 Ask. Knowledge level: what the agent knows. Implementation level: how it is stored.

## 2. The Wumpus world (slides 3-10)
![[cmpe421_lecture_07_logical_agents_p003.jpg]]
![[cmpe421_lecture_07_logical_agents_p004.jpg]]
![[cmpe421_lecture_07_logical_agents_p005.jpg]]
![[cmpe421_lecture_07_logical_agents_p006.jpg]]
![[cmpe421_lecture_07_logical_agents_p007.jpg]]
![[cmpe421_lecture_07_logical_agents_p008.jpg]]
![[cmpe421_lecture_07_logical_agents_p009.jpg]]
![[cmpe421_lecture_07_logical_agents_p010.jpg]]

**Notes.** Performance: gold $+1000$, death $-1000$, $-1$ per step, $-10$ for the arrow. Squares next to a pit are breezy, squares next to the wumpus are smelly, glitter means the gold is here. Each square has a pit with probability 0.2. Actuators: Left turn, Right turn, Forward, Grab, Release, Shoot. Sensors: Breeze, Glitter, Smell.

Slides 4-10 build the characterization one line at a time: partially observable, deterministic, sequential, static, discrete, single-agent.

> [!warning] Watch out
> Deterministic does not mean the agent knows the outcome. Forward into an unknown square has one fixed result, but the agent cannot see which.

## 3. Exploring a wumpus world (slides 11-19)
![[cmpe421_lecture_07_logical_agents_p011.jpg]]
![[cmpe421_lecture_07_logical_agents_p012.jpg]]
![[cmpe421_lecture_07_logical_agents_p013.jpg]]
![[cmpe421_lecture_07_logical_agents_p014.jpg]]
![[cmpe421_lecture_07_logical_agents_p015.jpg]]
![[cmpe421_lecture_07_logical_agents_p016.jpg]]
![[cmpe421_lecture_07_logical_agents_p017.jpg]]
![[cmpe421_lecture_07_logical_agents_p018.jpg]]
![[cmpe421_lecture_07_logical_agents_p019.jpg]]

**Notes.** Nothing perceived in $[1,1]$: both neighbours are safe. Breeze in one neighbour: a pit is in one of its unvisited neighbours. Stench in the other: the wumpus is next to it. Combining facts found at different times locates the pit and the wumpus and proves a third square safe. A missing percept is knowledge too: no stench clears all neighbours of the wumpus.

Tight spots (slide 19): breeze in both $(1,2)$ and $(2,1)$ leaves no safe action. Then use probabilities ($\Pr[\text{pit in }(2,2)]\approx 0.86$). With a smell in $(1,1)$, shoot straight ahead to make a square safe (coercion).

> [!warning] Watch out
> - Slides 11-19 use the slide 3 cave with $x$ and $y$ swapped: pit in $[1,3]$, wumpus in $[3,1]$, gold in $[3,2]$.
> - Slide 19's $0.31$ is the probability for each of $(1,3)$ and $(3,1)$, not for both together.

## 4. Logic, entailment and models (slides 20-22)
![[cmpe421_lecture_07_logical_agents_p020.jpg]]
![[cmpe421_lecture_07_logical_agents_p021.jpg]]
![[cmpe421_lecture_07_logical_agents_p022.jpg]]

**Notes.** A logic is a formal language. **Syntax** says which strings are sentences. **Semantics** says when a sentence is true in a world.

A possible world $m$ is a **model** of $\alpha$ if $\alpha$ is true in $m$. $M(\alpha)$ is the set of all models of $\alpha$.

$$KB\models\alpha\quad\Longleftrightarrow\quad M(KB)\subseteq M(\alpha)$$

Entailment goes one way. One world where the KB is true and $\alpha$ is false disproves it. To prove it, every model of the KB must be checked.

## 5. Entailment in the wumpus world, inference (slides 23-29)
![[cmpe421_lecture_07_logical_agents_p023.jpg]]
![[cmpe421_lecture_07_logical_agents_p024.jpg]]
![[cmpe421_lecture_07_logical_agents_p025.jpg]]
![[cmpe421_lecture_07_logical_agents_p026.jpg]]
![[cmpe421_lecture_07_logical_agents_p027.jpg]]
![[cmpe421_lecture_07_logical_agents_p028.jpg]]
![[cmpe421_lecture_07_logical_agents_p029.jpg]]

**Notes.** After nothing in $[1,1]$ and a breeze in $[2,1]$, three unknown squares give $2^3=8$ possible worlds. The KB is true in three of them. $\alpha_1$ = "$[1,2]$ is safe" is true in all three: entailed. $\alpha_2$ = "$[2,2]$ is safe" is false in some: not entailed. This procedure is **model checking**.

$KB\vdash_i\alpha$ means procedure $i$ derives $\alpha$ from the KB.
- **Sound**: whatever $i$ derives is entailed.
- **Complete**: whatever is entailed, $i$ can derive.

> [!warning] Watch out
> $KB\not\models\alpha_2$ does not mean "$[2,2]$ has a pit". The KB simply does not know. "Not entailed" and "the negation is entailed" are different.

## 6. Propositional logic: syntax and semantics (slides 30-33)
![[cmpe421_lecture_07_logical_agents_p030.jpg]]
![[cmpe421_lecture_07_logical_agents_p031.jpg]]
![[cmpe421_lecture_07_logical_agents_p032.jpg]]
![[cmpe421_lecture_07_logical_agents_p033.jpg]]

**Notes.** Sentences: proposition symbols, $\neg S$, $S_1\land S_2$, $S_1\lor S_2$, $S_1\Rightarrow S_2$, $S_1\Leftrightarrow S_2$. A world gives each symbol true or false.

| Sentence | True when |
|---|---|
| $\neg S$ | $S$ is false |
| $S_1\land S_2$ | both are true |
| $S_1\lor S_2$ | at least one is true |
| $S_1\Rightarrow S_2$ | $S_1$ is false or $S_2$ is true (false only for true, false) |
| $S_1\Leftrightarrow S_2$ | both sides have the same value |

Wumpus sentences: $P_{i,j}$ = pit in $[i,j]$, $B_{i,j}$ = breeze in $[i,j]$. "Pits cause breezes in adjacent squares": $R_1: B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$ and $R_2: B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$. The $\Leftrightarrow$ is needed: "breeze, so some pit is next to me" uses the other direction.

> [!warning] Error on slide 31
> The stated world (true, true, false) does not match the slide's computation. The computation uses the world false, false, true.

## 7. Truth tables for inference (slides 34-35)
![[cmpe421_lecture_07_logical_agents_p034.jpg]]
![[cmpe421_lecture_07_logical_agents_p035.jpg]]

**Notes.** One row per world. $KB\models\alpha$ exactly when no row has the KB true and $\alpha$ false. The algorithm `TT-Entails` enumerates all rows depth-first. It is sound and complete, but takes $O(2^n)$ time for $n$ symbols (the problem is co-NP-complete).

> [!warning] Error on slide 34
> The slide lists only four facts ($\neg B_{1,1}$, $B_{2,1}$, $\neg P_{1,1}$, $\neg P_{2,1}$) above the table. Its KB column also uses $R_1$ and $R_2$.

## 8. Proof methods, equivalence, validity (slides 36-38)
![[cmpe421_lecture_07_logical_agents_p036.jpg]]
![[cmpe421_lecture_07_logical_agents_p037.jpg]]
![[cmpe421_lecture_07_logical_agents_p038.jpg]]

**Notes.** Two kinds of proof method: model checking (truth table enumeration, improved backtracking such as DPLL, heuristic search in model space) and applying inference rules (sound generation of new sentences from old).

$\alpha\equiv\beta$ (logically equivalent) if they are true in the same models. Most used:

$$
\begin{aligned}
(\alpha\Rightarrow\beta)&\equiv(\neg\alpha\lor\beta) &&\text{implication elimination}\\
(\alpha\Rightarrow\beta)&\equiv(\neg\beta\Rightarrow\neg\alpha) &&\text{contraposition}\\
(\alpha\Leftrightarrow\beta)&\equiv((\alpha\Rightarrow\beta)\land(\beta\Rightarrow\alpha)) &&\text{biconditional elimination}\\
\neg(\alpha\land\beta)&\equiv(\neg\alpha\lor\neg\beta),\quad \neg(\alpha\lor\beta)\equiv(\neg\alpha\land\neg\beta) &&\text{De Morgan}\\
(\alpha\lor(\beta\land\gamma))&\equiv((\alpha\lor\beta)\land(\alpha\lor\gamma)) &&\text{distributivity}
\end{aligned}
$$

**Valid**: true in all worlds ($A\lor\neg A$). **Satisfiable**: true in at least one ($A\lor B$). **Unsatisfiable**: true in none ($A\land\neg A$).

$$KB\models\alpha\iff(KB\Rightarrow\alpha)\text{ is valid}\iff(KB\land\neg\alpha)\text{ is unsatisfiable}$$

> [!warning] Watch out
> $\models$ (entails), $\vdash$ (derives) and $\Rightarrow$ (a connective inside one sentence) are three different symbols. "Not valid" is not "unsatisfiable".

## 9. Horn clauses and forward chaining (slides 39-50)
![[cmpe421_lecture_07_logical_agents_p039.jpg]]
![[cmpe421_lecture_07_logical_agents_p040.jpg]]
![[cmpe421_lecture_07_logical_agents_p041.jpg]]
![[cmpe421_lecture_07_logical_agents_p042.jpg]]
![[cmpe421_lecture_07_logical_agents_p043.jpg]]
![[cmpe421_lecture_07_logical_agents_p044.jpg]]
![[cmpe421_lecture_07_logical_agents_p045.jpg]]
![[cmpe421_lecture_07_logical_agents_p046.jpg]]
![[cmpe421_lecture_07_logical_agents_p047.jpg]]
![[cmpe421_lecture_07_logical_agents_p048.jpg]]
![[cmpe421_lecture_07_logical_agents_p049.jpg]]
![[cmpe421_lecture_07_logical_agents_p050.jpg]]

**Notes.** A **Horn clause** is a symbol (a fact) or (conjunction of symbols) $\Rightarrow$ symbol (a rule). **Modus Ponens**:

$$\frac{\alpha_1,\ \dots,\ \alpha_n,\qquad \alpha_1\land\dots\land\alpha_n\Rightarrow\beta}{\beta}$$

It is complete for Horn KBs and runs in linear time.

**Forward chaining**: keep an agenda of known facts and, per rule, a count of premises still missing. Pop a fact, lower the count of each rule that contains it, and push the rule's head when the count reaches 0. Mark popped symbols as inferred so they are processed once.

> [!example] Slides 42-49
> KB: $P\Rightarrow Q$, $L\land M\Rightarrow P$, $B\land L\Rightarrow M$, $A\land P\Rightarrow L$, $A\land B\Rightarrow L$, facts $A$, $B$. Query $Q$. Counts start at $1,2,2,2,2$, agenda $\langle A,B\rangle$. Facts are derived in the order $A, B, L, M, P, Q$.

Slide 50 proves completeness: at the fixed point, the world that makes exactly the inferred symbols true is a model of the KB, so every entailed symbol was inferred.

> [!warning] Watch out
> - Slides 47-49 run on after the pseudocode of slide 41 would already return true.
> - If forward chaining ends without $q$, then $KB\not\models q$. It does not show $KB\models\neg q$.

## 10. Backward chaining (slides 51-63)
![[cmpe421_lecture_07_logical_agents_p051.jpg]]
![[cmpe421_lecture_07_logical_agents_p052.jpg]]
![[cmpe421_lecture_07_logical_agents_p053.jpg]]
![[cmpe421_lecture_07_logical_agents_p054.jpg]]
![[cmpe421_lecture_07_logical_agents_p055.jpg]]
![[cmpe421_lecture_07_logical_agents_p056.jpg]]
![[cmpe421_lecture_07_logical_agents_p057.jpg]]
![[cmpe421_lecture_07_logical_agents_p058.jpg]]
![[cmpe421_lecture_07_logical_agents_p059.jpg]]
![[cmpe421_lecture_07_logical_agents_p060.jpg]]
![[cmpe421_lecture_07_logical_agents_p061.jpg]]
![[cmpe421_lecture_07_logical_agents_p062.jpg]]
![[cmpe421_lecture_07_logical_agents_p063.jpg]]

**Notes.** Work backwards from the query $q$. To prove $q$, find a rule with head $q$ and prove all its premises. A known fact is proved at once. Avoid loops: check whether a new subgoal is already on the goal stack. Avoid repeated work: remember goals already proved or already failed.

In the example, proving $P$ needs $L$, and the rule $A\land P\Rightarrow L$ needs $P$ again. The loop check rejects it, and the second rule $A\land B\Rightarrow L$ succeeds. A failed rule is not a failed goal.

| | Forward chaining | Backward chaining |
|---|---|---|
| Driven by | data | goal |
| Good for | automatic processing (object recognition, routine decisions) | problem solving ("where are my keys?") |
| Cost | may do work irrelevant to the goal | can be much less than linear in the size of the KB |

## 11. Resolution (slides 64-69)
![[cmpe421_lecture_07_logical_agents_p064.jpg]]
![[cmpe421_lecture_07_logical_agents_p065.jpg]]
![[cmpe421_lecture_07_logical_agents_p066.jpg]]
![[cmpe421_lecture_07_logical_agents_p067.jpg]]
![[cmpe421_lecture_07_logical_agents_p068.jpg]]
![[cmpe421_lecture_07_logical_agents_p069.jpg]]

**Notes.** A **literal** is a symbol or its negation. A **clause** is an "or" of literals. **CNF** is an "and" of clauses.

**Resolution rule**: if clause $C$ contains $\ell$ and clause $D$ contains $\neg\ell$, infer the "or" of all other literals of both.

$$\frac{A\lor\neg B\qquad B\lor\neg C\lor\neg D}{A\lor\neg C\lor\neg D}\qquad\qquad\frac{P\qquad\neg P}{\square}$$

Conversion to CNF (slide 66), for $B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$:
1. Remove $\Leftrightarrow$: $(B_{1,1}\Rightarrow(P_{1,2}\lor P_{2,1}))\land((P_{1,2}\lor P_{2,1})\Rightarrow B_{1,1})$
2. Remove $\Rightarrow$: $(\neg B_{1,1}\lor P_{1,2}\lor P_{2,1})\land(\neg(P_{1,2}\lor P_{2,1})\lor B_{1,1})$
3. Move $\neg$ inwards: $(\neg B_{1,1}\lor P_{1,2}\lor P_{2,1})\land((\neg P_{1,2}\land\neg P_{2,1})\lor B_{1,1})$
4. Distribute $\lor$ over $\land$: $(\neg B_{1,1}\lor P_{1,2}\lor P_{2,1})\land(\neg P_{1,2}\lor B_{1,1})\land(\neg P_{2,1}\lor B_{1,1})$

**Resolution algorithm**: to show $KB\models\alpha$, put $KB\land\neg\alpha$ into CNF and resolve pairs of clauses. Empty clause $\square$: entailed. No new clause can be added: not entailed.

> [!example] Slide 68
> KB: the three clauses above plus $\neg B_{1,1}$. Query $\alpha=\neg P_{1,2}$, so add $P_{1,2}$. Resolve $P_{1,2}$ with $\neg P_{1,2}\lor B_{1,1}$ to get $B_{1,1}$. Resolve $B_{1,1}$ with $\neg B_{1,1}$ to get $\square$. So there is no pit in $[1,2]$.

> [!warning] Watch out
> - Slide 68 prints "$KB\land\alpha$". It must be $KB\land\neg\alpha$, and the clause the slide adds ($P_{1,2}$) is indeed $\neg\alpha$.
> - Resolve on one complementary pair at a time. $A\lor B$ and $\neg A\lor\neg B$ do not give $\square$.

## 12. A wumpus agent in propositional logic (slides 70-78)
![[cmpe421_lecture_07_logical_agents_p070.jpg]]
![[cmpe421_lecture_07_logical_agents_p071.jpg]]
![[cmpe421_lecture_07_logical_agents_p072.jpg]]
![[cmpe421_lecture_07_logical_agents_p073.jpg]]
![[cmpe421_lecture_07_logical_agents_p074.jpg]]
![[cmpe421_lecture_07_logical_agents_p075.jpg]]
![[cmpe421_lecture_07_logical_agents_p076.jpg]]
![[cmpe421_lecture_07_logical_agents_p077.jpg]]
![[cmpe421_lecture_07_logical_agents_p078.jpg]]

**Notes.** Fixed facts of the cave are plain symbols: one breeze axiom and one stench axiom per square, at least one wumpus, at most one wumpus (one clause per pair of squares).

Everything that changes is a **fluent** with a time index: $L^t_{x,y}$ (agent at $[x,y]$ at time $t$), $\mathit{HaveArrow}^t$, $\mathit{WumpusAlive}^t$, $\mathit{Breeze}^t$.

Effect axioms say what an action changes, but not what stays the same. This is the **frame problem**. Also: the qualification problem (endless caveats) and the ramification problem (secondary consequences).

**Successor state axiom**: fluent $f$ is true at $t+1$ exactly when an action at $t$ made it true, or it was true at $t$ and no action made it false. Example:

$$\mathit{HaveArrow}^{t+1}\Leftrightarrow(\mathit{HaveArrow}^t\land\neg\mathit{Shoot}^t)$$

With all axioms and a resolution prover the agent can ask where it is, where the pit and wumpus are, and whether a square is OK (slide 78).

> [!warning] Errors on the slides
> - Slide 70 writes $B_{1,2}\Leftrightarrow(P_{1,1}\land P_{2,2}\land P_{1,3})$. It should use $\lor$. The same slide gives $B_{4,4}$ the wrong neighbours: they are $[3,4]$ and $[4,3]$.
> - Slide 76 lacks brackets in the location axiom. As printed, any bump anywhere would put the agent in $[1,1]$.

## 13. Summary (slide 79)
![[cmpe421_lecture_07_logical_agents_p079.jpg]]

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Entailment | $KB\models\alpha$ iff $M(KB)\subseteq M(\alpha)$ |
| Sound / complete | derives only entailed sentences / derives all entailed sentences |
| Implication | $S_1\Rightarrow S_2$ is false only for (true, false) |
| Model checking | $2^n$ rows. Entailed iff no row with KB true and $\alpha$ false |
| Valid / satisfiable / unsatisfiable | true in all / some / no worlds |
| Deduction theorem | $KB\models\alpha$ iff $KB\Rightarrow\alpha$ is valid |
| Refutation | $KB\models\alpha$ iff $KB\land\neg\alpha$ is unsatisfiable |
| Horn clause | a symbol, or (symbols joined by $\land$) $\Rightarrow$ symbol |
| Modus Ponens | $\alpha_1,\dots,\alpha_n$ and $\alpha_1\land\dots\land\alpha_n\Rightarrow\beta$ give $\beta$ |
| Forward chaining | agenda of facts, count of missing premises, push head at 0 |
| Backward chaining | goal, rule with that head, its premises. Loop check |
| CNF steps | remove $\Leftrightarrow$, remove $\Rightarrow$, $\neg$ inwards, distribute $\lor$ over $\land$ |
| Resolution | $\ell$ and $\neg\ell$ cancel, the rest is joined by "or" |
| Fluent | time-indexed symbol, for example $\mathit{HaveArrow}^t$ |
| Successor state axiom | $f^{t+1}\Leftrightarrow$ (made true at $t$) $\lor$ ($f^t\land$ not made false at $t$) |

## Check yourself
> [!question]- 1. $KB=\{A\Rightarrow B,\ B\}$. Does $KB\models A$?
> No. In the world $A$ false, $B$ true, both KB sentences are true and $A$ is false. One such row is enough.

> [!question]- 2. Classify: (a) $A\Rightarrow\neg A$, (b) $(A\Rightarrow B)\lor(B\Rightarrow A)$, (c) $A\Leftrightarrow\neg A$.
> (a) True for $A$ false, false for $A$ true: satisfiable, not valid. (b) True in all four worlds: valid. (c) False in both worlds: unsatisfiable.

> [!question]- 3. Rewrite $\neg(A\Rightarrow B)$ without $\Rightarrow$.
> $\neg(\neg A\lor B)\equiv(\neg\neg A\land\neg B)\equiv A\land\neg B$.

> [!question]- 4. KB: $A\land B\Rightarrow C$, $C\Rightarrow D$, $D\land E\Rightarrow F$, $A\Rightarrow E$, $A$, $B$. Run forward chaining for $F$ (first-in first-out agenda).
> Counts $2,1,2,1$. Pop $A$: counts $1,1,2,0$, push $E$. Pop $B$: rule 1 hits 0, push $C$. Pop $E$: rule 3 to 1. Pop $C$: push $D$. Pop $D$: rule 3 hits 0, push $F$. Order $A,B,E,C,D,F$. So $KB\models F$.

> [!question]- 5. KB: $P\Rightarrow Q$, $Q\Rightarrow R$, $P$. Prove $R$ by resolution.
> Clauses: (1) $\neg P\lor Q$, (2) $\neg Q\lor R$, (3) $P$, (4) $\neg R$ (negated query). (3)+(1) give $Q$. $Q$+(2) give $R$. $R$+(4) give $\square$. Entailed.

> [!question]- 6. $\mathit{HaveArrow}^0$ is true and the agent shoots at time 2 only. Find $\mathit{HaveArrow}^t$ for $t=1,\dots,4$.
> Apply $\mathit{HaveArrow}^{t+1}\Leftrightarrow(\mathit{HaveArrow}^t\land\neg\mathit{Shoot}^t)$: true, true, false, false.

## Links
- Course index: [[CMPE421 Home]]
- Previous lecture: [[CMPE421 L07 - MDPs, probability, Bayes nets, HMMs]]
- Next lecture: [[CMPE421 L09 - Predicate logic and planning]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_08_Logical_Agents.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_08/CMPE421_Lecture_08_Logical_Agents.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_08/CMPE421_Lecture_08_Logical_Agents.pdf`
- Original slides (PDF, stored in the lecture_07 folder): [logical-agents.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/logical-agents.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/logical-agents.pdf`
