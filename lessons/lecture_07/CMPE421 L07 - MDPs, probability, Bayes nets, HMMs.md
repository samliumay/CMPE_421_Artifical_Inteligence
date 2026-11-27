---
course: CMPE 421 Artificial Intelligence
lecture: 7
title: MDPs, probability, Bayes nets, HMMs
tags: [cmpe421, lecture]
slides: lectures/lecture_07/Ch.7.pptx
full_notes: lecture_notes/lecture_07/CMPE421_Lecture_07_MDPs_Probability_BayesNets_HMMs.pdf
---
# CMPE 421 · Lecture 7 · MDPs, probability, Bayes' nets, HMMs

> [!info] No slide images in this note
> The four decks of this lecture exist only as PowerPoint files in `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/`: `Ch.9.pptx` (Probability, 39 slides), `Ch.10.0.pptx` (Bayes' nets, 34), `Ch.7.pptx` (MDPs, 107), `Ch.11.0.pptx` (HMMs, 56). No slide pictures are embedded. Slide numbers below are the ones our lecture notes give, and always refer to the named deck.
> Order here: probability, Bayes' nets, MDPs, HMMs (Moodle lists MDP first). Everything else uses probability.

> [!summary] In short
> - **Probability**: a joint table answers any query. Select the evidence rows, sum out the hidden variables, normalise.
> - **Bayes' rule**: posterior $\propto$ likelihood $\times$ prior.
> - **Bayes' net**: a DAG plus one table $P(X\mid \text{parents}(X))$ per node. The joint is the product of these tables.
> - **MDP**: states, actions, transition $T(s,a,s')$, reward $R(s,a,s')$, discount $\gamma$. Solved by value iteration or policy iteration (Bellman equations).
> - **HMM**: a hidden Markov chain plus a sensor table. Filtering alternates a time update and an observation update. Viterbi finds the most likely state sequence.

---
## Part A. Probability (deck Ch.9)

## 1. Distributions, joints, events, marginals (Ch.9, slides 1-15)

**Notes.** A random variable (capital letter) has a domain of values (lower case). $+r$ means $R=\text{true}$. A **distribution** is a table with $P(x)\ge 0$ and $\sum_x P(x)=1$. A **joint distribution** gives one number to each full assignment. With $n$ variables of $d$ values it has $d^n$ rows.

An **event** is a set of rows. Its probability is the sum of those rows. A **marginal** sums a variable out: $P(t)=\sum_w P(t,w)$.

> [!example] Running table $P(T,W)$
> (hot, sun) 0.4, (hot, rain) 0.1, (cold, sun) 0.2, (cold, rain) 0.3.
> $P(\text{hot})=0.5$. $P(\text{hot OR sun})=0.4+0.1+0.2=0.7$. Marginals: $P(T)=(0.5,0.5)$, $P(W)=(0.6,0.4)$.

> [!warning] Watch out
> For OR, list each row once. $P(\text{hot})+P(\text{sun})=1.1$ counts (hot, sun) twice.

> [!question]- Check (section 1): A joint $P(A,B)$ has rows $(+a,+b)\ 0.15$, $(+a,-b)\ 0.35$, $(-a,+b)\ 0.25$, $(-a,-b)\ 0.25$. Find both marginals and $P(+a\text{ OR }+b)$.
> Marginal of $A$: $P(+a)=0.15+0.35=0.5$, $P(-a)=0.25+0.25=0.5$.
> Marginal of $B$: $P(+b)=0.15+0.25=0.4$, $P(-b)=0.35+0.25=0.6$.
> Event $+a$ OR $+b$: the rows $(+a,+b)$, $(+a,-b)$, $(-a,+b)$, each once: $0.15+0.35+0.25=0.75$.
> Second way: $1-P(-a,-b)=1-0.25=0.75$.

> [!question]- Check (section 1, table size): A model has 3 random variables and each has 4 values. How many rows has the joint table, and how many of its numbers are free?
> Rows: $d^n=4^3=64$, one for each full assignment.
> Free numbers: $64-1=63$. The rows must sum to 1, so the last one is fixed by the others.

## 2. Conditional probability and normalisation (Ch.9, slides 16-23, 38-39)

**Notes.**

$$P(a\mid b)=\frac{P(a,b)}{P(b)}$$

**Normalisation trick**: select the rows that match the evidence, then divide them by their sum $Z$. The sum $Z$ is $P(\text{evidence})$, so it never has to be known in advance.

> [!example] $P(W\mid T=\text{cold})$
> Select $(0.2, 0.3)$, $Z=0.5$, result sun $0.4$, rain $0.6$. Also $P(T\mid \text{rain})=(0.25,0.75)$.

> [!warning] Watch out
> $P(a\mid b)\ne P(b\mid a)$ in general. A conditional table $P(D\mid W)$ sums to 1 for each value of $W$, not over all its rows.

> [!question]- Check (section 2): A joint $P(T,W)$ has rows (hot, sun) 0.3, (hot, rain) 0.2, (cold, sun) 0.1, (cold, rain) 0.4. Find $P(T\mid \text{rain})$ and $P(W\mid\text{hot})$ with the normalisation trick.
> $P(T\mid\text{rain})$: select the rain rows $(0.2,\ 0.4)$. $Z=0.2+0.4=0.6$. Divide: hot $0.2/0.6=\tfrac13$, cold $0.4/0.6=\tfrac23$.
> $P(W\mid\text{hot})$: select the hot rows $(0.3,\ 0.2)$. $Z=0.5$. Divide: sun $0.3/0.5=0.6$, rain $0.2/0.5=0.4$.
> In both cases $Z$ is the probability of the evidence: $P(\text{rain})=0.6$ and $P(\text{hot})=0.5$.

> [!question]- Check (section 2, find the mistake): "The conditional table $P(W\mid T)$ for two weather values and two temperatures has four rows, and the four numbers sum to 1." What is wrong?
> The table holds one distribution over $W$ for each value of $T$. Each of them sums to 1.
> With two values of $T$ the four numbers sum to 2. Only a joint table $P(T,W)$ sums to 1 over all its rows.

## 3. Inference by enumeration (Ch.9, slides 24-27)

**Notes.** Split the variables into evidence $E=e$, query $Q$ and hidden $H$. To get $P(Q\mid e)$:
1. **Select** the rows consistent with the evidence.
2. **Sum out** the hidden variables: $P(Q,e)=\sum_h P(Q,h,e)$.
3. **Normalise**: divide by $Z=\sum_q P(q,e)$.

Cost: time and space $O(d^n)$. Slide 26 answers: $P(W)=(0.65,0.35)$, $P(W\mid\text{winter})=(0.5,0.5)$, $P(W\mid\text{winter},\text{hot})=(\tfrac23,\tfrac13)$.

> [!info] The table of slide 26 and the first two answers, step by step
> The joint $P(S,T,W)$ (season, temperature, weather) has $2^3=8$ rows:
>
> | $S$ | $T$ | $W$ | $P$ |
> |---|---|---|---|
> | summer | hot | sun | 0.30 |
> | summer | hot | rain | 0.05 |
> | summer | cold | sun | 0.10 |
> | summer | cold | rain | 0.05 |
> | winter | hot | sun | 0.10 |
> | winter | hot | rain | 0.05 |
> | winter | cold | sun | 0.15 |
> | winter | cold | rain | 0.20 |
>
> $P(W)$: no evidence, hidden $S,T$. Sum the four sun rows: $0.30+0.10+0.10+0.15=0.65$. Sum the four rain rows: $0.05+0.05+0.05+0.20=0.35$. $Z=1$.
> $P(W\mid\text{winter})$: evidence $S=\text{winter}$, hidden $T$. Select the four winter rows. Sum out $T$: sun $0.10+0.15=0.25$, rain $0.05+0.20=0.25$. $Z=0.5$, so $(0.5,\ 0.5)$.

> [!warning] Watch out
> Normalise once, at the end, over the query variable. Never normalise slices separately before summing.

> [!question]- Check (section 3): Use the table of slide 26. Name the evidence, query and hidden variables for $P(T\mid\text{summer})$, then compute it. Then compute $P(S\mid\text{hot},\text{sun})$.
> $P(T\mid\text{summer})$: evidence $S=\text{summer}$, query $T$, hidden $W$.
> Select the four summer rows. Sum out $W$: hot $0.30+0.05=0.35$, cold $0.10+0.05=0.15$.
> Normalise: $Z=0.35+0.15=0.5$. Hot $0.35/0.5=0.7$, cold $0.15/0.5=0.3$.
> $P(S\mid\text{hot},\text{sun})$: evidence $T=\text{hot}$, $W=\text{sun}$, query $S$, nothing hidden.
> Select (summer, hot, sun) $0.30$ and (winter, hot, sun) $0.10$. $Z=0.40$. Summer $0.30/0.40=0.75$, winter $0.10/0.40=0.25$.

> [!question]- Check (section 3, cost): How many rows does enumeration touch in the worst case for 10 Boolean variables, and for 20? What does this say about the method?
> $d^n$ with $d=2$: $2^{10}=1024$ rows and $2^{20}=1\,048\,576$ rows.
> Ten more variables make the table about 1000 times bigger. Enumeration on the full joint is only practical for small models. This is the reason for Bayes' nets in Part B.

## 4. Product rule, chain rule, Bayes' rule (Ch.9, slides 28-37)

**Notes.**

$$
\begin{aligned}
\text{Product rule:}\quad & P(x,y)=P(y)\,P(x\mid y)\\
\text{Chain rule:}\quad & P(x_1,\dots,x_n)=\prod_{i=1}^{n}P(x_i\mid x_1,\dots,x_{i-1})\\
\text{Bayes' rule:}\quad & P(x\mid y)=\frac{P(y\mid x)\,P(x)}{P(y)},\qquad P(y)=\sum_x P(y\mid x)P(x)
\end{aligned}
$$

In practice: multiply prior by likelihood for every hypothesis, then normalise.

> [!example] Meningitis (slide 33)
> $P(+m)=0.0001$, $P(+s\mid +m)=0.8$, $P(+s\mid -m)=0.01$.
> $P(+s)=0.8\cdot 0.0001+0.01\cdot 0.9999=0.010079$. $P(+m\mid +s)=0.00008/0.010079\approx 0.0079$. Still below 1% because the prior is tiny.

> [!example] Bayes quiz (slide 34)
> $P(W)=(0.8,0.2)$ for sun, rain. $P(\text{dry}\mid\text{sun})=0.9$, $P(\text{dry}\mid\text{rain})=0.3$.
> Products $0.72$ and $0.06$, $P(\text{dry})=0.78$, so $P(W\mid\text{dry})=(\tfrac{12}{13},\tfrac{1}{13})\approx(0.923,0.077)$.

Ghostbusters (slides 35-37): $P(g\mid r)\propto P(r\mid g)P(g)$. With a uniform prior the posterior is the sensor column, normalised.

> [!warning] Errors on the slides
> - Slide 33 uses $P(-m)=0.999$. The correct value is $0.9999$. The answer still rounds to $0.0079$.
> - Slides 29, 34 and 39 head a weather column "R". It means $W$.

> [!question]- Check (section 4): A disease has prior $P(+d)=0.02$. A test has $P(+t\mid +d)=0.9$ and $P(+t\mid -d)=0.1$. Find $P(+d\mid +t)$.
> Prior times likelihood for both hypotheses:
> $P(+t\mid +d)\,P(+d)=0.9\cdot 0.02=0.018$.
> $P(+t\mid -d)\,P(-d)=0.1\cdot 0.98=0.098$.
> Sum: $P(+t)=0.018+0.098=0.116$.
> Normalise: $P(+d\mid +t)=0.018/0.116\approx 0.155$.
> The test is right 90% of the time, yet a positive result gives only about 16%. The healthy group is 49 times larger, so its false positives outnumber the true positives.

> [!question]- Check (section 4, Ghostbusters): The ghost is in one of three squares with prior $(0.5,\ 0.25,\ 0.25)$. The sensor reads red with $P(\text{red}\mid g)=(0.7,\ 0.2,\ 0.1)$ for the three squares. Find the posterior after a red reading. What would it be with a uniform prior?
> Products prior times likelihood: $0.5\cdot 0.7=0.35$, $0.25\cdot 0.2=0.05$, $0.25\cdot 0.1=0.025$.
> $Z=0.35+0.05+0.025=0.425$.
> Posterior: $0.35/0.425\approx 0.824$, $0.05/0.425\approx 0.118$, $0.025/0.425\approx 0.059$.
> Uniform prior: every product has the same factor $\tfrac13$, which cancels. The posterior is the sensor column normalised. Here the column already sums to 1, so it is $(0.7,\ 0.2,\ 0.1)$.

---
## Part B. Bayes' nets (deck Ch.10.0)

## 5. Independence and conditional independence (Ch.10.0, slides 1-14)

**Notes.** $X$ and $Y$ are **independent** ($X\perp Y$) if $P(x,y)=P(x)P(y)$ for all $x,y$, or equally $P(x\mid y)=P(x)$.

$X$ is **conditionally independent** of $Y$ given $Z$ ($X\perp Y\mid Z$) if for all $x,y,z$

$$P(x,y\mid z)=P(x\mid z)\,P(y\mid z)\quad\Longleftrightarrow\quad P(x\mid z,y)=P(x\mid z)$$

Examples: Traffic $\perp$ Umbrella given Rain (fork). Alarm $\perp$ Fire given Smoke (chain). With the chain rule: $P(T,R,U)=P(R)\,P(T\mid R)\,P(U\mid R)$.

> [!example] Slide 5
> $P_1(T,W)$ is the table of section 1: $0.5\cdot 0.6=0.3\ne 0.4$, so dependent. $P_2$ has the same marginals and every row equals the product: independent.

> [!warning] Watch out
> - To prove independence check every row. One failing row disproves it. Equal marginals prove nothing.
> - Conditional independence does not imply independence. Ghostbusters (slide 14): $T\perp B\mid G$, yet $P(+t,+b)=0.32\ne 0.36=P(+t)P(+b)$.

> [!question]- Check (section 5): A joint $P(X,Y)$ has rows $(+x,+y)\ 0.12$, $(+x,-y)\ 0.28$, $(-x,+y)\ 0.18$, $(-x,-y)\ 0.42$. Are $X$ and $Y$ independent?
> Marginals: $P(+x)=0.12+0.28=0.4$, $P(-x)=0.6$, $P(+y)=0.12+0.18=0.3$, $P(-y)=0.7$.
> Check every row against the product of its marginals:
> $0.4\cdot 0.3=0.12$, $0.4\cdot 0.7=0.28$, $0.6\cdot 0.3=0.18$, $0.6\cdot 0.7=0.42$.
> All four rows match, so $X\perp Y$.

> [!question]- Check (section 5, Ghostbusters): Slide 14 gives $P(+g)=0.5$, $P(+t\mid +g)=0.8$, $P(+t\mid -g)=0.4$, $P(+b\mid +g)=0.4$, $P(+b\mid -g)=0.8$, and $T\perp B\mid G$. Compute $P(+g\mid +t,-b)$.
> Because $T\perp B\mid G$, each joint row is $P(G)\,P(T\mid G)\,P(B\mid G)$.
> Complements: $P(-b\mid +g)=1-0.4=0.6$, $P(-b\mid -g)=1-0.8=0.2$.
> $P(+t,-b,+g)=0.5\cdot 0.8\cdot 0.6=0.24$.
> $P(+t,-b,-g)=0.5\cdot 0.4\cdot 0.2=0.04$.
> Normalise: $Z=0.24+0.04=0.28$, so $P(+g\mid +t,-b)=0.24/0.28=\tfrac67\approx 0.857$.
> Top red and bottom not red both point to a ghost in the top square.

## 6. Bayes' net semantics (Ch.10.0, slides 15-30)

**Notes.** A full joint is too big ($d^n$ rows) and too hard to learn. A **Bayes' net** has one node per variable, a directed acyclic graph, and one conditional probability table (CPT) $P(X\mid\text{parents}(X))$ per node. Its joint is

$$P(x_1,\dots,x_n)=\prod_{i=1}^{n}P\big(x_i\mid \text{parents}(X_i)\big)$$

This is the chain rule plus the assumption $P(x_i\mid x_1,\dots,x_{i-1})=P(x_i\mid\text{parents}(X_i))$. A missing arc is a claim of conditional independence. Size for $n$ Boolean variables: full joint $2^n-1$ numbers, Bayes' net $\sum_i 2^{|\text{parents}(X_i)|}$ (alarm net: 10 against 31).

Alarm net: $B\to A\leftarrow E$, $A\to J$, $A\to M$, joint $P(B)P(E)P(A\mid B,E)P(J\mid A)P(M\mid A)$.

> [!example] Slide 30
> $P(+b,-e,+a,-j,+m)=0.001\cdot 0.998\cdot 0.94\cdot 0.1\cdot 0.7\approx 6.57\times 10^{-5}$.

> [!warning] Error on slide 30
> The slide writes only $P(M\mid A)P(J\mid A)P(A\mid B,E)$. The joint also needs the root factors $P(B)P(E)$.

> [!info] The CPTs of the alarm net (slide 30)
> $P(+b)=0.001$ and $P(+e)=0.002$. Each table below gives the probability of the $+$ value. The $-$ value is 1 minus it.
>
> | $B$ | $E$ | $P(+a\mid B,E)$ |
> |---|---|---|
> | $+b$ | $+e$ | 0.95 |
> | $+b$ | $-e$ | 0.94 |
> | $-b$ | $+e$ | 0.29 |
> | $-b$ | $-e$ | 0.001 |
>
> | $A$ | $P(+j\mid A)$ | $P(+m\mid A)$ |
> |---|---|---|
> | $+a$ | 0.9 | 0.7 |
> | $-a$ | 0.05 | 0.01 |
>
> Count of free numbers: $B$ 1, $E$ 1, $A$ 4 (two parents, $2^2$), $J$ 2, $M$ 2. Total 10.

> [!question]- Check (section 6): Use the alarm CPTs of slide 30. Compute $P(-b,+e,+a,+j,-m)$.
> One factor per node, roots included:
> $P(-b)\,P(+e)\,P(+a\mid -b,+e)\,P(+j\mid +a)\,P(-m\mid +a)$.
> Values: $P(-b)=1-0.001=0.999$, $P(+e)=0.002$, $P(+a\mid -b,+e)=0.29$, $P(+j\mid +a)=0.9$, $P(-m\mid +a)=1-0.7=0.3$.
> Multiply left to right: $0.999\cdot 0.002=0.001998$. Times $0.29$: $0.00057942$. Times $0.9$: $0.000521478$. Times $0.3$: $0.000156443$.
> Result: about $1.56\times 10^{-4}$.

> [!question]- Check (section 6, size): A net has six Boolean nodes: $X_1$ and $X_2$ have no parents, $X_3$ has parents $X_1,X_2$, $X_4$ and $X_5$ each have parent $X_3$, and $X_6$ has parents $X_4,X_5$. Write its joint and count its free numbers against the full joint.
> Joint: $P(x_1)\,P(x_2)\,P(x_3\mid x_1,x_2)\,P(x_4\mid x_3)\,P(x_5\mid x_3)\,P(x_6\mid x_4,x_5)$.
> Each Boolean node needs $2^{\text{number of parents}}$ numbers: $1+1+4+2+2+4=14$.
> Full joint: $2^6-1=63$.
> The net is smaller because it claims conditional independences, for example $X_4\perp X_1\mid X_3$.

## 7. Causality (Ch.10.0, slides 31-34)

**Notes.** Net $R\to T$ with $P(+r)=\tfrac14$, $P(+t\mid +r)=\tfrac34$, $P(+t\mid -r)=\tfrac12$ gives the joint $\tfrac{3}{16},\tfrac{1}{16},\tfrac{6}{16},\tfrac{6}{16}$. The reversed net $T\to R$ with $P(+t)=\tfrac{9}{16}$, $P(+r\mid +t)=\tfrac13$, $P(+r\mid -t)=\tfrac17$ gives the same joint.

So arrows encode **conditional independence**, not necessarily causation. Causal nets are often simpler and easier to build, but a net need not be causal.

> [!question]- Check (section 7): A net $R\to T$ has $P(+r)=0.2$, $P(+t\mid +r)=0.9$, $P(+t\mid -r)=0.25$. Build the joint, then compute the tables $P(T)$ and $P(R\mid T)$ of the reversed net $T\to R$.
> Joint, one product per row:
> $P(+r,+t)=0.2\cdot 0.9=0.18$, $P(+r,-t)=0.2\cdot 0.1=0.02$, $P(-r,+t)=0.8\cdot 0.25=0.20$, $P(-r,-t)=0.8\cdot 0.75=0.60$.
> Root of the reversed net: $P(+t)=0.18+0.20=0.38$, $P(-t)=0.62$.
> Its CPT: $P(+r\mid +t)=0.18/0.38\approx 0.474$, $P(+r\mid -t)=0.02/0.62\approx 0.032$.
> Check one row: $P(+t)\,P(+r\mid +t)=0.38\cdot(0.18/0.38)=0.18$. The reversed net gives the same joint.

> [!question]- Check (section 7, find the mistake): "The net has an arrow $T\to R$, so the model says that traffic causes rain." What is wrong?
> An arrow only says which conditional table is stored, here $P(R\mid T)$. The nets $R\to T$ and $T\to R$ can hold the same joint.
> What a net really claims is its set of conditional independences, read from the missing arcs. With two nodes and one arc there is no missing arc, so no claim at all.

---
## Part C. Markov decision processes (deck Ch.7)

## 8. MDPs and policies (Ch.7, slides 1-16)

**Notes.** An **MDP** has states $S$, actions $A$, a transition function $T(s,a,s')=P(s'\mid s,a)$, a reward function $R(s,a,s')$, a start state and maybe terminal states. Markov property: the outcome of an action depends only on the current state.

The solution is a **policy** $\pi^*:S\to A$ that maximises expected utility. A fixed plan fails because actions can slip.

Grid World: an action goes straight with probability 0.8 and to each side with 0.1. If a wall blocks the move, the agent stays put. There is a small living reward per step and a big reward at the exits. The optimal policy changes with the living reward: careful at $-0.01$, risky at $-0.4$, suicidal at $-2.0$.

Racing MDP (slides 12, 14):

| State | Slow | Fast |
|---|---|---|
| Cool | to cool (1.0), reward $+1$ | to cool (0.5) or warm (0.5), reward $+2$ |
| Warm | to cool (0.5) or warm (0.5), reward $+1$ | to overheated (1.0), reward $-10$ |

Overheated is terminal. In the search tree, $(s,a)$ is a **q-state** (a chance node).

> [!question]- Check (section 8, Grid World): The agent stands in a corner cell. There are walls to its North and to its West, and a free cell to its East. It takes the action North. Give the distribution of the next state.
> North is the intended direction (0.8). The two sides of North are West and East (0.1 each).
> North: wall, stay put, 0.8. West: wall, stay put, 0.1. East: free, move, 0.1.
> So it stays in the corner with $0.8+0.1=0.9$ and moves East with $0.1$.

> [!question]- Check (section 8, racing): Start in cool. The policy is fast in cool and slow in warm. What is the probability of the state sequence cool, warm, cool, and what is the sum of rewards on it?
> Step 1: fast from cool goes to warm with 0.5, reward $+2$.
> Step 2: slow from warm goes to cool with 0.5, reward $+1$.
> Probability: $0.5\cdot 0.5=0.25$. Sum of rewards: $2+1=3$.
> The Markov property is what allows the product: step 2 depends only on being in warm, not on how the car got there.

## 9. Discounting (Ch.7, slides 17-27)

**Notes.** Prefer rewards now to rewards later. With discount factor $\gamma$ ($0<\gamma\le 1$):

$$U([r_0,r_1,r_2,\dots])=r_0+\gamma r_1+\gamma^2 r_2+\cdots\ \le\ \frac{R_{\max}}{1-\gamma}\quad(0<\gamma<1)$$

Slide 22, $\gamma=0.5$: $U([1,2,3])=1+1+0.75=2.75<U([3,2,1])=3+1+0.25=4.25$.

Fixes for infinite utilities (slide 26): finite horizon, discounting, or an absorbing state.

> [!example] Corridor quiz (slides 24-25)
> States $a$ to $e$, exit rewards 10 at $a$ and 1 at $e$. $\gamma=1$: all West. $\gamma=0.1$: $b,c$ West, $d$ East. From $d$, West is worth $10\gamma^3$ and East $1\cdot\gamma$, so the tie is at $\gamma=1/\sqrt{10}\approx 0.316$.

> [!question]- Check (section 9): With $\gamma=0.5$, compare $U([2,4,8])$ and $U([8,4,2])$. Then give the upper bound on any utility when $R_{\max}=5$ and $\gamma=0.9$.
> $U([2,4,8])=2+0.5\cdot 4+0.25\cdot 8=2+2+2=6$.
> $U([8,4,2])=8+0.5\cdot 4+0.25\cdot 2=8+2+0.5=10.5$.
> The same three rewards are worth more when the big one comes first.
> Bound: $R_{\max}/(1-\gamma)=5/(1-0.9)=5/0.1=50$. Even an infinite sequence of rewards 5 stays at or below 50.

> [!question]- Check (section 9, corridor): In the corridor of slides 24-25 (exit 10 at $a$, exit 1 at $e$), compare West and East from state $b$ and from state $c$ for $\gamma=0.5$. Is there any $\gamma$ with $0<\gamma\le 1$ for which $c$ prefers East?
> Count the moves before the exit reward, as in the example ($d$ West is $10\gamma^3$, $d$ East is $1\cdot\gamma$).
> From $b$: West $10\gamma=10\cdot 0.5=5$. East $1\cdot\gamma^3=0.125$. West.
> From $c$: West $10\gamma^2=10\cdot 0.25=2.5$. East $1\cdot\gamma^2=0.25$. West.
> From $c$ both exits are equally far, so both values have the same factor $\gamma^2$. West is 10 times East for every $\gamma>0$. State $c$ never prefers East. Only $d$ can switch.

## 10. Bellman equations and value iteration (Ch.7, slides 28-70)

**Notes.** $V^*(s)$ is the expected utility from $s$ acting optimally. $Q^*(s,a)$ is the same after first taking action $a$.

$$V^*(s)=\max_a Q^*(s,a),\qquad Q^*(s,a)=\sum_{s'}T(s,a,s')\big[R(s,a,s')+\gamma V^*(s')\big]$$

**Value iteration**: start with $V_0(s)=0$. Then for all states at once

$$V_{k+1}(s)\leftarrow\max_a\sum_{s'}T(s,a,s')\big[R(s,a,s')+\gamma V_k(s')\big]$$

Repeat until nothing changes. $V_k(s)$ is the best value if the world ends in $k$ steps. Cost $O(S^2A)$ per sweep. With $\gamma<1$, $V_k$ and $V_{k+1}$ differ by at most $\gamma^k\max|R|$, so the values converge to the unique optimum.

> [!example] Racing, values as (cool, warm, overheated)
> $\gamma=1$ (slide 65): $V_1=(2,1,0)$, $V_2=(3.5,2.5,0)$, $V_3=(5,4,0)$, growing for ever.
> $\gamma=0.5$ (slides 67-70): $V_1=(2,1,0)$, $V_2=(2.75,1.75,0)$, $V_3=(3.125,2.125,0)$, $V^*=(3.5,2.5,0)$.
> Working for $V_2(\text{cool})$ at $\gamma=0.5$: slow $1+0.5\cdot 2=2$, fast $0.5(2+0.5\cdot 2)+0.5(2+0.5\cdot 1)=2.75$. Take the max.

> [!warning] Watch out
> Use the old vector $V_k$ on the right-hand side for every state. Terminal states keep value 0.

> [!question]- Check (section 10): Racing MDP with $\gamma=0.9$ and $V_1=(2,1,0)$ for (cool, warm, overheated). Compute $V_2(\text{cool})$ and $V_2(\text{warm})$.
> Cool, slow: $1.0\,(1+0.9\cdot 2)=2.8$.
> Cool, fast: $0.5\,(2+0.9\cdot 2)+0.5\,(2+0.9\cdot 1)=0.5\cdot 3.8+0.5\cdot 2.9=1.9+1.45=3.35$.
> $V_2(\text{cool})=\max(2.8,\ 3.35)=3.35$.
> Warm, slow: $0.5\,(1+0.9\cdot 2)+0.5\,(1+0.9\cdot 1)=0.5\cdot 2.8+0.5\cdot 1.9=1.4+0.95=2.35$.
> Warm, fast: $1.0\,(-10+0.9\cdot 0)=-10$.
> $V_2(\text{warm})=\max(2.35,\ -10)=2.35$.
> So $V_2=(3.35,\ 2.35,\ 0)$.

> [!question]- Check (section 10, fixed point): Show that $V^*=(3.5,\ 2.5,\ 0)$ satisfies the Bellman equation of the racing MDP for $\gamma=0.5$.
> Put $V^*$ on the right-hand side and check that the same numbers come out.
> Cool, slow: $1+0.5\cdot 3.5=2.75$.
> Cool, fast: $0.5\,(2+0.5\cdot 3.5)+0.5\,(2+0.5\cdot 2.5)=0.5\cdot 3.75+0.5\cdot 3.25=1.875+1.625=3.5$.
> Max is $3.5=V^*(\text{cool})$.
> Warm, slow: $0.5\,(1+0.5\cdot 3.5)+0.5\,(1+0.5\cdot 2.5)=0.5\cdot 2.75+0.5\cdot 2.25=1.375+1.125=2.5$.
> Warm, fast: $-10$.
> Max is $2.5=V^*(\text{warm})$. One more update changes nothing, so value iteration has converged.

## 11. Policy evaluation, extraction, policy iteration (Ch.7, slides 71-107)

**Notes.** **Policy evaluation**: the value of a fixed policy $\pi$ drops the max.

$$V^\pi(s)=\sum_{s'}T(s,\pi(s),s')\big[R(s,\pi(s),s')+\gamma V^\pi(s')\big]$$

Iterate it ($O(S^2)$ per sweep) or solve it as a linear system.

**Policy extraction**: from values a one-step look-ahead is needed, $\pi^*(s)=\arg\max_a\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^*(s')]$. From q-values it is just $\pi^*(s)=\arg\max_a Q^*(s,a)$.

**Policy iteration**: evaluate $\pi_i$, improve it by one-step look-ahead with $V^{\pi_i}$, repeat until $\pi_{i+1}=\pi_i$. It is optimal and often faster than value iteration.

> [!example] Slides 100-103, racing, $\gamma=0.5$
> Start with always slow: $V^{\pi_0}=(2,2)$. Improve: $\pi_1=$ (cool fast, warm slow). Evaluate: $V^{\pi_1}=(3.5,2.5)$. Improve again: no change, so $\pi_1=\pi^*$.

> [!warning] Error on slide 102
> The slide writes the slow q-value in warm as 3. It is $0.5\cdot 2+0.5\cdot 2=2$. The chosen action (slow) is unchanged.

> [!question]- Check (section 11, evaluation): Racing MDP, $\gamma=0.5$. Evaluate the policy "always fast" by solving the linear system for $V^\pi(\text{cool})$ and $V^\pi(\text{warm})$.
> Warm, fast: goes to overheated for sure, reward $-10$, and $V^\pi(\text{overheated})=0$. So $V^\pi(\text{warm})=-10+0.5\cdot 0=-10$.
> Cool, fast: $V^\pi(\text{cool})=0.5\,[2+0.5\,V^\pi(\text{cool})]+0.5\,[2+0.5\,V^\pi(\text{warm})]$.
> Substitute $V^\pi(\text{warm})=-10$: $V^\pi(\text{cool})=1+0.25\,V^\pi(\text{cool})+1-2.5=-0.5+0.25\,V^\pi(\text{cool})$.
> So $0.75\,V^\pi(\text{cool})=-0.5$ and $V^\pi(\text{cool})=-\tfrac23\approx -0.667$.
> $V^\pi=(-0.667,\ -10)$. There is no max anywhere, so the equations are linear.

> [!question]- Check (section 11, improvement): Continue with $V^\pi=(-\tfrac23,\ -10)$ of "always fast" ($\gamma=0.5$). Do one policy improvement step. Is the new policy optimal?
> One-step look-ahead with $V^\pi$ in each state.
> Cool, slow: $1+0.5\cdot(-\tfrac23)=1-0.333=0.667$.
> Cool, fast: $0.5\,[2+0.5\cdot(-\tfrac23)]+0.5\,[2+0.5\cdot(-10)]=0.5\cdot 1.667+0.5\cdot(-3)=0.833-1.5=-0.667$.
> Cool chooses slow.
> Warm, slow: $0.5\,[1+0.5\cdot(-\tfrac23)]+0.5\,[1+0.5\cdot(-10)]=0.5\cdot 0.667+0.5\cdot(-4)=0.333-2=-1.667$.
> Warm, fast: $-10$.
> Warm chooses slow.
> New policy: always slow. It is not optimal yet. The example above starts from always slow and needs one more round to reach (cool fast, warm slow).

---
## Part D. Hidden Markov models (deck Ch.11.0)

## 12. Markov chains (Ch.11.0, slides 1-17)

**Notes.** A **Markov model** has an initial distribution $P(X_1)$ and one transition table $P(X_t\mid X_{t-1})$ used at every step (stationarity). Markov property: the future is independent of the past given the present.

$$P(X_1,\dots,X_T)=P(X_1)\prod_{t=2}^{T}P(X_t\mid X_{t-1})$$

**Mini-forward algorithm**: $P(x_t)=\sum_{x_{t-1}}P(x_t\mid x_{t-1})\,P(x_{t-1})$. No normalisation needed.

**Stationary distribution** $P_\infty$: one more step leaves it unchanged, $P_\infty(X)=\sum_x P(X\mid x)P_\infty(x)$.

> [!example] Weather chain (slides 8-14)
> sun to sun 0.9, sun to rain 0.1, rain to sun 0.3, rain to rain 0.7.
> From sun: $P(\text{sun})=1,\ 0.9,\ 0.84,\ 0.804,\dots\to 0.75$.
> Stationary: $P_\infty(\text{sun})=0.9P_\infty(\text{sun})+0.3P_\infty(\text{rain})$ gives $P_\infty(\text{sun})=3P_\infty(\text{rain})$, so $\langle 0.75,0.25\rangle$.

> [!warning] Two weather chains in the deck
> Slides 8-14 use 0.9 / 0.3 and start at $X_1$. Slides 11 and 41-46 use sun to sun 0.6, rain to sun 0.1 and start at $W_0$. Check which one a question uses.

> [!question]- Check (section 12): A weather chain has sun to sun 0.8 and rain to sun 0.4. Day 1 is rain. Find $P(\text{sun})$ on days 2, 3 and 4 with the mini-forward algorithm, then the stationary distribution.
> Update: $P(\text{sun}_t)=0.8\,P(\text{sun}_{t-1})+0.4\,P(\text{rain}_{t-1})$.
> Day 1: $P(\text{sun})=0$, $P(\text{rain})=1$.
> Day 2: $0.8\cdot 0+0.4\cdot 1=0.4$.
> Day 3: $0.8\cdot 0.4+0.4\cdot 0.6=0.32+0.24=0.56$.
> Day 4: $0.8\cdot 0.56+0.4\cdot 0.44=0.448+0.176=0.624$.
> Stationary: $P_\infty(\text{sun})=0.8\,P_\infty(\text{sun})+0.4\,P_\infty(\text{rain})$, so $0.2\,P_\infty(\text{sun})=0.4\,P_\infty(\text{rain})$ and $P_\infty(\text{sun})=2\,P_\infty(\text{rain})$.
> With the sum equal to 1: $\langle \tfrac23,\tfrac13\rangle\approx\langle 0.667,0.333\rangle$. The values $0.4,\ 0.56,\ 0.624$ move towards $0.667$.

> [!question]- Check (section 12, joint): Use the chain of slides 8-14 (sun to sun 0.9, rain to sun 0.3) with $P(X_1=\text{sun})=0.5$. Compute $P(\text{sun},\text{sun},\text{rain},\text{rain})$ for days 1 to 4.
> $P(X_1)\,P(X_2\mid X_1)\,P(X_3\mid X_2)\,P(X_4\mid X_3)$.
> Factors: $P(\text{sun})=0.5$, sun to sun $0.9$, sun to rain $0.1$, rain to rain $1-0.3=0.7$.
> $0.5\cdot 0.9=0.45$. Times $0.1$: $0.045$. Times $0.7$: $0.0315$.
> The same transition table is used at every step (stationarity).

## 13. HMMs and filtering (Ch.11.0, slides 18-40)

**Notes.** An **HMM** adds an evidence variable $E_t$ to each hidden state $X_t$. Three tables: initial $P(X_1)$, transitions $P(X_t\mid X_{t-1})$, emissions (sensor model) $P(E_t\mid X_t)$.

$$P(X_1,E_1,\dots,X_T,E_T)=P(X_1)\,P(E_1\mid X_1)\prod_{t=2}^{T}P(X_t\mid X_{t-1})\,P(E_t\mid X_t)$$

**Filtering** tracks the belief $B(X_t)=P(X_t\mid e_{1:t})$. $B'$ is the belief before today's evidence.

$$
\begin{aligned}
\text{Time update:}\quad & B'(X_{t+1})=\sum_{x_t}P(X_{t+1}\mid x_t)\,B(x_t)\\
\text{Observation update:}\quad & B(X_{t+1})\propto P(e_{t+1}\mid X_{t+1})\,B'(X_{t+1})
\end{aligned}
$$

The time update blurs the belief. The observation update sharpens it and needs normalising.

> [!example] Umbrella HMM (slide 24)
> $P(+r\mid +r)=0.7$, $P(+r\mid -r)=0.3$, $P(+u\mid +r)=0.9$, $P(+u\mid -r)=0.2$. The slide gives no $P(R_1)$, our notes use $\langle 0.5,0.5\rangle$.
> Day 1, see $+u$: $(0.5\cdot 0.9,\ 0.5\cdot 0.2)=(0.45,0.10)$, so $B(R_1)=0.818$.
> Day 2: time update $0.818\cdot 0.7+0.182\cdot 0.3=0.627$. See $+u$: $(0.627\cdot 0.9,\ 0.373\cdot 0.2)$, so $B(R_2)=0.883$.

> [!warning] Watch out
> Evidence variables are not independent of each other. They are only conditionally independent given the hidden states (slide 27 quiz).

> [!question]- Check (section 13): Umbrella HMM with $P(R_1)=\langle 0.5,0.5\rangle$. On day 1 there is no umbrella ($-u$), on day 2 there is one ($+u$). Compute $B(R_1)$ and $B(R_2)$.
> Sensor values for $-u$: $P(-u\mid +r)=1-0.9=0.1$, $P(-u\mid -r)=1-0.2=0.8$.
> Day 1, observation: $(0.5\cdot 0.1,\ 0.5\cdot 0.8)=(0.05,\ 0.40)$. $Z=0.45$. $B(R_1=+r)=0.05/0.45=0.111$, $B(R_1=-r)=0.889$.
> Day 2, time update: $B'(R_2=+r)=0.7\cdot 0.111+0.3\cdot 0.889=0.0778+0.2667=0.344$, $B'(R_2=-r)=0.656$.
> Day 2, observation $+u$: $(0.344\cdot 0.9,\ 0.656\cdot 0.2)=(0.310,\ 0.131)$. $Z=0.441$.
> $B(R_2=+r)=0.310/0.441\approx 0.703$.
> One umbrella moves the belief in rain from 0.11 to 0.70.

> [!question]- Check (section 13, time update): In the umbrella HMM the belief is $B(R_t=+r)=0.9$. No evidence arrives for the next three days. Give the belief in rain after each day. What does it tend to?
> Only time updates: $B'(+r)=0.7\,B(+r)+0.3\,B(-r)$.
> Day 1: $0.7\cdot 0.9+0.3\cdot 0.1=0.63+0.03=0.66$.
> Day 2: $0.7\cdot 0.66+0.3\cdot 0.34=0.462+0.102=0.564$.
> Day 3: $0.7\cdot 0.564+0.3\cdot 0.436=0.3948+0.1308=0.5256$.
> It tends to $0.5$, the stationary distribution of this chain. Without evidence the belief blurs, and the sum stays 1 without normalising.

## 14. Forward algorithm and Viterbi (Ch.11.0, slides 41-56)

**Notes.** The **forward algorithm** does both updates in one step and normalises once at the end:

$$f_t(x)=P(e_t\mid x)\sum_{x'}P(x\mid x')\,f_{t-1}(x'),\qquad \sum_x f_t(x)=P(e_{1:t})$$

Slides 42-45 (chain 0.6 / 0.1, $W_0=\langle 0.8,0.2\rangle$): $B'(W_1)=\langle 0.5,0.5\rangle$. After the forecast "good" the unnormalised values are $\langle 0.4,0.15\rangle$, so $B(W_1)=\langle 8/11,3/11\rangle$.

> [!info] The tables of slide 42 and the steps of slides 43-45
> State $W_i$ (weather on day $i$), evidence $F_i$ (forecast for day $i$). Vectors are $\langle\text{sun},\text{rain}\rangle$.
> Initial: $B(W_0)=\langle 0.8,0.2\rangle$.
> Transitions: sun to sun 0.6, sun to rain 0.4, rain to sun 0.1, rain to rain 0.9.
> Sensor: $P(\text{good}\mid\text{sun})=0.8$, $P(\text{bad}\mid\text{sun})=0.2$, $P(\text{good}\mid\text{rain})=0.3$, $P(\text{bad}\mid\text{rain})=0.7$.
> Time update: $B'(W_1=\text{sun})=0.6\cdot 0.8+0.1\cdot 0.2=0.5$, $B'(W_1=\text{rain})=0.4\cdot 0.8+0.9\cdot 0.2=0.5$.
> Observation update with good: $0.8\cdot 0.5=0.4$ and $0.3\cdot 0.5=0.15$.
> Normalise: $0.4+0.15=0.55$, so $\langle 0.4/0.55,\ 0.15/0.55\rangle=\langle 8/11,3/11\rangle$.

**Viterbi** finds the most likely sequence of hidden states. Replace the sum by a max and keep back-pointers:

$$m_{1:t+1}=P(e_{t+1}\mid X_{t+1})\max_{x_t}P(X_{t+1}\mid x_t)\,m_{1:t}$$

Slide 56 (umbrella HMM): the best path is $+r,+r,-r,+r,+r$. Forward and Viterbi cost $O(T|X|^2)$. Enumeration costs $O(|X|^T)$.

> [!info] How to run Viterbi by hand
> $m_{1:t}(x)$ is the probability of the best path that ends in state $x$ at time $t$, together with the evidence up to $t$.
> 1. First column: $m_{1:1}(x)=P(x)\,P(e_1\mid x)$. Slide 56 uses the normalised version of this column, $\langle 0.8182,0.1818\rangle$. Scaling a whole column never changes the best path.
> 2. Next column, for each state $x$: compute $P(x\mid x')\,m_{1:t}(x')$ for every predecessor $x'$. Keep the largest and note which $x'$ gave it (the back-pointer). Multiply by $P(e_{t+1}\mid x)$.
> 3. After the last column: take the state with the largest $m$. Follow the back-pointers to the first column. Read the path in forward order.
>
> The path cannot be fixed earlier, because later evidence can make another final state win.

> [!warning] Watch out
> - The most likely sequence is not the sequence of the most likely single states.
> - Slide 45's last table is headed $B'(W_1)$ but holds $B(W_1)=\langle 8/11,3/11\rangle$.

> [!question]- Check (section 14, forward): Use the forecast HMM of slide 42, where $B'(W_1)=\langle 0.5,0.5\rangle$. The forecast for day 1 is "bad". Compute $B(W_1)$ and the probability of this forecast.
> Sensor values for bad: $P(\text{bad}\mid\text{sun})=0.2$, $P(\text{bad}\mid\text{rain})=0.7$.
> Unnormalised: sun $0.2\cdot 0.5=0.10$, rain $0.7\cdot 0.5=0.35$.
> Sum: $0.10+0.35=0.45$. This is $P(F_1=\text{bad})$, the probability of the evidence.
> Normalise: $B(W_1)=\langle 0.10/0.45,\ 0.35/0.45\rangle=\langle \tfrac29,\tfrac79\rangle\approx\langle 0.222,0.778\rangle$.

> [!question]- Check (section 14, Viterbi): Umbrella HMM with $P(R_1)=\langle 0.5,0.5\rangle$ and evidence $+u_1,-u_2$. Run Viterbi without normalising. Give the most likely weather sequence and its joint probability.
> First column: $m_{1:1}=\langle 0.5\cdot 0.9,\ 0.5\cdot 0.2\rangle=\langle 0.45,\ 0.10\rangle$ for $(+r,-r)$.
> Second column, evidence $-u$ with $P(-u\mid +r)=0.1$, $P(-u\mid -r)=0.8$:
> $m_{1:2}(+r)=0.1\cdot\max(0.7\cdot 0.45,\ 0.3\cdot 0.10)=0.1\cdot\max(0.315,\ 0.03)=0.0315$, back-pointer $+r$.
> $m_{1:2}(-r)=0.8\cdot\max(0.3\cdot 0.45,\ 0.7\cdot 0.10)=0.8\cdot\max(0.135,\ 0.07)=0.108$, back-pointer $+r$.
> Best final state: $-r$ with $0.108$. Its back-pointer is $+r$.
> Most likely sequence: $(+r,-r)$ with $P(x_{1:2},e_{1:2})=0.108$.
> Check against all four paths: $(+r,+r)\ 0.0315$, $(+r,-r)\ 0.108$, $(-r,+r)\ 0.10\cdot 0.3\cdot 0.1=0.003$, $(-r,-r)\ 0.10\cdot 0.7\cdot 0.8=0.056$. The largest is $0.108$.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Marginal | $P(t)=\sum_w P(t,w)$ |
| Conditional | $P(a\mid b)=P(a,b)/P(b)$ |
| Enumeration | select evidence, sum out hidden, normalise. $O(d^n)$ |
| Bayes' rule | $P(x\mid y)=P(y\mid x)P(x)/P(y)$ |
| Independence | $P(x,y)=P(x)P(y)$ for all $x,y$ |
| Conditional independence | $P(x,y\mid z)=P(x\mid z)P(y\mid z)$ for all $x,y,z$ |
| Bayes' net joint | $\prod_i P(x_i\mid\text{parents}(X_i))$ |
| MDP | $S$, $A$, $T(s,a,s')$, $R(s,a,s')$, start, terminals, $\gamma$ |
| Discounted utility | $r_0+\gamma r_1+\gamma^2 r_2+\cdots\le R_{\max}/(1-\gamma)$ |
| Bellman | $V^*(s)=\max_a\sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^*(s')]$ |
| Value iteration | $V_0=0$, repeat the Bellman update with $V_k$. $O(S^2A)$ per sweep |
| Policy evaluation | Bellman without the max, $a=\pi(s)$ |
| Policy iteration | evaluate, improve, repeat until the policy stops changing |
| Mini-forward | $P(x_t)=\sum_{x_{t-1}}P(x_t\mid x_{t-1})P(x_{t-1})$ |
| Stationary distribution | $P_\infty(X)=\sum_x P(X\mid x)P_\infty(x)$ |
| Filtering | time update, then observation update and normalise |
| Viterbi | forward with $\max$ instead of $\sum$, plus back-pointers |

## Check yourself
> [!question]- 1. With $P(X,Y)$: $(+x,+y)\ 0.2$, $(+x,-y)\ 0.3$, $(-x,+y)\ 0.4$, $(-x,-y)\ 0.1$, find $P(+x\mid +y)$ and $P(X\mid -y)$.
> $P(+y)=0.6$, so $P(+x\mid +y)=0.2/0.6=\tfrac13$. For $-y$: select $(0.3,0.1)$, $Z=0.4$, so $P(X\mid -y)=(0.75,0.25)$.

> [!question]- 2. Meningitis: $P(+m)=0.0001$, $P(+s\mid +m)=0.8$, $P(+s\mid -m)=0.01$. Find $P(+m\mid +s)$.
> $P(+s)=0.00008+0.01\cdot 0.9999=0.010079$. $P(+m\mid +s)=0.00008/0.010079\approx 0.0079$.

> [!question]- 3. In the net $R\to T$ with $P(+r)=\tfrac14$ and $P(-t\mid +r)=\tfrac14$, find $P(+r,-t)$.
> One factor per node: $P(+r)\,P(-t\mid +r)=\tfrac14\cdot\tfrac14=\tfrac{1}{16}$.

> [!question]- 4. Racing MDP, $\gamma=0.5$, $V_1=(2,1,0)$. Compute $V_2(\text{warm})$.
> Slow: $0.5(1+0.5\cdot 2)+0.5(1+0.5\cdot 1)=1+0.75=1.75$. Fast: $-10+0=-10$. $V_2(\text{warm})=1.75$.

> [!question]- 5. Find the stationary distribution of the chain sun to sun 0.9, rain to sun 0.3.
> $P_\infty(\text{sun})=0.9P_\infty(\text{sun})+0.3P_\infty(\text{rain})$, so $0.1P_\infty(\text{sun})=0.3P_\infty(\text{rain})$. With the sum equal to 1: $\langle 0.75,0.25\rangle$.

> [!question]- 6. Which HMM update needs normalisation, and why?
> The observation update. Multiplying by $P(e\mid X)$ gives numbers that sum to $P(e_{t+1}\mid e_{1:t})$, not 1. The time update keeps the sum at 1.

## Links
- Course index: [[CMPE421 Home]]
- Previous lecture: [[CMPE421 L06 - Adversarial search and games]]
- Next lecture: [[CMPE421 L08 - Logical agents]]
- Probability again, from another deck: [[CMPE421 L10 - Quantifying uncertainty]]
- Full step-by-step notes (PDF, with 24 exam-style questions): [CMPE421_Lecture_07_MDPs_Probability_BayesNets_HMMs.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_07/CMPE421_Lecture_07_MDPs_Probability_BayesNets_HMMs.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_07/CMPE421_Lecture_07_MDPs_Probability_BayesNets_HMMs.pdf`
- Original slides (PowerPoint only): [Ch.7.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/Ch.7.pptx), [Ch.9.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/Ch.9.pptx), [Ch.10.0.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/Ch.10.0.pptx), [Ch.11.0.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/Ch.11.0.pptx) in `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/`
- Quiz: [[CMPE421 L07 - MDPs, probability, Bayes nets, HMMs - quiz]]
