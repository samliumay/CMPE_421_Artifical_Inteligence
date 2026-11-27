---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L06 - Adversarial search and games
type: quiz
questions: 28
---

# CMPE421 L06 - Adversarial search and games - quiz

Lesson note: [[CMPE421 L06 - Adversarial search and games]]. One question per check question of the lesson. Pick an option, then open the answer.

## 1. Games (Ch.6 deck, slides 1-11)

### Q1 (section 1, classify)

Backgammon and Battleship are both 2-player zero-sum games. Classify each on the two other axes: deterministic or stochastic, and perfect or imperfect information.

- A) Backgammon: deterministic, imperfect. Battleship: stochastic, perfect.
- B) Backgammon: stochastic, imperfect. Battleship: stochastic, imperfect.
- C) Backgammon: stochastic, perfect. Battleship: deterministic, imperfect.
- D) Backgammon: deterministic, perfect. Battleship: deterministic, perfect.

> [!success]- Answer
> **C.** Backgammon has dice, so it is stochastic. Both players see the whole board and the dice, so the information is perfect.
> Battleship has no chance once the ships are placed, so it is deterministic. You do not see the opponent's ships, so the information is imperfect.
> - A: This swaps the two games.
> - B: This treats the dice as hidden information and the hidden ships as chance. A die you can see is chance, not hidden information.
> - D: This ignores the dice of backgammon and the hidden ships of Battleship.

### Q2 (section 1, payoffs and ply)

An outcome gives player 1 the utility $u_1=7$. What is $u_2$ in a zero-sum game, what is $u_2$ in a common-payoff game, and how many ply are 3 full chess moves (White then Black)?

- A) Zero-sum $-7$, common payoff $7$, 6 ply.
- B) Zero-sum $7$, common payoff $-7$, 6 ply.
- C) Zero-sum $-7$, common payoff $7$, 3 ply.
- D) Zero-sum $7$, common payoff $-7$, 3 ply.

> [!success]- Answer
> **A.** Zero-sum: $u_2=-u_1=-7$. Common payoff: $u_2=u_1=7$. A ply is one move by one player, so one chess move (White then Black) is 2 ply, and 3 moves are $3\cdot 2=6$ ply.
> - B: This swaps the two game types. In a zero-sum game one player's gain is the other's loss.
> - C: The utilities are right, but this counts one chess move as 1 ply.
> - D: This swaps the game types and also counts one chess move as 1 ply.

### Q3 (section 1, policy)

Why is a fixed sequence of actions not enough as the solution of a game?

- A) A fixed sequence needs too much memory, and a policy is shorter.
- B) The utilities are known only at terminal states, not along the way.
- C) Our own actions may have random results, so a plan may fail.
- D) We cannot know the opponent's replies, so we need a move for each state.

> [!success]- Answer
> **D.** After each of our moves the opponent chooses a reply, and we do not know which one. Each reply leads to a different state, and we need a move in each of them. A policy $S\to A$ gives a move for every state.
> - A: A policy is larger than one sequence, not shorter. Memory is not the reason.
> - B: This is true, but minimax handles it by backing values up. It is not why a sequence fails.
> - C: Chess is deterministic and still needs a policy. The reason is the opponent, not chance.

## 2. Minimax (Ch.6 deck, slides 12-23)

### Q4 (section 2, trace)

The root is a MAX node with three MIN children. Their leaves, left to right, are $(4,-2)$, $(1,7)$ and $(-3,9)$. What is the minimax value of the root?

- A) $4$
- B) $1$
- C) $9$
- D) $-3$

> [!success]- Answer
> **B.** MIN layer: $\min(4,-2)=-2$, $\min(1,7)=1$, $\min(-3,9)=-3$. Root: $\max(-2,1,-3)=1$. MAX plays the middle move.
> - A: MIN and MAX are swapped: $\max$ in the second layer (4, 7, 9), then $\min$ at the root gives 4.
> - C: $\max$ is used at both layers: the largest leaf. MIN would answer that move with $-3$.
> - D: $\min$ is used at both layers: the smallest leaf.

### Q5 (section 2, roles swapped)

Same leaves, left to right: $(4,-2)$, $(1,7)$, $(-3,9)$. Now the root is a MIN node and its three children are MAX nodes. What is the root value?

- A) $1$
- B) $-3$
- C) $9$
- D) $4$

> [!success]- Answer
> **D.** MAX layer: $\max(4,-2)=4$, $\max(1,7)=7$, $\max(-3,9)=9$. Root: $\min(4,7,9)=4$. MIN plays the left move.
> - A: This keeps MAX at the root and MIN below, as in Q4. Who moves at which layer changes the value.
> - B: $\min$ is used at both layers: the smallest leaf.
> - C: $\max$ is used at both layers: the largest leaf.

### Q6 (section 2, code)

In `max_value` the variable `v` starts at `-inf`. A MAX node has two children with values $-8$ and $-5$. What does `max_value` return if `v` starts at `0` instead?

- A) $0$
- B) $-5$
- C) $-8$
- D) $-6.5$

> [!success]- Answer
> **A.** `v = max(0, -8) = 0`, then `v = max(0, -5) = 0`. No child beats the start value, so the node returns 0, a value that no child has. With `-inf` it returns $-5$.
> - B: This is the right value of the node, but only with the start value `-inf`.
> - C: This is the result of `min_value`, not `max_value`.
> - D: This is the average of the two children, as at a chance node.

## 3. Properties of minimax (Ch.6 deck, slides 24-28)

### Q7 (section 3, imperfect opponent)

MAX root with two MIN children. Left leaves: $6,6$. Right leaves: $2,20$. MIN plays its best move with probability $p$ and the other move otherwise. Which move does minimax play, and for which $p$ is going right better than going left?

- A) Minimax goes left. Right is better for $p>\tfrac29$.
- B) Minimax goes right. Right is better for $p>\tfrac29$.
- C) Minimax goes left. Right is better for $p<\tfrac79$.
- D) Minimax goes right. Right is better for $p<\tfrac79$.

> [!success]- Answer
> **C.** Minimax: left $=\min(6,6)=6$, right $=\min(2,20)=2$, root $\max(6,2)=6$, so left.
> MIN's best move on the right is 2. Expected value of right: $2p+20(1-p)=20-18p$.
> Right is better if $20-18p>6$, so $18p<14$, so $p<\tfrac{14}{18}=\tfrac79$.
> - A: This takes 20 as MIN's best move: $20p+2(1-p)=2+18p>6$ gives $p>\tfrac29$.
> - B: Minimax does not go right. Right is worth only 2 against a perfect MIN. The bound also uses the wrong best move.
> - D: The bound is right, but minimax compares 6 with 2, not the largest leaves.

### Q8 (section 3, cost)

A game tree has branching factor $b=4$ and maximum depth $m=10$. How many leaves does minimax visit, and about how many nodes does it keep in memory?

- A) $10\,000$ leaves, about $40$ nodes.
- B) $1\,048\,576$ leaves, about $40$ nodes.
- C) $1\,048\,576$ leaves, about $10$ nodes.
- D) $10\,000$ leaves, about $10$ nodes.

> [!success]- Answer
> **B.** Leaves: $b^m=4^{10}=1\,048\,576$. Memory: about $bm=4\cdot 10=40$ nodes, one path of 10 levels with up to 4 siblings per level.
> - A: This swaps $b$ and $m$: $m^b=10^4=10\,000$.
> - C: This keeps only the path of 10 nodes and forgets the waiting siblings.
> - D: This swaps $b$ and $m$ and also forgets the siblings.

## 4. Alpha-beta pruning (Ch.6 deck, slides 29-36)

### Q9 (section 4, trace)

MAX root with three MIN children. Leaves, left to right: A $=(6,9,4)$, B $=(3,8,7)$, C $=(5,2,10)$. Run alpha-beta with children visited left to right. Which leaves are pruned?

- A) 8, 7 and 10 are pruned. 6 of 9 leaves are seen.
- B) No leaf is pruned. All 9 leaves are seen.
- C) 8 and 7 are pruned. 7 of 9 leaves are seen.
- D) 2 and 10 are pruned. 7 of 9 leaves are seen.

> [!success]- Answer
> **A.** A: $\alpha=-\infty$, so all three leaves are needed. A $=4$, root $\alpha=4$.
> B: first leaf 3, $3\le 4$, return. Leaves 8 and 7 are pruned.
> C: first leaf 5, $5>4$, go on. Second leaf 2, $2\le 4$, return. Leaf 10 is pruned.
> Seen: 6, 9, 4, 3, 5, 2. Root 4.
> - B: This forgets to pass $\alpha$ and $\beta$ down. Each MIN node starts with $\alpha=-\infty$ and never prunes.
> - C: This tests only the first leaf of each MIN node. In C the leaf 5 is above 4, but the next leaf 2 is not.
> - D: This swaps the update and the test at every node: MIN updates $\alpha$ and stops when $v\ge\beta$ (as in Q11), MAX updates $\beta$ and stops when $v\le\alpha$. C then stops after the leaf 5, and the root gets 5, which is wrong.

### Q10 (section 4, ordering)

Same tree: A $=(6,9,4)$, B $=(3,8,7)$, C $=(5,2,10)$, MAX root. In the order A, B, C alpha-beta sees 6 leaves and returns 4. Now the root visits its children in the order C, B, A (leaves inside each node still left to right). How many leaves are seen, and what is the root value?

- A) 6 leaves, root value 4.
- B) 8 leaves, root value 8.
- C) 6 leaves, root value 8.
- D) 9 leaves, root value 4.

> [!success]- Answer
> **D.** C: all three leaves, C $=2$, $\alpha=2$. B: 3, 8, 7 are all above 2, no prune, B $=3$, $\alpha=3$. A: 6, 9, 4 are all above 3, no prune, A $=4$.
> All 9 leaves are seen. The best child came last, so $\alpha$ was never high enough to prune. Ordering changes the work, never the root value.
> - A: This assumes the order does not change the number of pruned leaves.
> - B: MIN and MAX are swapped (MIN root, MAX children). Alpha-beta then gives the root 8 and prunes only the last leaf 4.
> - C: MIN and MAX are swapped, which gives the root 8, and the 6 leaves of the order A, B, C are kept, as if the order did not change the work.

### Q11 (section 4, find the mistake)

A student writes: "At a MIN node we update $\alpha$, and we stop when $v\ge\beta$." What is the correct rule at a MIN node?

- A) Update $\alpha=\max(\alpha,v)$. Stop when $v\le\alpha$.
- B) Update $\beta=\min(\beta,v)$. Stop when $v\le\alpha$.
- C) Update $\beta=\min(\beta,v)$. Stop when $v\ge\beta$.
- D) Update $\alpha=\max(\alpha,v)$. Stop when $v\ge\beta$.

> [!success]- Answer
> **B.** A MIN node lowers its value, so it updates MIN's own bound $\beta=\min(\beta,v)$. It stops when $v\le\alpha$, because MAX higher in the tree already has something at least as good. Both parts of the statement are swapped.
> - A: The test is right, but updating $\alpha$ belongs to MAX nodes.
> - C: The update is right, but the test $v\ge\beta$ belongs to MAX nodes.
> - D: This is the student's statement. It is the rule of a MAX node.

## 5. Resource limits (Ch.6 deck, slides 37-51)

### Q12 (section 5, evaluation)

$\mathit{Eval}(s)=w_1f_1+w_2f_2+w_3f_3+w_4f_4$. Weights: queen 9, rook 5, knight 3, pawn 1. Features (White minus Black): queens $-1$, rooks $+1$, knights $+1$, pawns $+2$. What is $\mathit{Eval}(s)$?

- A) $19$
- B) $-1$
- C) $1$
- D) $3$

> [!success]- Answer
> **C.** $9\cdot(-1)+5\cdot 1+3\cdot 1+1\cdot 2=-9+5+3+2=1$. It is positive, so White is slightly ahead in material, although White has one queen less.
> - A: This drops the minus sign of the queen feature: $9+5+3+2$.
> - B: This reads the features as Black minus White, so every sign flips.
> - D: This adds the features and forgets the weights: $-1+1+1+2$.

### Q13 (section 5, depth)

You have 10 s per move at 50 000 nodes per second, and $b=20$. Plain minimax needs about $20^d$ nodes for depth $d$. Alpha-beta with perfect ordering needs about $20^{d/2}$. How many full ply can each search?

- A) Minimax 4, alpha-beta 8.
- B) Minimax 5, alpha-beta 8.
- C) Minimax 4, alpha-beta 9.
- D) Minimax 5, alpha-beta 9.

> [!success]- Answer
> **A.** Budget: $10\cdot 50\,000=500\,000$ nodes.
> Minimax: $20^4=160\,000$ fits, $20^5=3\,200\,000$ does not. So 4 ply.
> Alpha-beta: $d=8$ gives $20^4=160\,000$, which fits. $d=9$ gives $20^{4.5}\approx 715\,542$, which does not. So 8 ply.
> - B: Minimax would need $d\le 4.38$. Rounding up to 5 gives a depth that does not fit.
> - C: Alpha-beta would need $d\le 8.76$. Rounding up to 9 gives 715 542 nodes, above the budget.
> - D: This rounds both depths up instead of down.

### Q14 (section 5, iterative deepening)

With $b=20$, a search to depth 4 has $20^4=160\,000$ leaves. Iterative deepening first runs depths 1, 2 and 3. How many extra nodes do these earlier rounds cost, and about what fraction of the depth-4 round is that?

- A) $8000$, about 5%.
- B) $168\,420$, about 105%.
- C) $480\,000$, about 300%.
- D) $8420$, about 5%.

> [!success]- Answer
> **D.** $20+20^2+20^3=20+400+8000=8420$. Then $8420/160\,000\approx 0.053$, about 5%. The price is small, and the last finished depth always gives an answer when the clock runs out.
> - A: This counts only the depth-3 round and forgets depths 1 and 2.
> - B: This also adds the depth-4 round, which is not extra.
> - C: This assumes each earlier round costs as much as the last one: $3\cdot 160\,000$.

## 6. Several players (Ch.6-contd deck, slides 31-32)

### Q15 (section 6, tuples)

Each leaf has a tuple $(u_1,u_2,u_3)$. Player 1 moves at the root, then player 2. Left node leaves: $(4,3,1)$ and $(2,5,0)$. Right node leaves: $(3,1,6)$ and $(6,0,2)$. Which tuple reaches the root?

- A) $(6,0,2)$
- B) $(3,1,6)$
- C) $(4,3,1)$
- D) $(2,5,0)$

> [!success]- Answer
> **B.** Player 2 maximises the second component. Left: $5>3$, so $(2,5,0)$. Right: $1>0$, so $(3,1,6)$.
> Player 1 maximises the first component: $3>2$, so it goes right. Root tuple: $(3,1,6)$.
> - A: Player 2 maximises $u_1$ instead of its own $u_2$. Then the largest $u_1$ wins.
> - C: Player 2 maximises $u_3$ instead of $u_2$. It picks $(4,3,1)$ and $(3,1,6)$, and player 1 takes $(4,3,1)$.
> - D: Player 1 maximises $u_2$ instead of its own $u_1$.

### Q16 (section 6, link to minimax)

Two players, zero-sum. A node of player 2 has the leaves $(3,-3)$ and $(5,-5)$, written as $(u_1,u_2)$. Which leaf does player 2 choose, and why?

- A) $(5,-5)$: player 2 maximises $u_1$, like MAX.
- B) $(5,-5)$: player 2 minimises its own $u_2$, like MIN.
- C) $(3,-3)$: maximising $u_2$ is the same as minimising $u_1$.
- D) $(3,-3)$: maximising $u_2$ is the same as maximising $u_1$.

> [!success]- Answer
> **C.** Player 2 maximises its own component: $-3>-5$, so $(3,-3)$. In terms of $u_1$ this is $\min(3,5)=3$. Because $u_2=-u_1$, maximising $u_2$ is minimising $u_1$: a MIN node.
> - A: Player 2 does not maximise player 1's utility.
> - B: Every player maximises its own component. Minimising $u_2$ would hurt player 2.
> - D: The choice is right, but the reason is wrong. $u_2=-u_1$ turns max into min.

## 7. Expectimax (Ch.6-contd deck, slides 33-42)

### Q17 (section 7, trace)

MAX root with two chance nodes. Left: children $20,4,-2$ with probabilities $0.5,0.3,0.2$. Right: children $6,7$ with probability $\tfrac12$ each. What does expectimax play, and what does minimax play (chance nodes replaced by MIN)?

- A) Expectimax: left (value 7.33). Minimax: left.
- B) Expectimax: left (value 10.8). Minimax: left.
- C) Expectimax: left (value 7.33). Minimax: right.
- D) Expectimax: left (value 10.8). Minimax: right.

> [!success]- Answer
> **D.** Left: $0.5\cdot 20+0.3\cdot 4+0.2\cdot(-2)=10+1.2-0.4=10.8$. Right: $\tfrac12\cdot 6+\tfrac12\cdot 7=6.5$. Expectimax goes left.
> Minimax: left $=\min(20,4,-2)=-2$, right $=\min(6,7)=6$, so right.
> - A: This averages without the probabilities, $(20+4-2)/3=7.33$, and uses $\max$ instead of $\min$ for minimax.
> - B: The value is right, but minimax here uses $\max$ instead of $\min$ (20 against 7).
> - C: Minimax is right, but the left value averages without the probabilities.

### Q18 (section 7, pruning with bounds)

All utilities lie in $[0,10]$. A MAX root has two chance nodes. The first chance node is worth 7. The second has 4 children with probability $\tfrac14$ each. Can we prune the second node after seeing only the leaf 1? After seeing 1 and 3?

- A) After 1: no. After 1 and 3: yes.
- B) After 1: yes. After 1 and 3: yes.
- C) After 1: no. After 1 and 3: no.
- D) After 1: yes. After 1 and 3: no.

> [!success]- Answer
> **A.** Best case after one leaf: the unseen leaves are all 10. $(1+10+10+10)/4=7.75>7$, so the node could still win: no prune.
> Best case after two leaves: $(1+3+10+10)/4=6<7$, so even the best case loses: prune.
> - B: This fills the unseen leaves with the lower bound 0 instead of the upper bound 10.
> - C: Without bounds an average can never be pruned. Here the bound 10 is given.
> - D: This reverses the test and prunes when the best case is above 7.

## 8. Probabilities and modelling assumptions (Ch.6-contd deck, slides 43-56, 76-77)

### Q19 (section 8, opponent model)

The opponent has two moves. Its minimax move leads to value 2, the other move to value 10. The opponent plays its minimax move 80% of the time and moves uniformly at random otherwise. What is the value of this node under this model?

- A) $3.6$
- B) $2.8$
- C) $9.2$
- D) $6$

> [!success]- Answer
> **B.** The random part also picks the minimax move half the time: $P=0.8+0.2\cdot\tfrac12=0.9$, the other move $0.1$. Value: $0.9\cdot 2+0.1\cdot 10=1.8+1.0=2.8$.
> - A: This gives the minimax move only 0.8: $0.8\cdot 2+0.2\cdot 10$.
> - C: The probabilities are attached to the wrong moves: $0.1\cdot 2+0.9\cdot 10$.
> - D: This averages without the probabilities, as for a uniformly random opponent.

### Q20 (section 8, table)

Pac-Man results (5 games each):

| | Adversarial ghost | Random ghost |
|---|---|---|
| Minimax Pac-Man | won 5/5, avg. 483 | won 5/5, avg. 493 |
| Expectimax Pac-Man | won 1/5, avg. $-303$ | won 5/5, avg. 503 |

Which cell shows dangerous optimism, and which cell shows the price of pessimism?

- A) Optimism: expectimax, adversarial. Pessimism: minimax, adversarial.
- B) Optimism: expectimax, random. Pessimism: minimax, random.
- C) Optimism: expectimax, adversarial. Pessimism: minimax, random.
- D) Optimism: expectimax, random. Pessimism: minimax, adversarial.

> [!success]- Answer
> **C.** Expectimax against the adversarial ghost (1/5, $-303$): Pac-Man assumes random moves and takes risks, but the real ghost always hurts him. Minimax against the random ghost (493 instead of 503): he avoids chances that would have been safe.
> - A: Minimax against an adversarial ghost uses the right model. That cell shows no price.
> - B: Expectimax against a random ghost uses the right model. It is the best cell, not a danger.
> - D: Both cells are the ones where the model fits the world.

### Q21 (section 8, expectation)

A trip takes 10, 20 or 50 minutes with probabilities 0.2, 0.5, 0.3. What is the expected time?

- A) $26.67$ minutes
- B) $23$ minutes
- C) $20$ minutes
- D) $27$ minutes

> [!success]- Answer
> **D.** The probabilities are non-negative and $0.2+0.5+0.3=1$, so the distribution is valid. $E[T]=0.2\cdot 10+0.5\cdot 20+0.3\cdot 50=2+10+15=27$.
> - A: This averages without the probabilities: $80/3$.
> - B: The probabilities are attached in reverse order: $0.3\cdot 10+0.5\cdot 20+0.2\cdot 50$.
> - C: This is the most likely time, not the expected time.

## 9. Expectiminimax (Ch.6-contd deck, slides 57-58)

### Q22 (section 9, trace)

MAX root with two chance nodes (fair coin), each with two MIN nodes. Left chance node: MIN nodes with leaves $(4,8)$ and $(10,6)$. Right chance node: MIN nodes with leaves $(0,2)$ and $(12,14)$. Give the root value and the move.

- A) 6, go right.
- B) 4, go left.
- C) 9, go left.
- D) 12, go right.

> [!success]- Answer
> **A.** MIN layer: left $\min(4,8)=4$, $\min(10,6)=6$. Right $\min(0,2)=0$, $\min(12,14)=12$.
> Chance layer: left $\tfrac12\cdot 4+\tfrac12\cdot 6=5$. Right $\tfrac12\cdot 0+\tfrac12\cdot 12=6$. Root $\max(5,6)=6$, go right.
> - B: The chance nodes are replaced by MIN: $\min(4,6)=4$ against $\min(0,12)=0$.
> - C: MIN and MAX are swapped in the MIN layer: chance values 9 and 8.
> - D: The chance nodes are replaced by MAX: $\max(4,6)=6$ against $\max(0,12)=12$.

### Q23 (section 9, tree size)

A game uses one die (6 outcomes) and has 10 legal moves per turn. Count the positions after the sequence: our move, roll, reply, roll, move.

- A) $6000$
- B) $216\,000$
- C) $36\,000$
- D) $3600$

> [!success]- Answer
> **C.** Our move gives 10. Each "roll, then move" pair multiplies by $6\cdot 10=60$: $10\cdot 60\cdot 60=36\,000$. Without dice it is $10\cdot 10\cdot 10=1000$, so the dice make it $6^2=36$ times larger.
> - A: This counts only one roll: $10\cdot 60\cdot 10$.
> - B: This adds a roll before our first move too: $(6\cdot 10)^3$.
> - D: This leaves out the last move: $10\cdot 6\cdot 10\cdot 6$.

## 10. Utilities (Ch.6-contd deck, slides 59-74)

### Q24 (section 10, MEU)

$U(A)=10$, $U(B)=4$, $U(C)=0$. With $U$, the lottery $L=[0.6,A;\ 0.4,C]$ is worth 6 and beats $B$ for sure. Now use $U'=2U+3$. What are $U'(L)$ and $U'(B)$?

- A) $U'(L)=12$, $U'(B)=8$.
- B) $U'(L)=15$, $U'(B)=11$.
- C) $U'(L)=12$, $U'(B)=11$.
- D) $U'(L)=15$, $U'(B)=8$.

> [!success]- Answer
> **B.** $U'(A)=23$, $U'(B)=11$, $U'(C)=3$. $U'(L)=0.6\cdot 23+0.4\cdot 3=13.8+1.2=15=2\cdot 6+3$. $15>11$, so the lottery still wins. $aU+b$ with $a>0$ keeps the order.
> - A: This forgets the $+3$ everywhere and uses $2U$.
> - C: This forgets the $+3$ for the lottery only: $2\cdot 6=12$.
> - D: This forgets the $+3$ for $B$ only: $2\cdot 4=8$.

### Q25 (section 10, scale)

A MAX root has two chance nodes, and each leaf below a chance node has probability $\tfrac12$. Left leaves $(1,9)$, right leaves $(4,5)$. With $U$, left is worth 5 and right 4.5, so expectimax goes left. Which side does expectimax choose with $\sqrt{U}$, and which with $3U+2$?

- A) $\sqrt{U}$: left. $3U+2$: left.
- B) $\sqrt{U}$: right. $3U+2$: right.
- C) $\sqrt{U}$: left. $3U+2$: right.
- D) $\sqrt{U}$: right. $3U+2$: left.

> [!success]- Answer
> **D.** $\sqrt{U}$: left $(1+3)/2=2$, right $(2+2.236)/2=2.118$. Go right: the decision flips.
> $3U+2$: left $(5+29)/2=17$, right $(14+17)/2=15.5$. Go left, as before.
> - A: This takes the square root of the averages ($\sqrt5$ against $\sqrt{4.5}$). The average must be taken of the transformed leaves.
> - B: This assumes every change of scale can flip the decision. $aU+b$ with $a>0$ cannot.
> - C: This reverses the rule: only $aU+b$ with $a>0$ is safe for expectimax.

### Q26 (section 10, money)

A person has $U(x)=\sqrt{x}$. For the lottery $L=[0.25,\$1600;\ 0.75,\$0]$, find the certainty equivalent CE (the sure amount with $U(\mathrm{CE})=U(L)$) and the premium $\mathrm{EMV}-\mathrm{CE}$.

- A) CE \$100, premium \$300.
- B) CE \$10, premium \$390.
- C) CE \$20, premium \$380.
- D) CE \$400, premium \$0.

> [!success]- Answer
> **A.** $\mathrm{EMV}=0.25\cdot 1600=\$400$. $U(L)=0.25\cdot 40+0.75\cdot 0=10$. $\sqrt{\mathrm{CE}}=10$, so $\mathrm{CE}=\$100$. Premium $400-100=\$300$.
> - B: This takes $U(L)=10$ as the CE and forgets to undo the square root.
> - C: This takes $\sqrt{\mathrm{EMV}}=20$ as the CE.
> - D: This uses $U(\mathrm{EMV})=20$ instead of $U(L)$, which is the risk-neutral answer.

### Q27 (section 10, Allais)

$A=[0.8,\$4\text{k};\ 0.2,\$0]$, $B=[1.0,\$3\text{k}]$, $C=[0.2,\$4\text{k};\ 0.8,\$0]$, $D=[0.25,\$3\text{k};\ 0.75,\$0]$. A risk-neutral agent has $U(\$x)=x$. What does it choose in each pair, and is it consistent with MEU?

- A) $B$ and $C$, not consistent.
- B) $A$ and $D$, not consistent.
- C) $A$ and $C$, consistent.
- D) $B$ and $D$, consistent.

> [!success]- Answer
> **C.** $U(A)=0.8\cdot 4000=3200>3000=U(B)$: $A$. $U(C)=0.2\cdot 4000=800>750=0.25\cdot 3000=U(D)$: $C$. Both say $0.8\,U(\$4\text{k})>U(\$3\text{k})$, so it is consistent.
> - A: This is the human pattern of the paradox, not what this agent computes.
> - B: This swaps the probabilities of $D$: $0.75\cdot 3000=2250>800$.
> - D: This swaps the probabilities of $A$ and of $D$: $0.2\cdot 4000=800<3000$ and $2250>800$.

### Q28 (section 10, money pump)

An agent prefers $A\succ B\succ C\succ A$ and pays 1 cent for every swap to something it prefers. It holds $C$. What does it hold after 300 swaps, and how much has it paid?

- A) It holds $A$ and has paid \$3.
- B) It holds $C$ and has paid \$3.
- C) It holds $C$ and has paid \$0.30.
- D) It holds $A$ and has paid \$0.30.

> [!success]- Answer
> **B.** $C\to B\to A\to C$: every 3 swaps it is back at $C$. $300=3\cdot 100$, so it holds $C$. It paid $300$ cents $=\$3$.
> - A: This counts one swap too few (299 swaps end at $A$).
> - C: This converts 300 cents to \$0.30. 100 cents make one dollar.
> - D: This counts one swap too few and also converts the cents wrongly.
