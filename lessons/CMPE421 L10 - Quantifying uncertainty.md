---
course: CMPE 421 Artificial Intelligence
lecture: 10
title: Quantifying uncertainty
tags: [cmpe421, lecture]
slides: lectures/lecture_07/QuantifyingUncertainty.pdf
full_notes: lecture_notes/lecture_10/CMPE421_Lecture_10_Quantifying_Uncertainty.pdf
---
# CMPE 421 · Lecture 10 · Quantifying uncertainty

> [!info] Slide numbers in this note
> The numbers used here are the ones printed in the footer of each slide, and they are the numbers our lecture notes use. The deck starts at printed number 2, so printed slide $n$ is PDF page $n-1$ and image file `..._p0(n-1).jpg`.

> [!summary] In short
> - Logic alone cannot decide under uncertainty. Probability summarises laziness and ignorance, and decision theory = utility theory + probability theory.
> - A probability model gives each sample point a number in $[0,1]$, summing to 1. Events, random variables and distributions follow from it.
> - Conditional probability, product rule, chain rule and Bayes' rule.
> - Inference by enumeration: sum out the hidden variables, then normalise with $\alpha$. Cost $O(d^n)$.
> - Independence and conditional independence shrink the joint. Naive Bayes needs a number of parameters linear in $n$.
> - Wumpus world: after breezes in $[1,2]$ and $[2,1]$, the pit probabilities are $0.31$ for $[1,3]$, $0.86$ for $[2,2]$, $0.31$ for $[3,1]$.

## 1. Acting under uncertainty (slides 2-6)
![[cmpe421_lecture_07_quantifyinguncertainty_p001.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p002.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p004.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p005.jpg]]

**Notes.** Action $A_t$ = leave for the airport $t$ minutes before the flight. Will it get me there on time? A purely logical answer is either false ("$A_{25}$ will get me there") or too weak to act on. Probability handles this.

Probabilistic assertions **summarise** laziness (failure to list all exceptions) and ignorance (missing facts). Probabilities are **subjective** (Bayesian): they relate a proposition to one's own state of knowledge, and change with new evidence. $P(A_{25}\mid\text{no reported accidents})=0.06$, and with "5 a.m." added it is $0.15$.

Suppose $P(\text{on time})$ is $0.04$ for $A_{25}$, $0.70$ for $A_{90}$, $0.95$ for $A_{120}$, $0.9999$ for $A_{1440}$. The choice depends on the probabilities and on my preferences. **Decision theory = utility theory + probability theory.**

> [!example] With numbers (our notes)
> Missing the flight costs 1000 points, each minute early costs 1. $EU(A_t)=-(1-P(\text{on time}))\cdot 1000-t$.
> $A_{25}$: $-985$. $A_{90}$: $-390$. $A_{120}$: $-170$. $A_{1440}$: $-1440.1$. Best: $A_{120}$.

## 2. Probability basics (slides 7-9)
![[cmpe421_lecture_07_quantifyinguncertainty_p006.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p007.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p008.jpg]]

**Notes.** The **sample space** $\Omega$ is the set of sample points $\omega$ (possible worlds, atomic events). A **probability model** assigns $P(\omega)$ with

$$0\le P(\omega)\le 1,\qquad \sum_{\omega\in\Omega}P(\omega)=1$$

An **event** $A$ is a subset of $\Omega$, with $P(A)=\sum_{\omega\in A}P(\omega)$. A **random variable** is a function from sample points to a range, and $P(X=x_i)=\sum\{P(\omega): X(\omega)=x_i\}$. Die: $P(\text{roll}<4)=\tfrac16+\tfrac16+\tfrac16=\tfrac12$.

$$P(a\lor b)=P(a)+P(b)-P(a\land b)$$

Why use probability (de Finetti, 1931): an agent whose betting probabilities violate the axioms can be forced to lose money whatever the outcome.

> [!warning] Error on slides 8 and 10
> The slides define $\mathit{Odd}(\omega)=\mathit{true}$ "if $\omega$ is even". Odd is meant. Slide 10 then prints $P(\mathit{odd})=\tfrac16$ and $P(\neg\mathit{odd})=\tfrac56$. The correct values are $\tfrac12$ and $\tfrac12$.

## 3. Propositions and distributions (slides 10-13)
![[cmpe421_lecture_07_quantifyinguncertainty_p009.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p010.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p011.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p012.jpg]]

**Notes.** A Boolean random variable has range $\{\mathit{true},\mathit{false}\}$. Lower case names the event that it is true: $P(\mathit{odd})=P(\mathit{Odd}=\mathit{true})$. A Boolean formula is the disjunction of the sample points where it is true.

A discrete distribution is a vector that sums to 1: $\mathbf{P}(\mathit{Weather})=\langle 0.72,0.1,0.08,0.1\rangle$ for sunny, rain, cloudy, snow. A **joint distribution** gives the probability of every atomic event:

| | sunny | rain | cloudy | snow |
|---|---|---|---|---|
| $\mathit{Cavity}=\mathit{true}$ | 0.144 | 0.02 | 0.016 | 0.02 |
| $\mathit{Cavity}=\mathit{false}$ | 0.576 | 0.08 | 0.064 | 0.08 |

Continuous variables use a **density** that integrates to 1, for example the uniform density $U[18,26]$ or the Gaussian

$$f(x)=\frac{1}{\sqrt{2\pi}\,\sigma}\,e^{-(x-\mu)^2/2\sigma^2}$$

> [!warning] Watch out
> - A density is not a probability. A single point has probability 0, and a density value can exceed 1.
> - Slide 11 says $P(\mathit{cavity})=0.1$ as a notation example. Every table in this lecture gives $P(\mathit{cavity})=0.2$.

## 4. Conditional probability, product rule, chain rule (slides 14-16)
![[cmpe421_lecture_07_quantifyinguncertainty_p013.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p014.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p015.jpg]]

**Notes.**

$$P(a\mid b)=\frac{P(a\land b)}{P(b)}\quad(P(b)\ne 0),\qquad P(a\land b)=P(a\mid b)\,P(b)$$

In distribution form, $\mathbf{P}(\mathit{Weather},\mathit{Cavity})=\mathbf{P}(\mathit{Weather}\mid\mathit{Cavity})\,\mathbf{P}(\mathit{Cavity})$ stands for one equation per pair of values. It is not matrix multiplication.

Chain rule, by repeated use of the product rule:

$$\mathbf{P}(X_1,\dots,X_n)=\prod_{i=1}^{n}\mathbf{P}(X_i\mid X_1,\dots,X_{i-1})$$

> [!warning] Watch out
> - $P(\mathit{cavity}\mid\mathit{toothache})$ is a belief given that toothache is all I know. It is not an if-then rule.
> - The $0.8$ on slide 14 is an illustration. The dentist table gives $P(\mathit{cavity}\mid\mathit{toothache})=0.6$.

## 5. Inference by enumeration (slides 17-21)
![[cmpe421_lecture_07_quantifyinguncertainty_p016.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p017.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p018.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p019.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p020.jpg]]

**Notes.** Dentist joint:

| | toothache, catch | toothache, $\neg$catch | $\neg$toothache, catch | $\neg$toothache, $\neg$catch |
|---|---|---|---|---|
| cavity | 0.108 | 0.012 | 0.072 | 0.008 |
| $\neg$cavity | 0.016 | 0.064 | 0.144 | 0.576 |

For any proposition $\phi$, add the atomic events where it is true: $P(\phi)=\sum_{\omega\models\phi}P(\omega)$.

> [!example] Slide 18
> $P(\mathit{cavity}\mid\mathit{toothache})=\dfrac{0.108+0.012}{0.108+0.012+0.016+0.064}=\dfrac{0.12}{0.2}=0.6$, and $P(\neg\mathit{cavity}\mid\mathit{toothache})=0.4$.

The shared denominator is the **normalisation constant**: $\alpha=1/P(\mathit{toothache})=5$. It is whatever makes the vector sum to 1, so it never has to be computed first.

General method: evidence variables $E=e$, query variables $Y$, hidden variables $H$.

$$\mathbf{P}(Y\mid E=e)=\alpha\,\mathbf{P}(Y,E=e)=\alpha\sum_h\mathbf{P}(Y,E=e,H=h)$$

Problems: time $O(d^n)$, space $O(d^n)$, and where do $O(d^n)$ numbers come from?

## 6. Independence and conditional independence (slides 22-24)
![[cmpe421_lecture_07_quantifyinguncertainty_p021.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p022.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p023.jpg]]

**Notes.** $A$ and $B$ are **independent** if $\mathbf{P}(A,B)=\mathbf{P}(A)\,\mathbf{P}(B)$, or equally $\mathbf{P}(A\mid B)=\mathbf{P}(A)$. Weather is independent of the three dentist variables, which cuts the joint into two smaller tables.

$\mathit{Catch}$ is **conditionally independent** of $\mathit{Toothache}$ given $\mathit{Cavity}$:

$$\mathbf{P}(\mathit{Catch}\mid\mathit{Toothache},\mathit{Cavity})=\mathbf{P}(\mathit{Catch}\mid\mathit{Cavity})$$

Chain rule plus this fact:

$$\mathbf{P}(\mathit{Toothache},\mathit{Catch},\mathit{Cavity})=\mathbf{P}(\mathit{Toothache}\mid\mathit{Cavity})\,\mathbf{P}(\mathit{Catch}\mid\mathit{Cavity})\,\mathbf{P}(\mathit{Cavity})$$

This needs $2+2+1=5$ independent numbers, against $2^3-1=7$ for the full joint.

> [!warning] Watch out
> Independent and conditionally independent are different, and neither implies the other. Toothache and catch are conditionally independent given cavity, but not independent.

## 7. Bayes' rule and naive Bayes (slides 25-26)
![[cmpe421_lecture_07_quantifyinguncertainty_p024.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p025.jpg]]

**Notes.**

$$P(a\mid b)=\frac{P(b\mid a)\,P(a)}{P(b)},\qquad \mathbf{P}(Y\mid X)=\alpha\,\mathbf{P}(X\mid Y)\,\mathbf{P}(Y)$$

It turns a causal probability into a diagnostic one. Meningitis in this deck: $P(s\mid m)=0.8$, $P(m)=0.0001$, $P(s)=0.1$, so $P(m\mid s)=0.8\cdot 0.0001/0.1=0.0008$.

**Naive Bayes model**: the effects are conditionally independent given the cause.

$$\mathbf{P}(\mathit{Cause},\mathit{Effect}_1,\dots,\mathit{Effect}_n)=\mathbf{P}(\mathit{Cause})\prod_i\mathbf{P}(\mathit{Effect}_i\mid\mathit{Cause})$$

The number of parameters is linear in $n$ ($1+2n$ for Boolean variables).

> [!warning] Watch out
> Lecture 7 solves meningitis with other data ($P(+s\mid -m)=0.01$ given instead of $P(s)$) and gets about $0.0079$. Both answers are correct for their own data. Check whether $P(\text{effect})$ is given or must be built by total probability.

## 8. Wumpus world: probabilistic reasoning (slides 27-45)
![[cmpe421_lecture_07_quantifyinguncertainty_p026.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p027.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p028.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p029.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p030.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p031.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p032.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p033.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p034.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p035.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p036.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p037.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p038.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p039.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p040.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p041.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p042.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p043.jpg]]
![[cmpe421_lecture_07_quantifyinguncertainty_p044.jpg]]

**Notes.** $P_{ij}$ is true if $[i,j]$ has a pit, $B_{ij}$ if $[i,j]$ is breezy. Model, by the product rule:

$$\mathbf{P}(P_{1,1},\dots,P_{4,4},B_{1,1},B_{1,2},B_{2,1})=\mathbf{P}(B_{1,1},B_{1,2},B_{2,1}\mid P_{1,1},\dots,P_{4,4})\,\mathbf{P}(P_{1,1},\dots,P_{4,4})$$

First term: 1 if the breezes match the pits, 0 otherwise. Second term: pits are independent with probability 0.2 per square.

Evidence: $b^*=\neg b_{1,1}\land b_{1,2}\land b_{2,1}$ and $p^*=\neg p_{1,1}\land\neg p_{1,2}\land\neg p_{2,1}$. Query: $\mathbf{P}(P_{1,3}\mid p^*,b^*)$.

Plain enumeration sums over 12 unknown squares ($2^{12}$ terms). Insight: split the unknown squares into the **fringe** ($[2,2]$ and $[3,1]$, next to visited squares) and the **other** 10. Given the fringe, the query and the known squares, $b^*$ is conditionally independent of the other squares, so they sum out to 1. Slides 32-42 do this one algebra step per slide. Result:

$$\mathbf{P}(P_{1,3}\mid p^*,b^*)=\alpha'\,\mathbf{P}(P_{1,3})\sum_{\mathit{fringe}}\mathbf{P}(b^*\mid p^*,P_{1,3},\mathit{fringe})\,P(\mathit{fringe})$$

> [!example] Getting the answer (slides 43-45)
> Fringe models and their probabilities: (pit, pit) $0.04$, (pit, none) $0.16$, (none, pit) $0.16$, (none, none) $0.64$.
> If $p_{1,3}$: three fringe models fit the breezes, $0.04+0.16+0.16=0.36$. If $\neg p_{1,3}$: two fit, $0.04+0.16=0.20$.
> $\alpha'\langle 0.2\cdot 0.36,\ 0.8\cdot 0.20\rangle=\alpha'\langle 0.072,0.16\rangle\approx\langle 0.31,0.69\rangle$.
> Slide 45: $P(p_{2,2}\mid p^*,b^*)\approx 0.86$ and $P(p_{3,1}\mid p^*,b^*)\approx 0.31$. So avoid $[2,2]$.

> [!warning] Watch out
> $0.31$, $0.86$, $0.31$ belong to three different variables. They need not sum to 1.

## 9. Summary (slide 46)
![[cmpe421_lecture_07_quantifyinguncertainty_p045.jpg]]

Slides not embedded: PDF page 3 (printed 4, outline), PDF page 46 (printed 47, homework notice).

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Probability model | $0\le P(\omega)\le 1$, $\sum_\omega P(\omega)=1$ |
| Event | $P(A)=\sum_{\omega\in A}P(\omega)$ |
| Or | $P(a\lor b)=P(a)+P(b)-P(a\land b)$ |
| Uniform density on $[a,b]$ | height $1/(b-a)$, probability = area |
| Conditional | $P(a\mid b)=P(a\land b)/P(b)$ |
| Product rule | $P(a\land b)=P(a\mid b)\,P(b)$ |
| Chain rule | $\mathbf{P}(X_1,\dots,X_n)=\prod_i\mathbf{P}(X_i\mid X_1,\dots,X_{i-1})$ |
| Enumeration | $\mathbf{P}(Y\mid E=e)=\alpha\sum_h\mathbf{P}(Y,E=e,H=h)$, cost $O(d^n)$ |
| $\alpha$ | normalisation constant: makes the vector sum to 1 |
| Independence | $\mathbf{P}(A,B)=\mathbf{P}(A)\,\mathbf{P}(B)$ |
| Conditional independence | $\mathbf{P}(A,B\mid C)=\mathbf{P}(A\mid C)\,\mathbf{P}(B\mid C)$ |
| Bayes' rule | $P(a\mid b)=P(b\mid a)P(a)/P(b)$ |
| Naive Bayes | $\mathbf{P}(\mathit{Cause})\prod_i\mathbf{P}(\mathit{Effect}_i\mid\mathit{Cause})$ |
| Decision theory | utility theory + probability theory |

## Check yourself
> [!question]- 1. Die: $C$ = "the roll is even", $D$ = "the roll is above 4". Find $P(C\lor D)$.
> $C=\{2,4,6\}$, $D=\{5,6\}$, $C\land D=\{6\}$. $P=\tfrac36+\tfrac26-\tfrac16=\tfrac23$.

> [!question]- 2. $X$ has density $U[10,30]$. Find the height and $P(12\le X\le 17)$.
> Height $1/20=0.05$. Area $0.05\cdot 5=0.25$.

> [!question]- 3. From the dentist table find $\mathbf{P}(\mathit{Cavity}\mid\neg\mathit{toothache})$. Name the hidden variable.
> Hidden: $\mathit{Catch}$. Sum it out: $\alpha\langle 0.072+0.008,\ 0.144+0.576\rangle=\alpha\langle 0.08,0.72\rangle$. $\alpha=1/0.8=1.25$, result $\langle 0.1,0.9\rangle$.

> [!question]- 4. $P(a,b)=0.12$, $P(a,\neg b)=0.28$, $P(\neg a,b)=0.18$, $P(\neg a,\neg b)=0.42$. Are $A$ and $B$ independent?
> $P(a)=0.4$, $P(b)=0.3$. Products: $0.12$, $0.28$, $0.18$, $0.42$. All four match: independent.

> [!question]- 5. A disease has $P(d)=0.01$, a symptom has $P(s\mid d)=0.9$ and $P(s)=0.05$. Find $P(d\mid s)$.
> $P(d\mid s)=0.9\cdot 0.01/0.05=0.18$.

> [!question]- 6. Wumpus world: the agent visited only $[1,1]$ (no breeze) and $[1,2]$ (breeze). Pit probability 0.2. Find $P(p_{1,3}\mid\text{evidence})$.
> No breeze in $[1,1]$ clears $[2,1]$. The breeze needs a pit in $[1,3]$ or $[2,2]$. If $p_{1,3}$: any $[2,2]$ fits, sum 1. If $\neg p_{1,3}$: needs a pit in $[2,2]$, $0.2$. $\alpha'\langle 0.2\cdot 1,\ 0.8\cdot 0.2\rangle=\alpha'\langle 0.2,0.16\rangle$, so $0.2/0.36\approx 0.556$.

## Links
- Course index: [[CMPE421 Home]]
- Previous lecture: [[CMPE421 L09 - Predicate logic and planning]]
- Same topic from the other deck (probability, Bayes' nets): [[CMPE421 L07 - MDPs, probability, Bayes nets, HMMs]]
- The logical wumpus agent: [[CMPE421 L08 - Logical agents]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_10_Quantifying_Uncertainty.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_10/CMPE421_Lecture_10_Quantifying_Uncertainty.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_10/CMPE421_Lecture_10_Quantifying_Uncertainty.pdf`
- Original slides (PDF, stored in the lecture_07 folder): [QuantifyingUncertainty.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/QuantifyingUncertainty.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/QuantifyingUncertainty.pdf`
