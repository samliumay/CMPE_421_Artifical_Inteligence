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

> [!info] Narrow and general AI (beyond the slides)
> **Narrow AI** does one task, or one small family of tasks, well: drive a car, follow a runner with a camera, play chess. It cannot use its skill for a different task.
> **General AI** would do any intellectual task that a person can do, and carry what it learned from one task over to another. No such system is shown in this lecture.
> Every machine on slides 3 to 6 is narrow AI.

> [!question]- Check (slides 3-4): Slide 3 shows a driverless car and slide 4 a hospital robot that carries drugs along a corridor. For each machine, name one thing it must sense, one action it can take, and what "doing the job well" means. Must the car drive like a human to be useful?
> Car: it senses the road, other cars and people (the sensor is on its roof). It steers, brakes and accelerates. Doing well means arriving safely, legally and in good time.
> Hospital robot: it senses walls, doors and people in the corridor. It moves, stops and turns. Doing well means delivering the right drugs to the right place without hitting anyone.
> The car does not have to drive like a human. A human driver gets tired and makes mistakes. We want the car to do the right thing, not the human thing. This is the idea of "acting rationally" in section 2.

> [!question]- Check (slides 5-6): The same camera drone follows a runner in a forest (slide 5) and films a wedding (slide 6). In which scene is it harder to keep the person in view and to fly safely, and why? Is the drone narrow or general AI?
> The forest is harder. The runner moves fast, the trees hide her from time to time, and the trees are obstacles, so the drone must keep finding her and plan a safe path in real time. At the wedding the people move slowly and the space is open. The main demand there is to keep a safe distance from the guests.
> The drone is narrow AI. It tracks and films. It cannot drive the car of slide 3 or answer a question.

> [!question]- Check (slides 8-9): The agenda asks what we should and should not worry about. Sort these four into "real worry" or "science fiction", and name the kind of worry: (a) a hiring program trained on old decisions rejects women more often, (b) a chatbot wakes up one night and starts to hate its users, (c) a criminal copies a voice and phones the victim's family for money, (d) lorry drivers lose work to driverless lorries.
> (a) Real: bias and fairness. (b) Science fiction: a conscious machine with its own feelings. (c) Real: misuse. (d) Real: jobs.
> The slides only ask the question. The lists come from our notes: real worries are jobs, bias, safety and alignment, misuse and privacy.

## 2. Four ways to define AI (slide 10)
![[cmpe421_lecture_01_ch_1_ab_p010.jpg]]

**Notes.** Each definition answers two questions. Do we care about *thinking* (what happens inside) or *acting* (what the machine does)? Do we compare with *humans* or with an ideal standard, *rationality*?

| | like humans | rationally |
|---|---|---|
| **thinking** | cognitive modelling | laws of thought (logic) |
| **acting** | Turing test | **rational agents (this course)** |

**Rational** means doing the right thing with respect to a fixed standard (a goal or a score), given what you know. "Intelligent" is the everyday word and has no fixed yardstick.

> [!question]- Check (slide 10): An autopilot lands a plane more smoothly than any human pilot. It uses calculations that no pilot does in his head. Under which of the four definitions is this a success, and under which is it not?
> **Acting rationally**: success. It does the right thing (a safe, smooth landing) by a fixed standard.
> **Acting humanly**: not a success. It does not behave like a human pilot, it behaves better and differently.
> **Thinking humanly**: not a success. Its inner steps are not the steps of a human mind.
> **Thinking rationally**: only if we look inside and find correct reasoning. The landing alone does not show it, because this definition is about the thinking and not about the result.

## 3. Thinking humanly: cognitive modelling (slides 11-12)
![[cmpe421_lecture_01_ch_1_ab_p011.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p012.jpg]]

**Notes.** A **cognitive model** is a precise, testable theory of how the mind works, detailed enough to run as a program. Two hard questions: at which level do we model (knowledge or circuitry), and how do we validate the model?

| Field | Evidence | Direction |
|---|---|---|
| Cognitive science | human behaviour (choices, errors, reaction times) | top-down |
| Cognitive neuroscience | brain data (scans, recordings) | bottom-up |

Both fields are now separate from AI. All three agree that today's theories do not explain anything like full human intelligence.

> [!question]- Check (slide 11, validation): A program solves arithmetic puzzles. It is always correct and ten times faster than people. Its author says: "So it is a good cognitive model." Find the mistake. How would you validate a cognitive model, according to the slide?
> A cognitive model is a theory of how *humans* think. It is good when it behaves like people, not when it performs well. People are slow on hard puzzles and make typical errors, so a good model must be slow there and make the same errors.
> The slide gives two ways to validate: predict human behaviour and test the prediction (errors, reaction times), or identify the model from neurological data (brain measurements).

> [!question]- Check (slide 11, level): Two teams model how people recognise a word. Team A writes rules about letters, word lists and meanings. Team B simulates 10 000 neurons and their connections. Which "level of abstraction" does each team choose, and which field (cognitive science or cognitive neuroscience) is closer to each?
> Team A models **knowledge**. This is the top-down way of cognitive science, which starts from behaviour.
> Team B models **circuitry**. This is the bottom-up way of cognitive neuroscience, which starts from brain data.
> The slide asks this as an open question. A model can be right at one level and say nothing about the other.

## 4. Acting humanly: the Turing test (slides 13-14)
![[cmpe421_lecture_01_ch_1_ab_p013.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p014.jpg]]

**Notes.** Turing (1950, "Computing Machinery and Intelligence") replaced "can machines think?" with an operational test, the **imitation game**. A human interrogator chats by text with a hidden human and a hidden machine. The machine passes if the interrogator cannot reliably tell which is which. Passing needs knowledge, reasoning, language understanding and learning working together.

The slide lists three problems. The test is **not reproducible** (different interrogators, different verdicts), **not constructive** (it says whether you succeeded, not how to build the machine), and **not amenable to mathematical analysis** ("fools a human" is not a quantity to optimise).

> [!warning] Watch out
> Passing is about behaviour only. A machine that fails (a chess engine) can still be very capable, and a machine that passes is not shown to be generally intelligent.

> [!info] Reading the figure on slide 13 (beyond the slides)
> The figure has no labels. The person on the left of the wall is the **interrogator** at a terminal. Behind the wall, the top right shows the hidden **human** at a terminal and the grey box at the bottom right is the hidden **machine**. The two lines are the text channels. The interrogator sees only text, so voice and appearance play no role.
> "Operational" means: defined by a procedure that you can carry out. Turing does not define thinking. He gives a procedure and calls its result the answer.

> [!question]- Check (slide 13): The interrogator types "What is 48 271 times 9 316?". A machine answers correctly in 0.1 seconds. Does this help the machine to pass? What does your answer say about what the Turing test measures?
> No. A person needs minutes and often makes a mistake, so the instant correct answer shows which player is the machine. To pass, the machine must wait, or even answer wrongly.
> So the test measures how human the behaviour looks. It does not measure how capable the machine is. A machine may have to hide an ability to pass.

> [!question]- Check (slide 14): A speech recogniser is scored by its word error rate: on a fixed test recording of 400 words it gets 12 words wrong. Compute the rate. Then compare this score with "passes the Turing test" on the three problems of the slide.
> Rate $=12/400=0.03=3\%$.
> Reproducible: the same recording and the same program give 3% again. A Turing-test verdict depends on the interrogator.
> Constructive: the 12 wrong words show where the system fails, so they point to what to improve. A failed Turing test does not say what to change.
> Mathematical analysis: the error rate is a number. It can be compared, averaged and minimised. "Fools a human" is not such a quantity.

## 5. Thinking rationally: the laws of thought (slides 15-16)
![[cmpe421_lecture_01_ch_1_ab_p015.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p016.jpg]]

**Notes.** Aristotle asked which arguments are correct whatever their topic. That started **logic**. A **syllogism** is an argument pattern that gives a true conclusion from true premises: "Socrates is a man, all men are mortal, so Socrates is mortal". The **logicist tradition** writes knowledge as formulas and lets a program derive new facts (logic programming, Prolog).

*Valid* is about the form of the argument. *True* is about the world. A valid form with a false premise can give a false conclusion.

Problems from the slide: not all intelligent behaviour is mediated by logic (catching a ball, a reflex), and logic does not say what the purpose of thinking is or which thoughts to have.

> [!warning] Watch out
> "All cats are animals. Tom is an animal. So Tom is a cat." is **not** valid: the direction is reversed. Counterexample with the same form: Rex the dog is an animal.

> [!question]- Check (slide 15): (a) "All birds can fly. A penguin is a bird. So a penguin can fly." Is the argument valid? Is the conclusion true? (b) A goalkeeper dives to the correct corner 0.3 seconds after the shot. Which of the slide's two problems does this show?
> (a) The argument is **valid**: it has the same form as the Socrates syllogism, so if both premises were true the conclusion could not be false (this is what the slide calls "irrefutable"). The conclusion is **false**, because the first premise is false. Logic guarantees the step, not the premises.
> (b) "Not all intelligence is mediated by logic." The goalkeeper acts well without deriving the dive from premises. There is no time for it.

> [!question]- Check (slide 16): The slide points to Prolog. A Prolog program holds the facts `parent(ali, berk).` and `parent(berk, can).` and the rule `grandparent(X, Z) :- parent(X, Y), parent(Y, Z).` What does the query `grandparent(ali, W).` return, and why is this an example of the logicist tradition? Which slide-15 problem remains?
> It returns `W = can`. The rule reads: X is a grandparent of Z if X is a parent of some Y and Y is a parent of Z. With X = ali the first fact gives Y = berk, and the second fact gives Z = can.
> The programmer wrote only knowledge (facts and a rule). The program derived a new fact by correct inference. That is the logicist idea.
> The remaining problem: the program can derive many true facts, but logic does not tell it which ones are worth deriving. Someone has to ask the query. "What thought should one have?" stays open.

## 6. Acting rationally: rational agents (slides 17-18)
![[cmpe421_lecture_01_ch_1_ab_p017.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p018.jpg]]

**Notes.** **Rational behaviour** is doing the right thing: the action that is *expected* to maximise goal achievement, *given the available information*. It can include thinking, but thinking serves the action. It can also happen without thinking (a reflex).

Two advantages: it is **more general** than the laws-of-thought approach (correct inference is only one way to act well), and it is **more amenable to scientific development** (rationality is precisely defined).

Two limits: rationality is only applicable in ideal environments (in a complex world there is no time to compute the perfect action), and it is not a very good model of real people.

> [!info] Rational is not perfect and not all-knowing (beyond the slides)
> The slide's definition has two guards. "**Expected** to maximise" means the agent is judged before the result is known. "**Given the available information**" means it is judged only on what it could know.
> An **omniscient** agent would know the real outcome of every action in advance. A **perfect** agent would always get the best real outcome. Neither is possible in an uncertain world, and rationality asks for neither. It asks for the best choice with the knowledge at hand.
> A rational agent may still have a duty to look first. If looking is cheap and the information matters, collecting it is part of doing the right thing.

> [!question]- Check (slide 17): Deniz looks left and right, sees an empty street and crosses. A tile falls from a roof and hits her. Emre crosses a busy street without looking and arrives safely. Who acted rationally? Use the words of the slide.
> Deniz acted rationally. With the available information (an empty street) crossing was expected to reach her goal safely. Nobody could know about the tile. Her result was bad luck.
> Emre did not act rationally. He could get the information cheaply by looking and did not. With what he knew, crossing was not the action expected to maximise goal achievement. His good result was luck.
> Rationality is judged by expectation and available information, not by the outcome.

> [!question]- Check (slide 17, limits): A chess program has one second per move. In one second it cannot search all continuations, so it cannot find the provably best move. Which sentence of the slide describes this situation? Does the program then stop being useful?
> "Rationality is only applicable in ideal environments." Perfect rationality needs enough time and computing power to find the best action. A real environment does not give them.
> The program is still useful. It picks the best move it can find in one second. This is acting as well as possible with limited resources, sometimes called limited rationality (beyond the slides).

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

> [!question]- Check (slide 19): Program A stores a huge table "situation → action". Program B reasons step by step with logic. In every situation both choose the same action. Which program is more rational in the slide's technical sense? Which of the four definitions of AI would see a difference between them?
> They are equally rational. The slide says rationality only concerns what decisions are made, not the thought process behind them. Same decisions, same rationality.
> The two "thinking" definitions see a difference. Thinking rationally asks whether the inner reasoning is correct, and thinking humanly asks whether it is like a human's. Program B has reasoning to inspect, program A has none.

> [!question]- Check (slide 20): The hospital robot of slide 4 can take the lift or the ramp. Lift: it arrives fast with probability 0.8 (utility 50) and gets stuck with probability 0.2 (utility −100). Ramp: it arrives slowly for sure (utility 30). Which action maximises expected utility? At which success probability $p$ of the lift are the two equal?
> $\mathrm{EU}(\text{lift})=0.8\cdot50+0.2\cdot(-100)=40-20=20$. $\mathrm{EU}(\text{ramp})=1\cdot30=30$. Since $30>20$, the robot takes the **ramp**.
> The most likely result of the lift (utility 50) is better than the ramp (30). The lift still loses, because the rare bad outcome is very bad and the average counts it.
> Equal when $50p-100(1-p)=30$, so $150p-100=30$, $150p=130$, $p=13/15\approx0.867$. The lift is the rational choice only if it works more than about 87% of the time.

## 8. What about the brain? (slide 25)
![[cmpe421_lecture_01_ch_1_ab_p025.jpg]]

**Notes.** Brains are very good at rational decisions but not perfect. They are not modular like software, so they are hard to reverse-engineer. "Brains are to intelligence as wings are to flight": aeroplanes use the physics of flight without copying feathers, and AI uses the principles of intelligence without copying neurons. Two lessons from the brain: **memory** and **simulation** are key to decision making.

> [!info] Memory and simulation (beyond the slides)
> **Memory**: keep what happened before and learn from it. **Simulation**: imagine "what would happen if I did this?" before acting. A decision maker needs both: memory supplies the experience, simulation uses it to predict the results of possible actions. In this course memory returns as learning, and simulation returns as search and planning.

> [!question]- Check (slide 25): (a) The drawing shows two people inside a head: one looks into a crystal ball, the other reads among piles of books. Which of the slide's two lessons does each stand for? (b) A student says: "The brain is the only intelligent thing we know, so AI must copy neurons exactly." Answer with the slide's analogy.
> (a) The crystal ball stands for **simulation**: looking ahead at what could happen. The reader with the books stands for **memory**: stored knowledge and experience.
> (b) "Brains are to intelligence as wings are to flight." Birds showed that flight is possible, but aeroplanes do not flap feathered wings. Engineers used the principles behind flight. In the same way AI takes principles from the brain (memory, simulation) without copying its parts. The slide adds a practical reason: brains are not modular like software, so they are hard to reverse-engineer.

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

> [!question]- Check (slide 26): Name the field from the slide that each idea comes from: (a) a proof that no program can decide for every program whether it stops, (b) a cruise control that holds 90 km/h uphill and downhill, (c) two shops that each set a price while guessing the price of the other, (d) a tree diagram that splits a sentence into subject, verb and object, (e) the claim that the mind is a physical system, (f) a recording of which brain cells fire when a person sees a face, (g) a study of how reaction time falls with practice.
> (a) Mathematics: (un)decidability. (b) Control theory: a homeostatic system that keeps a stable state. (c) Economics: game theory. (d) Linguistics: grammar. (e) Philosophy. (f) Neuroscience: the physical substrate of mental activity. (g) Psychology: adaptation.

> [!question]- Check (slide 26, terms): Generalised chess is chess on a board of $n\times n$ squares. The problem: given a position, can White force a win? Find the mistake: "A board has only finitely many positions, so a program can go through all of them. So the problem is decidable, and so it is tractable."
> The first part is right. For every $n$ the number of positions is finite, so a program that examines all of them always stops with the correct answer. That is what decidable means.
> "So it is tractable" does not follow. Tractable means that the running time grows at most like a polynomial in the input size $n$. The number of positions grows exponentially with $n$, and for generalised chess it is proved that no algorithm runs in polynomial time (beyond the slides). The problem is decidable but intractable.
> Ordinary chess has one fixed $8\times8$ board, so there is no $n$ that grows, and "tractable" in this strict sense does not apply to it. For it we can only say: the best move is computable in finite time, but the game tree has roughly $10^{120}$ possible games (beyond the slides), far too many for any real computer.
> Mathematics gave AI both ideas: what can be computed at all, and what can be computed in reasonable time.

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

> [!info] Words on the history slide (beyond the slides)
> "Look, Ma, no hands!" is what a child shouts when riding a bicycle without holding on. The slide uses it for 1950-70: each early program was shown off as a surprising trick.
> An **expert system** is a program with hundreds of hand-written if-then rules taken from human experts, for example for medical diagnosis. Such systems were expensive to build and failed outside their narrow area. The promises of the boom were not kept and the money left: that is the "AI winter".
> "AI Spring"? on the slide is the hopeful name for the period after the winter.

> [!question]- Check (slide 31, order): Put these six in time order and give the slide's era for each: the Dartmouth meeting, the expert-systems boom, the McCulloch and Pitts model, Robinson's algorithm, the AI winter, Turing's paper. How many years lie between the first artificial neuron model and the name "Artificial Intelligence"?
> 1943 McCulloch and Pitts and 1950 Turing's paper: early days (1940-1950).
> 1956 Dartmouth meeting and 1965 Robinson's algorithm: excitement (1950-70).
> 1980-88 expert-systems boom and 1988-93 AI winter: knowledge-based approaches (1970-90).
> From the neuron model to the name: $1956-1943=13$ years. The idea of an artificial neuron is older than the name of the field.

> [!question]- Check (slide 31, eras): Find two mistakes: "The AI winter came because computers could not do logical reasoning. It ended when Robinson found his complete reasoning algorithm."
> Mistake 1: the winter (1988-93) was the bust of the expert-systems industry. Logical reasoning was not missing. Hand-written certain rules did not deliver what the boom of 1980-88 had promised.
> Mistake 2: Robinson's algorithm is from 1965, which is $1988-1965=23$ years before the winter began. What came after the winter was a different idea: statistical approaches, with probability, a focus on uncertainty, and agents and learning systems.

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

> [!info] The third story on slide 33 (beyond the slides)
> The third story is the fable of the fox and the crow gone wrong. In the real fable the dishonest fox praises the vain crow, the crow opens his beak to sing, the cheese falls, and the fox takes it. In the Tale-Spin version the crow "noticed that he was holding the piece of cheese", became hungry and swallowed it. His own goal was satisfied at once, so the fox arrives and has nothing to do.
> Two things are missing. A creature does not need to discover what is in its own mouth. And the program does not know what makes a story: a conflict between characters who want something from each other.

> [!question]- Check (slide 32): The quiz has two grocery lines (buy on the web, buy at a supermarket) and two dishwasher lines (any dishwasher in any home, a known dishwasher with a person). Use the pattern from the notes to explain why the lines of each pair differ in difficulty. Then predict: which is easier for AI, solving a Sudoku or folding the laundry from a basket?
> Groceries: a web shop is a digital world with fixed pages and buttons, so no body is needed. A supermarket needs a body that moves between people, finds items on changing shelves and handles soft or fragile things.
> Dishwasher: a known machine with a person helping is a narrow, prepared situation. "Any dishwasher in any home" means unknown kitchens, unknown machines and unknown dishes.
> Prediction: Sudoku is much easier. It has clear rules and a clear test of success. Folding laundry means handling soft objects of unknown shape in the physical world.

> [!question]- Check (slide 33): Each Tale-Spin story follows a valid plan. For each of the three stories, say which piece of everyday knowledge the program did not have.
> Story 1 (Joe Bear): honey is *inside* a beehive. You eat the honey, not the hive.
> Story 2 (Henry Squirrel): gravity is a force, not a creature. It cannot fall into a river or drown.
> Story 3 (fox and crow): a creature knows what it holds in its mouth, and a story needs a conflict. The crow eats the cheese at once, so nothing happens between him and the fox.
> In all three the reasoning is correct and the result is absurd. Correct inference without common-sense knowledge is not enough.

> [!question]- Check (slide 34): The photo shows the 1997 match in which Deep Blue beat the world chess champion. A newspaper writes: "Machines are now more intelligent than humans." Find the mistake. Would Deep Blue pass the Turing test?
> Deep Blue is narrow AI. It is better than any human at one task with clear rules and a clear score. It cannot grab a cup, drive, or tell a story, and it cannot move its skill to another task. "More intelligent" in general does not follow from one task.
> It would not pass the Turing test. It cannot hold a conversation at all. A machine can be far above humans in one task and fail the test of acting humanly.

## 12. Textbook, reading and related fields (slides 28, 35-37)
![[cmpe421_lecture_01_ch_1_ab_p028.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p035.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p036.jpg]]
![[cmpe421_lecture_01_ch_1_ab_p037.jpg]]

**Notes.** The textbook is Russell and Norvig, *Artificial Intelligence: A Modern Approach* (AIMA), recommended but not required. This lecture is AIMA Ch. 1. One course is a limited view of a vast area. The suggested reading is on embodied cognitive science: intelligence arises from a body interacting with its environment. AI continues in machine learning, automated reasoning, computational linguistics, computer vision and other fields.

> [!question]- Check (slide 35): The reading is about embodied cognitive science: intelligence arises from a body that interacts with its environment. Which of the four approaches of this lecture is furthest from this view? Which lines of the quiz on slide 32 support the embodied view?
> The laws-of-thought approach (thinking rationally) is furthest. It treats intelligence as correct reasoning on symbols and needs no body and no environment. (The Turing test also leaves the body out, because it uses text only. But it still judges behaviour in an interaction, so it is closer to the embodied view.)
> The quiz supports the embodied view with its hard lines: play tennis, grab a cup, unload any dishwasher, shop in a supermarket. These need a body in a changing world, and they are harder for AI than chess or Go.

> [!question]- Check (slides 36-37): Slide 36 lists courses where AI continues. Match each task to the closest course on the list: (a) read handwritten postal codes on envelopes, (b) ten warehouse robots divide the orders among themselves, (c) a grammar checker for Turkish, (d) interview doctors and write their know-how as rules for a program, (e) a program that proves a sorting routine correct.
> (a) Pattern Recognition (Image Processing and Computer Vision are close). (b) Distributed AI. (c) Computational Linguistics. (d) Knowledge Engineering. (e) Automated Reasoning.
> The point of slides 35 to 37: one course shows a limited part of a vast area, and each line on the list is a field of its own.

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
- Quiz: [[CMPE421 L01 - What is AI - quiz]]

Slides not embedded: 1, 2, 7 (titles), 21-24 (repeats of slide 20), 27 (laptop policy), 29 (section title), 30 (video placeholder).
