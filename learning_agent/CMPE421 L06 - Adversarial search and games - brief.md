# Learning brief: CMPE421 L06 - Adversarial search and games

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 28 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_06/CMPE421 L06 - Adversarial search and games.md`
2. **Quiz note:** not written yet for this lesson. Use the check questions of the lesson note.
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): see `others/slides/` in the vault
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6.pptx`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_06/CMPE421_Lecture_06_Adversarial_Search.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_06/lecture_06.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. Games (Ch.6 deck, slides 1-11)
- 2. Minimax (Ch.6 deck, slides 12-23)
- 3. Properties of minimax (Ch.6 deck, slides 24-28)
- 4. Alpha-beta pruning (Ch.6 deck, slides 29-36)
- 5. Resource limits (Ch.6 deck, slides 37-51)
- 6. Several players (Ch.6-contd deck, slides 31-32)
- 7. Expectimax (Ch.6-contd deck, slides 33-42)
- 8. Probabilities and modelling assumptions (Ch.6-contd deck, slides 43-56, 76-77)
- 9. Expectiminimax (Ch.6-contd deck, slides 57-58)
- 10. Utilities (Ch.6-contd deck, slides 59-74)

## Known errors and traps in the sources (do not teach the errors)

- **Watch out:** - Slide 9 still says the best humans beat the best Go programs. Slide 10 corrects it: AlphaGo won in 2016. - A **ply** is one move by one player. An everyday chess "move" (White then Black) is 2 ply.
- **Watch out:** $b^m$ is a power (enormous). $bm$ is a product (tiny). Space is small because depth-first search keeps only one path plus the waiting siblings.
- **Watch out:** A child of the root can report a wrong value. Tree $[[10],[10,0]]$: the right child returns 10 after its first leaf ($10\le\alpha=10$), but its true value is 0. Remember which child set $\alpha$ and replace the best move only on a strictly larger value.
- **Error on slide 58:** The slide writes $20\times(21\times 20)^3=1.2\times 10^9$. The product is $20\times 420^3=1\,481\,760\,000\approx 1.5\times 10^9$.
- **Watch out:** An agent can be rational (consistent with MEU) without ever storing utilities or probabilities, for example a lookup table for perfect tic-tac-toe. MEU describes behaviour, not the inside of the program.

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
