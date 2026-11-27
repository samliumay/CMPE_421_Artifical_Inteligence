---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L08 - Logical agents
type: quiz
questions: 54
---

# CMPE421 L08 - Logical agents - quiz

Lesson note: [[CMPE421 L08 - Logical agents]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Knowledge-based agents (slides 1-2)

### Q1 (slide 1)

A knowledge-based agent has two boxes: the knowledge base (KB) and the inference engine. A programmer replaces the list that stores the sentences by a hash table, and the agent behaves exactly as before. Which level changed, and which box holds the sentence "squares next to a pit are breezy"?

- A) The knowledge level changed. The sentence is in the inference engine.
- B) The implementation level changed. The sentence is in the inference engine.
- C) The implementation level changed. The sentence is in the knowledge base.
- D) The knowledge level changed. The sentence is in the knowledge base.

> [!success]- Answer
> **C.** A hash table is a data structure, so only the implementation level changed. What the agent knows (the knowledge level) is the same, so every Ask gets the same answer.
> The sentence is domain-specific content, so it belongs to the knowledge base. The inference engine holds only domain-independent algorithms.
> - A: This mixes up both: the knowledge did not change, and the engine holds no domain facts.
> - B: The level is right, but a fact about pits is domain content, not an algorithm.
> - D: The box is right, but the agent knows the same things as before. Only the storage changed.

### Q2 (slide 2)

Each call of `KB-Agent` does `Tell(KB, percept)`, `Ask(KB, action?)`, `Tell(KB, action)` and then $t\leftarrow t+1$. The counter starts at $t=0$. The agent is called 5 times. How many Tell calls and Ask calls happen, and what is $t$ at the end?

- A) 10 Tells, 5 Asks, $t=5$.
- B) 5 Tells, 5 Asks, $t=4$.
- C) 10 Tells, 10 Asks, $t=4$.
- D) 5 Tells, 10 Asks, $t=5$.

> [!success]- Answer
> **A.** One call makes 2 Tells and 1 Ask. Five calls: $5\cdot2=10$ Tells and $5\cdot1=5$ Asks. The counter is raised once per call: $0\to5$.
> The second Tell (the action) keeps the KB's picture of the world right. After Forward the agent is in a new square.
> - B: This forgets the Tell of the action, and it gives the value of $t$ during the fifth call, before the last increment.
> - C: This counts one Ask per Tell, and it uses $t$ before the last increment.
> - D: This swaps the Tell and Ask counts.

## 2. The Wumpus world (slides 3-10)

### Q3 (slide 3, figure)

![[cmpe421_lecture_07_logical_agents_p003.jpg]]

A square is written $[x,y]$: $x$ is the column, $y$ is the row. Neighbours share a side. The Breeze in $[2,3]$ comes from the pit in $[3,3]$. Which square is breezy because of both the pit in $[3,1]$ and the pit in $[3,3]$?

- A) $[2,1]$
- B) $[4,3]$
- C) $[4,1]$
- D) $[3,2]$

> [!success]- Answer
> **D.** Neighbours of $[3,1]$: $[2,1]$, $[4,1]$, $[3,2]$. Neighbours of $[3,3]$: $[2,3]$, $[4,3]$, $[3,2]$, $[3,4]$. The only square in both lists is $[3,2]$.
> In $[2,3]$ the Breeze comes from $[3,3]$ (right), the Stench from the wumpus in $[1,3]$ (left), and the Gold lies in $[2,3]$ itself.
> All four options show Breeze in the figure, so the figure alone does not decide. The neighbour lists do.
> - A: $[2,1]$ is next to $[3,1]$ only. $[3,3]$ is two rows higher and one column to the right.
> - B: $[4,3]$ is next to $[3,3]$ and to the pit in $[4,4]$, not to $[3,1]$.
> - C: $[4,1]$ is next to $[3,1]$ only. $[3,3]$ is two rows higher and one column to the left.

### Q4 (slide 3, score)

Score: gold $+1000$, death $-1000$, $-1$ per action, $-10$ for using the arrow. Agent X takes 14 actions in total, one of them is Shoot, and it grabs the gold and stays alive. Agent Y takes 3 actions, and the third one moves it into a pit. What are the two scores?

- A) X: $986$, Y: $-1003$.
- B) X: $976$, Y: $-1003$.
- C) X: $986$, Y: $-1000$.
- D) X: $976$, Y: $-1000$.

> [!success]- Answer
> **B.** X: $1000-14\cdot1-10=976$. Y: $-3\cdot1-1000=-1003$.
> - A: X forgets the $-10$ for the arrow: $1000-14=986$.
> - C: X forgets the arrow, and Y forgets the $-1$ for each of its 3 actions.
> - D: Y is right only for the death. Each of the 3 actions also costs $-1$.

### Q5 (slides 4-10)

A student writes: "The wumpus world is fully observable because the agent has sensors, and it is episodic because every step costs $-1$." Slide 10 answers six questions: fully observable? deterministic? episodic? static? discrete? single-agent? Which description is right, and which answer would change if the wumpus walked one square per time step?

- A) Partially observable and episodic. "Discrete?" becomes no.
- B) Partially observable and sequential. "Static?" becomes no.
- C) Fully observable and sequential. "Discrete?" becomes no.
- D) Fully observable and episodic. "Static?" becomes no.

> [!success]- Answer
> **B.** The sensors are local: the agent perceives only its own square, so the world is partially observable. One move decides where the agent is and what it can do next, so the world is sequential, not episodic.
> A walking wumpus changes the world while the agent acts, so "Static?" becomes no.
> - A: Episodic means an action does not affect later ones. Here it does. A wumpus that moves square by square still moves on a grid, so the world stays discrete.
> - C: Having sensors does not make a world fully observable. The world stays discrete.
> - D: This keeps both of the student's mistakes.

## 3. Exploring a wumpus world (slides 11-19)

### Q6 (slides 11-13)

A square is breezy exactly when a neighbour has a pit. The agent perceives nothing in $[1,1]$, moves up to $[1,2]$ and feels a breeze. The neighbours of $[1,2]$ are $[1,1]$, $[1,3]$ and $[2,2]$. What does the agent know about pits in $[1,3]$ and $[2,2]$?

- A) Both $[1,3]$ and $[2,2]$ hold a pit.
- B) Exactly one of $[1,3]$ and $[2,2]$ holds a pit.
- C) $[1,3]$ holds a pit, since $[2,2]$ is not next to $[1,2]$.
- D) At least one of $[1,3]$ and $[2,2]$ holds a pit.

> [!success]- Answer
> **D.** The breeze says that at least one neighbour of $[1,2]$ has a pit. $[1,1]$ was visited, so it has no pit. What is left: $[1,3]$ or $[2,2]$, or both. The line that joins the two "P?" marks on slide 13 means "at least one".
> $[1,2]$ and $[2,1]$ are OK because nothing was perceived in $[1,1]$: no pit and no wumpus next to $[1,1]$.
> - A: Each "P?" is only a possibility, not a fact.
> - B: This reads the inclusive "or" as an exclusive "or". Both squares may hold a pit.
> - C: $[2,2]$ is the right neighbour of $[1,2]$. They share a side.

### Q7 (slides 14-15)

Breeze means a pit next to the square, stench means the wumpus next to it. The agent perceived nothing in $[1,1]$. In $[1,2]$ it perceives a stench and no breeze. In $[2,1]$ it perceives nothing. Where is the wumpus, and which of $[1,3]$, $[2,2]$, $[3,1]$ are provably safe?

- A) Wumpus in $[1,3]$. Safe: $[2,2]$ and $[3,1]$.
- B) Wumpus in $[1,3]$. Safe: $[3,1]$ only.
- C) Wumpus in $[1,3]$ or $[2,2]$. Safe: $[2,2]$ and $[3,1]$.
- D) Wumpus in $[1,3]$ or $[2,2]$. Safe: $[3,1]$ only.

> [!success]- Answer
> **A.** Stench in $[1,2]$: the wumpus is in $[1,3]$ or $[2,2]$ ($[1,1]$ is safe). Nothing in $[2,1]$: no wumpus and no pit in $[2,2]$ and $[3,1]$. So the wumpus is in $[1,3]$, and $[2,2]$, $[3,1]$ are safe.
> - B: The missing breeze and stench in $[2,1]$ also clear $[2,2]$, not only $[3,1]$.
> - C: If $[2,2]$ is safe, the wumpus cannot be there. This leaves the two facts uncombined.
> - D: This ignores that "nothing in $[2,1]$" is knowledge too. A missing percept removes $[2,2]$.

### Q8 (slides 16-18)

The agent knows: a pit in $[1,3]$, the wumpus in $[3,1]$, $[2,2]$ is safe, and there was no breeze in $[2,1]$. Now it stands in $[3,2]$ and perceives Breeze, Glitter and Stench. The neighbours of $[3,2]$ are $[2,2]$, $[3,1]$, $[3,3]$, $[4,2]$. Where can the new pit be, and what should the agent do?

- A) In $[3,1]$, $[3,3]$ or $[4,2]$. Grab the gold, go back over OK squares.
- B) In $[3,3]$ or $[4,2]$. Grab the gold, then move on to $[4,2]$.
- C) In $[3,3]$ or $[4,2]$. Grab the gold, go back over OK squares.
- D) In $[3,1]$, $[3,3]$ or $[4,2]$. Grab the gold, then move on to $[4,2]$.

> [!success]- Answer
> **C.** Stench: the known wumpus in $[3,1]$. Glitter: the gold is in $[3,2]$. Breeze: a pit is next to $[3,2]$. $[2,2]$ is safe, and $[3,1]$ has no pit (no breeze in $[2,1]$). So the pit is in $[3,3]$ or $[4,2]$ (or both).
> The agent should grab the gold and go back over the squares marked OK.
> - A: The missing breeze in $[2,1]$ rules out a pit in $[3,1]$.
> - B: $[4,2]$ may hold the pit. Moving there risks death for nothing, the gold is already found.
> - D: This keeps $[3,1]$ as a pit candidate and also walks into a possible pit.

### Q9 (slide 19, pits)

Tight spot: the agent has visited $(1,1)$, $(1,2)$, $(2,1)$ and feels a breeze in $(1,2)$ and in $(2,1)$. The unknown squares are $(1,3)$, $(2,2)$, $(3,1)$. The unknown neighbours of $(1,2)$ are $(1,3)$ and $(2,2)$, those of $(2,1)$ are $(2,2)$ and $(3,1)$. Each square has a pit with probability $0.1$, independently. Find $\Pr[\text{pit in }(2,2)]$ and $\Pr[\text{pit in }(1,3)]$ given the two breezes.

- A) $0.92$ and $0.08$
- B) $0.10$ and $0.17$
- C) $0.10$ and $0.08$
- D) $0.92$ and $0.17$

> [!success]- Answer
> **D.** Worlds that explain both breezes: a pit in $(2,2)$ (anything elsewhere), weight $0.1$. Or no pit in $(2,2)$ and pits in both $(1,3)$ and $(3,1)$: $0.9\cdot0.1\cdot0.1=0.009$. Sum $0.109$.
> $\Pr[(2,2)]=0.1/0.109\approx0.92$. $\Pr[(1,3)]=(0.1\cdot0.1+0.009)/0.109=0.019/0.109\approx0.17$.
> - A: $0.08=0.009/0.109$ forgets the worlds with pits in both $(1,3)$ and $(2,2)$ (weight $0.1\cdot0.1$).
> - B: $0.10$ is the prior. It ignores the evidence and does not divide by $0.109$.
> - C: This makes both mistakes.

### Q10 (slide 19, coercion)

The agent is in $(1,1)$. It smells the wumpus (the wumpus is in a neighbour) and feels no breeze (no pit in a neighbour). The neighbours are $(1,2)$ and $(2,1)$. It faces $(2,1)$ and shoots. An arrow flies straight and kills the wumpus if it hits it, and then a scream is heard. It hears no scream. What does it know?

- A) The wumpus is in $(1,2)$, and $(2,1)$ is safe.
- B) The wumpus is in $(1,2)$ or $(2,1)$. No square is safe yet.
- C) The wumpus is dead, so $(1,2)$ and $(2,1)$ are both safe.
- D) The wumpus is in $(1,2)$, but $(2,1)$ may still hold a pit.

> [!success]- Answer
> **A.** The arrow flew through $(2,1)$ and hit nothing, so the wumpus is not there. The stench puts it in $(1,2)$. No breeze in $(1,1)$ means no pit in $(2,1)$. So $(2,1)$ is safe. With a scream, $(2,1)$ would be safe too: this is coercion.
> - B: A miss is information. The arrow would have killed a wumpus in $(2,1)$.
> - C: A kill gives a scream. No scream means a miss.
> - D: No breeze in $(1,1)$ already rules out a pit in $(2,1)$.

## 4. Logic, entailment and models (slides 20-22)

### Q11 (slide 20)

In the language of arithmetic: is the string "$\geq x\ y\ +$" a sentence? Is $x+2\geq y$ true in the world $x=3,\ y=5$? Is it true in the world $x=1,\ y=4$?

- A) Not a sentence. False. False.
- B) Not a sentence. True. False.
- C) A sentence. True. False.
- D) A sentence. False. False.

> [!success]- Answer
> **B.** The symbols are legal, but their order breaks the grammar: not a sentence (a syntax answer). $3+2=5\geq5$: true. $1+2=3\geq4$: false. These two are semantics answers.
> - A: This reads $\geq$ as $>$. But $5\geq5$ holds.
> - C: Legal symbols are not enough. The grammar decides what a sentence is.
> - D: This makes both mistakes.

### Q12 (slide 21)

(a) Does $x+y=4$ entail $x=2$? (b) Does "$x=2$ and $y=2$" entail $x+y=4$? (c) The KB says "Maryland beat UNC". Does it entail "Maryland beat UNC or Duke beat UNC"?

- A) (a) yes, (b) yes, (c) yes.
- B) (a) no, (b) yes, (c) no.
- C) (a) no, (b) yes, (c) yes.
- D) (a) yes, (b) yes, (c) no.

> [!success]- Answer
> **C.** (a) The world $x=1,\ y=3$ makes $x+y=4$ true and $x=2$ false. One such world is enough. (b) In every world with $x=2$, $y=2$ the sum is $4$. (c) "Or" is inclusive: it is true as soon as one part is true.
> - A: $x=2$ is one way to get $x+y=4$, not the only one. Entailment needs every world.
> - B: This reads "or" as "both happened".
> - D: This makes both mistakes.

### Q13 (slide 22)

Two symbols: $M$ = "Maryland beat UNC", $D$ = "Duke beat UNC". $KB=M\land D$, $\alpha=M$, $\beta=(M\Leftrightarrow D)$. Count only entailments between two different sentences. Which entailments hold?

- A) Exactly two: $KB\models\alpha$ and $KB\models\beta$.
- B) Exactly one: $KB\models\alpha$.
- C) Exactly three: these two and $\alpha\models KB$.
- D) Exactly three: these two and $\alpha\models\beta$.

> [!success]- Answer
> **A.** Write a world as $(M,D)$. $M(KB)=\{(T,T)\}$, $M(\alpha)=\{(T,T),(T,F)\}$, $M(\beta)=\{(T,T),(F,F)\}$. $M(KB)$ lies in both other sets. No other set inclusion holds: $(T,F)$ breaks $\alpha\models KB$ and $\alpha\models\beta$, $(F,F)$ breaks $\beta\models\alpha$ and $\beta\models KB$.
> - B: This reads $\Leftrightarrow$ as "exactly one is true". Then $\beta$ is false in $(T,T)$.
> - C: Entailment goes one way. $(T,T)$ and $(T,F)$ are models of $\alpha$, only $(T,T)$ is a model of $KB$.
> - D: This assumes that two sentences entailed by the same KB entail each other. In $(T,F)$ $\alpha$ is true and $\beta$ is false.

## 5. Entailment in the wumpus world, inference (slides 23-29)

### Q14 (slides 23-24)

Each unknown square has a pit or not. (a) How many possible worlds are there with 5 unknown squares? (b) The agent feels nothing in $[1,1]$ and NO breeze in $[2,1]$. The unknown squares are $[1,2]$, $[2,2]$, $[3,1]$; the neighbours of $[1,1]$ are $[1,2]$, $[2,1]$, and those of $[2,1]$ are $[1,1]$, $[2,2]$, $[3,1]$. How many of the 8 worlds are models of this KB?

- A) (a) 32, (b) 3
- B) (a) 10, (b) 1
- C) (a) 10, (b) 3
- D) (a) 32, (b) 1

> [!success]- Answer
> **D.** (a) Two choices per square: $2^5=32$. (b) No breeze in $[1,1]$: no pit in $[1,2]$. No breeze in $[2,1]$: no pit in $[2,2]$ and $[3,1]$. Only the world without any pit is left: 1 model. All three squares are safe.
> - A: 3 is the count on slide 25, where $[2,1]$ is breezy.
> - B: $2\cdot5=10$ adds the choices. They multiply.
> - C: This makes both mistakes.

### Q15 (slide 25)

KB: $R_1: B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$, $R_2: B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$, $\neg B_{1,1}$, $B_{2,1}$, $\neg P_{1,1}$, $\neg P_{2,1}$. Here $P_{i,j}$ = pit in $[i,j]$, $B_{i,j}$ = breeze in $[i,j]$. Why is the world with a pit only in $[1,2]$ not a model?

- A) It is a model: the pit in $[1,2]$ explains the breeze in $[2,1]$.
- B) $R_1$ fails ($[1,1]$ would be breezy) and $R_2$ fails ($[2,1]$ has no cause).
- C) Only $R_2$ fails. $R_1$ holds, as it only says "breeze means a pit".
- D) Only $R_1$ fails. $R_2$ holds, as the pit in $[1,2]$ explains the breeze in $[2,1]$.

> [!success]- Answer
> **B.** $P_{1,2}$ true makes $P_{1,2}\lor P_{2,1}$ true, so $R_1$ needs $B_{1,1}$ true, but the KB says $\neg B_{1,1}$. $R_2$ needs a pit in $[1,1]$, $[2,2]$ or $[3,1]$, and there is none. Both fail.
> - A: $[1,2]$ and $[2,1]$ are diagonal, not neighbours. $[1,2]$ is not in $R_2$.
> - C: This reads $\Leftrightarrow$ as $\Rightarrow$. The direction "pit next to me, so breeze" is part of $R_1$.
> - D: $P_{1,2}$ does not appear in $R_2$, so it cannot explain $B_{2,1}$.

### Q16 (slides 26-27)

The KB of the last question has three models. Write a model as (pit in $[2,2]$, pit in $[3,1]$): $(F,T)$, $(T,F)$, $(T,T)$. Let $\alpha_3$ = "pit in $[3,1]$" and $\alpha_4$ = "pit in $[2,2]$ or in $[3,1]$". What is entailed?

- A) Neither $\alpha_3$ nor $\neg\alpha_3$. $\alpha_4$ is not entailed.
- B) $\neg\alpha_3$ is entailed. $\alpha_4$ is entailed.
- C) Neither $\alpha_3$ nor $\neg\alpha_3$. $\alpha_4$ is entailed.
- D) $\neg\alpha_3$ is entailed. $\alpha_4$ is not entailed.

> [!success]- Answer
> **C.** $\alpha_3$ is false in $(T,F)$, so it is not entailed. $\neg\alpha_3$ is false in $(F,T)$ and $(T,T)$, so it is not entailed either: the KB cannot decide. $\alpha_4$ is true in all three models: entailed.
> - A: This reads "or" as "and". "Pit in both" is false in two models.
> - B: "Not entailed" is not "the negation is entailed".
> - D: This makes both mistakes.

### Q17 (slide 28)

Sound: whatever the procedure derives is entailed. Complete: whatever is entailed, the procedure derives. Procedure X answers "derived" for every sentence. Procedure Y never derives anything. Classify both.

- A) X is sound, not complete. Y is complete, not sound.
- B) X is complete, not sound. Y is complete, not sound.
- C) X is sound, not complete. Y is sound, not complete.
- D) X is complete, not sound. Y is sound, not complete.

> [!success]- Answer
> **D.** X derives everything, so it misses nothing (complete), but it also derives sentences that are not entailed (not sound). Y derives nothing, so nothing it derives is wrong (sound), but it misses every entailed sentence (not complete).
> - A: This swaps the two definitions for both procedures.
> - B: Y misses entailed sentences, so it cannot be complete.
> - C: X derives non-entailed sentences, so it cannot be sound.

## 6. Propositional logic: syntax and semantics (slides 30-33)

### Q18 (slide 30)

Sentences of propositional logic: a symbol, $\neg S$, $S_1\land S_2$, $S_1\lor S_2$, $S_1\Rightarrow S_2$, $S_1\Leftrightarrow S_2$, where $S$, $S_1$, $S_2$ are sentences. Which strings are sentences? (a) $P\land\lor Q$ (b) $\neg(P\Rightarrow Q)\Leftrightarrow R$ (c) $\neg\neg P$ (d) $P\Rightarrow$

- A) (b) and (c)
- B) (a), (c) and (d)
- C) (a), (b), (c) and (d)
- D) (b) only

> [!success]- Answer
> **A.** (b) is an equivalence with $S_1=\neg(P\Rightarrow Q)$ and $S_2=R$. (c) applies negation twice: $P$, then $\neg P$, then $\neg\neg P$.
> (a) is not a sentence: $\lor Q$ is not a sentence, so $\land$ has no right side. (d) has no right side of $\Rightarrow$.
> - B: This takes every string of the listed symbols as a sentence and rejects (b) because brackets are not in the list. Brackets only group a sentence. Legal symbols are not enough: $\land$ and $\Rightarrow$ need a sentence on both sides.
> - C: This takes every string of legal symbols as a sentence. The grammar decides: (a) and (d) miss a sentence on one side.
> - D: The negation rule can be applied to any sentence, also to $\neg P$.

### Q19 (slide 31)

World: $P_{1,2}=\text{false}$, $P_{2,2}=\text{true}$, $P_{3,1}=\text{false}$. Evaluate $(P_{1,2}\Rightarrow P_{2,2})\land\neg(P_{2,2}\Leftrightarrow P_{3,1})$. Give the value of $P_{1,2}\Rightarrow P_{2,2}$, of $P_{2,2}\Leftrightarrow P_{3,1}$, and of the whole sentence.

- A) False, false. The sentence is false.
- B) False, true. The sentence is false.
- C) True, false. The sentence is true.
- D) True, true. The sentence is false.

> [!success]- Answer
> **C.** false $\Rightarrow$ true is true (an implication is false only for true $\Rightarrow$ false). true $\Leftrightarrow$ false is false (the sides differ). $\neg$false is true. true $\land$ true $=$ true.
> - A: This reads the implication as a biconditional: false $\Leftrightarrow$ true is false.
> - B: This reads the implication as a biconditional and the biconditional as "or".
> - D: This reads the biconditional as "or": true $\lor$ false is true, so $\neg$ gives false.

### Q20 (slides 32-33)

$P_{i,j}$ = pit in $[i,j]$, $B_{i,j}$ = breeze in $[i,j]$. (b) The KB is $R_1: B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$ and $\neg B_{1,1}$. (c) The KB is $R_2: B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$, $B_{2,1}$ and $\neg P_{1,1}$. In each case, which sentence about the pits is the strongest conclusion? The strongest conclusion is the entailed sentence that entails every other entailed sentence about these pits.

- A) (b) $\neg P_{1,2}\lor\neg P_{2,1}$. (c) $P_{2,2}\lor P_{3,1}$.
- B) (b) $\neg P_{1,2}\land\neg P_{2,1}$. (c) $P_{2,2}\lor P_{3,1}$.
- C) (b) $\neg P_{1,2}\lor\neg P_{2,1}$. (c) Nothing new follows.
- D) (b) $\neg P_{1,2}\land\neg P_{2,1}$. (c) Nothing new follows.

> [!success]- Answer
> **B.** (b) Use the direction $(P_{1,2}\lor P_{2,1})\Rightarrow B_{1,1}$. $B_{1,1}$ is false, so $P_{1,2}\lor P_{2,1}$ is false, and by De Morgan $\neg P_{1,2}\land\neg P_{2,1}$.
> (c) Use the direction $B_{2,1}\Rightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$. With $\neg P_{1,1}$: $P_{2,2}\lor P_{3,1}$. Each percept uses one direction of $\Leftrightarrow$.
> - A: $\neg P_{1,2}\lor\neg P_{2,1}$ is entailed too, but it is weaker: it does not entail $\neg P_{1,2}\land\neg P_{2,1}$. This applies De Morgan without flipping the connective: $\neg(P_{1,2}\lor P_{2,1})$ is an "and", not an "or".
> - C: De Morgan without the flip, and in (c) only the direction "pit, so breeze" is used. That direction says nothing when the breeze is true.
> - D: (b) is right, but (c) uses only the direction "pit, so breeze".

## 7. Truth tables for inference (slides 34-35)

### Q21 (slide 34)

![[cmpe421_lecture_07_logical_agents_p034.jpg]]

The table uses the 7 symbols $B_{1,1}$, $B_{2,1}$, $P_{1,1}$, $P_{1,2}$, $P_{2,1}$, $P_{2,2}$, $P_{3,1}$ ($P$ = pit, $B$ = breeze). How many rows does the full table have? Using the three rows where the KB is true, is $P_{2,2}\lor P_{3,1}$ entailed? Is $P_{2,2}$ entailed?

- A) 128 rows. $P_{2,2}\lor P_{3,1}$ is entailed, $P_{2,2}$ is not.
- B) 128 rows. Both $P_{2,2}\lor P_{3,1}$ and $P_{2,2}$ are entailed.
- C) 14 rows. $P_{2,2}\lor P_{3,1}$ is entailed, $P_{2,2}$ is not.
- D) 14 rows. Both $P_{2,2}\lor P_{3,1}$ and $P_{2,2}$ are entailed.

> [!success]- Answer
> **A.** $2^7=128$ rows. In the three KB rows, $(P_{2,2},P_{3,1})$ is (false, true), (true, false), (true, true). The "or" is true in all three: entailed. $P_{2,2}$ is false in the first: not entailed.
> With one $P$ and one $B$ symbol for each of 16 squares the table would need $2^{32}\approx4.3\cdot10^9$ rows.
> - B: $P_{2,2}$ is true in 2 of 3 KB rows. Entailment needs all of them.
> - C: $2\cdot7=14$ adds the choices. Each symbol doubles the number of rows.
> - D: This makes both mistakes.

### Q22 (slide 35)

`TT-Check-All` takes one symbol per level and calls itself twice (symbol true, symbol false). With no symbols left it returns "is $\alpha$ true" if the KB is true in the world, else true. For $n=4$ symbols it checks $2^4=16$ complete worlds. How many calls are made in total, and why does it return true when the KB is false?

- A) 16 calls. A world where the KB is false makes $\alpha$ true.
- B) 31 calls. A world where the KB is false makes $\alpha$ true.
- C) 16 calls. A world where the KB is false cannot be a counterexample.
- D) 31 calls. A world where the KB is false cannot be a counterexample.

> [!success]- Answer
> **D.** Calls per depth: $1+2+4+8+16=2^5-1=31$. Entailment speaks only about worlds where the KB is true. A world where the KB is false must not make the final "and" false, so it returns true ("no problem here").
> - A: 16 counts only the calls at the last level, and $\alpha$ can be false in such a world.
> - B: The count is right. The KB being false says nothing about $\alpha$.
> - C: The reason is right. 16 counts only the complete worlds, not the calls above them.

## 8. Proof methods, equivalence, validity (slides 36-38)

### Q23 (slide 36)

DPLL on the clauses $\neg C$, $\ \neg A\lor C$, $\ A\lor B$, $\ B\lor D$. A pure symbol has the same sign in every clause where it appears. A unit clause has one literal left that is not yet false. Which symbols are pure at the start, and what does unit propagation from $\neg C$ give?

- A) Pure: $D$ only. $C$ false, then $A$ false, then $B$ true.
- B) Pure: $B$ and $D$. $C$ false, then $A$ false, then $B$ true.
- C) Pure: $B$ and $D$. $C$ false, then $A$ true, and $B$ stays free.
- D) Pure: $D$ only. $C$ false, then $A$ true, and $B$ stays free.

> [!success]- Answer
> **B.** $B$ and $D$ appear only positive. $A$ and $C$ appear with both signs.
> $\neg C$: $C$ false. $\neg A\lor C$ keeps only $\neg A$: $A$ false. $A\lor B$ keeps only $B$: $B$ true. $D$ is pure: true. Model $A=F$, $B=T$, $C=F$, $D=T$. A truth table would need $2^4=16$ rows.
> - A: This takes "pure" as "appears in only one clause". $B$ appears in two clauses, always positive.
> - C: With $C$ false, the clause $\neg A\lor C$ needs $\neg A$ true, so $A$ is false. This drops the sign.
> - D: This makes both mistakes.

### Q24 (slide 37)

Use truth tables. (a) Is $(A\Rightarrow B)\equiv(B\Rightarrow A)$? (b) Is $\neg(A\land B)\equiv(A\Rightarrow\neg B)$?

- A) (a) Equivalent. (b) Equivalent.
- B) (a) Equivalent. (b) Not equivalent.
- C) (a) Not equivalent. (b) Equivalent.
- D) (a) Not equivalent. (b) Not equivalent.

> [!success]- Answer
> **C.** (a) $A$ true, $B$ false: $A\Rightarrow B$ is false, $B\Rightarrow A$ is true. One row is enough. (The true law is contraposition: $\neg B\Rightarrow\neg A$.)
> (b) $A\Rightarrow\neg B\equiv\neg A\lor\neg B\equiv\neg(A\land B)$. All four rows agree: F, T, T, T.
> - A: This reads the implication as a biconditional, which is symmetric.
> - B: Both slips: the symmetric implication, and De Morgan without the flip ($\neg A\land\neg B$).
> - D: De Morgan without flipping gives $\neg A\land\neg B$. That differs from $A\Rightarrow\neg B$ at $A$ true, $B$ false.

### Q25 (slide 38)

$KB=\{A\Rightarrow B,\ \neg B\}$ and $\alpha=\neg A$. Use $KB\models\alpha$ iff $KB\land\neg\alpha$ is unsatisfiable. Then classify $(A\Rightarrow B)\Rightarrow A$.

- A) $KB\land\neg\alpha$ is unsatisfiable, so $KB\models\neg A$. The sentence is satisfiable, not valid.
- B) $KB\land\neg\alpha$ is satisfiable, so $KB\not\models\neg A$. The sentence is satisfiable, not valid.
- C) $KB\land\neg\alpha$ is unsatisfiable, so $KB\models\neg A$. The sentence is valid.
- D) $KB\land\neg\alpha$ is satisfiable, so $KB\not\models\neg A$. The sentence is valid.

> [!success]- Answer
> **A.** $KB\land\neg\alpha=(A\Rightarrow B)\land\neg B\land A$. $A$ true and $\neg B$ make $A\Rightarrow B$ false. No world satisfies it, so $KB\models\neg A$ (Modus Tollens).
> $(A\Rightarrow B)\Rightarrow A$: true when $A$ is true. When $A$ is false, $A\Rightarrow B$ is true and the right side false, so false. Satisfiable, not valid.
> - B: This tests $KB\land\alpha$ (forgot to negate $\alpha$). It is true at $A$, $B$ false.
> - C: This checks only the rows with $A$ true.
> - D: This makes both mistakes.

## 9. Horn clauses and forward chaining (slides 39-50)

### Q26 (slide 39)

A Horn clause is a symbol, or (conjunction of symbols) $\Rightarrow$ symbol. A sentence also counts if it is logically equivalent to such a clause. Which are Horn clauses: (a) $A\land B\Rightarrow C$ (b) $A\Rightarrow B\lor C$ (c) $D$ (d) $\neg A\Rightarrow B$ (e) $\neg A\lor\neg B\lor C$? And can Modus Ponens derive $A$ from $C$ and $A\land B\Rightarrow C$?

- A) (a), (c) and (e). Yes, it derives $A$.
- B) (a) and (c). No, nothing follows about $A$.
- C) (a) and (c). Yes, it derives $A$.
- D) (a), (c) and (e). No, nothing follows about $A$.

> [!success]- Answer
> **D.** (e) is $A\land B\Rightarrow C$ by implication elimination and De Morgan, so it is (a) again. (b) has an "or" on the right. (d) is $A\lor B$, which has two positive literals.
> Modus Ponens goes from the premises to the head. The world $A$, $B$ false, $C$ true makes $C$ and the rule true and $A$ false.
> - A: This runs Modus Ponens backwards, from the head to a premise.
> - B: (e) is the same clause as (a), only written with $\lor$.
> - C: This makes both mistakes.

### Q27 (slide 40)

![[cmpe421_lecture_07_logical_agents_p040.jpg]]

The graph shows the rules on the left. What does a small arc that joins two edges mean, and which symbol has two different rules that derive it?

- A) Any one of the joined premises is enough (OR). $L$ has two rules.
- B) All the joined premises are needed (AND). $L$ has two rules.
- C) All the joined premises are needed (AND). $P$ has two rules.
- D) Any one of the joined premises is enough (OR). $P$ has two rules.

> [!success]- Answer
> **B.** An arc joins the premises of one rule, and all are needed: the dot below $P$ joins $L$ and $M$ for $L\land M\Rightarrow P$. $L$ has two dots below it, for $A\land P\Rightarrow L$ and $A\land B\Rightarrow L$. One of them is enough.
> The long curved line carries $P$ down to the dot of $A\land P\Rightarrow L$. It makes the cycle $P\to L\to P$.
> - A: Two separate dots into one symbol mean OR. An arc inside one dot means AND.
> - C: The curved line starts at $P$ and is a premise of a rule for $L$. Only $L\land M\Rightarrow P$ has head $P$.
> - D: This makes both mistakes.

### Q28 (slide 41, code)

Forward chaining: `count[c]` is the number of premises of rule $c$ not yet known, `agenda` holds known symbols not yet processed, `inferred[p]` is true once $p$ has been processed. A popped $p$ lowers the count of every rule with $p$ as a premise, unless `inferred[p]` is true. On slide 40, $L$ is pushed twice. What could go wrong without the test "unless `inferred[p]`"?

- A) Nothing changes: a count drops only when the head of its rule is popped.
- B) Entailed symbols would be missed, as the second copy of $L$ is skipped.
- C) Counts would drop twice for one premise, so a rule could fire too early.
- D) The agenda would never empty, as $L$ would push itself again and again.

> [!success]- Answer
> **C.** The second pop of $L$ would lower the counts of $L\land M\Rightarrow P$ and $B\land L\Rightarrow M$ again. In another KB a count could reach 0 while a premise is still unknown, and a symbol that is not entailed would be derived (not sound). The test also stops repeated work.
> - A: A count drops when a premise is popped, not the head.
> - B: The test skips only symbols already processed. Nothing new is lost.
> - D: Counts only go down and a head is pushed only when its count hits 0 exactly. The run still ends.

### Q29 (slide 41, trace)

KB: $R_1: A\Rightarrow B$, $R_2: B\land C\Rightarrow D$, $R_3: A\land D\Rightarrow E$, $R_4: E\land F\Rightarrow G$, facts $A$, $C$. Forward chaining with a first-in first-out agenda, start agenda $\langle A,C\rangle$, query $E$. It returns true when a rule with head $E$ reaches count 0. In which order are symbols popped before that? And does $KB\models G$?

- A) $A, B, C, D$. $KB\not\models G$.
- B) $A, C, B, D$. $KB\not\models G$.
- C) $A, B, C, D$. $KB\models G$.
- D) $A, C, B, D$. $KB\models G$.

> [!success]- Answer
> **B.** Pop $A$: $R_1\to0$, push $B$; $R_3\to1$. Agenda $\langle C,B\rangle$. Pop $C$: $R_2\to1$. Pop $B$: $R_2\to0$, push $D$. Pop $D$: $R_3\to0$, head $E$: true.
> For $G$: pop $E$, $R_4\to1$. The agenda is empty and $F$ is never known. $KB\not\models G$.
> - A: This puts $B$ in front of $C$, as a stack would. The agenda is first-in first-out.
> - C: Stack order, and $R_4$ fired before its premise $F$ was known.
> - D: The order is right, but $R_4$ fires before all its premises are known. $F$ is not a fact and no rule derives it.

### Q30 (slides 42-44)

Rules in this order: $P\Rightarrow Q$, $L\land M\Rightarrow P$, $B\land L\Rightarrow M$, $A\land P\Rightarrow L$, $A\land B\Rightarrow L$. Facts $A$, $B$. After $A$ is popped (slide 43) the counts are $1,2,2,1,1$ and the agenda is $\langle B\rangle$. What are the counts and the agenda after $B$ is popped?

- A) Counts $1,2,1,1,0$. Agenda $\langle L\rangle$.
- B) Counts $1,2,2,1,0$. Agenda $\langle L\rangle$.
- C) Counts $1,2,1,1,0$. Agenda $\langle L,M\rangle$.
- D) Counts $1,2,2,1,0$. Agenda $\langle L,M\rangle$.

> [!success]- Answer
> **A.** $B$ is a premise of $B\land L\Rightarrow M$ ($2\to1$) and of $A\land B\Rightarrow L$ ($1\to0$). The second rule fires: push $L$.
> - B: This forgets that $B$ is also a premise of $B\land L\Rightarrow M$.
> - C: $M$ is pushed only after $L$ is popped. This fires $B\land L\Rightarrow M$ before all its premises are known.
> - D: This makes both mistakes.

### Q31 (slides 45-47)

Same KB and rule order. Slide 45 shows counts $1,1,0,1,0$ and agenda $\langle M\rangle$. The algorithm pushes a head whenever its count reaches 0, and skips a popped $p$ when `inferred[p]` is true. After the next two pops ($M$, then $P$), what is on the agenda, and what happens when $L$ is popped?

- A) Only $Q$. $L$ is not pushed again, since `inferred[L]` is true.
- B) $L$ and $Q$. Popping $L$ lowers the counts of its rules again.
- C) Only $Q$. The count of $A\land P\Rightarrow L$ stays at 1.
- D) $L$ and $Q$. Popping $L$ does nothing, since `inferred[L]` is true.

> [!success]- Answer
> **D.** Pop $M$: $L\land M\Rightarrow P$ goes $1\to0$, push $P$. Pop $P$: $P\Rightarrow Q$ and $A\land P\Rightarrow L$ both go $1\to0$, so $Q$ and $L$ are pushed. $L$ is there again because a second rule for $L$ fired. When it is popped, `inferred[L]` is true, so it is skipped.
> - A: The test is made at the pop, not at the push.
> - B: The test `inferred[L]` stops this.
> - C: $P$ is a premise of $A\land P\Rightarrow L$, so popping $P$ lowers that count to 0.

### Q32 (slides 48-49)

Same rules, but the fact $B$ is removed: only $A$ is a fact. Run forward chaining again. Which symbols are inferred, and what follows about $Q$?

- A) Only $A$. As $Q$ is not derived, $KB\models\neg Q$.
- B) $A$ and $L$. $Q$ is not entailed.
- C) Only $A$. $Q$ is not entailed.
- D) $A$, $B$, $L$, $M$, $P$, $Q$. $Q$ is entailed.

> [!success]- Answer
> **C.** Pop $A$: $A\land P\Rightarrow L$ and $A\land B\Rightarrow L$ go $2\to1$. No count reaches 0 and the agenda is empty. $L$ needs $B$ or $P$, and $P$ needs $L$: the cycle gives nothing. Forward chaining is complete for Horn KBs, so $KB\not\models Q$.
> - A: "Not derived" is not "the negation is entailed". The world with every symbol true is a model where $Q$ is true.
> - B: This fires $A\land B\Rightarrow L$ with only $A$ known.
> - D: This is the run of the original KB, with $B$ still a fact.

### Q33 (slide 50)

Same KB (rules of slide 40, only the fact $A$). Forward chaining stops with only $A$ inferred. The proof of slide 50 builds the world $m$: $A$ true, and $B$, $L$, $M$, $P$, $Q$ false. What does $m$ show?

- A) $m$ is a model of the KB with $Q$ false, so $KB\not\models Q$.
- B) $m$ is not a model: $P\Rightarrow Q$ is false when both sides are false.
- C) $m$ is a model of the KB with $Q$ false, so $KB\models\neg Q$.
- D) $m$ is not a model: $A\land P\Rightarrow L$ fails, as $A$ is true and $L$ false.

> [!success]- Answer
> **A.** $P\Rightarrow Q$: false $\Rightarrow$ false, true. The other four rules have a false premise ($L$, $B$ or $P$ is false), so they are true. $A$ is true. So $m$ is a model, and $Q$ is false in it: $KB\not\models Q$.
> - B: An implication is false only for true $\Rightarrow$ false. This treats it as an "and".
> - C: One model with $Q$ false shows only "not entailed". Other models (all true) make $Q$ true.
> - D: The premise is $A\land P$, and $P$ is false. Only one premise was checked.

## 10. Backward chaining (slides 51-63)

### Q34 (slide 51)

A KB has only the rules $L\land M\Rightarrow P$ and $A\land P\Rightarrow L$ and the facts $A$, $M$. Backward chaining gets the query $P$: to prove a goal it uses a fact or a rule with that head and proves all premises. What happens without the check "is the goal already on the recursion stack", and what happens with it?

- A) Without: it returns false. With: it returns true.
- B) Without: it never ends. With: it returns false.
- C) Without: it never ends. With: it returns true.
- D) Without: it returns false. With: it returns false.

> [!success]- Answer
> **B.** Without the check: $P$ needs $L$, $L$ needs $A$ and $P$, $P$ needs $L$ again, and so on forever. With it: $P$ as a subgoal of $L$ is already on the stack, so that rule fails. $L$ has no other rule and is no fact, so $L$ and then $P$ fail. "False" is right: forward chaining derives only $A$ and $M$.
> - A: Without the check nothing stops the recursion. And a goal on the stack is not proved.
> - C: The loop check makes the rule fail. It does not count the repeated goal as proved.
> - D: The answer with the check is right, but without it the recursion has no end.

### Q35 (slides 52-54)

KB: $P\Rightarrow Q$, $L\land M\Rightarrow P$, $B\land L\Rightarrow M$, $A\land P\Rightarrow L$, $A\land B\Rightarrow L$, facts $A$, $B$. Query $Q$. Backward chaining first uses $P\Rightarrow Q$ and opens the goal $P$. Which rule does it use for $P$, and which goals are open (not yet proved) on slide 54?

- A) $A\land P\Rightarrow L$. Open goals: $A$ and $B$.
- B) $L\land M\Rightarrow P$. Open goals: $A$ and $B$.
- C) $A\land P\Rightarrow L$. Open goals: $L$ and $M$.
- D) $L\land M\Rightarrow P$. Open goals: $L$ and $M$.

> [!success]- Answer
> **D.** Backward chaining needs a rule whose head is the goal $P$: $L\land M\Rightarrow P$. Its premises $L$ and $M$ are the new open goals. Nothing is proved yet. $A$ and $B$ are known facts.
> - A: $A\land P\Rightarrow L$ has $P$ as a premise, not as its head. And facts are never open goals.
> - B: The rule is right, but $A$ and $B$ are facts. They are known at once.
> - C: This uses a rule in the forward direction, from the premise $P$ to the head $L$.

### Q36 (slides 55-57)

Same KB as in the last question, but the rule $A\land B\Rightarrow L$ is deleted. Backward chaining uses the loop check. What does it answer for $Q$, and is the answer right?

- A) True: $L$ is proved, as $P$ is already a goal on the stack.
- B) False, and this is wrong: $Q$ is entailed, but the loop check loses it.
- C) False, and this is right: forward chaining does not derive $Q$ either.
- D) It never answers: the loop $P\to L\to P$ repeats forever.

> [!success]- Answer
> **C.** Goal $L$ has only $A\land P\Rightarrow L$ left. $P$ is on the stack, so the rule fails, $L$ fails, then $P$ and $Q$ fail. Forward chaining pops $A$ and $B$ and stops: no count reaches 0. So $Q$ is not entailed and "false" is right.
> - A: A goal on the stack is not proved. Using it would be circular.
> - B: $Q$ is not entailed here. Backward chaining with the loop check is complete for Horn KBs.
> - D: The loop check stops exactly this repetition.

### Q37 (slides 58-60)

Full KB of Q35 again. $L$ has just been proved for the goal $P$ (slide 58), and the goal $M$ is still open. Backward chaining remembers proved goals. Which rule proves $M$, and how much new work do its premises need?

- A) $B\land L\Rightarrow M$. $B$ is a fact and $L$ is remembered as proved.
- B) $B\land L\Rightarrow M$. $L$ must be proved again with $A\land B\Rightarrow L$.
- C) $L\land M\Rightarrow P$. $L$ and $P$ must be proved before $M$.
- D) $B\land L\Rightarrow M$. $L$ fails, as it is still on the goal stack.

> [!success]- Answer
> **A.** The rule with head $M$ is $B\land L\Rightarrow M$. $B$ is a fact. $L$ was proved a moment ago and is remembered, so no rule for $L$ is tried again. $M$ is proved at once (slide 60).
> - B: This ignores the advice "avoid repeated work".
> - C: $M$ is a premise of this rule, not its head.
> - D: $L$ is finished and has left the stack. Only goals still being proved are on it.

### Q38 (slides 61-62)

Full KB of Q35, rules tried in this order: $P\Rightarrow Q$, $L\land M\Rightarrow P$, $B\land L\Rightarrow M$, $A\land P\Rightarrow L$, $A\land B\Rightarrow L$. The query is now $M$. What does backward chaining (with the loop check) do?

- A) Proves $L$, $M$, and $P$ on a second try. $Q$ is never touched.
- B) Proves $L$, $M$. $P$ is tried and fails. $Q$ is never touched.
- C) Proves $L$, $M$ with $A\land B\Rightarrow L$. $P$, $Q$ are never touched.
- D) Proves $L$, $M$, $P$, $Q$, the same goals as for the query $Q$.

> [!success]- Answer
> **B.** Goal $M$: rule $B\land L\Rightarrow M$, $B$ is a fact. Goal $L$: the first rule $A\land P\Rightarrow L$ opens $P$. $P$ needs $L$, which is on the stack, so $P$ fails. Then $A\land B\Rightarrow L$ proves $L$, and $M$ follows.
> - A: $P$ failed in its only try. Backward chaining does not go back to prove it later.
> - C: $A\land P\Rightarrow L$ comes first in the list, so $P$ is tried.
> - D: Backward chaining works only on goals the query needs. $Q$ is not one of them.

### Q39 (slide 63)

KB: $A\Rightarrow B$, $B\land C\Rightarrow D$, $A\land D\Rightarrow E$, $C\Rightarrow H$, $H\Rightarrow I$, $I\Rightarrow J$, facts $A$, $C$. Query $B$. How many rules does backward chaining look at, and how many symbols does forward chaining infer if it runs until the agenda is empty (facts included)?

- A) Backward: 1 rule. Forward: 6 symbols.
- B) Backward: 6 rules. Forward: 6 symbols.
- C) Backward: 1 rule. Forward: 8 symbols.
- D) Backward: 6 rules. Forward: 8 symbols.

> [!success]- Answer
> **C.** Backward: goal $B$, rule $A\Rightarrow B$, $A$ is a fact. Done after 1 rule. Forward infers $A, C, B, H, D, I, E, J$: 8 symbols, and 6 of them ($C,H,D,I,E,J$) do not matter for $B$.
> - A: 6 counts only the irrelevant symbols and leaves out $A$ and $B$.
> - B: Backward chaining looks only at rules whose head is a goal. It does not scan the whole KB.
> - D: The forward count is right, but backward chaining does not read all rules.

## 11. Resolution (slides 64-69)

### Q40 (slide 64)

Resolution: if one clause has a literal $\ell$ and the other has $\neg\ell$, the resolvent is the "or" of all other literals of both. (b) $X\lor\neg Y$ and $Y\lor Z$ give $X\lor Z$, and this with $\neg X$ gives $Z$. Now resolve (a) $P\lor Q\lor\neg R$ and $R\lor S$, and (c) $A\lor B$ and $A\lor C$.

- A) (a) $P\lor Q$. (c) $A\lor B\lor C$.
- B) (a) $P\lor Q\lor S$. (c) $A\lor B\lor C$.
- C) (a) $P\lor Q$. (c) No resolvent.
- D) (a) $P\lor Q\lor S$. (c) No resolvent.

> [!success]- Answer
> **D.** (a) The pair is $\neg R$ and $R$. All other literals of both clauses stay: $P\lor Q\lor S$. (c) No symbol is positive in one clause and negative in the other, so there is no resolvent.
> - A: This keeps only the literals of the first clause, and it joins clauses without a complementary pair.
> - B: $A$ and $A$ have the same sign. Resolution needs $\ell$ and $\neg\ell$.
> - C: $S$ from the second clause must stay in the resolvent.

### Q41 (slide 65)

Resolve $\neg P\lor Q$ with $\neg Q\lor R$ and write the resolvent as an implication. Then write $A\lor\neg B$ as an implication in two ways.

- A) $\neg P\lor R$, so $P\Rightarrow R$. $A\lor\neg B$ is $B\Rightarrow A$ and $\neg A\Rightarrow\neg B$.
- B) $\neg P\lor R$, so $R\Rightarrow P$. $A\lor\neg B$ is $B\Rightarrow A$ and $\neg A\Rightarrow\neg B$.
- C) $\neg P\lor R$, so $P\Rightarrow R$. $A\lor\neg B$ is $A\Rightarrow B$ and $\neg B\Rightarrow\neg A$.
- D) $\neg P\lor R$, so $R\Rightarrow P$. $A\lor\neg B$ is $A\Rightarrow B$ and $\neg B\Rightarrow\neg A$.

> [!success]- Answer
> **A.** $\alpha\Rightarrow\beta\equiv\neg\alpha\lor\beta$. So $\neg P\lor R$ is $P\Rightarrow R$: from $P\Rightarrow Q$ and $Q\Rightarrow R$ we get $P\Rightarrow R$, chaining two rules. $A\lor\neg B=\neg B\lor A$ is $B\Rightarrow A$, and by contraposition $\neg A\Rightarrow\neg B$.
> - B: The negated literal is the premise. $\neg P\lor R$ has $P$ on the left.
> - C: $A\Rightarrow B$ is $\neg A\lor B$, a different clause. It is false at $A$ true, $B$ false, where $A\lor\neg B$ is true.
> - D: This makes both mistakes.

### Q42 (slide 66)

Convert $(A\Rightarrow B)\Leftrightarrow C$ to CNF: remove $\Leftrightarrow$, remove $\Rightarrow$, move $\neg$ inwards, distribute $\lor$ over $\land$.

- A) $(A\lor\neg B\lor C)\land(\neg A\lor B\lor\neg C)$
- B) $(A\lor C)\land(\neg B\lor C)\land(\neg A\lor B\lor\neg C)$
- C) $(\neg A\lor C)\land(\neg B\lor C)\land(\neg A\lor B\lor\neg C)$
- D) $(A\lor C)\land(\neg B\lor C)$

> [!success]- Answer
> **B.** 1: $((A\Rightarrow B)\Rightarrow C)\land(C\Rightarrow(A\Rightarrow B))$. 2: $(\neg(\neg A\lor B)\lor C)\land(\neg C\lor\neg A\lor B)$. 3: $((A\land\neg B)\lor C)\land(\neg C\lor\neg A\lor B)$. 4: $(A\lor C)\land(\neg B\lor C)\land(\neg A\lor B\lor\neg C)$. A truth table agrees in all 8 rows.
> - A: De Morgan without flipping the connective: $\neg(\neg A\lor B)$ became $A\lor\neg B$ instead of $A\land\neg B$.
> - C: The double negation was lost: $\neg\neg A$ is $A$, not $\neg A$.
> - D: This keeps only the direction $(A\Rightarrow B)\Rightarrow C$, as if $\Leftrightarrow$ were $\Rightarrow$.

### Q43 (slide 67)

`PL-Resolution` on the clauses of $KB\land\neg\alpha$ resolves all pairs each round. It returns true if a resolvent is the empty clause, and false if `new` $\subseteq$ `clauses`. Run it for $KB=\{A\lor B\}$ and $\alpha=A$. Which line ends it, and why must the loop always end?

- A) The empty clause: true. Each round makes the clauses shorter.
- B) `new` $\subseteq$ `clauses`: false. Each round makes the clauses shorter.
- C) The empty clause: true. Only finitely many clauses exist.
- D) `new` $\subseteq$ `clauses`: false. Only finitely many clauses exist.

> [!success]- Answer
> **D.** Clauses: $A\lor B$, $\neg A$. Round 1 gives $B$, which is added. Round 2 gives only $B$ again, so `new` $\subseteq$ `clauses`: false. $KB\not\models A$ (world $A$ false, $B$ true).
> From finitely many symbols only finitely many clauses can be built, so a round must at some point add nothing new.
> - A: $B$ and $\neg A$ have no complementary pair, so $\square$ never appears. And resolvents can be longer than their parents.
> - B: The line is right, but clauses do not get shorter in general (see (a) of Q40: 3 and 2 literals give 3).
> - C: The reason is right, but no empty clause is derived.

### Q44 (slide 68)

KB: $B_{2,1}\Leftrightarrow(P_{1,1}\lor P_{2,2}\lor P_{3,1})$, $B_{2,1}$, $\neg P_{1,1}$, $\neg P_{2,2}$ ($P$ = pit, $B$ = breeze). Its CNF contains $\neg B_{2,1}\lor P_{1,1}\lor P_{2,2}\lor P_{3,1}$. To prove $\alpha=P_{3,1}$ by refutation, which clause is added? Then resolve the long clause with one unit clause per step (one complementary pair per step). How many steps until $\square$?

- A) Add $P_{3,1}$. $\square$ after 3 steps.
- B) Add $\neg P_{3,1}$. $\square$ after 3 steps.
- C) Add $\neg P_{3,1}$. $\square$ after 4 steps.
- D) Add $P_{3,1}$. $\square$ after 4 steps.

> [!success]- Answer
> **C.** Refutation adds $\neg\alpha=\neg P_{3,1}$. With $B_{2,1}$: $P_{1,1}\lor P_{2,2}\lor P_{3,1}$. With $\neg P_{1,1}$: $P_{2,2}\lor P_{3,1}$. With $\neg P_{2,2}$: $P_{3,1}$. With $\neg P_{3,1}$: $\square$. Four steps.
> - A: Adding $\alpha$ itself copies the slide's typo "$KB\land\alpha$". That set is satisfiable, so no $\square$ comes.
> - B: 3 steps means two literals were resolved away in one step. One step removes one pair.
> - D: The step count fits $\neg P_{3,1}$, but the added clause must be the negated query.

### Q45 (slide 69)

![[cmpe421_lecture_07_logical_agents_p069.jpg]]

The top row holds the clauses of the KB and the negated query $P_{1,2}$. The second row contains the clause $\neg B_{1,1}\lor P_{1,2}\lor B_{1,1}$. Which statement about this clause is right?

- A) It is true in every world, so it cannot help reach $\square$ and can be dropped.
- B) It is false in every world, so it already is the empty clause $\square$.
- C) $B_{1,1}$ and $\neg B_{1,1}$ cancel inside it, so it is the unit clause $P_{1,2}$.
- D) It is equivalent to $\neg B_{1,1}\lor P_{1,2}$ and can replace that clause.

> [!success]- Answer
> **A.** It contains $B_{1,1}$ and $\neg B_{1,1}$, so one of them is true in every world: a tautology. It says nothing and cannot lead to a contradiction.
> The figure's proof: $\neg P_{1,2}\lor B_{1,1}$ and $\neg B_{1,1}$ give $\neg P_{1,2}$, which with $P_{1,2}$ gives $\square$.
> - B: That is the opposite. An "or" with $\ell$ and $\neg\ell$ is always true, not always false.
> - C: Complementary literals cancel only between two clauses in a resolution step. Inside one clause they make it true.
> - D: $\neg B_{1,1}\lor P_{1,2}$ is false at $B_{1,1}$ true, $P_{1,2}$ false. The tautology is not.

## 12. A wumpus agent in propositional logic (slides 70-78)

### Q46 (slide 70)

$P_{i,j}$ = pit in $[i,j]$, $B_{i,j}$ = breeze in $[i,j]$. Neighbours share a side. Which is the breeze axiom for $[2,3]$ in the $4\times4$ cave, and how many breeze axioms does the cave need in total?

- A) $B_{2,3}\Leftrightarrow(P_{1,3}\land P_{3,3}\land P_{2,2}\land P_{2,4})$. 16 axioms.
- B) $B_{2,3}\Leftrightarrow(P_{1,3}\lor P_{3,3}\lor P_{2,2}\lor P_{2,4})$. 16 axioms.
- C) $B_{2,3}\Leftrightarrow(P_{1,3}\lor P_{3,3}\lor P_{2,2}\lor P_{2,4})$. 24 axioms.
- D) $B_{2,3}\Leftrightarrow(P_{1,3}\land P_{3,3}\land P_{2,2}\land P_{2,4})$. 24 axioms.

> [!success]- Answer
> **B.** A square is breezy when at least one neighbour has a pit: an "or" over the 4 neighbours. There is one axiom per square: 16. The stench axiom for the corner $[4,1]$ is $S_{4,1}\Leftrightarrow(W_{3,1}\lor W_{4,2})$.
> - A: This copies the slide's error ($\land$). One pit is enough for a breeze.
> - C: 24 is the number of neighbour pairs. The axiom is one per square, not one per pair.
> - D: This makes both mistakes.

### Q47 (slide 71)

$W_{i,j}$ = the wumpus is in $[i,j]$. What does the clause $\neg W_{1,1}\lor\neg W_{1,2}$ say, and how many clauses of this kind (one per pair of squares) does the $4\times4$ cave need?

- A) The wumpus is in neither square. 240 clauses.
- B) The wumpus is not in both squares. 240 clauses.
- C) The wumpus is not in both squares. 120 clauses.
- D) The wumpus is in neither square. 120 clauses.

> [!success]- Answer
> **C.** The clause is false only when $W_{1,1}$ and $W_{1,2}$ are both true, so it says "not in both". Unordered pairs: $\binom{16}{2}=\frac{16\cdot15}{2}=120$ (a $3\times3$ cave: $\binom{9}{2}=36$). These say "at most one". The long clause $W_{1,1}\lor\dots\lor W_{4,4}$ adds "at least one".
> - A: Both slips: the clause read as $\neg(W_{1,1}\lor W_{1,2})$, and ordered pairs.
> - B: $16\cdot15=240$ counts each pair twice. The clause for $(a,b)$ is the same as for $(b,a)$.
> - D: This applies De Morgan without flipping: $\neg W_{1,1}\lor\neg W_{1,2}$ is $\neg(W_{1,1}\land W_{1,2})$.

### Q48 (slide 72)

A fluent is a symbol with a time index, used for what can change. Which of these are fluents: the pit in $[3,1]$, the agent's location, the stench percept, the wumpus's square, "the wumpus is alive", "the agent has the arrow"? How many location symbols $L^t_{x,y}$ exist for the $4\times4$ cave and $t=0,\dots,10$?

- A) Location, wumpus alive, has arrow. 176 symbols.
- B) Location, stench percept, wumpus alive, has arrow. 160 symbols.
- C) Location, wumpus alive, has arrow. 160 symbols.
- D) Location, stench percept, wumpus alive, has arrow. 176 symbols.

> [!success]- Answer
> **D.** The stench percept depends on where the agent is at that time, so it is a fluent too. Pits and the wumpus's square never change. $t=0,\dots,10$ are 11 time points: $16\cdot11=176$.
> - A: The percept $\mathit{Stench}^t$ changes as the agent moves. The fixed fact is $S_{x,y}$, not the percept.
> - B: The list is right, but $16\cdot10=160$ forgets $t=0$.
> - C: This makes both mistakes.

### Q49 (slide 73)

Axiom pattern: $L^t_{x,y}\Rightarrow(\mathit{Breeze}^t\Leftrightarrow B_{x,y})$. Here $\mathit{Breeze}^t$ is the percept at time $t$, $B_{x,y}$ = square $[x,y]$ is breezy, $P_{x,y}$ = pit in $[x,y]$. At time 3 the agent is in $[2,1]$ and feels a breeze. What does the KB derive, and why is the premise $L^3_{2,1}$ needed?

- A) $P_{2,1}$. Without $L^3_{2,1}$ the percept is not tied to a square.
- B) $B_{2,1}$. Without $L^3_{2,1}$ the percept is not tied to a square.
- C) $B_{2,1}$. $L^3_{2,1}$ says which time step the breeze belongs to.
- D) $P_{2,1}$. $L^3_{2,1}$ says which time step the breeze belongs to.

> [!success]- Answer
> **B.** $L^3_{2,1}$ is true, so $\mathit{Breeze}^3\Leftrightarrow B_{2,1}$. $\mathit{Breeze}^3$ is true, so $B_{2,1}$. The percept does not say where the breeze is. The location premise links it to one square.
> - A: A breeze means a pit next to the square, not in it. The agent is alive in $[2,1]$.
> - C: The time is already in $\mathit{Breeze}^3$. The premise adds the place.
> - D: This makes both mistakes.

### Q50 (slides 74-75)

The KB has $\mathit{HaveArrow}^0$, $L^0_{1,1}$, $\mathit{East}^0$, $\mathit{Forward}^0$ and the effect axiom $L^0_{1,1}\land\mathit{East}^0\land\mathit{Forward}^0\Rightarrow(L^1_{2,1}\land\neg L^1_{1,1})$. Can it derive $\mathit{HaveArrow}^1$? Then name the problem: (a) "Grab fails if the gold is glued to the floor." (b) "Grab makes a noise."

- A) Not derivable. (a) qualification problem, (b) ramification problem.
- B) Not derivable. (a) ramification problem, (b) qualification problem.
- C) Derivable. (a) qualification problem, (b) ramification problem.
- D) Derivable. (a) ramification problem, (b) qualification problem.

> [!success]- Answer
> **A.** The KB derives $L^1_{2,1}$ and $\neg L^1_{1,1}$. No axiom says that Forward leaves the arrow alone, so $\mathit{HaveArrow}^1$ cannot be derived: the frame problem. (a) A caveat under which the action fails: qualification. (b) A secondary consequence: ramification.
> - B: This swaps the two names.
> - C: Logic does not assume that things stay the same. That needs a successor state axiom.
> - D: This makes both mistakes.

### Q51 (slide 76)

Location axiom (brackets corrected): $L^{t+1}_{1,1}\Leftrightarrow(L^t_{1,1}\land(\neg\mathit{Forward}^t\lor\mathit{Bump}^{t+1}))\lor(L^t_{1,2}\land\mathit{South}^t\land\mathit{Forward}^t)\lor(L^t_{2,1}\land\mathit{West}^t\land\mathit{Forward}^t)$. Is the agent in $[1,1]$ at $t+1$? (a) In $[1,1]$, facing west, Forward, bump. (b) In $[1,2]$, facing south, Forward. (c) In $[1,1]$, facing east, Forward, no bump.

- A) (a) no, (b) yes, (c) no.
- B) (a) no, (b) no, (c) no.
- C) (a) yes, (b) yes, (c) no.
- D) (a) yes, (b) no, (c) no.

> [!success]- Answer
> **C.** (a) $L^t_{1,1}$ true and $\neg\mathit{Forward}^t\lor\mathit{Bump}^{t+1}$ = false $\lor$ true = true: stays. (b) The second part is true: arrives in $[1,1]$. (c) First part: false $\lor$ false; the other parts need $[1,2]$ or $[2,1]$. All false, so by $\Leftrightarrow$ the agent is not in $[1,1]$.
> - A: This ignores the bump. Walking into the wall leaves the agent where it is.
> - B: Both slips: the bump ignored, and south read as a step up.
> - D: South from $[1,2]$ lowers the row: it leads to $[1,1]$, not $[1,3]$.

### Q52 (slide 77)

$\mathit{OK}^t_{x,y}\Leftrightarrow\neg P_{x,y}\land(\neg W_{x,y}\lor\neg\mathit{WumpusAlive}^t)$. $P_{2,2}$ is false, $W_{2,2}$ is true and $\mathit{WumpusAlive}^3$ is false. What is $\mathit{OK}^3_{2,2}$, and why does $\mathit{OK}$ have a time index although $P$ and $W$ have none?

- A) False: the wumpus is in $[2,2]$. Pits can appear and disappear.
- B) True. Pits can appear and disappear.
- C) False: the wumpus is in $[2,2]$. Whether the wumpus is alive can change.
- D) True. Whether the wumpus is alive can change.

> [!success]- Answer
> **D.** $\neg P_{2,2}$ is true. $\neg W_{2,2}\lor\neg\mathit{WumpusAlive}^3$ = false $\lor$ true = true. So $\mathit{OK}^3_{2,2}$ is true: the wumpus is there, but dead. Pits and the wumpus's square never change, but $\mathit{WumpusAlive}$ can (after a shot), so OK can change over time.
> - A: Both slips: the bracket read as $\neg W\land\neg\mathit{Alive}$ (De Morgan without the flip), and changing pits.
> - B: Pits never change: $P_{x,y}$ has no time index.
> - C: The bracket is an "or". A dead wumpus is enough.

### Q53 (slide 78)

The agent starts in $[1,1]$ facing north (up). Actions: $t=0$ Forward, $t=1$ Turn right, $t=2$ Turn right, $t=3$ Forward, $t=4$ Turn left, $t=5$ Forward. A turn is 90 degrees. Percepts: breeze at $t=1,2,3$, stench at $t=6$, nothing else. Where is the agent at $t=4$, and which percept proves that $[2,2]$ has no pit?

- A) $[1,1]$. No breeze at $t=6$, in $[2,1]$.
- B) $[2,2]$. No breeze at $t=6$, in $[2,1]$.
- C) $[1,1]$. No breeze at $t=0$, in $[1,1]$.
- D) $[2,2]$. No breeze at $t=0$, in $[1,1]$.

> [!success]- Answer
> **A.** $t=1$: $[1,2]$. Two right turns: east, then south. $t=4$: Forward south gives $[1,1]$. Turn left: east. $t=6$: $[2,1]$. No breeze in $[2,1]$ clears its neighbour $[2,2]$. Then the breeze in $[1,2]$ forces $P_{1,3}$, and the stench in $[2,1]$ (no stench in $[1,2]$) gives $W_{3,1}$.
> - B: This counts only one right turn, so Forward from $[1,2]$ would go east to $[2,2]$.
> - C: $[2,2]$ is diagonal to $[1,1]$, not a neighbour.
> - D: This makes both mistakes.

## 13. Summary (slide 79)

### Q54 (slide 79)

Three inference methods: truth table enumeration, forward or backward chaining (Modus Ponens on Horn clauses), and resolution. The KB is $A\lor B$, $\neg A$. Which methods can show that $B$ follows?

- A) All three, since $A\lor B$ is a Horn clause.
- B) Truth tables and resolution, but not chaining.
- C) Only resolution: a truth table does not derive sentences.
- D) Only truth tables: resolution needs Horn clauses.

> [!success]- Answer
> **B.** $A\lor B$ has two positive literals, so it is not Horn, and chaining cannot use it. The truth table has one model ($A$ false, $B$ true), and $B$ is true in it. Resolution: $A\lor B$ with $\neg A$ gives $B$. Truth tables and resolution are complete for all of propositional logic.
> - A: A Horn clause has at most one positive literal. $A\lor B$ has two.
> - C: Model checking decides entailment for every KB.
> - D: It is chaining that needs Horn clauses. Resolution works on any CNF.
