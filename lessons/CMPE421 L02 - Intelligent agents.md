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

> [!question]- Check (Ch.2 deck, slide 2): A room thermostat measures the temperature and switches a heater on or off. Name its sensor, one percept, its actuator, one action, and say what the "?" box is.
> Sensor: the thermometer. Percept: one reading, for example "19 °C". Actuator: the heater switch. Action: "heater on".
> The "?" box is the agent program: the rule that turns readings into switch commands. It is the part the designer writes.

> [!question]- Check (Ch.2 deck, slide 3): The ghosts in Pac-Man move partly at random. Which word in the definition of a rational agent on slide 2 becomes necessary because of this, and why?
> The word "expected". Pac-Man cannot know where a ghost will go, so he cannot know the real result of a move.
> He can only choose the move that is best on average over the possible ghost moves. That is maximising expected utility.

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

> [!info] The picture-only slides (beyond the slides)
> Several slides of this part have no text. What they show:
> - **Slide 7**: a Go player thinking at a Go board. The slide has no caption. Our reading of the picture: it looks like a press photo of the 2016 match between the Go program AlphaGo and the professional player Lee Sedol. Go has far more positions than chess, so fast search alone, as in Deep Blue, is not enough. AlphaGo also learned from games how good a position is.
> - **Slide 8**: a simulated human figure that learns to walk. "Iteration 0" is the start of learning, before any practice. One iteration is one round of trying and then improving the program.
> - **Slide 10**: robot arms that handle everyday objects (a food box, a drill).
> - **Slide 12**: robots that work next to a person. The person is part of the robot's environment.
> - **Slide 13**: programs that predict or decide for us: a route planner, a weather forecast, film recommendations, stock prices, a thermostat.
> - **Slide 17**: the course problems as pictures (routes, chess, walking, driving, spam, digits). The footnotes say that walking and driving need continuous-time versions of the methods, and that spam and digit recognition come only briefly in the machine learning topic.

> [!question]- Check (Ch.2 deck, slide 5): Deep Blue searched 200 million positions per second. How many positions is that in a 3-minute think? Does the slide claim that this makes Deep Blue think like a human?
> $3\ \text{min}=180\ \text{s}$. Positions $=200\,000\,000\times180=36\,000\,000\,000=3.6\times10^{10}$.
> No. The slide puts "intelligent creative" in quotation marks, and Kasparov said in 1997 that Deep Blue "hasn't proven anything". Deep Blue is an agent because it chooses good actions (moves). Its method, very fast search, is not the human method.

> [!question]- Check (Ch.2 deck, slides 6-7): Deep Blue and the Atari agents of slide 6 both play games. Where does the playing knowledge come from in each case? What are the percepts of an Atari agent?
> Deep Blue: the programmers wrote the search and the evaluation of positions. The Atari agents use reinforcement learning: they start without knowing the game and improve from the score they get.
> Atari percepts: the screen pixels and the score. Actions: the joystick moves.

> [!question]- Check (Ch.2 deck, slide 8): The figure is labelled "Iteration 0" and the body is bent over. What do you expect to see at iteration 0 and at a late iteration, and why?
> At iteration 0 the agent has learned nothing, so its movements are almost random and it falls.
> After many iterations it walks better, because after each round of trials the program is changed toward the actions that gave a higher score.

> [!question]- Check (Ch.2 deck, slides 9-10): The slide says "Reality much harder than simulations". Give three reasons in terms of sensors, actuators and time.
> Sensors: real cameras and sonars are noisy and see only part of the scene. A simulator can give the exact state.
> Actuators: a real motor does not do exactly what was commanded (slipping wheels, a soft towel). In a simulator the action has the programmed effect.
> Time: the real world keeps changing while the robot computes. A simulator can wait.

> [!question]- Check (Ch.2 deck, slide 12): A robot arm hands objects to a person. Why is this harder for the robot than working alone at the same table?
> The person is another agent. The robot does not control the person's movements and cannot predict them exactly.
> It must perceive what the person is doing and wants, and choose actions that are safe and helpful for both. The two work toward a shared result, so the setting is cooperative.

> [!question]- Check (Ch.2 deck, slide 13): Take the weather forecast and the thermostat on this slide. Which one only predicts, and which one decides and acts? Where does the route planner fit?
> The weather forecast predicts: it outputs an estimate and the human decides what to do with it.
> The thermostat decides and acts: it reads the temperature and switches the heating by itself.
> The route planner does both: it predicts the travel time of each route (the slide shows several routes with their minutes) and then recommends one.

> [!question]- Check (Ch.2 deck, slide 14): What is the difference between ASR and TTS? In a voice assistant, in which order do ASR, the dialog system and TTS work, and which of them sits on the sensor side of the agent?
> ASR (automatic speech recognition) turns sound into text. TTS (text-to-speech) turns text into sound.
> Order: ASR, then the dialog system (it decides what to answer), then TTS.
> ASR is on the sensor side (it interprets the percept). TTS is on the actuator side (it produces the action).

> [!question]- Check (Ch.2 deck, slide 15): Read the caption of the third picture in the top row and compare it with the picture. What does this tell you about such a system?
> The caption says "two young girls are playing with lego toy". Our reading of the picture: one small child sits in front, and the second person behind looks like an adult. The toy part fits. "Two young girls" does not seem to fit.
> The caption is a fluent sentence, but its details can be wrong. A vision system gives a likely description of the percept, not a guaranteed one. An agent that uses it must expect errors in what it believes about the world.

> [!question]- Check (Ch.2 deck, slides 16-17): Match each problem to a course topic of slide 16: (a) an exam timetable for 40 courses with no clashes, (b) the fastest road route between two cities, (c) a robot with noisy sensors that must find out where it is in a building, (d) deciding whether an e-mail is spam.
> (a) Constraint satisfaction (scheduling).
> (b) Search and planning (routing).
> (c) Probabilistic inference (robot localisation).
> (d) Supervised machine learning (spam detection). Slide 17 marks this one "briefly in our ML topic".

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

> [!info] The two quotations on slide 18 (beyond the slides)
> The first quotation (Martha Pollack) gives the reason for the agent view: we can only judge intelligence from what a system does, so AI should build systems that act, not systems that only think. The second explains the word: in daily life an agent is someone who acts for you (a film star's agent). A software agent acts for its user in the same sense.

> [!question]- Check (Ch.2 deck, slide 18): A program finds the best chess move but never outputs it. Following the first quotation, why can we not call it intelligent? What is missing in the agent picture?
> We can only assess intelligence through behaviour. A system that never acts shows nothing that we could measure.
> In the agent picture the actuators and the action arrow are missing. Without actions the environment never changes, so there is no performance to score.

> [!question]- Check (Ch.2 deck, slide 19): What does the star in $\mathcal P^{*}$ mean? For $\mathcal P=\{x,y\}$, list all percept sequences of length 2. Is $f$ defined only on single percepts?
> The star means "all finite sequences of elements of $\mathcal P$".
> Length 2: $xx$, $xy$, $yx$, $yy$. That is $2^{2}=4$ sequences.
> No. $f$ gives an action for every sequence of every length: 2 sequences of length 1, 4 of length 2, 8 of length 3, and so on.

> [!question]- Check (Ch.2 deck, slide 20): One vacuum agent uses a lookup table. Another uses three if-rules. For every percept sequence they choose the same action. Do they have the same agent function? The same agent program?
> The same agent function: the function is only the map from percept sequences to actions, and the two maps are equal.
> Different agent programs: the program is the concrete code that produces the map. Many programs can implement one function.

> [!question]- Check (Ch.2 deck, slide 21): Find what is missing in this statement: "A rational agent selects the action that maximises its performance measure."
> Two things are missing.
> 1. "Expected": the agent maximises the expected value of the performance measure, because it cannot know the real outcome.
> 2. The information it may use: the evidence of the percept sequence so far and its built-in knowledge. An action is judged against what the agent could know, not against the full truth.

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

> [!question]- Check (Ch.2 deck, slide 22): In the picture the agent is in A and both squares are dirty. Which percept does the agent receive? How many world states fit this percept?
> The percept is $[A,\mathit{Dirty}]$: the agent's location and the status of that square only.
> The percept says nothing about B. Two world states fit it: (agent in A, A dirty, B dirty) and (agent in A, A dirty, B clean).

> [!question]- Check (Ch.2 deck, slide 23): The table has the row $[A,\mathit{Clean}],[A,\mathit{Clean}]\to$ Right. Can this percept sequence happen if the agent follows the table and Right always moves it to B? Why is the row in the table anyway? How many rows of length 2 does the full table have?
> It cannot happen. After the first percept $[A,\mathit{Clean}]$ the table says Right, so the agent is in B and the second percept starts with B.
> The row is there because the agent function must give an action for every percept sequence, also for the ones that only occur when something goes wrong (for example the move fails).
> Rows of length 2: $4\times4=16$. The slide shows 2 of them.

> [!question]- Check (Ch.2 deck, slide 24): The agent starts in B. A is dirty, B is clean. Run REFLEX-VACUUM-AGENT for 5 steps. Give the percept and the action of each step. Then compute the score for "one point per clean square per step", counted after each action.
> | Step | Percept | Action | Clean squares after the action |
> |---|---|---|---|
> | 1 | $[B,\mathit{Clean}]$ | Left | 1 |
> | 2 | $[A,\mathit{Dirty}]$ | Suck | 2 |
> | 3 | $[A,\mathit{Clean}]$ | Right | 2 |
> | 4 | $[B,\mathit{Clean}]$ | Left | 2 |
> | 5 | $[A,\mathit{Clean}]$ | Right | 2 |
>
> Score $=1+2+2+2+2=9$.
> No agent can do better here: the first step must be the move to A, so A cannot be clean before step 2.

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

> [!question]- Check (Ch.2 deck, slide 25): A designer scores a taxi agent by "number of smooth brake applications". Which rule on the slide does this break? Give a better measure.
> It breaks the last line: measure what is wanted in the environment, not how the agent should behave.
> "Smooth braking" is a behaviour. The agent can collect points by braking smoothly again and again without taking anybody anywhere.
> Better: score the results, for example passengers delivered, no collisions, travel time, comfort as reported by the passenger.

> [!question]- Check (Ch.2 deck, slide 26): A vacuum agent stops for ever (NoOp) when it knows both squares are clean. With "one point per clean square per step, minus one per move" and squares that stay clean, this is rational. Now change only the prior knowledge: a clean square can become dirty again at any time. Is the same agent still rational?
> No. If dirt can come back, an agent that never looks again loses a point in every step in which a square is dirty.
> A rational agent now checks the squares from time to time, when the expected gain from finding new dirt is larger than the cost of the move.
> The performance measure, the actions and the percepts did not change. Only the prior knowledge changed, and it changed what is rational.

> [!question]- Check (Ch.2 deck, slide 27): Carrying an umbrella costs 1. Getting wet costs 5. The forecast says rain with probability 0.1. Which choice is rational? It then rains. Was the agent rational? Perfect? What would an omniscient agent have done?
> Expected cost without umbrella: $0.1\times5+0.9\times0=0.5$. Expected cost with umbrella: $1$.
> $0.5<1$, so going without the umbrella is rational.
> When it rains the real cost is 5. The agent was still rational, because the choice was the best one for what it knew. It was not perfect, because the actual result was not the best possible.
> An omniscient agent knows that it will rain and takes the umbrella (cost 1 instead of 5).

> [!question]- Check (Ch.2 deck, slide 28): An agent crosses a road without looking. Its percept sequence contains no car, so it says "crossing was rational given my percepts". What is wrong with this argument?
> The slide says rationality requires information gathering. Looking left and right is an action. It costs almost nothing and it changes the future percepts, so it raises the expected performance.
> A rational agent first chooses the looking action. The argument only works if looking was impossible.

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

> [!question]- Check (Ch.2 deck, slide 29): A robot vacuum cleans a flat. Put each item under P, E, A or S: bumper switch, wheels, furniture, fraction of the floor that is clean after one hour, battery gauge, suction motor, the cat.
> P: fraction of the floor that is clean after one hour.
> E: furniture, the cat.
> A: wheels, suction motor.
> S: bumper switch, battery gauge.

> [!question]- Check (Ch.2 deck, slide 30): Write a PEAS description for a parcel delivery drone. Use the taxi slide as a pattern.
> P: parcels delivered on time and undamaged, safety of people, energy used, legality (no-fly zones).
> E: airspace, buildings, trees, weather, birds, other drones, the customers.
> A: rotors, parcel release, lights or speaker.
> S: camera, GPS, altimeter, accelerometer and gyroscope, battery gauge.
> "Delivered on time" is in P, not in S: the designer computes it from outside. The drone only senses its position and the clock.

## 7. Fully or partially observable (slides 31-34)
![[cmpe421_lecture_02_ch_2_p031.jpg]]
![[cmpe421_lecture_02_ch_2_p034.jpg]]

**Notes.** **Fully observable**: the sensors give the complete state at each time, or at least everything relevant to the choice of action. Then the agent needs no internal state. **Partially observable**: sensors are noisy, or parts of the state are missing. The vacuum agent with a local dirt sensor cannot see the other square, so its world is partially observable.

> [!question]- Check (Ch.2 deck, slide 31): Give the vacuum agent a second sensor that reports the status of the other square. How many percepts are possible now? Is the world now fully observable?
> A percept now holds the location (2 values), the status here (2 values) and the status of the other square (2 values): $2\times2\times2=8$ percepts.
> The world has $8$ states (see section 4). Each percept identifies exactly one state, so the environment is fully observable. The agent no longer needs an internal state.

> [!question]- Check (Ch.2 deck, slide 34): The table says Backgammon is FULL, although nobody knows the next dice roll. It says Internet shopping is PARTIAL. Explain both entries.
> Observability is about the current state. In Backgammon the whole board and the dice that were just rolled are visible. The unknown future roll makes the game stochastic. It does not make it partially observable.
> In Internet shopping the agent sees only the pages it has loaded. Offers on other sites and the stock of the vendors are part of the state and are relevant to the choice, but they are not in the percept.

## 8. Deterministic, stochastic, strategic (slides 35-37)
![[cmpe421_lecture_02_ch_2_p035.jpg]]
![[cmpe421_lecture_02_ch_2_p037.jpg]]

**Notes.** **Deterministic**: the next state is completely determined by the current state and the agent's action. Otherwise **stochastic**. **Strategic**: deterministic except for the actions of other agents (chess). Taxi driving is stochastic.

> [!warning] Watch out
> A partially observable environment can *appear* stochastic. Unpredictable results do not prove the world is stochastic. It may be deterministic with hidden parts.

> [!question]- Check (Ch.2 deck, slide 35): Classify as deterministic, stochastic or strategic: (a) Sudoku, (b) chess, (c) a vacuum world in which Suck cleans the square only with probability 0.75. For (c), what is the probability that the square is still dirty after two Suck actions?
> (a) Deterministic: writing a digit has exactly one result.
> (b) Strategic: the rules are deterministic, the only uncertainty is the move of the other agent.
> (c) Stochastic: the same state and the same action can give two different next states.
> One Suck fails with probability $1-0.75=0.25$. Two independent failures: $0.25\times0.25=0.0625$.

> [!question]- Check (Ch.2 deck, slide 37): Backgammon and Taxi both have NO in the Deterministic row. Name the source of randomness in each. What would Backgammon be without dice?
> Backgammon: the dice. The same position and the same plan give different results for different rolls.
> Taxi: other drivers and pedestrians, the weather, a tyre that bursts, actuators that do not act exactly as commanded.
> Without dice only the opponent's moves are uncertain. With the vocabulary of slide 35 the game is then strategic, like chess.

## 9. Episodic or sequential (slides 38-41)
![[cmpe421_lecture_02_ch_2_p038.jpg]]
![[cmpe421_lecture_02_ch_2_p040.jpg]]
![[cmpe421_lecture_02_ch_2_p041.jpg]]

**Notes.** **Episodic**: experience splits into atomic episodes. In each one the agent perceives and performs a single action, and the next episode does not depend on earlier actions. Example: spotting defective parts on an assembly line. **Sequential**: the current decision can affect all future decisions (taxi, chess). Episodic environments are simpler because the agent does not need to think ahead.

> [!question]- Check (Ch.2 deck, slide 38): Find the mistake: "The assembly-line inspector makes thousands of decisions one after the other, so its environment is sequential."
> The number of decisions does not matter. What matters is whether a decision changes the later ones.
> Accepting or rejecting one part does not change the next part or the best decision for it. Each part is its own episode, so the environment is episodic.

> [!question]- Check (Ch.2 deck, slide 40): All four environments have NO in the Episodic row. Give one concrete reason for Solitaire and one for Internet shopping.
> Solitaire: moving a card changes which cards are free, so it changes which moves are possible later.
> Internet shopping: the link the agent follows decides which pages and offers it sees next, and money spent on one item is not available for the next.

> [!question]- Check (Ch.2 deck, slide 41): Why does an agent in an episodic environment not need to think ahead? Suppose every rejected part makes the machine recalibrate, and this changes how many later parts are defective. Is the inspection task still episodic?
> In an episodic environment the current action has no effect on later episodes. The best action now is simply the best action for this episode.
> With the recalibration the current decision changes the future parts. Short-term actions now have long-term consequences, so the task is sequential and the agent should think ahead.

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

> [!question]- Check (Ch.2 deck, slide 42): Classify as static, semi-dynamic or dynamic: (a) a crossword with no time limit, (b) the same crossword in a contest where every minute costs one point, (c) taxi driving. What happens in (c) to an agent that thinks for a long time?
> (a) Static: nothing changes while the agent thinks.
> (b) Semi-dynamic: the grid does not change, but the score falls with time.
> (c) Dynamic: the traffic moves while the agent thinks.
> In a dynamic environment thinking takes time in which the world goes on. Not deciding has the same effect as deciding to do nothing.

> [!question]- Check (Ch.2 deck, slide 44): Backgammon has YES in the Static row, although the opponent moves pieces and dice are rolled. Why is it not dynamic?
> Static or dynamic asks one thing: can the environment change while the agent is deliberating?
> While I think about my move, the board and the dice stay as they are. The opponent moves only in the opponent's turn. Those changes make the game multi-agent and stochastic. They do not make it dynamic.

> [!question]- Check (Ch.2 deck, slide 45): Apply the slide to the two-square vacuum world: are state, time, percepts and actions discrete? Give the counts. What becomes continuous if the agent can stand at any position $x\in[0,1]$ between the squares?
> State: discrete, $2\times2\times2=8$ states. Time: discrete, one action per step. Percepts: discrete, $2\times2=4$. Actions: discrete, 4 (Left, Right, Suck, NoOp).
> With a position $x\in[0,1]$ the state is continuous, because there are infinitely many positions. If the agent senses $x$, the percepts are continuous too.

> [!question]- Check (Ch.2 deck, slide 47): Backgammon uses dice and still has YES in the Discrete row. Why? How many different rolls of two dice are there if the order of the dice does not matter?
> Discrete is about how many distinct states, percepts and actions exist. It is not about chance. The dice change the Deterministic row.
> Rolls: 6 doubles plus $\dfrac{6\times5}{2}=15$ mixed pairs, $6+15=21$ different rolls (36 if the order counts). The number of board positions is finite too.

> [!question]- Check (Ch.2 deck, slide 48): Two vacuum robots clean the same flat. Case 1: each is scored on the total cleanliness of the flat. Case 2: each is scored on the amount of dirt it collects itself. Single-agent or multi-agent? Cooperative or competitive?
> Both cases are multi-agent: the score of each robot depends on what the other one does.
> Case 1 is cooperative: the two scores are the same number, so helping the other robot helps me.
> Case 2 is competitive: dirt collected by the other robot is dirt I cannot collect.

> [!question]- Check (Ch.2 deck, slide 50): For each column of the finished table, count the rows with the "easy" answer (FULL, or YES). Count SEMI as not easy. Which environment is the easiest and which the hardest?
> Solitaire: FULL, YES, NO, YES, YES, YES gives 5 of 6.
> Backgammon: FULL, NO, NO, YES, YES, NO gives 3 of 6.
> Internet shopping: PARTIAL, YES, NO, SEMI, YES, NO gives 2 of 6.
> Taxi: PARTIAL, NO, NO, NO, NO, NO gives 0 of 6.
> Solitaire is the easiest (only "sequential" is hard). Taxi is the hardest on every row.

## 11. More environments (slides 51, 53)
![[cmpe421_lecture_02_ch_2_p051.jpg]]
![[cmpe421_lecture_02_ch_2_p053.jpg]]

**Notes.** Entries that are often answered wrongly:
- **Poker**: partially observable (hidden hands) and stochastic (the deal).
- **Medical diagnosis**: single-agent.
- **Image analysis**: episodic, and semi (the image does not change but taking longer lowers the score).
- **Part-picking robot**: episodic, each part is a fresh decision.

The simplest environment is fully observable, deterministic, episodic, static, discrete and single-agent. Most real ones are the opposite on every line.

> [!info] Known or unknown (beyond the slides)
> Russell and Norvig add a seventh question that the deck does not list. An environment is **known** if the agent (or its designer) knows the rules: what each action does, or with which probabilities. It is **unknown** if the agent must first learn how the world works.
> This is not the same as observable. Solitaire is known (you know the rules) but partially observable (some cards are face down). This is the book's view of Solitaire. The deck's table writes FULL for it, see the warning in section 17. A new video game can be fully observable (you see the whole screen) but unknown (you do not know what the buttons do).
> An unknown environment is the main reason for a learning agent (section 16).

> [!question]- Check (Ch.2 deck, slide 51, question a): Two rows of the table have the same entry in all six columns. Which ones? Which single column separates Poker from Backgammon?
> Medical diagnosis and Refinery controller: both are Partially, Stochastic, Sequential, Dynamic, Continuous, Single.
> Poker and Backgammon differ only in Observable: Poker is Partially (hidden hands), Backgammon is Fully (everything is on the board).

> [!question]- Check (Ch.2 deck, slide 51, question b): Classify a new environment: a robot that plays table tennis against a human. It watches the ball with one camera. Its designer has programmed in the rules of the game and the physics of the ball's flight. Give the reason for each column, and say whether it is known or unknown.
> - Partially observable: the camera is noisy and cannot see the spin of the ball or the plan of the opponent.
> - Stochastic: the same stroke does not always give the same flight (air, bounce, motor errors).
> - Sequential: where the robot stands after a stroke decides which balls it can reach next.
> - Dynamic: the ball flies while the robot computes.
> - Continuous: positions, speeds, time and motor commands are real numbers.
> - Multi-agent, competitive: a point for the human is a lost point for the robot.
> - Known (beyond the slides): the designer gave the agent the rules of the game and the physics. What the opponent will do is not known, but that is a matter of the other agent, not of the rules of the world.

> [!question]- Check (Ch.2 deck, slide 53): Use the table of slide 51. In how many of the six properties does "Crossword puzzle" differ from the simplest environment? And "Chess with a clock"?
> Simplest: fully observable, deterministic, episodic, static, discrete, single-agent.
> Crossword: Fully, Deterministic, Sequential, Static, Discrete, Single. It differs in 1 property (sequential).
> Chess with a clock: Fully, Strategic, Sequential, Semi, Discrete, Multi. It differs in 4 properties (strategic, sequential, semi, multi). Only "fully observable" and "discrete" stay simple.

## 12. Agent programs and the table-driven agent (slides 52-56)
![[cmpe421_lecture_02_ch_2_p052.jpg]]
![[cmpe421_lecture_02_ch_2_p054.jpg]]
![[cmpe421_lecture_02_ch_2_p055.jpg]]
![[cmpe421_lecture_02_ch_2_p056.jpg]]

**Notes.** All agent programs have the same skeleton: input = current percept, output = action. The **table-driven agent** stores the whole percept sequence and looks it up in a table. It can implement any agent function, but the table is too large. With $|\mathcal P|$ possible percepts and a lifetime of $T$ steps:

$$\text{rows}=\sum_{t=1}^{T}|\mathcal P|^{t}=\frac{|\mathcal P|\left(|\mathcal P|^{T}-1\right)}{|\mathcal P|-1}.$$

> [!example] Vacuum world, $\lvert\mathcal P\rvert=4$
> $T=3$: $4+16+64=84$ rows. $T=10$: $\dfrac{4\,(4^{10}-1)}{3}=\dfrac{4\cdot 1\,048\,575}{3}=1\,398\,100$ rows. The reflex agent needs 4 rules for any $T$.

Why it is doomed: no agent can store the table, the designer cannot write it, the agent could never learn all entries, and nobody knows how to fill it in.

> [!question]- Check (Ch.2 deck, slide 52): The reflex vacuum program of section 4 is copied to a robot that has wheels and a suction motor but no dirt sensor. Does the agent work? What does this show about "agent = architecture + program"?
> It does not work. The program expects a percept $[\mathit{location},\mathit{status}]$, and this architecture cannot deliver the status.
> The program and the architecture must fit: the architecture must supply the percepts the program reads and must be able to carry out the actions it returns. A good program alone is not an agent.

> [!question]- Check (Ch.2 deck, slide 54): The skeleton says "Input = current percepts". The agent function depends on the whole percept sequence. How can a program that receives one percept per call still implement such a function?
> The program keeps memory between the calls. It stores the earlier percepts, or a summary of them (an internal state), and combines the stored information with the new percept.
> This is the difference the slide points at: the function takes the whole history as its argument, the program gets one percept and must remember the rest itself.

> [!question]- Check (Ch.2 deck, slide 55): What does `percepts` contain after the third call of TABLE-DRIVEN-AGENT, and what is the key of the lookup? How many table rows are needed for $\lvert\mathcal P\rvert=5$ and $T=4$? How many more for $T=5$?
> After three calls `percepts` holds three percepts in order of arrival. The key of the lookup is this whole sequence, not the last percept.
> $T=4$: $5+25+125+625=780$ rows. Check with the formula: $\dfrac{5\,(5^{4}-1)}{4}=\dfrac{5\times624}{4}=780$.
> $T=5$ adds $5^{5}=3125$ rows: $780+3125=3905$. One more step makes the table about five times larger.

> [!question]- Check (Ch.2 deck, slide 56): Put the four kinds in order and say what each adds to the one before. Which is the first that can work in a partially observable world? Which is the first that can say that one way of reaching the goal is better than another?
> 1. Simple reflex: rules on the current percept.
> 2. Model-based reflex: adds an internal state and a model of the world. It is the first that can handle partial observability.
> 3. Goal-based: adds goals and looks at the results of actions.
> 4. Utility-based: adds a number for how good a state is. It is the first that can compare two ways of reaching the same goal.

## 13. Simple reflex agents (slides 57-60)
![[cmpe421_lecture_02_ch_2_p057.jpg]]
![[cmpe421_lecture_02_ch_2_p058.jpg]]
![[cmpe421_lecture_02_ch_2_p059.jpg]]
![[cmpe421_lecture_02_ch_2_p060.jpg]]

**Notes.** A **simple reflex agent** chooses on the **current percept only**, with **condition-action rules** ("if dirty then suck", "if car-in-front-is-braking then initiate-braking"). `INTERPRET-INPUT` describes the situation, `RULE-MATCH` finds the first matching rule, `RULE-ACTION` reads off its action.

It works only if the right decision can be made from the current percept, so only if the environment is **fully observable**. Otherwise infinite loops can occur. Example: a vacuum agent with no location sensor does Left on $[\mathit{Clean}]$ forever and never reaches B. Randomising the move is one way out. Memory is the other.

> [!warning] Slide slips
> Slide 59 writes "4T" for $4^{T}$. Slide 60 writes `RULE-MATCH(state, rule)`. The second argument is the rule set `rules`.

> [!question]- Check (Ch.2 deck, slide 57): Read the figure. Which two boxes lie on the path from Sensors to Actuators, and what feeds the second box? Which ovals of the model-based figure (slide 61) are missing here?
> The path is Sensors, "What the world is like now", "What action I should do now", Actuators. The oval "Condition-action rules" feeds the second box.
> Missing: "State", "How the world evolves" and "What my actions do". The simple reflex agent has no memory and no model. The first box is filled from the current percept alone.

> [!question]- Check (Ch.2 deck, slide 58): A thermostat should switch the heater on below 20 °C, switch it off above 22 °C, and between 20 and 22 °C keep the heater as it is. Write the first two as condition-action rules. Why can a simple reflex agent that senses only the temperature not carry out the third?
> Rules: "if temperature < 20 then heater on" and "if temperature > 22 then heater off".
> "Keep the heater as it is" depends on whether the heater is on now. This is not in the percept (only the temperature is). A simple reflex agent ignores the percept history, so it does not know what it did before.
> It needs the heater status as a second percept, or one bit of internal state. With the internal state it is a model-based reflex agent.

> [!question]- Check (Ch.2 deck, slide 59): For a lifetime of $T=6$ steps, how many percept sequences of length 6 does the vacuum world have, and how many entries does the reflex agent need? What does the agent give up for this reduction?
> Sequences of length 6: $4^{6}=4096$. The reflex agent needs 4 entries, one per percept. That is $4096/4=1024$ times fewer.
> The price: the agent ignores the history. Two different histories that end in the same percept get the same action. For example it cannot stop when it has seen that both squares are clean.

> [!question]- Check (Ch.2 deck, slide 60): A vacuum agent has no location sensor, so its percept is only Clean or Dirty. It starts in A. A is clean, B is dirty. On Clean it chooses Left or Right with probability 0.5 each (Left in A does nothing). What is the probability that it has not reached B at all after 3 actions? Compare with the fixed rule "on Clean go Left".
> It has never left A only if it chooses Left three times: $0.5\times0.5\times0.5=0.125$.
> So it reaches B within 3 actions with probability $1-0.125=0.875$.
> With the fixed rule it chooses Left for ever and never reaches B. This is the infinite loop of the slide. The random choice breaks the loop without any memory.

## 14. Model-based reflex agents (slides 61-63)
![[cmpe421_lecture_02_ch_2_p061.jpg]]
![[cmpe421_lecture_02_ch_2_p062.jpg]]
![[cmpe421_lecture_02_ch_2_p063.jpg]]

**Notes.** A **model-based reflex agent** keeps an **internal state** that depends on the percept history and tracks the part of the world it cannot see now. To update the state it needs a **model of the world**: how the world evolves by itself, and how the agent's actions affect it. Example: when changing lanes, keep track of the cars you cannot see.

The **state** is what the agent believes now. It changes every step. The **model** is the fixed knowledge of how the world changes. The only change from the simple reflex program is the line `state ← UPDATE-STATE(state, action, percept)`.

A model-based vacuum agent that remembers which squares are clean and does NoOp when both are clean scores 9 in the run of section 5, against 6 for the simple reflex agent.

> [!question]- Check (Ch.2 deck, slide 61): In the figure, three ovals feed the box "What the world is like now" together with the sensors. Name them and give a vacuum-world example of each.
> - "State": what the agent believed before, for example "B was clean when I left it".
> - "How the world evolves": for example "a clean square stays clean".
> - "What my actions do": for example "Right in A brings me to B" and "Suck makes the current square clean".

> [!question]- Check (Ch.2 deck, slide 62): A model-based vacuum agent has the state (agent in A, A clean, B unknown). Its last action was Right. The new percept is $[B,\mathit{Dirty}]$. What does UPDATE-STATE return, and which action follows? Do one more step with the percept $[B,\mathit{Clean}]$.
> Step 1: UPDATE-STATE gives (agent in B, A clean, B dirty). A stays "clean" because the model says clean squares stay clean. The matching rule is "current square dirty", so the action is Suck.
> Step 2: the last action is Suck and the percept is $[B,\mathit{Clean}]$. The new state is (agent in B, A clean, B clean). The rule "both squares clean" matches, so the action is NoOp.
> A simple reflex agent would go Left here, because it has forgotten that A is clean.

> [!question]- Check (Ch.2 deck, slide 63): Sort each piece of knowledge into "how the world evolves independently of the agent" or "how the agent's own actions affect the world": (a) an overtaking car is closer now than one second ago, (b) turning the steering wheel clockwise turns the car to the right, (c) a traffic light goes from green to yellow to red, (d) after 5 minutes of driving north at 60 miles per hour the car is 5 miles further north.
> (a) World evolves by itself.
> (b) Effect of my action.
> (c) World evolves by itself.
> (d) Effect of my action: $60\ \text{miles/hour}\times\dfrac{5}{60}\ \text{hour}=5\ \text{miles}$.

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

> [!question]- Check (Ch.2 deck, slide 64): Compare this figure with the model-based figure of slide 61. Which box is new, and which oval has replaced "Condition-action rules"? What can the agent do now that it could not do before?
> New box: "What it will be like if I do action A". The oval "Goals" has replaced "Condition-action rules".
> The agent now predicts the result of an action with its model and checks it against the goal. It takes the future into account. A reflex agent only reacts to the present situation.

> [!question]- Check (Ch.2 deck, slide 65): Vacuum world, goal "both squares clean". The agent is in A, A is clean, B is dirty. What is the shortest action sequence that reaches the goal? How many action sequences of length 2 exist with the 4 actions, and how many of them reach the goal?
> Shortest sequence: Right, Suck (2 actions). One action is not enough, because the agent must first be in B.
> Sequences of length 2: $4\times4=16$. Only one of them, (Right, Suck), reaches the goal.
> Finding the good sequence among many is the job of search and planning. With longer sequences the number grows fast: $4^{n}$ for length $n$.

> [!question]- Check (Ch.2 deck, slide 66): An agent can try for goal $G_1$ (worth 10, success probability 0.3) or for goal $G_2$ (worth 4, success probability 0.9). Failure is worth 0. Which goal does a utility-based agent choose?
> $\mathrm{EU}(G_1)=0.3\times10+0.7\times0=3$.
> $\mathrm{EU}(G_2)=0.9\times4+0.1\times0=3.6$.
> $3.6>3$, so it tries for $G_2$. This is the slide's "select between several goals based on likelihood of success": the larger prize loses because it is too unlikely.

> [!question]- Check (Ch.2 deck, slide 67): Two routes reach the destination. Route A: 20 minutes, accident probability 0.01. Route B: 25 minutes, accident probability 0.001. Use $U=-\text{minutes}-1000$ for a trip with an accident and $U=-\text{minutes}$ otherwise. Which route is chosen? What if the accident penalty is 100 instead of 1000? Why can a goal-based agent not decide?
> Penalty 1000: $\mathrm{EU}_A=-20-0.01\times1000=-30$. $\mathrm{EU}_B=-25-0.001\times1000=-26$. Route B is chosen.
> Penalty 100: $\mathrm{EU}_A=-20-0.01\times100=-21$. $\mathrm{EU}_B=-25-0.001\times100=-25.1$. Route A is chosen.
> The utility function fixes the trade-off between the two conflicting goals "fast" and "safe". A goal-based agent only tests "destination reached: yes or no". Both routes pass this test, so it has no reason to prefer one.

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

> [!question]- Check (Ch.2 deck, slide 68): Read the figure. Which component receives the "Performance standard"? Which labelled arrows connect the learning element with the critic, the performance element and the problem generator?
> The critic receives the performance standard (and the percepts from the sensors).
> Critic to learning element: "feedback".
> Learning element to performance element: "changes". Performance element to learning element: "knowledge".
> Learning element to problem generator: "learning goals".

> [!question]- Check (Ch.2 deck, slide 69): A learning vacuum robot has four modules. Name the component for each: (a) it chooses Suck, Left or Right from the percept, (b) it reports "the floor was dirty in 12 of the last 20 steps, the target is at most 4", (c) it changes the rules so that the robot visits the kitchen more often, (d) it proposes to try the unknown route under the sofa. Which module stands for exploration?
> (a) Performance element.
> (b) Critic: it compares what happened with the fixed standard.
> (c) Learning element: it changes the performance element.
> (d) Problem generator. This is exploration: the route may be worse today, but the robot learns something new. Following the current best rules in (a) is exploitation.

> [!question]- Check (Ch.2 deck, slide 70): The critic judges the agent against a performance standard that is fixed and comes from outside. What could go wrong if the learning agent were allowed to change this standard itself?
> The agent could get a good judgement in the easy way: it lowers the standard until its present behaviour counts as good. For example a vacuum agent declares that dirty floors are fine.
> Then the feedback no longer says anything about the world, and the learning element stops improving the real behaviour. A fixed outside standard keeps "better" tied to what the designer wants.

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

> [!question]- Check (handout, slide 5): A is dirty, B is clean, the agent is in A, and $T=6$ steps. Agent X does Suck and then NoOp five times. Agent Y has an extra action Dump (a hypothetical action, it is not on the slide: it makes the current square dirty again) and does Suck, Dump, Suck, Dump, Suck, Dump. Score both with measure 1 (one point per square cleaned up) and with measure 2 (one point per clean square per step, minus one per move, counted after each action). Which measure is better?
> Measure 1: X cleans once, score 1. Y cleans three times, score 3. The useless agent Y wins.
> Measure 2: nobody moves. X has 2 clean squares after every step: $2+2+2+2+2+2=12$. Y has 2, 1, 2, 1, 2, 1 clean squares: $2+1+2+1+2+1=9$. X wins.
> Measure 2 is better. It scores the state of the environment (clean floor), so cheating by making new dirt does not pay.

> [!question]- Check (handout, slide 9): The only sensor of the shopping agent is "HTML pages". Name two things from its P or E lines that matter for the score but are not directly in the percepts. What does this say about observability?
> Examples: the real quality of the product (the page only shows the vendor's description) and how reliable the shipper is. The offers on sites the agent has not visited are another example.
> The relevant state is larger than what the sensors deliver, so the environment is partially observable. This fits the "No" in the first row of the handout's table (handout, slide 16).

> [!question]- Check (handout, slide 16): The slide says "The environment type largely determines the agent design". The Taxi column has "No" in all six rows. For each "No", name what the agent design needs because of it.
> - Not fully observable: an internal state and a model, to keep track of what it cannot see.
> - Not deterministic: reasoning with probabilities and expected utility.
> - Not episodic (sequential): thinking ahead, so search or planning.
> - Not static (dynamic): fast decisions, because the world does not wait.
> - Not discrete (continuous): it cannot list all states or actions, so it needs methods for real-valued quantities.
> - Not single-agent: it must take the behaviour of other agents into account.

> [!question]- Check (handout, slide 20): The agent senses only the dirt status and remembers `location` (initially A). In truth it starts in B. A is dirty, B is clean. Assume that Right in B does nothing: the agent stays in B. Trace 4 steps: percept, action, believed location and true location after the action. Which action is wasted? When is the belief correct again?
> | Step | Percept | Action | Believed location after | True location after |
> |---|---|---|---|---|
> | 1 | Clean | Right | B | B (bumps into the wall) |
> | 2 | Clean | Left | A | A |
> | 3 | Dirty | Suck | A | A |
> | 4 | Clean | Right | B | B |
>
> Step 1 is wasted: the agent believes it is in A and goes Right, but it is already in B.
> After step 1 the belief (B) and the truth (B) agree, only because the wall stopped the move. From then on the agent behaves correctly.
> This answers the slide's "What if we didn't know this?": a wrong start belief costs actions. Without a location sensor the agent cannot notice the error itself.

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

> [!question]- 2. How many rows does a table-driven agent need for $\lvert\mathcal P\rvert=3$, $T=4$? And for the vacuum world with $T=5$?
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
