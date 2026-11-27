---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L10 - Quantifying uncertainty
type: quiz
questions: 39
---

# CMPE421 L10 - Quantifying uncertainty - quiz

Lesson note: [[CMPE421 L10 - Quantifying uncertainty]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Acting under uncertainty (slides 2-6)

### Q1 (slide 2)

Action $A_t$ = leave for the airport $t$ minutes before the flight. A logical agent may only state rules that are certainly true. How does it fail on this problem, and which of the slide's four problems (partial observability, noisy sensors, uncertainty in action outcomes, complexity of modelling traffic) does a flat tire belong to?

- A) Its rules contradict each other, or it runs too slowly. A flat tire is partial observability.
- B) It risks falsehood, or its conclusion is too weak to act on. A flat tire is partial observability.
- C) It risks falsehood, or its conclusion is too weak to act on. A flat tire is uncertainty in action outcomes.
- D) Its rules contradict each other, or it runs too slowly. A flat tire is uncertainty in action outcomes.

> [!success]- Answer
> **C.** The short rule "$A_{25}$ will get me there on time" is not always true: it risks falsehood.
> The true rule needs an endless list of conditions (no accident, no rain, no flat tire, ...). The agent can never check them all, so its conclusion is too weak to act on.
> A flat tire means that the action "drive" does not always have the planned result. That is uncertainty in action outcomes.
> - A: Contradiction and speed are not the problem. And partial observability is about not seeing the road state, not about an action going wrong.
> - B: The two failures are right, but a flat tire is an outcome of the action, not something hidden from the sensors.
> - D: The flat-tire part is right, but a logical agent with only true rules does not contradict itself, and speed is not the issue.

### Q2 (slide 3)

The slide has the fudge-factor rules $\mathit{Sprinkler}\mapsto_{0.99}\mathit{WetGrass}$ and $\mathit{WetGrass}\mapsto_{0.7}\mathit{Rain}$. Use them like logic rules, one after the other, and multiply the strengths. The sprinkler is on. What strength do you get for $\mathit{Rain}$, and why is this result nonsense?

- A) 0.693; it is nonsense because the sprinkler already explains the wet grass, so the grass gives no reason for rain.
- B) 0.700; it is nonsense because chaining two rules should make $\mathit{Rain}$ more certain than one rule alone.
- C) 0.693; it is nonsense because chaining two rules should make $\mathit{Rain}$ more certain than one rule alone.
- D) 0.700; it is nonsense because the sprinkler already explains the wet grass, so the grass gives no reason for rain.

> [!success]- Answer
> **A.** Chain and multiply: $0.99\cdot 0.7=0.693$.
> So the system believes in rain with strength about $0.69$ only because the sprinkler is on. The sprinkler already explains the wet grass, so wet grass is no longer evidence for rain.
> A conditional probability sees this: $P(\mathit{rain}\mid\mathit{wetGrass},\mathit{sprinkler})$ differs from $P(\mathit{rain}\mid\mathit{wetGrass})$.
> - B: $0.7$ uses only the second rule. And a longer chain of uncertain rules does not make a conclusion more certain.
> - C: The number is right, but the reason is wrong: the problem is the explaining-away, not a too-low number.
> - D: The reason is right, but $0.7$ ignores the first rule's strength $0.99$.

### Q3 (slide 5)

The slide gives $P(A_{25}\mid\text{no reported accidents})=0.06$ and $P(A_{25}\mid\text{no reported accidents},\ 5\text{ a.m.})=0.15$. The road did not change between the two statements. Which number is correct?

- A) Only 0.06, because a probability may not change when the road does not change.
- B) Only 0.15, because the newer statement corrects the older one, which was a mistake.
- C) Neither, because a probability must be measured on the road, not taken from knowledge.
- D) Both, because they use different evidence; at 5 a.m. the agent must use 0.15.

> [!success]- Answer
> **D.** A subjective (Bayesian) probability describes a state of knowledge, not the road. The evidence after the bar is different, so the two numbers answer two different questions.
> $0.06$ is right for an agent that knows only "no reported accidents". $0.15$ is right for an agent that also knows it is 5 a.m. The agent uses the number conditioned on all it knows.
> - A: Probabilities change with new evidence even when the world stays the same.
> - B: $0.06$ was not a mistake. It is correct for the smaller evidence.
> - C: On the slides, probabilities are subjective: they relate a proposition to the agent's knowledge.

### Q4 (slide 6)

$P(\text{on time})$ is $0.04$ for $A_{25}$, $0.70$ for $A_{90}$, $0.95$ for $A_{120}$, $0.9999$ for $A_{1440}$. Missing the flight costs 100 points and each minute of leaving early costs 1 point: $EU(A_t)=-(1-P(\text{on time}))\cdot 100-t$. Which action has the maximum expected utility?

- A) $A_{25}$
- B) $A_{90}$
- C) $A_{120}$
- D) $A_{1440}$

> [!success]- Answer
> **B.** $A_{25}$: $-0.96\cdot 100-25=-121$. $A_{90}$: $-0.30\cdot 100-90=-120$.
> $A_{120}$: $-0.05\cdot 100-120=-125$. $A_{1440}$: $-0.0001\cdot 100-1440=-1440.01$.
> The maximum is $-120$, so $A_{90}$. With a cost of 1000 the best action was $A_{120}$: same probabilities, other preferences, other choice.
> - A: $-121$ is close, but one point below $-120$.
> - C: $A_{120}$ is the best action when missing the flight costs 1000, not 100.
> - D: $A_{1440}$ has the highest probability of being on time, but waiting 1440 minutes costs far more.

## 2. Probability basics (slides 7-9)

### Q5 (slide 7)

A loaded die has $P(1)=P(2)=P(3)=P(4)=\tfrac18$ and $P(5)=P(6)=\tfrac14$. Check that this is a valid probability model, then find $P(\text{roll}<4)$ and $P(\text{roll}\ge 5)$.

- A) $P(\text{roll}<4)=0.500$ and $P(\text{roll}\ge 5)=0.333$.
- B) $P(\text{roll}<4)=0.375$ and $P(\text{roll}\ge 5)=0.500$.
- C) $P(\text{roll}<4)=0.500$ and $P(\text{roll}\ge 5)=0.500$.
- D) $P(\text{roll}<4)=0.375$ and $P(\text{roll}\ge 5)=0.333$.

> [!success]- Answer
> **B.** Valid: every value is in $[0,1]$ and $4\cdot\tfrac18+2\cdot\tfrac14=1$.
> Event $\{1,2,3\}$: $\tfrac18+\tfrac18+\tfrac18=\tfrac38=0.375$.
> Event $\{5,6\}$: $\tfrac14+\tfrac14=0.500$.
> - A: These are the fair-die values $\tfrac36$ and $\tfrac26$. The die here is loaded.
> - C: $0.500$ for "roll $<4$" is the fair-die value (or it also counts the face 4).
> - D: $0.375$ is right, but $0.333=\tfrac26$ is the fair-die value for $\{5,6\}$.

### Q6 (slide 8)

Use the loaded die of Q5: $P(1)=P(2)=P(3)=P(4)=\tfrac18$, $P(5)=P(6)=\tfrac14$. Let $X(\omega)=\omega \bmod 3$ (the remainder after division by 3). Which vector is $\langle P(X=0),P(X=1),P(X=2)\rangle$?

- A) $\langle 0.375,\ 0.375,\ 0.250\rangle$
- B) $\langle 0.333,\ 0.333,\ 0.333\rangle$
- C) $\langle 0.250,\ 0.375,\ 0.375\rangle$
- D) $\langle 0.375,\ 0.250,\ 0.375\rangle$

> [!success]- Answer
> **D.** For each value, add the sample points that map to it.
> $X=0$ for $\{3,6\}$: $\tfrac18+\tfrac14=0.375$. $X=1$ for $\{1,4\}$: $\tfrac18+\tfrac18=0.250$. $X=2$ for $\{2,5\}$: $\tfrac18+\tfrac14=0.375$.
> Check: the three values sum to 1.
> - A: This reads the heavy faces as 3 and 4 instead of 5 and 6.
> - B: This is the fair die, where every class has two faces of $\tfrac16$.
> - C: This uses $(\omega-1)\bmod 3$, as if the faces were numbered 0 to 5.

### Q7 (slide 9)

An agent states $P(a)=0.4$, $P(b)=0.3$ and $P(a\lor b)=0.8$. Use $P(a\lor b)=P(a)+P(b)-P(a\land b)$ to find the overlap $P(a\land b)$ these beliefs imply. What does de Finetti's result say about this agent?

- A) $P(a\land b)=-0.1$. It can be offered bets that it accepts one by one and that lose money in every outcome.
- B) $P(a\land b)=0.1$. It can be offered bets that it accepts one by one and that lose money in every outcome.
- C) $P(a\land b)=0.1$. It loses money on average over many bets, but it can still win in some outcomes.
- D) $P(a\land b)=-0.1$. It loses money on average over many bets, but it can still win in some outcomes.

> [!success]- Answer
> **A.** $P(a\land b)=P(a)+P(b)-P(a\lor b)=0.4+0.3-0.8=-0.1$.
> A probability cannot be negative, so no probability model has these three values. The two circles together can cover at most $0.7$ of the box.
> De Finetti: such an agent accepts a set of bets that loses money whatever the outcome.
> - B: $0.1$ is $P(a\lor b)-P(a)-P(b)$, the sign is turned around.
> - C: Wrong sign, and de Finetti's loss is sure, not only on average.
> - D: The overlap is right, but the loss happens in every outcome, not only on average.

## 3. Propositions and distributions (slides 10-13)

### Q8 (slide 10)

Given $P(a\land b)=0.1$, $P(a\land\neg b)=0.3$ and $P(\neg a\land b)=0.2$. Find $P(a\lor b)$ and $P(\neg a\land\neg b)$.

- A) $P(a\lor b)=0.7$ and $P(\neg a\land\neg b)=0.3$.
- B) $P(a\lor b)=0.6$ and $P(\neg a\land\neg b)=0.3$.
- C) $P(a\lor b)=0.6$ and $P(\neg a\land\neg b)=0.4$.
- D) $P(a\lor b)=0.7$ and $P(\neg a\land\neg b)=0.4$.

> [!success]- Answer
> **C.** The three cases cannot be true together, so they add: $P(a\lor b)=0.2+0.3+0.1=0.6$.
> Check: $P(a)=0.4$, $P(b)=0.3$, and $0.4+0.3-0.1=0.6$.
> The four cases sum to 1: $P(\neg a\land\neg b)=1-0.6=0.4$.
> - A: $0.7=P(a)+P(b)$ forgets to subtract the overlap. Then $1-0.7=0.3$ carries the error on.
> - B: $0.6$ is right, but $0.3=1-P(a)-P(b)$ counts the overlap twice.
> - D: $0.4$ is right, but $0.7$ forgets to subtract the overlap $0.1$.

### Q9 (slide 11)

Someone defines $\mathit{Weather}$ with the values $\{\mathit{sunny},\mathit{rain},\mathit{warm}\}$. Which two requirements on the values of a random variable does this break? And what kind of random variable is $\mathit{Temp}$ in the proposition $\mathit{Temp}<22.0$?

- A) The values must be mutually exclusive and exhaustive. $\mathit{Temp}$ is a continuous variable.
- B) The values must be ordered and must be numbers. $\mathit{Temp}$ is a Boolean variable.
- C) The values must be ordered and must be numbers. $\mathit{Temp}$ is a continuous variable.
- D) The values must be mutually exclusive and exhaustive. $\mathit{Temp}$ is a Boolean variable.

> [!success]- Answer
> **A.** A day can be sunny and warm, so the values are not mutually exclusive. A cold cloudy day has none of them, so they are not exhaustive. Without both, the probabilities would not sum to 1.
> $\mathit{Temp}$ takes real values, so it is continuous. $\mathit{Temp}<22.0$ is a proposition (true or false), but the variable itself is not Boolean.
> - B: Discrete values need not be numbers or ordered ($\mathit{sunny}$ is neither). And the proposition is Boolean, not $\mathit{Temp}$.
> - C: The kind of $\mathit{Temp}$ is right, but "ordered and numeric" is not a requirement.
> - D: The two requirements are right, but this confuses the proposition $\mathit{Temp}<22.0$ with the variable.

### Q10 (slide 12)

The joint $\mathbf{P}(\mathit{Weather},\mathit{Cavity})$ of the slide:

| | sunny | rain | cloudy | snow |
|---|---|---|---|---|
| $\mathit{cavity}$ | 0.144 | 0.02 | 0.016 | 0.02 |
| $\neg\mathit{cavity}$ | 0.576 | 0.08 | 0.064 | 0.08 |

Find $P(\mathit{cavity}\lor\mathit{Weather}=\mathit{rain})$. How many of the 8 entries are free to choose?

- A) $P(\mathit{cavity}\lor\mathit{rain})=0.30$, and 8 entries are free.
- B) $P(\mathit{cavity}\lor\mathit{rain})=0.28$, and 8 entries are free.
- C) $P(\mathit{cavity}\lor\mathit{rain})=0.30$, and 7 entries are free.
- D) $P(\mathit{cavity}\lor\mathit{rain})=0.28$, and 7 entries are free.

> [!success]- Answer
> **D.** $P(\mathit{cavity})=0.144+0.02+0.016+0.02=0.2$ and $P(\mathit{rain})=0.02+0.08=0.1$.
> $P(\mathit{cavity}\lor\mathit{rain})=0.2+0.1-0.02=0.28$ (the first row plus the $0.08$ under it).
> The 8 entries must sum to 1, so 7 are free and the last is fixed.
> - A: $0.30=0.2+0.1$ counts the entry $0.02$ twice, and 8 forgets the sum-to-1 rule.
> - B: The probability is right, but the last entry is fixed by the other seven.
> - C: The count is right, but $0.30$ forgets to subtract the overlap $0.02$.

### Q11 (slide 13)

$X$ has the uniform density $U[18,26]$ (height $\tfrac18=0.125$ on $[18,26]$). Find $P(X=20)$. Then find the height of a Gaussian density $f(x)=\frac{1}{\sqrt{2\pi}\,\sigma}e^{-(x-\mu)^2/2\sigma^2}$ at $x=\mu$ for $\sigma=0.1$.

- A) $P(X=20)=0.125$ and the height is $0.399$.
- B) $P(X=20)=0.000$ and the height is $3.989$.
- C) $P(X=20)=0.125$ and the height is $3.989$.
- D) $P(X=20)=0.000$ and the height is $0.399$.

> [!success]- Answer
> **B.** A probability is an area. Over one point the width is 0, so $P(X=20)=0\cdot 0.125=0$.
> At $x=\mu$ the exponent is 0: $f(\mu)=\dfrac{1}{2.5066\cdot 0.1}\approx 3.989$.
> A density above 1 is allowed: it is probability per unit of $x$, and only the total area must be 1.
> - A: $0.125$ is the height, not a probability. And $0.399=1/\sqrt{2\pi}$ drops $\sigma$.
> - C: The height is right, but the height of a density is not the probability of a point.
> - D: $P(X=20)$ is right, but $0.399$ forgets to divide by $\sigma=0.1$.

## 4. Conditional probability, product rule, chain rule (slides 14-16)

### Q12 (slide 14)

$\mathit{Weather}$ has 4 values and $\mathit{Cavity}$ is Boolean. How many numbers of $\mathbf{P}(\mathit{Weather}\mid\mathit{Cavity})$ are free? Also give $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})$. (The dentist table gives $P(\mathit{cavity}\mid\mathit{toothache})=0.6$.)

- A) 7 numbers are free, and $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})=0.6$.
- B) 6 numbers are free, and $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})=0.6$.
- C) 6 numbers are free, and $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})=0.0$.
- D) 7 numbers are free, and $P(\mathit{cavity}\mid\mathit{toothache},\neg\mathit{cavity})=0.0$.

> [!success]- Answer
> **C.** The table has $4\cdot 2=8$ numbers. For each condition ($\mathit{cavity}$, $\neg\mathit{cavity}$) the 4 weather values sum to 1. So each group has $3$ free numbers: $2\cdot 3=6$.
> The evidence already says $\neg\mathit{cavity}$, so $P(\mathit{cavity}\mid\ldots,\neg\mathit{cavity})=0$.
> - A: 7 treats all 8 numbers as one distribution that sums to 1. And $0.6$ ignores the evidence $\neg\mathit{cavity}$.
> - B: The count is right, but $0.6$ drops $\neg\mathit{cavity}$ from the condition.
> - D: The value is right, but the 8 numbers sum to 2, not 1: there are two groups.

### Q13 (slide 15)

Given $P(\mathit{cavity})=0.2$, $P(\mathit{rain}\mid\mathit{cavity})=0.3$ and $P(\mathit{rain}\mid\neg\mathit{cavity})=0.1$. Find $P(\mathit{cavity}\mid\mathit{rain})$.

- A) $0.300$
- B) $0.060$
- C) $0.429$
- D) $0.375$

> [!success]- Answer
> **C.** Product rule: $P(\mathit{rain},\mathit{cavity})=0.3\cdot 0.2=0.06$ and $P(\mathit{rain},\neg\mathit{cavity})=0.1\cdot 0.8=0.08$.
> Add: $P(\mathit{rain})=0.14$.
> $P(\mathit{cavity}\mid\mathit{rain})=0.06/0.14=\tfrac37\approx 0.429$.
> - A: $0.3$ is $P(\mathit{rain}\mid\mathit{cavity})$, the condition the other way round.
> - B: $0.06$ is the joint $P(\mathit{rain},\mathit{cavity})$. It was not divided by $P(\mathit{rain})$.
> - D: $0.06/(0.06+0.1)$ uses $0.1$ as $P(\mathit{rain},\neg\mathit{cavity})$, without multiplying by $P(\neg\mathit{cavity})=0.8$.

### Q14 (slide 16)

Which is the chain rule for four variables $A,B,C,D$, and what is $P(a,b,c)$ for $P(a)=0.5$, $P(b\mid a)=0.4$, $P(c\mid a,b)=0.3$?

- A) $\mathbf{P}(A)\,\mathbf{P}(B\mid A)\,\mathbf{P}(C\mid A,B)\,\mathbf{P}(D\mid A,B,C)$, and $P(a,b,c)=0.06$.
- B) $\mathbf{P}(A)\,\mathbf{P}(B\mid A)\,\mathbf{P}(C\mid B)\,\mathbf{P}(D\mid C)$, and $P(a,b,c)=0.20$.
- C) $\mathbf{P}(A)\,\mathbf{P}(B\mid A)\,\mathbf{P}(C\mid B)\,\mathbf{P}(D\mid C)$, and $P(a,b,c)=0.06$.
- D) $\mathbf{P}(A)\,\mathbf{P}(B\mid A)\,\mathbf{P}(C\mid A,B)\,\mathbf{P}(D\mid A,B,C)$, and $P(a,b,c)=0.20$.

> [!success]- Answer
> **A.** Each variable is conditioned on all the variables before it. This is always true and needs no independence.
> $P(a,b,c)=0.5\cdot 0.4\cdot 0.3=0.06$.
> - B: Conditioning only on the previous variable is an extra (Markov) assumption, not the chain rule. And $0.20=0.5\cdot 0.4$ stops after two factors.
> - C: The number is right, but the formula drops variables from the conditions.
> - D: The formula is right, but $0.20$ is $P(a,b)$: the factor $P(c\mid a,b)$ is missing.

## 5. Inference by enumeration (slides 17-21)

### Q15 (slide 17)

Practice table over the Boolean variables $\mathit{Flu}$, $\mathit{Fever}$, $\mathit{Cough}$ (the eight entries sum to 1). Q16, Q17 and Q19 to Q21 use it too.

| | fever, cough | fever, $\neg$cough | $\neg$fever, cough | $\neg$fever, $\neg$cough |
|---|---|---|---|---|
| flu | 0.112 | 0.028 | 0.048 | 0.012 |
| $\neg$flu | 0.020 | 0.060 | 0.180 | 0.540 |

Find $P(\mathit{fever})$ and $P(\mathit{flu}\lor\mathit{fever})$ by adding atomic events.

- A) $P(\mathit{fever})=0.14$ and $P(\mathit{flu}\lor\mathit{fever})=0.42$.
- B) $P(\mathit{fever})=0.22$ and $P(\mathit{flu}\lor\mathit{fever})=0.42$.
- C) $P(\mathit{fever})=0.14$ and $P(\mathit{flu}\lor\mathit{fever})=0.28$.
- D) $P(\mathit{fever})=0.22$ and $P(\mathit{flu}\lor\mathit{fever})=0.28$.

> [!success]- Answer
> **D.** $P(\mathit{fever})$: the two fever columns, $0.112+0.028+0.020+0.060=0.22$.
> $P(\mathit{flu}\lor\mathit{fever})$: the whole flu row plus the fever entries of the $\neg$flu row: $0.2+0.020+0.060=0.28$.
> Check: $1-(0.180+0.540)=0.28$.
> - A: $0.14$ adds only the flu row's fever entries, which is $P(\mathit{flu},\mathit{fever})$. $0.42=0.2+0.22$ counts the overlap twice.
> - B: $P(\mathit{fever})$ is right, but $0.42=P(\mathit{flu})+P(\mathit{fever})$ counts the overlap $0.14$ twice.
> - C: The OR value is right, but $0.14$ is the AND event $P(\mathit{flu},\mathit{fever})$.

### Q16 (slide 18)

Use the practice table in Q15 above. The query is $\mathit{Flu}$ and the evidence is $\mathit{fever}$. Which variable is hidden, and what is $P(\mathit{flu}\mid\mathit{fever})$?

- A) $\mathit{Fever}$ is hidden, and $P(\mathit{flu}\mid\mathit{fever})=0.700$.
- B) $\mathit{Cough}$ is hidden, and $P(\mathit{flu}\mid\mathit{fever})=0.636$.
- C) $\mathit{Cough}$ is hidden, and $P(\mathit{flu}\mid\mathit{fever})=0.700$.
- D) $\mathit{Fever}$ is hidden, and $P(\mathit{flu}\mid\mathit{fever})=0.636$.

> [!success]- Answer
> **B.** Hidden = neither query nor evidence: $\mathit{Cough}$. Add over both of its values.
> $P(\mathit{flu},\mathit{fever})=0.112+0.028=0.14$ and $P(\neg\mathit{flu},\mathit{fever})=0.020+0.060=0.08$.
> $P(\mathit{flu}\mid\mathit{fever})=0.14/0.22=\tfrac{7}{11}\approx 0.636$.
> - A: $\mathit{Fever}$ is the evidence, not hidden. And $0.700=0.14/0.2$ is $P(\mathit{fever}\mid\mathit{flu})$, the condition the other way round.
> - C: The hidden variable is right, but $0.700$ divides by $P(\mathit{flu})$ instead of $P(\mathit{fever})$.
> - D: The number is right, but $\mathit{Fever}$ is observed, so it is the evidence.

### Q17 (slides 19-20)

Use the practice table in Q15 above. Compute $\mathbf{P}(\mathit{Flu}\mid\mathit{cough})=\alpha\,\mathbf{P}(\mathit{Flu},\mathit{cough})$. What is $P(\mathit{flu}\mid\mathit{cough})$, and which probability is $1/\alpha$?

- A) $P(\mathit{flu}\mid\mathit{cough})=0.160$, and $1/\alpha=P(\mathit{cough})=0.36$.
- B) $P(\mathit{flu}\mid\mathit{cough})=0.444$, and $1/\alpha=P(\mathit{flu})=0.20$.
- C) $P(\mathit{flu}\mid\mathit{cough})=0.160$, and $1/\alpha=P(\mathit{flu})=0.20$.
- D) $P(\mathit{flu}\mid\mathit{cough})=0.444$, and $1/\alpha=P(\mathit{cough})=0.36$.

> [!success]- Answer
> **D.** Sum out $\mathit{Fever}$: $\alpha\,[\langle 0.112,0.020\rangle+\langle 0.048,0.180\rangle]=\alpha\,\langle 0.16,0.20\rangle$.
> The entries must sum to 1: $\alpha=1/0.36\approx 2.78$.
> $P(\mathit{flu}\mid\mathit{cough})=0.16/0.36=\tfrac49\approx 0.444$. And $1/\alpha=0.36=P(\mathit{cough})$, the probability of the evidence.
> - A: $0.160$ is $P(\mathit{flu},\mathit{cough})$: it forgot to normalise.
> - B: The value is right, but $1/\alpha$ is the probability of the evidence $\mathit{cough}$, not of the query value.
> - C: It forgot to normalise, and $1/\alpha$ is not $P(\mathit{flu})$.

### Q18 (slide 21)

A model has 10 Boolean variables. The query has 1 variable and the evidence has 2. How many table entries are added for the whole answer (both values of the query)? How many entries would the full joint have with 3 values per variable?

- A) 256 entries are added; with 3 values the joint has 59049 entries.
- B) 128 entries are added; with 3 values the joint has 1000 entries.
- C) 256 entries are added; with 3 values the joint has 1000 entries.
- D) 128 entries are added; with 3 values the joint has 59049 entries.

> [!success]- Answer
> **A.** Hidden variables: $10-1-2=7$. One query value needs a sum over $2^7=128$ entries. Two query values: $2\cdot 128=256$.
> With $d=3$ and $n=10$: $d^n=3^{10}=59049$.
> - B: $128$ counts only one query value. $1000=10^3$ is $n^d$, the exponent is swapped.
> - C: The sum count is right, but the table size is $d^n$, not $n^d$.
> - D: The table size is right, but the query has two values, each with its own sum.

## 6. Independence and conditional independence (slides 22-24)

### Q19 (slide 22)

Use the practice table in Q15 above. Are $\mathit{Fever}$ and $\mathit{Cough}$ independent? Then add a variable $\mathit{Day}$ with 5 values that is independent of the other three. How many entries are left after the decomposition $\mathbf{P}(\mathit{Flu},\mathit{Fever},\mathit{Cough})\,\mathbf{P}(\mathit{Day})$?

- A) $\mathit{Fever}$ and $\mathit{Cough}$ are independent; 11 entries are left.
- B) $\mathit{Fever}$ and $\mathit{Cough}$ are not independent; 11 entries are left.
- C) $\mathit{Fever}$ and $\mathit{Cough}$ are not independent; 13 entries are left.
- D) $\mathit{Fever}$ and $\mathit{Cough}$ are independent; 13 entries are left.

> [!success]- Answer
> **C.** $P(\mathit{fever})=0.22$, $P(\mathit{cough})=0.112+0.048+0.020+0.180=0.36$, $P(\mathit{fever},\mathit{cough})=0.112+0.020=0.132$.
> $0.22\cdot 0.36=0.0792\ne 0.132$, so they are not independent. One failing pair is enough.
> Full joint: $2\cdot 2\cdot 2\cdot 5=40$ entries. Decomposed: $8+5=13$.
> - A: The product test fails. And 11 $=2+2+2+5$ treats all four variables as independent, which the decomposition does not say.
> - B: The test is right, but only $\mathit{Day}$ is split off: the three-variable table keeps its 8 entries.
> - D: The count is right, but $0.0792\ne 0.132$, so they are dependent.

### Q20 (slide 23)

Use the practice table in Q15 above. Find $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})$ and decide whether $\mathit{Cough}$ is conditionally independent of $\mathit{Fever}$ given $\mathit{Flu}$.

- A) $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=0.509$. Not conditionally independent, because $\mathit{Fever}$ and $\mathit{Cough}$ are dependent.
- B) $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=0.800$. Conditionally independent, because it equals $P(\mathit{cough}\mid\mathit{flu})$, and the same holds for $\neg\mathit{flu}$.
- C) $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=0.509$. Conditionally independent, because it equals $P(\mathit{cough}\mid\mathit{flu})$, and the same holds for $\neg\mathit{flu}$.
- D) $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=0.800$. Not conditionally independent, because $\mathit{Fever}$ and $\mathit{Cough}$ are dependent.

> [!success]- Answer
> **B.** Case $\mathit{flu}$: $P(\mathit{cough}\mid\mathit{fever},\mathit{flu})=0.112/0.14=0.8$ and $P(\mathit{cough}\mid\mathit{flu})=0.16/0.2=0.8$.
> Case $\neg\mathit{flu}$: $0.020/0.08=0.25$ and $0.20/0.8=0.25$.
> Equal in both cases, so knowing $\mathit{fever}$ does not change the belief in $\mathit{cough}$ once $\mathit{Flu}$ is known.
> - A: $0.509=0.112/0.22$ divides by $P(\mathit{fever})$ instead of $P(\mathit{fever},\mathit{flu})$. And dependence without $\mathit{Flu}$ says nothing about dependence given $\mathit{Flu}$.
> - C: The conclusion is right, but $0.509$ is not $P(\mathit{cough}\mid\mathit{flu})=0.8$, so it would not support it.
> - D: The value is right, but independence and conditional independence are different properties.

### Q21 (slide 24)

The slide writes:
$\mathbf{P}(T,Ca,C)\overset{1}{=}\mathbf{P}(T\mid Ca,C)\,\mathbf{P}(Ca,C)\overset{2}{=}\mathbf{P}(T\mid Ca,C)\,\mathbf{P}(Ca\mid C)\,\mathbf{P}(C)\overset{3}{=}\mathbf{P}(T\mid C)\,\mathbf{P}(Ca\mid C)\,\mathbf{P}(C)$,
with $T$ = Toothache, $Ca$ = Catch, $C$ = Cavity. Which step uses conditional independence? Then rebuild $P(\mathit{flu},\neg\mathit{fever},\mathit{cough})$ from $P(\mathit{flu})=0.2$, $P(\mathit{fever}\mid\mathit{flu})=0.7$, $P(\mathit{cough}\mid\mathit{flu})=0.8$, using the same factorisation.

- A) Only step 3 uses it; the entry is $0.048$.
- B) Steps 1 and 2 use it; the entry is $0.112$.
- C) Only step 3 uses it; the entry is $0.112$.
- D) Steps 1 and 2 use it; the entry is $0.048$.

> [!success]- Answer
> **A.** Steps 1 and 2 are the product rule and are always true. Step 3 replaces $\mathbf{P}(T\mid Ca,C)$ by $\mathbf{P}(T\mid C)$: that is conditional independence.
> Entry: $P(\mathit{flu})\,P(\neg\mathit{fever}\mid\mathit{flu})\,P(\mathit{cough}\mid\mathit{flu})=0.2\cdot 0.3\cdot 0.8=0.048$, the table entry.
> - B: Steps 1 and 2 need no assumption. And $0.112=0.2\cdot 0.7\cdot 0.8$ uses $P(\mathit{fever}\mid\mathit{flu})$ where $\neg\mathit{fever}$ is asked.
> - C: The step is right, but the entry needs $P(\neg\mathit{fever}\mid\mathit{flu})=1-0.7=0.3$.
> - D: The number is right, but the product rule needs no independence assumption.

## 7. Bayes' rule and naive Bayes (slides 25-26)

### Q22 (slide 25)

A disease has prior $P(d)=0.02$. A test has $P(\mathit{pos}\mid d)=0.9$ and $P(\mathit{pos}\mid\neg d)=0.05$. $P(\mathit{pos})$ is not given. Find $P(d\mid\mathit{pos})$.

- A) $0.900$
- B) $0.947$
- C) $0.269$
- D) $0.018$

> [!success]- Answer
> **C.** $P(\mathit{pos},d)=0.9\cdot 0.02=0.018$ and $P(\mathit{pos},\neg d)=0.05\cdot 0.98=0.049$.
> $P(\mathit{pos})=0.018+0.049=0.067$.
> $P(d\mid\mathit{pos})=0.018/0.067\approx 0.269$. The disease is rare, so healthy positives outnumber sick positives.
> - A: $0.9$ is $P(\mathit{pos}\mid d)$, the condition the other way round.
> - B: $0.9/(0.9+0.05)$ ignores the prior: it treats $d$ and $\neg d$ as equally likely.
> - D: $0.018$ is the joint $P(\mathit{pos},d)$. It forgot to normalise.

### Q23 (slide 26, part a)

Continue Q22 ($P(d)=0.02$, $P(\mathit{pos}_1\mid d)=0.9$, $P(\mathit{pos}_1\mid\neg d)=0.05$). A second test has $P(\mathit{pos}_2\mid d)=0.8$ and $P(\mathit{pos}_2\mid\neg d)=0.1$. The two tests are conditionally independent given $D$. Both are positive. Find $P(d\mid\mathit{pos}_1,\mathit{pos}_2)$.

- A) $0.269$
- B) $0.746$
- C) $0.014$
- D) $0.993$

> [!success]- Answer
> **B.** Naive Bayes: $\alpha\,\mathbf{P}(\mathit{pos}_1\mid D)\,\mathbf{P}(\mathit{pos}_2\mid D)\,\mathbf{P}(D)$.
> Entry for $d$: $0.9\cdot 0.8\cdot 0.02=0.0144$. Entry for $\neg d$: $0.05\cdot 0.1\cdot 0.98=0.0049$.
> $P(d\mid\mathit{pos}_1,\mathit{pos}_2)=0.0144/0.0193\approx 0.746$.
> - A: $0.269$ uses only the first test.
> - C: $0.014$ is the entry $0.0144$ before normalising.
> - D: $0.72/(0.72+0.005)$ multiplies the likelihoods but leaves out the prior $\langle 0.02,0.98\rangle$.

### Q24 (slide 26, part b)

A naive Bayes model has one Boolean cause and 20 Boolean effects. How many numbers does it need? How many does the full joint over the same variables need?

- A) Naive Bayes needs 40; the full joint needs 1048575.
- B) Naive Bayes needs 41; the full joint needs 1048575.
- C) Naive Bayes needs 40; the full joint needs 2097151.
- D) Naive Bayes needs 41; the full joint needs 2097151.

> [!success]- Answer
> **D.** Naive Bayes: 1 number for $\mathbf{P}(\mathit{Cause})$ and 2 for each effect: $1+2\cdot 20=41$.
> Full joint: 21 Boolean variables, $2^{21}-1=2097151$.
> - A: $40=2n$ forgets the cause's prior. $1048575=2^{20}-1$ forgets the cause variable.
> - B: The naive Bayes count is right, but the joint has 21 variables, not 20.
> - C: The joint is right, but $\mathbf{P}(\mathit{Cause})$ needs its own number.

## 8. Wumpus world: probabilistic reasoning (slides 27-45)

### Q25 (slide 27)

![[cmpe421_lecture_07_quantifyinguncertainty_p026.jpg]]

How many entries has the full table of the joint distribution on the slide? What do the three yellow squares have in common?

- A) $2^{16}=65536$ entries. The yellow squares are the squares that certainly contain a pit.
- B) $2^{19}=524288$ entries. The yellow squares are the squares that certainly contain a pit.
- C) $2^{19}=524288$ entries. The yellow squares are not visited and are next to a visited square.
- D) $2^{16}=65536$ entries. The yellow squares are not visited and are next to a visited square.

> [!success]- Answer
> **C.** Pit variables $P_{1,1},\dots,P_{4,4}$: 16. Breeze variables $B_{1,1},B_{1,2},B_{2,1}$: 3. So 19 Boolean variables and $2^{19}=524288$ entries.
> $[1,3]$, $[2,2]$, $[3,1]$ are not visited and border a visited square. The agent can move only there, and logic cannot prove any of them safe.
> - A: $2^{16}$ counts only the pit variables. And no square is known to have a pit: that is what the agent must work out.
> - B: The count is right, but the breezes only say that some neighbour has a pit, not which one.
> - D: The yellow squares are right, but the 3 breeze variables are part of the joint too.

### Q26 (slide 28)

Each square has a pit with probability $0.2$, independently. $\mathbf{P}(\text{breezes}\mid\text{pits})$ is 1 if the breezes match the pits and 0 otherwise. World A has pits exactly in $[2,2]$ and $[4,4]$. World B has a pit only in $[1,3]$. Neighbours: $[1,1]$: $[1,2],[2,1]$; $[1,2]$: $[1,1],[1,3],[2,2]$; $[2,1]$: $[1,1],[2,2],[3,1]$. Find the joint probability of each world (all 16 pit variables) together with the observed breezes $\neg b_{1,1},b_{1,2},b_{2,1}$.

- A) World A: $0.00176$. World B: $0.00000$.
- B) World A: $0.04000$. World B: $0.00704$.
- C) World A: $0.00176$. World B: $0.00704$.
- D) World A: $0.04000$. World B: $0.00000$.

> [!success]- Answer
> **A.** World A: $[2,2]$ explains both breezes, and $[1,1]$ has no pit next to it. Breeze term 1. Pit term $0.2^2\cdot 0.8^{14}\approx 0.00176$.
> World B: no neighbour of $[2,1]$ has a pit, but a breeze was observed there. Breeze term 0, so the product is 0.
> - B: $0.04=0.2^2$ leaves out the 14 squares without a pit. $0.00704=0.2\cdot 0.8^{15}$ ignores the breeze term.
> - C: World A is right, but World B does not explain the breeze in $[2,1]$, so its term is 0.
> - D: World B is right, but every one of the 16 squares gives a factor, also the 14 empty ones ($0.8$ each).

### Q27 (slide 29)

The 4x4 board has 3 visited squares and 1 query square $[1,3]$. All other squares are $\mathit{unknown}$. How many terms has the sum over $\mathit{unknown}$ for one value of $P_{1,3}$? How many on a 6x6 board with the same three visited squares?

- A) 4x4: 8192 terms. 6x6: $2^{35}$ terms.
- B) 4x4: 4096 terms. 6x6: $2^{32}$ terms.
- C) 4x4: 8192 terms. 6x6: $2^{32}$ terms.
- D) 4x4: 4096 terms. 6x6: $2^{35}$ terms.

> [!success]- Answer
> **B.** 4x4: $16-3-1=12$ unknown squares, each pit or no pit: $2^{12}=4096$.
> 6x6: $36-3-1=32$ unknown squares: $2^{32}\approx 4.3$ billion.
> - A: $8192=2^{13}$ forgets to remove the query square. $2^{35}$ removes only the query, not the visited squares.
> - C: The 6x6 count is right, but on 4x4 the query square is not in $\mathit{unknown}$.
> - D: The 4x4 count is right, but the 3 visited squares are known, not unknown.

### Q28 (slide 30)

![[cmpe421_lecture_07_quantifyinguncertainty_p026.jpg]]

The fringe is $[2,2]$ and $[3,1]$. The query is $[1,3]$. $b^*$ = the breeze facts of $[1,1]$, $[1,2]$, $[2,1]$. Is $[2,3]$ a fringe square or an *other* square? Why does a pit in $[4,4]$ not change $P(b^*\mid P_{1,3},p^*,\mathit{fringe})$?

- A) $[2,3]$ is fringe. A pit in $[4,4]$ has prior $0.2$, and this constant cancels in $\alpha$.
- B) $[2,3]$ is other. A pit in $[4,4]$ has prior $0.2$, and this constant cancels in $\alpha$.
- C) $[2,3]$ is fringe. $[4,4]$ is not a neighbour of any square in $b^*$, whose breezes depend only on neighbours.
- D) $[2,3]$ is other. $[4,4]$ is not a neighbour of any square in $b^*$, whose breezes depend only on neighbours.

> [!success]- Answer
> **D.** The neighbours of $[2,3]$ are $[1,3]$, $[3,3]$, $[2,2]$, $[2,4]$. None is visited, so $[2,3]$ is not next to an observed breeze: it is *other*.
> A breeze depends only on pits in its neighbours. The neighbours of $[1,1]$, $[1,2]$, $[2,1]$ are all known, query or fringe squares. Once those are fixed, $b^*$ is decided and $[4,4]$ adds nothing.
> - A: $[2,3]$ borders no visited square. And the term is about breezes, not about the prior of $[4,4]$.
> - B: The class is right, but the reason is wrong: the breeze term does not contain any prior.
> - C: The reason is right, but a fringe square must border a visited square.

### Q29 (slide 31)

The second line of the slide sums over all 12 unknown squares. The last line sums over the 2 fringe squares only. Counting both values of $P_{1,3}$, how many terms are added in each line?

- A) Second line: 8192 terms. Last line: 8 terms.
- B) Second line: 4096 terms. Last line: 4 terms.
- C) Second line: 8192 terms. Last line: 4 terms.
- D) Second line: 4096 terms. Last line: 8 terms.

> [!success]- Answer
> **A.** Second line: $2^{12}=4096$ terms for each value of $P_{1,3}$, so $2\cdot 4096=8192$.
> Last line: $2^2=4$ terms for each value, so $2\cdot 4=8$.
> The 10 *other* squares have left the formula: a saving by $2^{10}=1024$.
> - B: Both counts are for one value of $P_{1,3}$ only.
> - C: The second line counts both values, but the last line counts only one.
> - D: The last line counts both values, but the second line counts only one.

### Q30 (slides 32-33)

$\mathbf{P}(P_{1,3}\mid p^*,b^*)=\mathbf{P}(P_{1,3},p^*,b^*)/P(p^*,b^*)$. Why may $1/P(p^*,b^*)$ be written as a constant $\alpha$ when we do not know its value?

- A) Because the evidence was observed, so its probability is $P(p^*,b^*)=1$ and $\alpha=1$.
- B) Because $P(p^*,b^*)$ is close to 1, so $\alpha$ changes the result only a little.
- C) Because it does not contain $P_{1,3}$; it is the same for both values and is fixed by summing to 1.
- D) Because it equals one over the prior of the query, $1/P(p_{1,3})=1/0.2=5$, which is known in advance.

> [!success]- Answer
> **C.** $P(p^*,b^*)$ contains only the evidence, so it is one number, the same in the entry for $p_{1,3}$ and for $\neg p_{1,3}$.
> Dividing both by the same number keeps their ratio. Compute the two entries, then choose $\alpha$ so that they sum to 1.
> - A: Observed evidence is given, but its prior probability $P(p^*,b^*)$ is not 1 (it is about $0.119$).
> - B: It is not close to 1, and its size does not matter: it is removed by normalising.
> - D: $\alpha$ is $1/P(\text{evidence})$, not one over the prior of the query.

### Q31 (slides 34-35)

Slide 35 uses the product rule to write $\mathbf{P}(b^*\mid P_{1,3},p^*,\mathit{unknown})\,\mathbf{P}(P_{1,3},p^*,\mathit{unknown})$, with $b^*$ in front of the bar. Why $b^*$, and not for example $P_{1,3}$?

- A) Because $b^*$ is independent of the pits, so its factor can be dropped from the sum.
- B) Because given all 16 pit variables, the breeze term is 1 or 0 and can be read off directly.
- C) Because the product rule only allows evidence variables to stand in front of the bar.
- D) Because $b^*$ has only three variables, so its table is smaller than the table of $P_{1,3}$.

> [!success]- Answer
> **B.** The model of slide 28 gives the probability of the breezes given all the pits: 1 if they match, 0 if not.
> After slide 34, every term fixes all 16 pit variables ($P_{1,3}$, $p^*$, $\mathit{unknown}$). So the first factor needs no computation, and the second factor contains only pits: a product of $0.2$ and $0.8$ factors.
> - A: The breezes depend strongly on the pits. Only the *other* squares drop out later.
> - C: The product rule works with any variables in front of the bar.
> - D: Table size is not the reason. The reason is that this factor is known from the model.

### Q32 (slides 36-37)

On slide 37, $\mathit{other}$ is removed from the first factor $\mathbf{P}(b^*\mid P_{1,3},p^*,\mathit{fringe},\mathit{other})$, but not from the second factor $\mathbf{P}(P_{1,3},p^*,\mathit{fringe},\mathit{other})$. Why not from the second?

- A) Because the *other* squares are next to the breeze squares, so their pits affect the breezes $b^*$.
- B) Because conditional independence holds only for variables in front of the bar.
- C) Because removing it there would make the sum over $\mathit{other}$ equal to 0 instead of 1.
- D) Because the second factor is the prior of a full pit layout, and it depends on the *other* pits.

> [!success]- Answer
> **D.** Given the fringe, query and known squares, the breezes do not depend on $\mathit{other}$, so it leaves the first factor.
> The second factor is the probability of a whole pit layout. A layout with 3 pits among the *other* squares has a different probability than one with none. It is removed later (slide 41), when it sums to 1.
> - A: No *other* square touches a breeze square. That is why $\mathit{other}$ could leave the first factor.
> - B: The second factor has no bar at all. The reason is that it really depends on $\mathit{other}$.
> - C: The sum over $\mathit{other}$ of its probabilities is 1, not 0.

### Q33 (slides 38-39)

Each square has a pit with probability $0.2$, independently of all other squares. $p^*=\neg p_{1,1}\land\neg p_{1,2}\land\neg p_{2,1}$. Compute $P(p^*)$ and $P(\mathit{fringe})$ for the fringe case "pit in $[2,2]$, no pit in $[3,1]$".

- A) $P(p^*)=0.008$ and $P(\mathit{fringe})=0.040$.
- B) $P(p^*)=0.512$ and $P(\mathit{fringe})=0.040$.
- C) $P(p^*)=0.512$ and $P(\mathit{fringe})=0.160$.
- D) $P(p^*)=0.008$ and $P(\mathit{fringe})=0.160$.

> [!success]- Answer
> **C.** Independence lets us multiply: $P(p^*)=0.8\cdot 0.8\cdot 0.8=0.512$.
> Fringe case: $P(p_{2,2})\,P(\neg p_{3,1})=0.2\cdot 0.8=0.160$.
> - A: $0.008=0.2^3$ and $0.040=0.2^2$ use $0.2$ for "no pit" too.
> - B: $P(p^*)$ is right, but "no pit in $[3,1]$" has probability $0.8$, not $0.2$.
> - D: The fringe value is right, but $p^*$ says "no pit" three times: $0.8^3$.

### Q34 (slides 40-41)

Check $\sum_{\mathit{other}}P(\mathit{other})$ for a small case: only 2 *other* squares, each with pit probability $0.1$, independently. What is the sum over all pit layouts of these two squares?

- A) $1.00$
- B) $0.81$
- C) $0.20$
- D) $0.19$

> [!success]- Answer
> **A.** The four layouts: $0.1\cdot 0.1+0.1\cdot 0.9+0.9\cdot 0.1+0.9\cdot 0.9=0.01+0.09+0.09+0.81=1$.
> The layouts exclude each other and one of them must be true, so the sum is 1 for any pit probability and any number of squares.
> - B: $0.81$ is only the layout with no pit.
> - C: $0.1+0.1$ adds the two pit probabilities. That is not a sum over layouts.
> - D: $0.19$ leaves out the layout with no pit.

### Q35 (slide 42)

Slide 44 finds $\alpha'\approx 4.31$. Slide 42 defines $\alpha'=\alpha\,P(p^*)$ with $P(p^*)=0.512$ and $\alpha=1/P(p^*,b^*)$. Find $\alpha$ and $P(p^*,b^*)$.

- A) $\alpha=2.21$ and $P(p^*,b^*)=0.232$.
- B) $\alpha=8.42$ and $P(p^*,b^*)=0.119$.
- C) $\alpha=2.21$ and $P(p^*,b^*)=0.119$.
- D) $\alpha=8.42$ and $P(p^*,b^*)=0.232$.

> [!success]- Answer
> **B.** $\alpha=\alpha'/P(p^*)=4.3103/0.512\approx 8.42$.
> $P(p^*,b^*)=1/\alpha\approx 0.119$. Check: $0.512\cdot(0.072+0.16)=0.512\cdot 0.232\approx 0.119$.
> - A: $2.21=4.31\cdot 0.512$ multiplies where it should divide. $0.232=1/\alpha'$ uses $\alpha'$ instead of $\alpha$.
> - C: $P(p^*,b^*)$ is right, but $\alpha'=\alpha\,P(p^*)$ gives $\alpha=\alpha'/P(p^*)$.
> - D: $\alpha$ is right, but $0.232$ is $1/\alpha'$, not $1/\alpha$.

### Q36 (slide 43)

The breeze in $[1,2]$ needs a pit in one of its neighbours $[1,1]$, $[1,3]$, $[2,2]$. The breeze in $[2,1]$ needs a pit in one of $[1,1]$, $[2,2]$, $[3,1]$. $[1,1]$ has no pit. Take $\neg p_{1,3}$. What is $P(b^*\mid\ldots)$ in the fringe case "pit in $[2,2]$ only", and in the case "pit in $[3,1]$ only"?

- A) Pit in $[2,2]$ only: 0. Pit in $[3,1]$ only: 1.
- B) Pit in $[2,2]$ only: 1. Pit in $[3,1]$ only: 1.
- C) Pit in $[2,2]$ only: 0. Pit in $[3,1]$ only: 0.
- D) Pit in $[2,2]$ only: 1. Pit in $[3,1]$ only: 0.

> [!success]- Answer
> **D.** Pit in $[2,2]$ only: $[2,2]$ is next to both $[1,2]$ and $[2,1]$, so both breezes are explained. Value 1.
> Pit in $[3,1]$ only: the breeze in $[2,1]$ is explained, but $[1,2]$ has no pit next to it ($\neg p_{1,3}$, no pit in $[2,2]$). Value 0.
> - A: This swaps the two cases. $[3,1]$ is not a neighbour of $[1,2]$.
> - B: The second case would be 1 only with $p_{1,3}$, which explains the breeze in $[1,2]$.
> - C: One pit in $[2,2]$ is enough for both breezes.

### Q37 (slide 44)

Repeat slide 44 with pit probability $0.1$ per square instead of $0.2$. Fringe cases (pit in $[2,2]$?, pit in $[3,1]$?): (pit, pit) $0.01$, (pit, none) $0.09$, (none, pit) $0.09$, (none, none) $0.81$. With $p_{1,3}$ the cases (pit, pit), (pit, none), (none, pit) fit the breezes. With $\neg p_{1,3}$ only (pit, pit) and (pit, none) fit. Find $P(p_{1,3}\mid p^*,b^*)$.

- A) $0.174$
- B) $0.655$
- C) $0.019$
- D) $0.310$

> [!success]- Answer
> **A.** For $p_{1,3}$: $0.01+0.09+0.09=0.19$. For $\neg p_{1,3}$: $0.01+0.09=0.10$.
> Multiply by the prior: $\alpha'\,\langle 0.1\cdot 0.19,\ 0.9\cdot 0.10\rangle=\alpha'\,\langle 0.019,0.09\rangle$.
> $P(p_{1,3}\mid p^*,b^*)=0.019/0.109\approx 0.174$.
> - B: $0.19/(0.19+0.10)$ leaves out the prior $\mathbf{P}(P_{1,3})=\langle 0.1,0.9\rangle$.
> - C: $0.019$ is the entry before normalising.
> - D: $0.31$ is the answer for pit probability $0.2$, not $0.1$.

### Q38 (slide 45)

Find $P(p_{2,2}\mid p^*,b^*)$ with pit probability $0.2$. Use the query $P_{2,2}$ and the fringe $[1,3]$, $[3,1]$. The breeze in $[1,2]$ needs a pit in $[1,3]$ or $[2,2]$; the breeze in $[2,1]$ needs a pit in $[2,2]$ or $[3,1]$. Fringe case probabilities: (pit, pit) $0.04$, (pit, none) $0.16$, (none, pit) $0.16$, (none, none) $0.64$.

- A) $0.200$
- B) $0.410$
- C) $0.862$
- D) $0.962$

> [!success]- Answer
> **C.** If $p_{2,2}$: both breezes are explained, all four cases fit, sum $=1$.
> If $\neg p_{2,2}$: both $[1,3]$ and $[3,1]$ need a pit, only (pit, pit) fits, sum $=0.04$.
> $\alpha'\,\langle 0.2\cdot 1,\ 0.8\cdot 0.04\rangle=\alpha'\,\langle 0.2,0.032\rangle$, so $0.2/0.232\approx 0.862$.
> - A: $0.2$ is the prior, and also the entry for $p_{2,2}$ before normalising.
> - B: $0.2/(0.2+0.8\cdot 0.36)$ copies the sum $0.36$ from the query $P_{1,3}$, where three cases fit.
> - D: $1/(1+0.04)$ leaves out the prior $\langle 0.2,0.8\rangle$.

## 9. Summary (slide 46)

### Q39 (slide 46)

There are 30 Boolean variables. The full joint needs $2^{30}-1=1073741823$ free numbers. How many numbers are needed if all 30 variables are fully independent, and how many for a naive Bayes model with 1 cause and 29 effects?

- A) Fully independent: 60. Naive Bayes: 58.
- B) Fully independent: 30. Naive Bayes: 59.
- C) Fully independent: 60. Naive Bayes: 59.
- D) Fully independent: 30. Naive Bayes: 58.

> [!success]- Answer
> **B.** Fully independent: each Boolean variable needs one number, $P(x_i)$. Total 30.
> Naive Bayes: 1 for the cause and 2 per effect: $1+2\cdot 29=59$.
> - A: $60$ stores both $P(x_i)$ and $P(\neg x_i)$, but the second is $1$ minus the first. $58=2\cdot 29$ forgets the cause's number.
> - C: Naive Bayes is right, but one number per Boolean variable is enough.
> - D: Independent is right, but the cause $\mathbf{P}(\mathit{Cause})$ needs its own number.
