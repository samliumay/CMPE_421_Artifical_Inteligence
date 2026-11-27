# Learning brief: CMPE421 L02 - Intelligent agents

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 62 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_02/CMPE421 L02 - Intelligent agents.md`
2. **Quiz note** (62 multiple-choice questions, one per check question, each with a folded answer that explains every option): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_02/CMPE421 L02 - Intelligent agents - quiz.md`
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_02_ch_2`, `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_02_intelligent_agents`
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_02/Ch.2.pdf`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_02/CMPE421_Lecture_02_Intelligent_Agents.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_02/lecture_02.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. Agents and rational agents (slides 2-3)
- 2. Agents in action (slides 5-17)
- 3. Agent function and agent program (slides 18-21)
- 4. The vacuum-cleaner world (slides 22-24)
- 5. Rationality (slides 25-28)
- 6. PEAS (slides 29-30)
- 7. Fully or partially observable (slides 31-34)
- 8. Deterministic, stochastic, strategic (slides 35-37)
- 9. Episodic or sequential (slides 38-41)
- 10. Static, discrete, single-agent (slides 42-50)
- 11. More environments (slides 51, 53)
- 12. Agent programs and the table-driven agent (slides 52-56)
- 13. Simple reflex agents (slides 57-60)
- 14. Model-based reflex agents (slides 61-63)
- 15. Goal-based and utility-based agents (slides 64-67)
- 16. Learning agents (slides 68-70)
- 17. From the handout `intelligent-agents.pdf` (handout pages 5, 9, 16, 20)

## Known errors and traps in the sources (do not teach the errors)

- **Watch out:** P is the designer's score, computed from the world from outside. S is what the agent can read. "Passenger comfort" is in P. The taxi senses it only indirectly.
- **Watch out:** A partially observable environment can *appear* stochastic. Unpredictable results do not prove the world is stochastic. It may be deterministic with hidden parts.
- **Watch out:** Not every moving object is an agent. A falling leaf is part of the environment. Another car is an agent because its driver's performance depends on what you do.
- **Slide slips:** Slide 59 writes "4T" for $4^{T}$. Slide 60 writes `RULE-MATCH(state, rule)`. The second argument is the rule set `rules`.
- **Watch out:** The performance standard is outside the agent and fixed. The critic judges, the learning element changes the program. A learning agent is not a fifth kind: any of the four kinds can be made to learn. Slide 70 starts with a sentence left over from slide 67 ("Go...
- **The two decks disagree on the environment table:** | | Deck | Handout | |---|---|---| | Solitaire, observable | Full | No | | Solitaire, deterministic | Yes | Yes, after the cards are dealt | | Internet shopping, deterministic | Yes | Partly | | Internet shopping, static | Semi | Partly | | Backgammon, static ...
- **Handout slip:** The handout's `Vacuum-Agent-With-State([location])` tests `status`. The real percept is the dirt status. `location` is the agent's memory.

## How to run a session

1. Ask which part I want, or start from section 1. Ask two or three short questions to see what I already know.
2. Go section by section. Explain one slide or one idea, then ask the **check question** of that slide from the lesson note (the callouts `> [!question]- Check (slide N): ...`). Do not show the answer. Wait for mine, then compare it with the answer in the note and tell me what is missing.
3. At the end of a section, or in Practice mode, ask the **multiple-choice questions** of that section from the quiz note. Show the four options, wait for my letter, then explain why the right option is right and why my choice was wrong, using the answer callout.
4. Make new questions of the same kind (new numbers, a changed condition) for the points I miss.
5. Finish with a short list: what I know, what I should repeat, and which slides to read again.

## About me

MS student at TED University (CS/CMPE). I study alone, often on a phone. English is not my first language.

- Probe my starting point first.
- One idea at a time, short explanations, a check question after each step. Never skip steps.
- Give the Turkish equivalent of a key term at its first use.
- Ask me to think out loud. When I am wrong, show me which step went wrong.
- For graded homework or labs: hints and explanations only, never the submission.
