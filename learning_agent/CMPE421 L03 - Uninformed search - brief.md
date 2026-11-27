# Learning brief: CMPE421 L03 - Uninformed search

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 45 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_03/CMPE421 L03 - Uninformed search.md`
2. **Quiz note** (45 multiple-choice questions, one per check question, each with a folded answer that explains every option): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_03/CMPE421 L03 - Uninformed search - quiz.md`
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_03_ch_3`, `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_03_uninformed_search`
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_03/Ch.3.pdf`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_03/CMPE421_Lecture_03_Uninformed_Search.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_03/lecture_03.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. Reflex agents and planning agents (slides 2-10)
- 2. Search problems (slides 12-14)
- 3. What is in a state space (slides 15-17)
- 4. State space graphs and search trees (slides 19-24)
- 5. Tree search (slides 26-30)
- 6. Depth-first search (slides 32-35)
- 7. Breadth-first search (slides 37-40)
- 8. Iterative deepening (slide 43)
- 9. Uniform-cost search (slides 44-48)
- 10. The one queue (slide 53)
- 11. Search and models (slides 54-55)
- 12. Handout: problem types (handout pages 2-5, 10)
- 13. Handout: nodes, eager and cautious goal test (handout pages 17, 19)
- 14. Handout: depth-limited search and IDS (handout pages 57, 58, 67)
- 15. Handout: summary, repeated states, graph search (handout pages 30, 68-70)

## Known errors and traps in the sources (do not teach the errors)

- **Watch out:** Having a model does not make an agent a planning agent. A model-based reflex agent models the *current* state. Only a planning agent uses the model to predict *future* states.
- **Slide slip:** In the PDF export the exponents are flattened: "230" means $2^{30}$ and "122" means $12^{2}$.
- **Watch out:** A finite graph does not give a finite tree. One cycle is enough to make the tree infinite.
- **Watch out:** Do not mix up $b^{m}$ and $bm$. For $b=10$, $m=5$ they are $100\,000$ and $50$. In the PDF export "O(bm)" stands for both: time is $b^{m}$, space is $b\cdot m$. On the tiny graph DFS returns S-d-e-r-f-G (cost 10) with the leftmost-first rule. With the opposite...
- **Slide error:** Slide 44 labels the arc r→f with cost 2. Slide 46 labels the same arc with 1, and the numbers in its tree (r 7, f 8, G 10) use 1. This note uses 1 everywhere. With cost 2 the paths would cost 11 (S-d-e-r-f-G) and 15 (S-e-r-f-G). The order of the two paths stay...
- **Source error (handout page 30):** The handout writes the BFS time as $1+b+\dots+b^{d}+b(b^{d}-1)=O(b^{d})$. The last term is $b^{d+1}-b$, so the cautious count is $O(b^{d+1})$. $O(b^{d})$ is right for eager BFS. For $b=10$, $d=5$: eager about $111\,111$ nodes, cautious up to $1\,111\,101$. The...

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
