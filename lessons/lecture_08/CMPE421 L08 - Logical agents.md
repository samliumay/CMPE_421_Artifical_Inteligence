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

> [!question]- Check (slide 1): A programmer replaces the list that stores the sentences by a hash table. The agent behaves as before. Which level changed? Which box of the figure holds the sentence "squares next to a pit are breezy"?
> Only the implementation level changed (data structures and algorithms). The knowledge level (what the agent knows) is the same, so every Ask gets the same answer.
> The sentence is domain-specific content, so it is in the knowledge base. The inference engine holds only domain-independent algorithms.

> [!question]- Check (slide 2): `KB-Agent` is called 5 times. How many Tell and Ask calls happen, what is $t$ at the end, and why does the agent Tell the KB its own action?
> Each call makes 2 Tells and 1 Ask. Five calls: $5\cdot2=10$ Tells and $5\cdot1=5$ Asks. The counter goes from $t=0$ to $t=5$.
> The KB must know what the agent did to keep its picture of the world correct. After Forward the agent is in a new square. After Shoot the arrow is gone. Without the action sentence, the next percept cannot be linked to the right square.

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

> [!question]- Check (slide 3, figure): In the figure, square $[2,3]$ shows Breeze, Stench and Gold. What causes each of the three? Which squares are breezy because of the pit in $[3,1]$?
> Breeze in $[2,3]$: the pit in $[3,3]$ is the right neighbour. Stench: the wumpus in $[1,3]$ is the left neighbour. Gold: it lies in $[2,3]$ itself, and glitter is perceived only in the gold square.
> The pit in $[3,1]$ has three neighbours: $[2,1]$, $[4,1]$ and $[3,2]$. All three show Breeze in the figure. Diagonal squares such as $[2,2]$ are not neighbours.

> [!question]- Check (slide 3, score): Agent X takes 14 actions in total, one of them is Shoot, and it grabs the gold and stays alive. Agent Y takes 3 actions and the third one moves it into a pit. Compute both scores.
> Agent X: $+1000$ for the gold, $-1$ for each of the 14 actions, $-10$ for the arrow. $1000-14-10=976$.
> Agent Y: $-1$ for each of the 3 actions and $-1000$ for death. $-3-1000=-1003$.

> [!question]- Check (slides 4-10): Find the two mistakes: "The wumpus world is fully observable because the agent has sensors, and it is episodic because every step costs $-1$." Which answer on slide 10 would change if the wumpus walked one square per time step?
> Mistake 1: the sensors are local. The agent perceives only its own square, so the world is partially observable.
> Mistake 2: episodic means that the current action does not affect later ones. Here one move decides where the agent is and what it can do next, so the world is sequential.
> A walking wumpus: "Static?" becomes no (the world changes while the agent thinks and acts). The wumpus would then also act more like a second agent than like a natural feature.

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

> [!info] Where 0.86 and 0.31 come from (beyond the slides)
> Three unknown squares: $(1,3)$, $(2,2)$, $(3,1)$. Each has a pit with probability $0.2$, independently. That gives $2^3=8$ worlds.
> The two breezes need a pit next to $(1,2)$ and a pit next to $(2,1)$: $(P_{1,3}\lor P_{2,2})\land(P_{2,2}\lor P_{3,1})$. Five worlds pass this test:
> - pit in $(2,2)$, anything in the other two squares (4 worlds): total weight $0.2$.
> - no pit in $(2,2)$, pits in both $(1,3)$ and $(3,1)$ (1 world): weight $0.8\cdot0.2\cdot0.2=0.032$.
> 
> The sum is $0.2+0.032=0.232$. Divide the weight of the worlds with the pit by this sum:
> $$\Pr[\text{pit in }(2,2)]=\frac{0.2}{0.232}\approx0.86$$
> $$\Pr[\text{pit in }(1,3)]=\frac{0.2\cdot0.2+0.032}{0.232}=\frac{0.072}{0.232}\approx0.31$$
> In the second line, $0.2\cdot0.2$ is "pit in $(1,3)$ and pit in $(2,2)$" and $0.032$ is the world without a pit in $(2,2)$. The same value holds for $(3,1)$ by symmetry. So the least dangerous move is to $(1,3)$ or $(3,1)$, not to $(2,2)$.

> [!question]- Check (slides 11-13): The agent perceives nothing in $[1,1]$, moves up to $[1,2]$ and feels a breeze (B). Why are $[1,2]$ and $[2,1]$ marked OK on slide 11? Why do the two "P?" marks on slide 13 go into $[1,3]$ and $[2,2]$ only?
> No breeze and no stench in $[1,1]$ means: no pit and no wumpus in any neighbour of $[1,1]$. The neighbours are $[1,2]$ and $[2,1]$, so both are OK.
> A breeze in $[1,2]$ means: at least one neighbour of $[1,2]$ has a pit. The neighbours are $[1,1]$, $[1,3]$ and $[2,2]$. The agent has been in $[1,1]$, so it has no pit. The pit is in $[1,3]$ or $[2,2]$ (or both). The line that joins the two "P?" marks says "at least one of these".

> [!question]- Check (slides 14-15): Change the story. In $[1,2]$ the agent perceives a stench and no breeze. In $[2,1]$ it perceives nothing. Where is the wumpus? Which of $[1,3]$, $[2,2]$, $[3,1]$ are provably safe?
> Stench in $[1,2]$: the wumpus is in $[1,3]$ or $[2,2]$ (the start square $[1,1]$ is safe).
> Nothing in $[2,1]$: no wumpus and no pit in its neighbours $[2,2]$ and $[3,1]$.
> So the wumpus is not in $[2,2]$. It must be in $[1,3]$.
> $[2,2]$ and $[3,1]$ are provably safe. $[1,3]$ is deadly. (No breeze in $[1,2]$ also says $[1,3]$ has no pit, but the wumpus is there.)
> This is the same pattern as on slides 14-15, with the roles of breeze and stench exchanged: a missing percept in one square removes one of two candidates from the other square.

> [!question]- Check (slides 16-18): On slide 17 the agent stands in $[2,2]$ and the slide marks $[2,3]$ and $[3,2]$ OK. What did the agent perceive in $[2,2]$? On slide 18 it perceives B, G and S in $[3,2]$. Explain each letter and say what the agent should do.
> In $[2,2]$ it perceived no breeze and no stench. So the unvisited neighbours $[2,3]$ and $[3,2]$ have no pit and no wumpus.
> S: the wumpus in $[3,1]$ is directly below $[3,2]$. This fits what the agent already knows.
> G: glitter, so the gold is in $[3,2]$.
> B: a pit is next to $[3,2]$. The neighbours of $[3,2]$ are $[2,2]$, $[3,1]$, $[3,3]$ and $[4,2]$. The known pit in $[1,3]$ is not one of them. $[2,2]$ is safe, and $[3,1]$ has no pit (no breeze was felt in $[2,1]$). So there is a new pit in $[3,3]$ or $[4,2]$ (or both).
> Action: Grab the gold and go back over the squares marked OK. Do not move to $[3,3]$ or $[4,2]$.

> [!question]- Check (slide 19, pits): Repeat the probability computation of the first tight spot for a cave where each square has a pit with probability $0.1$ instead of $0.2$. Find $\Pr[\text{pit in }(2,2)]$ and $\Pr[\text{pit in }(1,3)]$.
> Worlds that explain both breezes:
> - pit in $(2,2)$: weight $0.1$.
> - no pit in $(2,2)$, pits in $(1,3)$ and $(3,1)$: weight $0.9\cdot0.1\cdot0.1=0.009$.
> 
> Sum: $0.1+0.009=0.109$.
> $\Pr[\text{pit in }(2,2)]=0.1/0.109\approx0.92$.
> $\Pr[\text{pit in }(1,3)]=(0.1\cdot0.1+0.009)/0.109=0.019/0.109\approx0.17$.
> Rarer pits make the one-pit explanation $(2,2)$ even more likely than the two-pit explanation.

> [!question]- Check (slide 19, coercion): The agent smells the wumpus in $(1,1)$ and feels no breeze. It faces right, towards $(2,1)$, and shoots. It hears no scream. What does it know now, and where can it go?
> The stench says: the wumpus is in $(1,2)$ or $(2,1)$. The arrow flew through $(2,1)$ and hit nothing, so the wumpus is not in $(2,1)$. It is in $(1,2)$.
> No breeze in $(1,1)$: no pit in $(2,1)$. So $(2,1)$ is safe and the agent can move there. $(1,2)$ is deadly.
> With a scream the wumpus would be dead, and $(2,1)$ would be safe too. That is the idea of coercion: the action makes the square safe in both cases.

## 4. Logic, entailment and models (slides 20-22)
![[cmpe421_lecture_07_logical_agents_p020.jpg]]
![[cmpe421_lecture_07_logical_agents_p021.jpg]]
![[cmpe421_lecture_07_logical_agents_p022.jpg]]

**Notes.** A logic is a formal language. **Syntax** says which strings are sentences. **Semantics** says when a sentence is true in a world.

A possible world $m$ is a **model** of $\alpha$ if $\alpha$ is true in $m$. $M(\alpha)$ is the set of all models of $\alpha$.

$$KB\models\alpha\quad\Longleftrightarrow\quad M(KB)\subseteq M(\alpha)$$

Entailment goes one way. One world where the KB is true and $\alpha$ is false disproves it. To prove it, every model of the KB must be checked.

> [!question]- Check (slide 20): In the language of arithmetic, is "$\geq x\ y\ +$" a sentence? Is $x+2\geq y$ true in the world $x=3,\ y=5$? In the world $x=1,\ y=4$? Which of your answers used syntax and which used semantics?
> "$\geq x\ y\ +$" is not a sentence. The symbols are legal but the order breaks the grammar. This is a syntax answer.
> World $x=3,\ y=5$: $3+2=5\geq5$, true. World $x=1,\ y=4$: $1+2=3\geq4$, false. These are semantics answers: the same sentence gets a truth value in each world.

> [!question]- Check (slide 21): Decide and give a reason: (a) does $x+y=4$ entail $x=2$? (b) does "$x=2$ and $y=2$" entail $x+y=4$? (c) The KB says "Maryland beat UNC". Does it entail "Maryland beat UNC or Duke beat UNC"?
> (a) No. The world $x=1,\ y=3$ makes $x+y=4$ true and $x=2$ false. One such world is enough.
> (b) Yes. In every world with $x=2$ and $y=2$ the sum is $4$.
> (c) Yes. "Or" is inclusive: it is true when at least one part is true. In every world where Maryland beat UNC, the "or" sentence is true, whatever Duke did.

> [!question]- Check (slide 22): Use two symbols: $M$ = "Maryland beat UNC", $D$ = "Duke beat UNC". Write a world as $(M,D)$. List $M(KB)$ for $KB=M\land D$, and $M(\alpha)$ for $\alpha=M$ and for $\beta=(M\Leftrightarrow D)$. Which entailments hold between $KB$, $\alpha$ and $\beta$?
> There are $2^2=4$ worlds: $(T,T)$, $(T,F)$, $(F,T)$, $(F,F)$.
> $M(KB)=\{(T,T)\}$. $M(\alpha)=\{(T,T),(T,F)\}$. $M(\beta)=\{(T,T),(F,F)\}$.
> $M(KB)\subseteq M(\alpha)$ and $M(KB)\subseteq M(\beta)$, so $KB\models\alpha$ and $KB\models\beta$.
> $\alpha\not\models KB$: the world $(T,F)$ is in $M(\alpha)$ but not in $M(KB)$. Also $\alpha\not\models\beta$ (world $(T,F)$) and $\beta\not\models\alpha$ (world $(F,F)$).
> In the slide's figure this is the picture: the small set $M(KB)$ lies inside the larger set $M(\alpha)$. A stronger sentence has fewer models.

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

> [!question]- Check (slides 23-24): The three "?" squares are $[1,2]$, $[2,2]$ and $[3,1]$. (a) How many possible worlds would there be with 5 unknown squares? (b) Suppose the agent feels NO breeze in $[2,1]$ (and nothing in $[1,1]$). How many of the 8 worlds on slide 24 are models of this new KB?
> (a) Each unknown square has a pit or not: $2^5=32$ worlds.
> (b) No breeze in $[1,1]$: no pit in $[1,2]$. No breeze in $[2,1]$: no pit in $[2,2]$ and no pit in $[3,1]$. Only the world without any pit is left. The new KB has exactly 1 model, so all three squares are provably safe.

> [!question]- Check (slide 25): The KB (rules plus "nothing in $[1,1]$, breeze in $[2,1]$") has three models. Why is the world with no pit at all not a model? Why is the world with a pit only in $[1,2]$ not a model?
> No pit at all: then no square is breezy. But the agent observed a breeze in $[2,1]$. The observation is false in this world.
> Pit only in $[1,2]$: this pit is next to $[1,1]$, so $[1,1]$ would be breezy, but the agent felt nothing there. Also $[1,2]$ is not a neighbour of $[2,1]$, so the breeze in $[2,1]$ has no cause.
> The three models are the worlds without a pit in $[1,2]$ and with a pit in $[2,2]$, in $[3,1]$, or in both.

> [!question]- Check (slides 26-27): Use the three models of the KB. Is $\alpha_3$ = "there is a pit in $[3,1]$" entailed? Is $\neg\alpha_3$ entailed? Is $\alpha_4$ = "there is a pit in $[2,2]$ or in $[3,1]$" entailed?
> Write a model as (pit in $[2,2]$, pit in $[3,1]$). The models are $(F,T)$, $(T,F)$, $(T,T)$. $[1,2]$ has no pit in all of them.
> $\alpha_3$ is false in $(T,F)$: $KB\not\models\alpha_3$.
> $\neg\alpha_3$ is false in $(F,T)$ and $(T,T)$: $KB\not\models\neg\alpha_3$. The KB cannot decide this question.
> $\alpha_4$ is true in all three models: $KB\models\alpha_4$. The agent knows that a pit is near, but not where.

> [!question]- Check (slide 28): Procedure X answers "derived" for every sentence you give it. Procedure Y never derives anything. For each one: is it sound? Is it complete?
> X is complete: every entailed sentence is derived (because everything is). X is not sound: it also derives sentences that are not entailed, for example $P_{2,2}$ in the wumpus KB.
> Y is sound: it never derives a sentence that is not entailed (it derives nothing). Y is not complete: entailed sentences such as $\alpha_1$ are never derived.
> A useful procedure needs both. Truth table model checking has both, because the number of worlds is finite.

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

> [!question]- Check (slide 30): Which of these strings are sentences of propositional logic? (a) $P\land\lor Q$ (b) $\neg(P\Rightarrow Q)\Leftrightarrow R$ (c) $\neg\neg P$ (d) $P\Rightarrow$. For each sentence, say which rule of the slide is applied last.
> (a) Not a sentence: $\land$ needs a sentence on its right side, and $\lor Q$ is not one.
> (b) A sentence. Last rule: equivalence, with $S_1=\neg(P\Rightarrow Q)$ and $S_2=R$. Inside $S_1$: negation of an implication.
> (c) A sentence. Negation is applied twice: $P$ is a sentence, so $\neg P$ is one, so $\neg\neg P$ is one.
> (d) Not a sentence: the right side of $\Rightarrow$ is missing.

> [!question]- Check (slide 31): Take the world $P_{1,2}=\text{false}$, $P_{2,2}=\text{true}$, $P_{3,1}=\text{false}$. Evaluate $(P_{1,2}\Rightarrow P_{2,2})\land\neg(P_{2,2}\Leftrightarrow P_{3,1})$ step by step.
> $P_{1,2}\Rightarrow P_{2,2}$ is false $\Rightarrow$ true. The left side is false, so the implication is true.
> $P_{2,2}\Leftrightarrow P_{3,1}$ is true $\Leftrightarrow$ false. The two sides differ, so it is false.
> $\neg(\text{false})$ is true.
> Result: true $\land$ true $=$ true. The world is a model of the sentence.

> [!question]- Check (slides 32-33): (a) Write the breeze sentence for the corner square $[4,1]$. (b) The KB contains $R_1$ and $\neg B_{1,1}$. What follows about $P_{1,2}$ and $P_{2,1}$, and which direction of $\Leftrightarrow$ did you use? (c) The KB contains $R_2$, $B_{2,1}$ and $\neg P_{1,1}$. What follows?
> (a) The neighbours of $[4,1]$ are $[3,1]$ and $[4,2]$: $B_{4,1}\Leftrightarrow(P_{3,1}\lor P_{4,2})$.
> (b) Direction $(P_{1,2}\lor P_{2,1})\Rightarrow B_{1,1}$. $B_{1,1}$ is false, so the left side must be false: $\neg P_{1,2}$ and $\neg P_{2,1}$.
> (c) Direction $B_{2,1}\Rightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$. $B_{2,1}$ is true, so the "or" is true. With $\neg P_{1,1}$: $P_{2,2}\lor P_{3,1}$.
> Both directions are used, one for each percept. This is why the slide writes "if and only if".

## 7. Truth tables for inference (slides 34-35)
![[cmpe421_lecture_07_logical_agents_p034.jpg]]
![[cmpe421_lecture_07_logical_agents_p035.jpg]]

**Notes.** One row per world. $KB\models\alpha$ exactly when no row has the KB true and $\alpha$ false. The algorithm `TT-Entails` enumerates all rows depth-first. It is sound and complete, but takes $O(2^n)$ time for $n$ symbols (the problem is co-NP-complete).

> [!warning] Error on slide 34
> The slide lists only four facts ($\neg B_{1,1}$, $B_{2,1}$, $\neg P_{1,1}$, $\neg P_{2,1}$) above the table. Its KB column also uses $R_1$ and $R_2$.

> [!question]- Check (slide 34): (a) How many rows does the full table on the slide have? (b) Read the three rows where the KB is true. Is $\alpha=P_{2,2}\lor P_{3,1}$ entailed? Is $P_{2,2}$ entailed? (c) How many rows would a table need with one $P$ symbol and one $B$ symbol for each of the 16 squares?
> (a) 7 symbols: $2^7=128$ rows.
> (b) In the three KB rows, $(P_{2,2},P_{3,1})$ is (false, true), (true, false), (true, true). $P_{2,2}\lor P_{3,1}$ is true in all three: entailed. $P_{2,2}$ is false in the first of them: not entailed.
> (c) $16+16=32$ symbols: $2^{32}=4\,294\,967\,296$ rows. This is why enumeration does not scale.

> [!question]- Check (slide 35): In `TT-Check-All`, why does the line "else return true" return true for a world where the KB is false? How many complete worlds does the algorithm check for $n=4$ symbols, and how many calls of `TT-Check-All` are made in total?
> Entailment only speaks about worlds where the KB is true. A world where the KB is false cannot be a counterexample, so it must not make the final "and" false. Returning true means "this world is no problem".
> Complete worlds (symbols empty): $2^4=16$.
> Calls: one at depth 0, two at depth 1, and so on down to depth 4. $1+2+4+8+16=2^5-1=31$.

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

> [!info] DPLL in three ideas (beyond the slides)
> Slide 36 only names Davis-Putnam-Logemann-Loveland (DPLL). It is a depth-first search for a model of a set of CNF clauses (a clause is an "or" of literals, see section 11). It is faster than the truth table because of three ideas:
> - **Early termination**: a clause is true as soon as one of its literals is true. The whole set is false as soon as one clause has all its literals false. The other symbols need no value.
> - **Pure symbol**: a symbol that has the same sign in every clause where it appears. Give it the value that makes its literals true. This can never make a clause false.
> - **Unit clause**: a clause in which all literals but one are already false. The last literal must be true. Setting it can create new unit clauses (unit propagation).
> 
> If none of these applies, DPLL picks a symbol, tries true, and on failure tries false (backtracking). To test $KB\models\alpha$, run it on the clauses of $KB\land\neg\alpha$: no model means entailed.

> [!question]- Check (slide 36): Clause set: $\neg C$, $\ \neg A\lor C$, $\ A\lor B$, $\ B\lor D$. (a) Which symbols are pure at the start? (b) Start with the unit clause and follow the unit propagation. Give a model. (c) How many rows would a truth table need?
> (a) $B$ appears only as $B$, and $D$ only as $D$: both are pure. $A$ and $C$ appear with both signs, so they are not pure.
> (b) $\neg C$ is a unit clause: $C=\text{false}$.
> Then $\neg A\lor C$ has only $\neg A$ left: $A=\text{false}$.
> Then $A\lor B$ has only $B$ left: $B=\text{true}$.
> $B\lor D$ is now true. $D$ is free. It is pure, so set $D=\text{true}$.
> Model: $A=\text{false}$, $B=\text{true}$, $C=\text{false}$, $D=\text{true}$. All four clauses are true in it. No guess and no backtracking was needed.
> (c) 4 symbols: $2^4=16$ rows.

> [!question]- Check (slide 37): Use a truth table. (a) Refute: $(A\Rightarrow B)\equiv(B\Rightarrow A)$. (b) Prove: $\neg(A\land B)\equiv(A\Rightarrow\neg B)$.
> (a) One row with different values is enough. $A$ true, $B$ false: $A\Rightarrow B$ is false, $B\Rightarrow A$ is true. Not equivalent. (The correct law is contraposition: $\neg B\Rightarrow\neg A$.)
> (b) All four rows:
> 
> | $A$ | $B$ | $\neg(A\land B)$ | $A\Rightarrow\neg B$ |
> |---|---|---|---|
> | T | T | F | F |
> | T | F | T | T |
> | F | T | T | T |
> | F | F | T | T |
> 
> The two columns are equal in every row, so the sentences are equivalent. The same result without a table: $A\Rightarrow\neg B\equiv\neg A\lor\neg B$ (implication elimination) $\equiv\neg(A\land B)$ (De Morgan).

> [!question]- Check (slide 38): $KB=\{A\Rightarrow B,\ \neg B\}$ and $\alpha=\neg A$. Show $KB\models\alpha$ with the second connection on the slide (unsatisfiability). Then classify $(A\Rightarrow B)\Rightarrow A$ as valid, satisfiable or unsatisfiable.
> Test $KB\land\neg\alpha=(A\Rightarrow B)\land\neg B\land A$ in all four worlds:
> - $A$ T, $B$ T: $\neg B$ is false.
> - $A$ T, $B$ F: $A\Rightarrow B$ is false.
> - $A$ F, $B$ T: $A$ is false (and $\neg B$ too).
> - $A$ F, $B$ F: $A$ is false.
> 
> No world makes it true: unsatisfiable. So $KB\models\neg A$. (This inference is called Modus Tollens.)
> $(A\Rightarrow B)\Rightarrow A$: for $A$ true it is true (right side true). For $A$ false, $A\Rightarrow B$ is true and the right side is false, so it is false. True in some worlds and false in others: satisfiable, not valid.

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

> [!question]- Check (slide 39): Which of these are Horn clauses in the sense of the slide? (a) $A\land B\Rightarrow C$ (b) $A\Rightarrow B\lor C$ (c) $D$ (d) $\neg A\Rightarrow B$ (e) $\neg A\lor\neg B\lor C$. Then: from $C$ and $A\land B\Rightarrow C$, can Modus Ponens derive $A$?
> (a) Yes: conjunction of symbols $\Rightarrow$ symbol.
> (b) No: the right side is an "or" of two symbols.
> (c) Yes: a single symbol (a fact).
> (d) No: the premise is a negated symbol. It is equivalent to $A\lor B$, which the slide names as not Horn.
> (e) Yes: by implication elimination and De Morgan it is the same sentence as (a). As a clause, a Horn rule has exactly one positive literal. Such a clause is called a definite clause (beyond the slides).
> Modus Ponens goes from the premises to the head only. From $C$ and the rule nothing follows about $A$: the world $A$ false, $B$ false, $C$ true makes both true.

> [!question]- Check (slide 40): Read the graph. What does the small arc that joins two edges mean? Which symbol has two different ways to be derived? What is the long curved line from $P$ down to the lower left dot?
> An arc joins the premises of one rule: all of them are needed (AND). Example: the dot below $P$ joins $L$ and $M$, for $L\land M\Rightarrow P$.
> $L$ has two incoming dots, one for $A\land P\Rightarrow L$ and one for $A\land B\Rightarrow L$. One of the two rules is enough (OR).
> The curved line carries $P$ to the dot of $A\land P\Rightarrow L$: $P$ is a premise of that rule. This line makes the cycle $P\to L\to P$.

> [!question]- Check (slide 41, code): What do `count[c]`, `agenda` and `inferred[p]` store? Use the KB of slide 40 to show what could go wrong without the test "unless `inferred[p]`".
> `count[c]`: the number of premises of rule $c$ that are not yet known. `agenda`: symbols known to be true but not yet processed. `inferred[p]`: true once $p$ has been popped and processed.
> In the slide's KB, $L$ is pushed twice (by $A\land B\Rightarrow L$ and later by $A\land P\Rightarrow L$). Without the test, the second pop of $L$ would decrement the counts of $L\land M\Rightarrow P$ and $B\land L\Rightarrow M$ a second time. A count would then say "one more premise is known" although no new premise was proved. In another KB this can bring a count to 0 while a premise is still missing, and the algorithm would derive a symbol that is not entailed (not sound). The test also saves work: every symbol is processed only once.

> [!question]- Check (slide 41, trace): KB: $R_1: A\Rightarrow B$, $R_2: B\land C\Rightarrow D$, $R_3: A\land D\Rightarrow E$, $R_4: E\land F\Rightarrow G$, facts $A$, $C$. Run the algorithm (first-in first-out agenda) for the query $E$. Then decide: does $KB\models G$?
> Counts start at $(1,2,2,2)$ for $(R_1,R_2,R_3,R_4)$. Agenda $\langle A,C\rangle$.
> 1. Pop $A$: $R_1\to0$, push $B$. $R_3\to1$. Counts $(0,2,1,2)$, agenda $\langle C,B\rangle$.
> 2. Pop $C$: $R_2\to1$. Counts $(0,1,1,2)$, agenda $\langle B\rangle$.
> 3. Pop $B$: $R_2\to0$, push $D$. Counts $(0,0,1,2)$, agenda $\langle D\rangle$.
> 4. Pop $D$: $R_3\to0$. Its head is $E$, the query: return true.
> 
> So $KB\models E$.
> For the query $G$ the run goes on: push $E$, pop $E$: $R_4\to1$. The agenda is empty and $R_4$ still misses $F$. Return false: $KB\not\models G$. No rule has head $F$ and $F$ is not a fact.

> [!question]- Check (slides 42-44): Slide 42 shows counts $1,2,2,2,2$ and agenda $\langle A,B\rangle$. Slide 43 shows $1,2,2,1,1$ and agenda $\langle B\rangle$. Explain the change. Then predict the counts and the agenda after $B$ is popped (slide 44).
> From 42 to 43, $A$ was popped. $A$ is a premise of $A\land P\Rightarrow L$ and of $A\land B\Rightarrow L$, so both counts drop from 2 to 1. No count is 0, so nothing is pushed.
> Pop $B$: $B$ is a premise of $B\land L\Rightarrow M$ ($2\to1$) and of $A\land B\Rightarrow L$ ($1\to0$). The second rule fires: push $L$.
> Prediction: counts $1,2,1,1,0$ and agenda $\langle L\rangle$.

> [!question]- Check (slides 45-47): Slide 45 shows counts $1,1,0,1,0$ and agenda $\langle M\rangle$. Follow the next two pops. Why is $L$ on the agenda again on slide 47, and what happens when it is popped?
> Pop $M$: $L\land M\Rightarrow P$ goes $1\to0$, push $P$. Agenda $\langle P\rangle$ (slide 46).
> Pop $P$: $P\Rightarrow Q$ goes $1\to0$ and $A\land P\Rightarrow L$ goes $1\to0$. Their heads $L$ and $Q$ are pushed. All counts are 0, and the slide shows the agenda $\langle L,Q\rangle$ (slide 47).
> $L$ is there a second time because a second rule for $L$ fired. When it is popped, `inferred[L]` is already true, so nothing is done. Then $Q$ is popped.

> [!question]- Check (slides 48-49): On slide 49 the agenda is empty and all symbols are inferred. Now remove the fact $B$ from the KB and run forward chaining again. Which symbols are inferred? Is $Q$ entailed?
> Agenda $\langle A\rangle$. Pop $A$: $A\land P\Rightarrow L$ goes $2\to1$ and $A\land B\Rightarrow L$ goes $2\to1$. No count reaches 0, the agenda is empty.
> Only $A$ is inferred. $L$ needs $B$ (never known) or $P$, and $P$ needs $L$ and $M$: the cycle gives nothing.
> The algorithm returns false, so $KB\not\models Q$.

> [!question]- Check (slide 50): Use the proof idea on the KB of the last question (slide 40's rules, only the fact $A$). Forward chaining stops with only $A$ inferred. Build the world $m$ of step 1 and check that every clause of the KB is true in it. What does this show about $Q$?
> $m$: $A$ true, and $B$, $L$, $M$, $P$, $Q$ false.
> - $P\Rightarrow Q$: false $\Rightarrow$ false, true.
> - $L\land M\Rightarrow P$: premise false, true.
> - $B\land L\Rightarrow M$: premise false, true.
> - $A\land P\Rightarrow L$: premise false ($P$ is false), true.
> - $A\land B\Rightarrow L$: premise false ($B$ is false), true.
> - $A$: true.
> 
> So $m$ is a model of the KB, and $Q$ is false in $m$. A model of the KB with $Q$ false means $KB\not\models Q$. This is step 5 read backwards: whatever forward chaining does not derive is false in the model $m$, so it is not entailed.

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

> [!question]- Check (slide 51): A KB has only the rules $L\land M\Rightarrow P$ and $A\land P\Rightarrow L$ and the facts $A$, $M$. Backward chaining gets the query $P$. What happens without the check "is the goal already on the recursion stack"? What happens with it?
> Without the check: $P$ needs $L$ (and $M$). $L$ needs $A$ and $P$. $P$ needs $L$ again, and so on. The recursion never ends.
> With the check: when $P$ comes up as a subgoal of $L$, it is already on the stack, so this rule fails. $L$ has no other rule and is not a fact, so $L$ fails. Then $P$ fails. The answer "false" is correct: forward chaining on this KB derives only $A$ and $M$.

> [!question]- Check (slides 52-54): The query is $Q$. Write the chain of goals that backward chaining opens on these three slides, and name the rule used at each step. Which goals are open on slide 54?
> Goal $Q$: not a fact. Rule $P\Rightarrow Q$ has head $Q$, new goal $P$.
> Goal $P$: rule $L\land M\Rightarrow P$, new goals $L$ and $M$.
> On slide 54 the green circles are $Q$, $P$ (waiting for their premises) and $L$, $M$ (open). The rules $P\Rightarrow Q$ and $L\land M\Rightarrow P$ are printed in bold because they are in use. Nothing is proved yet. $A$ and $B$ are known facts (red).

> [!question]- Check (slides 55-57): Delete the rule $A\land B\Rightarrow L$ from the KB. What does backward chaining answer for $Q$ now? Is the answer correct?
> Goal $L$ has only the rule $A\land P\Rightarrow L$ left. $A$ is a fact. $P$ is already on the stack (slide 55), so the rule fails. No other rule: $L$ fails. Then $P$ fails and $Q$ fails. Answer: false.
> It is correct. Forward chaining on this KB pops $A$ and $B$ and then stops: no count reaches 0. So $Q$ is not entailed.
> In the original KB the second rule saves the proof (slide 57): $A$ and $B$ are facts, so $L$ is proved.

> [!question]- Check (slides 58-60): After $L$ is proved (slide 58), the goal $M$ is still open. Which rule proves $M$, and which of its premises need new work? How does the slide's advice "avoid repeated work" help here?
> Rule $B\land L\Rightarrow M$. $B$ is a fact. $L$ was proved a moment ago for the goal $P$.
> Backward chaining remembers proved goals. So $L$ is not proved a second time (no second try of $A\land P\Rightarrow L$ and $A\land B\Rightarrow L$). $M$ is proved at once (slide 60).

> [!question]- Check (slides 61-62): On slide 62 every rule has a check mark except $A\land P\Rightarrow L$. Now change the query to $M$. Backward chaining tries the rules for a goal in the order of the slide's list. Which symbols does it prove, and which symbols of the KB are never proved?
> Goal $M$: rule $B\land L\Rightarrow M$. $B$ is a fact. Goal $L$: the rule $A\land P\Rightarrow L$ opens the subgoal $P$, which needs $L$ (on the stack) and fails. The rule $A\land B\Rightarrow L$ succeeds with the facts $A$, $B$.
> Proved: $L$, then $M$. $P$ was tried once and failed in that try. $Q$ was never touched.
> Forward chaining would derive $A,B,L$ and then stop at $M$ if $M$ is the query. For the query $Q$ on slide 62 both methods prove the same symbols $L$, $M$, $P$, $Q$.

> [!question]- Check (slide 63): KB: $A\Rightarrow B$, $\ B\land C\Rightarrow D$, $\ A\land D\Rightarrow E$, $\ C\Rightarrow H$, $\ H\Rightarrow I$, $\ I\Rightarrow J$, facts $A$, $C$. The query is $B$. Count the work of backward chaining. Then count how many symbols forward chaining infers if it runs until the agenda is empty.
> Backward chaining: goal $B$, rule $A\Rightarrow B$, subgoal $A$ is a fact. Done: 2 goals and 1 rule. The other five rules are never looked at.
> Forward chaining until the agenda is empty (first-in first-out) infers $A, C, B, H, D, I, E, J$: 8 symbols. Six of them ($C,H,D,I,E,J$) are irrelevant to the query $B$.
> This is the slide's point: forward chaining is data-driven and may do much irrelevant work. Backward chaining is goal-driven and its cost can be much less than linear in the size of the KB.

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

> [!question]- Check (slide 64): Resolve each pair, or say why it cannot be resolved. (a) $P\lor Q\lor\neg R$ and $R\lor S$ (b) $X\lor\neg Y$ and $Y\lor Z$, then the result with $\neg X$ (c) $A\lor B$ and $A\lor C$.
> (a) The complementary pair is $\neg R$ and $R$. The rest is joined: $P\lor Q\lor S$.
> (b) Pair $\neg Y$ and $Y$: resolvent $X\lor Z$. Then $X\lor Z$ with $\neg X$: pair $X$ and $\neg X$, resolvent $Z$.
> (c) No resolution. No symbol appears positive in one clause and negative in the other.
> The wumpus example on the slide is the same step: $P_{1,3}\lor P_{2,2}$ and $\neg P_{2,2}$ give $P_{1,3}$.

> [!question]- Check (slide 65): Take the clauses $\neg P\lor Q$ and $\neg Q\lor R$. Resolve them. Then rewrite both clauses and the resolvent as implications. Which familiar reasoning step do you see? Also write $A\lor\neg B$ as an implication in two ways.
> Resolve on $Q$ and $\neg Q$: resolvent $\neg P\lor R$.
> As implications: $P\Rightarrow Q$ and $Q\Rightarrow R$ give $P\Rightarrow R$. This is chaining two rules, the step the slide calls Modus Ponens.
> $A\lor\neg B\equiv(B\Rightarrow A)$, and by contraposition also $(\neg A\Rightarrow\neg B)$. The slide uses the second form.

> [!question]- Check (slide 66): Convert $(A\Rightarrow B)\Leftrightarrow C$ to CNF with the four steps of the slide.
> 1. Eliminate $\Leftrightarrow$: $((A\Rightarrow B)\Rightarrow C)\land(C\Rightarrow(A\Rightarrow B))$
> 2. Eliminate $\Rightarrow$: $(\neg(\neg A\lor B)\lor C)\land(\neg C\lor\neg A\lor B)$
> 3. Move $\neg$ inwards (De Morgan, double negation): $((A\land\neg B)\lor C)\land(\neg C\lor\neg A\lor B)$
> 4. Distribute $\lor$ over $\land$: $(A\lor C)\land(\neg B\lor C)\land(\neg A\lor B\lor\neg C)$
> 
> Result: three clauses. Quick test in the world $A$ true, $B$ false, $C$ false: the original is (false $\Leftrightarrow$ false) $=$ true. The clauses are $A\lor C$ true, $\neg B\lor C$ true, $\neg A\lor B\lor\neg C$ true. Same value. A full truth table (8 rows) gives the same value in every row.

> [!question]- Check (slide 67): Run `PL-Resolution` for $KB=\{A\lor B\}$ and $\alpha=A$. Which line ends the algorithm, and what does it return? Why must the loop always end?
> Clauses of $KB\land\neg\alpha$: $A\lor B$ and $\neg A$.
> Round 1: the only pair resolves on $A$: resolvent $B$. No empty clause. `new` $=\{B\}$ is not a subset of `clauses`, so add it: $\{A\lor B,\ \neg A,\ B\}$.
> Round 2: $B$ with $A\lor B$: no complementary pair. $B$ with $\neg A$: none. The first pair gives $B$ again. `new` $=\{B\}\subseteq$ `clauses`: return false.
> So $KB\not\models A$. Counterexample world: $A$ false, $B$ true.
> The loop must end because only finitely many different clauses can be built from a finite set of symbols. Each round either adds a new clause or stops.

> [!question]- Check (slide 68): KB: $B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$, $\ B_{2,1}$, $\ \neg P_{1,1}$, $\ \neg P_{2,2}$. Prove $\alpha=P_{3,1}$ by resolution refutation.
> CNF of the biconditional (same pattern as slide 66): $\neg B_{2,1}\lor P_{1,1}\lor P_{2,2}\lor P_{3,1}$, and $\neg P_{1,1}\lor B_{2,1}$, $\neg P_{2,2}\lor B_{2,1}$, $\neg P_{3,1}\lor B_{2,1}$.
> Add the unit clauses $B_{2,1}$, $\neg P_{1,1}$, $\neg P_{2,2}$ and the negated query $\neg P_{3,1}$.
> 1. $\neg B_{2,1}\lor P_{1,1}\lor P_{2,2}\lor P_{3,1}$ with $B_{2,1}$: $\ P_{1,1}\lor P_{2,2}\lor P_{3,1}$
> 2. with $\neg P_{1,1}$: $\ P_{2,2}\lor P_{3,1}$
> 3. with $\neg P_{2,2}$: $\ P_{3,1}$
> 4. with $\neg P_{3,1}$: $\ \square$
> 
> The empty clause is derived, so $KB\models P_{3,1}$: breeze in $[2,1]$ and no pit in the other two neighbours forces the pit into $[3,1]$.

> [!question]- Check (slide 69): The top row of the figure holds the five clauses of slide 68. (a) The figure reaches $\square$ from $P_{1,2}$ and $\neg P_{1,2}$. Where does $\neg P_{1,2}$ come from? (b) The second row contains $\neg B_{1,1}\lor P_{1,2}\lor B_{1,1}$. Why is such a clause useless?
> (a) $\neg P_{1,2}$ is the resolvent of $\neg P_{1,2}\lor B_{1,1}$ and $\neg B_{1,1}$. So one proof is: first $\neg P_{1,2}$, then with $P_{1,2}$ the empty clause. The note's example takes another order ($B_{1,1}$ first, then with $\neg B_{1,1}$). Both are refutations with two steps.
> (b) It contains $B_{1,1}$ and $\neg B_{1,1}$, so it is true in every world (a tautology). A clause that is always true says nothing and cannot help to reach a contradiction. An implementation can throw such clauses away.

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

> [!question]- Check (slide 70): Write the breeze axiom for $[2,3]$ and the stench axiom for $[4,1]$. How many breeze axioms does the $4\times4$ cave need in total?
> Neighbours of $[2,3]$: $[1,3]$, $[3,3]$, $[2,2]$, $[2,4]$. Axiom: $B_{2,3}\Leftrightarrow(P_{1,3}\lor P_{3,3}\lor P_{2,2}\lor P_{2,4})$.
> Neighbours of the corner $[4,1]$: $[3,1]$ and $[4,2]$. Axiom: $S_{4,1}\Leftrightarrow(W_{3,1}\lor W_{4,2})$.
> One breeze axiom per square: 16. The same number of stench axioms.

> [!question]- Check (slide 71): What does the clause $\neg W_{1,1}\lor\neg W_{1,2}$ say? How many clauses of this kind does the $4\times4$ cave need, and how many would a $3\times3$ cave need? Why is the first long clause also needed?
> It is false only when $W_{1,1}$ and $W_{1,2}$ are both true. So it says: the wumpus is not in both squares.
> One clause for each pair of squares. $4\times4$: $\binom{16}{2}=\frac{16\cdot15}{2}=120$. $3\times3$: $\binom{9}{2}=\frac{9\cdot8}{2}=36$.
> The pair clauses say "at most one wumpus". They are all true in a world without any wumpus. The long clause $W_{1,1}\lor\dots\lor W_{4,4}$ says "at least one". Together: exactly one.

> [!question]- Check (slide 72): Which of these need a time index (are fluents): the pit in $[3,1]$, the agent's location, the stench percept, the wumpus's square, "the wumpus is alive", "the agent has the arrow"? How many location symbols $L^t_{x,y}$ exist for the $4\times4$ cave and $t=0,\dots,10$?
> Fluents: the agent's location, the stench percept (it depends on where the agent is at that time), "the wumpus is alive", "the agent has the arrow".
> Not fluents: the pit in $[3,1]$ and the wumpus's square. They never change, so $P_{3,1}$ and $W_{x,y}$ have no time index.
> Location symbols: 16 squares times 11 time points $=176$.

> [!question]- Check (slide 73): At time 3 the agent is in $[2,1]$ and feels a breeze. Which axiom of the slide's pattern is used, and what does the KB derive? Why is the premise $L^3_{2,1}$ needed?
> Axiom: $L^3_{2,1}\Rightarrow(\mathit{Breeze}^3\Leftrightarrow B_{2,1})$.
> $L^3_{2,1}$ is true, so $\mathit{Breeze}^3\Leftrightarrow B_{2,1}$ holds. $\mathit{Breeze}^3$ is true, so $B_{2,1}$ is derived. Now the breeze axiom of slide 70 for $B_{2,1}$ can be used.
> The percept $\mathit{Breeze}^3$ does not say where the breeze is. The location premise links it to one square. Without it the percept would make every square breezy.

> [!question]- Check (slides 74-75): The KB contains $\mathit{HaveArrow}^0$, $L^0_{1,1}$, $\mathit{East}^0$, the action Forward at time 0, and the first effect axiom of slide 74. What can the KB derive about time 1? Can it derive $\mathit{HaveArrow}^1$? Then name the problem: (a) "Grab fails if the gold is glued to the floor." (b) "Grab makes a noise."
> The premise of the effect axiom is true, so $L^1_{2,1}$ and $\neg L^1_{1,1}$ are derived.
> $\mathit{HaveArrow}^1$ cannot be derived. No axiom says that Forward leaves the arrow alone. This is the frame problem: effect axioms describe what changes, not what stays the same.
> (a) Qualification problem: a caveat under which the action does not work. (b) Ramification problem: a secondary consequence of the action.

> [!question]- Check (slide 76): Use the location axiom with the brackets corrected: $L^{t+1}_{1,1}\Leftrightarrow(L^t_{1,1}\land(\neg\mathit{Forward}^t\lor\mathit{Bump}^{t+1}))\lor(L^t_{1,2}\land\mathit{South}^t\land\mathit{Forward}^t)\lor(L^t_{2,1}\land\mathit{West}^t\land\mathit{Forward}^t)$. Is the agent in $[1,1]$ at $t+1$ in these cases? (a) In $[1,1]$, facing west, Forward, bump. (b) In $[1,2]$, facing south, Forward. (c) In $[1,1]$, facing east, Forward, no bump.
> (a) First part: $L^t_{1,1}$ true, and $\neg\mathit{Forward}^t\lor\mathit{Bump}^{t+1}$ is false $\lor$ true $=$ true. The right side is true: the agent stays in $[1,1]$.
> (b) Second part: $L^t_{1,2}$, $\mathit{South}^t$ and $\mathit{Forward}^t$ are all true. True: the agent arrives in $[1,1]$.
> (c) First part: $\neg\mathit{Forward}^t\lor\mathit{Bump}^{t+1}$ is false $\lor$ false $=$ false. Second and third part: the agent is not in $[1,2]$ or $[2,1]$, false. The right side is false, so $L^{t+1}_{1,1}$ is false: the agent has left $[1,1]$.
> Because of $\Leftrightarrow$, the axiom gives the answer "no" in case (c) too. An effect axiom alone could not do that.

> [!question]- Check (slide 77): $P_{2,2}$ is false, $W_{2,2}$ is true and $\mathit{WumpusAlive}^3$ is false. Evaluate $\mathit{OK}^3_{2,2}$. Why does $\mathit{OK}$ have a time index although $P$ and $W$ have none?
> $\mathit{OK}^3_{2,2}\Leftrightarrow\neg P_{2,2}\land(\neg W_{2,2}\lor\neg\mathit{WumpusAlive}^3)$.
> $\neg P_{2,2}$ is true. $\neg W_{2,2}$ is false, $\neg\mathit{WumpusAlive}^3$ is true, so the bracket is false $\lor$ true $=$ true. Result: true $\land$ true $=$ true. The square is OK at time 3: the wumpus is there, but dead.
> Pits and the wumpus's square never change. Whether the wumpus is alive can change (after a shot). So a square can be not OK at time 1 and OK at time 3.

> [!question]- Check (slide 78): Follow the percept and action list. Assume the agent starts in $[1,1]$ facing up (north). The slide does not say this, but the figure shows it. Give the agent's square at each time $0,\dots,6$. Then explain why $P_{1,3}$, $W_{3,1}$ and $\mathit{OK}^6_{2,2}$ can be derived.
> - $t=0$: $[1,1]$, no percept. Forward.
> - $t=1$: $[1,2]$, breeze. Turn right (now facing east).
> - $t=2$: $[1,2]$, breeze. Turn right (now facing south).
> - $t=3$: $[1,2]$, breeze. Forward.
> - $t=4$: $[1,1]$. Turn left (now facing east).
> - $t=5$: $[1,1]$. Forward.
> - $t=6$: $[2,1]$, stench. So $L^6_{2,1}$ is true.
> 
> $P_{1,3}$: breeze in $[1,2]$ gives $P_{1,1}\lor P_{1,3}\lor P_{2,2}$. $[1,1]$ has no pit. No breeze in $[2,1]$ gives $\neg P_{2,2}$. So $P_{1,3}$.
> $W_{3,1}$: stench in $[2,1]$ gives $W_{1,1}\lor W_{2,2}\lor W_{3,1}$. No stench in $[1,2]$ gives $\neg W_{2,2}$, and the start square has no wumpus. So $W_{3,1}$.
> $\mathit{OK}^6_{2,2}$: $\neg P_{2,2}$ and $\neg W_{2,2}$ are both known, so the axiom of slide 77 gives true.

## 13. Summary (slide 79)
![[cmpe421_lecture_07_logical_agents_p079.jpg]]

> [!question]- Check (slide 79): The summary names three inference methods. For each one, say for which KBs it is complete and what it costs: truth table enumeration, forward or backward chaining, resolution. Then: the KB is $A\lor B$, $\neg A$. Which of the three can derive $B$?
> Truth table enumeration: sound and complete for all of propositional logic, $2^n$ rows for $n$ symbols.
> Forward and backward chaining (Modus Ponens): complete only for Horn KBs, linear time.
> Resolution: sound and complete for all of propositional logic (after conversion to CNF). In the worst case it also needs exponential time.
> $A\lor B$ is not a Horn clause, so chaining cannot be used on this KB. A truth table works: the only model is $A$ false, $B$ true, and $B$ is true in it. Resolution works in one step: $A\lor B$ with $\neg A$ gives $B$. As a refutation: add $\neg B$, get $B$, then $\square$.

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
