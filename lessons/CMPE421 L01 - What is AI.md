---
course: CMPE 421 Artificial Intelligence
lecture: 1
title: What is AI
tags: [cmpe421, lecture]
slides: lectures/lecture_01/Ch.1_AB.pdf
full_notes: lecture_notes/lecture_01/CMPE421_Lecture_01_What_is_AI.pdf
---
# CMPE 421 · Lecture 1 · What is AI?

> [!summary] In short
> - Four definitions of AI: {thinking, acting} × {like humans, rationally}. This course takes **acting rationally**.
> - The Turing test (acting humanly) is not reproducible, not constructive, not amenable to mathematical analysis.
> - Rational = maximise **expected utility**: $\mathrm{EU}(a)=\sum_o P(o\mid a)\,U(o)$, choose $\arg\max_a \mathrm{EU}(a)$.
> - Rationality judges the decision with the information at hand, never the outcome.
> - AI grew out of seven older fields and went through booms and one "AI winter" (1988-93).
> - Tasks with clear rules and a score are solved. Open-ended physical tasks are not.

## 1. Examples and agenda (slides 3-9)
![[cmpe421_lecture_01_ch_1_ab_p003.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p004.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p005.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p006.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p008.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p009.jpg]]

**Notes.** Slides 3 to 6 are photos of AI systems in use (for example a driverless car on slide 3 and a camera drone following a runner on slide 5). The agenda asks three things: what AI is, where it came from and what it can do, and what we should and should not worry about.

The deck has no slide text on the "worry" question. Our notes add their own summary: the real worries are jobs, bias, safety and alignment, misuse and privacy. Conscious evil robots are science fiction. A rational agent maximises exactly the utility we give it, so a wrong utility gives wrong behaviour (the alignment problem).

## 2. Four ways to define AI (slide 10)
![[cmpe421_lecture_01_ch_1_ab_p010.jpg]]

**Notes.** Each definition answers two questions. Do we care about *thinking* (what happens inside) or *acting* (what the machine does)? Do we compare with *humans* or with an ideal standard, *rationality*?

| | like humans | rationally |
|---|---|---|
| **thinking** | cognitive modelling | laws of thought (logic) |
| **acting** | Turing test | **rational agents (this course)** |

**Rational** means doing the right thing with respect to a fixed standard (a goal or a score), given what you know. "Intelligent" is the everyday word and has no fixed yardstick.

## 3. Thinking humanly: cognitive modelling (slides 11-12)
![[cmpe421_lecture_01_ch_1_ab_p011.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p012.jpg]]

**Notes.** A **cognitive model** is a precise, testable theory of how the mind works, detailed enough to run as a program. Two hard questions: at which level do we model (knowledge or circuitry), and how do we validate the model?

| Field | Evidence | Direction |
|---|---|---|
| Cognitive science | human behaviour (choices, errors, reaction times) | top-down |
| Cognitive neuroscience | brain data (scans, recordings) | bottom-up |

Both fields are now separate from AI. All three agree that today's theories do not explain anything like full human intelligence.

## 4. Acting humanly: the Turing test (slides 13-14)
![[cmpe421_lecture_01_ch_1_ab_p013.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p014.jpg]]

**Notes.** Turing (1950, "Computing Machinery and Intelligence") replaced "can machines think?" with an operational test, the **imitation game**. A human interrogator chats by text with a hidden human and a hidden machine. The machine passes if the interrogator cannot reliably tell which is which. Passing needs knowledge, reasoning, language understanding and learning working together.

The slide lists three problems. The test is **not reproducible** (different interrogators, different verdicts), **not constructive** (it says whether you succeeded, not how to build the machine), and **not amenable to mathematical analysis** ("fools a human" is not a quantity to optimise).

> [!warning] Watch out
> Passing is about behaviour only. A machine that fails (a chess engine) can still be very capable, and a machine that passes is not shown to be generally intelligent.

## 5. Thinking rationally: the laws of thought (slides 15-16)
![[cmpe421_lecture_01_ch_1_ab_p015.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p016.jpg]]

**Notes.** Aristotle asked which arguments are correct whatever their topic. That started **logic**. A **syllogism** is an argument pattern that gives a true conclusion from true premises: "Socrates is a man, all men are mortal, so Socrates is mortal". The **logicist tradition** writes knowledge as formulas and lets a program derive new facts (logic programming, Prolog).

*Valid* is about the form of the argument. *True* is about the world. A valid form with a false premise can give a false conclusion.

Problems from the slide: not all intelligent behaviour is mediated by logic (catching a ball, a reflex), and logic does not say what the purpose of thinking is or which thoughts to have.

> [!warning] Watch out
> "All cats are animals. Tom is an animal. So Tom is a cat." is **not** valid: the direction is reversed. Counterexample with the same form: Rex the dog is an animal.

## 6. Acting rationally: rational agents (slides 17-18)
![[cmpe421_lecture_01_ch_1_ab_p017.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p018.jpg]]

**Notes.** **Rational behaviour** is doing the right thing: the action that is *expected* to maximise goal achievement, *given the available information*. It can include thinking, but thinking serves the action. It can also happen without thinking (a reflex).

Two advantages: it is **more general** than the laws-of-thought approach (correct inference is only one way to act well), and it is **more amenable to scientific development** (rationality is precisely defined).

Two limits: rationality is only applicable in ideal environments (in a complex world there is no time to compute the perfect action), and it is not a very good model of real people.

## 7. Rational decisions: maximise expected utility (slides 19-20)
![[cmpe421_lecture_01_ch_1_ab_p019.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p020.jpg]]

**Notes.** The slides fix the technical meaning. Rational means maximally achieving pre-defined goals. It concerns only *which decisions are made*, not the thought process. Goals are expressed as the **utility** of outcomes.

A utility $U(o)$ is a number saying how good outcome $o$ is. If action $a$ leads to outcome $o$ with probability $P(o\mid a)$, its **expected utility** is

$$\mathrm{EU}(a)=\sum_{o}P(o\mid a)\,U(o), \qquad a^{*}=\arg\max_{a}\ \mathrm{EU}(a).$$

$a^{*}$ is the action with the largest expected utility (MEU). The slides say a better course title would be "Computational Rationality".

> [!example] Umbrella (from our notes)
> Chance of rain $p=0.3$. Utilities: take = 70 (rain), 80 (dry). Leave = 0 (rain), 100 (dry).
> $\mathrm{EU}(\text{take})=0.3\cdot70+0.7\cdot80=77$. $\mathrm{EU}(\text{leave})=0.3\cdot0+0.7\cdot100=70$. So **take**.
> Break-even: $80-10p=100-100p \Rightarrow p=2/9\approx0.222$. Take the umbrella above this, leave it below.

> [!warning] Watch out
> Rational is not the same as lucky or successful. Expected utility is a probability-weighted average, not the utility of the most likely outcome. Judging a decision by its outcome is hindsight.

## 8. What about the brain? (slide 25)
![[cmpe421_lecture_01_ch_1_ab_p025.jpg]]

**Notes.** Brains are very good at rational decisions but not perfect. They are not modular like software, so they are hard to reverse-engineer. "Brains are to intelligence as wings are to flight": aeroplanes use the physics of flight without copying feathers, and AI uses the principles of intelligence without copying neurons. Two lessons from the brain: **memory** and **simulation** are key to decision making.

## 9. Foundations of AI (slide 26)
![[cmpe421_lecture_01_ch_1_ab_p026.jpg]]

**Notes.** Seven older fields gave AI its ideas.

| Field | What it gave AI |
|---|---|
| Philosophy | logic, reasoning, mind as a physical system, foundations of learning, language, rationality |
| Mathematics | formal representation and proof, computation, (un)decidability, (in)tractability, probability |
| Psychology | adaptation, perception and motor control |
| Economics | formal theory of rational decisions, game theory |
| Linguistics | knowledge representation, grammar |
| Neuroscience | the physical substrate of mental activity |
| Control theory | homeostatic systems, stability, optimal agent design |

**Decidable**: some program always answers correctly in finite time. **Tractable**: solvable in reasonable (polynomial) time. Every tractable problem is decidable, not the other way round. **Homeostatic**: keeps itself in a stable state, like a thermostat.

## 10. A short history of AI (slides 29-31)
![[cmpe421_lecture_01_ch_1_ab_p031.jpg]]

**Notes.** The history is a cycle of big promises, progress, and disappointment.

| Years | Event |
|---|---|
| 1943 | McCulloch and Pitts: Boolean circuit model of the brain (first artificial neurons) |
| 1950 | Turing's paper |
| 1950s | Samuel's checkers program, Logic Theorist, Geometry Engine |
| 1956 | Dartmouth meeting: the name "Artificial Intelligence" |
| 1965 | Robinson's complete algorithm for logical reasoning (resolution) |
| 1969-79 | early knowledge-based systems |
| 1980-88 | expert-systems industry booms |
| 1988-93 | expert-systems industry busts: the "AI winter" |
| 1990- | statistical approaches: probability, uncertainty, agents and learning |

Knowledge-based systems take rules from people and treat them as certain. Statistical systems take numbers from data and treat the world as uncertain.

## 11. What can AI do? (slides 32-34)
![[cmpe421_lecture_01_ch_1_ab_p032.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p033.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p034.jpg]]

**Notes.** The slide only asks the quiz. The answers are our notes' reading as of 2026, not slide content.

- **Yes**: Jeopardy, chess, Go, real-time speech translation.
- **Partly**: grab a cup, highway and downtown driving, web grocery shopping, proving theorems, surgery (the surgeon controls the robot), a known dishwasher with a person, an intentionally funny story.
- **Not reliably**: tennis, any dishwasher in any home, shopping in a supermarket.

Pattern: clear rules and a clear score make a task easy for AI. A body in an unpredictable physical world makes it hard (Moravec's paradox). Slide 34 shows Kasparov losing to Deep Blue (1997).

**Tale-Spin** (slide 33) planned actions to satisfy character goals. Its plans were valid and its stories absurd: "He ate the beehive" (the honey is inside the hive), "Gravity drowned" (gravity is a force, not a creature). The missing ingredient is **common-sense knowledge**.

> [!warning] Slide typos
> Slide 32 writes "know dishwasher" for "known dishwasher". Slide 33 cites "Shank" for Schank.

## 12. Textbook, reading and related fields (slides 28, 35-37)
![[cmpe421_lecture_01_ch_1_ab_p028.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p035.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p036.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p037.jpg]]

**Notes.** The textbook is Russell and Norvig, *Artificial Intelligence: A Modern Approach* (AIMA), recommended but not required. This lecture is AIMA Ch. 1. One course is a limited view of a vast area. The suggested reading is on embodied cognitive science: intelligence arises from a body interacting with its environment. AI continues in machine learning, automated reasoning, computational linguistics, computer vision and other fields.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Rational | doing the right thing by a fixed standard, given what you know |
| Cognitive model | testable theory of the mind that can run as a program |
| Turing test | interrogator cannot tell machine from human by text chat. Not reproducible, constructive or analysable |
| Syllogism | argument form that gives true conclusions from true premises |
| Rational behaviour | action expected to maximise goal achievement given the available information |
| Utility $U(o)$ | number saying how good outcome $o$ is |
| Expected utility | $\mathrm{EU}(a)=\sum_o P(o\mid a)\,U(o)$ |
| MEU | choose $a^{*}=\arg\max_a \mathrm{EU}(a)$ |
| AI winter | 1988-93, collapse of the expert-systems industry |
| Tale-Spin | 1970s story generator. Shows missing common-sense knowledge |

## Check yourself
> [!question]- 1. Place each system in the 2×2 table: (a) a program that must make the same mistakes as students, (b) a theorem prover, (c) a chatbot built for a Turing-test contest, (d) a chess engine that maximises its winning chance.
> (a) thinking humanly, (b) thinking rationally, (c) acting humanly, (d) acting rationally.

> [!question]- 2. A chatbot passes with interrogator Ali and fails with Berk. Which problem of the Turing test is this? Which problem is "it failed, how do I improve it?"
> Two verdicts for one machine: **not reproducible**. The test gives no hint about what to change: **not constructive**.

> [!question]- 3. Umbrella example with a 10% chance of rain: which action is rational?
> $\mathrm{EU}(\text{take})=0.1\cdot70+0.9\cdot80=79$. $\mathrm{EU}(\text{leave})=0.1\cdot0+0.9\cdot100=90$. **Leave** ($0.1<2/9$).

> [!question]- 4. A sure prize of utility 40, or a fair coin flip for 100 or 0. Which is rational under MEU?
> $\mathrm{EU}(\text{sure})=40$. $\mathrm{EU}(\text{flip})=0.5\cdot100+0.5\cdot0=50$. The **coin flip**.

> [!question]- 5. Route A takes 20 min for sure. Route B takes 10 min with probability 0.6 and 40 min with probability 0.4. $U=100-2t$. Which route is rational? For which jam probability $q$ are they equal?
> $U(20)=60$, $U(10)=80$, $U(40)=20$. $\mathrm{EU}(A)=60$. $\mathrm{EU}(B)=0.6\cdot80+0.4\cdot20=56$. **Route A**.
> In general $\mathrm{EU}(B)=80-60q$. Equal when $80-60q=60$, so $q=1/3$. B is better for $q<1/3$.

> [!question]- 6. A cleaning robot gets utility +1 for each piece of dirt it picks up. How can a perfectly rational robot behave badly?
> Only the count of pick-ups is rewarded. Dumping the dirt and picking it up again scores without end, so MEU prefers it. This is the safety and alignment problem: the robot does what we told it, not what we meant.

## Links
- Course index: [[CMPE421 Home]]
- Next lecture: [[CMPE421 L02 - Intelligent agents]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_01_What_is_AI.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_01/CMPE421_Lecture_01_What_is_AI.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_01/CMPE421_Lecture_01_What_is_AI.pdf`
- Original slides (PDF): [Ch.1_AB.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_01/Ch.1_AB.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_01/Ch.1_AB.pdf`

Slides not embedded: 1, 2, 7 (titles), 21-24 (repeats of slide 20), 27 (laptop policy), 29 (section title), 30 (video placeholder).
