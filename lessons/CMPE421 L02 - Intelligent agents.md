---
course: CMPE 421 Artificial Intelligence
lecture: 2
title: Intelligent agents
tags: [cmpe421, lecture]
slides: lectures/lecture_02/Ch.2.pdf
full_notes: lecture_notes/lecture_02/CMPE421_Lecture_02_Intelligent_Agents.pdf
---
# CMPE 421 · Lecture 2 · Intelligent agents

> [!summary] In short
> - An agent perceives through sensors and acts through actuators. Agent = architecture + program.
> - The agent function $f:\mathcal P^{*}\to\mathcal A$ maps whole percept histories to actions. The agent program gets one percept at a time.
> - Rational = for each percept sequence, the action with the highest *expected* performance, given percepts and built-in knowledge.
> - Describe a task with PEAS, then classify the environment on six dimensions.
> - Agent programs: table-driven (doomed), simple reflex, model-based reflex, goal-based, utility-based. Any of them can be a learning agent.

Main deck: `Ch.2.pdf` (70 pages). Slide numbers below are the page numbers of this PDF. Section 13 adds four slides from the handout `intelligent-agents.pdf`.

## 1. Agents and rational agents (slides 2-3)
![[cmpe421_lecture_02_ch_2_p002.jpg]]
![[cmpe421_lecture_02_ch_2_p003.jpg]]

**Notes.** An **agent** perceives its **environment** through **sensors** and acts on it through **actuators**. What it senses at one moment is a **percept**. The "?" in the picture is the agent program, the part we design. A **rational agent** selects actions that maximise its expected utility. The percepts, the environment and the action space decide which technique fits. Pac-Man is the running example: percepts are the maze, dots and ghosts, and actions are the four moves.

## 2. Agents in action (slides 5-17)
![[cmpe421_lecture_02_ch_2_p005.jpg]]
![[cmpe421_lecture_02_ch_2_p006.jpg]]
![[cmpe421_lecture_02_ch_2_p007.jpg]]
![[cmpe421_lecture_02_ch_2_p008.jpg]]
![[cmpe421_lecture_02_ch_2_p009.jpg]]
![[cmpe421_lecture_02_ch_2_p010.jpg]]
![[cmpe421_lecture_02_ch_2_p011.jpg]]
![[cmpe421_lecture_02_ch_2_p012.jpg]]
![[cmpe421_lecture_02_ch_2_p013.jpg]]
![[cmpe421_lecture_02_ch_2_p014.jpg]]
![[cmpe421_lecture_02_ch_2_p015.jpg]]
![[cmpe421_lecture_02_ch_2_p016.jpg]]
![[cmpe421_lecture_02_ch_2_p017.jpg]]

**Notes.** These slides are examples, mostly pictures and videos.

- **Deep Blue (May 1997)** beat Kasparov, the first match won against a reigning world champion. It searched about 200 million positions per second.
- **Reinforcement learning** agents learn Atari games from the pixels and the score. Simulated robots learn to walk.
- **Robotics** is part mechanical engineering, part AI. Reality is much harder than simulation. This course ignores the mechanics and studies planning and control.
- **Natural language** (speech recognition, translation, spam filtering) and **computer vision** (describing an image) are the other application areas.

| Course topic | Typical application |
|---|---|
| Constraint satisfaction | scheduling |
| Search, planning, reinforcement learning | routing, robot navigation |
| Probabilistic inference | robot localisation |
| A bit of supervised learning | spam detection |

## 3. Agent function and agent program (slides 18-21)
![[cmpe421_lecture_02_ch_2_p018.jpg]]
![[cmpe421_lecture_02_ch_2_p019.jpg]]
![[cmpe421_lecture_02_ch_2_p020.jpg]]
![[cmpe421_lecture_02_ch_2_p021.jpg]]

**Notes.** $\mathcal P$ is the set of possible percepts, $\mathcal P^{*}$ the set of all finite percept sequences, $\mathcal A$ the set of actions. The **agent function** maps every percept sequence to an action:

$$f:\ \mathcal P^{*}\to\mathcal A.$$

The **agent program** is the code that implements $f$. It runs on the **architecture** (the computing device with its sensors and actuators).

| | Agent function | Agent program |
|---|---|---|
| What it is | a mathematical map (possibly infinite table) | a finite piece of code |
| Input | the whole percept sequence | only the current percept |
| Memory | not needed | must store the history it needs |

An agent can perceive its own actions, but not always their effects.

## 4. The vacuum-cleaner world (slides 22-24)
![[cmpe421_lecture_02_ch_2_p022.jpg]]
![[cmpe421_lecture_02_ch_2_p023.jpg]]
![[cmpe421_lecture_02_ch_2_p024.jpg]]

**Notes.** Two squares A and B, each clean or dirty. Percept: location and status of that square, for example $[A, \mathit{Dirty}]$. Actions: Left, Right, Suck, NoOp.

There are $2\times2=4$ percepts but $2\times2\times2=8$ world states, because the percept hides the other square. The table of the agent function never ends: there are $4^{t}$ sequences of length $t$. The three-line reflex program replaces it because it reads only the current percept.

```python
def reflex_vacuum_agent(percept):
    location, status = percept
    if status == "Dirty":
        return "Suck"
    elif location == "A":
        return "Right"
    elif location == "B":
        return "Left"
```

## 5. Rationality (slides 25-28)
![[cmpe421_lecture_02_ch_2_p025.jpg]]
![[cmpe421_lecture_02_ch_2_p026.jpg]]
![[cmpe421_lecture_02_ch_2_p027.jpg]]
![[cmpe421_lecture_02_ch_2_p028.jpg]]

**Notes.** A **performance measure** is a fixed, objective score on the sequence of environment states. The designer chooses it. Measure **what you want in the environment**, not how you think the agent should behave. "Dirt cleaned" is a bad measure: the agent can dump the dirt and clean it again for more points.

What is rational depends on four things: the performance measure, prior knowledge of the environment, the available actions, and the percept sequence to date.

Rational is not **omniscient** (knowing the actual outcome) and not **perfect** (maximising actual performance). Rationality maximises *expected* performance. It requires information gathering, learning from percepts, and autonomy.

> [!example] Is the reflex vacuum agent rational?
> Both squares dirty, start in A, 6 steps. Run: Suck, Right, Suck, Left, Right, Left.
> - Measure "one point per clean square per step, moves free": $1+1+2+2+2+2=10$, the best possible. **Rational.**
> - Same measure minus one per move: score 6. An agent that does NoOp once both squares are known clean scores 9. **Not rational.**
>
> To stop safely the agent must remember that A is clean. That needs internal state.

## 6. PEAS (slides 29-30)
![[cmpe421_lecture_02_ch_2_p029.jpg]]
![[cmpe421_lecture_02_ch_2_p030.jpg]]

**Notes.** A task environment is described by **PEAS**: Performance measure, Environment, Actuators, Sensors.

| | Automated taxi |
|---|---|
| P | safety, destination, profits, legality, comfort |
| E | streets and freeways, other traffic, pedestrians, weather |
| A | steering, accelerator, brake, horn, speaker/display |
| S | video, sonar, speedometer, engine sensors, keyboard, GPS |

> [!warning] Watch out
> P is the designer's score, computed from the world from outside. S is what the agent can read. "Passenger comfort" is in P. The taxi senses it only indirectly.

## 7. Fully or partially observable (slides 31-34)
![[cmpe421_lecture_02_ch_2_p031.jpg]]
![[cmpe421_lecture_02_ch_2_p034.jpg]]

**Notes.** **Fully observable**: the sensors give the complete state at each time, or at least everything relevant to the choice of action. Then the agent needs no internal state. **Partially observable**: sensors are noisy, or parts of the state are missing. The vacuum agent with a local dirt sensor cannot see the other square, so its world is partially observable.

## 8. Deterministic, stochastic, strategic (slides 35-37)
![[cmpe421_lecture_02_ch_2_p035.jpg]]
![[cmpe421_lecture_02_ch_2_p037.jpg]]

**Notes.** **Deterministic**: the next state is completely determined by the current state and the agent's action. Otherwise **stochastic**. **Strategic**: deterministic except for the actions of other agents (chess). Taxi driving is stochastic.

> [!warning] Watch out
> A partially observable environment can *appear* stochastic. Unpredictable results do not prove the world is stochastic. It may be deterministic with hidden parts.

## 9. Episodic or sequential (slides 38-41)
![[cmpe421_lecture_02_ch_2_p038.jpg]]
![[cmpe421_lecture_02_ch_2_p040.jpg]]
![[cmpe421_lecture_02_ch_2_p041.jpg]]

**Notes.** **Episodic**: experience splits into atomic episodes. In each one the agent perceives and performs a single action, and the next episode does not depend on earlier actions. Example: spotting defective parts on an assembly line. **Sequential**: the current decision can affect all future decisions (taxi, chess). Episodic environments are simpler because the agent does not need to think ahead.

## 10. Static, discrete, single-agent (slides 42-50)
![[cmpe421_lecture_02_ch_2_p042.jpg]]
![[cmpe421_lecture_02_ch_2_p044.jpg]]
![[cmpe421_lecture_02_ch_2_p045.jpg]]
![[cmpe421_lecture_02_ch_2_p047.jpg]]
![[cmpe421_lecture_02_ch_2_p048.jpg]]
![[cmpe421_lecture_02_ch_2_p050.jpg]]

**Notes.**
- **Dynamic**: the environment can change while the agent deliberates. Otherwise **static**. **Semi-dynamic**: the environment does not change but the performance score does (chess with a clock).
- **Discrete or continuous** applies to the state, to time, and to percepts and actions. Chess is discrete. Taxi driving is continuous in all three.
- **Multi-agent**: there are other agents whose performance measure depends on our actions. Chess is competitive. Taxi driving is partially cooperative. A crossword is single-agent.

The finished table of the deck:

| | Solitaire | Backgammon | Internet shopping | Taxi |
|---|---|---|---|---|
| Observable | Full | Full | Partial | Partial |
| Deterministic | Yes | No | Yes | No |
| Episodic | No | No | No | No |
| Static | Yes | Yes | Semi | No |
| Discrete | Yes | Yes | Yes | No |
| Single-agent | Yes | No | No | No |

> [!warning] Watch out
> Not every moving object is an agent. A falling leaf is part of the environment. Another car is an agent because its driver's performance depends on what you do.

## 11. More environments (slides 51, 53)
![[cmpe421_lecture_02_ch_2_p051.jpg]]
![[cmpe421_lecture_02_ch_2_p053.jpg]]

**Notes.** Entries that are often answered wrongly:
- **Poker**: partially observable (hidden hands) and stochastic (the deal).
- **Medical diagnosis**: single-agent.
- **Image analysis**: episodic, and semi (the image does not change but taking longer lowers the score).
- **Part-picking robot**: episodic, each part is a fresh decision.

The simplest environment is fully observable, deterministic, episodic, static, discrete and single-agent. Most real ones are the opposite on every line.

## 12. Agent programs and the table-driven agent (slides 52-56)
![[cmpe421_lecture_02_ch_2_p052.jpg]]
![[cmpe421_lecture_02_ch_2_p054.jpg]]
![[cmpe421_lecture_02_ch_2_p055.jpg]]
![[cmpe421_lecture_02_ch_2_p056.jpg]]

**Notes.** All agent programs have the same skeleton: input = current percept, output = action. The **table-driven agent** stores the whole percept sequence and looks it up in a table. It can implement any agent function, but the table is too large. With $|\mathcal P|$ possible percepts and a lifetime of $T$ steps:

$$\text{rows}=\sum_{t=1}^{T}|\mathcal P|^{t}=\frac{|\mathcal P|\left(|\mathcal P|^{T}-1\right)}{|\mathcal P|-1}.$$

> [!example] Vacuum world, $|\mathcal P|=4$
> $T=3$: $4+16+64=84$ rows. $T=10$: $\dfrac{4\,(4^{10}-1)}{3}=\dfrac{4\cdot 1\,048\,575}{3}=1\,398\,100$ rows. The reflex agent needs 4 rules for any $T$.

Why it is doomed: no agent can store the table, the designer cannot write it, the agent could never learn all entries, and nobody knows how to fill it in.

## 13. Simple reflex agents (slides 57-60)
![[cmpe421_lecture_02_ch_2_p057.jpg]]
![[cmpe421_lecture_02_ch_2_p058.jpg]]
![[cmpe421_lecture_02_ch_2_p059.jpg]]
![[cmpe421_lecture_02_ch_2_p060.jpg]]

**Notes.** A **simple reflex agent** chooses on the **current percept only**, with **condition-action rules** ("if dirty then suck", "if car-in-front-is-braking then initiate-braking"). `INTERPRET-INPUT` describes the situation, `RULE-MATCH` finds the first matching rule, `RULE-ACTION` reads off its action.

It works only if the right decision can be made from the current percept, so only if the environment is **fully observable**. Otherwise infinite loops can occur. Example: a vacuum agent with no location sensor does Left on $[\mathit{Clean}]$ forever and never reaches B. Randomising the move is one way out. Memory is the other.

> [!warning] Slide slips
> Slide 59 writes "4T" for $4^{T}$. Slide 60 writes `RULE-MATCH(state, rule)`. The second argument is the rule set `rules`.

## 14. Model-based reflex agents (slides 61-63)
![[cmpe421_lecture_02_ch_2_p061.jpg]]
![[cmpe421_lecture_02_ch_2_p062.jpg]]
![[cmpe421_lecture_02_ch_2_p063.jpg]]

**Notes.** A **model-based reflex agent** keeps an **internal state** that depends on the percept history and tracks the part of the world it cannot see now. To update the state it needs a **model of the world**: how the world evolves by itself, and how the agent's actions affect it. Example: when changing lanes, keep track of the cars you cannot see.

The **state** is what the agent believes now. It changes every step. The **model** is the fixed knowledge of how the world changes. The only change from the simple reflex program is the line `state ← UPDATE-STATE(state, action, percept)`.

A model-based vacuum agent that remembers which squares are clean and does NoOp when both are clean scores 9 in the run of section 5, against 6 for the simple reflex agent.

## 15. Goal-based and utility-based agents (slides 64-67)
![[cmpe421_lecture_02_ch_2_p064.jpg]]
![[cmpe421_lecture_02_ch_2_p065.jpg]]
![[cmpe421_lecture_02_ch_2_p066.jpg]]
![[cmpe421_lecture_02_ch_2_p067.jpg]]

**Notes.** A **goal-based agent** has goal information and asks "what will happen if I do action $a$, and does it bring me closer to the goal?". The future is taken into account. It is flexible: to send a taxi somewhere new you change the goal, not the rules. Finding long action sequences is the job of **search** and **planning**.

A **utility function** maps a state (or a sequence of states) to a real number. A **utility-based agent** picks the action with the highest expected utility. Utility helps when goals conflict (speed and safety) and when several goals are uncertain.

| | What it is | Who uses it |
|---|---|---|
| Goal | yes/no test on a state | goal-based agent |
| Utility | a real number per state | utility-based agent |
| Performance measure | a score on the whole sequence of states | the designer |

> [!example] Goal versus utility
> Route 1: 20 min for sure. Route 2: 15 min, with probability 0.1 a jam adds 40 min. $U=-\text{minutes}$.
> The goal-based agent has no preference. $\mathrm{EU}_1=-20$. $\mathrm{EU}_2=0.9\cdot(-15)+0.1\cdot(-55)=-19$. The utility-based agent picks **route 2**.

## 16. Learning agents (slides 68-70)
![[cmpe421_lecture_02_ch_2_p068.jpg]]
![[cmpe421_lecture_02_ch_2_p069.jpg]]
![[cmpe421_lecture_02_ch_2_p070.jpg]]

**Notes.** The earlier designs say how to select actions, not where the program comes from. A **learning agent** has four components:

| Component | Job | Learning taxi |
|---|---|---|
| Performance element | selects actions (one of the earlier programs) | the driving program |
| Critic | gives feedback against a fixed performance standard | "that braking was bad" |
| Learning element | improves the performance element | "brake earlier" |
| Problem generator | suggests new, informative experiences | try a wet road |

**Exploitation** is doing what looks best now. **Exploration** is trying something else to learn more. The problem generator is the source of exploration.

> [!warning] Watch out
> The performance standard is outside the agent and fixed. The critic judges, the learning element changes the program. A learning agent is not a fifth kind: any of the four kinds can be made to learn. Slide 70 starts with a sentence left over from slide 67 ("Goals alone are not...").

## 17. From the handout `intelligent-agents.pdf` (handout pages 5, 9, 16, 20)
![[cmpe421_lecture_02_intelligent_agents_p005.jpg]]
![[cmpe421_lecture_02_intelligent_agents_p009.jpg]]
![[cmpe421_lecture_02_intelligent_agents_p016.jpg]]
![[cmpe421_lecture_02_intelligent_agents_p020.jpg]]

**Notes.** The handout (CMSC 421, Russell and Norvig) adds four things.

- **Three performance measures** for the vacuum world: one point per square cleaned in time $T$, one point per clean square per step minus one per move, a penalty for more than $k$ dirty squares. It also adds: rational is not **clairvoyant** (outcomes may differ from what was expected), so rational is not successful.
- **Internet shopping PEAS**: P = price, quality, appropriateness, efficiency. E = WWW sites, vendors, shippers. A = display to user, follow URL, fill in form. S = HTML pages.
- **A vacuum agent with state** that cannot sense its room but remembers its location. It assumes it starts in A. If it really starts in B, its belief is wrong and it wastes moves (score 3 instead of 4 after 4 steps in our notes' run).

> [!warning] The two decks disagree on the environment table
> | | Deck | Handout |
> |---|---|---|
> | Solitaire, observable | Full | No |
> | Solitaire, deterministic | Yes | Yes, after the cards are dealt |
> | Internet shopping, deterministic | Yes | Partly |
> | Internet shopping, static | Semi | Partly |
> | Backgammon, static | Yes | Partly |
>
> The answers depend on what counts as the state (for example the face-down cards in Solitaire). In an exam, give the deck's answer and state the assumption in one line.

> [!warning] Handout slip
> The handout's `Vacuum-Agent-With-State([location])` tests `status`. The real percept is the dirt status. `location` is the agent's memory.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Agent | perceives through sensors, acts through actuators. Agent = architecture + program |
| Agent function | $f:\mathcal P^{*}\to\mathcal A$ |
| Performance measure | fixed score on the sequence of environment states |
| Rational agent | maximises expected performance given the percept sequence and built-in knowledge |
| PEAS | Performance measure, Environment, Actuators, Sensors |
| Strategic | deterministic except for other agents' actions |
| Semi-dynamic | world static, score changes with time |
| Table-driven rows | $\sum_{t=1}^{T}\lvert\mathcal P\rvert^{t}$ |
| Simple reflex | condition-action rules on the current percept. Needs full observability |
| Model-based reflex | adds internal state and a model of the world |
| Goal-based | adds goals and prediction of action results |
| Utility-based | adds a real number per state |
| Learning agent | performance element, learning element, critic, problem generator |

## Check yourself
> [!question]- 1. List the four things rationality depends on.
> Performance measure, prior knowledge of the environment, available actions, percept sequence to date.

> [!question]- 2. How many rows does a table-driven agent need for $|\mathcal P|=3$, $T=4$? And for the vacuum world with $T=5$?
> $3+9+27+81=120$. Vacuum: $4+16+64+256+1024=1364$, check $\dfrac{4(4^{5}-1)}{3}=1364$.

> [!question]- 3. Why is chess with a clock "semi" and not "dynamic"?
> While a player thinks the board does not change. Only the remaining time, which is part of the score, changes. Dynamic would need the board itself to change.

> [!question]- 4. Classify a Sudoku solver on the six dimensions.
> Fully observable, deterministic, sequential (each filled cell restricts later ones), static, discrete, single-agent. Same row as the crossword.

> [!question]- 5. In the taxi example, for which jam probability $q$ are the two routes equally good?
> $\mathrm{EU}_2=(1-q)(-15)+q(-55)=-15-40q$. Set equal to $-20$: $q=0.125$. Below that, route 2 wins.

> [!question]- 6. A chess program loses to a stronger program. Is it therefore not rational?
> No. Rational means maximising expected performance with the available knowledge. Losing is an outcome, and outcomes do not decide rationality.

## Links
- Course index: [[CMPE421 Home]]
- Previous: [[CMPE421 L01 - What is AI]] · Next: [[CMPE421 L03 - Uninformed search]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_02_Intelligent_Agents.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_02/CMPE421_Lecture_02_Intelligent_Agents.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_02/CMPE421_Lecture_02_Intelligent_Agents.pdf`
- Original slides (PDF): [Ch.2.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_02/Ch.2.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_02/Ch.2.pdf`
- Handout (PDF, 24 pages): [intelligent-agents.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_02/intelligent-agents.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_02/intelligent-agents.pdf`

Slides not embedded (Ch.2): 1 (title), 4 (media placeholder), 32, 33, 36, 39, 43, 46, 49 (earlier builds of the environment table). Handout: only pages 5, 9, 16, 20 are embedded. The rest repeats the deck.
