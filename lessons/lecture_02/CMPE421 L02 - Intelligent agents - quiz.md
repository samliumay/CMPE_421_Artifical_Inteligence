---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L02 - Intelligent agents
type: quiz
questions: 62
---

# CMPE421 L02 - Intelligent agents - quiz

Lesson note: [[CMPE421 L02 - Intelligent agents]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Agents and rational agents (slides 2-3)

### Q1 (Ch.2 deck, slide 2)

A room thermostat measures the temperature and switches a heater on or off. In the agent picture of slide 2 there is a box with "?" between the sensors and the actuators. Which description is right?

![[cmpe421_lecture_02_ch_2_p002.jpg]]

- A) Sensor: the thermometer. Percept: "19 °C". Actuator: the heater switch. Action: "heater on". The "?" box is the agent program.
- B) Sensor: the heater switch. Percept: "heater on". Actuator: the heater. Action: "warm the room". The "?" box is the performance measure.
- C) Sensor: the thermometer. Percept: "19 °C". Actuator: the heater switch. Action: "heater on". The "?" box is the performance measure.
- D) Sensor: the heater switch. Percept: "heater on". Actuator: the heater. Action: "warm the room". The "?" box is the agent program.

> [!success]- Answer
> **A.** The thermometer senses, one reading is a percept, the switch acts, and "heater on" is an action. The "?" is the agent program: the rule that turns readings into switch commands. It is the part the designer writes.
> - B: The switch is what the agent acts with, so it is an actuator, not a sensor. And the performance measure is a score computed from outside, not a box inside the agent.
> - C: The performance measure is the designer's score, computed from outside. It is not a box inside the agent.
> - D: The switch is what the agent acts with, so it is the actuator. The sensor is the thermometer.

### Q2 (Ch.2 deck, slide 3)

The ghosts in Pac-Man move partly at random. Slide 2 says: "A rational agent selects actions that maximize its (expected) utility." Which word becomes necessary because of the random ghosts?

- A) "Expected": Pac-Man cannot know where a ghost will go, so he chooses the move that is best on average over the possible ghost moves.
- B) "Utility": random ghosts make the points vary, so Pac-Man needs a number for each state instead of a yes/no goal.
- C) "Selects": random ghosts force Pac-Man to choose a move at random too, so that the ghosts cannot predict him.
- D) "Maximize": Pac-Man must take the move with the highest actual score, whatever the ghosts do next.

> [!success]- Answer
> **A.** Pac-Man cannot know the real result of a move, because it depends on the random ghost moves. He can only maximise the average over the possible ghost moves: the expected utility.
> - B: A utility is needed to compare states, also with no randomness. The randomness is what makes "expected" necessary.
> - C: Random opponents do not require random actions. A rational agent picks the move with the best expected result.
> - D: The actual score depends on the ghosts, which Pac-Man cannot know. He can only maximise the expected score.

## 2. Agents in action (slides 5-17)

### Q3 (Ch.2 deck, slide 5)

Deep Blue searched about 200 million positions per second. How many positions is that in a 3-minute think? Does the slide claim that this makes Deep Blue think like a human?

- A) $6\times10^{8}$ positions. Yes: the slide calls Deep Blue "intelligent creative", so it thinks like a human.
- B) $3.6\times10^{10}$ positions. Yes: the slide calls Deep Blue "intelligent creative", so it thinks like a human.
- C) $6\times10^{8}$ positions. No: "intelligent creative" is in quotation marks, and Kasparov said it "hasn't proven anything".
- D) $3.6\times10^{10}$ positions. No: "intelligent creative" is in quotation marks, and Kasparov said it "hasn't proven anything".

> [!success]- Answer
> **D.** $3\ \text{min}=180\ \text{s}$. Positions $=2\times10^{8}\times180=3.6\times10^{10}$.
> The quotation marks show doubt. Deep Blue is an agent because it chooses good moves. Its method, very fast search, is not the human method.
> - A: $2\times10^{8}\times3=6\times10^{8}$ forgets to turn minutes into seconds. And the slide does not claim human thinking.
> - B: The number is right, but the slide does not claim that Deep Blue thinks like a human.
> - C: The reading of the slide is right, but the minutes were not turned into seconds.

### Q4 (Ch.2 deck, slides 6-7)

Deep Blue and the Atari agents of slide 6 both play games. Where does the playing knowledge come from in each case, and what are the percepts of an Atari agent?

- A) Deep Blue learned its play from many games it played. The Atari agents learn from the score. Atari percepts: the screen pixels and the score.
- B) Deep Blue: programmers wrote the search and the evaluation of positions. The Atari agents learn from the score. Atari percepts: the screen pixels and the score.
- C) Deep Blue: programmers wrote the search and the evaluation of positions. The Atari agents learn from the score. Atari percepts: the joystick moves and the score.
- D) Deep Blue learned its play from many games it played. The Atari agents learn from the score. Atari percepts: the joystick moves and the score.

> [!success]- Answer
> **B.** Deep Blue's knowledge was written by its programmers: the search and the evaluation of positions. The Atari agents use reinforcement learning: they start without knowing the game and improve from the score.
> The Atari agent sees the screen pixels and the score. The joystick moves are its actions.
> - A: Learning from its own games is the Atari (reinforcement learning) idea, not Deep Blue's.
> - C: The joystick moves are actions (actuator side), not percepts.
> - D: Both parts are wrong: Deep Blue was programmed, and joystick moves are actions.

### Q5 (Ch.2 deck, slide 8)

A simulated figure learns to walk. The figure is labelled "Iteration 0" and its body is bent over. What do you expect at iteration 0 and at a late iteration?

![[cmpe421_lecture_02_ch_2_p008.jpg]]

- A) At iteration 0 the movements are almost random and it falls. Later it walks better, because after each round the program moves toward actions with a higher score.
- B) At iteration 0 it already walks slowly, because walking was programmed in. Later it only walks faster, because the program adds speed each round.
- C) At iteration 0 it stands still, because it has no actions yet. Later it walks, because each round adds one new action to the program.
- D) At iteration 0 the movements are almost random and it falls. Later it walks better, but only because a human corrects the program after each round.

> [!success]- Answer
> **A.** Iteration 0 is the start of learning: the agent has learned nothing, so it moves almost at random and falls. After each round of trials the program is changed toward the actions that gave a higher score, so it walks better.
> - B: Nothing about walking is programmed in. The agent learns it.
> - C: The actions (joint motions) exist from the start. What changes is which actions the program chooses.
> - D: The improvement comes from the score, not from a human. That is the point of learning.

### Q6 (Ch.2 deck, slides 9-10)

The slide says "Reality much harder than simulations". In all options below, real motors do not do exactly what was commanded (slipping wheels). Which option also gets the sensors and the time right?

- A) Sensors: real cameras give the exact state, but more slowly than a simulator. Time: the real world keeps changing while the robot computes.
- B) Sensors: real cameras are noisy and see only part of the scene. Time: the real world waits for the robot, just as a simulator does.
- C) Sensors: real cameras are noisy and see only part of the scene. Time: the real world keeps changing while the robot computes.
- D) Sensors: real cameras give the exact state, but more slowly than a simulator. Time: the real world waits for the robot, just as a simulator does.

> [!success]- Answer
> **C.** Real sensors are noisy and partial; a simulator can give the exact state. The real world does not stop while the robot thinks; a simulator can wait.
> - A: Exact state is what a simulator gives. Real sensors are noisy and partial.
> - B: The real world goes on while the robot computes. Only a simulator can wait.
> - D: Both parts describe the simulator, not reality.

### Q7 (Ch.2 deck, slide 12)

A robot arm hands objects to a person. Why is this harder for the robot than working alone at the same table?

- A) The person is part of the environment like any moving object. The robot only needs faster sensors to follow the moving hand.
- B) The person is another agent. The robot cannot control or exactly predict the person, and it must act safely toward a shared result.
- C) The person is another agent who competes with the robot for the objects. The robot must act so that it wins the objects first.
- D) The person adds noise to the robot's sensors. The setting becomes partially observable, but it stays a single-agent task.

> [!success]- Answer
> **B.** The person is another agent with own movements and aims. The robot cannot control them or predict them exactly. It must perceive what the person does and wants, and act safely and helpfully. They work toward a shared result, so the setting is cooperative.
> - A: A person is not a falling leaf. The person acts with a purpose, and the result for both depends on the robot's actions.
> - C: Handing over objects is cooperative: both want the handover to succeed.
> - D: The person is an agent, so the task is multi-agent. Sensor noise is not the main difficulty.

### Q8 (Ch.2 deck, slide 13)

Take the weather forecast, the thermostat and the route planner of slide 13. Which one only predicts, which one decides and acts, and where does the route planner fit?

- A) The forecast only predicts. The thermostat decides and acts. The route planner only predicts the travel time of each route.
- B) The forecast and the thermostat both decide and act. The route planner predicts the travel times and recommends a route.
- C) The forecast only predicts. The thermostat decides and acts. The route planner predicts the travel times and recommends a route.
- D) The forecast and the thermostat both decide and act. The route planner only predicts the travel time of each route.

> [!success]- Answer
> **C.** The forecast outputs an estimate; the human decides what to do with it. The thermostat reads the temperature and switches the heating by itself. The route planner does both: it predicts the minutes of each route and then recommends one.
> - A: The route planner does more than predict: it recommends the best route.
> - B: Choosing what to announce is not acting on the world. The forecast only predicts.
> - D: Both parts are wrong: the forecast only predicts, and the planner also recommends.

### Q9 (Ch.2 deck, slide 14)

ASR and TTS in a voice assistant: what does each do, in which order do ASR, the dialog system and TTS work, and which one sits on the sensor side of the agent?

- A) ASR turns sound into text, TTS turns text into sound. Order: ASR, dialog system, TTS. ASR sits on the sensor side.
- B) ASR turns sound into text, TTS turns text into sound. Order: ASR, dialog system, TTS. TTS sits on the sensor side.
- C) ASR turns text into sound, TTS turns sound into text. Order: TTS, dialog system, ASR. TTS sits on the sensor side.
- D) ASR turns text into sound, TTS turns sound into text. Order: TTS, dialog system, ASR. ASR sits on the sensor side.

> [!success]- Answer
> **A.** ASR (automatic speech recognition) turns sound into text. TTS (text-to-speech) turns text into sound. The user's speech is recognised first, the dialog system decides the answer, TTS speaks it. ASR interprets the percept, so it is on the sensor side; TTS produces the action.
> - B: TTS produces what the user hears. It is on the actuator side.
> - C: The two names are swapped. "Speech recognition" turns sound into text.
> - D: The names are swapped, and so the order is reversed.

### Q10 (Ch.2 deck, slide 15)

Look at the third picture in the top row and its caption "two young girls are playing with lego toy". What does the comparison tell you about such a system?

![[cmpe421_lecture_02_ch_2_p015.jpg]]

- A) The caption fits the picture in every detail. A vision system gives a correct description, so an agent can trust it fully.
- B) The caption is wrong in every word. The system does not recognise objects at all, so its output is of no use to an agent.
- C) The caption is wrong only because the picture is small. With a better camera the system would always give the right description.
- D) The toy fits, but "two young girls" does not seem to: the person behind looks like an adult. Fluent captions can have wrong details.

> [!success]- Answer
> **D.** One small child sits in front; the second person looks like an adult. The toy part fits; "two young girls" does not seem to. A vision system gives a likely description of the percept, not a guaranteed one. An agent that uses it must expect errors.
> - A: The person behind does not look like a young girl, so the caption is not right in every detail.
> - B: The toy and the play are right. The system recognises much, but not everything.
> - C: A better camera does not remove recognition errors. The output stays a likely guess.

### Q11 (Ch.2 deck, slides 16-17)

Match each problem to a course topic of slide 16: (a) an exam timetable for 40 courses with no clashes, (b) the fastest road route between two cities, (c) a robot with noisy sensors that must find out where it is in a building, (d) deciding whether an e-mail is spam.

- A) (a) search, (b) search and planning, (c) probabilistic inference, (d) supervised learning.
- B) (a) constraint satisfaction, (b) search and planning, (c) probabilistic inference, (d) supervised learning.
- C) (a) constraint satisfaction, (b) search and planning, (c) reinforcement learning, (d) supervised learning.
- D) (a) search, (b) search and planning, (c) reinforcement learning, (d) supervised learning.

> [!success]- Answer
> **B.** (a) A clash-free timetable is scheduling: constraint satisfaction. (b) Routing: search and planning. (c) Finding its position from noisy sensors is localisation: probabilistic inference. (d) Spam detection: supervised learning.
> - A: A timetable is a set of constraints (no two exams of one student at the same time). Slide 16 puts scheduling under constraint satisfaction.
> - C: Reinforcement learning is for learning how to act (navigation). Finding out where the robot is, is localisation: probabilistic inference.
> - D: Both (a) and (c) are wrong, as explained for A and C.

## 3. Agent function and agent program (slides 18-21)

### Q12 (Ch.2 deck, slide 18)

A program finds the best chess move but never outputs it. Following the first quotation of slide 18, why can we not call it intelligent, and what is missing in the agent picture?

- A) It does not think like a human, it only searches. Missing: the sensors and the percept arrow.
- B) We can only judge intelligence from behaviour, and it never acts. Missing: the sensors and the percept arrow.
- C) We can only judge intelligence from behaviour, and it never acts. Missing: the actuators and the action arrow.
- D) It does not think like a human, it only searches. Missing: the actuators and the action arrow.

> [!success]- Answer
> **C.** The quotation says we can only assess intelligence through behaviour. A system that never acts shows nothing we could measure. In the picture the actuators and the action arrow are missing: without actions the environment never changes, and there is no performance to score.
> - A: The quotation is about acting, not about thinking like a human. And the program does receive the position, so it has percepts.
> - B: The reason is right, but the program gets the position. What it lacks is output, the action.
> - D: The missing part is right, but "human-like thinking" is not the reason of the quotation.

### Q13 (Ch.2 deck, slide 19)

The agent function is $f:\mathcal P^{*}\to\mathcal A$, where the star means "all finite sequences of elements of $\mathcal P$". For $\mathcal P=\{x,y\}$, how many percept sequences of length 2 are there, and on which sequences is $f$ defined?

- A) 3 sequences: $xx$, $xy$, $yy$. $f$ gives an action for every sequence of every length.
- B) 4 sequences: $xx$, $xy$, $yx$, $yy$. $f$ gives an action only for single percepts; the history is kept elsewhere.
- C) 3 sequences: $xx$, $xy$, $yy$. $f$ gives an action only for single percepts; the history is kept elsewhere.
- D) 4 sequences: $xx$, $xy$, $yx$, $yy$. $f$ gives an action for every sequence of every length.

> [!success]- Answer
> **D.** A sequence has an order, so $xy$ and $yx$ are different: $2^{2}=4$ sequences. $f$ maps every sequence of every length to an action: 2 of length 1, 4 of length 2, 8 of length 3, and so on.
> - A: $xy$ and $yx$ are two different sequences (first $x$ then $y$, or the other way).
> - B: The count is right, but $f$ takes whole histories as input. That is what $\mathcal P^{*}$ means.
> - C: Both parts are wrong: order matters, and $f$ is defined on all sequences.

### Q14 (Ch.2 deck, slide 20)

One vacuum agent uses a lookup table. Another uses three if-rules. For every percept sequence they choose the same action. Do they have the same agent function? The same agent program?

- A) Different agent functions and different agent programs: a table and a set of rules are different objects.
- B) The same agent function, but different agent programs: many programs can implement one function.
- C) The same agent function and the same agent program: equal behaviour means the same program.
- D) Different agent functions, but the same agent program: the program is the behaviour, the function is the code.

> [!success]- Answer
> **B.** The agent function is only the map from percept sequences to actions, and the two maps are equal. The agent program is the concrete code that produces the map; a table and three rules are different code.
> - A: The function does not care how the map is produced. Equal maps are one function.
> - C: Two different pieces of code can produce the same behaviour. They stay different programs.
> - D: The definitions are swapped: the function is the map (behaviour), the program is the code.

### Q15 (Ch.2 deck, slide 21)

What is missing in this statement? "A rational agent selects the action that maximises its performance measure."

- A) "Expected" (it maximises the expected performance) and "given the evidence of its percept sequence and its built-in knowledge".
- B) "Expected" (it maximises the expected performance) and "given full knowledge of the true state of the world".
- C) "Human-like" (it chooses as a human would) and "given the evidence of its percept sequence and its built-in knowledge".
- D) "Human-like" (it chooses as a human would) and "given full knowledge of the true state of the world".

> [!success]- Answer
> **A.** The agent cannot know the real outcome, so it maximises the expected value. And an action is judged against what the agent could know: its percepts so far and its built-in knowledge.
> - B: Full knowledge of the true state is omniscience. Rationality does not assume it.
> - C: Rationality is about the best expected result, not about acting like a human.
> - D: Both parts are wrong, as explained for B and C.

## 4. The vacuum-cleaner world (slides 22-24)

### Q16 (Ch.2 deck, slide 22)

In the vacuum world (squares A and B) the agent is in A and both squares are dirty. The agent senses its location and whether its own square is dirty. Which percept does it receive, and how many world states fit this percept?

- A) The percept is $[A,\mathit{Dirty},B\ \mathit{Dirty}]$. Exactly 1 world state fits it.
- B) The percept is $[A,\mathit{Dirty}]$. Exactly 1 world state fits it.
- C) The percept is $[A,\mathit{Dirty},B\ \mathit{Dirty}]$. 2 world states fit it.
- D) The percept is $[A,\mathit{Dirty}]$. 2 world states fit it.

> [!success]- Answer
> **D.** The agent senses only its location and its own square: $[A,\mathit{Dirty}]$. The percept says nothing about B, so two states fit: (in A, A dirty, B dirty) and (in A, A dirty, B clean).
> - A: The sensor does not report B. The percept has only two parts.
> - B: The percept is right, but B can be clean or dirty, so 2 states fit.
> - C: The percept is wrong, and with this percept only 1 state would fit.

### Q17 (Ch.2 deck, slide 23)

The table of the agent function has the row $[A,\mathit{Clean}],[A,\mathit{Clean}]\to$ Right. Can this percept sequence happen if the agent follows the table and Right always moves it to B? Why is the row in the table? How many rows of length 2 does the full table have?

- A) It cannot happen. The row is there because $f$ must give an action for every percept sequence. 16 rows of length 2.
- B) It cannot happen. The row is there because $f$ must give an action for every percept sequence. 8 rows of length 2.
- C) It can happen: the agent may stay in A to check it again. That is why the row exists. 16 rows of length 2.
- D) It can happen: the agent may stay in A to check it again. That is why the row exists. 8 rows of length 2.

> [!success]- Answer
> **A.** After $[A,\mathit{Clean}]$ the table says Right, so the agent is in B and the next percept starts with B. The row still exists because $f$ must be defined for every sequence, also for those that occur only when something goes wrong (a failed move). There are 4 percepts, so $4\times4=16$ rows of length 2.
> - B: $8$ is $4\times2$. Each of the two positions can hold any of the 4 percepts: $4^{2}=16$.
> - C: On $[A,\mathit{Clean}]$ the table says Right, so the agent does not stay in A.
> - D: The agent does not stay in A, and the count is $4^{2}=16$, not 8.

### Q18 (Ch.2 deck, slide 24)

The REFLEX-VACUUM-AGENT does Suck if its square is dirty, else Right in A and Left in B. It starts in B. A is dirty, B is clean. Run it for 5 steps. Score: one point per clean square per step, counted after each action. What is the score?

- A) 8
- B) 10
- C) 5
- D) 9

> [!success]- Answer
> **D.** Step 1: $[B,\mathit{Clean}]$, Left, 1 clean square. Step 2: $[A,\mathit{Dirty}]$, Suck, 2. Steps 3-5: Right, Left, Right, 2 each.
> Score $=1+2+2+2+2=9$.
> - A: 8 counts the clean squares before each action: $1+1+2+2+2$.
> - B: 10 counts 2 clean squares in every step. After step 1 A is still dirty.
> - C: 5 subtracts one point for each of the 4 moves. This measure has no move cost.

## 5. Rationality (slides 25-28)

### Q19 (Ch.2 deck, slide 25)

A designer scores a taxi agent by "number of smooth brake applications". Which rule on the slide does this break, and what is a better measure?

- A) It breaks "the measure must be objective". Better: score passengers delivered, no collisions, travel time and comfort.
- B) It breaks "measure what you want in the environment, not how the agent should behave". Better: score smooth steering and smooth braking together.
- C) It breaks "measure what you want in the environment, not how the agent should behave". Better: score passengers delivered, no collisions, travel time and comfort.
- D) It breaks "the measure must be objective". Better: score smooth steering and smooth braking together.

> [!success]- Answer
> **C.** Smooth braking is a behaviour. The agent can collect points by braking smoothly again and again without taking anybody anywhere. Score the results in the world instead.
> - A: Counting brake applications is objective. The problem is that it scores behaviour.
> - B: The rule is right, but steering and braking are still behaviours. The new measure has the same problem.
> - D: Both parts are wrong, as explained for A and B.

### Q20 (Ch.2 deck, slide 26)

A vacuum agent stops for ever (NoOp) once it knows both squares are clean. With "one point per clean square per step, minus one per move" and squares that stay clean, this is rational. Now change only the prior knowledge: a clean square can become dirty again at any time. Is the same agent still rational?

- A) Yes. It knows that both squares are clean, so stopping is still the best action.
- B) No. It should check the squares from time to time, when the expected gain from new dirt is larger than the cost of a move.
- C) No. It should move back and forth in every step, so that no new dirt is ever missed, whatever the moves cost.
- D) Yes. The performance measure did not change, so an agent that was rational stays rational.

> [!success]- Answer
> **B.** If dirt can come back, an agent that never looks again loses a point in every step a square is dirty. A rational agent checks when the expected gain is larger than the move cost. Only the prior knowledge changed, and it changed what is rational.
> - A: The agent knew the squares were clean at one time. With the new knowledge they may not stay clean.
> - C: Each move costs a point. Moving in every step can cost more than the dirt it finds.
> - D: Rationality depends on four things, one of them the prior knowledge. Changing it can change the rational action.

### Q21 (Ch.2 deck, slide 27)

Carrying an umbrella costs 1. Getting wet costs 5. The forecast says rain with probability 0.1. Which choice is rational? It then rains. Was the agent rational? Perfect? What would an omniscient agent have done?

- A) Take the umbrella, because 1 is less than 5. After the rain the agent was rational and perfect, because it stayed dry.
- B) Go without, because $0.5<1$. After the rain it was still rational, but not perfect. An omniscient agent takes the umbrella.
- C) Go without, because $0.5<1$. After the rain it was not rational, because the result was bad. An omniscient agent takes the umbrella.
- D) Take the umbrella, because 1 is less than 5. After the rain it was rational, but not perfect. An omniscient agent goes without.

> [!success]- Answer
> **B.** Expected cost without umbrella: $0.1\times5+0.9\times0=0.5$. With umbrella: $1$. So going without is rational.
> When it rains the real cost is 5. The choice was still the best for what the agent knew, so it was rational. It was not perfect: the actual result was not the best possible. An omniscient agent knows it will rain and takes the umbrella (cost 1).
> - A: Comparing 1 with 5 ignores the probability 0.1 of rain. The expected cost of going without is only 0.5.
> - C: Rationality is judged by the expected result at the time of choice, not by the outcome.
> - D: The umbrella is not the rational choice, and an omniscient agent who knows it will rain takes it.

### Q22 (Ch.2 deck, slide 28)

An agent crosses a road without looking. Its percept sequence contains no car, so it says "crossing was rational given my percepts". What is wrong with this argument?

- A) Rationality requires information gathering. Looking is a cheap action that changes the future percepts and raises the expected performance, so the agent should look first.
- B) Nothing is wrong. Rationality is judged only against the percepts the agent had, and its percepts showed no car on the road.
- C) A rational agent must be omniscient on a road. It must know where every car is before it crosses, whatever its sensors are.
- D) It depends on the result. If no car came, crossing was rational. If a car came, crossing was not rational.

> [!success]- Answer
> **A.** The slide says rationality requires information gathering. Looking left and right costs almost nothing and changes the future percepts, so it raises the expected performance. A rational agent first chooses the looking action.
> - B: The agent chose not to collect percepts. Rationality also covers the choice to gather information.
> - C: Rational is not omniscient. The agent must look, not know everything.
> - D: Rationality does not depend on the outcome. Not looking is irrational whether a car came or not.

## 6. PEAS (slides 29-30)

### Q23 (Ch.2 deck, slide 29)

A robot vacuum cleans a flat. In all options: P = fraction of the floor that is clean after one hour; E = furniture, the cat; A = wheels, suction motor. Where do the bumper switch and the battery gauge go?

- A) Bumper switch: A (it is pressed when the robot drives into something). Battery gauge: P (a low battery is bad performance).
- B) Bumper switch: A (it is pressed when the robot drives into something). Battery gauge: S (the robot reads its charge from it).
- C) Bumper switch: S (it tells the robot that it touched something). Battery gauge: P (a low battery is bad performance).
- D) Bumper switch: S (it tells the robot that it touched something). Battery gauge: S (the robot reads its charge from it).

> [!success]- Answer
> **D.** Both are things the robot reads. The bumper switch reports a collision; the battery gauge reports the charge. So both are sensors.
> - A: The switch is pressed by the world, not by the robot's choice. It delivers information, so it is a sensor. The gauge is not a score; the designer's score is the clean floor.
> - B: The gauge is right, but the bumper switch does not move anything. It is a sensor.
> - C: The bumper switch is right, but P is the designer's score of the result (clean floor), not a reading inside the robot.

### Q24 (Ch.2 deck, slide 30)

A PEAS description for a parcel delivery drone. In all options A = rotors, parcel release; E also holds airspace, buildings, trees, birds, other drones and the customers; S also holds camera, GPS, altimeter and battery gauge. Where do "parcels delivered on time and undamaged" and "weather" belong?

- A) "Delivered on time and undamaged" in S, because the drone must know it. "Weather" in E.
- B) "Delivered on time and undamaged" in P, because the designer scores it from outside. "Weather" in E.
- C) "Delivered on time and undamaged" in P, because the designer scores it from outside. "Weather" in S.
- D) "Delivered on time and undamaged" in S, because the drone must know it. "Weather" in S.

> [!success]- Answer
> **B.** "Delivered on time" is a result the designer computes from outside: it belongs in P. The drone only senses its position and the clock. The weather is part of the world around the drone: E. As in the taxi slide, weather is listed under E.
> - A: The drone does not sense "on time and undamaged" directly. It is the score, so P.
> - C: Weather is something in the world. A sensor is a device of the drone, like a camera.
> - D: Both are wrong, as explained for A and C.

## 7. Fully or partially observable (slides 31-34)

### Q25 (Ch.2 deck, slide 31)

The two-square vacuum agent senses its location and the status of its own square. Give it a second sensor that reports the status of the other square. How many percepts are possible now, and is the world now fully observable? (Dirt does not appear by itself.)

- A) 4 percepts. Fully observable: each percept identifies one world state.
- B) 8 percepts. Fully observable: each percept identifies one world state.
- C) 8 percepts. Still partially observable: the agent senses only one square at a time.
- D) 4 percepts. Still partially observable: the agent senses only one square at a time.

> [!success]- Answer
> **B.** A percept holds the location (2), the status here (2) and the status of the other square (2): $2\times2\times2=8$. The world has 8 states, and each percept fits exactly one. So it is fully observable, and the agent needs no internal state.
> - A: The new sensor adds a third part with 2 values, so the count doubles from 4 to 8.
> - C: The new sensor reports the other square too, so the agent senses both squares.
> - D: Both parts are wrong, as explained for A and C.

### Q26 (Ch.2 deck, slide 34)

The table says Backgammon is FULL, although nobody knows the next dice roll. It says Internet shopping is PARTIAL. Which explanation of both entries is right?

- A) Backgammon: the dice belong to the agent, not to the state, so they do not count. Shopping: the agent sees only the pages it has loaded, not offers and stock elsewhere.
- B) Backgammon: the board and the dice just rolled are visible; the next roll makes the game stochastic. Shopping: the web changes while the agent browses.
- C) Backgammon: the board and the dice just rolled are visible; the next roll makes the game stochastic. Shopping: the agent sees only the pages it has loaded, not offers and stock elsewhere.
- D) Backgammon: the dice belong to the agent, not to the state, so they do not count. Shopping: the web changes while the agent browses.

> [!success]- Answer
> **C.** Observability is about the current state. In Backgammon the whole board and the rolled dice are visible; the unknown future roll is about the Deterministic row. In shopping, offers on other sites and vendor stock are relevant to the choice but are not in the percept.
> - A: The rolled dice are part of the state, and they are visible. That is why the game is fully observable.
> - B: A web that changes while the agent thinks is about the Static row, not the Observable row.
> - D: Both parts are wrong, as explained for A and B.

## 8. Deterministic, stochastic, strategic (slides 35-37)

### Q27 (Ch.2 deck, slide 35)

Slide 35 uses three words. Deterministic: the next state is fixed by the current state and the agent's action. Stochastic: it is not. Strategic: deterministic except for the actions of other agents (when other agents are the only source of uncertainty, the slide uses "strategic", not "stochastic"). Sudoku is deterministic. Classify chess, and a vacuum world in which Suck cleans the square only with probability 0.75 (this world is stochastic). What is the probability that the square is still dirty after two Suck actions?

- A) Chess is stochastic. The square is still dirty with probability 0.0625.
- B) Chess is strategic. The square is still dirty with probability 0.0625.
- C) Chess is strategic. The square is still dirty with probability 0.5.
- D) Chess is stochastic. The square is still dirty with probability 0.5.

> [!success]- Answer
> **B.** Chess rules are deterministic; the only uncertainty is the opponent's move. That is strategic.
> One Suck fails with probability $1-0.75=0.25$. Two independent failures: $0.25\times0.25=0.0625$.
> - A: The opponent's moves are not chance in the rules. By slide 35, uncertainty from other agents only makes chess strategic.
> - C: $0.5=0.25+0.25$ adds the failure probabilities. Both failures must happen, so multiply.
> - D: Both parts are wrong, as explained for A and C.

### Q28 (Ch.2 deck, slide 37)

Backgammon and Taxi both have NO in the Deterministic row. Use the words of slide 35: deterministic, stochastic, and strategic = deterministic except for other agents' actions (when other agents are the only source of uncertainty, the word is "strategic", not "stochastic"). What is the source of randomness in Taxi, and what would Backgammon be without dice?

- A) Taxi: other drivers and pedestrians, the weather, a tyre that bursts, imprecise actuators. Backgammon without dice: stochastic, because the opponent is unpredictable.
- B) Taxi: other drivers and pedestrians, the weather, a tyre that bursts, imprecise actuators. Backgammon without dice: strategic, like chess.
- C) Taxi: only the noise of its GPS and cameras, which hides the true position. Backgammon without dice: strategic, like chess.
- D) Taxi: only the noise of its GPS and cameras, which hides the true position. Backgammon without dice: stochastic, because the opponent is unpredictable.

> [!success]- Answer
> **B.** In Taxi the same state and action can give different next states because of other road users, the weather, failures and imprecise actuators. Without dice only the opponent's moves are uncertain, so Backgammon becomes strategic, like chess.
> - A: With the words of slide 35, uncertainty that comes only from another agent is called strategic.
> - C: Sensor noise makes the world partially observable (it can make it look stochastic), but it does not change what the next state is. And it is not the only source: other road users, weather and failures are real randomness.
> - D: Both parts are wrong, as explained for A and C.

## 9. Episodic or sequential (slides 38-41)

### Q29 (Ch.2 deck, slide 38)

Find the mistake: "The assembly-line inspector makes thousands of decisions one after the other, so its environment is sequential."

- A) The number of decisions does not matter. Rejecting one part does not change the next part or the best decision for it.
- B) The inspector is the only agent. A single-agent environment cannot be sequential, so it is episodic.
- C) The environment is static: parts do not change while the inspector thinks. Static environments are always episodic.
- D) There is no mistake. Thousands of decisions in a row make the environment sequential by definition.

> [!success]- Answer
> **A.** Sequential means that a decision changes later ones. Accepting or rejecting one part does not change the next part. Each part is its own episode, so the environment is episodic.
> - B: Single-agent and episodic are separate properties. A crossword is single-agent and sequential.
> - C: Static and episodic are separate properties. A crossword is static and sequential.
> - D: Many decisions can still be independent. The question is whether one decision affects the later ones.

### Q30 (Ch.2 deck, slide 40)

All four environments have NO in the Episodic row. Which reasons for Solitaire and for Internet shopping are right?

- A) Solitaire: a game has many moves, one after the other. Shopping: the prices change while the agent compares offers.
- B) Solitaire: a game has many moves, one after the other. Shopping: the link it follows decides which pages it sees next, and money spent is gone.
- C) Solitaire: moving a card changes which cards are free and so which moves are possible later. Shopping: the prices change while the agent compares offers.
- D) Solitaire: moving a card changes which cards are free and so which moves are possible later. Shopping: the link it follows decides which pages it sees next, and money spent is gone.

> [!success]- Answer
> **D.** Both reasons show that a current action changes what is possible later. That is sequential.
> - A: Many moves alone do not make a task sequential (see the inspector). Changing prices are about the Static row.
> - B: The shopping reason is right, but "many moves" is not a reason, as for the inspector.
> - C: The Solitaire reason is right, but changing prices are about the Static row, not the Episodic row.

### Q31 (Ch.2 deck, slide 41)

Why does an agent in an episodic environment not need to think ahead? Suppose every rejected part makes the machine recalibrate, and this changes how many later parts are defective. Is the inspection task still episodic?

- A) Because all episodes are the same, the same action is always right. With recalibration the task is still episodic, because each part is judged alone.
- B) Because the current action has no effect on later episodes. With recalibration the task is still episodic, because each part is judged alone.
- C) Because all episodes are the same, the same action is always right. With recalibration the task becomes sequential.
- D) Because the current action has no effect on later episodes. With recalibration the task becomes sequential.

> [!success]- Answer
> **D.** In an episodic environment the best action now is simply the best action for this episode. With recalibration a decision now changes the future parts, so it has long-term effects: the task is sequential, and the agent should think ahead.
> - A: Episodes need not be the same (parts differ). And the recalibration links the episodes.
> - B: The first part is right, but now a rejection changes later parts. That is sequential.
> - C: The task does become sequential, but the reason for not thinking ahead is that actions do not affect later episodes, not that episodes are equal.

## 10. Static, discrete, single-agent (slides 42-50)

### Q32 (Ch.2 deck, slide 42)

Classify as static, semi-dynamic or dynamic: (a) a crossword with no time limit, (b) the same crossword in a contest where every minute costs one point, (c) taxi driving. What happens in (c) to an agent that thinks for a long time?

- A) (a) static, (b) semi-dynamic, (c) dynamic. In (c) the world goes on while it thinks: not deciding has the effect of deciding to do nothing.
- B) (a) static, (b) dynamic, (c) dynamic. In (c) the world goes on while it thinks: not deciding has the effect of deciding to do nothing.
- C) (a) static, (b) semi-dynamic, (c) dynamic. In (c) a longer think always gives a better action, so it should think as long as it can.
- D) (a) static, (b) dynamic, (c) dynamic. In (c) a longer think always gives a better action, so it should think as long as it can.

> [!success]- Answer
> **A.** (a) Nothing changes while the agent thinks. (b) The grid stays the same, but the score falls with time: semi-dynamic. (c) The traffic moves while the agent thinks: dynamic. In a dynamic world thinking takes time in which the world goes on.
> - B: In (b) the grid does not change. Only the score changes with time: that is semi-dynamic.
> - C: In a dynamic world a late action can be useless. The car may already have hit the obstacle.
> - D: Both parts are wrong, as explained for B and C.

### Q33 (Ch.2 deck, slide 44)

Backgammon has YES in the Static row, although the opponent moves pieces and dice are rolled. Why is it not dynamic?

- A) The opponent's moves count as part of my own action, so the board never changes at all during the game.
- B) While I think, the board and the dice stay as they are. The opponent moves only in its turn: multi-agent and stochastic, not dynamic.
- C) "Dynamic" is only used for continuous worlds. A board game is discrete, so it is static by definition.
- D) I roll the dice myself, so those changes are my actions. Only changes made by other agents would make it dynamic.

> [!success]- Answer
> **B.** Static or dynamic asks one thing: can the environment change while the agent is deliberating? In Backgammon it cannot. The opponent's moves and the dice make the game multi-agent and stochastic.
> - A: The board does change: the opponent moves pieces. It just does not change while I think.
> - C: Static and discrete are separate properties. A discrete world can be dynamic (an English tutor that waits for no one).
> - D: Other agents' moves do not make a game dynamic if they happen in their own turn. Chess is static (without a clock).

### Q34 (Ch.2 deck, slide 45)

The two-square vacuum world: are state, time, percepts and actions discrete, and how many are there? Then let the agent stand at any position $x\in[0,1]$ between the squares, with the actions still Left, Right, Suck and NoOp. What becomes continuous?

- A) All discrete: 8 states, 4 percepts, 4 actions, one action per step. With $x$ the actions become continuous; the state stays discrete.
- B) All discrete: 4 states, 4 percepts, 4 actions, one action per step. With $x$ the state becomes continuous (and the percepts, if it senses $x$).
- C) All discrete: 4 states, 4 percepts, 4 actions, one action per step. With $x$ the actions become continuous; the state stays discrete.
- D) All discrete: 8 states, 4 percepts, 4 actions, one action per step. With $x$ the state becomes continuous (and the percepts, if it senses $x$).

> [!success]- Answer
> **D.** States: $2\times2\times2=8$ (location, A status, B status). Percepts: $2\times2=4$. Actions: 4. Time: one action per step. A position $x\in[0,1]$ has infinitely many values, so the state is continuous; if the agent senses $x$, its percepts are too.
> - A: The actions are still the four named ones, so they stay discrete. The position is part of the state.
> - B: 4 is the number of percepts. The state also holds the status of the other square: 8.
> - C: Both parts are wrong, as explained for A and B.

### Q35 (Ch.2 deck, slide 47)

Backgammon uses dice and still has YES in the Discrete row. Why? How many different rolls of two dice are there if the order of the dice does not matter?

- A) Discrete means no chance acts during a turn; the dice are thrown only between turns. 36 rolls.
- B) Discrete counts the distinct states, percepts and actions; chance belongs to the Deterministic row. 36 rolls.
- C) Discrete counts the distinct states, percepts and actions; chance belongs to the Deterministic row. 21 rolls.
- D) Discrete means no chance acts during a turn; the dice are thrown only between turns. 21 rolls.

> [!success]- Answer
> **C.** Discrete asks how many distinct states, percepts and actions exist, not whether there is chance. The dice change the Deterministic row.
> Rolls: 6 doubles plus $\dfrac{6\times5}{2}=15$ mixed pairs: $6+15=21$.
> - A: Chance is about the Deterministic row, not the Discrete row. Discrete asks how many distinct states, percepts and actions exist. And 36 counts ordered rolls.
> - B: The reason is right, but 36 counts (1,2) and (2,1) as two rolls. Without order they are one.
> - D: The count is right, but the reason is wrong, as explained for A.

### Q36 (Ch.2 deck, slide 48)

Two vacuum robots clean the same flat. Case 1: each is scored on the total cleanliness of the flat. Case 2: each is scored on the amount of dirt it collects itself. Single-agent or multi-agent? Cooperative or competitive?

- A) Case 1: single-agent, because both share one score. Case 2: multi-agent, competitive.
- B) Case 1: multi-agent, cooperative. Case 2: multi-agent, competitive.
- C) Case 1: multi-agent, cooperative. Case 2: multi-agent, cooperative.
- D) Case 1: single-agent, because both share one score. Case 2: multi-agent, cooperative.

> [!success]- Answer
> **B.** In both cases the score of each robot depends on what the other does, so both are multi-agent. In case 1 the two scores are the same number, so helping the other helps me: cooperative. In case 2 dirt collected by the other is dirt I cannot collect: competitive.
> - A: A shared score makes the setting cooperative. There are still two agents whose actions affect the score.
> - C: In case 2 the robots take dirt away from each other, so they compete.
> - D: Both parts are wrong, as explained for A and C.

### Q37 (Ch.2 deck, slide 50)

Follow the finished table of the deck below (slide 50), not the textbook or the handout. For each column count the rows with the "easy" answer (FULL, or YES). Count SEMI as not easy. How many easy rows does Internet shopping have, and which row keeps Solitaire from 6 of 6?

![[cmpe421_lecture_02_ch_2_p050.jpg]]

- A) Internet shopping: 3. Solitaire misses only Observable, because some cards are face down.
- B) Internet shopping: 2. Solitaire misses only Episodic, because it is sequential.
- C) Internet shopping: 3. Solitaire misses only Episodic, because it is sequential.
- D) Internet shopping: 2. Solitaire misses only Observable, because some cards are face down.

> [!success]- Answer
> **B.** Internet shopping: PARTIAL, YES, NO, SEMI, YES, NO gives 2 of 6. Solitaire: FULL, YES, NO, YES, YES, YES gives 5 of 6; the only NO is Episodic.
> - A: SEMI was counted as easy, against the rule. And the deck's table writes FULL for Solitaire; the face-down cards are the handout's view.
> - C: Solitaire is right, but SEMI must not be counted as easy.
> - D: Shopping is right, but this table writes FULL for Solitaire in the Observable row.

## 11. More environments (slides 51, 53)

### Q38 (Ch.2 deck, slide 51, question a)

Two rows of the table have the same entry in all six columns. Which ones? Which single column separates Poker from Backgammon?

![[cmpe421_lecture_02_ch_2_p051.jpg]]

- A) Taxi driving and Refinery controller. Only the Deterministic column separates Poker from Backgammon.
- B) Medical diagnosis and Refinery controller. Only the Deterministic column separates Poker from Backgammon.
- C) Taxi driving and Refinery controller. Only the Observable column separates Poker from Backgammon.
- D) Medical diagnosis and Refinery controller. Only the Observable column separates Poker from Backgammon.

> [!success]- Answer
> **D.** Medical diagnosis and Refinery controller: both Partially, Stochastic, Sequential, Dynamic, Continuous, Single. Poker is Partially observable (hidden hands), Backgammon Fully; all other columns agree.
> - A: Taxi driving is Multi, the refinery controller Single. And both games are Stochastic.
> - B: The pair is right, but both games are Stochastic (cards and dice).
> - C: The column is right, but Taxi driving differs from the refinery in the Agents column.

### Q39 (Ch.2 deck, slide 51, question b)

A robot plays table tennis against a human. It watches the ball with one camera, which cannot see the ball's spin. Where it stands after a stroke decides which balls it can reach next. The ball flies while the robot computes. Its designer has programmed in the rules of the game and the physics of the ball's flight. "Known" means the agent knows the rules of the world: the outcomes (or outcome probabilities) of its actions. In all options the task is stochastic, dynamic, continuous and multi-agent (competitive). Which option completes it correctly?

- A) Partially observable, sequential, known.
- B) Partially observable, episodic, unknown.
- C) Fully observable, sequential, unknown.
- D) Fully observable, episodic, known.

> [!success]- Answer
> **A.** The camera cannot see the spin (or the opponent's plan): partially observable. The robot's position after a stroke affects later strokes: sequential. The rules and the physics are given to the agent: known. What the opponent will do is a matter of the other agent, not of the rules of the world.
> - B: Each stroke affects which balls it can reach next, so it is not episodic. And the rules are known.
> - C: The camera sees the table but not the spin, which matters for the bounce. And the rules are known.
> - D: Fully observable and episodic are both wrong, as explained for B and C.

### Q40 (Ch.2 deck, slide 53)

The simplest environment is fully observable, deterministic, episodic, static, discrete and single-agent. From the table of slide 51: Crossword = Fully, Deterministic, Sequential, Static, Discrete, Single. Chess with a clock = Fully, Strategic, Sequential, Semi, Discrete, Multi. In how many of the six properties does each differ from the simplest environment?

- A) Crossword: 1. Chess with a clock: 4.
- B) Crossword: 1. Chess with a clock: 3.
- C) Crossword: 1. Chess with a clock: 2.
- D) Crossword: 5. Chess with a clock: 2.

> [!success]- Answer
> **A.** Crossword differs only in Sequential: 1. Chess with a clock differs in Strategic, Sequential, Semi and Multi: 4. Only Fully and Discrete stay simple.
> - B: 3 counts Semi as static. Semi is not static: the score changes with time.
> - C: 2 also counts Strategic as deterministic. Strategic is not fully deterministic: the opponent's moves are uncertain.
> - D: 5 and 2 count the properties that are the same, not the ones that differ.

## 12. Agent programs and the table-driven agent (slides 52-56)

### Q41 (Ch.2 deck, slide 52)

The reflex vacuum program (Suck if dirty, else Right in A and Left in B) is copied to a robot that has wheels and a suction motor but no dirt sensor. Does the agent work? What does this show about "agent = architecture + program"?

- A) It works. The program is the agent; the architecture only runs it, so any robot with wheels and suction will do.
- B) It works, only more slowly. The robot moves between the squares and sucks often enough to clean them by chance.
- C) It does not work. The program reads $[\mathit{location},\mathit{status}]$, and this architecture cannot deliver the status: program and architecture must fit.
- D) It does not work, because the program has a bug: it has no rule for NoOp. With a NoOp rule it would work on this robot.

> [!success]- Answer
> **C.** The program expects a percept with the dirt status, and this architecture cannot supply it. The architecture must deliver the percepts the program reads and carry out the actions it returns. A good program alone is not an agent.
> - A: The program needs its percepts from the architecture. Without the status it cannot choose.
> - B: The program has no rule to run without the status. It does not act at random.
> - D: A NoOp rule does not give the robot a dirt sensor. The missing percept is the problem.

### Q42 (Ch.2 deck, slide 54)

The skeleton says "Input = current percepts". The agent function depends on the whole percept sequence. How can a program that receives one percept per call still implement such a function?

- A) It cannot. A program can only implement agent functions that depend on the current percept alone.
- B) It keeps memory between calls: the earlier percepts or a summary of them (an internal state), combined with the new percept.
- C) The environment sends the whole percept sequence again at each call, so the program sees the history every time.
- D) It does not need to. The last percept always contains everything that matters from the history.

> [!success]- Answer
> **B.** The program stores the earlier percepts, or a summary (an internal state), between the calls. The function takes the whole history as its argument; the program gets one percept and must remember the rest itself.
> - A: The table-driven agent stores all percepts and so implements any agent function.
> - C: The input is only the current percept. Keeping the history is the program's job.
> - D: That is true only in a fully observable world. In general the history matters.

### Q43 (Ch.2 deck, slide 55)

TABLE-DRIVEN-AGENT appends each new percept to `percepts` and looks up `percepts` in a table. What is the key of the lookup after the third call? With $\lvert\mathcal P\rvert=5$ possible percepts and a lifetime of $T=4$ steps, how many rows does the table need, and how many for $T=5$?

- A) Key: the last percept only. $T=4$: 780 rows. $T=5$: 3905 rows.
- B) Key: the whole sequence of three percepts. $T=4$: 625 rows. $T=5$: 3125 rows.
- C) Key: the last percept only. $T=4$: 625 rows. $T=5$: 3125 rows.
- D) Key: the whole sequence of three percepts. $T=4$: 780 rows. $T=5$: 3905 rows.

> [!success]- Answer
> **D.** `percepts` holds the three percepts in order, and the whole sequence is the key.
> $T=4$: $5+25+125+625=780$. Check: $\dfrac{5\,(5^{4}-1)}{4}=780$. $T=5$ adds $5^{5}=3125$: $780+3125=3905$.
> - A: The key is the whole list `percepts`, not the last percept. A last-percept key would need only 5 rows.
> - B: 625 and 3125 count only sequences of length exactly $T$. The table also needs the shorter sequences.
> - C: Both parts are wrong, as explained for A and B.

### Q44 (Ch.2 deck, slide 56)

The four kinds of agent programs, in order, are: simple reflex, model-based reflex, goal-based, utility-based. Which is the first that can work in a partially observable world, and which is the first that can say that one way of reaching the goal is better than another?

- A) Partially observable: goal-based. Better way to the goal: utility-based.
- B) Partially observable: model-based reflex. Better way to the goal: goal-based.
- C) Partially observable: model-based reflex. Better way to the goal: utility-based.
- D) Partially observable: goal-based. Better way to the goal: goal-based.

> [!success]- Answer
> **C.** The model-based reflex agent adds an internal state and a model of the world, so it can track what it cannot see. The utility-based agent adds a number for how good a state is, so it can compare two ways of reaching the same goal.
> - A: The internal state already comes with the model-based reflex agent. Goals come one step later.
> - B: A goal is a yes/no test. Two ways that both reach the goal pass it equally.
> - D: Both parts are wrong, as explained for A and B.

## 13. Simple reflex agents (slides 57-60)

### Q45 (Ch.2 deck, slide 57)

Compare this simple reflex figure (slide 57, first image) with the model-based figure of slide 61 (second image). Which ovals of the model-based figure are missing here, and what does that mean for the box "What the world is like now"?

![[cmpe421_lecture_02_ch_2_p057.jpg]]

![[cmpe421_lecture_02_ch_2_p061.jpg]]

- A) Missing: "Goals" and "Utility". The box is filled from the stored percept history, but the agent cannot predict the future.
- B) Missing: "State", "How the world evolves", "What my actions do". The box is filled from the stored percept history, but the agent cannot predict the future.
- C) Missing: "Goals" and "Utility". The box is filled from the current percept alone: the agent has no memory and no model.
- D) Missing: "State", "How the world evolves", "What my actions do". The box is filled from the current percept alone: the agent has no memory and no model.

> [!success]- Answer
> **D.** The model-based figure has three more ovals: "State", "How the world evolves", "What my actions do". Without them the simple reflex agent has no memory and no model, so "What the world is like now" comes from the current percept alone.
> - A: The model-based figure has no "Goals" or "Utility" ovals either. And a simple reflex agent stores no history.
> - B: The ovals are right, but without "State" nothing is stored. There is no percept history.
> - C: The meaning is right, but "Goals" and "Utility" are not ovals of the model-based figure.

### Q46 (Ch.2 deck, slide 58)

A thermostat should switch the heater on below 20 °C, switch it off above 22 °C, and between 20 and 22 °C keep the heater as it is. It senses only the temperature. Which option gives the first two as condition-action rules and the reason why a simple reflex agent cannot do the third?

- A) "If temperature < 20 then heater on", "if temperature > 22 then heater off". Reason: the band 20-22 °C holds infinitely many values, too many for rules.
- B) "If temperature < 20 then heater on, else heater off". Reason: keeping the heater as it is needs the heater's present status, which is not in the percept.
- C) "If temperature < 20 then heater on, else heater off". Reason: the band 20-22 °C holds infinitely many values, too many for rules.
- D) "If temperature < 20 then heater on", "if temperature > 22 then heater off". Reason: keeping the heater as it is needs the heater's present status, which is not in the percept.

> [!success]- Answer
> **D.** Two rules: below 20 on, above 22 off. "Keep as it is" depends on whether the heater is on now. Only the temperature is in the percept, and a simple reflex agent does not remember what it did. It needs the heater status as a percept, or one bit of internal state (then it is model-based).
> - A: One rule "if $20\le T\le22$" covers the whole band. The problem is not the number of values, but the missing heater status.
> - B: "Else heater off" switches off at 20.5 °C. The task wants the heater kept as it is there.
> - C: Both parts are wrong, as explained for A and B.

### Q47 (Ch.2 deck, slide 59)

The vacuum world has 4 percepts. For a lifetime of $T=6$ steps, how many percept sequences of length 6 are there? The reflex agent needs 4 entries, one per percept. What does it give up for this reduction?

- A) $6^{4}=1296$ sequences. It ignores the history: two histories that end in the same percept get the same action.
- B) $4^{6}=4096$ sequences. It can no longer be rational for any performance measure, because it uses so few entries.
- C) $4^{6}=4096$ sequences. It ignores the history: two histories that end in the same percept get the same action.
- D) $6^{4}=1296$ sequences. It can no longer be rational for any performance measure, because it uses so few entries.

> [!success]- Answer
> **C.** Each of the 6 positions holds one of 4 percepts: $4^{6}=4096$, which is $4096/4=1024$ times more than 4 entries. The price is the history: for example the agent cannot stop when it has seen that both squares are clean.
> - A: $6^{4}$ swaps base and exponent. The base is the number of percepts, the exponent the length.
> - B: The reflex agent is rational for some measures (for example one point per clean square per step, with no cost for moves). It is not rational for all.
> - D: Both parts are wrong, as explained for A and B.

### Q48 (Ch.2 deck, slide 60)

A vacuum agent has no location sensor, so its percept is only Clean or Dirty. It starts in A. A is clean, B is dirty. On Clean it chooses Left or Right with probability 0.5 each (Left in A does nothing). What is the probability that it has not reached B at all after 3 actions?

- A) 0.5
- B) 0.875
- C) 0.25
- D) 0.125

> [!success]- Answer
> **D.** It stays in A only if it chooses Left three times: $0.5\times0.5\times0.5=0.125$. So it reaches B with probability $0.875$. With the fixed rule "on Clean go Left" it never reaches B: the infinite loop of the slide. The random choice breaks the loop without memory.
> - A: 0.5 is the probability for one action, not three.
> - B: 0.875 is the probability that it has reached B, the opposite event.
> - C: 0.25 is $0.5^{2}$, two actions instead of three.

## 14. Model-based reflex agents (slides 61-63)

### Q49 (Ch.2 deck, slide 61)

Three ovals feed the box "What the world is like now": "State", "How the world evolves", "What my actions do". Put each vacuum-world fact into its oval: (i) "B was clean when I left it", (ii) "a clean square stays clean", (iii) "Suck makes the current square clean".

![[cmpe421_lecture_02_ch_2_p061.jpg]]

- A) (i) State, (ii) What my actions do, (iii) How the world evolves.
- B) (i) How the world evolves, (ii) State, (iii) What my actions do.
- C) (i) State, (ii) How the world evolves, (iii) What my actions do.
- D) (i) What my actions do, (ii) How the world evolves, (iii) State.

> [!success]- Answer
> **C.** (i) is a belief about the present world, remembered from before: State. (ii) is how the world changes by itself: How the world evolves. (iii) is the effect of the agent's own action: What my actions do.
> - A: (ii) involves no action of the agent; (iii) does. They are swapped.
> - B: (i) is one remembered fact, not a general law. (ii) is a law about the world, not a belief.
> - D: (i) is a remembered fact, not the effect of an action. (iii) is a law about an action, not a belief.

### Q50 (Ch.2 deck, slide 62)

A model-based vacuum agent has the state (agent in A, A clean, B unknown). Its model says clean squares stay clean. Its rules: if the current square is dirty, Suck; if both squares are known clean, NoOp; else move to the other square. Its last action was Right; the new percept is $[B,\mathit{Dirty}]$. Then, after its next action, the percept is $[B,\mathit{Clean}]$. What are the state after the first update and the second action?

- A) State: (in B, A unknown, B dirty), action Suck. Second action: Left.
- B) State: (in B, A clean, B dirty), action Suck. Second action: Left.
- C) State: (in B, A unknown, B dirty), action Suck. Second action: NoOp.
- D) State: (in B, A clean, B dirty), action Suck. Second action: NoOp.

> [!success]- Answer
> **D.** UPDATE-STATE keeps A clean, because the model says clean squares stay clean. B is dirty, so Suck. After Suck the percept $[B,\mathit{Clean}]$ gives (in B, A clean, B clean). Both are known clean: NoOp.
> - A: The agent knew A was clean, and the model keeps it clean. A simple reflex agent would forget it and go Left.
> - B: The state is right, but then both squares are known clean, so the rule NoOp matches, not "move".
> - C: With A unknown the agent cannot know both squares are clean, so NoOp would not match.

### Q51 (Ch.2 deck, slide 63)

Sort into "how the world evolves independently of the agent" or "how the agent's own actions affect the world": (a) an overtaking car is closer now than one second ago, (b) turning the steering wheel clockwise turns the car to the right, (c) a traffic light goes from green to yellow to red, (d) after 5 minutes of driving north at 60 miles per hour the car is 5 miles further north. (b) is an action effect and (c) is world evolution. What are (a) and (d)?

- A) (a) effect of my action. (d) effect of my action.
- B) (a) world evolves by itself. (d) world evolves by itself.
- C) (a) world evolves by itself. (d) effect of my action.
- D) (a) effect of my action. (d) world evolves by itself.

> [!success]- Answer
> **C.** (a) The other car moves by its driver's choice, not by mine: the world evolves. (d) My own driving moves my car: $60\ \text{miles/hour}\times\dfrac{5}{60}\ \text{hour}=5\ \text{miles}$, an effect of my action.
> - A: The overtaking car comes closer because of its own speed. That is not my action.
> - B: My car moving north is the result of my driving, so (d) is an action effect.
> - D: Both are swapped, as explained for A and B.

## 15. Goal-based and utility-based agents (slides 64-67)

### Q52 (Ch.2 deck, slide 64)

Compared with the model-based figure of slide 61, this figure has the new box "What it will be like if I do action A", and "Goals" has replaced "Condition-action rules". How does the agent use the new box and "Goals" to choose its action?

![[cmpe421_lecture_02_ch_2_p064.jpg]]

- A) For each possible action it predicts the next state with its model and picks an action whose predicted state satisfies the goal.
- B) For each possible action it predicts the next state with its model and picks the action whose predicted state gets the highest number.
- C) It looks up the current state in "Goals", which lists the right action for each state, just like condition-action rules.
- D) It predicts its next percept and compares it with the real next percept, so that it can improve its rules.

> [!success]- Answer
> **A.** The new box uses "How the world evolves" and "What my actions do" to predict the result of an action. "Goals" says which results are wanted. So the agent looks ahead and picks an action that leads to the goal. A reflex agent only reacts to the present situation.
> - B: A number for how good a state is, is a utility. That is the utility-based agent of the next slides. A goal is only a yes/no test.
> - C: A goal describes a wanted situation, not an action. Matching the state to an action is what the condition-action rules did.
> - D: Comparing predictions with results to improve the rules is learning (critic and learning element), not the goal-based agent.

### Q53 (Ch.2 deck, slide 65)

Vacuum world, goal "both squares clean". The agent is in A, A is clean, B is dirty. The shortest sequence that reaches the goal is Right, Suck. How many action sequences of length 2 exist with the 4 actions (Left, Right, Suck, NoOp), and how many of them end with both squares clean?

- A) 16 sequences. 2 reach the goal: (Right, Suck) and (Suck, Right).
- B) 16 sequences. 1 reaches the goal: (Right, Suck).
- C) 8 sequences. 1 reaches the goal: (Right, Suck).
- D) 8 sequences. 2 reach the goal: (Right, Suck) and (Suck, Right).

> [!success]- Answer
> **B.** Each of the 2 steps has 4 choices: $4\times4=16$. The agent must first be in B and then suck, so only (Right, Suck) works.
> - A: (Suck, Right) sucks in A, which is already clean, and then moves to the dirty square. Order matters.
> - C: 8 is $4+4$. The choices multiply: $4^{2}=16$.
> - D: Both parts are wrong, as explained for A and C.

### Q54 (Ch.2 deck, slide 66)

An agent can try for goal $G_1$ (worth 10, success probability 0.3) or for goal $G_2$ (worth 4, success probability 0.9). Failure is worth 0. What are the expected utilities, and which goal does a utility-based agent choose?

- A) $\mathrm{EU}(G_1)=3$, $\mathrm{EU}(G_2)=3.6$. It chooses $G_2$.
- B) $\mathrm{EU}(G_1)=10$, $\mathrm{EU}(G_2)=4$. It chooses $G_1$.
- C) $\mathrm{EU}(G_1)=0.3$, $\mathrm{EU}(G_2)=0.9$. It chooses $G_2$.
- D) $\mathrm{EU}(G_1)=7$, $\mathrm{EU}(G_2)=0.4$. It chooses $G_1$.

> [!success]- Answer
> **A.** $\mathrm{EU}(G_1)=0.3\times10+0.7\times0=3$. $\mathrm{EU}(G_2)=0.9\times4+0.1\times0=3.6$. $3.6>3$, so $G_2$: the larger prize is too unlikely.
> - B: 10 and 4 are the values if the goal is sure. The probabilities are ignored.
> - C: 0.3 and 0.9 are only the success probabilities. The values are ignored.
> - D: 7 and 0.4 multiply the values by the failure probabilities (0.7 and 0.1).

### Q55 (Ch.2 deck, slide 67)

Two routes reach the destination. Route A: 20 minutes, accident probability 0.01. Route B: 25 minutes, accident probability 0.001. Use $U=-\text{minutes}-1000$ for a trip with an accident and $U=-\text{minutes}$ otherwise. Which route is chosen? Which if the accident penalty is 100 instead of 1000? (A goal-based agent cannot decide: both routes reach the goal.)

- A) Penalty 1000: route A. Penalty 100: route A.
- B) Penalty 1000: route B. Penalty 100: route B.
- C) Penalty 1000: route B. Penalty 100: route A.
- D) Penalty 1000: route A. Penalty 100: route B.

> [!success]- Answer
> **C.** Penalty 1000: $\mathrm{EU}_A=-20-0.01\times1000=-30$, $\mathrm{EU}_B=-25-0.001\times1000=-26$. Route B.
> Penalty 100: $\mathrm{EU}_A=-20-0.01\times100=-21$, $\mathrm{EU}_B=-25-0.001\times100=-25.1$. Route A.
> - A: With penalty 1000 the accident risk of A costs 10 expected points, more than the 5 minutes saved.
> - B: With penalty 100 the extra risk of A costs only 0.9 expected points, less than the 5 minutes saved.
> - D: Both choices are swapped.

## 16. Learning agents (slides 68-70)

### Q56 (Ch.2 deck, slide 68)

A learning taxi brakes too late. Follow the arrows of the figure: which path does this experience take until the driving program (the performance element) is changed?

![[cmpe421_lecture_02_ch_2_p068.jpg]]

- A) Sensors, Performance element, "knowledge", Learning element, "changes", Performance element. The critic is not needed.
- B) Performance standard, Performance element. The standard changes the driving program directly.
- C) Sensors, Critic, "feedback", Learning element, "changes", Performance element.
- D) Sensors, Critic, "learning goals", Problem generator, Performance element.

> [!success]- Answer
> **C.** The critic compares the percepts with the performance standard and sends "feedback" to the learning element. The learning element sends "changes" to the performance element.
> - A: The performance element does not judge itself. Without the critic nobody says the braking was bad.
> - B: The standard goes into the critic, not into the performance element.
> - D: "Learning goals" go from the learning element to the problem generator. The problem generator suggests new experiences; it does not fix the braking.

### Q57 (Ch.2 deck, slide 69)

A learning vacuum robot has four modules: (a) it chooses Suck, Left or Right from the percept, (b) it reports "the floor was dirty in 12 of the last 20 steps, the target is at most 4", (c) it changes the rules so that the robot visits the kitchen more often, (d) it proposes to try the unknown route under the sofa. In all options (a) is the performance element and (d) the problem generator. Name (b) and (c), and the module that stands for exploration.

- A) (b) critic, (c) learning element. Exploration: (d).
- B) (b) learning element, (c) critic. Exploration: (d).
- C) (b) critic, (c) learning element. Exploration: (c).
- D) (b) learning element, (c) critic. Exploration: (c).

> [!success]- Answer
> **A.** (b) compares what happened with the fixed standard: critic. (c) changes the performance element: learning element. (d) suggests a new experience that may be worse today but teaches something: exploration.
> - B: Judging against a target is the critic's job. Changing the rules is the learning element's job.
> - C: (c) changes the rules toward what already looks good. That is improvement, not trying the unknown.
> - D: Both parts are wrong, as explained for B and C.

### Q58 (Ch.2 deck, slide 70)

The critic judges the agent against a performance standard that is fixed and comes from outside. What could go wrong if the learning agent were allowed to change this standard itself?

- A) Nothing. A learning agent should set its own standard, so that it can adapt to new environments.
- B) The agent would learn too slowly, because the critic would have to compute a new standard in every step.
- C) The agent could lower the standard until its present behaviour counts as good. Then the feedback says nothing about the world.
- D) The agent would become stochastic, because its standard would change at random from one step to the next.

> [!success]- Answer
> **C.** The easy way to a good judgement is to lower the bar (for example, declare dirty floors fine). Then the learning element stops improving the real behaviour. A fixed outside standard keeps "better" tied to what the designer wants.
> - A: Adapting is the job of the learning element. The standard must stay fixed so that "better" keeps its meaning.
> - B: Speed is not the issue. The issue is that the agent can cheat its own judgement.
> - D: The agent would not change the standard at random. It would change it in its own favour.

## 17. From the handout `intelligent-agents.pdf` (handout pages 5, 9, 16, 20)

### Q59 (handout, slide 5)

A is dirty, B is clean, the agent is in A, and $T=6$ steps. Agent X does Suck and then NoOp five times. Agent Y has an extra action Dump (it makes the current square dirty again) and does Suck, Dump, Suck, Dump, Suck, Dump. Measure 1: one point per square cleaned up. Measure 1 gives X 1 and Y 3. Measure 2: one point per clean square per step, minus one per move, counted after each action. What does measure 2 give, and which measure is better?

- A) Measure 2: X 11, Y 3. Measure 2 is better, because it scores the clean floor.
- B) Measure 2: X 12, Y 9. Measure 1 is better, because it rewards the agent that cleans more.
- C) Measure 2: X 12, Y 9. Measure 2 is better, because it scores the clean floor.
- D) Measure 2: X 11, Y 3. Measure 1 is better, because it rewards the agent that cleans more.

> [!success]- Answer
> **C.** Nobody moves, so there is no move cost. X has 2 clean squares after every step: $6\times2=12$. Y has 2, 1, 2, 1, 2, 1: $9$. Measure 2 scores the state of the floor, so making new dirt does not pay.
> - A: 11 and 3 subtract a point for every Suck or Dump. Only moves (Left, Right) cost a point.
> - B: Under measure 1 the useless agent Y wins by dumping and cleaning again. That is the flaw of measure 1.
> - D: Both parts are wrong, as explained for A and B.

### Q60 (handout, slide 9)

The only sensor of the shopping agent is "HTML pages". Its P line is price, quality, appropriateness, efficiency; its E line is WWW sites, vendors, shippers. Name two things that matter for the score but are not directly in the percepts, and what this says about observability.

- A) The price and the product description: they are only text on a page. The environment is fully observable, because any page can be loaded.
- B) The real quality of the product and the reliability of the shipper. The environment is partially observable.
- C) The real quality of the product and the reliability of the shipper. The environment is fully observable, because any page can be loaded.
- D) The price and the product description: they are only text on a page. The environment is partially observable.

> [!success]- Answer
> **B.** The page shows the vendor's description, not the real quality, and nothing shows how reliable the shipper is. The relevant state is larger than what the sensors deliver: partially observable, as in the "No" of the handout's table.
> - A: Price and description are on the page, so they are in the percept. And loading pages does not show the hidden facts.
> - C: The two things are right, but they stay hidden however many pages the agent loads.
> - D: The conclusion is right, but price and description are visible on the page.

### Q61 (handout, slide 16)

"The environment type largely determines the agent design." The Taxi column has "No" in all six rows. For example, not fully observable means the taxi needs an internal state and a model. What do "not deterministic" and "not episodic (sequential)" require?

- A) Not deterministic: reasoning with probabilities and expected utility. Sequential: thinking ahead, so search or planning.
- B) Not deterministic: an internal state and a model of the world. Sequential: thinking ahead, so search or planning.
- C) Not deterministic: reasoning with probabilities and expected utility. Sequential: fast decisions, because the world does not wait.
- D) Not deterministic: an internal state and a model of the world. Sequential: fast decisions, because the world does not wait.

> [!success]- Answer
> **A.** Uncertain results call for probabilities and expected utility. When today's action affects later ones, the agent must look ahead: search or planning.
> - B: The internal state answers partial observability, not randomness of the results.
> - C: Fast decisions answer a dynamic world. Sequential asks for looking ahead.
> - D: Both parts are wrong, as explained for B and C.

### Q62 (handout, slide 20)

The agent senses only the dirt status and remembers `location`, initially A. It does Suck on Dirty; on Clean it goes Right if it believes it is in A, Left if it believes it is in B. In truth it starts in B. A is dirty, B is clean. Right in B does nothing. Trace 4 steps. Which action is wasted, and from when is the belief correct?

- A) Step 1 (Right in B) is wasted. The belief is correct from step 1 on, because the wall stops the move.
- B) Step 2 (Left from B to A) is wasted. The belief is correct only after step 3, when the agent finds dirt in A.
- C) Step 1 (Right in B) is wasted. The belief is correct only after step 3, when the agent finds dirt in A.
- D) Step 2 (Left from B to A) is wasted. The belief is correct from step 1 on, because the wall stops the move.

> [!success]- Answer
> **A.** Step 1: Clean, Right; believed B, true B (wall). Step 2: Clean, Left; A and A. Step 3: Dirty, Suck; A and A. Step 4: Clean, Right; B and B.
> Step 1 does nothing, because the agent is already in B. After it, belief and truth agree only because the wall stopped the move.
> - B: Step 2 brings the agent to the dirty square, so it is useful. And belief and truth agree already after step 1.
> - C: The wasted step is right, but the belief is already correct after step 1. Finding dirt does not tell the agent where it is.
> - D: The belief part is right, but step 2 is the useful move to A.
