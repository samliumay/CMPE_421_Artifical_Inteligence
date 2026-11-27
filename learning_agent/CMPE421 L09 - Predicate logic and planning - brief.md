# Learning brief: CMPE421 L09 - Predicate logic and planning

This file is for the LearnHarness teacher agent. Read it first, then read the files it lists.

## Goal

Teach me this lecture of CMPE 421 Artificial Intelligence so that I understand every slide and can answer its questions without help.
Exams ask to trace algorithms by hand (search, CSP, minimax, logic inference, probability) and to explain why an algorithm behaves as it does.

## Sources, in priority order

1. **Lesson note** (my notes, slide by slide, with 94 check questions and their answers): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_09/CMPE421 L09 - Predicate logic and planning.md`
2. **Quiz note** (94 multiple-choice questions, one per check question, each with a folded answer that explains every option): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/lessons/lecture_09/CMPE421 L09 - Predicate logic and planning - quiz.md`
3. **Slide images** (one jpg per slide; the lesson note embeds them as `![[<deck>_pNNN.jpg]]`): `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_07_deterministicplanning`, `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_07_predicatelogic`, `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/CMPE_421_Artificial_Intelligence/others/slides/cmpe421_lecture_8_inference_predicatelogic`
4. **Original slides (PDF):** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/PredicateLogic.pdf`
5. **Full step-by-step notes:** `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf` (LaTeX source: `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/lecture_09.tex`)

The slides are the authority for what the course teaches. Text marked "(beyond the slides)" in the lesson note is extra explanation: use it to explain, do not present it as exam material.

## What the lecture covers

- 1. What FOL talks about (slides 1-2)
- 2. Syntax (slides 3-5)
- 3. Truth: worlds and models (slides 6-8)
- 4. Quantifiers (slides 9-11)
- 5. Translating English, equality (slides 12-13)
- 6. Substitutions, numbers, sets, lists (slides 14-16)
- 7. Using a FOL knowledge base (slides 17-20)
- 8. Blocks world and states (slides 1-5)
- 9. Operators and actions (slides 6-11)
- 10. Plans, problems, complexity (slides 12-16)
- 11. Forward search (slide 17)
- 12. Graphplan and the planning graph (slides 18-32)
- 13. Heuristics and FastForward (slides 33-38)
- 14. Instantiation and propositionalization (inference deck, slides 1-8)
- 15. Unification and Generalized Modus Ponens (inference deck, slides 9-20)
- 16. Forward chaining in FOL (inference deck, slides 21-31)
- 17. Backward chaining and Prolog (inference deck, slides 32-44)
- 18. Resolution in FOL (inference deck, slides 45-48)

## Known errors and traps in the sources (do not teach the errors)

- **Watch out:** A predicate may contain terms, but a term never contains a predicate. $\mathit{LeftLegOf}(\mathit{Brother}(\mathit{Richard},\mathit{John}))$ is not allowed.
- **Watch out:** $\forall$ goes with $\Rightarrow$, $\exists$ goes with $\land$. $\forall x\ \mathit{At}(x,\mathit{UMD})\land\mathit{Smart}(x)$ says everything is at UMD and smart (too strong). $\exists x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$ is true as soo...
- **Errors on the slides:** - Slide 19: the brackets of the adjacency axiom do not match, and the pit axiom is printed without $\exists r$. - Slide 17 asks for $\mathit{Action}(a,5)$, but the rules on slide 18 conclude $\mathit{BestAction}(\mathit{Grab},t)$.
- **Watch out:** - Test executability first, then delete, then add. - Slide 11 prints the result with an extra `clear(b)` that is missing from the state printed above it. The state has lost `clear(b)` (slides 13 and 15 list it).
- **Watch out:** - Slide 30 says "twelve combinations at level 4". The graph has only levels 0 to 2, and the combinations are at level 2. - Parallel length is not the number of actions: the plan above has 2 levels but 3 actions.
- **Slide error (slide 20):** The second step of the proof says "Since $p_i\theta=p_i\theta$". It must be $p_i'\theta=p_i\theta$: the fact $p_i'$ and the premise $p_i$ become the same after the substitution. This is the condition of the theorem.
- **Slide errors (slides 47 and 48):** Slide 47, step 6 is titled "Distribute $\land$ over $\lor$". The step that is done distributes $\lor$ over $\land$: $(a\land b)\lor c$ becomes $(a\lor c)\land(b\lor c)$. The result on the slide is correct. Slide 48: the last green box on the right shows $\math...

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
