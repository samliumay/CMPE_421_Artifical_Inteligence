---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L01 - What is AI
type: quiz
questions: 24
---

# CMPE421 L01 - What is AI - quiz

Lesson note: [[CMPE421 L01 - What is AI]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Examples and agenda (slides 3-9)

### Q1 (slides 3-4)

A driverless car senses the road, other cars and people, and it steers, brakes and accelerates. What does "doing the job well" mean for this car, and must it drive like a human to be useful?

- A) It must drive like a skilled human driver, because a useful car must behave the way people expect a human to behave.
- B) It must arrive safely, legally and in good time. It need not drive like a human: we want the right action, not the human one.
- C) It must derive every action by correct logic from the traffic rules. A safe arrival is not enough if the reasoning is wrong.
- D) It must process road images the way a human brain does. Otherwise its decisions on the road cannot be trusted.

> [!success]- Answer
> **B.** The job is judged by its result: arrive safely, legally and in good time. A human driver gets tired and makes mistakes, so copying a human is not the goal. Doing the right thing is the idea of "acting rationally".
> - A: This is "acting humanly". The car is useful because it drives well, not because it drives like a person.
> - C: This is "thinking rationally". It judges the inner reasoning, but the car is useful because of what it does.
> - D: This is "thinking humanly". A car can drive well with methods that are nothing like a human brain.

### Q2 (slides 5-6)

The same camera drone follows a runner through a forest (slide 5) and films a wedding in an open space (slide 6). In which scene is it harder to keep the person in view and to fly safely? Is the drone narrow AI or general AI?

- A) The wedding, because there are many people to avoid. The drone is general AI, because it works in two different scenes.
- B) The forest, because the runner is fast and trees hide her and block the path. The drone is general AI, because it works in two scenes.
- C) The wedding, because there are many people to avoid. The drone is narrow AI, because it does one task.
- D) The forest, because the runner is fast and trees hide her and block the path. The drone is narrow AI, because it does one task.

> [!success]- Answer
> **D.** In the forest the runner moves fast, the trees hide her from time to time and the trees are obstacles. The drone must find her again and plan a safe path in real time. At the wedding people move slowly in an open space; it only has to keep a safe distance.
> The drone tracks and films, and nothing else. It cannot drive a car or answer a question, so it is narrow AI.
> - A: The wedding guests move slowly in open space. And two scenes of the same task (tracking and filming) do not make it general AI.
> - B: The scene is right, but working in two scenes of one task is still narrow AI. General AI would do any intellectual task.
> - C: Narrow AI is right, but the wedding is the easier scene: slow people, open space, no trees.

### Q3 (slides 8-9)

Four cases: (a) a hiring program trained on old decisions rejects women more often, (b) a chatbot wakes up one night and starts to hate its users, (c) a criminal copies a voice and phones the victim's family for money, (d) lorry drivers lose work to driverless lorries. Which case is science fiction, and what kind of real worry is case (a)?

- A) Science fiction: (b). Case (a) is a worry about bias and fairness.
- B) Science fiction: (c). Case (a) is a worry about bias and fairness.
- C) Science fiction: (b). Case (a) is a worry about misuse by people.
- D) Science fiction: (c). Case (a) is a worry about misuse by people.

> [!success]- Answer
> **A.** (b) needs a conscious machine with its own feelings: that is science fiction. (c) is real: voice cloning is already used for fraud. (d) is real: jobs.
> In (a) nobody misuses the program. It learned the unfair pattern from old decisions: that is bias.
> - B: Voice cloning sounds like science fiction, but it exists today. It is a real misuse worry.
> - C: (b) is right, but in (a) nobody uses the program for a bad aim. The harm comes from biased training data.
> - D: Both parts are wrong: (c) is real, and (a) is bias, not misuse.

## 2. Four ways to define AI (slide 10)

### Q4 (slide 10)

An autopilot lands a plane more smoothly than any human pilot. It uses calculations that no pilot does in his head. Under which of the four definitions of AI is the smooth landing alone enough to call it a success?

- A) Acting humanly, because it does the job of a human pilot, only better than a pilot does it.
- B) Thinking humanly, because it solves the same landing problem that a pilot's mind solves.
- C) Acting rationally, because it does the right thing by a fixed standard, whatever its inner steps are.
- D) Thinking rationally, because a smooth landing shows that its inner reasoning must be correct.

> [!success]- Answer
> **C.** Acting rationally judges only the result against a fixed standard: a safe, smooth landing. That is a success.
> - A: Acting humanly asks for human-like behaviour. The autopilot behaves better and differently, so it fails this standard.
> - B: Thinking humanly asks whether the inner steps are a human mind's steps. They are not: no pilot does these calculations.
> - D: Thinking rationally is about the thinking, not the result. We must look inside to judge it; the landing alone does not show it.

## 3. Thinking humanly: cognitive modelling (slides 11-12)

### Q5 (slide 11, validation)

A program solves arithmetic puzzles. It is always correct and ten times faster than people. Its author says: "So it is a good cognitive model." What is the mistake, and how is a cognitive model validated?

- A) It is not fast enough to be sure. A cognitive model is validated by its score on even harder puzzles.
- B) There is no mistake. A cognitive model is validated by checking that all of its answers are correct.
- C) It must behave like people, slowness and errors included. It is validated by predicting human behaviour, or by brain data.
- D) It must use the rules people learn at school. It is validated by checking that each of its steps is logically valid.

> [!success]- Answer
> **C.** A cognitive model is a theory of how *humans* think. It is good when it behaves like people: slow on hard puzzles, with their typical errors.
> The slide gives two ways to validate: predict human behaviour and test it (errors, reaction times), or identify the model from neurological data.
> - A: Speed and score measure performance. A model that beats people is less like people, not more.
> - B: Always correct is the mistake itself: people are not always correct, so the model does not match them.
> - D: Checking logical validity tests "thinking rationally", not whether the model thinks like a human.

### Q6 (slide 11, level)

Two teams model how people recognise a word. Team A writes rules about letters, word lists and meanings. Team B simulates 10 000 neurons and their connections. At which level of abstraction does Team B work, and in which direction does Team A work?

- A) Team B: the circuitry level. Team A: top-down, like cognitive science.
- B) Team B: the knowledge level. Team A: top-down, like cognitive science.
- C) Team B: the circuitry level. Team A: bottom-up, like cognitive neuroscience.
- D) Team B: the knowledge level. Team A: bottom-up, like cognitive neuroscience.

> [!success]- Answer
> **A.** Team B models neurons and their connections: the circuitry level. That is the bottom-up way of cognitive neuroscience, which starts from brain data.
> Team A writes rules about letters and meanings: the knowledge level. That is the top-down way of cognitive science, which starts from behaviour.
> - B: Neurons and connections are circuitry, not knowledge. Rules about words would be the knowledge level.
> - C: Team B is right, but the directions are swapped. Starting from rules and behaviour is top-down.
> - D: Both parts are swapped: Team B is circuitry, and Team A works top-down.

## 4. Acting humanly: the Turing test (slides 13-14)

### Q7 (slide 13)

In a Turing test the interrogator types "What is 48 271 times 9 316?". A machine answers correctly in 0.1 seconds. Does this help the machine to pass, and what does the Turing test measure?

- A) It helps, because it shows strong reasoning. The test measures how capable the machine is.
- B) It helps, because the answer is correct. The test measures how human the behaviour looks.
- C) It does not help, because it gives the machine away. The test measures how capable the machine is.
- D) It does not help, because it gives the machine away. The test measures how human the behaviour looks.

> [!success]- Answer
> **D.** A person needs minutes and often makes a mistake. An instant correct answer shows which player is the machine. To pass, the machine must wait, or even answer wrongly.
> So the test measures how human the behaviour looks, not how capable the machine is.
> - A: Strong reasoning does not help here; it makes the machine look less human. And the test does not measure capability.
> - B: The second part is right, but a correct answer in 0.1 s does not look human. It gives the machine away.
> - C: The first part is right, but then the test cannot measure capability: the machine must hide an ability to pass.

### Q8 (slide 14)

A speech recogniser gets 12 words wrong on a fixed test recording of 400 words. (a) What is its word error rate? (b) The list of the 12 wrong words shows where the system fails. Which of the Turing test's problems does this kind of score avoid, thanks to that list?

- A) (a) 97%. (b) The test is not constructive.
- B) (a) 3%. (b) The test is not constructive.
- C) (a) 3%. (b) The test is not reproducible.
- D) (a) 97%. (b) The test is not reproducible.

> [!success]- Answer
> **B.** (a) Rate $=12/400=0.03=3\%$.
> (b) The wrong words point to what to improve. A failed Turing test does not say what to change: it is not constructive.
> - A: 97% is the share of correct words, $388/400$. The error rate counts the wrong ones.
> - C: The rate is right. The score is also reproducible, but that comes from the fixed recording, not from the list of wrong words.
> - D: Both slips: 97% is the accuracy, and the list of errors answers "not constructive".

## 5. Thinking rationally: the laws of thought (slides 15-16)

### Q9 (slide 15)

(a) "All birds can fly. A penguin is a bird. So a penguin can fly." Is the argument valid? (b) A goalkeeper dives to the correct corner 0.3 seconds after the shot. Which problem of the laws-of-thought approach does this show?

- A) (a) Valid, but the conclusion is false. (b) Not all intelligent behaviour is mediated by logic.
- B) (a) Not valid, because the conclusion is false. (b) Not all intelligent behaviour is mediated by logic.
- C) (a) Valid, but the conclusion is false. (b) Logic does not say which thoughts one should have.
- D) (a) Not valid, because the conclusion is false. (b) Logic does not say which thoughts one should have.

> [!success]- Answer
> **A.** (a) The argument has the same form as "Socrates is a man, all men are mortal, so Socrates is mortal". If both premises were true, the conclusion could not be false. So it is valid. The conclusion is false because the first premise is false.
> (b) The goalkeeper acts well with no time to derive the dive from premises: not all intelligence is mediated by logic.
> - B: Validity is about the form, not about the truth of the conclusion. A valid form with a false premise can give a false conclusion.
> - C: (a) is right, but the goalkeeper knows what to do. The point is that he acts well without logical inference.
> - D: Both parts are wrong, see B and C.

### Q10 (slide 16)

A Prolog program holds the facts `parent(ali, berk).` and `parent(berk, can).` and the rule `grandparent(X, Z) :- parent(X, Y), parent(Y, Z).` The programmer wrote only knowledge, and the program derives new facts: the logicist tradition. What does the query `grandparent(ali, W).` return, and which problem of the laws-of-thought approach remains?

- A) `W = berk`. Logic does not say which facts are worth deriving; someone must ask the query.
- B) `W = berk`. The program cannot derive any fact that the programmer did not write.
- C) `W = can`. Logic does not say which facts are worth deriving; someone must ask the query.
- D) `W = can`. The program cannot derive any fact that the programmer did not write.

> [!success]- Answer
> **C.** With X = ali the first fact gives Y = berk, and the second fact gives Z = can. So `W = can`.
> The program can derive many true facts, but logic does not tell it which ones are worth deriving. "What thought should one have?" stays open.
> - A: berk is the value of Y, ali's child, not the grandchild.
> - B: Both parts are wrong: berk is the child, and the program does derive a new fact.
> - D: The answer `W = can` is itself a fact nobody wrote. Deriving new facts is exactly what the logicist tradition does.

## 6. Acting rationally: rational agents (slides 17-18)

### Q11 (slide 17)

Deniz looks left and right, sees an empty street and crosses. A tile falls from a roof and hits her. Emre crosses a busy street without looking and arrives safely. Who acted rationally?

- A) Emre, because he reached his goal safely, and rationality is judged by the outcome.
- B) Deniz, because with the available information crossing was expected to reach her goal.
- C) Both of them, because each chose to cross and each wanted to reach the other side.
- D) Neither of them, because a rational agent must foresee every danger, the tile included.

> [!success]- Answer
> **B.** The slide: the right thing is what is *expected* to maximise goal achievement *given the available information*. Deniz looked; with an empty street, crossing was the right choice. The tile was bad luck.
> - A: Emre's good result was luck. He could look cheaply and did not, so his choice was not the expected best one.
> - C: Wanting the goal is not enough. Emre did not use the information he could easily get.
> - D: That asks for an omniscient agent. Rationality is judged only on what the agent could know.

### Q12 (slide 17, limits)

A chess program has one second per move. In one second it cannot search all continuations, so it cannot find the provably best move. Which sentence of slide 17 describes this situation, and does the program stop being useful?

- A) "Action without thinking: e.g. reflexes." It stops being useful, because it plays without real search.
- B) "Rationality is only applicable in ideal environments." It stops being useful, because it can miss the best move.
- C) "Action without thinking: e.g. reflexes." It is still useful: it picks the best move it can find in time.
- D) "Rationality is only applicable in ideal environments." It is still useful: it picks the best move it can find in time.

> [!success]- Answer
> **D.** Perfect rationality needs enough time and computing power to find the best action. A real environment does not give them: rationality is only applicable in ideal environments.
> The program still picks the best move it can find in one second. That is acting as well as possible with limited resources.
> - A: The program does search; it only cannot finish. A reflex has no search at all. And it is still useful.
> - B: The sentence is right, but missing the provably best move does not make the program useless.
> - C: The program still thinks (it searches). A reflex is action with no thinking, so this sentence does not fit.

## 7. Rational decisions: maximise expected utility (slides 19-20)

### Q13 (slide 19)

Program A stores a huge table "situation → action". Program B reasons step by step with logic. In every situation both choose the same action. Which is more rational in the slide's technical sense, and which definitions of AI see a difference between them?

- A) Program B is more rational. Only the two "acting" definitions see a difference.
- B) They are equally rational. Only the two "acting" definitions see a difference.
- C) Program B is more rational. Only the two "thinking" definitions see a difference.
- D) They are equally rational. Only the two "thinking" definitions see a difference.

> [!success]- Answer
> **D.** The slide: rationality concerns only what decisions are made, not the thought process. Same decisions, same rationality.
> The "thinking" definitions look inside: thinking rationally asks whether the reasoning is correct, thinking humanly whether it is like a human's. B has reasoning to inspect, A has none.
> - A: Step-by-step logic does not make B more rational. And the "acting" definitions see only the actions, which are the same.
> - B: Equal rationality is right, but the "acting" definitions cannot tell two programs with the same actions apart.
> - C: The "thinking" part is right, but rationality in this sense ignores the thought process.

### Q14 (slide 20)

A hospital robot can take the lift or the ramp. Lift: it arrives fast with probability 0.8 (utility 50) and gets stuck with probability 0.2 (utility $-100$). Ramp: it arrives slowly for sure (utility 30). Which action maximises expected utility, and at which success probability $p$ of the lift are the two equal?

- A) The lift, with $\mathrm{EU}=60>30$. Equal at $p=0.6$.
- B) The ramp, with $30>\mathrm{EU}(\text{lift})=20$. Equal at $p=13/15\approx0.867$.
- C) The lift, with $\mathrm{EU}=60>30$. Equal at $p=13/15\approx0.867$.
- D) The ramp, with $30>\mathrm{EU}(\text{lift})=20$. Equal at $p=0.6$.

> [!success]- Answer
> **B.** $\mathrm{EU}(\text{lift})=0.8\cdot50+0.2\cdot(-100)=40-20=20$. $\mathrm{EU}(\text{ramp})=1\cdot30=30$. So the ramp.
> Equal when $50p-100(1-p)=30$: $150p-100=30$, $150p=130$, $p=13/15\approx0.867$.
> - A: 60 comes from $0.8\cdot50+0.2\cdot100$: the minus sign of the stuck utility is lost. $p=0.6$ comes from $50p=30$: the stuck outcome is left out.
> - C: The break-even is right, but $\mathrm{EU}=60$ loses the minus sign of $-100$.
> - D: The choice is right, but $50p=30$ forgets the cost of getting stuck with probability $1-p$.

## 8. What about the brain? (slide 25)

### Q15 (slide 25)

![[cmpe421_lecture_01_ch_1_ab_p025.jpg]]

(a) In the drawing, one person looks into a crystal ball and the other reads among piles of books. Which of the slide's two lessons does each stand for? (b) A student says: "The brain is the only intelligent thing we know, so AI must copy neurons exactly." How does the slide's analogy answer this?

- A) Crystal ball: memory, books: simulation. The student is right: as flight needs wings, intelligence needs copied neurons.
- B) Crystal ball: simulation, books: memory. The student is right: as flight needs wings, intelligence needs copied neurons.
- C) Crystal ball: simulation, books: memory. The student is wrong: planes fly without feathered wings, so AI need not copy neurons.
- D) Crystal ball: memory, books: simulation. The student is wrong: planes fly without feathered wings, so AI need not copy neurons.

> [!success]- Answer
> **C.** (a) The crystal ball looks ahead at what could happen: simulation. The books are stored knowledge and experience: memory.
> (b) Birds showed that flight is possible, but aeroplanes do not flap feathered wings. Engineers used the principles of flight. AI likewise takes principles from the brain without copying its parts.
> - A: Both parts are wrong: the two pictures are swapped, and the analogy is read backwards.
> - B: (a) is right, but the analogy says the opposite: planes use the principles of flight, not copies of bird wings.
> - D: (b) is right, but a crystal ball shows the future, which is simulation. Books hold the past: memory.

## 9. Foundations of AI (slide 26)

### Q16 (slide 26)

From which of the slide's seven foundation fields does each idea come? (a) A proof that no program can decide, for every program, whether it stops. (b) Two shops that each set a price while guessing the price of the other.

- A) (a) Mathematics. (b) Economics.
- B) (a) Philosophy. (b) Economics.
- C) (a) Mathematics. (b) Psychology.
- D) (a) Philosophy. (b) Psychology.

> [!success]- Answer
> **A.** (a) is (un)decidability, which the slide lists under mathematics. (b) is game theory, which comes from economics.
> - B: Philosophy gave logic and reasoning, but the proof about what programs can decide is mathematics (computation, decidability).
> - C: (a) is right. Guessing the other side's price sounds like the study of the mind, but strategic choice between players is game theory: economics.
> - D: Both parts are wrong, see B and C.

### Q17 (slide 26, terms)

Generalised chess is chess on an $n\times n$ board. The problem: given a position, can White force a win? Find the mistake: "A board has only finitely many positions, so a program can go through all of them. So the problem is decidable, and so it is tractable."

- A) There is no mistake. Finitely many positions make it decidable, and every decidable problem is also tractable.
- B) Decidable is right, but tractable does not follow: the positions grow exponentially with $n$, and no polynomial algorithm exists.
- C) Both are wrong. A problem with so many positions can never be decided, so it cannot be tractable either.
- D) The order is reversed. Each position is checked fast, so it is tractable, but no program can decide the whole game.

> [!success]- Answer
> **B.** For every $n$ the positions are finite, so a program that examines all of them stops with the correct answer: decidable.
> Tractable means the running time grows at most like a polynomial in $n$. The positions grow exponentially, and for generalised chess no polynomial algorithm exists. Decidable but intractable.
> - A: Every tractable problem is decidable, not the other way round.
> - C: Decidable asks only for a finite time, however long. Many positions make it slow, not undecidable.
> - D: Tractable implies decidable, so "tractable but not decidable" is impossible. And checking one position fast does not make the whole search fast.

## 10. A short history of AI (slides 29-31)

### Q18 (slide 31, order)

Events: McCulloch and Pitts' neuron model (1943), Turing's paper (1950), the Dartmouth meeting that named the field (1956), Robinson's algorithm (1965), the expert-systems boom (1980-88), the AI winter (1988-93). How many years lie between the first artificial neuron model and the name "Artificial Intelligence", and under which era does the slide list the AI winter?

- A) 13 years. The winter is in the era of knowledge-based approaches (1970-90).
- B) 6 years. The winter is in the era of knowledge-based approaches (1970-90).
- C) 13 years. The winter is in the era of statistical approaches (1990-).
- D) 6 years. The winter is in the era of statistical approaches (1990-).

> [!success]- Answer
> **A.** The first artificial neuron model is McCulloch and Pitts (1943). The name comes from Dartmouth (1956): $1956-1943=13$ years.
> The winter is the bust of the expert-systems industry, which closes the era of knowledge-based approaches (1970-90).
> - B: $1956-1950=6$ starts from Turing's paper, not from the neuron model.
> - C: The years are right. The winter runs into 1993, but the slide lists it under the knowledge-based era; the statistical era is what came after it.
> - D: Both slips, see B and C.

### Q19 (slide 31, eras)

Find the two mistakes: "The AI winter came because computers could not do logical reasoning. It ended when Robinson found his complete reasoning algorithm."

- A) The winter came because computers were too weak for logical reasoning. Robinson's algorithm dates from 1965.
- B) The winter was the bust of the expert-systems industry. Robinson's algorithm came around 1993 and ended it.
- C) The winter came because computers were too weak for logical reasoning. Robinson's algorithm came around 1993 and ended it.
- D) The winter was the bust of the expert-systems industry. Robinson's algorithm dates from 1965.

> [!success]- Answer
> **D.** Mistake 1: the winter (1988-93) was the bust of the expert-systems industry. Hand-written certain rules did not deliver what the boom of 1980-88 promised.
> Mistake 2: Robinson's algorithm is from 1965, $1988-1965=23$ years before the winter began. What came after the winter was statistical approaches.
> - A: The date is right, but logical reasoning was not missing; the expert-systems business collapsed.
> - B: The cause is right, but Robinson's algorithm is from 1965, long before the winter.
> - C: Both parts repeat the two mistakes of the statement.

## 11. What can AI do? (slides 32-34)

### Q20 (slide 32)

The slide's quiz asks which tasks AI can do. Pattern from the notes: clear rules and a clear score make a task easy for AI; a body in an unpredictable physical world makes it hard. Which is easier for AI today, and why: solving a Sudoku or folding the laundry from a basket?

- A) Folding laundry, because children learn it early, while Sudoku needs logical thinking.
- B) Folding laundry, because it has no strict rules, so the program cannot break one.
- C) Sudoku, because it has clear rules and a clear test of success, and needs no body.
- D) Sudoku, because laundry would need a general AI that can also hold a conversation.

> [!success]- Answer
> **C.** Sudoku is a closed puzzle with fixed rules and a clear test. Folding laundry means handling soft objects of unknown shape in the physical world, which is hard for AI.
> - A: What is easy for children is often hard for AI (Moravec's paradox). Logical puzzles with clear rules are easy for AI.
> - B: Having no clear rules and no clear score makes a task harder for AI, not easier.
> - D: Laundry needs a body and a hand, not conversation. A narrow robot could do it if handling soft objects were solved.

### Q21 (slide 33)

![[cmpe421_lecture_01_ch_1_ab_p033.jpg]]

Each Tale-Spin story follows a valid plan. Which piece of everyday knowledge is missing in story 1 (Joe Bear), story 2 (Henry Squirrel) and story 3 (fox and crow)?

- A) 1: the honey is inside the hive. 2: gravity is a force, not a creature. 3: a creature knows what it holds in its mouth.
- B) 1: the honey is inside the hive. 2: a squirrel that falls into a river can drown. 3: a fox does not eat cheese.
- C) 1: a bird cannot know where honey is. 2: gravity is a force, not a creature. 3: a fox does not eat cheese.
- D) 1: a bird cannot know where honey is. 2: a squirrel that falls into a river can drown. 3: a creature knows what it holds in its mouth.

> [!success]- Answer
> **A.** Story 1: Joe eats the beehive; you eat the honey inside it. Story 2: "Gravity drowned"; gravity is a force and cannot drown. Story 3: the crow "noticed" the cheese in his own mouth and ate it, so nothing happens with the fox.
> In all three the reasoning is correct and the result is absurd: common-sense knowledge is missing.
> - B: The absurd line in story 2 is "Gravity drowned", not Henry's fall. In story 3 the fox wanting cheese is the normal fable.
> - C: Irving telling Joe where the beehive is causes no absurd line; eating the hive does. Story 3 is wrong as in B.
> - D: Story 3 is right, but stories 1 and 2 point at the wrong lines, see B and C.

### Q22 (slide 34)

In 1997 Deep Blue beat the world chess champion. A newspaper writes: "Machines are now more intelligent than humans." What is the mistake, and would Deep Blue pass the Turing test?

- A) There is no mistake: beating the best human at chess shows general intelligence. It would pass the test.
- B) Deep Blue is narrow AI: it is better at one task only. It would pass the test, because its moves look human.
- C) There is no mistake: beating the best human at chess shows general intelligence. It would not pass the test.
- D) Deep Blue is narrow AI: it is better at one task only. It would not pass, because it cannot hold a conversation.

> [!success]- Answer
> **D.** Deep Blue beats humans at one task with clear rules and a clear score. It cannot grab a cup, drive or tell a story. "More intelligent" in general does not follow.
> The Turing test is a text conversation. Deep Blue cannot converse at all, so it fails.
> - A: Being best at one task is narrow AI. And Deep Blue cannot take part in a text conversation.
> - B: The first part is right, but the test is a conversation, not a chess game.
> - C: The second part is right, but one task does not show general intelligence.

## 12. Textbook, reading and related fields (slides 28, 35-37)

### Q23 (slide 35)

The suggested reading is about embodied cognitive science: intelligence arises from a body that interacts with its environment. Which of the four approaches to AI is furthest from this view?

- A) Acting humanly (the Turing test), because it judges only the correctness of the answers, not the behaviour.
- B) Acting rationally, because a rational agent only computes expected utilities and has no environment.
- C) Thinking rationally (laws of thought), because it treats intelligence as reasoning on symbols, with no body.
- D) Thinking humanly, because a cognitive model ignores the brain and the body and is only a program.

> [!success]- Answer
> **C.** The laws-of-thought approach treats intelligence as correct reasoning on symbols. It needs no body and no environment.
> - A: The Turing test judges how human the behaviour looks, not whether the answers are correct. It judges behaviour in an interaction, and the total Turing test even adds perception and object manipulation, so it is closer to the embodied view.
> - B: A rational agent acts in an environment and is judged by what it does there.
> - D: Cognitive modelling uses brain data too (cognitive neuroscience), so it does not ignore the brain.

### Q24 (slides 36-37)

![[cmpe421_lecture_01_ch_1_ab_p036.jpg]]

Match each task to the closest course on the list: (a) ten warehouse robots divide the orders among themselves, (b) interview doctors and write their know-how as rules for a program, (c) a program that proves a sorting routine correct.

- A) (a) Distributed AI. (b) Automated Reasoning. (c) Knowledge Engineering.
- B) (a) Distributed AI. (b) Knowledge Engineering. (c) Automated Reasoning.
- C) (a) Machine Learning. (b) Knowledge Engineering. (c) Knowledge Engineering.
- D) (a) Machine Learning. (b) Automated Reasoning. (c) Automated Reasoning.

> [!success]- Answer
> **B.** (a) Several agents that share work: Distributed AI. (b) Collecting experts' know-how as rules: Knowledge Engineering. (c) Proving a program correct by logic: Automated Reasoning.
> - A: (b) and (c) are swapped. Knowledge Engineering builds the rule base; Automated Reasoning does the proving.
> - C: The robots do not learn from data here; they divide work among many agents. And proving a routine correct is reasoning, not writing expert rules.
> - D: (c) is right, but (a) is Distributed AI, and (b) collects rules from people, which is Knowledge Engineering.
