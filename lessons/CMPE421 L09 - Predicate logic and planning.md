---
course: CMPE 421 Artificial Intelligence
lecture: 9
title: Predicate logic and planning
tags: [cmpe421, lecture]
slides: lectures/lecture_07/PredicateLogic.pdf
full_notes: lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf
---
# CMPE 421 · Lecture 9 · Predicate logic and deterministic planning

> [!summary] In short
> - First-order logic (FOL) talks about objects, relations and functions, not only facts. Sentences use terms, predicates, connectives, $=$, $\forall$ and $\exists$.
> - Truth is defined in a world $M=(D,I)$: a domain plus an interpretation.
> - $\forall$ goes with $\Rightarrow$, $\exists$ goes with $\land$. Quantifier order matters when the two differ.
> - A classical planning state is a set of ground atoms. An operator has preconditions and effects. Result: $\gamma(s,a)=(s-\text{effects}^-(a))\cup\text{effects}^+(a)$.
> - Graphplan builds a planning graph (a relaxed problem) with mutex links, then extracts a plan backwards.
> - FastForward uses planning-graph heuristics in a greedy forward search.

Three decks: `PredicateLogic.pdf` (part A), `DeterministicPlanning.pdf` (part B), and the extra deck `Inference-PredicateLogic.pdf` (part C, not covered by our lecture notes).

---
## Part A. Predicate logic (deck PredicateLogic)

## 1. What FOL talks about (slides 1-2)
![[cmpe421_lecture_07_predicatelogic_p001.jpg]]
![[cmpe421_lecture_07_predicatelogic_p002.jpg]]

**Notes.** Propositional logic assumes the world contains only facts. FOL assumes the world contains **objects** (people, numbers), **relations** (x is red, x is y's brother: true or false of its objects) and **functions** (the sister of x: gives back one object). Probability theory keeps facts but adds a degree of belief. Fuzzy logic gives facts a degree of truth.

> [!question]- Check (PredicateLogic, slide 1): Object, relation or function: (a) the number 7, (b) "x is older than y", (c) "the birth year of x", (d) "x is even"? What is the test that separates a relation from a function?
> (a) Object. (b) Relation between two objects. (c) Function. (d) Relation with one argument (a property).
> Test: a relation applied to its objects is true or false. A function applied to its objects gives back one object. "The birth year of x" gives back a number, so it is a function. "x is older than y" gives back true or false, so it is a relation.

> [!question]- Check (PredicateLogic, slide 2): "I believe with 0.7 that it rains tomorrow" and "the room is warm to degree 0.7". Which row of the table fits each sentence, and what is the difference?
> The first is probability theory. The fact "it rains tomorrow" is either true or false in the world. The 0.7 is the agent's degree of belief.
> The second is fuzzy logic. The fact itself is true to a degree: the room is partly warm. Nothing is unknown here.
> First-order logic is like propositional logic in the last column: a sentence is true, false, or unknown to the agent. It differs in the middle column: it talks about objects and relations, not only facts.

## 2. Syntax (slides 3-5)
![[cmpe421_lecture_07_predicatelogic_p003.jpg]]
![[cmpe421_lecture_07_predicatelogic_p004.jpg]]
![[cmpe421_lecture_07_predicatelogic_p005.jpg]]

**Notes.** Symbols: constants ($\mathit{KingJohn}$, $2$), predicates ($\mathit{Brother}$, $>$), functions ($\mathit{Sqrt}$, $\mathit{LeftLegOf}$), variables ($x,y$), connectives, equality, quantifiers.

- **Term** = constant, variable, or $\mathit{function}(\mathit{term}_1,\dots,\mathit{term}_n)$. A term names an object.
- **Atomic sentence** = $\mathit{predicate}(\mathit{term}_1,\dots,\mathit{term}_n)$ or $\mathit{term}_1=\mathit{term}_2$. It is true or false.
- **Complex sentence**: atomic sentences joined by $\neg,\land,\lor,\Rightarrow,\Leftrightarrow$.

> [!warning] Watch out
> A predicate may contain terms, but a term never contains a predicate. $\mathit{LeftLegOf}(\mathit{Brother}(\mathit{Richard},\mathit{John}))$ is not allowed.

> [!question]- Check (PredicateLogic, slide 3): Name the kind of every symbol in $\forall x\ \mathit{Enrolled}(x,\mathit{CMPE421})\Rightarrow\ {>}(\mathit{Age}(x),18)$.
> $\forall$: quantifier. $x$: variable. $\mathit{Enrolled}$: predicate symbol (the result is true or false). $\mathit{CMPE421}$ and $18$: constant symbols. $\Rightarrow$: connective. $>$: predicate symbol. $\mathit{Age}$: function symbol (it gives back an object, a number). Brackets and commas: punctuation.
> Reading: every object enrolled in CMPE421 has an age greater than 18.

> [!question]- Check (PredicateLogic, slide 4): The slide's second example is ${>}(\mathit{Length}(\mathit{LeftLegOf}(\mathit{Richard})),\mathit{Length}(\mathit{LeftLegOf}(\mathit{KingJohn})))$. How many terms does it contain, counting the nested ones? Which symbol makes it a sentence?
> Six terms: $\mathit{Richard}$, $\mathit{LeftLegOf}(\mathit{Richard})$, $\mathit{Length}(\mathit{LeftLegOf}(\mathit{Richard}))$, and the same three for $\mathit{KingJohn}$.
> A function applied to a term is again a term, so terms can be nested.
> The predicate $>$ makes it a sentence. It takes the two length terms and gives true or false: Richard's left leg is longer than King John's.

> [!question]- Check (PredicateLogic, slide 5): The slide gives ${>}(1,2)\lor{\le}(1,2)$ and ${>}(1,2)\land\neg{>}(1,2)$. Which one is false in every world? Is the other one true in every world?
> The second has the form $A\land\neg A$. It is false in every world, whatever relation $>$ refers to.
> The first is true with the usual meaning of $>$ and $\le$. But it is not true in every world. $>$ and $\le$ are only two predicate symbols. A world may map both to relations that do not contain the pair of objects for $(1,2)$. Then both parts are false, and so is the sentence.
> Compare with ${>}(1,2)\lor\neg{>}(1,2)$: this has the form $A\lor\neg A$ and is true in every world.

## 3. Truth: worlds and models (slides 6-8)
![[cmpe421_lecture_07_predicatelogic_p006.jpg]]
![[cmpe421_lecture_07_predicatelogic_p007.jpg]]
![[cmpe421_lecture_07_predicatelogic_p008.jpg]]

**Notes.** A world is $M=(D,I)$. $D$ is the **domain**: a nonempty set of objects. $I$ is the **interpretation**: it maps constant symbols to objects, predicate symbols to relations, function symbols to functions. An atomic sentence is true in $M$ if the objects its terms refer to satisfy the relation its predicate refers to. $M$ is a model of $\alpha$ if $\alpha$ is true in $M$.

Example (slide 7): $\mathit{Brother}(\mathit{Richard},\mathit{John})$ is true if the pair (Richard the Lionheart, the evil King John) is in the brotherhood relation.

There are far too many worlds to enumerate (slide 8), so model checking by enumeration is not practical in FOL.

Counting the worlds of slide 8 for a fixed domain size $n$: a $k$-ary relation is a set of $k$-tuples. There are $n^k$ tuples, and each one is in the relation or not, so there are $2^{n^k}$ possible relations per $k$-ary predicate. Each constant symbol has $n$ possible referents. Multiply these numbers over all symbols. Then repeat for $n=1,2,3,\dots$ without end.

> [!question]- Check (PredicateLogic, slide 6): World $M$: $D=\{a,b,c\}$. $I$ maps the constant $A$ to $a$ and $B$ to $b$. $\mathit{Likes}$ is the relation $\{(a,b),(b,b),(c,a)\}$. The function $\mathit{Boss}$ maps $a\to b$, $b\to b$, $c\to a$. Which are true in $M$: (1) $\mathit{Likes}(A,B)$, (2) $\mathit{Likes}(B,A)$, (3) $\mathit{Likes}(\mathit{Boss}(A),B)$, (4) $\mathit{Boss}(B)=B$, (5) $\mathit{Likes}(A,\mathit{Boss}(A))\land\neg\mathit{Likes}(B,A)$?
> First find the object of each term, then look the tuple up in the relation.
> (1) $(a,b)$ is in $\mathit{Likes}$: true.
> (2) $(b,a)$ is not in $\mathit{Likes}$: false.
> (3) $\mathit{Boss}(A)$ refers to $b$. $(b,b)$ is in $\mathit{Likes}$: true.
> (4) $\mathit{Boss}(B)$ refers to $b$, and $B$ refers to $b$. Same object: true.
> (5) $\mathit{Likes}(A,\mathit{Boss}(A))$ is $(a,b)$: true. $\neg\mathit{Likes}(B,A)$ is true by (2). True and true: true.
> So $M$ is a model of sentences 1, 3, 4 and 5, and not a model of sentence 2.

> [!question]- Check (PredicateLogic, slide 7): Use the figure. (a) How many objects does the domain have? (b) Is $\mathit{Brother}(\mathit{John},\mathit{Richard})$ true in $M$? (c) Keep $D$, but let $I$ map $\mathit{Richard}$ to the crown. Is $\mathit{Brother}(\mathit{Richard},\mathit{John})$ still true?
> (a) Five: Richard, John, the crown and the two left legs.
> (b) True. The figure has two brother arrows, one in each direction. So the pair (the evil King John, Richard the Lionheart) is in the brotherhood relation too.
> (c) False. The sentence now talks about the pair (crown, King John). No brother arrow starts at the crown, so this pair is not in the relation.
> The same sentence is true in one world and false in another. Truth depends on the interpretation, not on the names of the symbols.

> [!question]- Check (PredicateLogic, slide 8): A vocabulary has one unary predicate $P$, one binary predicate $R$ and one constant $C$. How many worlds are there with exactly 2 domain elements? With exactly 3? Why does this make model checking impossible?
> Unary relation on $n$ objects: $2^n$ choices. Binary relation: $2^{n^2}$ choices. Referent of $C$: $n$ choices.
> $n=2$: $2^2\cdot2^4\cdot2=4\cdot16\cdot2=128$ worlds.
> $n=3$: $2^3\cdot2^9\cdot3=8\cdot512\cdot3=12\,288$ worlds.
> The outer loop on the slide runs over $n=1$ to $\infty$, so the enumeration never ends. In propositional logic $k$ symbols give only $2^k$ worlds, a finite number. FOL needs inference rules instead of enumeration.

## 4. Quantifiers (slides 9-11)
![[cmpe421_lecture_07_predicatelogic_p009.jpg]]
![[cmpe421_lecture_07_predicatelogic_p010.jpg]]
![[cmpe421_lecture_07_predicatelogic_p011.jpg]]

**Notes.** $\forall x\ P(x)$ is true in a world if $P$ holds for every object. $\exists x\ P(x)$ is true if $P$ holds for at least one object.

Order: $\forall x\,\forall y$ equals $\forall y\,\forall x$, and the same for two $\exists$. But $\exists x\,\forall y\ \mathit{Loves}(x,y)$ (one person loves everyone) is not $\forall y\,\exists x\ \mathit{Loves}(x,y)$ (everyone is loved by someone).

Duality:

$$\forall x\,P\equiv\neg\exists x\,\neg P,\qquad \exists x\,P\equiv\neg\forall x\,\neg P$$

> [!warning] Watch out
> $\forall$ goes with $\Rightarrow$, $\exists$ goes with $\land$. $\forall x\ \mathit{At}(x,\mathit{UMD})\land\mathit{Smart}(x)$ says everything is at UMD and smart (too strong). $\exists x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$ is true as soon as one object is not at UMD (too weak).

> [!question]- Check (PredicateLogic, slide 9): Domain $\{j,r,u\}$. The objects at UMD: $\{j\}$. The smart objects: $\{j,r\}$. (a) Is $\forall x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$ true? (b) Is it still true if nobody is at UMD and nobody is smart?
> (a) Write the conjunction of the three instantiations.
> $j$: at UMD and smart, true $\Rightarrow$ true is true.
> $r$: not at UMD, false $\Rightarrow$ true is true.
> $u$: not at UMD, false $\Rightarrow$ false is true.
> All three are true, so the sentence is true.
> (b) Yes. Every premise is false, so every implication is true. A universal rule says nothing about objects that do not satisfy its premise. This is called vacuous truth (beyond the slides).
> With $\land$ in place of $\Rightarrow$ the sentence in (a) is false: $r$ is not at UMD.

> [!question]- Check (PredicateLogic, slide 10): Domain $\{j,r,u\}$. Nobody is at UMD. The smart objects: $\{j\}$. Evaluate (a) $\exists x\ \mathit{At}(x,\mathit{UMD})\land\mathit{Smart}(x)$ and (b) $\exists x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$. Which one says "someone at UMD is smart"?
> (a) Disjunction of three conjunctions. The first part $\mathit{At}(x,\mathit{UMD})$ is false for $j$, $r$ and $u$. Every conjunction is false, so (a) is false. This is correct: nobody is at UMD.
> (b) For $x=r$: false $\Rightarrow$ false is true. One true instantiation is enough, so (b) is true.
> Only (a) says "someone at UMD is smart". (b) is true in a world where UMD is empty, so it does not say what we want.

> [!question]- Check (PredicateLogic, slide 11, order): Domain $\{a,b,c\}$, $\mathit{Loves}=\{(a,b),(b,c),(c,a)\}$. Evaluate $\forall y\,\exists x\ \mathit{Loves}(x,y)$ and $\exists x\,\forall y\ \mathit{Loves}(x,y)$. Does one of the two sentences always imply the other?
> $\forall y\,\exists x$: $b$ is loved by $a$, $c$ is loved by $b$, $a$ is loved by $c$. Every object is loved by someone: true.
> $\exists x\,\forall y$: $a$ loves only $b$, $b$ loves only $c$, $c$ loves only $a$. No object loves all three: false.
> $\exists x\,\forall y$ implies $\forall y\,\exists x$: if one person loves everyone, then everyone is loved by that person. The other direction fails, as this world shows.
> Reason: in $\forall y\,\exists x$ the $x$ may be a different object for each $y$. In $\exists x\,\forall y$ one fixed $x$ must work for all $y$.

> [!question]- Check (PredicateLogic, slide 11, duality): Write the negation of $\forall x\,\exists y\ \mathit{Loves}(x,y)$ with the $\neg$ directly in front of $\mathit{Loves}$. Say both sentences in English.
> Move $\neg$ inwards. Each quantifier it passes flips.
> $\neg\forall x\,\exists y\ \mathit{Loves}(x,y)\equiv\exists x\,\neg\exists y\ \mathit{Loves}(x,y)\equiv\exists x\,\forall y\ \neg\mathit{Loves}(x,y)$.
> Original: everyone loves someone. Negation: there is someone who loves nobody.
> This is De Morgan's law for quantifiers: $\forall$ is a big conjunction and $\exists$ is a big disjunction.

## 5. Translating English, equality (slides 12-13)
![[cmpe421_lecture_07_predicatelogic_p012.jpg]]
![[cmpe421_lecture_07_predicatelogic_p013.jpg]]

**Notes.** Examples from slide 12:
- Brothers are siblings: $\forall x,y\ \mathit{Brother}(x,y)\Rightarrow\mathit{Sibling}(x,y)$
- Sibling is symmetric: $\forall x,y\ \mathit{Sibling}(x,y)\Leftrightarrow\mathit{Sibling}(y,x)$
- One's mother is one's female parent: $\forall x,y\ \mathit{Mother}(x,y)\Leftrightarrow(\mathit{Female}(x)\land\mathit{Parent}(x,y))$

$\mathit{term}_1=\mathit{term}_2$ is true if both terms refer to the same object. Equality is needed to say "two different": $\exists x\,\exists y\ \mathit{Brother}(x,\mathit{Richard})\land\mathit{Brother}(y,\mathit{Richard})\land\neg(x=y)$.

The fourth example of slide 12 nests an $\exists$ inside a $\forall$. A first cousin is a child of a parent's sibling:

$$\forall x,y\ \mathit{FirstCousin}(x,y)\Leftrightarrow\exists p_x,p_y\ \mathit{Parent}(p_x,x)\land\mathit{Sibling}(p_x,p_y)\land\mathit{Parent}(p_y,y)$$

Read it as: $x$ and $y$ are first cousins exactly when some parent $p_x$ of $x$ and some parent $p_y$ of $y$ are siblings. $\mathit{Parent}(p,c)$ means $p$ is a parent of $c$.

Satisfiable and valid (slide 13): a sentence is **satisfiable** if it is true under at least one interpretation, and **valid** if it is true under every interpretation. $1=2$ looks false, but $1$ and $2$ are only constant symbols. An interpretation may map both to the same object, and then $1=2$ is true. So $1=2$ is satisfiable but not valid. $2=2$ is valid: both sides are the same term, so they refer to the same object under every interpretation.

The slide also defines $\mathit{Sibling}$ from $\mathit{Parent}$: $x$ and $y$ are siblings if they are different and share two different parents $m$ and $f$.

$$\forall x,y\ \mathit{Sibling}(x,y)\Leftrightarrow[\neg(x=y)\land\exists m\,\exists f\ \neg(m=f)\land\mathit{Parent}(m,x)\land\mathit{Parent}(f,x)\land\mathit{Parent}(m,y)\land\mathit{Parent}(f,y)]$$

> [!question]- Check (PredicateLogic, slide 12): Use $\mathit{Parent}(p,c)$ for "$p$ is a parent of $c$". (a) Write in FOL: "A grandparent is a parent of one's parent." (b) Write in FOL: "Nobody is their own parent." (c) Say in English: $\forall x,y\ \mathit{Parent}(x,y)\Rightarrow\neg\mathit{Parent}(y,x)$.
> (a) $\forall x,y\ \mathit{Grandparent}(x,y)\Leftrightarrow\exists z\ \mathit{Parent}(x,z)\land\mathit{Parent}(z,y)$. It is a definition, so it uses $\Leftrightarrow$. The person in the middle is "some $z$", so $\exists$ with $\land$.
> (b) $\forall x\ \neg\mathit{Parent}(x,x)$, or the same thing with duality: $\neg\exists x\ \mathit{Parent}(x,x)$.
> (c) If $x$ is a parent of $y$, then $y$ is not a parent of $x$. The parent relation never holds in both directions.

> [!question]- Check (PredicateLogic, slide 13): Domain $\{m,f,k_1,k_2\}$, $\mathit{Parent}=\{(m,k_1),(f,k_1),(m,k_2)\}$. (a) Is $\mathit{Sibling}(k_1,k_2)$ true by the slide's definition? (b) What changes if $\neg(m=f)$ is removed? (c) What does $\neg(x=y)$ prevent?
> (a) False. We need two different objects that are both parents of $k_1$ and of $k_2$. The parents of $k_1$ are $m$ and $f$. The only parent of $k_2$ is $m$. The pair $m,f$ fails at $\mathit{Parent}(f,k_2)$, and no other pair of different objects works.
> (b) Then the two variables $m$ and $f$ may refer to the same object. Take both as the object $m$: $\mathit{Parent}(m,k_1)$ and $\mathit{Parent}(m,k_2)$ hold, so $\mathit{Sibling}(k_1,k_2)$ becomes true. One shared parent would be enough (half-siblings).
> (c) Without it everybody with two parents is their own sibling: $\mathit{Sibling}(k_1,k_1)$ would be true with the parents $m$ and $f$.
> Two different variables do not mean two different objects. Only $\neg(x=y)$ says that.

## 6. Substitutions, numbers, sets, lists (slides 14-16)
![[cmpe421_lecture_07_predicatelogic_p014.jpg]]
![[cmpe421_lecture_07_predicatelogic_p015.jpg]]
![[cmpe421_lecture_07_predicatelogic_p016.jpg]]

**Notes.** A **substitution** is a set of variable bindings. These slides write $\theta=\{x=1,\ y=f(z)\}$ (Russell and Norvig write $\{x/1,\ y/f(z)\}$). $S\theta$ is the result of applying $\theta$ to sentence $S$. Bindings are applied simultaneously.

A **unifier** of $p$ and $q$ is a substitution with $p\theta=q\theta$ (our notes add this, the deck covers it in part C).

Natural numbers (slide 15): $\mathit{NatNum}(0)$, and $\forall n\ \mathit{NatNum}(n)\Rightarrow\mathit{NatNum}(S(n))$, where $S$ is the successor function. Addition: $+(0,m)=m$ and $+(S(m),n)=S(+(m,n))$.

The two other axioms of slide 15 make all numbers different. $\forall n\ 0\neq S(n)$: zero is not the successor of any number. $\forall m,n\ m\neq n\Rightarrow S(m)\neq S(n)$: different numbers have different successors. Without them a world with one single object, where $S(0)=0$, would satisfy the first two axioms. The numbers are the terms $0$, $S(0)$, $S(S(0))$, ... (0, 1, 2, ...).

The last lines of slide 15: these axioms are good for proofs, but slow for computing. A real system calls the machine's adder. The adder is a problem when the expression is only partly instantiated: it can compute $2+1$, but it cannot solve $x+1=3$ for an unbound variable $x$. The axioms can.

> [!info] Ground terms, sets and lists (beyond the slides)
> A **ground term** is a term without variables, for example $S(S(0))$ or $\mathit{LeftLegOf}(\mathit{John})$. A ground sentence has no variables either.
> Slide 16 only says that the book has axioms for finite sets and finite lists. The idea for lists (Russell and Norvig, Section 8.3.3): one constant $\mathit{Nil}$ for the empty list and one function $\mathit{Cons}(x,l)$ that puts element $x$ in front of list $l$. The list $[A,B]$ is the term $\mathit{Cons}(A,\mathit{Cons}(B,\mathit{Nil}))$. Every finite list is a ground term built this way, just as every number is built from $0$ and $S$.
> Membership can be defined with two sentences: $\forall x,l\ \mathit{Member}(x,\mathit{Cons}(x,l))$ and $\forall x,y,l\ \mathit{Member}(x,l)\Rightarrow\mathit{Member}(x,\mathit{Cons}(y,l))$.
> Sets work the same way with the empty set $\{\}$ and a function that adds one element. Prolog programs are sentences of this kind, and running a program is inference. The wumpus slides use the list notation $[s,b,g,m,c]$ for a percept.

> [!question]- Check (PredicateLogic, slide 14): $S=P(x,f(y),z)$ and $\sigma=\{x=y,\ y=g(z),\ z=A\}$. Compute $S\sigma$. What wrong result comes out if the bindings are applied one after another, from left to right? Is $S\sigma$ ground?
> Simultaneous: replace every variable of the original $S$ once. $x$ becomes $y$, $y$ becomes $g(z)$, $z$ becomes $A$.
> $S\sigma=P(y,f(g(z)),A)$.
> One after another (wrong): $P(y,f(y),z)$, then $P(g(z),f(g(z)),z)$, then $P(g(A),f(g(A)),A)$.
> $S\sigma$ is not ground. It still contains the variables $y$ and $z$. They came in through the bindings and are not replaced again.

> [!question]- Check (PredicateLogic, slide 15): Compute $2+1$ with the two addition axioms, that is $+(S(S(0)),S(0))$. Which axiom is used in each step?
> Step 1, axiom $+(S(m),n)=S(+(m,n))$ with $m=S(0)$, $n=S(0)$: $+(S(S(0)),S(0))=S(+(S(0),S(0)))$.
> Step 2, same axiom with $m=0$, $n=S(0)$: $=S(S(+(0,S(0))))$.
> Step 3, axiom $+(0,m)=m$ with $m=S(0)$: $=S(S(S(0)))$.
> $S(S(S(0)))$ is the number 3. Three inference steps for one small sum: this is why the slide calls the axioms computationally inefficient.

> [!question]- Check (PredicateLogic, slide 16): Write the list $[A,B,C]$ as a term with $\mathit{Cons}$ and $\mathit{Nil}$. Then show with the two $\mathit{Member}$ sentences from the callout above that $\mathit{Member}(B,[A,B,C])$ holds.
> $[A,B,C]=\mathit{Cons}(A,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil})))$. It is a ground term.
> Step 1, first sentence with $\{x=B,\ l=\mathit{Cons}(C,\mathit{Nil})\}$: $\mathit{Member}(B,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil})))$. $B$ is the first element of $[B,C]$.
> Step 2, second sentence with $\{x=B,\ y=A,\ l=\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))\}$: its premise is the result of step 1, so $\mathit{Member}(B,\mathit{Cons}(A,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))))$ follows.
> A Prolog program for `member` has exactly these two clauses.

## 7. Using a FOL knowledge base (slides 17-20)
![[cmpe421_lecture_07_predicatelogic_p017.jpg]]
![[cmpe421_lecture_07_predicatelogic_p018.jpg]]
![[cmpe421_lecture_07_predicatelogic_p019.jpg]]
![[cmpe421_lecture_07_predicatelogic_p020.jpg]]

**Notes.** `Tell(KB, S)` adds sentence $S$. `Ask(KB, S)` returns some or all substitutions $\sigma$ with $KB\models S\sigma$.

Wumpus KB (slide 18): perception rule $\forall t,s,g,m,c\ \mathit{Percept}([s,\mathit{Breeze},g,m,c],t)\Rightarrow\mathit{Breeze}(t)$, reflex rule $\forall t\ \mathit{Glitter}(t)\Rightarrow\mathit{BestAction}(\mathit{Grab},t)$, and a successor state axiom

$$\forall t\ \mathit{HaveArrow}(t+1)\Leftrightarrow[\mathit{HaveArrow}(t)\land\neg\mathit{Action}(\mathit{Shoot},t)]$$

One quantified sentence replaces one propositional sentence per square or per time step.

> [!warning] Errors on the slides
> - Slide 19: the brackets of the adjacency axiom do not match, and the pit axiom is printed without $\exists r$.
> - Slide 17 asks for $\mathit{Action}(a,5)$, but the rules on slide 18 conclude $\mathit{BestAction}(\mathit{Grab},t)$.

Slide 19 with the brackets repaired and the missing quantifier added (the intended axioms, as in Russell and Norvig):

$$\forall x,y,a,b\ \mathit{Adjacent}([x,y],[a,b])\Leftrightarrow\big(x=a\land(y=b-1\lor y=b+1)\big)\lor\big(y=b\land(x=a-1\lor x=a+1)\big)$$

$$\forall s\ \mathit{Breezy}(s)\Leftrightarrow\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)$$

Two squares are adjacent if they share one coordinate and differ by 1 in the other. A square is breezy exactly when some adjacent square has a pit. The other two axioms of the slide: the agent is in only one square at a time ($\mathit{At}(x,s_1,t)\land\mathit{At}(x,s_2,t)\Rightarrow s_1=s_2$), and a breeze perceived at time $t$ makes the agent's square breezy ($\mathit{At}(\mathit{Agent},x,t)\land\mathit{Breeze}(t)\Rightarrow\mathit{Breezy}(x)$). $\mathit{Breeze}(t)$ is about a time, $\mathit{Breezy}(x)$ is about a square.

> [!question]- Check (PredicateLogic, slide 17): A KB contains only $\mathit{Brother}(\mathit{Richard},\mathit{John})$, $\mathit{Brother}(\mathit{John},\mathit{Richard})$ and $\mathit{King}(\mathit{John})$. What do these calls return: (a) `Ask(KB,` $\exists x\ \mathit{Brother}(x,\mathit{John})$`)`, (b) `Ask(KB,` $\mathit{Brother}(x,y)\land\mathit{King}(y)$`)`, (c) `Ask(KB,` $\mathit{King}(\mathit{Richard})$`)`?
> (a) Yes, with $\sigma=\{x=\mathit{Richard}\}$. $\mathit{Brother}(\mathit{Richard},\mathit{John})$ is in the KB, so the KB entails $S\sigma$.
> (b) Yes, with $\sigma=\{x=\mathit{Richard},\ y=\mathit{John}\}$. The other brother fact gives $y=\mathit{Richard}$, but $\mathit{King}(\mathit{Richard})$ is not entailed, so that binding is not returned.
> (c) No substitution: the KB does not entail $\mathit{King}(\mathit{Richard})$. This does not mean the KB says Richard is not a king. $\neg\mathit{King}(\mathit{Richard})$ is not entailed either.
> The substitution (binding list) is more useful than a plain "yes": it tells the agent which object makes the query true.

> [!question]- Check (PredicateLogic, slide 18, percept): The agent does `Tell(KB,` $\mathit{Percept}([\mathit{Stench},\mathit{Breeze},\mathit{Glitter},\mathit{None},\mathit{None}],5)$`)`. Which substitution makes the second perception rule fire, and what follows from it and the reflex rule?
> Match $\mathit{Percept}([s,b,\mathit{Glitter},m,c],t)$ with the percept: $\{s=\mathit{Stench},\ b=\mathit{Breeze},\ m=\mathit{None},\ c=\mathit{None},\ t=5\}$. The third element is the constant $\mathit{Glitter}$ in both, so the match works.
> The rule gives $\mathit{Glitter}(5)$.
> Reflex rule with $\{t=5\}$: $\mathit{Glitter}(5)\Rightarrow\mathit{BestAction}(\mathit{Grab},5)$, so $\mathit{BestAction}(\mathit{Grab},5)$.
> `Ask(KB,` $\exists a\ \mathit{BestAction}(a,5)$`)` then returns $\{a=\mathit{Grab}\}$.

> [!question]- Check (PredicateLogic, slide 18, arrow): $\mathit{HaveArrow}(3)$ is true. The agent shoots at $t=3$ and does not shoot at $t=4$. Use the successor state axiom to find $\mathit{HaveArrow}(4)$ and $\mathit{HaveArrow}(5)$. Why does the axiom use $\Leftrightarrow$ and not $\Rightarrow$?
> $t=3$: $\mathit{HaveArrow}(4)\Leftrightarrow[\mathit{HaveArrow}(3)\land\neg\mathit{Action}(\mathit{Shoot},3)]$ = true $\land$ false = false.
> $t=4$: $\mathit{HaveArrow}(5)\Leftrightarrow[\mathit{HaveArrow}(4)\land\neg\mathit{Action}(\mathit{Shoot},4)]$ = false $\land$ true = false. A lost arrow never comes back.
> With only $\Rightarrow$ (from left to right) the KB could not conclude that the agent still has the arrow after a step without shooting. With only $\Leftarrow$ it could not conclude that the arrow is gone after shooting. $\Leftrightarrow$ gives both: the right side lists all the ways the fact can be true at $t+1$.

> [!question]- Check (PredicateLogic, slide 19): Use the repaired axioms above. (a) Is $\mathit{Adjacent}([2,3],[2,4])$ true? $\mathit{Adjacent}([2,3],[3,4])$? (b) The agent knows $\neg\mathit{Breezy}([1,1])$. What follows about pits, and which direction of the $\Leftrightarrow$ is used? (c) What is wrong with the pit axiom as printed, without $\exists r$?
> (a) $[2,3],[2,4]$: $x=a$ ($2=2$) and $y=b-1$ ($3=4-1$): true. $[2,3],[3,4]$: $x\neq a$ and $y\neq b$, both parts fail: false. Diagonal squares are not adjacent.
> (b) The direction $[\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)]\Rightarrow\mathit{Breezy}(s)$, read backwards (contrapositive): not breezy, so no adjacent square has a pit. The squares adjacent to $[1,1]$ inside the grid are $[1,2]$ and $[2,1]$. So $\neg\mathit{Pit}([1,2])$ and $\neg\mathit{Pit}([2,1])$.
> (c) $r$ is not bound by any quantifier, so it is a free variable and the sentence has no clear meaning. If $r$ is read as "for all $r$", the axiom says that a breezy square has a pit in every square and every square is adjacent to it. That is wrong. The meaning is "some adjacent square has a pit", so $\exists r$.

> [!question]- Check (PredicateLogic, slide 20): The summary says FOL has more expressive power than propositional logic. In a $4\times4$ wumpus world, how many propositional sentences of the kind $B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$ are needed for "a square is breezy exactly when an adjacent square has a pit"? How many FOL sentences?
> Propositional logic has no variables for squares. It needs one sentence per square: $4\cdot4=16$ sentences.
> FOL needs one: $\forall s\ \mathit{Breezy}(s)\Leftrightarrow\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)$ (plus the definition of $\mathit{Adjacent}$, which is also written once).
> The FOL sentence does not change when the grid grows. The number of propositional sentences grows with the number of squares, and for facts that change over time also with the number of time steps.

---
## Part B. Deterministic planning (deck DeterministicPlanning)

## 8. Blocks world and states (slides 1-5)
![[cmpe421_lecture_07_deterministicplanning_p001.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p002.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p003.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p004.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p005.jpg]]

**Notes.** The planning language is FOL without function symbols, plus operator names. Blocks world predicates: `ontable(x)`, `on(x,y)`, `clear(x)`, `holding(x)`, `handempty`.

A **state** is a set of ground atoms that are currently true. An atom not in the set is false. With $c$ constants and $p$ predicates of $k$ arguments there are $p\,c^k$ ground atoms and $2^{p\,c^k}$ possible states: finite.

> [!info] Two different counts of states (beyond the slides)
> Slide 2 says "more than $n!$ states" and slide 5 says "$2^{p\,c^k}$ states". They count different things.
> - $2^{p\,c^k}$ counts every set of ground atoms. Most of these sets describe nothing physical, for example a set with both `handempty` and `holding(a)`.
> - "More than $n!$" counts the physically possible situations. $n$ blocks can be put in one tower in $n!$ orders, and there are more situations with several towers or a held block.
>
> The formula $p\,c^k$ assumes that all $p$ predicates have the same number $k$ of arguments. In the blocks world the arities differ, so count each predicate alone: `on` has $c^2$ ground atoms, `ontable`, `clear`, `holding` have $c$ each, `handempty` has 1.

> [!question]- Check (DeterministicPlanning, slide 1): The slide compares three ways to choose actions. What is good and what is missing in (a) the search algorithms of Chapter 3 and (b) the logical agent of Chapter 7? What does classical planning take from each?
> (a) Search algorithms are domain-independent: the same A* works for any problem. But each problem has its own state representation, so the heuristic must be written by hand for each domain.
> (b) The logical agent has one standard language (logic), so general inference methods work. But it needs a huge number of rules (one copy per square and per time step), and inference becomes very slow.
> Classical planning takes the standard representation idea from logic (states and actions written in one fixed language) and the search idea from Chapter 3. Because the representation is always the same, the algorithms and also the heuristics can be domain-independent.

> [!question]- Check (DeterministicPlanning, slide 2): The slide says that $n$ blocks give more than $n!$ states. Count the physically possible states for $n=3$ blocks a, b, c (positions on the table are ignored) and compare with $3!$.
> Hand empty:
> - all three on the table: 1
> - one tower of two and one single block: choose the top and the bottom in order, $3\cdot2=6$
> - one tower of three: $3!=6$
>
> That is $1+6+6=13$ states.
> Hand holds one block: 3 choices for the held block. The other two blocks are both on the table, or one is on the other (2 ways): 3 arrangements. $3\cdot3=9$ states.
> Total: $13+9=22$ states, and $22>3!=6$.
> The towers of three alone already give $3!$ states. This is where the $n!$ comes from.

> [!question]- Check (DeterministicPlanning, slide 3): The planning language has no function symbols but unlimited variable symbols. (a) What would go wrong with a function symbol, for example $\mathit{top}(x)$? (b) Why do infinitely many variables not make the number of states infinite?
> (a) A function symbol makes infinitely many ground terms: $\mathit{top}(a)$, $\mathit{top}(\mathit{top}(a))$, and so on. Then there are infinitely many ground atoms and infinitely many states. A search could run forever. Slide 16 says that planning then becomes undecidable.
> (b) A state contains only ground atoms, and a ground atom has no variables. Ground atoms are built from the predicates and the constants only, and both sets are finite. Variables appear only in operators. They are replaced by constants before an action is executed.

> [!question]- Check (DeterministicPlanning, slide 4): Use the symbols of the slide (blocks a to e). Which of these are atoms, and which are ground: (a) `on(c,a)`, (b) `on(c,x)`, (c) `handempty`, (d) $\neg$`clear(a)`? How many ground instances does `on(c,x)` have?
> (a) Atom, ground (no variable).
> (b) Atom, unground (variable $x$).
> (c) Atom, ground. It is a predicate with zero arguments.
> (d) Not an atom. It is a negative literal: the negation of the ground atom `clear(a)`.
> `on(c,x)` has 5 ground instances, one per constant: `on(c,a)`, `on(c,b)`, `on(c,c)`, `on(c,d)`, `on(c,e)`. `on(c,c)` is a correct ground atom of the language. It is only never true in a real situation.

> [!question]- Check (DeterministicPlanning, slide 5): (a) Write the state for 3 blocks where b is on a, a and c are on the table and the hand is empty. (b) Is $\{$`holding(a)`, `handempty`, `on(a,a)`$\}$ a state by the definition of the slide? (c) For 3 blocks the formula gives $2^{19}=524\,288$ states. Only 22 can be reached in the real blocks world. Why is the formula still useful?
> (a) $\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. `clear(a)` is not in the set, so it is false: b is on a.
> (b) Yes. The definition only asks for a set of ground atoms. The formula $2^{p\,c^k}$ counts this set too, but no sequence of actions can produce it from a real situation.
> (c) The formula is an upper bound. It proves that the number of states is finite, so a search with loop-checking always ends. The exact number of reachable states is not needed for that.

## 9. Operators and actions (slides 6-11)
![[cmpe421_lecture_07_deterministicplanning_p006.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p007.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p008.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p009.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p010.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p011.jpg]]

**Notes.** An **operator** is (head, preconditions, effects). An **action** is a ground instance of an operator.

| Operator | Precond | Effects |
|---|---|---|
| `unstack(x,y)` | `on(x,y)`, `clear(x)`, `handempty` | $\neg$`on(x,y)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)`, `clear(y)` |
| `stack(x,y)` | `holding(x)`, `clear(y)` | $\neg$`holding(x)`, $\neg$`clear(y)`, `on(x,y)`, `clear(x)`, `handempty` |
| `pickup(x)` | `ontable(x)`, `clear(x)`, `handempty` | $\neg$`ontable(x)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)` |
| `putdown(x)` | `holding(x)` | $\neg$`holding(x)`, `ontable(x)`, `clear(x)`, `handempty` |

For a set of literals $S$, $S^+$ holds the atoms that appear positively and $S^-$ those that appear negatively. Action $a$ is **executable** in $s$ if

$$\text{precond}^+(a)\subseteq s\quad\text{and}\quad\text{precond}^-(a)\cap s=\emptyset$$

Result of performing it:

$$\gamma(s,a)=\big(s-\text{effects}^-(a)\big)\cup\text{effects}^+(a)$$

> [!warning] Watch out
> - Test executability first, then delete, then add.
> - Slide 11 prints the result with an extra `clear(b)` that is missing from the state printed above it. The state has lost `clear(b)` (slides 13 and 15 list it).

> [!info] Add list and delete list (beyond the slides)
> Many books split the effects in two lists. The **add list** is $\text{effects}^+(a)$: the atoms the action makes true. The **delete list** is $\text{effects}^-(a)$: the atoms the action makes false. An atom that is in neither list keeps its old value. So an operator only has to name what changes.

> [!question]- Check (DeterministicPlanning, slide 6): Find the mistake in this operator: head `lift(x)`, Precond: `on(x,y)`, `clear(x)`, `handempty`, Effects: $\neg$`on(x,y)`, `holding(x)`, `clear(y)`. Which rule of the slide is broken, and why does the rule exist?
> The variable $y$ is used in the preconditions and effects but is not in the parameter list. The slide says that the parameter list must include all of the operator's variables.
> Reason: an action is made by replacing the parameters by constants. If $y$ is not a parameter, `lift(c)` does not say which block c is lifted from, so its preconditions and effects are not fixed. The correct head is `lift(x,y)`. This is the `unstack(x,y)` of slide 7.

> [!question]- Check (DeterministicPlanning, slide 7): (a) `pickup(x)` and `unstack(x,y)` both end with `holding(x)`. Why does `unstack` have the extra effect `clear(y)` and `pickup` has no such effect? (b) How many ground actions do the four operators have for 5 blocks?
> (a) `unstack(x,y)` takes $x$ from the top of block $y$. After that nothing is on $y$, so `clear(y)` becomes true. `pickup(x)` takes $x$ from the table. The table has no `clear` atom (it is infinitely wide), so nothing else changes.
> (b) Replace each parameter by one of the 5 constants.
> `unstack`: $5\cdot5=25$, `stack`: $5\cdot5=25$, `pickup`: 5, `putdown`: 5. Total $25+25+5+5=60$.
> 10 of them have $x=y$, for example `unstack(a,a)`. They can never be executed in a real situation. Without them: $20+20+5+5=50$.

> [!question]- Check (DeterministicPlanning, slide 8): Write the action `stack(b,c)`: give the substitution and the ground preconditions and effects.
> Substitution on `stack(x,y)`: $\{x=\text{b},\ y=\text{c}\}$.
> Precond: `holding(b)`, `clear(c)`.
> Effects: $\neg$`holding(b)`, $\neg$`clear(c)`, `on(b,c)`, `clear(b)`, `handempty`.
> Every $x$ becomes b and every $y$ becomes c, in the preconditions and in the effects.

> [!question]- Check (DeterministicPlanning, slide 9): Give $\text{precond}^+$, $\text{precond}^-$, $\text{effects}^+$ and $\text{effects}^-$ of the action `putdown(e)`.
> `putdown(e)`: Precond `holding(e)`. Effects $\neg$`holding(e)`, `ontable(e)`, `clear(e)`, `handempty`.
> $\text{precond}^+=\{$`holding(e)`$\}$, $\text{precond}^-=\emptyset$ (no negative precondition).
> $\text{effects}^+=\{$`ontable(e)`, `clear(e)`, `handempty`$\}$, $\text{effects}^-=\{$`holding(e)`$\}$.
> The sets contain atoms, without the $\neg$ sign. The sign only decides in which set the atom goes.

> [!question]- Check (DeterministicPlanning, slide 10): $s=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. Which of these actions are executable in $s$: `unstack(b,a)`, `pickup(a)`, `pickup(c)`, `stack(c,b)`? Which of the four operators are applicable to $s$?
> All four blocks-world operators have only positive preconditions, so the test is $\text{precond}^+(a)\subseteq s$.
> - `unstack(b,a)`: needs `on(b,a)`, `clear(b)`, `handempty`. All in $s$: executable.
> - `pickup(a)`: needs `ontable(a)`, `clear(a)`, `handempty`. `clear(a)` is not in $s$: not executable.
> - `pickup(c)`: needs `ontable(c)`, `clear(c)`, `handempty`. All in $s$: executable.
> - `stack(c,b)`: needs `holding(c)`, `clear(b)`. `holding(c)` is not in $s$: not executable.
>
> Applicable operators: `unstack` and `pickup` (each has an executable ground instance). `stack` and `putdown` are not applicable: every instance needs a `holding` atom, and $s$ has none.

> [!question]- Check (DeterministicPlanning, slide 11): Take $s$ from the question on slide 10. (a) Compute $s_1=\gamma(s,$`pickup(c)`$)$ and $s_2=\gamma(s_1,$`stack(c,b)`$)$. (b) What comes out if you use the formula for `pickup(a)` in $s$ without the executability test?
> (a) `pickup(c)`: delete `ontable(c)`, `clear(c)`, `handempty`. Add `holding(c)`.
> $s_1=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `holding(c)`$\}$.
> `stack(c,b)` is executable in $s_1$ (`holding(c)` and `clear(b)` are there). Delete `holding(c)`, `clear(b)`. Add `on(c,b)`, `clear(c)`, `handempty`.
> $s_2=\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(c)`, `handempty`$\}$: the tower c on b on a.
> (b) Delete `ontable(a)`, `clear(a)` (not there), `handempty`. Add `holding(a)`. Result: $\{$`on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `holding(a)`$\}$.
> This says that the hand holds a while b is still on a. The model does not allow this. The formula for $\gamma$ is only defined when the action is executable.

## 10. Plans, problems, complexity (slides 12-16)
![[cmpe421_lecture_07_deterministicplanning_p012.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p013.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p014.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p015.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p016.jpg]]

**Notes.** A **plan** $\pi=(a_1,\dots,a_n)$ is executable in $s_0$ if each action is executable in the state produced by the previous one. A **planning problem** is $P=(O,s_0,g)$: operators, initial state, goal (a set of literals). $\pi$ is a **solution** if it is executable in $s_0$ and $\gamma(s_0,\pi)$ satisfies $g$.

> [!example] Slide 15
> $s_0=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `handempty`$\}$, $g=\{$`on(a,b)`$\}$.
> One solution: `unstack(c,a)`, `putdown(c)`, `pickup(a)`, `stack(a,b)`.

Complexity (slide 16): deciding whether a classical (function-free) problem has a solution is PSPACE-complete. With function symbols it is undecidable.

> [!info] Reading the complexity slide (beyond the slides)
> - **PSPACE**: problems that can be solved with a polynomial amount of memory (time may be exponential). NP is inside PSPACE. "Much harder than NP-complete" is what people believe, it is not proved.
> - Why planning is harder than checking a guess: the shortest solution plan can have exponentially many actions (in the size of the problem description). Then even writing the plan down takes exponential time.
> - **Undecidable**: no algorithm gives the correct yes/no answer for every input and always stops.
> - **NEXPTIME**: solvable in exponential time by a machine that may guess.

> [!question]- Check (DeterministicPlanning, slide 12): $s_0=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. Is the plan $\pi=($`pickup(c)`, `stack(c,b)`, `pickup(a)`$)$ executable in $s_0$? What is $\gamma(s_0,\pi)$?
> $s_1=\gamma(s_0,$`pickup(c)`$)=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `holding(c)`$\}$. Executable.
> $s_2=\gamma(s_1,$`stack(c,b)`$)=\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(c)`, `handempty`$\}$. Executable.
> `pickup(a)` needs `ontable(a)`, `clear(a)`, `handempty`. `clear(a)` is not in $s_2$. Not executable.
> One action fails, so the whole plan is not executable in $s_0$, and $\gamma(s_0,\pi)$ is not defined. The first two actions alone form an executable plan with result $s_2$.

> [!question]- Check (DeterministicPlanning, slide 13): Follow the slide. (a) Write $s_2$ and $s_4$ as sets of atoms. (b) What happens if `putdown(c)` and `pickup(b)` change places in the plan?
> $s_0=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `handempty`$\}$.
> $s_1$ (after `unstack(c,a)`) $=\{$`ontable(a)`, `clear(a)`, `ontable(b)`, `clear(b)`, `holding(c)`$\}$.
> (a) $s_2$ (after `putdown(c)`) $=\{$`ontable(a)`, `clear(a)`, `ontable(b)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$: 7 atoms, all blocks on the table.
> $s_3$ (after `pickup(b)`) $=\{$`ontable(a)`, `clear(a)`, `ontable(c)`, `clear(c)`, `holding(b)`$\}$.
> $s_4$ (after `stack(b,a)`) $=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$: 6 atoms.
> (b) Then `pickup(b)` must be executed in $s_1$. It needs `handempty`, and $s_1$ has `holding(c)` and no `handempty`. Not executable. The same actions in another order are not an executable plan.

> [!question]- Check (DeterministicPlanning, slide 14): Take $s_4$ of slide 13 (b on a, c on the table, hand empty). (a) Is $s_4$ a goal state for $g_1=\{$`on(b,a)`, `clear(c)`$\}$? For $g_2=\{$`on(b,a)`, $\neg$`handempty`$\}$? (b) Is the empty plan a solution of $(O,s_0,\{$`ontable(a)`$\})$ with $s_0$ from slide 13?
> (a) $g_1$: both atoms are in $s_4$: goal state. A goal is a set of literals, it does not have to describe the whole state. Many states satisfy $g_1$.
> $g_2$: the negative literal $\neg$`handempty` asks that `handempty` is not in the state. It is in $s_4$: not a goal state.
> (b) Yes. The empty plan is executable in every state and $\gamma(s_0,())=s_0$. `ontable(a)` is in $s_0$, so $s_0$ already satisfies the goal.

> [!question]- Check (DeterministicPlanning, slide 15): Same $s_0$ and $g=\{$`on(a,b)`$\}$ as on the slide. (a) Is $\pi'=($`unstack(c,a)`, `stack(c,b)`, `pickup(a)`, `stack(a,c)`$)$ a solution? (b) Can a solution be shorter than 4 actions?
> (a) Every action is executable:
> after `unstack(c,a)`: $\{$`ontable(a)`, `clear(a)`, `ontable(b)`, `clear(b)`, `holding(c)`$\}$,
> after `stack(c,b)`: $\{$`ontable(a)`, `clear(a)`, `ontable(b)`, `on(c,b)`, `clear(c)`, `handempty`$\}$,
> after `pickup(a)`: $\{$`ontable(b)`, `on(c,b)`, `clear(c)`, `holding(a)`$\}$,
> after `stack(a,c)`: $\{$`ontable(b)`, `on(c,b)`, `on(a,c)`, `clear(a)`, `handempty`$\}$.
> The result has `on(a,c)` but not `on(a,b)`. $\pi'$ is executable but it is not a solution.
> (b) No. `on(a,b)` can only come from `stack(a,b)`, which needs `holding(a)`. Block a is on the table, so that needs `pickup(a)`, which needs `clear(a)` and `handempty`. So c must first be taken off a (`unstack(c,a)`), and then the hand must be emptied without covering b (`putdown(c)`). That is 4 actions, and the plan of the slide is the only solution of length 4.

> [!question]- Check (DeterministicPlanning, slide 16): (a) Why is "does a classical planning problem have a solution?" decidable at all? (b) Why can this change when function symbols are added? (c) With function symbols the question "is there a solution of length $k$ or less?" is decidable again. What does the bound $k$ give?
> (a) A classical problem has finitely many ground atoms, so finitely many states (at most $2^{p\,c^k}$). A search with loop-checking visits each state at most once and then stops with a plan or with "no solution". It can take very long (PSPACE-complete), but it always ends.
> (b) Function symbols give infinitely many ground terms, so infinitely many atoms and states. A search may go on forever without repeating a state, and no algorithm can decide every case.
> (c) The bound limits the depth of the search: only plans with at most $k$ actions must be examined. The search can stop after depth $k$ and answer "no". Without a bound there is no depth at which "no" is safe.

## 11. Forward search (slide 17)
![[cmpe421_lecture_07_deterministicplanning_p017.jpg]]

**Notes.** Search forward from $s_0$, applying executable actions, until a state satisfies $g$. Sound: a returned plan is a solution. Breadth-first and best-first are complete but need memory exponential in the solution length. Depth-first and greedy need linear memory but are not complete, unless loop-checking is added (the number of states is finite).

Backward search (our notes add this): regress the goal through a relevant action, $\gamma^{-1}(g,a)=(g-\text{effects}^+(a))\cup\text{precond}(a)$, until the goal is a subset of $s_0$.

> [!question]- Check (DeterministicPlanning, slide 17): Two blocks: $s_0=\{$`ontable(a)`, `ontable(b)`, `clear(a)`, `clear(b)`, `handempty`$\}$, $g=\{$`on(a,b)`$\}$. (a) Do a breadth-first forward search by hand until a goal state appears. (b) How many states can be reached from $s_0$ at all? (c) How can depth-first search fail here, and what repairs it?
> (a) Depth 0: $s_0$ does not contain `on(a,b)`.
> Executable in $s_0$: `pickup(a)` and `pickup(b)` (the `stack`, `unstack`, `putdown` actions need `holding` or `on` atoms).
> Depth 1: $s_1=\gamma(s_0,$`pickup(a)`$)=\{$`ontable(b)`, `clear(b)`, `holding(a)`$\}$ and $s_2=\gamma(s_0,$`pickup(b)`$)=\{$`ontable(a)`, `clear(a)`, `holding(b)`$\}$. No goal state.
> Depth 2, from $s_1$: `putdown(a)` gives $s_0$ again. `stack(a,b)` gives $\{$`ontable(b)`, `on(a,b)`, `clear(a)`, `handempty`$\}$, which contains `on(a,b)`: goal.
> Plan: (`pickup(a)`, `stack(a,b)`), 2 actions. Breadth-first search finds a shortest plan.
> (b) 5 states: hand empty with both on the table, a on b, or b on a (3 states), and hand holding a or holding b (2 states).
> (c) Depth-first search can follow `pickup(a)`, `putdown(a)`, `pickup(a)`, `putdown(a)`, ... forever. It never returns a plan, so it is not complete. Loop-checking (do not visit a state that is already on the current path) cuts this branch. With only 5 states the search must then end.

## 12. Graphplan and the planning graph (slides 18-32)
![[cmpe421_lecture_07_deterministicplanning_p018.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p019.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p020.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p021.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p022.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p023.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p024.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p025.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p026.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p027.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p028.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p029.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p030.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p031.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p032.jpg]]

**Notes.** Idea: solve a **relaxed problem** first (easy, polynomial time, its solutions include all real solutions), then search only among the actions that appear in it.

**Planning graph**: alternating layers. State-level $i$ holds the literals that might be true at time $i$. Action-level $i$ holds the actions that might be executable at time $i$. Edges are preconditions and effects. A **maintenance action** per literal carries it forward unchanged.

**Mutex** (mutually exclusive), slide 23. Two actions at one level are mutex if:
- inconsistent effects: an effect of one negates an effect of the other,
- interference: one deletes a precondition of the other,
- competing needs: their preconditions are mutex.

Two literals are mutex if one negates the other, or all ways of achieving them are pairwise mutex (inconsistent support).

**Graphplan**: for $k=0,1,2,\dots$ expand the graph to $k$ levels. If all goals are present and pairwise non-mutex (necessary, not sufficient), run **solution extraction**: a backward search that picks, for each goal literal, an action at the previous level, backtracks on mutex pairs, and recurses on the preconditions.

> [!example] Dinner date (slides 21-32)
> $s_0=\{$garbage, cleanHands, quiet$\}$, $g=\{$dinner, present, $\neg$garbage$\}$.
> `cook`: needs cleanHands, gives dinner. `wrap`: needs quiet, gives present. `carry`: gives $\neg$garbage, $\neg$cleanHands. `dolly`: gives $\neg$garbage, $\neg$quiet.
> With one level the goals cannot be achieved without mutex actions (`carry` deletes the precondition of `cook`, `dolly` that of `wrap`). With two levels a plan exists, for example level 1 `cook`, `wrap`, level 2 `carry`.

> [!warning] Watch out
> - Slide 30 says "twelve combinations at level 4". The graph has only levels 0 to 2, and the combinations are at level 2.
> - Parallel length is not the number of actions: the plan above has 2 levels but 3 actions.

> [!info] How to read and build the planning-graph figures (beyond the slides)
> In the dinner figures (slides 22-32) each column is one level. The boxes with a name are the real actions. The small blue rectangles are the maintenance actions (also called no-ops): `no-op(p)` has the precondition $p$ and the effect $p$. Straight lines go from the preconditions to an action and from the action to its effects. The thin curved arcs join two nodes that are mutex.
>
> Building one more level by hand:
> 1. State-level 0: every atom of $s_0$, and the negation of every other atom.
> 2. Action-level $i$: every real action whose preconditions are all in state-level $i-1$ and are not mutex with each other there. Add one no-op for each literal of state-level $i-1$.
> 3. Mark the mutex pairs of actions (inconsistent effects, interference, competing needs).
> 4. State-level $i$: all effects of all actions of action-level $i$.
> 5. Mark the mutex pairs of literals: a literal and its negation, and every pair with inconsistent support.
>
> The graph is optimistic. A literal in state-level $i$ only "might" be true after $i$ steps: the graph puts all effects of all actions in one level, and the mutex arcs remove only some of the impossible combinations (pairs, never triples).

> [!info] A second tiny problem: shopping (beyond the slides)
> Some check questions below use this problem, so that the graph is not the one of the slides.
> $s_0=\{$money$\}$, $g=\{$food, ticket$\}$. Atoms: money, food, ticket.
> - `buyFood`: needs money, gives food, $\neg$money.
> - `buyTicket`: needs money, gives ticket, $\neg$money.
> - `work`: needs nothing, gives money.
>
> There is money for one purchase only. A real solution is `buyFood`, `work`, `buyTicket` (3 actions, no shorter plan exists).

> [!question]- Check (DeterministicPlanning, slide 18): (a) The slide says the search space for 450 blocks has more than $10^{1000}$ states. Check this with the bound "more than $n!$ states" of slide 2. (b) Which two properties must the relaxed problem have, and what is each one good for?
> (a) $\log_{10}(450!)=\sum_{i=1}^{450}\log_{10} i\approx1000.24$, so $450!\approx1.7\cdot10^{1000}$. This is more than $10^{1000}$.
> (b) First: it is easy to solve (polynomial time). If not, solving it first would not save time.
> Second: its solutions include all solutions of the original problem. Then an action that appears in no relaxed solution appears in no real solution. The real search can drop such actions and still finds every real plan.

> [!question]- Check (DeterministicPlanning, slides 19-20): (a) Write the maintenance action for the literal $\neg$garbage and say why the graph needs such actions. (b) Graphplan checks "a necessary (but insufficient) condition for plan existence". What do the two words mean for the algorithm? (c) Which part of Graphplan is the relaxed problem?
> (a) `no-op(`$\neg$`garbage)`: precondition $\neg$garbage, effect $\neg$garbage. A real action changes only a few literals. All other literals stay as they are. In the graph a literal can reach the next state-level only as the effect of an action, so "nothing happened to this literal" needs its own action.
> (b) Necessary: if the condition fails at level $k$ (a goal is missing, or two goals are mutex), no plan with $k$ levels exists. Graphplan can skip the expensive search and expand the graph. Insufficient: if the condition holds, a plan may still not exist. So the solution extraction must run, and it may fail.
> (c) The graph expansion and the check. The graph says which literals and actions are possible at each time, with only pairwise mutex tests. That is easier than the real problem (polynomial time).

> [!question]- Check (DeterministicPlanning, slides 21-22): Build state-level 0, action-level 1 and state-level 1 of the planning graph for the shopping problem (callout above). Do not mark mutexes yet.
> State-level 0: the atoms of $s_0$ and the negations of the others: money, $\neg$food, $\neg$ticket.
> Action-level 1: real actions whose preconditions are in state-level 0: `buyFood` (money), `buyTicket` (money), `work` (no precondition). No-ops: `no-op(money)`, `no-op(`$\neg$`food)`, `no-op(`$\neg$`ticket)`. That is 6 action nodes.
> State-level 1: all effects.
> - `buyFood`: food, $\neg$money
> - `buyTicket`: ticket, $\neg$money
> - `work`: money
> - no-ops: money, $\neg$food, $\neg$ticket
>
> State-level 1 $=\{$money, $\neg$money, food, $\neg$food, ticket, $\neg$ticket$\}$: all 6 literals.
> Like on slide 22, state-level 1 is not a state. It contains money and $\neg$money together. It lists what might be true after one step.

> [!question]- Check (DeterministicPlanning, slide 23): Shopping problem, level 1. Give the reason for each mutex, or say "not mutex": (a) `buyFood` and `buyTicket`, (b) `buyFood` and `work`, (c) `work` and `no-op(money)`, (d) the literals food and ticket, (e) the literals food and money.
> (a) Mutex by interference: `buyFood` deletes money, and money is a precondition of `buyTicket` (also the other way round). It is not inconsistent effects: both give $\neg$money, and no effect of one negates an effect of the other.
> (b) Mutex by inconsistent effects: `buyFood` gives $\neg$money, `work` gives money. It is not interference, because `work` has no precondition and deletes nothing.
> (c) Not mutex. Both give money, no effect is negated, no precondition is deleted, and the only precondition (money) is not mutex with anything in state-level 0.
> (d) Mutex by inconsistent support. Food is produced only by `buyFood`, ticket only by `buyTicket`, and this pair is mutex by (a).
> (e) Mutex by inconsistent support. Food comes only from `buyFood`. Money comes from `work` or `no-op(money)`. `buyFood` is mutex with `work` by (b), and with `no-op(money)` by inconsistent effects ($\neg$money against money). All pairs are mutex.

> [!question]- Check (DeterministicPlanning, slide 24): Dinner date, level 1. (a) The slide says "each of cook and wrap is mutex with a maintenance operation". Which ones, and why? (b) Is `carry` mutex with `dolly`? (c) Are the literals $\neg$cleanHands and dinner mutex in state-level 1? And $\neg$garbage and dinner?
> (a) `cook` gives dinner, `no-op(`$\neg$`dinner)` gives $\neg$dinner: inconsistent effects. In the same way `wrap` (present) is mutex with `no-op(`$\neg$`present)`.
> (b) Not mutex. Effects of `carry`: $\neg$garbage, $\neg$cleanHands. Effects of `dolly`: $\neg$garbage, $\neg$quiet. No effect negates another one, and both actions have no preconditions, so there is no interference and no competing needs.
> (c) $\neg$cleanHands comes only from `carry`, dinner only from `cook`. `carry` deletes cleanHands, the precondition of `cook`: the two actions are mutex (interference). So $\neg$cleanHands and dinner are mutex (inconsistent support).
> $\neg$garbage comes from `carry` or `dolly`, dinner from `cook`. The pair `dolly`, `cook` is not mutex. One non-mutex pair is enough: $\neg$garbage and dinner are not mutex.

> [!question]- Check (DeterministicPlanning, slide 25): Shopping problem. The goals food and ticket are both in state-level 1. Does Graphplan start solution extraction at level 1? At level 2? At which level does it start?
> Level 1: no. Food and ticket are mutex (inconsistent support, question on slide 23). The necessary condition fails, so Graphplan expands the graph.
> Level 2: food can come from `buyFood` or `no-op(food)`, ticket from `buyTicket` or `no-op(ticket)`. All four pairs are mutex:
> - `buyFood`, `buyTicket`: interference.
> - `buyFood`, `no-op(ticket)`: competing needs. The preconditions money and ticket are mutex in state-level 1.
> - `no-op(food)`, `buyTicket`: competing needs. Food and money are mutex in state-level 1.
> - `no-op(food)`, `no-op(ticket)`: competing needs. Food and ticket are mutex in state-level 1.
>
> So food and ticket are still mutex in state-level 2: no extraction.
> Level 3: in state-level 2 money and ticket are no longer mutex (`work` together with `no-op(ticket)`). So `buyFood` and `no-op(ticket)` are not mutex at action-level 3, and food and ticket are not mutex in state-level 3. Graphplan starts solution extraction at level 3. This matches the real plan length 3.

> [!question]- Check (DeterministicPlanning, slide 26): Dinner date with two levels. Solution-extraction is called with $g=\{\neg$garbage, dinner, present$\}$ and $j=2$. It chooses `carry` for $\neg$garbage, `no-op(dinner)` for dinner and `wrap` for present. (a) What is $g'$? (b) What is the next call? (c) Why does the procedure stop at $j=0$?
> (a) $g'$ is the set of the preconditions of the chosen actions. `carry`: none. `no-op(dinner)`: dinner. `wrap`: quiet. So $g'=\{$dinner, quiet$\}$.
> (b) Solution-extraction($\{$dinner, quiet$\}$, 1): now these two literals must be achieved by non-mutex actions of action-level 1.
> (c) State-level 0 is the initial state. The literals that arrive there are preconditions of actions from action-level 1, and an action is in action-level 1 only if its preconditions are true in $s_0$. So nothing is left to achieve, and the chosen actions of all levels form the plan.

> [!question]- Check (DeterministicPlanning, slide 27): Dinner date. In state-level 1 the three goals are present and no two of them are mutex (slide 25). Still no plan with one level exists. How is this possible?
> A mutex is a test on pairs. Each pair of goals can be achieved together in one level:
> - $\neg$garbage and dinner: `dolly` and `cook` (not mutex),
> - $\neg$garbage and present: `carry` and `wrap` (not mutex),
> - dinner and present: `cook` and `wrap` (not mutex).
>
> But all three together need `cook`, `wrap` and one of `carry`, `dolly`. `carry` is mutex with `cook` and `dolly` is mutex with `wrap` (interference). No choice works for the triple.
> This is why the condition is necessary but insufficient, and why solution extraction must check the chosen action sets.

> [!question]- Check (DeterministicPlanning, slide 29): Dinner date, after the second expansion. (a) How many action nodes do action-level 1 and action-level 2 have? (b) Does state-level 2 have new literals? (c) $\neg$quiet and present are mutex in state-level 1. Are they still mutex in state-level 2?
> (a) Action-level 1: 4 real actions and one no-op for each of the 5 literals of state-level 0: 9 nodes. Action-level 2: 4 real actions and one no-op for each of the 10 literals of state-level 1: 14 nodes.
> (b) No. State-level 1 already has all 10 literals (5 atoms, each positive and negative). The new level changes the mutexes, not the literals.
> (c) No. In level 2 present has a second producer: `no-op(present)`. `dolly` (gives $\neg$quiet) and `no-op(present)` are not mutex: no effect is negated, `dolly` does not delete present, and `dolly` has no precondition. One non-mutex pair of producers is enough.
> In general, from one level to the next the literals and actions can only grow and the mutexes can only shrink.

> [!question]- Check (DeterministicPlanning, slides 30-31): Dinner date, solution extraction at state-level 2. (a) Where does the number twelve come from? (b) How many of the twelve combinations contain a mutex pair at action-level 2, and which?
> (a) $\neg$garbage: `carry`, `dolly`, `no-op(`$\neg$`garbage)`: 3 ways. Dinner: `cook`, `no-op(dinner)`: 2 ways. Present: `wrap`, `no-op(present)`: 2 ways. $3\cdot2\cdot2=12$.
> (b) The mutex pairs among these actions are `carry` with `cook` and `dolly` with `wrap` (interference, as in level 1).
> Combinations with `carry` and `cook`: 2 (present by `wrap` or by the no-op).
> Combinations with `dolly` and `wrap`: 2 (dinner by `cook` or by the no-op).
> $2+2=4$ combinations fail, and $12-4=8$ have no mutex pair. This is the "several of the combinations look OK" of slide 31. For these 8 the search must still continue at level 1.

> [!question]- Check (DeterministicPlanning, slide 32): Finish the extraction of the slide: the subgoals at state-level 1 are dinner and quiet. (a) Which actions of action-level 1 are chosen, and why does the search stop with success? (b) Write the plan by levels and as a sequence. (c) Is the sequence `carry`, `cook`, `wrap` also a correct plan?
> (a) Dinner: `cook` is the only producer in level 1. Quiet: `no-op(quiet)`. The two are not mutex. Their preconditions are cleanHands and quiet, and both are in $s_0$ (level 0), so the extraction succeeds.
> (b) Level 1: `cook`. Level 2: `carry`, `wrap`. Parallel length 2, with 3 real actions (no-ops are not counted).
> Actions of one level are pairwise non-mutex, so they can be done in any order: `cook`, `carry`, `wrap` or `cook`, `wrap`, `carry`.
> Test of the first one: $\{$garbage, cleanHands, quiet$\}$, after `cook` add dinner, after `carry` remove garbage and cleanHands, after `wrap` add present. Result $\{$quiet, dinner, present$\}$: no garbage, dinner, present. The goal holds.
> (c) No. `carry` removes cleanHands, and then `cook` is not executable. The levels of the plan matter: `cook` must be in an earlier level than `carry`.

## 13. Heuristics and FastForward (slides 33-38)
![[cmpe421_lecture_07_deterministicplanning_p033.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p034.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p035.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p036.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p037.jpg]]
![[cmpe421_lecture_07_deterministicplanning_p038.jpg]]

**Notes.** Forward search needs a good heuristic. Graph expansion is polynomial, solution extraction is exponential, so use only the expansion: $\Delta^g(s,g)$ = level of the first layer of a planning graph from $s$ that "possibly achieves" the goal.

**FastForward (FF)**: no A*-style search (too much memory). Greedy: expand the current state and move to the child with the smallest $h$. In a local minimum FF does a breadth-first search until it finds a node with lower $h$. In a dead end FF fails. No guarantee, but it works well on many classical problems.

> [!info] Heuristics from the relaxed problem (beyond the slides)
> Slide 35 says "some ways to improve this, but I'll skip the details". These are the usual ones (Russell and Norvig, Chapter 10).
> - **Level cost** of a literal: the first state-level in which it appears.
> - **Max-level**: the largest level cost of the goals. Admissible (never too large), but often far too small.
> - **Level-sum**: the sum of the level costs of the goals. It assumes the goals are independent. It can be too large (not admissible) when one action achieves two goals, but it works well in practice.
> - **Set-level**: the first level where all goals are present and no two are mutex. Admissible. This is how our notes read "possibly achieves the goal", so $\Delta^g$ is the set-level.
> - **Ignore delete lists**: remove all negative effects from all actions. Then nothing that became true can become false again, and a plan for this relaxed problem is easy to find (polynomial time). Every real plan is also a relaxed plan.
> - **FastForward's $h(s)$**: build the planning graph of the delete-free problem from $s$ (no mutexes are needed), extract one relaxed plan backward from the goals, and count its actions. This number is $h(s)$.
> - Real FF moves only to a state with strictly smaller $h$. If no child is better, it does the breadth-first search of slide 37.

> [!question]- Check (DeterministicPlanning, slide 33): Take the picture of the slide: 500 blocks on the table, hand empty, goal `on(a1,a2)`, `on(a2,a3)`. (a) How many actions are executable in the initial state? (b) How long is the shortest plan? (c) How many of the first actions from (a) start a shortest plan?
> (a) Every block is on the table and clear, and the hand is empty: 500 `pickup` actions. No `stack`, `unstack` or `putdown` is executable. So 500 actions.
> (b) 4 actions: `pickup(a2)`, `stack(a2,a3)`, `pickup(a1)`, `stack(a1,a2)`. Each `stack` needs a `pickup` before it, and two `stack` actions are needed.
> (c) Only 1: `pickup(a2)`. The lower `stack` must come first, because `stack(a2,a3)` needs `holding(a2)` and a2 can no longer be picked up from the table when a1 is on it.
> A blind forward search treats all 500 choices in the same way, and 497 of them touch blocks that the goal does not mention. A heuristic must tell the search which child is closer to the goal.

> [!question]- Check (DeterministicPlanning, slide 34): (a) Which of the two parts of Graphplan is cheap and which is expensive? (b) A heuristic $h(s)$ is computed for every state that forward search generates. Which part can it use, and why not the other one?
> (a) Graph expansion takes polynomial time. Solution extraction is a backward search with backtracking and takes exponential time in the worst case.
> (b) Only the graph expansion. A heuristic is called thousands of times, so it must be fast. Solution extraction from $s$ would solve the whole planning problem from $s$. Then the forward search would not be needed, and the "heuristic" would cost as much as planning.

> [!question]- Check (DeterministicPlanning, slide 35): Compute $\Delta^g(s_0,g)$ for (a) the shopping problem (section 12) and (b) the dinner date. Compare each with the real number of actions. Why is $\Delta^g$ a lower bound?
> (a) Food and ticket are in state-level 1 and 2 but are mutex there. In state-level 3 they are not mutex (question on slide 25). $\Delta^g=3$. The shortest real plan (`buyFood`, `work`, `buyTicket`) has 3 actions: the estimate is exact here.
> (b) All three goals are in state-level 1 and no two are mutex (slide 25). $\Delta^g=1$. The real plan needs 3 actions in 2 levels. $1\le3$: a lower bound, but not a tight one.
> Reason: if a plan with $n$ actions exists, the goals are present and non-mutex at level $n$ or earlier, because the graph is optimistic. So the first such level is at most $n$. One level can hold several actions, so the level number can be smaller than the number of actions.

> [!question]- Check (DeterministicPlanning, slide 35, level heuristics): Use the callout above. (a) Compute the level costs, max-level, level-sum and set-level for the shopping problem. The real cost is 3. (b) Dinner date with the goal $\{\neg$garbage, $\neg$cleanHands$\}$: compute level-sum and the real cost. What does this show?
> (a) Food first appears in state-level 1 (`buyFood`), ticket in state-level 1 (`buyTicket`): both level costs are 1.
> Max-level $=\max(1,1)=1$. Level-sum $=1+1=2$. Set-level $=3$ (first level without the mutex between food and ticket).
> All three are at most 3. Set-level is the best one here, because it is the only one that uses the mutexes. Max-level and level-sum do not see that the money is enough for one purchase only.
> (b) Both literals appear in state-level 1 (`carry`). Level-sum $=1+1=2$. The real cost is 1: the single action `carry` gives both.
> $2>1$: level-sum can overestimate, so it is not admissible. It counts `carry` twice.

> [!question]- Check (DeterministicPlanning, slide 36): Shopping problem. Use FastForward's $h$ (number of actions of a relaxed plan without delete lists, callout above). (a) Compute $h$ for $s_0=\{$money$\}$ and for its three children. (b) Run the greedy procedure of the slide, taking `buyFood` first. (c) What does FF keep in memory, compared with A*?
> (a) Without delete lists `buyFood` and `buyTicket` no longer remove money.
> $s_0=\{$money$\}$: relaxed plan `buyFood`, `buyTicket`. $h=2$. The real cost is 3: the relaxation does not see that the money is used up.
> Child by `buyFood`: $\{$food$\}$. Relaxed plan `work`, `buyTicket`. $h=2$.
> Child by `buyTicket`: $\{$ticket$\}$. Relaxed plan `work`, `buyFood`. $h=2$.
> Child by `work`: $\{$money$\}$, the same state as $s_0$. $h=2$.
> (b) All children have $h=2$, so the choice is a tie. With `buyFood`: $s=\{$food$\}$. Its only executable action is `work`, which gives $\{$food, money$\}$ with relaxed plan `buyTicket`, $h=1$. From there `buyTicket` gives $\{$food, ticket$\}$, $h=0$: the goal.
> Plan: `buyFood`, `work`, `buyTicket`.
> If the tie in $s_0$ is broken by `work`, the search stays in the same state and can loop. The slide's procedure has no protection against this.
> (c) Only the current state and its children. A* keeps every generated state in its frontier, which grows exponentially. This is the "takes too much memory" of the slide.

> [!question]- Check (DeterministicPlanning, slide 37): (a) By the definition of the slide, is $s_0$ of the shopping problem a local minimum (values from the question on slide 36)? What does real FF do there? (b) Remove the action `work` from the problem. What happens after `buyFood`, and what does FF do?
> (a) The slide asks for $h(s')>h(s)$ for every successor. Here every successor has $h=2=h(s_0)$: not larger, so it is not a local minimum in this strict sense. It is a plateau: no child is better.
> Real FF wants a strictly smaller $h$, so it searches breadth-first from $s_0$. Depth 1: $h=2,2,2$. Depth 2: $\{$food, money$\}$ (after `buyFood`, `work`) has $h=1<2$. FF jumps to this state and continues greedily.
> (b) After `buyFood` the state is $\{$food$\}$. `buyFood` and `buyTicket` need money, and nothing produces money: no action is executable and the goal is not reached. This is a dead end, and FF fails.
> In this small problem no solution exists at all (two purchases, money for one), so failing is correct. In general FF can also fail when a solution exists: the greedy step went into a dead end, and FF did not keep the other branches. This is why the slide says "no guarantee".
> Note that $h(s_0)$ is still 2 without `work`. The relaxed problem is solvable, the real one is not.

> [!question]- Check (DeterministicPlanning, slide 38): The slide says about FastForward: "large variation in how good or bad its plans were, but it found them very quickly". Which design decisions of slides 34-37 explain each half of this sentence?
> Quickly: the heuristic uses only the polynomial graph expansion, and the search is greedy. It follows one path and does not keep a large frontier like A*.
> Variation in quality: the greedy search takes the child that looks best and never compares complete plans. Nothing makes the plan short. If the heuristic leads it well, the plan is good. If it leads through plateaus and detours, the plan is long. There is no guarantee on the length (slide 37).

---
## Part C. Extra deck: inference in predicate logic

> [!info] Not in our lecture notes
> The deck `lectures/Lecture 8/Inference-PredicateLogic.pdf` (48 slides, CMSC 421 Chapter 9) has no easybook notes yet. The notes below are taken from the slide text only. Our Lecture 9 notes cover unification and Generalized Modus Ponens briefly as additions.

## 14. Instantiation and propositionalization (inference deck, slides 1-8)
![[cmpe421_lecture_8_inference_predicatelogic_p001.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p002.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p003.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p004.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p005.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p006.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p007.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p008.jpg]]

**Notes.** **Universal instantiation (UI)**: from $\forall v\ \alpha$ infer $\alpha\{v=g\}$ for any ground term $g$. **Existential instantiation (EI)**: from $\exists v\ \alpha$ infer $\alpha\{v=k\}$ for a new constant $k$ (a Skolem constant) that appears nowhere else. UI can be used many times, EI once.

Instantiating every universal sentence in all possible ways turns a FOL KB into a propositional one. Problem: with function symbols there are infinitely many ground terms ($\mathit{father}(\mathit{father}(\mathit{John}))$, ...). Herbrand's theorem: an entailed sentence is entailed by a finite subset of the propositionalized KB.

> [!info] Words used on slides 3-8 (beyond the slides)
> - A **ground term** has no variables: $\mathit{John}$, $\mathit{father}(\mathit{John})$. A **ground sentence** has no variables either.
> - $\alpha\theta$ means: apply the substitution $\theta$ to $\alpha$. With $\theta=\{v=g\}$ every free $v$ in $\alpha$ becomes $g$.
> - The rule is written as a fraction: the sentence above the line is given, the sentence below the line may be inferred.
> - The **Skolem constant** of EI must be new. The sentence $\exists x\ \alpha$ only says that some object exists. It does not say which one. A new name carries no other knowledge, so nothing false is added. An old name (for example $\mathit{John}$) would claim that this known object is the one.
> - **Depth** of a term (slide 8): a constant has depth 0, $\mathit{father}(\mathit{John})$ depth 1, $\mathit{father}(\mathit{father}(\mathit{John}))$ depth 2.
> - **Semidecidable**: there is an algorithm that answers "yes" in finite time for every entailed sentence. For a sentence that is not entailed no algorithm can always answer "no". It may run forever.

> [!question]- Check (inference deck, slides 1-2): Slide 1 lists two results of Gödel, 1930 and 1931. What is the difference? Which two lines of the table name methods that this deck uses later? On slide 2, where is the premise $A$ and where is the conclusion $B$ in Frege's drawing of $\forall x\,(A(x)\to B(x))$?
> 1930: a complete proof procedure for FOL exists ($\exists$). Every entailed sentence can be proved. 1931: no complete procedure exists for arithmetic ($\neg\exists$). Some true sentences about numbers cannot be proved.
> Used later: Herbrand 1930 (reduce FOL to propositional logic, slides 6-8) and Robinson 1965 (resolution, slides 45-48).
> Frege: the conclusion $B$ is on the upper line, the premise $A$ hangs below it. The small hollow with the letter in it is the universal quantifier.

> [!question]- Check (inference deck, slide 3): The KB has the constants $\mathit{Ann}$ and $\mathit{Bob}$, the function $\mathit{mother}$, and the sentence $\forall x\ \mathit{Student}(x)\Rightarrow\mathit{Reads}(x,\mathit{mother}(x))$. Write three sentences that UI gives. Is $\mathit{Student}(y)\Rightarrow\mathit{Reads}(y,\mathit{mother}(y))$ one of them?
> Replace every $x$ by one ground term:
> $\{x=\mathit{Ann}\}$: $\mathit{Student}(\mathit{Ann})\Rightarrow\mathit{Reads}(\mathit{Ann},\mathit{mother}(\mathit{Ann}))$.
> $\{x=\mathit{Bob}\}$: $\mathit{Student}(\mathit{Bob})\Rightarrow\mathit{Reads}(\mathit{Bob},\mathit{mother}(\mathit{Bob}))$.
> $\{x=\mathit{mother}(\mathit{Ann})\}$: $\mathit{Student}(\mathit{mother}(\mathit{Ann}))\Rightarrow\mathit{Reads}(\mathit{mother}(\mathit{Ann}),\mathit{mother}(\mathit{mother}(\mathit{Ann})))$.
> The sentence with $y$ is not a UI instance: the rule needs a ground term $g$, and $y$ is a variable.

> [!question]- Check (inference deck, slide 4): The KB contains $\mathit{Cat}(\mathit{Tom})$, $\mathit{Hungry}(\mathit{Tom})$ and $\exists x\ \mathit{Cat}(x)\land\mathit{Owns}(\mathit{Mary},x)$. A student applies EI with $\{x=\mathit{Tom}\}$. What is wrong, and what is the correct result?
> $\mathit{Tom}$ already appears in the KB. The result $\mathit{Owns}(\mathit{Mary},\mathit{Tom})$ does not follow: Mary owns some cat, maybe a different one. With it we could even "prove" that Mary owns a hungry cat.
> Correct: take a new constant, for example $C_1$: $\mathit{Cat}(C_1)\land\mathit{Owns}(\mathit{Mary},C_1)$. $C_1$ is a Skolem constant. Nothing else is known about it.

> [!question]- Check (inference deck, slide 5): (a) Why is the KB after EI not logically equivalent to the old KB, and why is that no problem? (b) In the limit proof on the slide, which of the steps "let $\epsilon$ be any number $>0$" and "let $\delta_1>0$ be such that ..." is UI and which is EI?
> (a) The new KB says $\mathit{Crown}(C_1)$. The old KB has models where the object named $C_1$ is not a crown, so the old KB does not entail it. But the new KB is satisfiable exactly when the old one is. Proofs by refutation only ask whether a set of sentences is satisfiable, so this is enough.
> (b) "Any $\epsilon$" removes $\forall\epsilon$: universal instantiation. It can be repeated with other values. "Let $\delta_1$ be such that" gives a name to the object that $\exists\delta$ promises: existential instantiation. $\delta_1$ is a Skolem constant. It is done once.

> [!question]- Check (inference deck, slide 6): Propositionalize this KB: $\forall x\ \mathit{Cat}(x)\Rightarrow\mathit{Mammal}(x)$, $\mathit{Cat}(\mathit{Tom})$, $\mathit{Dog}(\mathit{Rex})$. How many instances of the rule are there, and what are the proposition symbols? Does the new KB entail $\mathit{Mammal}(\mathit{Tom})$? Does it entail $\mathit{Mammal}(\mathit{Rex})$?
> There are 2 constants and no functions, so 2 instances: $\mathit{Cat}(\mathit{Tom})\Rightarrow\mathit{Mammal}(\mathit{Tom})$ and $\mathit{Cat}(\mathit{Rex})\Rightarrow\mathit{Mammal}(\mathit{Rex})$. The facts stay.
> Proposition symbols: $\mathit{Cat}(\mathit{Tom})$, $\mathit{Mammal}(\mathit{Tom})$, $\mathit{Cat}(\mathit{Rex})$, $\mathit{Mammal}(\mathit{Rex})$, $\mathit{Dog}(\mathit{Rex})$. Each is one indivisible symbol now.
> $\mathit{Mammal}(\mathit{Tom})$: yes, by Modus Ponens on the first instance. $\mathit{Mammal}(\mathit{Rex})$: no, $\mathit{Cat}(\mathit{Rex})$ is not known. The original KB gives the same two answers (entailment of ground sentences is preserved).

> [!question]- Check (inference deck, slide 7): A KB without functions has $p=3$ predicates of arity $k=2$ and $n=10$ constants. How many ground atoms can be built? How many for $p=4$, $k=3$, $n=20$? Why is this a problem for a proof of one small fact?
> Each predicate has $n^k$ argument tuples, so there are $p\cdot n^k$ ground atoms.
> First case: $3\cdot10^2=300$. Second case: $4\cdot20^3=4\cdot8000=32\,000$.
> The proof of one fact needs only a few of them. In the slide example the proof of $\mathit{Evil}(\mathit{John})$ needs $\mathit{Greedy}(\mathit{John})$, but $\mathit{Greedy}(\mathit{Richard})$ and $\mathit{Greedy}(\mathit{Joanna})$ are created too. The propositional prover must search among all these irrelevant sentences.

> [!question]- Check (inference deck, slide 8): A KB has one constant $A$ and one binary function $g$. How many ground terms have depth $\le0$, $\le1$, $\le2$, $\le3$? What does the loop on the slide do if $\alpha$ is entailed and its proof needs terms of depth 2? What does it do if $\alpha$ is not entailed?
> Let $t_n$ be the number of terms of depth $\le n$. A term is $A$ or $g(s_1,s_2)$ with $s_1,s_2$ of depth $\le n-1$: $t_n=1+t_{n-1}^2$.
> $t_0=1$ ($A$). $t_1=1+1^2=2$. $t_2=1+2^2=5$. $t_3=1+5^2=26$. The number grows without end, so the full propositionalized KB is infinite.
> Entailed: the rounds $n=0$ and $n=1$ fail, the round $n=2$ finds the proof and the loop stops. Herbrand's theorem promises that such a finite round exists.
> Not entailed: every round fails and there is always a next round. The loop never stops. We never learn "no". This is what semidecidable means.

## 15. Unification and Generalized Modus Ponens (inference deck, slides 9-20)
![[cmpe421_lecture_8_inference_predicatelogic_p009.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p010.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p011.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p012.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p013.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p014.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p015.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p016.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p017.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p018.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p019.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p020.jpg]]

**Notes.** A **unifier** of $\alpha$ and $\beta$ is a substitution $\theta$ with $\alpha\theta=\beta\theta$.

| $p$ | $q$ | $\theta$ |
|---|---|---|
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(\mathit{John},\mathit{Jane})$ | $\{x=\mathit{Jane}\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(y,\mathit{Joanna})$ | $\{x=\mathit{Joanna},\ y=\mathit{John}\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(y,\mathit{mother}(y))$ | $\{y=\mathit{John},\ x=\mathit{mother}(\mathit{John})\}$ |
| $\mathit{Knows}(\mathit{John},x)$ | $\mathit{Knows}(x,\mathit{Joanna})$ | fail (rename one $x$ first: standardizing apart) |

A **most general unifier (mgu)** binds no more than needed.

**Generalized Modus Ponens**: from facts $p_1',\dots,p_n'$ and a rule $p_1\land\dots\land p_n\Rightarrow q$, with $p_i'\theta=p_i\theta$ for all $i$, infer $q\theta$. Example: $\mathit{King}(\mathit{John})$, $\mathit{Greedy}(y)$ and $\mathit{King}(x)\land\mathit{Greedy}(x)\Rightarrow\mathit{Evil}(x)$ give $\mathit{Evil}(\mathit{John})$ with $\theta=\{x=\mathit{John},\ y=\mathit{John}\}$.

> [!info] Computing an mgu by hand (beyond the slides)
> Slide 18 gives the algorithm in words. In practice:
> 1. If the two literals share a variable name, rename the variables of one literal first (**standardizing apart**). The two literals come from different sentences. Each sentence has its own $\forall$, so the same letter in both is an accident.
> 2. Go through the arguments from left to right. Keep one substitution $\theta$, at first $\{\}$.
> 3. Before comparing two arguments, apply the current $\theta$ to both.
> 4. Same after that: go on. One is a variable $x$ and the other an expression $e$ without $x$ in it: add $x=e$ and also replace $x$ by $e$ in the bindings found so far. Two different constants or two different function names: fail.
> 5. **Occur check**: $x$ against an expression that contains $x$, for example $x$ and $\mathit{mother}(x)$, fails. No finite term is equal to its own mother term.
> 
> **Substitution instance** (slide 17): $e'$ is a substitution instance of $e$ if $e'=e\sigma$ for some substitution $\sigma$. So an mgu is a unifier from which every other unifier can be reached by a further substitution. It makes no choice that is not forced.
> 
> **Composition** $\theta\theta'$: first apply $\theta$, then $\theta'$. To write it as one substitution, apply $\theta'$ to the right sides of $\theta$ and then add the bindings of $\theta'$ for variables that $\theta$ does not bind. Example: $\{x=f(y)\}\{y=A,\ z=B\}=\{x=f(A),\ y=A,\ z=B\}$.
> 
> **Definite clause** (slide 19): a disjunction with exactly one positive literal. $\neg p_1\lor\dots\lor\neg p_n\lor q$ is the same sentence as $p_1\land\dots\land p_n\Rightarrow q$. A single fact $q$ is the case $n=0$.

> [!warning] Slide error (slide 20)
> The second step of the proof says "Since $p_i\theta=p_i\theta$". It must be $p_i'\theta=p_i\theta$: the fact $p_i'$ and the premise $p_i$ become the same after the substitution. This is the condition of the theorem.

> [!question]- Check (inference deck, slide 9): KB: $\forall x\ \mathit{Bird}(x)\land\mathit{Small}(x)\Rightarrow\mathit{Flies}(x)$, $\mathit{Bird}(\mathit{Tweety})$, $\forall y\ \mathit{Small}(y)$, $\mathit{Friend}(\mathit{Rex},\mathit{Bob})$. Which substitution lets us infer $\mathit{Flies}(\mathit{Tweety})$ in one step? How many $\mathit{Small}$ sentences would propositionalization create first?
> We need $\mathit{Bird}(x)$ to match $\mathit{Bird}(\mathit{Tweety})$ and $\mathit{Small}(x)$ to match $\mathit{Small}(y)$: $\theta=\{x=\mathit{Tweety},\ y=\mathit{Tweety}\}$. Then both premises are known and the conclusion is $\mathit{Flies}(x)\theta=\mathit{Flies}(\mathit{Tweety})$.
> Propositionalization instantiates $\forall y\ \mathit{Small}(y)$ with all 3 constants: $\mathit{Small}(\mathit{Tweety})$, $\mathit{Small}(\mathit{Rex})$, $\mathit{Small}(\mathit{Bob})$. Two of them are useless. Unification creates only the binding that the proof needs.

> [!question]- Check (inference deck, slides 10-12): Find a unifier or say why there is none: (a) $\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(y,\mathit{Pizza})$, (b) $\mathit{Likes}(x,\mathit{Tea})$ and $\mathit{Likes}(\mathit{Bob},\mathit{Tea})$, (c) $\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(\mathit{Bob},y)$.
> (a) First arguments: $\mathit{Ann}$ and $y$ give $y=\mathit{Ann}$. Second arguments: $x$ and $\mathit{Pizza}$ give $x=\mathit{Pizza}$. $\theta=\{y=\mathit{Ann},\ x=\mathit{Pizza}\}$. Both literals become $\mathit{Likes}(\mathit{Ann},\mathit{Pizza})$.
> (b) $x=\mathit{Bob}$, then $\mathit{Tea}$ and $\mathit{Tea}$ are the same. $\theta=\{x=\mathit{Bob}\}$.
> (c) No unifier. The first arguments are the two different constants $\mathit{Ann}$ and $\mathit{Bob}$. A substitution changes only variables, so it cannot make them equal.

> [!question]- Check (inference deck, slide 13): Unify (a) $\mathit{Teaches}(\mathit{Ali},x)$ with $\mathit{Teaches}(y,\mathit{course}(y))$, (b) $\mathit{Teaches}(x,\mathit{AI})$ with $\mathit{Teaches}(y,\mathit{course}(y))$, where $\mathit{AI}$ is a constant.
> (a) First arguments: $y=\mathit{Ali}$. Apply it to the second arguments: $x$ against $\mathit{course}(\mathit{Ali})$, so $x=\mathit{course}(\mathit{Ali})$. $\theta=\{y=\mathit{Ali},\ x=\mathit{course}(\mathit{Ali})\}$. Writing $x=\mathit{course}(y)$ is not finished: the earlier binding of $y$ must be applied, as on the slide ($x=\mathit{mother}(\mathit{John})$).
> (b) First arguments: $x=y$. Second arguments: the constant $\mathit{AI}$ against the function term $\mathit{course}(y)$. A constant and a function term are different and neither is a variable: fail.

> [!question]- Check (inference deck, slides 14-15): Why does $\mathit{Likes}(\mathit{Ann},x)$ not unify with $\mathit{Likes}(x,\mathit{Pizza})$? Repair it by standardizing apart and give the unifier. Why is the renaming allowed?
> First arguments: $x=\mathit{Ann}$. Apply it to the second arguments: $\mathit{Ann}$ against $\mathit{Pizza}$, two different constants: fail. The one variable $x$ would have to be $\mathit{Ann}$ and $\mathit{Pizza}$ at the same time.
> Rename the $x$ of the second literal to $x_2$: $\mathit{Likes}(x_2,\mathit{Pizza})$. Now $x_2=\mathit{Ann}$ and $x=\mathit{Pizza}$: $\theta=\{x_2=\mathit{Ann},\ x=\mathit{Pizza}\}$.
> Allowed because the two literals come from two different sentences, each with its own universal quantifier. $\forall x\ \mathit{Likes}(x,\mathit{Pizza})$ and $\forall x_2\ \mathit{Likes}(x_2,\mathit{Pizza})$ mean the same.

> [!question]- Check (inference deck, slide 16): Which pairs unify? (a) $P(x,x)$ and $P(y,f(y))$, (b) $P(x,g(x))$ and $P(g(y),y)$, (c) $P(x,f(y))$ and $P(z,f(z))$.
> (a) $x=y$. Then the second arguments are $y$ and $f(y)$. The variable $y$ occurs inside $f(y)$: the occur check fails. No unifier.
> (b) $x=g(y)$. Then the second arguments are $g(g(y))$ and $y$. Again $y$ occurs inside the other term: fail.
> (c) $x=z$. Second arguments: $f(y)$ and $f(z)$, same function, so $y=z$. $\theta=\{x=z,\ y=z\}$. Both become $P(z,f(z))$. Here no variable has to equal a term that contains it, so there is no occur problem.

> [!question]- Check (inference deck, slide 17): $\alpha=P(x,f(y))$ and $\beta=P(g(z),w)$. Give an mgu. Is $\theta_3=\{x=g(A),\ z=A,\ w=f(y)\}$ a unifier? Is it an mgu? Write it as the mgu composed with one more substitution.
> mgu: first arguments $x=g(z)$, second arguments $w=f(y)$. $\theta=\{x=g(z),\ w=f(y)\}$. Both become $P(g(z),f(y))$.
> $\theta_3$: $\alpha\theta_3=P(g(A),f(y))$ and $\beta\theta_3=P(g(A),f(y))$. Equal, so it is a unifier.
> It is not an mgu. It binds $z$ to $A$, and nothing forces that. The mgu result $P(g(z),f(y))$ cannot be reached from $P(g(A),f(y))$ by a substitution, because a constant cannot be turned back into a variable.
> $\theta_3=\theta\{z=A\}$: apply $\{z=A\}$ to the right sides of $\theta$: $x=g(A)$, $w=f(y)$, then add $z=A$.

> [!question]- Check (inference deck, slide 18): Run the algorithm on $Q(x,g(x),y)$ and $Q(A,z,f(z))$. Show $\theta$ after each argument. What changes if the first literal is $Q(x,g(x),x)$?
> Start: $\theta=\{\}$.
> Argument 1: $x$ and $A$. $x$ is a variable: $\theta=\{x=A\}$.
> Argument 2: apply $\theta$: $g(A)$ and $z$. $z$ is a variable and does not occur in $g(A)$: $\theta=\{x=A,\ z=g(A)\}$.
> Argument 3: apply $\theta$: $y$ and $f(g(A))$. $\theta=\{x=A,\ z=g(A),\ y=f(g(A))\}$.
> Check: both literals become $Q(A,g(A),f(g(A)))$.
> With $Q(x,g(x),x)$: argument 3 is $x$, which is $A$ after substituting, against $f(g(A))$. A constant against a function term: FAIL.

> [!question]- Check (inference deck, slide 19): Facts: $\mathit{Child}(\mathit{Ece})$, $\mathit{Likes}(w,\mathit{IceCream})$ (everybody likes ice cream), $\mathit{Sweet}(\mathit{IceCream})$. Rule: $\mathit{Child}(x)\land\mathit{Likes}(x,y)\land\mathit{Sweet}(y)\Rightarrow\mathit{Wants}(x,y)$. Apply GMP: give $\theta$ and the conclusion. Write the rule as a definite clause.
> Match the facts $p_i'$ with the premises $p_i$:
> $\mathit{Child}(\mathit{Ece})$ and $\mathit{Child}(x)$: $x=\mathit{Ece}$.
> $\mathit{Likes}(w,\mathit{IceCream})$ and $\mathit{Likes}(\mathit{Ece},y)$ (after $x=\mathit{Ece}$): $w=\mathit{Ece}$, $y=\mathit{IceCream}$.
> $\mathit{Sweet}(\mathit{IceCream})$ and $\mathit{Sweet}(\mathit{IceCream})$: the same.
> $\theta=\{x=\mathit{Ece},\ w=\mathit{Ece},\ y=\mathit{IceCream}\}$. Conclusion: $q\theta=\mathit{Wants}(\mathit{Ece},\mathit{IceCream})$.
> Definite clause: $\neg\mathit{Child}(x)\lor\neg\mathit{Likes}(x,y)\lor\neg\mathit{Sweet}(y)\lor\mathit{Wants}(x,y)$. Exactly one literal is positive.

> [!question]- Check (inference deck, slide 20): The soundness proof uses universal instantiation in two places and then one propositional rule. Name the three steps for the example of slide 19 ($\mathit{King}(\mathit{John})$, $\mathit{Greedy}(y)$, $\mathit{King}(x)\land\mathit{Greedy}(x)\Rightarrow\mathit{Evil}(x)$). What does "sound" mean here?
> $\theta=\{x=\mathit{John},\ y=\mathit{John}\}$.
> Step 1, UI on the facts: $\mathit{Greedy}(y)$ gives $\mathit{Greedy}(\mathit{John})$. $\mathit{King}(\mathit{John})$ has no variable and stays. These are the $p_i'\theta$, and they equal the $p_i\theta$.
> Step 2, UI on the rule: $\mathit{King}(\mathit{John})\land\mathit{Greedy}(\mathit{John})\Rightarrow\mathit{Evil}(\mathit{John})$.
> Step 3, ordinary Modus Ponens on the results of steps 1 and 2: $\mathit{Evil}(\mathit{John})$.
> Sound: every sentence that GMP derives is entailed by the premises. GMP only does several correct steps at once, so it can never derive something false from true premises.

## 16. Forward chaining in FOL (inference deck, slides 21-31)
![[cmpe421_lecture_8_inference_predicatelogic_p021.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p022.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p023.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p024.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p025.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p026.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p027.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p028.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p029.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p030.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p031.jpg]]

**Notes.** Colonel West KB: $\mathit{American}(x)\land\mathit{Weapon}(y)\land\mathit{Sells}(x,y,z)\land\mathit{Hostile}(z)\Rightarrow\mathit{Criminal}(x)$, $\mathit{Missile}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Sells}(\mathit{West},x,\mathit{Nono})$, $\mathit{Missile}(x)\Rightarrow\mathit{Weapon}(x)$, $\mathit{Enemy}(x,\mathit{America})\Rightarrow\mathit{Hostile}(x)$, plus the facts $\mathit{American}(\mathit{West})$, $\mathit{Missile}(M_1)$, $\mathit{Owns}(\mathit{Nono},M_1)$, $\mathit{Enemy}(\mathit{Nono},\mathit{America})$.

Forward chaining fires every rule whose premises unify with known facts and adds the conclusions, round after round: first $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$, $\mathit{Hostile}(\mathit{Nono})$, then $\mathit{Criminal}(\mathit{West})$.

It is sound and complete for first-order Horn clauses. If the query is not entailed it may not terminate (entailment is semidecidable). It always terminates for Datalog (Horn clauses without functions).

> [!info] Reading the algorithm and the proof figure (beyond the slides)
> **FOL-FC-Ask (slide 25).** One pass of the `repeat` loop is one **iteration**. In an iteration every rule is tried against the facts that were in the KB at the start of the iteration. The conclusions go into the set `new`. They are added to the KB only at the end of the iteration, so they can be used from the next iteration on. The loop stops when the query is derived, or when an iteration adds nothing (`new` is empty). The KB is then a **fixed point**: applying the rules again changes nothing.
> A **renaming** of a sentence is the same sentence with other variable names: $\mathit{Likes}(x,\mathit{IceCream})$ and $\mathit{Likes}(y,\mathit{IceCream})$. It carries no new information and must not count as a new fact.
> 
> **The figure (slides 26-28).** The bottom row holds the given facts. Every higher box is a derived fact. The lines go down to the premises that were used. An arc across the lines means AND: all these premises are needed together for one rule. Row 2 is made in iteration 1, row 3 in iteration 2. Read from the top, the finished figure is the **proof tree** of $\mathit{Criminal}(\mathit{West})$.
> 
> **Why matching is expensive (slides 30-31).** A rule with several premises that share variables is a small constraint satisfaction problem: find values for the variables so that every premise is a known fact. A **rete network** stores which facts already match which premises, so this work is not repeated in every iteration.

> [!question]- Check (inference deck, slides 21-23): Slide 23 asks what the problem is with the axiom $\forall x\ \mathit{LawSays}(x)\Rightarrow x$. Answer it. Which two rules does the story not say, although the proof needs them?
> In FOL a variable stands for an object, and the argument of a predicate must be a term. Here $x$ would stand for a whole sentence ("selling weapons is a crime"): it is used as an argument of $\mathit{LawSays}$ and also alone as a sentence. A sentence is not a term, so this is not a FOL sentence. Quantifying over sentences needs a higher-order logic.
> So each law is written directly as its own FOL rule.
> Missing background knowledge: "missiles are weapons" and "an enemy of America is a hostile nation". People add these without noticing. The KB must contain them, or the words *weapon* and *hostile* in the law never connect to *missile* and *enemy* in the facts.

> [!question]- Check (inference deck, slide 24): (a) How do the two facts $\mathit{Owns}(\mathit{Nono},M_1)$ and $\mathit{Missile}(M_1)$ come from the sentence with $\exists$, and why is this step needed for forward chaining? (b) Translate in the same style: "All of Nono's tanks were given to it by Ivan."
> (a) Existential instantiation: $\exists x\ \mathit{Owns}(\mathit{Nono},x)\land\mathit{Missile}(x)$ with the new Skolem constant $M_1$ gives $\mathit{Owns}(\mathit{Nono},M_1)\land\mathit{Missile}(M_1)$, which is two facts. A sentence with $\exists$ is not a definite clause. Forward chaining works on definite clauses only: facts, and rules with one positive conclusion.
> The slide asks if $\exists$ is the correct translation of "some". $\exists$ means "at least one". "Some missiles" may mean two or more, but one missile is enough for the proof.
> (b) $\forall x\ \mathit{Tank}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Gave}(\mathit{Ivan},x,\mathit{Nono})$. "All" becomes $\forall$ with $\Rightarrow$, and the conditions "is a tank" and "Nono owns it" are the premises.

> [!question]- Check (inference deck, slide 25): In the algorithm, (a) why is $q'$ added only if it is "not a renaming of a sentence already in KB or new", and (b) why are the conclusions put into `new` and not directly into the KB?
> (a) Without this test the algorithm would derive the same facts again in every iteration. `new` would never be empty and the loop would never stop. With the test, an iteration that finds nothing really new ends the loop (fixed point). A renaming such as $\mathit{Likes}(y,\mathit{IceCream})$ for $\mathit{Likes}(x,\mathit{IceCream})$ is the same fact, so it is refused too.
> (b) So that one iteration uses only the facts known at its start. Then "iteration $k$" has a clear meaning: the facts that need a proof of height $k$. The answer does not depend on the order of the rules. The check `new` is empty is also a clean test for the fixed point.

> [!question]- Check (inference deck, slides 26-27): Trace forward chaining on this KB (algorithm of slide 25: the new facts of an iteration are added to the KB at its end). Rules: R1 $\mathit{Parent}(x,y)\Rightarrow\mathit{Ancestor}(x,y)$, R2 $\mathit{Parent}(x,y)\land\mathit{Ancestor}(y,z)\Rightarrow\mathit{Ancestor}(x,z)$. Facts: $\mathit{Parent}(\mathit{Ali},\mathit{Can})$, $\mathit{Parent}(\mathit{Can},\mathit{Ece})$, $\mathit{Parent}(\mathit{Ece},\mathit{Naz})$. Which rule fires in each iteration, with which new facts? When is the fixed point reached? Draw the proof tree of $\mathit{Ancestor}(\mathit{Ali},\mathit{Naz})$.
> Iteration 1: no $\mathit{Ancestor}$ fact is known yet, so only R1 fires, once per $\mathit{Parent}$ fact: $\mathit{Ancestor}(\mathit{Ali},\mathit{Can})$, $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$, $\mathit{Ancestor}(\mathit{Ece},\mathit{Naz})$.
> Iteration 2: R2 with $\{x=\mathit{Ali},y=\mathit{Can},z=\mathit{Ece}\}$ gives $\mathit{Ancestor}(\mathit{Ali},\mathit{Ece})$. R2 with $\{x=\mathit{Can},y=\mathit{Ece},z=\mathit{Naz}\}$ gives $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$. R1 gives nothing new.
> Iteration 3: R2 with $\{x=\mathit{Ali},y=\mathit{Can},z=\mathit{Naz}\}$ uses the new $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$ and gives $\mathit{Ancestor}(\mathit{Ali},\mathit{Naz})$.
> Iteration 4: nothing new. Fixed point: 3 $\mathit{Parent}$ facts and 6 $\mathit{Ancestor}$ facts.
> Proof tree: $\mathit{Ancestor}(\mathit{Ali},\mathit{Naz})$ has the children $\mathit{Parent}(\mathit{Ali},\mathit{Can})$ and $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$ (R2, with an arc). $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$ has the children $\mathit{Parent}(\mathit{Can},\mathit{Ece})$ and $\mathit{Ancestor}(\mathit{Ece},\mathit{Naz})$ (R2). $\mathit{Ancestor}(\mathit{Ece},\mathit{Naz})$ has the one child $\mathit{Parent}(\mathit{Ece},\mathit{Naz})$ (R1).

> [!question]- Check (inference deck, slide 28): Read the finished figure. (a) Why do two lines start at $\mathit{Missile}(M_1)$? (b) What does the arc under $\mathit{Criminal}(\mathit{West})$ mean, and which substitution is used there? (c) Remove the fact $\mathit{Enemy}(\mathit{Nono},\mathit{America})$ from the KB. Which facts does forward chaining still derive?
> (a) $\mathit{Missile}(M_1)$ is a premise of two rule applications: $\mathit{Missile}(x)\Rightarrow\mathit{Weapon}(x)$ and $\mathit{Missile}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Sells}(\mathit{West},x,\mathit{Nono})$. A fact can be used as often as needed.
> (b) The arc joins the four premises of the crime rule: all four are needed (AND). The substitution is $\{x=\mathit{West},\ y=M_1,\ z=\mathit{Nono}\}$.
> (c) Iteration 1 still gives $\mathit{Weapon}(M_1)$ and $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$. $\mathit{Hostile}(\mathit{Nono})$ is not derived, because its only rule needs the $\mathit{Enemy}$ fact. Then the crime rule has no fact for $\mathit{Hostile}(z)$. Iteration 2 adds nothing: fixed point without $\mathit{Criminal}(\mathit{West})$.

> [!question]- Check (inference deck, slide 29): (a) The KB is $\mathit{Nat}(\mathit{Zero})$ and $\mathit{Nat}(x)\Rightarrow\mathit{Nat}(s(x))$. The query is $\mathit{Nat}(\mathit{Ali})$. What does forward chaining do? (b) For the ancestor KB of the previous question ($p=2$ predicates of arity $k=2$, $n=4$ constants), what is the bound $p\cdot n^k$, and what does it say about termination?
> (a) Iteration 1 adds $\mathit{Nat}(s(\mathit{Zero}))$, iteration 2 adds $\mathit{Nat}(s(s(\mathit{Zero})))$, and so on. Every iteration adds a new fact, so `new` is never empty. The query is not entailed and never appears. The algorithm runs forever. The cause is the function symbol $s$: it makes infinitely many ground terms.
> (b) $p\cdot n^k=2\cdot4^2=32$ possible ground atoms. Without functions (Datalog) no other facts can exist. Every iteration except the last adds at least one new fact, so there are at most 32 such iterations and then one that adds nothing. The trace needed 4 iterations and reached 9 facts.

> [!question]- Check (inference deck, slide 30): In the Colonel West KB, iteration 1 adds $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$ and $\mathit{Hostile}(\mathit{Nono})$. With the observation of this slide, which of the four rules must be looked at in iteration 2? Why are the other three skipped?
> Only the crime rule $\mathit{American}(x)\land\mathit{Weapon}(y)\land\mathit{Sells}(x,y,z)\land\mathit{Hostile}(z)\Rightarrow\mathit{Criminal}(x)$. Its premises contain the newly added predicates $\mathit{Weapon}$, $\mathit{Sells}$ and $\mathit{Hostile}$.
> The other rules have the premises $\mathit{Missile}$, $\mathit{Owns}$ and $\mathit{Enemy}$. Iteration 1 added no fact with these predicates. So these rules can only repeat the conclusions that they already gave in iteration 1.
> For the crime rule the remaining premise $\mathit{American}(x)$ is retrieved from the indexed facts: $\mathit{American}(\mathit{West})$.

> [!question]- Check (inference deck, slide 31): (a) The slide asks why statements like $nt=\mathit{Red}\lor nt=\mathit{Blue}\lor nt=\mathit{Green}$ are not needed. Why not? (b) The rule has 6 variables. How many ways are there to give each a colour, and how many of them satisfy all 9 premises? (c) Why is Tasmania (T) not in the rule?
> (a) Every variable of the rule appears inside a $\mathit{Diff}$ premise. A premise can only be matched with one of the six $\mathit{Diff}$ facts, and these contain only $\mathit{Red}$, $\mathit{Green}$ and $\mathit{Blue}$. So every variable gets one of the three colours automatically.
> (b) $3^6=729$ combinations for $wa,nt,sa,q,nsw,v$. Only 6 satisfy all premises. Reason: $wa$, $nt$, $sa$ touch each other, so they take the three colours in $3!=6$ ways. Then $q$ must be the colour of $wa$ (it touches $nt$ and $sa$), $nsw$ the colour of $nt$, and $v$ the colour of $wa$. Matching the rule means finding one of these 6 among the 729: this is solving the CSP.
> (c) T has no neighbour, so no $\mathit{Diff}$ premise mentions it. It can have any colour.

## 17. Backward chaining and Prolog (inference deck, slides 32-44)
![[cmpe421_lecture_8_inference_predicatelogic_p032.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p033.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p034.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p035.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p036.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p037.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p038.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p039.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p040.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p041.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p042.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p043.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p044.jpg]]

**Notes.** Backward chaining starts from the goal $\mathit{Criminal}(\mathit{West})$, unifies it with a rule head and proves the premises one by one, carrying the substitution along. It is a depth-first proof search: space is linear in the size of the proof. It is incomplete because of infinite loops (partly fixed by checking the goal stack) and inefficient because of repeated subgoals (fixed by caching).

**Prolog** = backward chaining with Horn clauses plus extras. A program is a set of clauses `head :- literal1, ..., literaln.` Capitalized words are variables. Negation is "negation as failure": `alive(joe)` succeeds if `dead(joe)` fails.

> [!info] Reading FOL-BC-Ask, the goal tree and Prolog (beyond the slides)
> **The algorithm (slide 32).** The input is a list of goals $[g_1,\dots,g_k]$ and the substitution $\theta$ built so far. Empty list: everything is proved, return $\theta$. Otherwise take the first goal $g_1$ and try every sentence of the KB, in order. A fact counts as a rule with no premises ($n=0$). If the head $q$ unifies with $g_1$ by the mgu $\theta'$, the premises of the rule replace $g_1$ in the list, $\theta'$ is applied to all goals, and the function calls itself with the composition $\theta\theta'$ (first $\theta$, then $\theta'$). If no sentence unifies, this branch fails and the caller tries its next sentence: **backtracking**.
> 
> **The tree (slides 33-39).** The children of a box are the premises of the rule whose head matched it. The arc means AND. Under a box stands the substitution found when this goal was matched: $\{\}$ means "matched a fact, no new binding". The substitution at the top right is the composition of all of them so far. The subgoals are proved from left to right, and each binding is applied to the goals that still wait. This is why $\mathit{Sells}(x,y,z)$ has become $\mathit{Sells}(\mathit{West},M_1,z)$ on slide 38.
> 
> **Prolog (slides 41-44).** `head :- a, b.` means $a\land b\Rightarrow\mathit{head}$. The sign `:-` reads "if" and the comma reads "and". Prolog tries the clauses from top to bottom and the subgoals from left to right. When a subgoal fails it goes back to the last choice and takes the next clause. The list `[X|L]` has the first element `X` and the rest `L`: `[1,2,3]` is `[1|[2,3]]`, and `[]` is the empty list.
> **Negation as failure** rests on the **closed-world assumption**: what cannot be proved from the program is taken to be false. Logic does not do this. A FOL KB that does not mention $\mathit{Dead}(\mathit{Joe})$ entails neither $\mathit{Dead}(\mathit{Joe})$ nor $\neg\mathit{Dead}(\mathit{Joe})$.

> [!question]- Check (inference deck, slide 32): The slide asks "What is $\theta\theta'$?". Answer it, and compute it for $\theta=\{x=f(y)\}$ and $\theta'=\{y=A,\ z=B\}$. What does the algorithm return when the goal list is empty, and why?
> $\theta\theta'$ is the composition: the substitution that has the same effect as applying $\theta$ first and $\theta'$ after it.
> Apply $\theta'$ to the right side of $\theta$: $x=f(A)$. Add the bindings of $\theta'$: $y=A$, $z=B$. $\theta\theta'=\{x=f(A),\ y=A,\ z=B\}$.
> Test on $P(x,y,z)$: after $\theta$ it is $P(f(y),y,z)$, after $\theta'$ it is $P(f(A),A,B)$. The composition gives $P(f(A),A,B)$ in one step.
> Empty goal list: all goals are proved, so the collected substitution $\theta$ is one answer. The function returns $\{\theta\}$. It contains the values of the query variables.

> [!question]- Check (inference deck, slides 33-35): (a) Which KB sentence is used on slide 34, and what is $\{x=\mathit{West}\}$? (b) What does $\{\}$ under $\mathit{American}(\mathit{West})$ on slide 35 mean? (c) How would the top of the tree differ if the query were $\mathit{Criminal}(w)$ with a variable $w$?
> (a) The crime rule. Its head $\mathit{Criminal}(x)$ is unified with the goal $\mathit{Criminal}(\mathit{West})$. The mgu is $\{x=\mathit{West}\}$. The four premises become the new subgoals, joined by an arc (all must be proved).
> (b) After applying $\{x=\mathit{West}\}$ the first subgoal is $\mathit{American}(\mathit{West})$. It is identical to a fact of the KB. The unifier is empty, so no new binding is added.
> (c) The head would unify with $\{x=w\}$ (the same variable under two names). The first subgoal would be $\mathit{American}(w)$. It matches the fact $\mathit{American}(\mathit{West})$ with $\{w=\mathit{West}\}$. So backward chaining would find out who the criminal is, and the final answer would contain $w=\mathit{West}$.

> [!question]- Check (inference deck, slides 36-37): (a) How is the goal $\mathit{Weapon}(y)$ proved, and where does $\{y=M_1\}$ come from? (b) Suppose the KB also has the fact $\mathit{Missile}(M_2)$, written before $\mathit{Missile}(M_1)$, and there is no fact $\mathit{Owns}(\mathit{Nono},M_2)$. What does backward chaining do?
> (a) $\mathit{Weapon}(y)$ matches no fact. It unifies with the head of $\mathit{Missile}(x')\Rightarrow\mathit{Weapon}(x')$ (variable renamed) by $\{x'=y\}$. The new subgoal is $\mathit{Missile}(y)$. This unifies with the fact $\mathit{Missile}(M_1)$ by $\{y=M_1\}$. The substitution at the top grows to $\{x=\mathit{West},\ y=M_1\}$.
> (b) Depth-first: it takes the first match, $\{y=M_2\}$. The next goal is then $\mathit{Sells}(\mathit{West},M_2,z)$. The Sells rule gives the subgoals $\mathit{Missile}(M_2)$, which succeeds, and $\mathit{Owns}(\mathit{Nono},M_2)$, which fails. The algorithm backtracks to the last choice, the goal $\mathit{Missile}(y)$, takes the next fact with $\{y=M_1\}$ and continues as on the slides. The work for $M_2$ is lost.

> [!question]- Check (inference deck, slides 38-39): Use the ancestor KB in this order: R1 $\mathit{Parent}(x,y)\Rightarrow\mathit{Ancestor}(x,y)$, R2 $\mathit{Parent}(x,y)\land\mathit{Ancestor}(y,z)\Rightarrow\mathit{Ancestor}(x,z)$, then the facts $\mathit{Parent}(\mathit{Ali},\mathit{Can})$, $\mathit{Parent}(\mathit{Can},\mathit{Ece})$. Trace backward chaining for the goal $\mathit{Ancestor}(\mathit{Ali},\mathit{Ece})$ (sentences are tried in this order, subgoals from left to right): the goal tree, the substitutions and the dead end.
> Goal $\mathit{Ancestor}(\mathit{Ali},\mathit{Ece})$.
> Try R1 (renamed $x_1,y_1$): mgu $\{x_1=\mathit{Ali},\ y_1=\mathit{Ece}\}$. Subgoal $\mathit{Parent}(\mathit{Ali},\mathit{Ece})$. No fact and no rule head unifies with it: fail. Backtrack.
> Try R2 (renamed $x_2,y_2,z_2$): mgu $\{x_2=\mathit{Ali},\ z_2=\mathit{Ece}\}$. Subgoals: $\mathit{Parent}(\mathit{Ali},y_2)$ and $\mathit{Ancestor}(y_2,\mathit{Ece})$, joined by an arc.
> $\mathit{Parent}(\mathit{Ali},y_2)$ unifies with the fact $\mathit{Parent}(\mathit{Ali},\mathit{Can})$: $\{y_2=\mathit{Can}\}$. Apply it to the waiting goal: $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$.
> $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$: try R1 (renamed $x_3,y_3$): $\{x_3=\mathit{Can},\ y_3=\mathit{Ece}\}$. Subgoal $\mathit{Parent}(\mathit{Can},\mathit{Ece})$ is a fact: $\{\}$.
> No goals are left: proved. Composition of all bindings: $\{x_2=\mathit{Ali},\ z_2=\mathit{Ece},\ y_2=\mathit{Can},\ x_3=\mathit{Can},\ y_3=\mathit{Ece}\}$.
> Compare with forward chaining on the same KB: it would derive every $\mathit{Ancestor}$ fact. Backward chaining touched only what the goal needs.

> [!question]- Check (inference deck, slide 40): The KB has the rule $\mathit{Even}(\mathit{plus2}(x))\Rightarrow\mathit{Even}(x)$ and the fact $\mathit{Even}(\mathit{Zero})$, in this order. The query is $\mathit{Even}(\mathit{One})$. Write the first three goals of backward chaining. Does the check "is the current goal already on the stack?" stop the loop? Why does it stop the loop for $P(x)\Rightarrow P(x)$?
> The goal $\mathit{Even}(\mathit{One})$ unifies with the rule head $\mathit{Even}(x)$: new goal $\mathit{Even}(\mathit{plus2}(\mathit{One}))$. Again: $\mathit{Even}(\mathit{plus2}(\mathit{plus2}(\mathit{One})))$. And so on.
> The goals are $\mathit{Even}(\mathit{One})$, $\mathit{Even}(\mathit{plus2}(\mathit{One}))$, $\mathit{Even}(\mathit{plus2}(\mathit{plus2}(\mathit{One})))$. Each one is different from all the goals before it, so the stack check never finds a repeat. The search goes down forever and gives no answer, although the true answer is simply "not provable". This is the case $Q(f(x))\Rightarrow Q(x)$ of the slide.
> With $P(x)\Rightarrow P(x)$ the goal $P(A)$ produces the subgoal $P(A)$ again. This is the same goal as one on the stack, so the check sees it and cuts this branch.

> [!question]- Check (inference deck, slide 41): Write the ancestor KB (R1, R2 and the two Parent facts of the previous questions) as a Prolog program. Then find the mistake in this line: `Parent(ali, Can).`
> ```
> ancestor(X,Y) :- parent(X,Y).
> ancestor(X,Z) :- parent(X,Y), ancestor(Y,Z).
> parent(ali,can).
> parent(can,ece).
> ```
> The head stands on the left of `:-`, the premises on the right, separated by commas. Every clause ends with a full stop. Variables are capitalized (`X`, `Y`, `Z`). Constants and predicate names start with a lower-case letter.
> Mistake: the capitalization is the opposite of our logic notation. In Prolog a capitalized word is a variable. So `Parent` is read as a variable and not as a predicate name, and the line is not a valid clause. `Can` is a variable too: even `parent(ali, Can).` would say "ali is a parent of everything". Correct: `parent(ali, can).`

> [!question]- Check (inference deck, slide 42): Program: `bird(tweety).` `bird(pingu).` `penguin(pingu).` `flies(X) :- bird(X), not penguin(X).` What does Prolog answer to `flies(tweety)`, `flies(pingu)` and `flies(rex)`? Which answer shows the difference between negation as failure and real negation $\neg$?
> `flies(tweety)`: `bird(tweety)` succeeds. `penguin(tweety)` cannot be proved, it fails, so `not penguin(tweety)` succeeds. Answer: yes.
> `flies(pingu)`: `bird(pingu)` succeeds. `penguin(pingu)` succeeds, so `not penguin(pingu)` fails. Answer: no.
> `flies(rex)`: `bird(rex)` fails. Answer: no.
> The first answer shows it. The program does not say that Tweety is not a penguin. It only does not say that Tweety is one. Prolog treats "not provable" as "false" (closed world). In FOL the facts $\mathit{Bird}(\mathit{Tweety})$ and $\forall x\ \mathit{Bird}(x)\land\neg\mathit{Penguin}(x)\Rightarrow\mathit{Flies}(x)$ do not entail $\mathit{Flies}(\mathit{Tweety})$, because $\neg\mathit{Penguin}(\mathit{Tweety})$ is not known.

> [!question]- Check (inference deck, slide 43): Clause: `avg(Y,Z,X) :- X is (Y + Z)/2.` What does Prolog answer to `avg(7,3,X)`? What happens with `avg(Y,3,5)`, and why does this not fit pure logic?
> `avg(7,3,X)`: `Y = 7`, `Z = 3` are bound by unification with the head. `is` evaluates the right side: $(7+3)/2=10/2=5$. Answer: `X = 5`, in the same way as the slide gets `X = 3` from $(4+2)/2$.
> `avg(Y,3,5)`: the right side `(Y + 3)/2` contains the unbound variable `Y`. The slide says that `is` only works if the variables on the right side are already instantiated. `is` evaluates a number, it does not solve the equation for `Y`. So Prolog does not answer `Y = 7`. What it prints depends on the Prolog system, usually an error message (beyond the slides).
> In logic the relation "X is the average of Y and Z" has no direction: $Y=7$ would be a correct answer. The built-in `is` works in one direction only, from right to left. This is why the slide calls it a kludge.

> [!question]- Check (inference deck, slide 44): With the two `append` clauses of the slide, show step by step how Prolog answers `append([1,2],[3],W)`. Then: how many answers does `append(A,B,[1,2,3])` give, and which comes first? In `dfs`, why must the clause `dfs(X) :- goal(X).` stand first?
> Goal `append([1,2],[3],W)`. Clause 1 needs an empty first list: no match. Clause 2 `append([X|L],Y,[X|Z])`: `X = 1`, `L = [2]`, `Y = [3]`, `W = [1|Z]`. New goal `append([2],[3],Z)`.
> Clause 2 again (new variables): `X' = 2`, `L' = []`, `Z = [2|Z']`. New goal `append([],[3],Z')`.
> Clause 1 `append([],Y,Y)`: `Z' = [3]`. No goals left.
> Put together: `Z = [2,3]`, `W = [1,2,3]`.
> `append(A,B,[1,2,3])`: 4 answers, one for each place where the list can be cut. Clause 1 is tried first, so the first answer is `A = []`, `B = [1,2,3]`. Then `A = [1]`, `B = [2,3]`, then `A = [1,2]`, `B = [3]`, then `A = [1,2,3]`, `B = []`.
> `dfs`: clauses are tried from top to bottom. With the goal test first, a goal state is recognised before any successor is expanded. In the other order Prolog would first search below every state, also below a goal state, and could run into a long or infinite path before it tests the state itself.

## 18. Resolution in FOL (inference deck, slides 45-48)
![[cmpe421_lecture_8_inference_predicatelogic_p045.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p046.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p047.jpg]]
![[cmpe421_lecture_8_inference_predicatelogic_p048.jpg]]

**Notes.** Resolution works as in propositional logic, with a substitution $\theta$ that unifies the two complementary literals. Example: $\neg\mathit{Rich}(x)\lor\mathit{Unhappy}(x)$ and $\mathit{Rich}(\mathit{Ken})$ give $\mathit{Unhappy}(\mathit{Ken})$ with $\theta=\{x=\mathit{Ken}\}$. To prove $\alpha$, convert $KB\land\neg\alpha$ to CNF and resolve.

Conversion to CNF: (1) remove $\Leftrightarrow$ and $\Rightarrow$, (2) move $\neg$ inwards, (3) standardize variables, (4) Skolemize: replace each existential variable by a Skolem function of the enclosing universal variables, (5) drop the universal quantifiers, (6) distribute $\lor$ over $\land$.

> [!info] Skolem functions and refutation proofs (beyond the slides)
> **The resolution rule (slide 45).** Take two clauses with different variable names. Find a literal $\ell_i$ in the first and a literal $m_j$ in the second that are complementary after unification: $\ell_i\theta=\neg m_j\theta$. Remove both, join the remaining literals with $\lor$, and apply $\theta$ to all of them. The result is the **resolvent**.
> 
> **Why a Skolem function (slide 47).** In $\forall x\ \exists y\ \mathit{Loves}(y,x)$ the person $y$ may be a different one for each $x$. A constant $C$ would say $\forall x\ \mathit{Loves}(C,x)$: one person loves everybody. This is the stronger sentence $\exists y\ \forall x$. The function $G(x)$ keeps the dependence: "the one who loves $x$". The rule: an existential variable becomes a new function of all universal variables in whose scope it stands. If there is none, it becomes a Skolem constant, as in existential instantiation.
> After step 4 only universal quantifiers remain. Step 5 drops them because every variable in a clause is read as universally quantified.
> 
> **Refutation (slides 45 and 48).** To prove $\alpha$ from the KB, add $\neg\alpha$ and resolve until the **empty clause** appears (the small empty box at the bottom of slide 48). The empty clause is a contradiction, so $KB\land\neg\alpha$ is unsatisfiable, and this means $KB\models\alpha$. The substitutions used on the way give the values of the query variables.
> On slide 48 the left boxes are the KB clauses. The right column starts with the negated query $\neg\mathit{Criminal}(\mathit{West})$. Each new box on the right is a resolvent. Its negative literals are the goals that are still open. The column runs through the same goals as the backward-chaining tree of slides 33-39: for definite clauses, backward chaining is this special form of resolution.

> [!warning] Slide errors (slides 47 and 48)
> Slide 47, step 6 is titled "Distribute $\land$ over $\lor$". The step that is done distributes $\lor$ over $\land$: $(a\land b)\lor c$ becomes $(a\lor c)\land(b\lor c)$. The result on the slide is correct.
> Slide 48: the last green box on the right shows $\mathit{Enemy}(\mathit{Nono},\mathit{America})$ without a negation sign. It is the resolvent of $\neg\mathit{Enemy}(x,\mathit{America})\lor\mathit{Hostile}(x)$ and $\neg\mathit{Hostile}(\mathit{Nono})$, so it must be $\neg\mathit{Enemy}(\mathit{Nono},\mathit{America})$. Only then does it resolve with the fact $\mathit{Enemy}(\mathit{Nono},\mathit{America})$ to the empty clause.

> [!question]- Check (inference deck, slide 45): Resolve $\neg\mathit{Likes}(x,\mathit{IceCream})\lor\mathit{Happy}(x)$ with $\mathit{Likes}(\mathit{Ece},y)\lor\neg\mathit{Sweet}(y)$. Give the unifier and the resolvent. What does the procedure do if the query is not entailed?
> Complementary pair: $\neg\mathit{Likes}(x,\mathit{IceCream})$ and $\mathit{Likes}(\mathit{Ece},y)$.
> Unify $\mathit{Likes}(x,\mathit{IceCream})$ with $\mathit{Likes}(\mathit{Ece},y)$: $\theta=\{x=\mathit{Ece},\ y=\mathit{IceCream}\}$.
> Remove the pair, keep the rest, apply $\theta$: $(\mathit{Happy}(x)\lor\neg\mathit{Sweet}(y))\theta=\mathit{Happy}(\mathit{Ece})\lor\neg\mathit{Sweet}(\mathit{IceCream})$.
> In words: if ice cream is sweet, Ece is happy.
> Not entailed: the empty clause never appears. The procedure may go on producing new resolvents forever (slide: "won't necessarily terminate"). Resolution is complete, but FOL entailment is only semidecidable.

> [!question]- Check (inference deck, slide 46): Do steps 1 and 2 for "it is not true that every student has a kind friend": $\neg\forall x\ [\mathit{Student}(x)\Rightarrow\exists y\ (\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$. Then finish the conversion. Which kind of Skolem term do you need?
> Step 1, remove $\Rightarrow$: $\neg\forall x\ [\neg\mathit{Student}(x)\lor\exists y\ (\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$.
> Step 2, move $\neg$ inwards, one level at a time:
> $\exists x\ \neg[\neg\mathit{Student}(x)\lor\exists y\ (\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$
> $\exists x\ [\mathit{Student}(x)\land\neg\exists y\ (\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$ (De Morgan, double negation)
> $\exists x\ [\mathit{Student}(x)\land\forall y\ \neg(\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$
> $\exists x\ [\mathit{Student}(x)\land\forall y\ (\neg\mathit{Friend}(x,y)\lor\neg\mathit{Kind}(y))]$ (De Morgan)
> Step 3: the variables $x$ and $y$ are already different.
> Step 4: $\exists x$ stands inside no universal quantifier, so $x$ becomes a Skolem constant $S_1$: $\mathit{Student}(S_1)\land\forall y\ (\neg\mathit{Friend}(S_1,y)\lor\neg\mathit{Kind}(y))$.
> Step 5, drop $\forall y$. Step 6: nothing to distribute. Two clauses: $\mathit{Student}(S_1)$ and $\neg\mathit{Friend}(S_1,y)\lor\neg\mathit{Kind}(y)$.
> In words: there is a student $S_1$, and no friend of $S_1$ is kind.

> [!question]- Check (inference deck, slide 47): Convert "between two numbers there is always a third": $\forall x\ \forall y\ [\mathit{Less}(x,y)\Rightarrow\exists z\ (\mathit{Less}(x,z)\land\mathit{Less}(z,y))]$ to CNF. Why would a Skolem constant be wrong here?
> Step 1: $\forall x\ \forall y\ [\neg\mathit{Less}(x,y)\lor\exists z\ (\mathit{Less}(x,z)\land\mathit{Less}(z,y))]$.
> Step 2: no negation stands in front of a quantifier or a bracket. Step 3: the variables are already different.
> Step 4: $\exists z$ is inside $\forall x$ and $\forall y$, so $z$ becomes a Skolem function of both, $M(x,y)$: $\forall x\ \forall y\ [\neg\mathit{Less}(x,y)\lor(\mathit{Less}(x,M(x,y))\land\mathit{Less}(M(x,y),y))]$.
> Step 5: $\neg\mathit{Less}(x,y)\lor(\mathit{Less}(x,M(x,y))\land\mathit{Less}(M(x,y),y))$.
> Step 6, distribute $\lor$ over $\land$: $[\neg\mathit{Less}(x,y)\lor\mathit{Less}(x,M(x,y))]\land[\neg\mathit{Less}(x,y)\lor\mathit{Less}(M(x,y),y)]$. Two clauses.
> A constant $C$ would say: one fixed number $C$ lies between every pair $x<y$. That is false for numbers. The number in the middle depends on $x$ and $y$, and $M(x,y)$ expresses this.

> [!question]- Check (inference deck, slide 48): KB: "everyone has a mother" $\forall x\ \exists y\ \mathit{Mother}(y,x)$ and "a mother is a parent" $\forall x\ \forall y\ \mathit{Mother}(x,y)\Rightarrow\mathit{Parent}(x,y)$. Prove by resolution that Ali has a parent: $\exists w\ \mathit{Parent}(w,\mathit{Ali})$. Give the clauses, each resolution step with its unifier, and the value found for $w$.
> Clauses of the KB: (1) $\mathit{Mother}(m(x_1),x_1)$, with the Skolem function $m$ for $y$. (2) $\neg\mathit{Mother}(x,y)\lor\mathit{Parent}(x,y)$.
> Negated query: $\neg\exists w\ \mathit{Parent}(w,\mathit{Ali})\equiv\forall w\ \neg\mathit{Parent}(w,\mathit{Ali})$. Clause (3) $\neg\mathit{Parent}(w,\mathit{Ali})$.
> Step 1, resolve (3) with (2) on $\mathit{Parent}$: $\theta=\{x=w,\ y=\mathit{Ali}\}$. Resolvent (4) $\neg\mathit{Mother}(w,\mathit{Ali})$.
> Step 2, resolve (4) with (1) on $\mathit{Mother}$: unify $\mathit{Mother}(w,\mathit{Ali})$ with $\mathit{Mother}(m(x_1),x_1)$: $\theta=\{x_1=\mathit{Ali},\ w=m(\mathit{Ali})\}$. Resolvent: the empty clause.
> The empty clause is a contradiction, so the KB entails $\exists w\ \mathit{Parent}(w,\mathit{Ali})$. The answer is $w=m(\mathit{Ali})$: the parent is "the mother of Ali", named by the Skolem function.
> As on the slide, the proof starts from the negated query and each resolvent is the list of goals that are still open.

## Key formulas and terms
| Term | Meaning / formula |
|---|---|
| Term | constant, variable, or function applied to terms. Names an object |
| Atomic sentence | predicate applied to terms, or $t_1=t_2$. True or false |
| World | $M=(D,I)$: domain and interpretation |
| Quantifier duality | $\neg\forall x\,P\equiv\exists x\,\neg P$, $\neg\exists x\,P\equiv\forall x\,\neg P$ |
| Substitution | $S\theta$: apply the bindings of $\theta$ to $S$, simultaneously |
| Unifier | $\theta$ with $p\theta=q\theta$ |
| GMP | facts $p_i'$, rule $p_1\land\dots\land p_n\Rightarrow q$, $p_i'\theta=p_i\theta$: infer $q\theta$ |
| State | set of ground atoms. $p\,c^k$ atoms, $2^{p\,c^k}$ states |
| Executable | $\text{precond}^+(a)\subseteq s$ and $\text{precond}^-(a)\cap s=\emptyset$ |
| Result | $\gamma(s,a)=(s-\text{effects}^-(a))\cup\text{effects}^+(a)$ |
| Solution | plan executable in $s_0$ whose result satisfies $g$ |
| Planning graph | alternating literal and action layers of a relaxed problem |
| Mutex actions | inconsistent effects, interference, competing needs |
| FF | greedy forward search on a planning-graph heuristic |

## Check yourself
> [!question]- 1. Term, atomic sentence, complex sentence or not allowed: (a) $\mathit{Sqrt}(2)$, (b) $\mathit{Brother}(x,\mathit{Richard})$, (c) $\mathit{Sqrt}({>}(1,2))$?
> (a) term. (b) atomic sentence. (c) not allowed: a function takes terms, not sentences.

> [!question]- 2. Rewrite $\neg\exists x\ (\mathit{Student}(x)\land\mathit{Lazy}(x))$ with $\forall$ and an implication.
> Flip the quantifier: $\forall x\ \neg(\mathit{Student}(x)\land\mathit{Lazy}(x))$, which is $\forall x\ \mathit{Student}(x)\Rightarrow\neg\mathit{Lazy}(x)$.

> [!question]- 3. Unify or say why it fails: (a) $P(x,x)$ and $P(A,B)$, (b) $Q(F(x),B)$ and $Q(y,x)$.
> (a) $x=A$, then $A$ against $B$: two different constants, fail. (b) $y=F(x)$, then $x=B$, applied to the first binding: $\{y=F(B),\ x=B\}$.

> [!question]- 4. How many ground atoms and states does the blocks world with 3 blocks have?
> `ontable` 3, `on` $3^2=9$, `clear` 3, `holding` 3, `handempty` 1: 19 atoms. $2^{19}=524\,288$ states.

> [!question]- 5. $s=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `holding(d)`, `ontable(e)`, `clear(e)`$\}$. Compute $\gamma(s,$`stack(d,c)`$)$.
> Executable: `holding(d)` and `clear(c)` are in $s$. Delete `holding(d)`, `clear(c)`. Add `on(d,c)`, `clear(d)`, `handempty`. Result: `ontable(a)`, `on(c,a)`, `on(d,c)`, `clear(d)`, `ontable(b)`, `clear(b)`, `ontable(e)`, `clear(e)`, `handempty` (9 atoms).

> [!question]- 6. Dinner date: at action-level 1, is `cook` mutex with `wrap`? Is `carry` mutex with `cook`?
> `cook` and `wrap`: no effect negates another, neither deletes a precondition, preconditions are not mutex. Not mutex. `carry` deletes cleanHands, the precondition of `cook`: interference, mutex.

## Links
- Course index: [[CMPE421 Home]]
- Previous lecture: [[CMPE421 L08 - Logical agents]]
- Next lecture: [[CMPE421 L10 - Quantifying uncertainty]]
- Full step-by-step notes (PDF): [CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lecture_notes/lecture_09/CMPE421_Lecture_09_Predicate_Logic_and_Planning.pdf`
- Original slides (PDF, stored in the lecture_07 folder): [PredicateLogic.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/PredicateLogic.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/PredicateLogic.pdf`
- Original slides (PDF): [DeterministicPlanning.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/DeterministicPlanning.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/lecture_07/DeterministicPlanning.pdf`
- Extra deck (PDF, no lecture notes yet): [Inference-PredicateLogic.pdf](file:///home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/Lecture%208/Inference-PredicateLogic.pdf) `/home/umaysamli/Desktop/university/ms/semester_2/cmpe_421/lectures/Lecture 8/Inference-PredicateLogic.pdf`
