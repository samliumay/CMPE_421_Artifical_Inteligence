# Learning brief: CMPE421 L07 - MDPs, probability, Bayes nets, HMMs

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 28 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_07/CMPE421 L07 - MDPs, probability, Bayes nets, HMMs.md`
2. **Quiz note:** not written yet for this lesson. Use the check questions of the lesson note.
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): see `others/slides/` in the vault
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/Ch.7.pptx`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_07/CMPE421_Lecture_07_MDPs_Probability_BayesNets_HMMs.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_07/lecture_07.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. Distributions, joints, events, marginals (Ch.9, slides 1-15)
- 2. Conditional probability and normalisation (Ch.9, slides 16-23, 38-39)
- 3. Inference by enumeration (Ch.9, slides 24-27)
- 4. Product rule, chain rule, Bayes' rule (Ch.9, slides 28-37)
- 5. Independence and conditional independence (Ch.10.0, slides 1-14)
- 6. Bayes' net semantics (Ch.10.0, slides 15-30)
- 7. Causality (Ch.10.0, slides 31-34)
- 8. MDPs and policies (Ch.7, slides 1-16)
- 9. Discounting (Ch.7, slides 17-27)
- 10. Bellman equations and value iteration (Ch.7, slides 28-70)
- 11. Policy evaluation, extraction, policy iteration (Ch.7, slides 71-107)
- 12. Markov chains (Ch.11.0, slides 1-17)
- 13. HMMs and filtering (Ch.11.0, slides 18-40)
- 14. Forward algorithm and Viterbi (Ch.11.0, slides 41-56)

## Known errors and traps in the sources (do not teach the errors)

- **Watch out:** For OR, list each row once. $P(\text{hot})+P(\text{sun})=1.1$ counts (hot, sun) twice.
- **Watch out:** $P(a\mid b)\ne P(b\mid a)$ in general. A conditional table $P(D\mid W)$ sums to 1 for each value of $W$, not over all its rows.
- **Watch out:** Normalise once, at the end, over the query variable. Never normalise slices separately before summing.
- **Errors on the slides:** - Slide 33 uses $P(-m)=0.999$. The correct value is $0.9999$. The answer still rounds to $0.0079$. - Slides 29, 34 and 39 head a weather column "R". It means $W$.
- **Watch out:** - To prove independence check every row. One failing row disproves it. Equal marginals prove nothing. - Conditional independence does not imply independence. Ghostbusters (slide 14): $T\perp B\mid G$, yet $P(+t,+b)=0.32\ne 0.36=P(+t)P(+b)$.
- **Error on slide 30:** The slide writes only $P(M\mid A)P(J\mid A)P(A\mid B,E)$. The joint also needs the root factors $P(B)P(E)$.
- **Watch out:** Use the old vector $V_k$ on the right-hand side for every state. Terminal states keep value 0.
- **Error on slide 102:** The slide writes the slow q-value in warm as 3. It is $0.5\cdot 2+0.5\cdot 2=2$. The chosen action (slow) is unchanged.
- **Two weather chains in the deck:** Slides 8-14 use 0.9 / 0.3 and start at $X_1$. Slides 11 and 41-46 use sun to sun 0.6, rain to sun 0.1 and start at $W_0$. Check which one a question uses.
- **Watch out:** Evidence variables are not independent of each other. They are only conditionally independent given the hidden states (slide 27 quiz).
- **Watch out:** - The most likely sequence is not the sequence of the most likely single states. - Slide 45's last table is headed $B'(W_1)$ but holds $B(W_1)=\langle 8/11,3/11\rangle$.

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
