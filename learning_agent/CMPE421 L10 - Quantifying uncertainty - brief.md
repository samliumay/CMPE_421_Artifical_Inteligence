# Learning brief: CMPE421 L10 - Quantifying uncertainty

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 39 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_10/CMPE421 L10 - Quantifying uncertainty.md`
2. **Quiz note** (39 multiple-choice questions, one per check question, each with a folded answer that explains every option): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_10/CMPE421 L10 - Quantifying uncertainty - quiz.md`
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_07_quantifyinguncertainty`
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/QuantifyingUncertainty.pdf`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_10/CMPE421_Lecture_10_Quantifying_Uncertainty.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_10/lecture_10.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. Acting under uncertainty (slides 2-6)
- 2. Probability basics (slides 7-9)
- 3. Propositions and distributions (slides 10-13)
- 4. Conditional probability, product rule, chain rule (slides 14-16)
- 5. Inference by enumeration (slides 17-21)
- 6. Independence and conditional independence (slides 22-24)
- 7. Bayes' rule and naive Bayes (slides 25-26)
- 8. Wumpus world: probabilistic reasoning (slides 27-45)
- 9. Summary (slide 46)

## Known errors and traps in the sources (do not teach the errors)

- **Error on slides 8 and 10:** The slides define $\mathit{Odd}(\omega)=\mathit{true}$ "if $\omega$ is even". Odd is meant. Slide 10 then prints $P(\mathit{odd})=\tfrac16$ and $P(\neg\mathit{odd})=\tfrac56$. The correct values are $\tfrac12$ and $\tfrac12$.
- **Watch out:** - A density is not a probability. A single point has probability 0, and a density value can exceed 1. - Slide 11 says $P(\mathit{cavity})=0.1$ as a notation example. Every table in this lecture gives $P(\mathit{cavity})=0.2$.
- **Watch out:** - $P(\mathit{cavity}\mid\mathit{toothache})$ is a belief given that toothache is all I know. It is not an if-then rule. - The $0.8$ on slide 14 is an illustration. The dentist table gives $P(\mathit{cavity}\mid\mathit{toothache})=0.6$.
- **Watch out:** Independent and conditionally independent are different, and neither implies the other. Toothache and catch are conditionally independent given cavity, but not independent.
- **Watch out:** Lecture 7 solves meningitis with other data ($P(+s\mid -m)=0.01$ given instead of $P(s)$) and gets about $0.0079$. Both answers are correct for their own data. Check whether $P(\text{effect})$ is given or must be built by total probability.
- **Watch out:** $0.31$, $0.86$, $0.31$ belong to three different variables. They need not sum to 1.

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
