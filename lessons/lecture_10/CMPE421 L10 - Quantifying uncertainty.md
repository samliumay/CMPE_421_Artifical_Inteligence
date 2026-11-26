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

> [!info] The other methods on slide 3 (beyond the slides in part)
> Slide 3 lists three ways to handle uncertainty. The note above only uses the third.
> - **Default (nonmonotonic) logic**: assume the normal case ("my car has no flat tire") until evidence contradicts it. *Nonmonotonic* means that a conclusion can be withdrawn when a new fact arrives. In ordinary logic, more facts never remove a conclusion. Open problems: which assumptions are reasonable, and what to do when two of them contradict each other.
> - **Rules with fudge factors**: $X\mapsto_{0.99}Y$ reads "if $X$ then $Y$, with strength $0.99$". The rules are used one after the other like logic rules. This goes wrong when a causal rule ($\mathit{Sprinkler}\mapsto\mathit{WetGrass}$) is chained with a diagnostic rule ($\mathit{WetGrass}\mapsto\mathit{Rain}$).
> - **Probability**: one number for the degree of belief given all the evidence, "$A_{25}$ gets me there on time with probability $0.04$".
> - **Fuzzy logic** is a different thing. It gives a degree of **truth**, not of belief. "$\mathit{WetGrass}$ is true to degree $0.2$" means the grass is a little wet, and I am sure about it. "$P(\mathit{wetGrass})=0.2$" means the grass is either wet or not, and I believe "wet" with strength $0.2$.

> [!info] Laziness and two kinds of ignorance (beyond the slides)
> The textbook (Russell and Norvig) splits the slide's "ignorance" in two. It uses a dentist rule $\mathit{Toothache}\Rightarrow\mathit{Cavity}$ as the example.
> - **Laziness**: the full list of causes and exceptions is too long to write down and to use.
> - **Theoretical ignorance**: the science itself has no complete theory of the domain.
> - **Practical ignorance**: even with a complete theory, I do not know all the facts about this case (not every test has been run on this patient).
>
> A probability such as $0.8$ summarises all three in one number.

> [!question]- Check (slide 2): A logical agent may only state rules that are certainly true. Name the two ways in which it fails on the airport problem, and say which of the four listed problems a flat tire belongs to.
> 1) It **risks falsehood**: the short rule "$A_{25}$ will get me there on time" is simply not always true.
> 2) Its conclusion is **too weak**: the true rule needs an endless list of conditions (no accident, no rain, no flat tire, ...). The agent can never check all of them, so it can never conclude anything it can act on.
> A flat tire is problem 3, **uncertainty in action outcomes**: the action "drive" does not always have the planned result.

> [!question]- Check (slide 3): Use the two fudge-factor rules of the slide like logic rules, one after the other. The sprinkler is on. What strength do you get for $\mathit{Rain}$, and why is this result nonsense?
> Chain the rules and multiply the strengths: $\mathit{Sprinkler}\mapsto_{0.99}\mathit{WetGrass}\mapsto_{0.7}\mathit{Rain}$ gives $0.99\cdot 0.7=0.693$.
> So the system believes in rain with strength about $0.69$ only because the sprinkler is on. A sprinkler does not cause rain.
> The sprinkler already **explains** the wet grass, so wet grass is no longer a reason to believe in rain. A rule with a fixed number cannot see this. A conditional probability can, because it always depends on all the evidence: $P(\mathit{rain}\mid\mathit{wetGrass},\mathit{sprinkler})$ is a different number from $P(\mathit{rain}\mid\mathit{wetGrass})$.

> [!question]- Check (slide 5): The slide gives $P(A_{25}\mid\text{no reported accidents})=0.06$ and $P(A_{25}\mid\text{no reported accidents},\ 5\text{ a.m.})=0.15$. The road did not change between the two statements. Which number is correct?
> Both are correct. A subjective (Bayesian) probability describes a **state of knowledge**, not the road. The two numbers answer two different questions, because the evidence after the bar is different.
> $0.06$ is right for an agent that knows only "no reported accidents". $0.15$ is right for an agent that also knows it is 5 a.m. (little traffic).
> The agent must use the number that is conditioned on **all** it knows, so at 5 a.m. it uses $0.15$.

> [!question]- Check (slide 6): Use the four probabilities of the slide. Now missing the flight costs only 100 points, and each minute of leaving early costs 1 point. Compute $EU(A_t)=-(1-P(\text{on time}))\cdot 100-t$ for the four actions. Which action is best, and what does this show?
> $A_{25}$: $-(1-0.04)\cdot 100-25=-96-25=-121$.
> $A_{90}$: $-(1-0.70)\cdot 100-90=-30-90=-120$.
> $A_{120}$: $-(1-0.95)\cdot 100-120=-5-120=-125$.
> $A_{1440}$: $-(1-0.9999)\cdot 100-1440=-0.01-1440=-1440.01$.
> The maximum expected utility is $-120$, so the agent chooses $A_{90}$.
> With a cost of 1000 (the example above) the best action was $A_{120}$. The probabilities are the same in both cases. Only the preferences changed, and the best action changed with them. This is why decision theory needs utility theory **and** probability theory.

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

> [!question]- Check (slide 7): A loaded die has $P(1)=P(2)=P(3)=P(4)=\tfrac18$ and $P(5)=P(6)=\tfrac14$. Is this a valid probability model? Find $P(\text{roll}<4)$ and $P(\text{roll}\ge 5)$.
> Check the two properties. Every value is between 0 and 1. The sum is $4\cdot\tfrac18+2\cdot\tfrac14=\tfrac12+\tfrac12=1$. So the model is valid.
> Event $\{1,2,3\}$: $P(\text{roll}<4)=\tfrac18+\tfrac18+\tfrac18=\tfrac38$.
> Event $\{5,6\}$: $P(\text{roll}\ge 5)=\tfrac14+\tfrac14=\tfrac12$.
> For a fair die the two answers are $\tfrac12$ and $\tfrac13$. The sample space is the same. Only the numbers $P(\omega)$ changed.

> [!question]- Check (slide 8): For the loaded die of the previous check, let $X(\omega)=\omega \bmod 3$ (the remainder after division by 3). Find the probability distribution of $X$.
> $X$ is a function from sample points to the range $\{0,1,2\}$. For each value, add the sample points that map to it.
> $X=0$ for $\omega\in\{3,6\}$: $P(X=0)=\tfrac18+\tfrac14=\tfrac38$.
> $X=1$ for $\omega\in\{1,4\}$: $P(X=1)=\tfrac18+\tfrac18=\tfrac14$.
> $X=2$ for $\omega\in\{2,5\}$: $P(X=2)=\tfrac18+\tfrac14=\tfrac38$.
> Distribution $\langle\tfrac38,\tfrac14,\tfrac38\rangle$. Check: $\tfrac38+\tfrac14+\tfrac38=1$.

> [!question]- Check (slide 9): An agent states $P(a)=0.4$, $P(b)=0.3$ and $P(a\lor b)=0.8$. Show that these three beliefs violate the rule on the slide. What does de Finetti's result say about this agent?
> Solve the rule for the overlap: $P(a\land b)=P(a)+P(b)-P(a\lor b)=0.4+0.3-0.8=-0.1$.
> A probability cannot be negative, so no probability model has these three values. In the picture: the two circles together can cover at most $0.4+0.3=0.7$ of the box, and only if they do not overlap.
> De Finetti: an agent that bets with such beliefs can be offered a set of bets that it accepts one by one, and that together lose money for it in every possible outcome.

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

> [!question]- Check (slide 10): Given $P(a\land b)=0.1$, $P(a\land\neg b)=0.3$ and $P(\neg a\land b)=0.2$. Find $P(a\lor b)$ with the last formula of the slide, then check it with inclusion-exclusion. What is $P(\neg a\land\neg b)$?
> The three cases $(\neg a\land b)$, $(a\land\neg b)$, $(a\land b)$ cannot be true together, so their probabilities add: $P(a\lor b)=0.2+0.3+0.1=0.6$.
> Check: $P(a)=0.1+0.3=0.4$ and $P(b)=0.1+0.2=0.3$, so $P(a)+P(b)-P(a\land b)=0.4+0.3-0.1=0.6$. The same.
> The four cases together have probability 1, so $P(\neg a\land\neg b)=1-0.6=0.4$.

> [!question]- Check (slide 11): Someone defines $\mathit{Weather}$ with the values $\{\mathit{sunny},\mathit{rain},\mathit{warm}\}$. Which two requirements of the slide does this break? And what kind of random variable is $\mathit{Temp}$ in the proposition $\mathit{Temp}<22.0$?
> The values must be **mutually exclusive**: only one can be true at a time. A day can be sunny and warm, so this fails.
> The values must be **exhaustive**: one of them is always true. A cold cloudy day has none of the three values, so this fails too.
> Without these two requirements the probabilities of the values would not sum to 1.
> $\mathit{Temp}$ is a **continuous** random variable. $\mathit{Temp}<22.0$ is still a proposition: it is either true or false.

> [!question]- Check (slide 12): Use the table $\mathbf{P}(\mathit{Weather},\mathit{Cavity})$. Find $P(\mathit{cavity})$, $P(\mathit{Weather}=\mathit{rain})$ and $P(\mathit{cavity}\lor\mathit{Weather}=\mathit{rain})$. How many of the 8 entries are free to choose?
> $P(\mathit{cavity})$ = sum of the first row: $0.144+0.02+0.016+0.02=0.2$.
> $P(\mathit{rain})$ = sum of the rain column: $0.02+0.08=0.1$.
> $P(\mathit{cavity}\lor\mathit{rain})$ = first row plus the rest of the rain column: $0.2+0.08=0.28$. Check with inclusion-exclusion: $0.2+0.1-0.02=0.28$.
> The 8 entries must sum to 1, so 7 are free and the last one is fixed.

> [!question]- Check (slide 13): For the density $U[18,26]$ of the slide, what is $P(X=20)$? Then compute the height of a Gaussian density at $x=\mu$ for $\sigma=0.1$. Why is this height not a contradiction?
> $P(X=20)$ is the area over one point: width 0 times height $0.125$ = 0. Only intervals have a probability, for example $P(20\le X\le 22)=0.125\cdot 2=0.25$.
> Gaussian at $x=\mu$: the exponent is 0 and $e^{0}=1$, so $f(\mu)=\dfrac{1}{\sqrt{2\pi}\,\sigma}=\dfrac{1}{2.5066\cdot 0.1}\approx 3.99$.
> A value above 1 is allowed, because a density is a probability **per unit of** $x$, not a probability. The rule is only that the total area is 1. A small $\sigma$ gives a narrow and high curve.

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

> [!question]- Check (slide 14): How many numbers does $\mathbf{P}(\mathit{Weather}\mid\mathit{Cavity})$ contain, which groups of them sum to 1, and how many are free? Also give $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})$.
> $\mathit{Weather}$ has 4 values and $\mathit{Cavity}$ has 2, so there are $4\cdot 2=8$ numbers.
> They do **not** sum to 1 all together. For each fixed condition the 4 weather values sum to 1: one group for $\mathit{cavity}$ and one for $\neg\mathit{cavity}$. The total is 2.
> Each group has $4-1=3$ free numbers, so $2\cdot 3=6$ are free.
> $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})=0$: the evidence already says that there is no cavity. This is the mirror case of the "$=1$" line on the slide.

> [!question]- Check (slide 15): Given $P(\mathit{cavity})=0.2$, $P(\mathit{rain}\mid\mathit{cavity})=0.3$ and $P(\mathit{rain}\mid\neg\mathit{cavity})=0.1$ (new numbers, not the table of slide 12). Find $P(\mathit{rain},\mathit{cavity})$, $P(\mathit{rain},\neg\mathit{cavity})$, $P(\mathit{rain})$ and $P(\mathit{cavity}\mid\mathit{rain})$.
> Product rule: $P(\mathit{rain},\mathit{cavity})=P(\mathit{rain}\mid\mathit{cavity})\,P(\mathit{cavity})=0.3\cdot 0.2=0.06$.
> Product rule: $P(\mathit{rain},\neg\mathit{cavity})=0.1\cdot(1-0.2)=0.1\cdot 0.8=0.08$.
> Add the two cases: $P(\mathit{rain})=0.06+0.08=0.14$.
> Definition: $P(\mathit{cavity}\mid\mathit{rain})=\dfrac{P(\mathit{rain},\mathit{cavity})}{P(\mathit{rain})}=\dfrac{0.06}{0.14}=\dfrac37\approx 0.429$.
> Each line is one of the "one equation per pair of values" on the slide. Nothing is multiplied as a matrix.

> [!question]- Check (slide 16): Write the chain rule for four variables $A,B,C,D$. Then compute $P(a,b,c)$ from $P(a)=0.5$, $P(b\mid a)=0.4$, $P(c\mid a,b)=0.3$.
> $\mathbf{P}(A,B,C,D)=\mathbf{P}(A)\,\mathbf{P}(B\mid A)\,\mathbf{P}(C\mid A,B)\,\mathbf{P}(D\mid A,B,C)$.
> Each variable is conditioned on all the variables before it. The first one has nothing before it.
> $P(a,b,c)=P(a)\,P(b\mid a)\,P(c\mid a,b)=0.5\cdot 0.4\cdot 0.3=0.06$.
> The rule is always true. It needs no independence assumption, and any order of the variables is allowed.

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

> [!info] The symbols on slide 21 (beyond the slides in part)
> - $\mathbf{X}$ = all variables. $\mathbf{E}$ = the ones whose values $\mathbf{e}$ we observed. $\mathbf{Y}$ = the ones we ask about. $\mathbf{H}=\mathbf{X}-\mathbf{Y}-\mathbf{E}$ = all the rest.
> - **Summing out** (marginalisation): $\mathbf{P}(\mathbf{Y},\mathbf{e})=\sum_{\mathbf{h}}\mathbf{P}(\mathbf{Y},\mathbf{e},\mathbf{h})$. The hidden variables can have any value, and the cases exclude each other, so their probabilities add.
> - $n$ = number of variables, $d$ = the largest number of values of one variable. The joint table has up to $d^n$ entries. For Boolean variables $d=2$, so $2^n$ entries, and $2^n-1$ of them are free (they sum to 1).
> - With $k$ Boolean hidden variables, each entry of the answer is a sum of $2^k$ table entries.

> [!info] Practice table for the checks of sections 5 and 6 (beyond the slides)
> Three Boolean variables: $\mathit{Flu}$, $\mathit{Fever}$, $\mathit{Cough}$. The eight entries sum to 1.
>
> | | fever, cough | fever, $\neg$cough | $\neg$fever, cough | $\neg$fever, $\neg$cough |
> |---|---|---|---|---|
> | flu | 0.112 | 0.028 | 0.048 | 0.012 |
> | $\neg$flu | 0.020 | 0.060 | 0.180 | 0.540 |

> [!question]- Check (slide 17): From the practice table, find $P(\mathit{fever})$ and $P(\mathit{flu}\lor\mathit{fever})$ by adding atomic events.
> $P(\mathit{fever})$: the four entries in the two "fever" columns. $0.112+0.028+0.020+0.060=0.22$.
> $P(\mathit{flu}\lor\mathit{fever})$: every entry where flu or fever (or both) is true. That is the whole flu row plus the fever entries of the $\neg$flu row.
> $0.112+0.028+0.048+0.012+0.020+0.060=0.28$.
> Check with the complement: the two entries left out are $0.180+0.540=0.72$, and $1-0.72=0.28$.

> [!question]- Check (slide 18): From the practice table, find $P(\mathit{flu}\mid\mathit{fever})$ and $P(\neg\mathit{flu}\mid\mathit{fever})$. Name the query, evidence and hidden variable.
> Query: $\mathit{Flu}$. Evidence: $\mathit{Fever}=\mathit{true}$. Hidden: $\mathit{Cough}$, so add over both of its values.
> $P(\mathit{flu},\mathit{fever})=0.112+0.028=0.14$.
> $P(\neg\mathit{flu},\mathit{fever})=0.020+0.060=0.08$.
> $P(\mathit{fever})=0.14+0.08=0.22$.
> $P(\mathit{flu}\mid\mathit{fever})=\dfrac{0.14}{0.22}=\dfrac{7}{11}\approx 0.636$ and $P(\neg\mathit{flu}\mid\mathit{fever})=\dfrac{0.08}{0.22}=\dfrac{4}{11}\approx 0.364$. The two sum to 1.

> [!question]- Check (slides 19-20): From the practice table, compute $\mathbf{P}(\mathit{Flu}\mid\mathit{cough})$ in the style of slide 20, with $\alpha$. What is $\alpha$, and which probability is $1/\alpha$?
> $\mathbf{P}(\mathit{Flu}\mid\mathit{cough})=\alpha\,\mathbf{P}(\mathit{Flu},\mathit{cough})=\alpha\,[\mathbf{P}(\mathit{Flu},\mathit{cough},\mathit{fever})+\mathbf{P}(\mathit{Flu},\mathit{cough},\neg\mathit{fever})]$.
> $=\alpha\,[\langle 0.112,0.020\rangle+\langle 0.048,0.180\rangle]=\alpha\,\langle 0.16,0.20\rangle$.
> The two entries must sum to 1: $\alpha=\dfrac{1}{0.16+0.20}=\dfrac{1}{0.36}\approx 2.78$.
> $\mathbf{P}(\mathit{Flu}\mid\mathit{cough})=\langle\tfrac{0.16}{0.36},\tfrac{0.20}{0.36}\rangle=\langle\tfrac49,\tfrac59\rangle\approx\langle 0.444,0.556\rangle$.
> $1/\alpha=0.36=P(\mathit{cough})$, the probability of the evidence. It came out as the sum of the two entries, so it was never computed separately.

> [!question]- Check (slide 21): A model has 10 Boolean variables. The query has 1 variable and the evidence has 2. How many entries has the full joint, how many are free, and how many entries are added for the whole answer? How many entries would the joint have with 3 values per variable?
> Entries: $2^{10}=1024$. Free: $2^{10}-1=1023$.
> Hidden variables: $10-1-2=7$. Each value of the query needs a sum over $2^7=128$ entries. The query has 2 values, so $2\cdot 128=256$ entries are added.
> With $d=3$: $3^{10}=59049$ entries.
> The table grows as $d^n$. This is the reason for all three problems on the slide: time, space, and where to get so many numbers.

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

> [!question]- Check (slide 22): In the practice table of section 5, are $\mathit{Fever}$ and $\mathit{Cough}$ independent? Then add a variable $\mathit{Day}$ with 5 values that is independent of the other three. How many entries has the joint of all four, and how many after the decomposition?
> $P(\mathit{fever})=0.112+0.028+0.020+0.060=0.22$.
> $P(\mathit{cough})=0.112+0.048+0.020+0.180=0.36$.
> $P(\mathit{fever},\mathit{cough})=0.112+0.020=0.132$.
> Product: $P(\mathit{fever})\,P(\mathit{cough})=0.22\cdot 0.36=0.0792$. This is not $0.132$, so they are **not independent**. One failing pair of values is enough.
> The other form shows the same: $P(\mathit{fever}\mid\mathit{cough})=0.132/0.36\approx 0.367$, which is larger than $P(\mathit{fever})=0.22$. A cough makes fever more likely.
> With $\mathit{Day}$: the full joint has $2\cdot 2\cdot 2\cdot 5=40$ entries. Decomposed into $\mathbf{P}(\mathit{Flu},\mathit{Fever},\mathit{Cough})\,\mathbf{P}(\mathit{Day})$ it has $8+5=13$ entries.

> [!question]- Check (slide 23): In the practice table of section 5, test whether $\mathit{Cough}$ is conditionally independent of $\mathit{Fever}$ given $\mathit{Flu}$. Test the case $\mathit{flu}$ and the case $\neg\mathit{flu}$.
> Case $\mathit{flu}$:
> $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=\dfrac{0.112}{0.112+0.028}=\dfrac{0.112}{0.14}=0.8$.
> $P(\mathit{cough}\mid\mathit{flu})=\dfrac{0.112+0.048}{0.2}=\dfrac{0.16}{0.2}=0.8$. Equal.
> Case $\neg\mathit{flu}$:
> $P(\mathit{cough}\mid\mathit{fever},\neg\mathit{flu})=\dfrac{0.020}{0.020+0.060}=\dfrac{0.02}{0.08}=0.25$.
> $P(\mathit{cough}\mid\neg\mathit{flu})=\dfrac{0.020+0.180}{0.8}=\dfrac{0.2}{0.8}=0.25$. Equal.
> In both cases, knowing $\mathit{fever}$ does not change the probability of $\mathit{cough}$. (The values for $\neg\mathit{fever}$ and for $\neg\mathit{cough}$ then match too, because they are fixed by "sum to 1".) So $\mathit{Cough}$ is conditionally independent of $\mathit{Fever}$ given $\mathit{Flu}$.
> Compare with the check for slide 22: the same two variables are **not** independent when $\mathit{Flu}$ is unknown.

> [!question]- Check (slide 24): Which of the three "=" steps on the slide uses conditional independence, and which use only the product rule? Then rebuild the entry $P(\mathit{flu},\neg\mathit{fever},\mathit{cough})$ of the practice table from $P(\mathit{flu})=0.2$, $P(\mathit{fever}\mid\mathit{flu})=0.7$, $P(\mathit{cough}\mid\mathit{flu})=0.8$.
> Steps 1 and 2 are the product rule (chain rule). They are always true.
> Step 3 replaces $\mathbf{P}(\mathit{Toothache}\mid\mathit{Catch},\mathit{Cavity})$ by $\mathbf{P}(\mathit{Toothache}\mid\mathit{Cavity})$. Only this step uses conditional independence.
> Entry: $P(\mathit{flu})\,P(\neg\mathit{fever}\mid\mathit{flu})\,P(\mathit{cough}\mid\mathit{flu})=0.2\cdot(1-0.7)\cdot 0.8=0.2\cdot 0.3\cdot 0.8=0.048$. This is the table entry.
> The factored model needs $1+2+2=5$ numbers ($P(\mathit{flu})$, then $P(\mathit{fever}\mid\cdot)$ and $P(\mathit{cough}\mid\cdot)$ for $\mathit{flu}$ and for $\neg\mathit{flu}$). The full table needs $2^3-1=7$.

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

> [!info] Where the count $1+2n$ comes from (beyond the slides)
> Boolean cause, $n$ Boolean effects. $\mathbf{P}(\mathit{Cause})$ needs 1 number (the other is 1 minus it). Each $\mathbf{P}(\mathit{Effect}_i\mid\mathit{Cause})$ needs 2 numbers: $P(\mathit{effect}_i\mid\mathit{cause})$ and $P(\mathit{effect}_i\mid\neg\mathit{cause})$. Total $1+2n$.
> The full joint over the same $n+1$ variables needs $2^{n+1}-1$ numbers.
> In a medical test, $P(\mathit{positive}\mid\mathit{disease})$ is called the **sensitivity** and $P(\mathit{positive}\mid\neg\mathit{disease})$ the **false-positive rate**.

> [!question]- Check (slide 25): A disease has prior $P(d)=0.02$. A test has sensitivity $P(\mathit{pos}\mid d)=0.9$ and false-positive rate $P(\mathit{pos}\mid\neg d)=0.05$. $P(\mathit{pos})$ is not given. Find $P(d\mid\mathit{pos})$.
> Build $P(\mathit{pos})$ from the two cases (product rule, then add):
> $P(\mathit{pos},d)=P(\mathit{pos}\mid d)\,P(d)=0.9\cdot 0.02=0.018$.
> $P(\mathit{pos},\neg d)=P(\mathit{pos}\mid\neg d)\,P(\neg d)=0.05\cdot 0.98=0.049$.
> $P(\mathit{pos})=0.018+0.049=0.067$.
> Bayes' rule: $P(d\mid\mathit{pos})=\dfrac{P(\mathit{pos}\mid d)\,P(d)}{P(\mathit{pos})}=\dfrac{0.018}{0.067}=\dfrac{18}{67}\approx 0.269$.
> The test is good, but the posterior is only about 27%. The disease is rare, so the healthy people who test positive ($0.049$) outnumber the sick people who test positive ($0.018$).
> The same in $\alpha$ form: $\alpha\,\langle 0.018,0.049\rangle$ with $\alpha=1/0.067$.

> [!question]- Check (slide 26, part a): Continue the previous check. A second test has $P(\mathit{pos}_2\mid d)=0.8$ and $P(\mathit{pos}_2\mid\neg d)=0.1$. The two tests are conditionally independent given $D$. Both are positive. Find $\mathbf{P}(D\mid\mathit{pos}_1,\mathit{pos}_2)$.
> Naive Bayes: $\mathbf{P}(D\mid\mathit{pos}_1,\mathit{pos}_2)=\alpha\,\mathbf{P}(\mathit{pos}_1\mid D)\,\mathbf{P}(\mathit{pos}_2\mid D)\,\mathbf{P}(D)$.
> Entry for $d$: $0.9\cdot 0.8\cdot 0.02=0.0144$.
> Entry for $\neg d$: $0.05\cdot 0.1\cdot 0.98=0.0049$.
> Normalise: sum $=0.0144+0.0049=0.0193$, so $\alpha=1/0.0193\approx 51.8$.
> $\mathbf{P}(D\mid\mathit{pos}_1,\mathit{pos}_2)=\langle\tfrac{0.0144}{0.0193},\tfrac{0.0049}{0.0193}\rangle\approx\langle 0.746,0.254\rangle$.
> One positive test gave about $0.269$. Two give about $0.746$. $P(\mathit{pos}_1,\mathit{pos}_2)$ was never needed: it is $1/\alpha=0.0193$.

> [!question]- Check (slide 26, part b): A naive Bayes model has one Boolean cause and 20 Boolean effects. How many numbers does it need? How many does the full joint over the same variables need?
> Naive Bayes: $1+2n=1+2\cdot 20=41$.
> Full joint: 21 Boolean variables, so $2^{21}-1=2097151$.
> One more effect adds 2 numbers to the naive Bayes model. It doubles the full joint. This is "linear in $n$" against "exponential in $n$".

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

> [!info] The derivation of slides 32-42, with the reason for each step (beyond the slides in part)
> Names: *unknown* = the 12 squares that are not visited and not the query. *fringe* = $[2,2]$ and $[3,1]$. *other* = the remaining 10 squares. A breeze in a square means a pit in one of its neighbours (up, down, left, right).
> 1. Slide 32, definition of conditional probability: $\mathbf{P}(P_{1,3}\mid p^*,b^*)=\mathbf{P}(P_{1,3},p^*,b^*)/P(p^*,b^*)$.
> 2. Slide 33: $P(p^*,b^*)$ is one number, the same for $p_{1,3}$ and $\neg p_{1,3}$. Write it as $\alpha=1/P(p^*,b^*)$.
> 3. Slide 34, summing out: $=\alpha\sum_{\mathit{unknown}}\mathbf{P}(P_{1,3},\mathit{unknown},p^*,b^*)$.
> 4. Slide 35, product rule with $b^*$ in front: $=\alpha\sum_{\mathit{unknown}}\mathbf{P}(b^*\mid P_{1,3},p^*,\mathit{unknown})\,\mathbf{P}(P_{1,3},p^*,\mathit{unknown})$. The first factor is the "breezes match the pits" term of the model: 1 or 0.
> 5. Slide 36: split $\mathit{unknown}$ into $\mathit{fringe}$ and $\mathit{other}$. One sum becomes two sums.
> 6. Slide 37, conditional independence: the neighbours of $[1,1]$, $[1,2]$, $[2,1]$ are all known, query or fringe squares. No *other* square touches a breeze square. So $\mathit{other}$ drops out of the first factor.
> 7. Slide 38: the first factor does not contain $\mathit{other}$, so it moves in front of $\sum_{\mathit{other}}$. This is $\sum_j a\,x_j=a\sum_j x_j$.
> 8. Slide 39, independence of the pits: $\mathbf{P}(P_{1,3},p^*,\mathit{fringe},\mathit{other})=\mathbf{P}(P_{1,3})\,P(p^*)\,P(\mathit{fringe})\,P(\mathit{other})$.
> 9. Slide 40: move $P(p^*)$, $\mathbf{P}(P_{1,3})$ and $P(\mathit{fringe})$ in front of the sums that do not contain them.
> 10. Slide 41: $\sum_{\mathit{other}}P(\mathit{other})=1$, because it adds the probabilities of all possible cases of the 10 squares.
> 11. Slide 42: $P(p^*)$ is a constant. Put it into the constant: $\alpha'=\alpha\,P(p^*)$.
>
> After step 11 only $\mathbf{P}(P_{1,3})=\langle 0.2,0.8\rangle$ and a sum over the 4 fringe cases are left.

> [!question]- Check (slide 27): How many Boolean variables does the joint distribution on the slide have, and how many entries does its full table have? Read from the figure: which squares are yellow, and what do they have in common?
> Pit variables: $4\cdot 4=16$. Breeze variables: 3. Total 19 Boolean variables, so $2^{19}=524288$ entries.
> Yellow: $[1,3]$, $[2,2]$, $[3,1]$. Each one is not visited and is next to a visited square. The agent can only move to one of these three, and logic cannot prove any of them safe.

> [!question]- Check (slide 28): World A has pits in exactly $[2,2]$ and $[4,4]$. World B has a pit only in $[1,3]$. For each world, find the joint probability of the world together with the observed breezes $\neg b_{1,1},b_{1,2},b_{2,1}$.
> World A, first term: $[1,1]$ has no pit next to it, so no breeze there. $[1,2]$ and $[2,1]$ are both next to the pit in $[2,2]$, so both are breezy. The breezes match: the term is 1.
> World A, second term: 2 squares with a pit and 14 without: $0.2^2\cdot 0.8^{14}=0.04\cdot 0.04398\approx 0.00176$.
> World A: $1\cdot 0.00176\approx 0.00176$.
> World B, first term: $[2,1]$ has the neighbours $[1,1]$, $[2,2]$, $[3,1]$. None has a pit, so $[2,1]$ would not be breezy. But a breeze was observed there. The term is 0.
> World B: $0\cdot(\ldots)=0$. This world is impossible given the evidence, whatever its prior is.

> [!question]- Check (slide 29): How many squares are in $\mathit{unknown}$, and how many terms has the sum for one value of $P_{1,3}$? How many would it be on a $6\times 6$ board with the same three visited squares?
> $16$ squares $-\,3$ visited $-\,1$ query $=12$ unknown squares. Each is pit or no pit: $2^{12}=4096$ terms.
> $6\times 6$: $36-3-1=32$ unknown squares, so $2^{32}=4294967296$ terms (about 4.3 billion).
> The number of terms doubles with every extra square. This is the "grows exponentially" line of the slide.

> [!question]- Check (slide 30): Is $[2,3]$ a fringe square or an *other* square? Why does a pit in $[4,4]$ not change $P(b^*\mid P_{1,3},p^*,\mathit{fringe})$?
> $[2,3]$ is an *other* square. Its neighbours are $[1,3]$, $[3,3]$, $[2,2]$, $[2,4]$. None of them is a visited square, so it is not next to any observed breeze.
> $b^*$ talks only about the breezes in $[1,1]$, $[1,2]$, $[2,1]$. A breeze in a square depends only on pits in its neighbours. All these neighbours are known squares, the query $[1,3]$, or the fringe $[2,2]$, $[3,1]$.
> $[4,4]$ is not a neighbour of any of the three breeze squares. Once the known, query and fringe squares are fixed, the three breezes are fully decided, and $[4,4]$ adds nothing.

> [!question]- Check (slide 31): Compare the second line of the slide with the last line. How many terms are added in each, counting both values of $P_{1,3}$?
> Second line: a sum over all 12 unknown squares, $2^{12}=4096$ terms for each value of $P_{1,3}$, so $2\cdot 4096=8192$ terms.
> Last line: a sum over the 2 fringe squares only, $2^{2}=4$ terms for each value, so $2\cdot 4=8$ terms.
> The 10 *other* squares have left the formula. The saving is a factor of $2^{10}=1024$.

> [!question]- Check (slides 32-33): Why may $1/P(p^*,b^*)$ be written as a constant $\alpha$, when we do not know its value?
> $P(p^*,b^*)$ contains only the evidence. It does not contain the query variable, so it is the same number in the entry for $p_{1,3}$ and in the entry for $\neg p_{1,3}$.
> Dividing both entries by the same number does not change their ratio. We compute the two entries without it. Then we choose $\alpha$ so that the two entries sum to 1.
> This is the same trick as $\alpha=1/P(\mathit{toothache})$ on slides 19-20.

> [!question]- Check (slides 34-35): Slide 35 uses the product rule to put $b^*$ in front of the bar. Why $b^*$, and not for example $P_{1,3}$?
> The model of slide 28 gives exactly this kind of term: the probability of the breezes **given all the pits** is 1 if the breezes match the pits, and 0 otherwise.
> After slide 34 every term of the sum fixes all 16 pit variables ($P_{1,3}$, $p^*$ and $\mathit{unknown}$). So $\mathbf{P}(b^*\mid P_{1,3},p^*,\mathit{unknown})$ can be read off from the pit positions with no computation.
> The second factor $\mathbf{P}(P_{1,3},p^*,\mathit{unknown})$ contains only pits. It is a product of $0.2$ and $0.8$ factors.

> [!question]- Check (slides 36-37): On slide 37, $\mathit{other}$ is removed from the first factor but not from the second factor. Why not from the second?
> The first factor is about the breezes. Given the fringe, the query and the known squares, the breezes do not depend on the *other* squares (conditional independence), so $\mathit{other}$ can be dropped there.
> The second factor $\mathbf{P}(P_{1,3},p^*,\mathit{fringe},\mathit{other})$ is the probability of a full pit layout. It does depend on $\mathit{other}$: a layout with 3 pits among the *other* squares has a different probability than a layout with none.
> So $\mathit{other}$ stays there. It is removed later, on slide 41, when it sums to 1.

> [!question]- Check (slides 38-39): Slide 39 writes $\mathbf{P}(P_{1,3},p^*,\mathit{fringe},\mathit{other})$ as a product of four factors. Which assumption allows this? Compute $P(p^*)$ and $P(\mathit{fringe})$ for the fringe case "pit in $[2,2]$, no pit in $[3,1]$".
> The assumption of slide 28: every square has a pit with probability $0.2$, independently of all other squares. For independent variables the joint is the product of the separate probabilities.
> $p^*=\neg p_{1,1}\land\neg p_{1,2}\land\neg p_{2,1}$: $P(p^*)=0.8\cdot 0.8\cdot 0.8=0.512$.
> Fringe case: $P(p_{2,2})\,P(\neg p_{3,1})=0.2\cdot 0.8=0.16$.

> [!question]- Check (slides 40-41): Why is $\sum_{\mathit{other}}P(\mathit{other})=1$? Check it for a small case with only 2 *other* squares. Is it still true if the pit probability is $0.1$?
> The sum goes over every possible pit layout of the *other* squares. These layouts exclude each other and one of them must be true, so their probabilities sum to 1.
> Two squares, $0.2$: $0.2\cdot 0.2+0.2\cdot 0.8+0.8\cdot 0.2+0.8\cdot 0.8=0.04+0.16+0.16+0.64=1$.
> Two squares, $0.1$: $0.01+0.09+0.09+0.81=1$.
> It is true for every pit probability and for every number of squares, here 10 squares and $2^{10}=1024$ layouts.

> [!question]- Check (slide 42): Slide 44 finds $\alpha'\approx 4.31$. Use $\alpha'=\alpha\,P(p^*)$ to find $\alpha$ and the probability of the evidence $P(p^*,b^*)$.
> $P(p^*)=0.8^3=0.512$.
> $\alpha=\dfrac{\alpha'}{P(p^*)}=\dfrac{4.3103}{0.512}\approx 8.42$.
> $P(p^*,b^*)=\dfrac{1}{\alpha}\approx 0.119$. Direct check: $P(p^*)\cdot(0.072+0.16)=0.512\cdot 0.232\approx 0.119$.
> We never need these two numbers. Any constant factor can go into the normalisation constant.

> [!question]- Check (slide 43): The slide says that for $\neg p_{1,3}$ the four fringe cases give 1, 1, 0, 0. Explain the second value (pit in $[2,2]$ only) and the third value (pit in $[3,1]$ only).
> The breeze in $[1,2]$ needs a pit in $[1,3]$ or $[2,2]$ ($[1,1]$ is known to be free). The breeze in $[2,1]$ needs a pit in $[2,2]$ or $[3,1]$.
> Second case, $\neg p_{1,3}$ and pit in $[2,2]$ only: $[2,2]$ is next to both breeze squares, so both breezes are explained. Value 1.
> Third case, $\neg p_{1,3}$ and pit in $[3,1]$ only: the breeze in $[2,1]$ is explained, but $[1,2]$ has no pit next to it. The observed breeze there cannot happen. Value 0.
> With $p_{1,3}$ the third case becomes 1, because the pit in $[1,3]$ explains the breeze in $[1,2]$.

> [!question]- Check (slide 44): Repeat the computation of the slide with pit probability $0.1$ per square instead of $0.2$. Find $P(p_{1,3}\mid p^*,b^*)$.
> Fringe cases: (pit, pit) $0.1\cdot 0.1=0.01$, (pit, none) $0.1\cdot 0.9=0.09$, (none, pit) $0.09$, (none, none) $0.9\cdot 0.9=0.81$.
> For $p_{1,3}$ the values are 1, 1, 1, 0: sum $=0.01+0.09+0.09=0.19$.
> For $\neg p_{1,3}$ the values are 1, 1, 0, 0: sum $=0.01+0.09=0.10$.
> $\mathbf{P}(P_{1,3}\mid p^*,b^*)=\alpha'\,\langle 0.1\cdot 0.19,\ 0.9\cdot 0.10\rangle=\alpha'\,\langle 0.019,0.09\rangle$.
> $\alpha'=\dfrac{1}{0.019+0.09}=\dfrac{1}{0.109}\approx 9.17$.
> $P(p_{1,3}\mid p^*,b^*)=\dfrac{0.019}{0.109}\approx 0.174$ and $P(\neg p_{1,3}\mid p^*,b^*)\approx 0.826$.
> Pits are rarer, so the answer falls from $0.31$ to about $0.17$. It is still above the prior $0.1$, because the breeze in $[1,2]$ is evidence for a pit.

> [!question]- Check (slide 45): Derive the value $0.86$ for $[2,2]$. Use the same method with query $P_{2,2}$ and fringe $[1,3]$, $[3,1]$.
> Formula: $\mathbf{P}(P_{2,2}\mid p^*,b^*)=\alpha'\,\mathbf{P}(P_{2,2})\sum_{\mathit{fringe}}\mathbf{P}(b^*\mid p^*,P_{2,2},\mathit{fringe})\,P(\mathit{fringe})$.
> If $p_{2,2}$: the pit in $[2,2]$ explains both breezes, so all four fringe cases fit. Sum $=0.04+0.16+0.16+0.64=1$.
> If $\neg p_{2,2}$: the breeze in $[1,2]$ needs a pit in $[1,3]$ and the breeze in $[2,1]$ needs a pit in $[3,1]$. Only the case (pit, pit) fits. Sum $=0.2\cdot 0.2=0.04$.
> $\alpha'\,\langle 0.2\cdot 1,\ 0.8\cdot 0.04\rangle=\alpha'\,\langle 0.2,0.032\rangle$.
> $\alpha'=\dfrac{1}{0.2+0.032}=\dfrac{1}{0.232}\approx 4.31$.
> $P(p_{2,2}\mid p^*,b^*)=\dfrac{0.2}{0.232}\approx 0.862$ and $P(\neg p_{2,2}\mid p^*,b^*)\approx 0.138$.
> $[2,2]$ is so likely because one pit there explains both breezes. Without it, two pits are needed.

## 9. Summary (slide 46)
![[cmpe421_lecture_07_quantifyinguncertainty_p045.jpg]]

Slides not embedded: PDF page 3 (printed 4, outline), PDF page 46 (printed 47, homework notice).

> [!question]- Check (slide 46): The last line of the slide says that independence reduces the "combinatorial explosion". Show it with numbers for 30 Boolean variables: the full joint, 30 fully independent variables, and a naive Bayes model with 1 cause and 29 effects.
> Full joint: $2^{30}-1=1073741823$ free numbers (about one billion).
> Fully independent: one number per variable, so $30$.
> Naive Bayes: $1+2\cdot 29=59$.
> The full joint can answer every query by enumeration, but nobody can store or estimate a billion numbers. The two factored models are small, but they are only correct if the independence statements are true in the domain.

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
