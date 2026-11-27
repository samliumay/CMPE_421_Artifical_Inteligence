---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L07 - MDPs, probability, Bayes nets, HMMs
type: quiz
questions: 28
---

# CMPE421 L07 - MDPs, probability, Bayes nets, HMMs - quiz

Lesson note: [[CMPE421 L07 - MDPs, probability, Bayes nets, HMMs]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Distributions, joints, events, marginals (Ch.9, slides 1-15)

### Q1 (section 1)

A joint $P(A,B)$ has the rows $(+a,+b)\ 0.15$, $(+a,-b)\ 0.35$, $(-a,+b)\ 0.25$, $(-a,-b)\ 0.25$. Find the marginal $P(+b)$ and the probability of the event $+a$ OR $+b$.

- A) $P(+b)=0.6$ and $P(+a\text{ OR }+b)=0.15$.
- B) $P(+b)=0.6$ and $P(+a\text{ OR }+b)=0.75$.
- C) $P(+b)=0.4$ and $P(+a\text{ OR }+b)=0.75$.
- D) $P(+b)=0.4$ and $P(+a\text{ OR }+b)=0.15$.

> [!success]- Answer
> **C.** Marginal: sum the rows with $+b$: $P(+b)=0.15+0.25=0.4$.
> The event $+a$ OR $+b$ holds in the rows $(+a,+b)$, $(+a,-b)$, $(-a,+b)$. Count each once: $0.15+0.35+0.25=0.75$.
> Check: $1-P(-a,-b)=1-0.25=0.75$.
> - A: $0.6$ is the sum of the $-b$ rows, which is $P(-b)$. And $0.15$ is only the row where both hold, the AND event.
> - B: The OR value is right, but $0.6=0.35+0.25$ sums the $-b$ rows.
> - D: The marginal is right, but $0.15$ is $P(+a,+b)$, the AND event, not OR.

### Q2 (section 1, table size)

A model has 3 random variables, and each variable has 4 values. How many rows has the joint table, and how many of its numbers are free?

- A) 64 rows, 63 free numbers.
- B) 81 rows, 80 free numbers.
- C) 64 rows, 64 free numbers.
- D) 81 rows, 81 free numbers.

> [!success]- Answer
> **A.** With $n$ variables of $d$ values there are $d^n=4^3=64$ rows, one per full assignment.
> The rows sum to 1, so the last number is fixed by the others: $64-1=63$ free numbers.
> - B: $81=3^4$ swaps $d$ and $n$. The exponent is the number of variables.
> - C: The row count is right, but this forgets that the rows must sum to 1.
> - D: This swaps $d$ and $n$ and also forgets the sum-to-1 saving.

## 2. Conditional probability and normalisation (Ch.9, slides 16-23, 38-39)

### Q3 (section 2)

A joint $P(T,W)$ has the rows (hot, sun) 0.3, (hot, rain) 0.2, (cold, sun) 0.1, (cold, rain) 0.4. Find $P(\text{hot}\mid\text{rain})$ and $P(\text{sun}\mid\text{hot})$.

- A) $P(\text{hot}\mid\text{rain})=0.2$ and $P(\text{sun}\mid\text{hot})=0.6$.
- B) $P(\text{hot}\mid\text{rain})=\tfrac13$ and $P(\text{sun}\mid\text{hot})=0.75$.
- C) $P(\text{hot}\mid\text{rain})=0.2$ and $P(\text{sun}\mid\text{hot})=0.75$.
- D) $P(\text{hot}\mid\text{rain})=\tfrac13$ and $P(\text{sun}\mid\text{hot})=0.6$.

> [!success]- Answer
> **D.** Select the rain rows $(0.2,\ 0.4)$, $Z=0.6$, so $P(\text{hot}\mid\text{rain})=0.2/0.6=\tfrac13$.
> Select the hot rows $(0.3,\ 0.2)$, $Z=0.5$, so $P(\text{sun}\mid\text{hot})=0.3/0.5=0.6$.
> - A: $0.2$ is the joint $P(\text{hot},\text{rain})$. It was not divided by $Z=0.6$.
> - B: $0.75=0.3/0.4$ is $P(\text{hot}\mid\text{sun})$. The condition and the query are swapped.
> - C: This forgets to normalise the first value and swaps the second one.

### Q4 (section 2, find the mistake)

A student says: "The conditional table $P(W\mid T)$ for two weather values and two temperatures has four rows, and the four numbers sum to 1." What is wrong?

- A) The table has only two rows, one for each value of $W$.
- B) Each of the two distributions over $W$ sums to 1, so the four numbers sum to 2.
- C) For each value of $W$ the numbers over $T$ sum to 1, not over all four rows.
- D) The four numbers sum to 1 only when $T$ and $W$ are independent.

> [!success]- Answer
> **B.** $P(W\mid T)$ holds one distribution over $W$ for each value of $T$. Each one sums to 1. With two values of $T$ the four numbers sum to 2. Only a joint table sums to 1 over all its rows.
> - A: There is one number for each pair $(t,w)$, so the table has four rows.
> - C: This is the wrong direction. In $P(W\mid T)$ we sum over $W$ for a fixed $T$.
> - D: With independence $P(w\mid t)=P(w)$, and the four numbers still sum to $2$.

## 3. Inference by enumeration (Ch.9, slides 24-27)

### Q5 (section 3)

A joint $P(S,T,W)$ (season, temperature, weather) has the rows:
(summer, hot, sun) 0.30, (summer, hot, rain) 0.05, (summer, cold, sun) 0.10, (summer, cold, rain) 0.05,
(winter, hot, sun) 0.10, (winter, hot, rain) 0.05, (winter, cold, sun) 0.15, (winter, cold, rain) 0.20.
Find $P(\text{hot}\mid\text{summer})$ and $P(\text{summer}\mid\text{hot},\text{sun})$.

- A) $P(\text{hot}\mid\text{summer})=0.35$ and $P(\text{summer}\mid\text{hot},\text{sun})=0.75$.
- B) $P(\text{hot}\mid\text{summer})=0.7$ and $P(\text{summer}\mid\text{hot},\text{sun})=0.75$.
- C) $P(\text{hot}\mid\text{summer})=0.7$ and $P(\text{summer}\mid\text{hot},\text{sun})=0.615$.
- D) $P(\text{hot}\mid\text{summer})=0.35$ and $P(\text{summer}\mid\text{hot},\text{sun})=0.615$.

> [!success]- Answer
> **B.** First query: evidence $S=\text{summer}$, query $T$, hidden $W$. Sum out $W$: hot $0.30+0.05=0.35$, cold $0.10+0.05=0.15$. $Z=0.5$, so hot $0.35/0.5=0.7$.
> Second query: evidence $T=\text{hot}$, $W=\text{sun}$, nothing hidden. Rows $0.30$ and $0.10$, $Z=0.40$, so summer $0.30/0.40=0.75$.
> - A: $0.35$ is $P(\text{hot},\text{summer})$. It was not divided by $Z=0.5$.
> - C: $0.615=0.40/0.65$ is $P(\text{summer}\mid\text{sun})$. It sums out $T$ although $T=\text{hot}$ is evidence.
> - D: This forgets to normalise the first query and drops the evidence $\text{hot}$ in the second.

### Q6 (section 3, cost)

Enumeration on a full joint of 10 Boolean variables touches $2^{10}=1024$ rows in the worst case. How many rows does it touch for 20 Boolean variables, and what does this say about the method?

- A) 2048 rows, so the method also works well for large models.
- B) 1 048 576 rows, so the method also works well for large models.
- C) 2048 rows, so the method is practical only for small models.
- D) 1 048 576 rows, so the method is practical only for small models.

> [!success]- Answer
> **D.** The table has $d^n$ rows: $2^{20}=1\,048\,576$. Ten more variables make it about 1000 times bigger. Enumeration on the full joint is practical only for small models. This is the reason for Bayes' nets.
> - A: $2048=2\cdot 1024$ treats the cost as linear in $n$. Each extra variable doubles the table, it does not add a fixed amount.
> - B: The count is right, but a table that grows by a factor 1000 for ten more variables does not scale.
> - C: $2048$ is the linear-growth mistake. The conclusion is right, but not for this count.

## 4. Product rule, chain rule, Bayes' rule (Ch.9, slides 28-37)

### Q7 (section 4)

A disease has prior $P(+d)=0.02$. A test has $P(+t\mid +d)=0.9$ and $P(+t\mid -d)=0.1$. Find $P(+d\mid +t)$.

- A) $0.155$.
- B) $0.900$.
- C) $0.018$.
- D) $0.116$.

> [!success]- Answer
> **A.** Prior times likelihood: $0.9\cdot 0.02=0.018$ for $+d$ and $0.1\cdot 0.98=0.098$ for $-d$.
> $P(+t)=0.018+0.098=0.116$. Normalise: $P(+d\mid +t)=0.018/0.116\approx 0.155$.
> - B: $0.9$ is $P(+t\mid +d)$. This uses $P(e\mid h)$ for $P(h\mid e)$.
> - C: $0.018$ is $P(+d,+t)$. It was not divided by $P(+t)$.
> - D: $0.116$ is $P(+t)$, the normalising constant, not the posterior.

### Q8 (section 4, Ghostbusters)

The ghost is in one of three squares with prior $(0.5,\ 0.25,\ 0.25)$. The sensor reads red with $P(\text{red}\mid g)=(0.7,\ 0.2,\ 0.1)$ for the three squares. After a red reading, what is the posterior of square 1? And what is it with a uniform prior $(\tfrac13,\tfrac13,\tfrac13)$?

- A) Given prior $0.824$, uniform prior $0.233$.
- B) Given prior $0.35$, uniform prior $0.7$.
- C) Given prior $0.824$, uniform prior $0.7$.
- D) Given prior $0.35$, uniform prior $0.233$.

> [!success]- Answer
> **C.** Products: $0.5\cdot 0.7=0.35$, $0.25\cdot 0.2=0.05$, $0.25\cdot 0.1=0.025$. $Z=0.425$, so square 1 has $0.35/0.425\approx 0.824$.
> Uniform prior: the factor $\tfrac13$ cancels in the normalisation. The posterior is the sensor column normalised. The column sums to 1, so square 1 has $0.7$.
> - A: $0.233=0.7/3$ is the uniform product, not normalised.
> - B: $0.35$ is the product for square 1, not normalised by $Z=0.425$.
> - D: Both values skip the normalisation.

## 5. Independence and conditional independence (Ch.10.0, slides 1-14)

### Q9 (section 5)

A joint $P(X,Y)$ has the rows $(+x,+y)\ 0.12$, $(+x,-y)\ 0.28$, $(-x,+y)\ 0.18$, $(-x,-y)\ 0.42$. Are $X$ and $Y$ independent?

- A) No, because $P(+x,+y)=0.12$ differs from $P(+x)=0.4$.
- B) We cannot tell, because a joint does not show independence.
- C) No, because the marginals $P(+x)=0.4$ and $P(+y)=0.3$ differ.
- D) Yes, because every row equals the product of its two marginals.

> [!success]- Answer
> **D.** Marginals: $P(+x)=0.4$, $P(-x)=0.6$, $P(+y)=0.3$, $P(-y)=0.7$.
> Products: $0.4\cdot 0.3=0.12$, $0.4\cdot 0.7=0.28$, $0.6\cdot 0.3=0.18$, $0.6\cdot 0.7=0.42$. All four rows match, so $X\perp Y$.
> - A: Independence compares $P(x,y)$ with $P(x)P(y)$, not with $P(x)$ alone.
> - B: The joint holds all we need: check every row against the product of the marginals.
> - C: Independent variables can have any marginals. Equal or different marginals prove nothing.

### Q10 (section 5, Ghostbusters)

$P(+g)=0.5$, $P(+t\mid +g)=0.8$, $P(+t\mid -g)=0.4$, $P(+b\mid +g)=0.4$, $P(+b\mid -g)=0.8$, and $T\perp B\mid G$. Compute $P(+g\mid +t,-b)$.

- A) $\tfrac67\approx 0.857$.
- B) $0.5$.
- C) $0.24$.
- D) $\tfrac23\approx 0.667$.

> [!success]- Answer
> **A.** By $T\perp B\mid G$ each row is $P(G)\,P(T\mid G)\,P(B\mid G)$. Complements: $P(-b\mid +g)=0.6$, $P(-b\mid -g)=0.2$.
> $P(+t,-b,+g)=0.5\cdot 0.8\cdot 0.6=0.24$ and $P(+t,-b,-g)=0.5\cdot 0.4\cdot 0.2=0.04$.
> $Z=0.28$, so $P(+g\mid +t,-b)=0.24/0.28=\tfrac67\approx 0.857$.
> - B: This uses $P(+b\mid G)$ instead of $P(-b\mid G)$: the products are $0.16$ and $0.16$.
> - C: $0.24$ is the joint row, not divided by $Z=0.28$.
> - D: $0.4/0.6$ is $P(+g\mid +t)$. It ignores the evidence $-b$.

## 6. Bayes' net semantics (Ch.10.0, slides 15-30)

### Q11 (section 6)

Alarm net $B\to A\leftarrow E$, $A\to J$, $A\to M$. CPTs: $P(+b)=0.001$, $P(+e)=0.002$, $P(+a\mid -b,+e)=0.29$, $P(+j\mid +a)=0.9$, $P(+m\mid +a)=0.7$. Compute $P(-b,+e,+a,+j,-m)$.

- A) About $3.65\times 10^{-4}$.
- B) About $7.83\times 10^{-2}$.
- C) About $1.56\times 10^{-4}$.
- D) About $1.57\times 10^{-7}$.

> [!success]- Answer
> **C.** One factor per node, roots included: $P(-b)\,P(+e)\,P(+a\mid -b,+e)\,P(+j\mid +a)\,P(-m\mid +a)$.
> $=0.999\cdot 0.002\cdot 0.29\cdot 0.9\cdot 0.3\approx 1.56\times 10^{-4}$.
> - A: This uses $P(+m\mid +a)=0.7$ instead of $P(-m\mid +a)=0.3$.
> - B: $0.29\cdot 0.9\cdot 0.3$ leaves out the root factors $P(-b)P(+e)$.
> - D: This uses $P(+b)=0.001$ instead of $P(-b)=0.999$.

### Q12 (section 6, size)

A net has six Boolean nodes. $X_1$ and $X_2$ have no parents, $X_3$ has parents $X_1,X_2$, $X_4$ and $X_5$ each have parent $X_3$, and $X_6$ has parents $X_4,X_5$. How many free numbers does the net need, and how many does the full joint need?

- A) Net 28, full joint 64.
- B) Net 14, full joint 63.
- C) Net 28, full joint 63.
- D) Net 14, full joint 64.

> [!success]- Answer
> **B.** A Boolean node needs $2^{\text{number of parents}}$ free numbers, one per row of its CPT: $1+1+4+2+2+4=14$.
> The full joint has $2^6=64$ rows that sum to 1, so $63$ free numbers.
> - A: This stores both $P(+x\mid\dots)$ and $P(-x\mid\dots)$ in the net, and counts all 64 joint rows. It skips the sum-to-1 saving twice.
> - C: The joint is right, but the net count stores both values of every CPT row.
> - D: The net is right, but the last joint row is fixed by the others.

## 7. Causality (Ch.10.0, slides 31-34)

### Q13 (section 7)

A net $R\to T$ has $P(+r)=0.2$, $P(+t\mid +r)=0.9$, $P(+t\mid -r)=0.25$. Reverse it to $T\to R$. Find $P(+t)$ and $P(+r\mid +t)$ of the reversed net.

- A) $P(+t)=0.38$ and $P(+r\mid +t)\approx 0.474$.
- B) $P(+t)=0.38$ and $P(+r\mid +t)=0.9$.
- C) $P(+t)=0.575$ and $P(+r\mid +t)\approx 0.313$.
- D) $P(+t)=0.575$ and $P(+r\mid +t)=0.9$.

> [!success]- Answer
> **A.** Joint: $P(+r,+t)=0.2\cdot 0.9=0.18$, $P(-r,+t)=0.8\cdot 0.25=0.20$.
> Sum out $R$: $P(+t)=0.18+0.20=0.38$. Then $P(+r\mid +t)=0.18/0.38\approx 0.474$.
> - B: $0.9$ is $P(+t\mid +r)$. This uses $P(e\mid h)$ for $P(h\mid e)$.
> - C: $0.575=(0.9+0.25)/2$ averages the CPT without the prior weights $0.2$ and $0.8$. Then $0.18/0.575\approx 0.313$.
> - D: This makes both mistakes: the unweighted average and the swapped conditional.

### Q14 (section 7, find the mistake)

A student says: "The net has an arrow $T\to R$, so the model says that traffic causes rain." What is wrong?

- A) The arrow points the other way in meaning: it says that rain causes traffic.
- B) An arrow between $T$ and $R$ claims that they are conditionally independent.
- C) The claim is right only if $P(R\mid T)$ differs from $P(R)$ for some value.
- D) The arrow only says $P(R\mid T)$ is stored; $R\to T$ can hold the same joint.

> [!success]- Answer
> **D.** An arrow tells which conditional table is stored. The nets $R\to T$ and $T\to R$ can hold the same joint. A net claims conditional independences, read from the missing arcs. Two nodes with one arc have no missing arc, so no claim at all.
> - A: An arrow makes no causal claim in either direction.
> - B: Independence claims come from missing arcs, not from arcs.
> - C: Dependence between $T$ and $R$ does not show which one causes the other.

## 8. MDPs and policies (Ch.7, slides 1-16)

### Q15 (section 8, Grid World)

Grid World: an action goes in the intended direction with probability 0.8 and to each side (90 degrees left or right) with 0.1. If a wall blocks the move, the agent stays put. The agent is in a corner cell with walls to its North and West and a free cell to its East. It takes the action North. What is the distribution of the next state?

- A) Stay with $0.8$, move East with $0.1$, move South with $0.1$.
- B) Stay with $0.9$, move East with $0.1$.
- C) Stay with $0.8$, move East with $0.2$.
- D) Stay with $1.0$, because North is blocked.

> [!success]- Answer
> **B.** North (0.8): wall, stay. The sides of North are West and East (0.1 each). West: wall, stay. East: free, move.
> Stay $0.8+0.1=0.9$, East $0.1$.
> - A: South is the opposite of North, not a side. The agent never slips backwards.
> - C: This gives the blocked side's 0.1 to East. A blocked move means staying put.
> - D: This forgets the slips to the sides.

### Q16 (section 8, racing)

Racing MDP with three states (cool, warm, overheated). Overheated is terminal. Transitions and rewards:

| State | Slow | Fast |
|---|---|---|
| Cool | to cool (1.0), reward $+1$ | to cool (0.5) or warm (0.5), reward $+2$ |
| Warm | to cool (0.5) or warm (0.5), reward $+1$ | to overheated (1.0), reward $-10$ |

Start in cool. The policy is fast in cool and slow in warm. Find the probability of the state sequence cool, warm, cool, and the sum of rewards on it.

- A) Probability $1.0$, sum of rewards $1.5$.
- B) Probability $0.25$, sum of rewards $1.5$.
- C) Probability $0.25$, sum of rewards $3$.
- D) Probability $1.0$, sum of rewards $3$.

> [!success]- Answer
> **C.** Step 1: fast from cool goes to warm with 0.5, reward $+2$. Step 2: slow from warm goes to cool with 0.5, reward $+1$.
> Probability $0.5\cdot 0.5=0.25$. Sum of rewards $2+1=3$.
> By the Markov property step 2 depends only on being in warm, so the probabilities multiply.
> - A: $1.0=0.5+0.5$ adds the step probabilities. $1.5=0.5\cdot 2+0.5\cdot 1$ weights the rewards by probability, but on a fixed sequence each reward counts fully.
> - B: The probability is right, but the rewards on this sequence are not weighted.
> - D: The sum is right, but the probabilities of the two steps must be multiplied.

## 9. Discounting (Ch.7, slides 17-27)

### Q17 (section 9)

The utility of a reward sequence is $U([r_0,r_1,r_2])=r_0+\gamma r_1+\gamma^2 r_2$. With $\gamma=0.5$, find $U([2,4,8])$ and $U([8,4,2])$. Then give the upper bound on any utility when $R_{\max}=5$ and $\gamma=0.9$.

- A) $U([2,4,8])=6$, $U([8,4,2])=10.5$, bound $50$.
- B) $U([2,4,8])=3$, $U([8,4,2])=5.25$, bound $50$.
- C) $U([2,4,8])=6$, $U([8,4,2])=10.5$, bound $45$.
- D) $U([2,4,8])=3$, $U([8,4,2])=5.25$, bound $45$.

> [!success]- Answer
> **A.** $U([2,4,8])=2+0.5\cdot 4+0.25\cdot 8=6$. $U([8,4,2])=8+0.5\cdot 4+0.25\cdot 2=10.5$.
> Bound: $R_{\max}/(1-\gamma)=5/0.1=50$.
> - B: This discounts the first reward too ($\gamma r_0+\gamma^2 r_1+\dots$), which halves both values. The first reward is not discounted.
> - C: $45=5\cdot 0.9/0.1$ starts the geometric sum at $\gamma$ instead of $1$.
> - D: Both mistakes: every reward one power of $\gamma$ too late.

### Q18 (section 9, corridor)

A corridor has states $a,b,c,d,e$ in a row. Exiting at $a$ gives reward 10, exiting at $e$ gives reward 1, all other rewards are 0. Count one factor $\gamma$ per move before the exit reward, as for $d$: West is worth $10\gamma^3$ and East $1\cdot\gamma$. For state $c$ and $\gamma=0.5$, give the values of West and East. Is there any $\gamma$ with $0<\gamma\le 1$ for which $c$ prefers East?

- A) West $2.5$, East $0.25$. Yes, $c$ prefers East for $\gamma<1/\sqrt{10}$.
- B) West $1.25$, East $0.5$. Yes, $c$ prefers East for $\gamma<1/\sqrt{10}$.
- C) West $1.25$, East $0.5$. No, $c$ prefers West for every $\gamma$.
- D) West $2.5$, East $0.25$. No, $c$ prefers West for every $\gamma$.

> [!success]- Answer
> **D.** From $c$ both exits are two moves away. West $10\gamma^2=10\cdot 0.25=2.5$, East $1\cdot\gamma^2=0.25$.
> Both values have the same factor $\gamma^2$, so West is 10 times East for every $\gamma>0$. Only $d$ can switch.
> - A: The values are right, but the threshold $1/\sqrt{10}$ belongs to $d$, where the distances differ.
> - B: $10\gamma^3$ and $\gamma$ are the distances of $d$, not of $c$.
> - C: The conclusion is right, but the values use the distances of $d$.

## 10. Bellman equations and value iteration (Ch.7, slides 28-70)

### Q19 (section 10)

Racing MDP with the transitions and rewards of the table in Q16 above, with $\gamma=0.9$. Overheated is terminal with value 0. The update is $V_{k+1}(s)=\max_a\sum_{s'}T(s,a,s')\,[R(s,a,s')+\gamma V_k(s')]$. Start from $V_1=(2,1,0)$ for (cool, warm, overheated). Compute $V_2(\text{cool})$ and $V_2(\text{warm})$.

- A) $V_2(\text{cool})=3.5$ and $V_2(\text{warm})=2.5$.
- B) $V_2(\text{cool})=2.8$ and $V_2(\text{warm})=2.35$.
- C) $V_2(\text{cool})=3.35$ and $V_2(\text{warm})=2.35$.
- D) $V_2(\text{cool})=3.0$ and $V_2(\text{warm})=2.5$.

> [!success]- Answer
> **C.** Cool, slow: $1+0.9\cdot 2=2.8$. Cool, fast: $0.5\,(2+0.9\cdot 2)+0.5\,(2+0.9\cdot 1)=1.9+1.45=3.35$. Max $3.35$.
> Warm, slow: $0.5\,(1+0.9\cdot 2)+0.5\,(1+0.9\cdot 1)=1.4+0.95=2.35$. Warm, fast: $-10$. Max $2.35$.
> - A: This forgets $\gamma$ (uses $\gamma=1$): cool fast $3.5$, warm slow $2.5$.
> - B: $2.8$ is the slow q-value in cool. The max over actions picks fast.
> - D: This forgets $\gamma$ and also takes slow in cool: $1+2=3$.

### Q20 (section 10, fixed point)

Racing MDP with the transitions and rewards of the table in Q16 above, $\gamma=0.5$. To check that $V^*=(3.5,\ 2.5,\ 0)$ satisfies the Bellman equation, we put $V^*$ on the right-hand side. Fast in cool gives $Q(\text{cool},\text{fast})=3.5$. What does slow in cool give?

- A) $Q(\text{cool},\text{slow})=4.5$.
- B) $Q(\text{cool},\text{slow})=2.75$.
- C) $Q(\text{cool},\text{slow})=2.25$.
- D) $Q(\text{cool},\text{slow})=1.375$.

> [!success]- Answer
> **B.** Slow from cool stays in cool for sure, reward $+1$: $1.0\cdot(1+0.5\cdot 3.5)=2.75$.
> The max is $\max(2.75,\ 3.5)=3.5=V^*(\text{cool})$, so the cool value does not change. Warm gives $2.5$ the same way, so $V^*$ is a fixed point.
> - A: $1+3.5$ forgets $\gamma$.
> - C: $1+0.5\cdot 2.5$ uses $V^*(\text{warm})$, but slow from cool stays in cool.
> - D: $0.5\cdot(1+1.75)$ uses probability 0.5, but slow from cool is deterministic.

## 11. Policy evaluation, extraction, policy iteration (Ch.7, slides 71-107)

### Q21 (section 11, evaluation)

Racing MDP with the transitions and rewards of the table in Q16 above, $\gamma=0.5$. Evaluate the policy "always fast". It gives $V^\pi(\text{warm})=-10+0.5\cdot 0=-10$, and $V^\pi(\text{cool})=0.5\,[2+0.5\,V^\pi(\text{cool})]+0.5\,[2+0.5\,V^\pi(\text{warm})]$. Solve for $V^\pi(\text{cool})$.

- A) $V^\pi(\text{cool})=-6$.
- B) $V^\pi(\text{cool})=-0.5$.
- C) $V^\pi(\text{cool})=-2$.
- D) $V^\pi(\text{cool})=-\tfrac23$.

> [!success]- Answer
> **D.** Substitute $V^\pi(\text{warm})=-10$: $V^\pi(\text{cool})=1+0.25\,V^\pi(\text{cool})+1-2.5=-0.5+0.25\,V^\pi(\text{cool})$.
> So $0.75\,V^\pi(\text{cool})=-0.5$ and $V^\pi(\text{cool})=-\tfrac23\approx -0.667$.
> - A: This drops $\gamma$: $V=1+0.5V-4$ gives $V=-6$.
> - B: This puts $V^\pi(\text{cool})=0$ on the right-hand side. That is one sweep from zero, not the solution.
> - C: $-0.5/0.25$ divides by the coefficient on the right. Move it to the left first: $1-0.25=0.75$.

### Q22 (section 11, improvement)

Racing MDP with the transitions and rewards of the table in Q16 above, $\gamma=0.5$. The policy "always fast" has $V^\pi=(-\tfrac23,\ -10)$ for (cool, warm). Do one policy improvement step (one-step look-ahead with $V^\pi$). What is the new policy, and is it optimal? (The optimal policy is fast in cool, slow in warm.)

- A) Slow in cool, slow in warm. Not optimal yet.
- B) Fast in cool, slow in warm. Optimal after this step.
- C) Slow in cool, slow in warm. Optimal after this step.
- D) Fast in cool, slow in warm. Not optimal yet.

> [!success]- Answer
> **A.** Cool: slow $1+0.5\cdot(-\tfrac23)=0.667$, fast $0.5\,[2-\tfrac13]+0.5\,[2-5]=-0.667$. Slow wins.
> Warm: slow $0.5\,[1-\tfrac13]+0.5\,[1-5]=-1.667$, fast $-10$. Slow wins.
> New policy: always slow. This is not the optimal policy, so one more round is needed.
> - B: Fast in cool has q-value $-0.667<0.667$ under $V^\pi$, so the step picks slow.
> - C: The policy is right, but it differs from the optimal one. A changed policy is not yet a converged one.
> - D: The look-ahead picks slow in cool, and this policy would be the optimal one anyway.

## 12. Markov chains (Ch.11.0, slides 1-17)

### Q23 (section 12)

A weather chain has $P(\text{sun}\to\text{sun})=0.8$ and $P(\text{rain}\to\text{sun})=0.4$. Day 1 is rain. Day 2 has $P(\text{sun})=0.4$. Find $P(\text{sun})$ on day 3 with the mini-forward algorithm, and the stationary $P_\infty(\text{sun})$.

- A) Day 3: $0.32$. Stationary: $\tfrac23$.
- B) Day 3: $0.56$. Stationary: $\tfrac23$.
- C) Day 3: $0.32$. Stationary: $\tfrac13$.
- D) Day 3: $0.56$. Stationary: $\tfrac13$.

> [!success]- Answer
> **B.** Day 3: $0.8\cdot 0.4+0.4\cdot 0.6=0.32+0.24=0.56$.
> Stationary: $P_\infty(\text{sun})=0.8\,P_\infty(\text{sun})+0.4\,P_\infty(\text{rain})$, so $0.2\,P_\infty(\text{sun})=0.4\,P_\infty(\text{rain})$ and $P_\infty(\text{sun})=2\,P_\infty(\text{rain})=\tfrac23$.
> - A: $0.32$ keeps only the sun term. The sum over the previous state needs the rain term $0.4\cdot 0.6$ too.
> - C: Both mistakes: the rain term is missing, and the ratio is upside down.
> - D: $\tfrac13$ turns the ratio upside down. Leaving sun ($0.2$) is rarer than leaving rain ($0.4$), so sun is more common.

### Q24 (section 12, joint)

A weather chain has $P(\text{sun}\to\text{sun})=0.9$ and $P(\text{rain}\to\text{sun})=0.3$, with $P(X_1=\text{sun})=0.5$. Compute $P(X_1=\text{sun},X_2=\text{sun},X_3=\text{rain},X_4=\text{rain})$.

- A) $0.063$.
- B) $0.0135$.
- C) $0.0315$.
- D) $0.2835$.

> [!success]- Answer
> **C.** $P(X_1)\,P(X_2\mid X_1)\,P(X_3\mid X_2)\,P(X_4\mid X_3)=0.5\cdot 0.9\cdot 0.1\cdot 0.7=0.0315$.
> Sun to rain is $1-0.9=0.1$, rain to rain is $1-0.3=0.7$.
> - A: $0.9\cdot 0.1\cdot 0.7$ leaves out the initial factor $P(X_1=\text{sun})=0.5$.
> - B: This uses $0.3$ (rain to sun) for rain to rain.
> - D: This uses $0.9$ for sun to rain, but $0.9$ is sun to sun.

## 13. HMMs and filtering (Ch.11.0, slides 18-40)

### Q25 (section 13)

Umbrella HMM: $P(+r_{t+1}\mid +r_t)=0.7$, $P(+r_{t+1}\mid -r_t)=0.3$, $P(+u\mid +r)=0.9$, $P(+u\mid -r)=0.2$, and $P(R_1)=\langle 0.5,0.5\rangle$. Day 1 has no umbrella ($-u$), day 2 has one ($+u$). Find $B(R_1=+r)$ and $B(R_2=+r)$.

- A) $B(R_1=+r)\approx 0.111$ and $B(R_2=+r)\approx 0.703$.
- B) $B(R_1=+r)\approx 0.818$ and $B(R_2=+r)\approx 0.883$.
- C) $B(R_1=+r)\approx 0.111$ and $B(R_2=+r)=0.36$.
- D) $B(R_1=+r)\approx 0.818$ and $B(R_2=+r)\approx 0.953$.

> [!success]- Answer
> **A.** Day 1: $P(-u\mid +r)=0.1$, $P(-u\mid -r)=0.8$. Products $(0.05,\ 0.40)$, $Z=0.45$, $B(R_1=+r)=0.111$.
> Day 2, time update: $0.7\cdot 0.111+0.3\cdot 0.889=0.344$.
> Observation $+u$: $(0.344\cdot 0.9,\ 0.656\cdot 0.2)=(0.310,\ 0.131)$, $Z=0.441$, $B(R_2=+r)\approx 0.703$.
> - B: This uses $P(+u\mid R)$ on day 1 although no umbrella was seen. It is the filter for $+u,+u$.
> - C: This skips the time update on day 2 and applies $+u$ directly to $B(R_1)$.
> - D: This uses $+u$ on day 1 and also skips the time update.

### Q26 (section 13, time update)

Umbrella HMM with $P(+r_{t+1}\mid +r_t)=0.7$ and $P(+r_{t+1}\mid -r_t)=0.3$. The belief is $B(R_t=+r)=0.9$. No evidence arrives for the next three days. What is the belief in rain after the third day, and what does it tend to?

- A) After day 3 about $0.309$. It tends to $0.7$.
- B) After day 3 about $0.309$. It tends to $0.5$.
- C) After day 3 about $0.526$. It tends to $0.7$.
- D) After day 3 about $0.526$. It tends to $0.5$.

> [!success]- Answer
> **D.** Only time updates: $B'(+r)=0.7\,B(+r)+0.3\,B(-r)$.
> Day 1: $0.63+0.03=0.66$. Day 2: $0.462+0.102=0.564$. Day 3: $0.3948+0.1308=0.5256$.
> It tends to $0.5$, the stationary distribution: $p=0.7p+0.3(1-p)$ gives $p=0.5$.
> - A: $0.9\cdot 0.7^3\approx 0.309$ drops the term $0.3\,B(-r)$. And $0.7$ is the stay probability, not the stationary value.
> - B: The limit is right, but the day 3 value drops the term from $-r$.
> - C: The day 3 value is right, but $0.7$ is $P(+r\mid +r)$, not the fixed point of the update.

## 14. Forward algorithm and Viterbi (Ch.11.0, slides 41-56)

### Q27 (section 14, forward)

A forecast HMM has $B'(W_1)=\langle 0.5,0.5\rangle$ for (sun, rain) before the evidence. Sensor: $P(\text{bad}\mid\text{sun})=0.2$, $P(\text{bad}\mid\text{rain})=0.7$. The forecast for day 1 is "bad". Find $B(W_1=\text{sun})$ and $P(F_1=\text{bad})$.

- A) $B(W_1=\text{sun})=\tfrac29\approx 0.222$ and $P(F_1=\text{bad})=0.9$.
- B) $B(W_1=\text{sun})=\tfrac29\approx 0.222$ and $P(F_1=\text{bad})=0.45$.
- C) $B(W_1=\text{sun})=0.10$ and $P(F_1=\text{bad})=0.9$.
- D) $B(W_1=\text{sun})=0.10$ and $P(F_1=\text{bad})=0.45$.

> [!success]- Answer
> **B.** Unnormalised: sun $0.2\cdot 0.5=0.10$, rain $0.7\cdot 0.5=0.35$.
> Sum $0.45=P(F_1=\text{bad})$. Normalise: $B(W_1)=\langle 0.10/0.45,\ 0.35/0.45\rangle=\langle\tfrac29,\tfrac79\rangle$.
> - A: $0.9=0.2+0.7$ adds the sensor values without the weights $B'(W_1)$.
> - C: Both mistakes: no normalisation, and the unweighted sensor sum.
> - D: $0.45$ is right, but $0.10$ was not divided by it.

### Q28 (section 14, Viterbi)

Umbrella HMM: $P(+r_{t+1}\mid +r_t)=0.7$, $P(+r_{t+1}\mid -r_t)=0.3$, $P(+u\mid +r)=0.9$, $P(+u\mid -r)=0.2$, $P(R_1)=\langle 0.5,0.5\rangle$. Evidence $+u_1,-u_2$. Run Viterbi without normalising. Give the most likely weather sequence and its joint probability $P(x_{1:2},e_{1:2})$.

- A) $(+r,-r)$ with $0.164$.
- B) $(-r,-r)$ with $0.108$.
- C) $(+r,-r)$ with $0.108$.
- D) $(-r,-r)$ with $0.164$.

> [!success]- Answer
> **C.** First column: $m_{1:1}=\langle 0.5\cdot 0.9,\ 0.5\cdot 0.2\rangle=\langle 0.45,\ 0.10\rangle$.
> $m_{1:2}(+r)=0.1\cdot\max(0.7\cdot 0.45,\ 0.3\cdot 0.10)=0.0315$. $m_{1:2}(-r)=0.8\cdot\max(0.3\cdot 0.45,\ 0.7\cdot 0.10)=0.108$, back-pointer $+r$.
> Best final state $-r$, back-pointer $+r$: the sequence is $(+r,-r)$ with $0.108$.
> - A: $0.164=0.8\,(0.135+0.07)$ sums over the previous state. That is the forward value, not the best path.
> - B: This ignores the back-pointer and assumes the path stayed in $-r$. The path $(-r,-r)$ has only $0.10\cdot 0.7\cdot 0.8=0.056$.
> - D: Both mistakes: the sum instead of the max, and no back-pointer.
