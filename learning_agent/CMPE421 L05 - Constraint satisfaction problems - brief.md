# Learning brief: CMPE421 L05 - Constraint satisfaction problems

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 32 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_05/CMPE421 L05 - Constraint satisfaction problems.md`
2. **Quiz note:** not written yet for this lesson. Use the check questions of the lesson note.
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_05_csp`
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_05/csp.pdf`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_05/CMPE421_Lecture_05_CSP.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_05/lecture_05.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. What is a CSP (slides 1-4)
- 2. Varieties of CSPs and constraints (slides 5-8)
- 3. Standard search formulation (slide 9)
- 4. Backtracking search (slides 10-15)
- 5. Ordering: MRV, degree, LCV (slides 16-18)
- 6. Forward checking (slides 19-23)
- 7. Arc consistency and AC-3 (slides 24-28)
- 8. Independent subproblems (slides 29-31)
- 9. Tree-structured CSPs (slides 32-33)
- 10. Nearly tree-structured CSPs (slide 34)
- 11. Iterative improvement: min-conflicts (slides 35-37)
- 12. Only in the instructor's decks (no slide images)

## Known errors and traps in the sources (do not teach the errors)

- **Source issue (slide 7):** The handout's solution $469+469=0938$ has $F=0$, a leading zero. With the usual rule $F\neq0$, one solution is $734+734=1468$.
- **Source error (slide 9):** The slide writes "for $i=1,\dots,n$, $b_i=(n-i)d$, so $b_0b_1\dots b_n=n!\,d^{n}$". The indices do not match: $b_n=0$. Correct: $i$ runs from $0$ to $n-1$ and $b_0b_1\cdots b_{n-1}=n!\,d^{n}$.
- **Watch out:** Heuristics are not guarantees. In our measurements 16 queens needed 44 assignments with FC + MRV but 332 with FC + MRV + LCV.
- **Watch out:** If AC empties a domain there is no solution. If AC empties no domain there may still be no solution. Example: a triangle of three mutually different variables with two colours is arc consistent and unsolvable. So arc consistency runs *inside* backtracking.
- **Watch out:** The backward pass must go from the leaves up. Going from $j=2$ upward, a later step can still delete values of $X_j$ and break an arc that was already checked.
- **Watch out:** Min-conflicts is incomplete. It can get stuck on a plateau or in a local minimum, and it can never prove that a CSP has no solution.

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
