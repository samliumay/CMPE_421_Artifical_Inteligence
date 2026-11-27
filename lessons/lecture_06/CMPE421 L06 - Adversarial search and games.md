---
course: CMPE 421 Artificial Intelligence
lecture: 6
title: Adversarial search and games
tags: [cmpe421, lecture]
slides: lectures/lecture_06/Ch.6.pptx
full_notes: lecture_notes/lecture_06/CMPE421_Lecture_06_Adversarial_Search.pdf
---
# CMPE 421 · Lecture 6 · Adversarial search and games

> [!info] No slide images in this note
> The two decks of this lecture exist only as PowerPoint files, so no slide pictures are embedded:
> `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6.pptx` and `Ch.6-contd.pptx` (plus `*_slim.pptx` copies).
> The slide numbers below are the ones our lecture notes give. The deck is called "Chapter 6" but follows AIMA Chapter 5 (and Chapter 16 for utilities).

> [!summary] In short
> - A game has states, players, actions, transitions, a terminal test and terminal utilities. The answer is a **policy** (state to action), not a fixed plan.
> - **Minimax**: utility at leaves, max at MAX nodes, min at MIN nodes. Time $O(b^m)$, space $O(bm)$.
> - **Alpha-beta** gives the same root value and skips branches that cannot matter. Perfect ordering: $O(b^{m/2})$, twice the depth.
> - Real games cut the search at a depth limit and score positions with an **evaluation function**.
> - **Expectimax** replaces MIN by chance nodes (weighted average). No pruning without bounds.
> - **Utilities**: rational preferences imply a utility function, act by maximum expected utility (MEU). Minimax needs only the order of utilities, expectimax needs their sizes.

## 1. Games (Ch.6 deck, slides 1-11)

**Notes.** Until now the agent was alone. In a game another agent also moves and may want the opposite.

Four axes classify a game: deterministic or stochastic, number of players, zero-sum or not, perfect information or not. Chess: deterministic, 2 players, zero-sum, perfect information. Poker: stochastic, zero-sum, imperfect information.

| | zero-sum | general | common payoff |
|---|---|---|---|
| utilities | $u_2=-u_1$ | unrelated | $u_2=u_1$ |
| numbers per outcome | 1 | one per player | 1 |
| solved by | minimax | utility tuples | max at every node |

Formal game (slide 11): states $S$ with start $s_0$, players $P=\{1,\dots,N\}$, actions $A$, transition function $S\times A\to S$, terminal test $S\to\{\text{true},\text{false}\}$, terminal utilities $S\times P\to\mathbb{R}$. A solution for a player is a **policy** $S\to A$: we need a move for whichever state the opponent leaves us in.

Milestones: Chinook (checkers, 1994), Deep Blue beat Kasparov (chess, 1997, 200 million positions per second), AlphaGo (Go, 2016, Monte Carlo tree search plus a learned evaluation function).

> [!warning] Watch out
> - Slide 9 still says the best humans beat the best Go programs. Slide 10 corrects it: AlphaGo won in 2016.
> - A **ply** is one move by one player. An everyday chess "move" (White then Black) is 2 ply.

> [!question]- Check (section 1, classify): Classify backgammon and Battleship on the four axes (deterministic or stochastic, players, zero-sum, perfect information).
> Backgammon: stochastic (dice), 2 players, zero-sum, perfect information (both see the whole board and the dice).
> Battleship: deterministic (no chance after the ships are placed), 2 players, zero-sum, imperfect information (you do not see the opponent's ships).

> [!question]- Check (section 1, payoffs and ply): An outcome gives player 1 the utility $u_1=7$. What is $u_2$ in a zero-sum game and in a common-payoff game? And how many ply are 3 full chess moves?
> Zero-sum: $u_2=-u_1=-7$. Common payoff: $u_2=u_1=7$.
> One chess move is White then Black, so 2 ply. Three moves: $3\cdot 2=6$ ply.

> [!question]- Check (section 1, policy): Why is a fixed sequence of actions not enough as the solution of a game?
> After each of our moves the opponent chooses a reply, and we cannot know which one. Each reply gives a different state, and we need a move in each of them.
> A policy $S\to A$ gives a move for every state, so it covers all replies.

## 2. Minimax (Ch.6 deck, slides 12-23)

**Notes.** The agent now maximises a score called **utility**. The value of a state is the best utility achievable from it against a rational opponent:

$$
V(s)=\begin{cases}
\text{utility}(s) & s \text{ terminal}\\
\max_{s'\in \text{succ}(s)} V(s') & \text{MAX to move}\\
\min_{s'\in \text{succ}(s)} V(s') & \text{MIN to move}
\end{cases}
$$

Here $s'$ is a state one move after $s$. Compute bottom-up. The best move is the child that carries the root value.

```python
def value(state):
    if terminal(state): return utility(state)
    if next_agent(state) == "MAX": return max_value(state)
    return min_value(state)

def max_value(state):
    v = -inf
    for s in successors(state):
        v = max(v, value(s))
    return v

def min_value(state):
    v = +inf
    for s in successors(state):
        v = min(v, value(s))
    return v
```

> [!example] Slide 23 tree (AIMA Figure 5.2)
> MAX root, three MIN nodes with leaves A $=(3,12,8)$, B $=(2,4,6)$, C $=(14,5,2)$.
> MIN layer: A $=3$, B $=2$, C $=2$. Root: $\max(3,2,2)=3$. MAX plays A and is sure of at least 3.

> [!example] Pac-Man against one ghost (slides 17-18)
> Left leaves $-8,-5$, right leaves $-10,+8$. Left MIN $=-8$, right MIN $=-10$, root $\max(-8,-10)=-8$: go left. The $+8$ is tempting, but a rational ghost would pick $-10$.

> [!question]- Check (section 2, trace): MAX root, three MIN nodes with leaves $(4,-2)$, $(1,7)$, $(-3,9)$. Give the minimax value and the best move.
> MIN layer: $\min(4,-2)=-2$, $\min(1,7)=1$, $\min(-3,9)=-3$.
> Root: $\max(-2,1,-3)=1$. MAX plays the middle move and is sure of at least 1.
> The 9 is the largest leaf, but MIN would answer that move with $-3$.

> [!question]- Check (section 2, roles swapped): Same leaves $(4,-2)$, $(1,7)$, $(-3,9)$, but now the root is a MIN node and the second layer are MAX nodes. What is the root value?
> MAX layer: $\max(4,-2)=4$, $\max(1,7)=7$, $\max(-3,9)=9$.
> Root: $\min(4,7,9)=4$. MIN plays the left move.
> Who moves at which layer changes the value: 4 here, 1 when MAX is at the root.

> [!question]- Check (section 2, code): In `max_value`, why does `v` start at `-inf` and not at 0?
> `v` must be a value that every real child beats, so that the first child always replaces it.
> With `v = 0` a node whose children are all negative (for example $-8$ and $-5$) would return 0, a value that no child has. With `-inf` it returns $-5$.

## 3. Properties of minimax (Ch.6 deck, slides 24-28)

**Notes.** Minimax is optimal against a perfect player. Against others it can be too careful. Slide 24: left leaves $10,10$, right leaves $9,100$. Minimax goes left for a sure 10. If MIN moved at random, right would be worth $\tfrac12\cdot 9+\tfrac12\cdot 100=54.5$.

Cost: minimax is a full depth-first search. With branching factor $b$ and maximum depth $m$: time $O(b^m)$, space $O(bm)$. Chess has $b\approx 35$, $m\approx 100$, so about $35^{100}\approx 2.6\times 10^{154}$ leaves. Exact solution is infeasible. Two remedies follow: pruning and depth limits.

> [!warning] Watch out
> $b^m$ is a power (enormous). $bm$ is a product (tiny). Space is small because depth-first search keeps only one path plus the waiting siblings.

> [!question]- Check (section 3, imperfect opponent): Left leaves $6,6$, right leaves $2,20$. What does minimax play? MIN plays its best move with probability $p$ and the other move otherwise. For which $p$ is going right better than going left?
> Minimax: left $=\min(6,6)=6$, right $=\min(2,20)=2$, root $\max(6,2)=6$: go left.
> Expected value of right: $2p+20(1-p)=20-18p$.
> Right is better if $20-18p>6$, so $18p<14$, so $p<\tfrac{14}{18}=\tfrac79\approx 0.78$.
> Against a uniformly random MIN ($p=\tfrac12$): $20-9=11>6$, so right is better there.

> [!question]- Check (section 3, cost): A game tree has $b=4$ and $m=10$. How many leaves does minimax visit, and about how many nodes does it hold in memory?
> Leaves: $b^m=4^{10}=1\,048\,576$, about one million.
> Memory: about $bm=4\cdot 10=40$ nodes (one path of 10 levels, with up to 4 siblings per level).
> Time grows as a power, space only as a product.

## 4. Alpha-beta pruning (Ch.6 deck, slides 29-36)

**Notes.** $\alpha$ = MAX's best option on the path to the root (a lower bound). $\beta$ = MIN's best option on the path to the root (an upper bound). At the root $\alpha=-\infty$, $\beta=+\infty$.

| | $\alpha$ | $\beta$ |
|---|---|---|
| updated at | MAX nodes: $\alpha=\max(\alpha,v)$ | MIN nodes: $\beta=\min(\beta,v)$ |
| used to prune at | MIN nodes: stop if $v\le\alpha$ | MAX nodes: stop if $v\ge\beta$ |

```python
def max_value(state, alpha, beta):
    v = -inf
    for s in successors(state):
        v = max(v, value(s, alpha, beta))
        if v >= beta: return v
        alpha = max(alpha, v)
    return v

def min_value(state, alpha, beta):
    v = +inf
    for s in successors(state):
        v = min(v, value(s, alpha, beta))
        if v <= alpha: return v
        beta = min(beta, v)
    return v
```

$\alpha$ and $\beta$ are passed down only. A node returns just $v$.

> [!example] Slide 30: alpha-beta on the slide 23 tree
> A needs all three leaves: A $=3$, so $\alpha=3$ at the root. B sees leaf 2: $2\le 3$, return, prune leaves 4 and 6. C sees 14, 5, 2 (the pruning leaf comes last). Root $=3$, 7 of 9 leaves seen.

> [!example] Quiz 1 (slide 34) and quiz 2 (slides 35-36)
> Quiz 1, tree $[[10,8],[4,50]]$: left MIN $=8$, $\alpha=8$. Right MIN sees 4, $4\le 8$, prune the 50. Root 8.
> Quiz 2, tree $[[[10,6],[100,8]],[[1,2],[20,4]]]$ (MAX, MIN, MAX): the leaf 8 is pruned ($100\ge\beta=10$), and the whole subtree $(20,4)$ is pruned ($2\le\alpha=10$). Root 10, leaves seen: 10, 6, 100, 1, 2.

Properties (slide 33): the root value is unchanged. Values of intermediate nodes may be wrong. Good child ordering helps: with perfect ordering time is $O(b^{m/2})$, which doubles the solvable depth. Alpha-beta is an example of metareasoning (computing about what to compute).

> [!warning] Watch out
> A child of the root can report a wrong value. Tree $[[10],[10,0]]$: the right child returns 10 after its first leaf ($10\le\alpha=10$), but its true value is 0. Remember which child set $\alpha$ and replace the best move only on a strictly larger value.

> [!info] Why the two pruning tests are safe (slide 31, with a window picture that goes beyond the slides)
> Take a MIN node $n$. It loops over its children, so its value $v$ can only go down.
> $\alpha$ is a value MAX can already get somewhere else on the path to the root.
> If $v\le\alpha$, then $n$ will end at or below $\alpha$, so MAX will never let the game reach $n$. The other children of $n$ cannot change that, so we skip them.
> The MAX node is the mirror image: its $v$ can only go up, and if $v\ge\beta$ then MIN, higher in the tree, will avoid it.
> Picture (beyond the slides): $(\alpha,\beta)$ is a window on the number line. Only values strictly inside the window can still change the decision at the root. A node whose value has left the window stops.

> [!question]- Check (section 4, trace): MAX root, three MIN nodes A $=(6,9,4)$, B $=(3,8,7)$, C $=(5,2,10)$. Run alpha-beta left to right. Which leaves are pruned, and how many are seen?
> A: no bound yet ($\alpha=-\infty$), so all three leaves are needed. A $=\min(6,9,4)=4$. Root: $\alpha=4$.
> B: first leaf 3, $v=3\le\alpha=4$, return. Leaves 8 and 7 are pruned.
> C: first leaf 5, $v=5>4$, go on ($\beta=5$). Second leaf 2, $v=2\le 4$, return. Leaf 10 is pruned.
> Root: $\max(4,3,2)=4$, move A. Seen: 6, 9, 4, 3, 5, 2, so 6 of 9 leaves.
> True values are B $=3$ and C $=2$. Here the returned values happen to be exact, but in general they are only upper bounds.

> [!question]- Check (section 4, ordering): Same tree, but the root visits its children in the order C, B, A. How many leaves are seen? Does the root value change?
> C $=(5,2,10)$: $\alpha=-\infty$, all three leaves, C $=2$. Root: $\alpha=2$.
> B $=(3,8,7)$: $3>2$, $\min(3,8)=3>2$, $\min(3,7)=3>2$, no prune. B $=3$. Root: $\alpha=3$.
> A $=(6,9,4)$: $6>3$, $6>3$, $4>3$, no prune. A $=4$.
> All 9 leaves are seen. The root value is still 4.
> The best child (A) came last, so $\alpha$ was never high enough to prune. Ordering changes the work, never the root value.

> [!question]- Check (section 4, find the mistake): "At a MIN node we update $\alpha$, and we stop when $v\ge\beta$." What is wrong?
> Both parts are swapped. A MIN node lowers its value, so it updates $\beta=\min(\beta,v)$, which is MIN's own guarantee.
> It stops when $v\le\alpha$, because then MAX, higher in the tree, already has something at least as good.
> Updating $\alpha$ and the test $v\ge\beta$ belong to MAX nodes.

## 5. Resource limits (Ch.6 deck, slides 37-51)

**Notes.** We cannot search to the leaves. **Depth-limited search** stops at a fixed depth and scores the cut-off positions with an evaluation function:

$$\mathit{Eval}(s)=w_1f_1(s)+w_2f_2(s)+\dots+w_nf_n(s)$$

Each feature $f_i$ is a number read off the position (for example white queens minus black queens). Each weight $w_i$ says how much it is worth. The ideal $\mathit{Eval}$ equals the true minimax value.

Consequences: the guarantee of optimal play is gone, more plies make a big difference, and **iterative deepening** (depth 1, then 2, then 3, ...) gives an anytime algorithm.

> [!example] How deep? (slide 38)
> 100 s per move at 10 000 nodes per second is $10^6$ nodes. Plain minimax with $b=35$: $35^3=42\,875$ fits, $35^4=1\,500\,625$ does not, so 3 ply. Alpha-beta with good ordering needs about $35^{d/2}$ nodes, so $d\le 7.77$, close to the slide's "depth 8".

Why Pac-Man starves (slide 46): with a depth-2 search, eating the dot now and eating it later score the same, so he may walk back and forth forever. Fix: an evaluation function that makes "now" strictly better, for example score minus distance to the nearest dot.

> [!question]- Check (section 5, evaluation): Weights: queen 9, rook 5, knight 3, pawn 1. Features (White minus Black): queens $-1$, rooks $+1$, knights $+1$, pawns $+2$. Compute $\mathit{Eval}(s)$. Who is ahead?
> $\mathit{Eval}(s)=w_1f_1+w_2f_2+w_3f_3+w_4f_4=9\cdot(-1)+5\cdot 1+3\cdot 1+1\cdot 2$
> $=-9+5+3+2=1$.
> The value is positive, so White is slightly ahead in material, although White has one queen less.

> [!question]- Check (section 5, depth): You have 10 s per move at 50 000 nodes per second, and $b=20$. How many full ply can plain minimax search? How many can alpha-beta with perfect ordering?
> Budget: $10\cdot 50\,000=500\,000$ nodes.
> Minimax needs about $20^d$ nodes: $20^4=160\,000$ fits, $20^5=3\,200\,000$ does not. So 4 ply.
> Alpha-beta with perfect ordering needs about $20^{d/2}$ nodes: $d=8$ gives $20^4=160\,000$, which fits. $d=9$ gives $20^{4.5}\approx 715\,542$, which does not. So 8 ply, twice as deep.

> [!question]- Check (section 5, iterative deepening): With $b=20$, a search to depth 4 has $20^4$ leaves. How many extra nodes do the earlier rounds (depths 1, 2, 3) cost? Why is iterative deepening still worth it in a game?
> Earlier rounds: $20+20^2+20^3=20+400+8000=8420$ nodes.
> Compared with $20^4=160\,000$: $8420/160\,000\approx 0.053$, about 5% extra.
> Benefit: the last finished depth always gives a complete answer, so we can stop when the clock runs out (anytime algorithm). The best move of depth $k$ is also a good first move to try at depth $k+1$, which helps alpha-beta.

## 6. Several players (Ch.6-contd deck, slides 31-32)

**Notes.** Not zero-sum, or more than two players: each leaf carries a **utility tuple** $(u_1,\dots,u_N)$, and each player maximises its own component. With two players and $u_2=-u_1$ this is exactly minimax. Cooperation can appear by itself when utilities happen to agree.

> [!example] Slide 32, three players
> Player 3 backs up $(1,6,6)$, $(6,1,2)$, $(5,1,7)$, $(5,2,5)$. Player 2 picks $(1,6,6)$ on the left and $(5,2,5)$ on the right. Player 1 picks $(5,2,5)$ and gets 5.

> [!question]- Check (section 6, tuples): Player 1 moves at the root, then player 2. Left node leaves: $(4,3,1)$ and $(2,5,0)$. Right node leaves: $(3,1,6)$ and $(6,0,2)$. Which tuple reaches the root?
> Player 2 maximises the second component. Left: $5>3$, so $(2,5,0)$. Right: $1>0$, so $(3,1,6)$.
> Player 1 maximises the first component: $3>2$, so it goes right. Root tuple: $(3,1,6)$.
> Player 1 gets 3. The leaves with $u_1=4$ and $u_1=6$ are not reached, because player 2 does not choose them.

> [!question]- Check (section 6, link to minimax): Two players, zero-sum. A node of player 2 has the leaves $(3,-3)$ and $(5,-5)$. Which leaf does player 2 choose, and why is this the same as a MIN node?
> Player 2 maximises its own component: $-3>-5$, so it chooses $(3,-3)$.
> In terms of $u_1$ this is $\min(3,5)=3$. Because $u_2=-u_1$, maximising $u_2$ is the same as minimising $u_1$.

## 7. Expectimax (Ch.6-contd deck, slides 33-42)

**Notes.** Outcomes are often decided by chance (dice, random ghosts, slipping wheels), not by an adversary. Then use the average, not the worst case. MAX nodes stay as in minimax. A **chance node** takes the expected value of its children:

$$V(s)=\sum_{s'}P(s')\,V(s')$$

$P(s')$ is the probability that chance picks child $s'$.

> [!example] Slides 39 and 40
> Children $8,24,-12$ with probabilities $\tfrac12,\tfrac13,\tfrac16$: $4+8-2=10$.
> Slide 40, uniform chance nodes over $(3,12,9)$, $(2,4,6)$, $(15,6,0)$: values $8,4,7$, root $8$.

No pruning (slide 41): an average depends on every child, so one unseen leaf could still raise it. Pruning is possible only if the leaf values are bounded. Depth-limited expectimax works as before, but now the sizes of the estimates matter.

> [!question]- Check (section 7, trace): MAX root with two chance nodes. Left: children $20,4,-2$ with probabilities $0.5,0.3,0.2$. Right: children $6,7$ with probability $\tfrac12$ each. What does expectimax play? What would minimax play?
> Left: $0.5\cdot 20+0.3\cdot 4+0.2\cdot(-2)=10+1.2-0.4=10.8$.
> Right: $\tfrac12\cdot 6+\tfrac12\cdot 7=6.5$.
> Expectimax: $\max(10.8,6.5)=10.8$, go left.
> Minimax: left $=\min(20,4,-2)=-2$, right $=\min(6,7)=6$, go right.
> The average accepts the small risk of $-2$. The worst case does not.

> [!question]- Check (section 7, pruning with bounds): All utilities lie in $[0,10]$. A MAX root has two chance nodes. The first chance node is worth 7. The second chance node has 4 children with probability $\tfrac14$ each. After seeing only the leaf 1, can we prune? After seeing 1 and 3?
> Best case after one leaf: the three unseen leaves are all 10. $(1+10+10+10)/4=31/4=7.75>7$. The node could still win: no prune.
> Best case after two leaves: $(1+3+10+10)/4=24/4=6<7$. Even the best case loses to 7: prune the last two leaves.
> Without the bound 10 we could never prune, because an unseen leaf could be arbitrarily large.

## 8. Probabilities and modelling assumptions (Ch.6-contd deck, slides 43-56, 76-77)

**Notes.** A random variable is a table of outcomes and probabilities. Probabilities are non-negative and sum to 1. Expected value: $E[f(X)]=\sum_x P(X=x)\,f(x)$. Airport trip of 20, 30, 60 minutes with probabilities 0.25, 0.5, 0.25: $5+15+15=35$ minutes.

The probabilities on a chance node are a **model** of the opponent or the environment. If the opponent plays its depth-2 minimax move 80% of the time and moves at random otherwise (two moves), the model is $0.8+0.2\cdot\tfrac12=0.9$ and $0.1$.

| | Adversarial ghost | Random ghost |
|---|---|---|
| Minimax Pac-Man | won 5/5, avg. 483 | won 5/5, avg. 493 |
| Expectimax Pac-Man | won 1/5, avg. $-303$ | won 5/5, avg. 503 |

Dangerous optimism: assuming chance when the world is adversarial. Dangerous pessimism: assuming the worst case when it is unlikely. Use the model that fits the world.

Two views of probability (slide 76): frequentist (averages over repeated experiments) and Bayesian (degrees of belief, updated by evidence).

> [!question]- Check (section 8, opponent model): The opponent has two moves. Its minimax move leads to value 2, the other move to value 10. Compute the value of this node under three models: pure minimax, uniformly random, and "minimax move 80% of the time, random otherwise".
> Minimax: $\min(2,10)=2$.
> Uniformly random: $\tfrac12\cdot 2+\tfrac12\cdot 10=6$.
> Mixed: the minimax move has probability $0.8+0.2\cdot\tfrac12=0.9$, the other $0.1$. Value: $0.9\cdot 2+0.1\cdot 10=1.8+1.0=2.8$.
> The mixed model is close to minimax, because the opponent is mostly rational.

> [!question]- Check (section 8, table): Which cell of the Pac-Man table shows dangerous optimism? Explain the result in that cell. Which cell shows the price of pessimism?
> Dangerous optimism: expectimax Pac-Man against the adversarial ghost (won 1/5, average $-303$). Pac-Man assumes the ghost moves at random, so he takes risks that pay off on average. The real ghost always chooses the move that hurts him.
> Price of pessimism: minimax Pac-Man against the random ghost (493) scores a little less than expectimax Pac-Man against the random ghost (503). He still wins 5/5, but he avoids chances that would have been safe.

> [!question]- Check (section 8, expectation): A trip takes 10, 20 or 50 minutes with probabilities 0.2, 0.5, 0.3. Check that this is a valid distribution and compute the expected time.
> Valid: all three are non-negative and $0.2+0.5+0.3=1$.
> $E[T]=0.2\cdot 10+0.5\cdot 20+0.3\cdot 50=2+10+15=27$ minutes.

## 9. Expectiminimax (Ch.6-contd deck, slides 57-58)

**Notes.** Backgammon has MAX, MIN and chance layers. Each node applies its own rule: max, min or expectation. Two dice give 21 distinct rolls (6 doubles plus 15 mixed), and about 20 legal moves per roll. TD-Gammon reached world-champion level with a depth-2 search, a very good evaluation function and reinforcement learning.

> [!warning] Error on slide 58
> The slide writes $20\times(21\times 20)^3=1.2\times 10^9$. The product is $20\times 420^3=1\,481\,760\,000\approx 1.5\times 10^9$.

> [!question]- Check (section 9, trace): MAX root, two chance nodes (fair coin), each with two MIN nodes. Left chance node: MIN nodes with leaves $(4,8)$ and $(10,6)$. Right chance node: MIN nodes with leaves $(0,2)$ and $(12,14)$. Give the root value and the move.
> MIN layer, left: $\min(4,8)=4$, $\min(10,6)=6$. Right: $\min(0,2)=0$, $\min(12,14)=12$.
> Chance layer: left $=\tfrac12\cdot 4+\tfrac12\cdot 6=5$. Right $=\tfrac12\cdot 0+\tfrac12\cdot 12=6$.
> Root: $\max(5,6)=6$, go right.
> A pure minimax player (chance node replaced by MIN) would compare $\min(4,6)=4$ with $\min(0,12)=0$ and go left.

> [!question]- Check (section 9, tree size): A game uses one die (6 outcomes) and has 10 legal moves per turn. Count the positions after: our move, roll, reply, roll, move. Compare with the same game without dice.
> Our move: 10. Each following "roll, then move" pair multiplies by $6\cdot 10=60$.
> With dice: $10\cdot 60\cdot 60=36\,000$.
> Without dice: $10\cdot 10\cdot 10=1000$.
> The chance layers make the tree $36$ times larger at this depth ($6^2$), and the factor grows with every extra roll.

## 10. Utilities (Ch.6-contd deck, slides 59-74)

**Notes.** A utility is a function from outcomes to real numbers that describes an agent's preferences. **MEU principle**: choose the action that maximises expected utility, given your knowledge.

A **lottery** $L=[p,A;\ (1-p),B]$ gives prize $A$ with probability $p$ and $B$ otherwise. $A\succ B$ means $A$ is preferred, $A\sim B$ means indifference.

Axioms of rationality (slide 67): orderability, transitivity, continuity, substitutability, monotonicity. An agent with a preference cycle ($A\succ B\succ C\succ A$) can be made to pay for each swap forever (the money pump).

Utility theorem (Ramsey; von Neumann and Morgenstern): if preferences satisfy the axioms, a real function $U$ exists with

$$U(A)\ge U(B)\iff A\succeq B,\qquad U([p_1,S_1;\dots;p_n,S_n])=\sum_i p_iU(S_i)$$

Scale (slides 63, 70): minimax decisions survive any strictly increasing transformation of the utilities. Expectimax decisions survive only $U'=aU+b$ with $a>0$.

> [!example] Squaring flips expectimax (slide 63)
> Leaves: left $(0,40)$, right $(20,30)$, chance $\tfrac12$ each. Before: $20$ vs $25$, go right. After squaring: $800$ vs $650$, go left. Minimax picks right both times ($0$ vs $20$, then $0$ vs $400$).

Money (slides 72-73): expected monetary value $\mathrm{EMV}(L)=pX+(1-p)Y$. People are usually **risk-averse**: $U(L)<U(\mathrm{EMV}(L))$. For $[0.5,\$1000;\ 0.5,\$0]$: EMV is \$500, the certainty equivalent is about \$400, so the insurance premium is \$100.

Allais paradox (slide 74): most people prefer $B=[1.0,\$3\text{k}]$ over $A=[0.8,\$4\text{k};\ 0.2,\$0]$, and $C=[0.2,\$4\text{k};\ 0.8,\$0]$ over $D=[0.25,\$3\text{k};\ 0.75,\$0]$. The first gives $U(\$3\text{k})>0.8\,U(\$4\text{k})$, the second gives the opposite. No utility function explains both.

> [!warning] Watch out
> An agent can be rational (consistent with MEU) without ever storing utilities or probabilities, for example a lookup table for perfect tic-tac-toe. MEU describes behaviour, not the inside of the program.

> [!info] Certainty equivalent, premium and risk attitudes (the terms are on slides 72-73, the table goes beyond the slides)
> | quantity | meaning | unit |
> |---|---|---|
> | $\mathrm{EMV}(L)$ | average money of the lottery | dollars |
> | $U(L)$ | average utility of the lottery, $pU(X)+(1-p)U(Y)$ | utility |
> | $U(\mathrm{EMV}(L))$ | utility of getting the average money for sure | utility |
> | certainty equivalent CE | the sure amount with $U(\mathrm{CE})=U(L)$ | dollars |
> | premium | $\mathrm{EMV}-\mathrm{CE}$ | dollars |
>
> Risk-averse: $U(L)<U(\mathrm{EMV}(L))$, CE below EMV, $U$ is concave (it bends down).
> Risk-neutral: equal, CE equals EMV, $U$ is linear.
> Risk-prone: $U(L)>U(\mathrm{EMV}(L))$, CE above EMV, $U$ is convex. The slides say people are often risk-prone when deep in debt.

> [!question]- Check (section 10, MEU): $U(A)=10$, $U(B)=4$, $U(C)=0$. Choose between the lottery $L=[0.6,A;\ 0.4,C]$ and $B$ for sure. Does the choice change under $U'=2U+3$?
> $U(L)=0.6\cdot 10+0.4\cdot 0=6$. $U(B)=4$. $6>4$: MEU chooses the lottery.
> New utilities: $U'(A)=23$, $U'(B)=11$, $U'(C)=3$. $U'(L)=0.6\cdot 23+0.4\cdot 3=13.8+1.2=15$, and $15>11$: still the lottery.
> Note $15=2\cdot 6+3$. A transformation $aU+b$ with $a>0$ passes through the expectation, so the order of the options stays.

> [!question]- Check (section 10, scale): Chance nodes with probability $\tfrac12$: left leaves $(1,9)$, right leaves $(4,5)$. Which side does expectimax choose with $U$, with $\sqrt{U}$, and with $3U+2$?
> $U$: left $(1+9)/2=5$, right $(4+5)/2=4.5$. Go left.
> $\sqrt{U}$: left $(1+3)/2=2$, right $(2+2.236)/2=2.118$. Go right. The decision flips.
> $3U+2$: left $(5+29)/2=17$, right $(14+17)/2=15.5$. Go left, as before.
> $\sqrt{\cdot}$ is increasing, so a minimax player would not change ($\min$ is 1 vs 4, then 1 vs 2: right both times). Only $aU+b$ with $a>0$ is safe for expectimax.

> [!question]- Check (section 10, money): A person has $U(x)=\sqrt{x}$. For $L=[0.25,\$1600;\ 0.75,\$0]$ find the EMV, $U(L)$, the certainty equivalent and the premium.
> $\mathrm{EMV}(L)=0.25\cdot 1600+0.75\cdot 0=\$400$.
> $U(L)=0.25\cdot\sqrt{1600}+0.75\cdot\sqrt{0}=0.25\cdot 40=10$.
> Certainty equivalent: $\sqrt{\mathrm{CE}}=10$, so $\mathrm{CE}=10^2=\$100$.
> Premium: $\mathrm{EMV}-\mathrm{CE}=400-100=\$300$.
> CE is below EMV, so the person is risk-averse. Check: $U(\mathrm{EMV})=\sqrt{400}=20>10=U(L)$.

> [!question]- Check (section 10, Allais): A risk-neutral agent has $U(\$x)=x$. What does it choose between $A$ and $B$, and between $C$ and $D$? Is this agent consistent with MEU?
> $U(A)=0.8\cdot 4000=3200$, $U(B)=3000$: it chooses $A$.
> $U(C)=0.2\cdot 4000=800$, $U(D)=0.25\cdot 3000=750$: it chooses $C$.
> Yes, it is consistent: both choices say $0.8\,U(\$4\text{k})>U(\$3\text{k})$.
> The paradox is the human pattern $B$ and $C$. The pattern $A$ and $C$ (or $B$ and $D$) fits a utility function.

> [!question]- Check (section 10, money pump): An agent prefers $A\succ B\succ C\succ A$ and pays 1 cent for every swap to something it prefers. It holds $C$. What happens after 3 swaps, and after 300?
> $C\to B$ (pays 1 cent), $B\to A$ (1 cent), $A\to C$ (1 cent, because $C\succ A$).
> After 3 swaps it holds $C$ again and is 3 cents poorer. After 300 swaps it has paid \$3 and still holds $C$.
> This is why transitivity is an axiom of rationality: an agent with a cycle can be drained of all its money.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Policy | A map $S\to A$: an action for every state |
| Minimax value | utility at leaves, $\max$ at MAX, $\min$ at MIN |
| Minimax cost | time $O(b^m)$, space $O(bm)$ |
| $\alpha$, $\beta$ | MAX's and MIN's best option on the path to the root |
| Pruning tests | MIN node: stop if $v\le\alpha$. MAX node: stop if $v\ge\beta$ |
| Perfect ordering | $O(b^{m/2})$, twice the depth |
| Evaluation function | $\mathit{Eval}(s)=\sum_i w_if_i(s)$ |
| Chance node | $V(s)=\sum_{s'}P(s')V(s')$ |
| Expectiminimax | max, min and chance layers together |
| Utility of a lottery | $U([p_1,S_1;\dots;p_n,S_n])=\sum_i p_iU(S_i)$ |
| Safe transformations | minimax: any increasing. Expectimax: $aU+b$, $a>0$ |
| EMV | $pX+(1-p)Y$ |
| Risk-averse | $U(L)<U(\mathrm{EMV}(L))$, certainty equivalent below EMV |

## Check yourself
> [!question]- 1. Minimax value and best move of $[[5,8,3],[6,9],[2,7,4]]$ (MAX root, MIN nodes)?
> MIN layer: $\min(5,8,3)=3$, $\min(6,9)=6$, $\min(2,7,4)=2$. Root: $\max(3,6,2)=6$, the middle move.

> [!question]- 2. In the slide 23 tree, reorder the leaves of C so that alpha-beta prunes most. How many leaves are seen?
> C's window is $(3,+\infty)$. Order $2,14,5$: first $v=2\le 3$, return at once. A needs 3 leaves, B needs 1, C needs 1: 5 of 9.

> [!question]- 3. True or false: alpha-beta may return a different root value if children are visited right to left.
> False. Pruning never changes the root's minimax value. Only which and how many leaves are pruned changes.

> [!question]- 4. Expectimax for $[[5,8,3],[6,9],[2,7,4]]$ with uniform chance nodes. Does the move differ from minimax?
> Chance values: $16/3=5.33$, $15/2=7.5$, $13/3=4.33$. Root $7.5$, the middle move. Same move as minimax, different values.

> [!question]- 5. An opponent with three moves plays its minimax move 70% of the time and moves at random otherwise. Give the three probabilities.
> Minimax move: $0.7+0.3\cdot\tfrac13=0.8$. Each other move: $0.3\cdot\tfrac13=0.1$. Check: $0.8+0.1+0.1=1$.

> [!question]- 6. With $U(x)=\sqrt{x}$, find the certainty equivalent of $[0.5,\$1000;\ 0.5,\$0]$.
> $U(L)=0.5\sqrt{1000}+0=15.81$. $\mathrm{CE}=15.81^2=\$250$. This is below the EMV of \$500, so the person is risk-averse.

## Links
- Course index: [[CMPE421 Home]]
- Next lecture: [[CMPE421 L07 - MDPs, probability, Bayes nets, HMMs]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_06_Adversarial_Search.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_06/CMPE421_Lecture_06_Adversarial_Search.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_06/CMPE421_Lecture_06_Adversarial_Search.pdf`
- Original slides (PowerPoint only): [Ch.6.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6.pptx) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6.pptx`
- Second deck: [Ch.6-contd.pptx](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6-contd.pptx) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_06/Ch.6-contd.pptx`
- Quiz: [[CMPE421 L06 - Adversarial search and games - quiz]]
