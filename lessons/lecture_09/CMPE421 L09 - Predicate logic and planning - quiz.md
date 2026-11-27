---
course: CMPE 421 Artificial Intelligence
lesson: CMPE421 L09 - Predicate logic and planning
type: quiz
questions: 94
---

# CMPE421 L09 - Predicate logic and planning - quiz

Lesson note: [[CMPE421 L09 - Predicate logic and planning]]. One question per check question of the lesson. Pick an option, then open the answer.

## Part A. Predicate logic (deck PredicateLogic)

## 1. What FOL talks about (slides 1-2)

### Q1 (PredicateLogic, slide 1)

In first-order logic a symbol can stand for an object, a relation or a function. Classify these three, in this order: (b) "x is older than y", (c) "the birth year of x", (d) "x is even".

- A) (b) relation, (c) relation, (d) function
- B) (b) function, (c) relation, (d) function
- C) (b) relation, (c) function, (d) relation
- D) (b) function, (c) function, (d) relation

> [!success]- Answer
> **C.** Test: a relation applied to its objects is true or false. A function applied to its objects gives back one object.
> (b) gives true or false: a relation between two objects. (c) gives back a number (an object): a function. (d) gives true or false: a relation with one argument (a property).
> - A: "The birth year of x" does not say true or false. It names a year, so it is a function. "x is even" is not a function: it gives true or false.
> - B: "Older than" is not a function. It does not give back an object, only true or false.
> - D: The last two are right, but "older than" gives true or false, so it is a relation.

### Q2 (PredicateLogic, slide 2)

Sentence 1: "I believe with 0.7 that it rains tomorrow." Sentence 2: "The room is warm to degree 0.7." Which language of the "Logics in general" table fits each sentence?

- A) Sentence 1: probability theory. Sentence 2: fuzzy logic.
- B) Sentence 1: fuzzy logic. Sentence 2: probability theory.
- C) Sentence 1: probability theory. Sentence 2: probability theory.
- D) Sentence 1: fuzzy logic. Sentence 2: fuzzy logic.

> [!success]- Answer
> **A.** In sentence 1 the fact "it rains tomorrow" is either true or false. The 0.7 is the agent's degree of belief: probability theory.
> In sentence 2 the fact itself is true to a degree: the room is partly warm. Nothing is unknown. This is a degree of truth: fuzzy logic.
> - B: This swaps the two. A degree of belief belongs to probability theory, a degree of truth to fuzzy logic.
> - C: Sentence 2 has nothing unknown, so it is not a degree of belief.
> - D: Rain tomorrow is a plain true-or-false fact. The 0.7 measures the agent's belief, not how true the rain is.

## 2. Syntax (slides 3-5)

### Q3 (PredicateLogic, slide 3)

Look at the sentence $\forall x\ \mathit{Enrolled}(x,\mathit{CMPE421})\Rightarrow\ {>}(\mathit{Age}(x),18)$. What kind of symbol is $\mathit{Age}$, and what kind is $>$?

- A) $\mathit{Age}$ is a predicate symbol. $>$ is a function symbol.
- B) $\mathit{Age}$ is a function symbol. $>$ is a function symbol.
- C) $\mathit{Age}$ is a predicate symbol. $>$ is a predicate symbol.
- D) $\mathit{Age}$ is a function symbol. $>$ is a predicate symbol.

> [!success]- Answer
> **D.** $\mathit{Age}(x)$ gives back an object (a number), so $\mathit{Age}$ is a function symbol. ${>}(\dots,18)$ gives true or false, so $>$ is a predicate symbol.
> The other symbols: $\forall$ quantifier, $x$ variable, $\mathit{Enrolled}$ predicate, $\mathit{CMPE421}$ and $18$ constants, $\Rightarrow$ connective.
> - A: This swaps the two kinds. $\mathit{Age}(x)$ stands inside ${>}(\dots)$ as an argument, so it must be a term, not a sentence.
> - B: $>$ does not give back an object. ${>}(\mathit{Age}(x),18)$ is true or false.
> - C: $\mathit{Age}(x)$ is not true or false. It names a number.

### Q4 (PredicateLogic, slide 4)

How many terms does ${>}(\mathit{Length}(\mathit{LeftLegOf}(\mathit{Richard})),\mathit{Length}(\mathit{LeftLegOf}(\mathit{KingJohn})))$ contain, counting the nested ones?

- A) 2
- B) 6
- C) 4
- D) 7

> [!success]- Answer
> **B.** A constant is a term, and a function applied to a term is again a term. Each side has three: $\mathit{Richard}$, $\mathit{LeftLegOf}(\mathit{Richard})$, $\mathit{Length}(\mathit{LeftLegOf}(\mathit{Richard}))$. The same three for $\mathit{KingJohn}$: $3+3=6$.
> The predicate $>$ turns the two length terms into a sentence.
> - A: This counts only the two outer arguments of $>$ and forgets the nested terms.
> - C: This counts only the function terms and forgets that the constants $\mathit{Richard}$ and $\mathit{KingJohn}$ are terms too.
> - D: This also counts the whole expression. It is a sentence (true or false), not a term.

### Q5 (PredicateLogic, slide 5)

Sentence 1 is ${>}(1,2)\lor{\le}(1,2)$. Sentence 2 is ${>}(1,2)\land\neg{>}(1,2)$. Here $>$ and $\le$ are predicate symbols, and $1$, $2$ are constant symbols. A world may interpret them in any way. Which statement is right?

- A) Sentence 2 is false in every world. Sentence 1 is true with the usual meaning, but false in some worlds.
- B) Sentence 2 is false in every world. Sentence 1 is true in every world.
- C) Sentence 2 is false only with the usual meaning of $>$. Sentence 1 is true in every world.
- D) Sentence 2 is false only with the usual meaning of $>$. Sentence 1 is false in some worlds.

> [!success]- Answer
> **A.** Sentence 2 has the form $A\land\neg A$, so it is false whatever relation $>$ refers to.
> Sentence 1 has the form $A\lor B$ with two different predicate symbols. A world may map both $>$ and $\le$ to relations that do not contain the pair for $(1,2)$. Then both parts are false.
> - B: This reads $>$ and $\le$ with their usual meaning. In FOL they are only symbols, and the world decides what they mean.
> - C: The form $A\land\neg A$ needs no meaning of $>$. It is false in every world. And sentence 1 is not true in every world.
> - D: Sentence 1 is right, but $A\land\neg A$ is false in every world, not only with the usual meaning.

## 3. Truth: worlds and models (slides 6-8)

### Q6 (PredicateLogic, slide 6)

World $M$: domain $D=\{a,b,c\}$. The interpretation maps the constant $A$ to $a$ and $B$ to $b$. $\mathit{Likes}$ is the relation $\{(a,b),(b,b),(c,a)\}$. The function $\mathit{Boss}$ maps $a\to b$, $b\to b$, $c\to a$. Which sentence is FALSE in $M$?

- A) $\mathit{Likes}(\mathit{Boss}(A),B)$
- B) $\mathit{Boss}(B)=B$
- C) $\mathit{Likes}(B,A)$
- D) $\mathit{Likes}(A,\mathit{Boss}(A))\land\neg\mathit{Likes}(B,A)$

> [!success]- Answer
> **C.** $\mathit{Likes}(B,A)$ asks for the pair $(b,a)$. It is not in $\mathit{Likes}$, so the sentence is false.
> - A: $\mathit{Boss}(A)$ refers to $b$, and $(b,b)$ is in $\mathit{Likes}$: true. It looks false only if you take the object whose boss is $a$, which is $c$.
> - B: $\mathit{Boss}(B)$ refers to $b$, and $B$ refers to $b$. Same object: true. It looks false only if you compare the symbols instead of the objects.
> - D: $\mathit{Likes}(A,\mathit{Boss}(A))$ is $(a,b)$: true. $\neg\mathit{Likes}(B,A)$ is true by C. Both true: true. It looks false only if you read the pair in the wrong order, as $(b,a)$.

### Q7 (PredicateLogic, slide 7)

![[cmpe421_lecture_07_predicatelogic_p007.jpg]]

Use the figure as the world $M=(D,I)$. How many objects does the domain $D$ have? Keep $D$ and the brother relation, but change $I$ so that the constant $\mathit{Richard}$ refers to the crown. Is $\mathit{Brother}(\mathit{Richard},\mathit{John})$ true in this new world?

- A) 5 objects. Still true.
- B) 5 objects. Now false.
- C) 2 objects. Now false.
- D) 2 objects. Still true.

> [!success]- Answer
> **B.** The domain has Richard, John, the crown and the two left legs: 5 objects.
> With the new $I$ the sentence talks about the pair (crown, King John). No brother arrow starts at the crown, so the pair is not in the relation: false.
> - A: The count is right, but truth depends on what the symbol refers to, not on its name.
> - C: The legs and the crown are objects too, not only the two persons.
> - D: Both parts are wrong: the count misses three objects, and the name $\mathit{Richard}$ alone does not make the sentence true.

### Q8 (PredicateLogic, slide 8)

A vocabulary has one unary predicate $P$, one binary predicate $R$ and one constant $C$. How many worlds are there with exactly 3 domain elements?

- A) $1536$
- B) $4096$
- C) $32768$
- D) $12288$

> [!success]- Answer
> **D.** A $k$-ary relation is a set of $k$-tuples. With $n=3$ there are $3^k$ tuples, each in or out.
> $P$: $2^{3}=8$. $R$: $2^{3^2}=2^9=512$. $C$: $3$ choices of object.
> $8\cdot512\cdot3=12\,288$.
> - A: This uses $2^{2n}=2^6=64$ for $R$ instead of $2^{n^2}$: $8\cdot64\cdot3=1536$.
> - B: This forgets the constant: $8\cdot512=4096$.
> - C: This gives $C$ $2^n=8$ choices, as if it were a relation: $8\cdot512\cdot8=32\,768$. A constant refers to exactly one of the $3$ objects.

## 4. Quantifiers (slides 9-11)

### Q9 (PredicateLogic, slide 9)

Sentence: $\forall x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$, domain $\{j,r,u\}$. World 1: the objects at UMD are $\{j\}$, the smart objects are $\{j,r\}$. World 2: nobody is at UMD and nobody is smart. Is the sentence true in world 1, and in world 2?

- A) World 1: false. World 2: false.
- B) World 1: true. World 2: false.
- C) World 1: true. World 2: true.
- D) World 1: false. World 2: true.

> [!success]- Answer
> **C.** World 1: $j$ gives true $\Rightarrow$ true, $r$ gives false $\Rightarrow$ true, $u$ gives false $\Rightarrow$ false. All three are true.
> World 2: every premise is false, so every implication is true (vacuous truth).
> - A: This uses $\land$ instead of $\Rightarrow$. Then $r$ (not at UMD) already makes world 1 false.
> - B: This assumes a universal rule needs at least one object that meets the premise. It does not.
> - D: This reverses the implication to $\mathit{Smart}(x)\Rightarrow\mathit{At}(x,\mathit{UMD})$. Then $r$ (smart, not at UMD) makes world 1 false.

### Q10 (PredicateLogic, slide 10)

Domain $\{j,r,u\}$. Nobody is at UMD. The smart objects are $\{j\}$. Sentence (a) is $\exists x\ \mathit{At}(x,\mathit{UMD})\land\mathit{Smart}(x)$. Sentence (b) is $\exists x\ \mathit{At}(x,\mathit{UMD})\Rightarrow\mathit{Smart}(x)$. What are their truth values?

- A) (a) false, (b) true.
- B) (a) true, (b) true.
- C) (a) false, (b) false.
- D) (a) true, (b) false.

> [!success]- Answer
> **A.** (a) is a disjunction of three conjunctions. $\mathit{At}(x,\mathit{UMD})$ is false for all three objects, so every conjunction is false.
> (b) For $x=r$: false $\Rightarrow$ false is true. One true instance is enough.
> So only (a) says "someone at UMD is smart". (b) is true even though UMD is empty.
> - B: This checks only $\mathit{Smart}(x)$ and ignores $\mathit{At}$. Then $j$ seems to make (a) true.
> - C: This assumes an implication needs a true premise to be true. False $\Rightarrow$ anything is true.
> - D: This swaps the two results.

### Q11 (PredicateLogic, slide 11, order)

Domain $\{a,b,c\}$, $\mathit{Loves}=\{(a,b),(b,c),(c,a)\}$. Sentence 1 is $\forall y\,\exists x\ \mathit{Loves}(x,y)$. Sentence 2 is $\exists x\,\forall y\ \mathit{Loves}(x,y)$. Which option gives their truth values here and the implication that holds in every world?

- A) 1 true, 2 false. Sentence 1 always implies sentence 2.
- B) 1 false, 2 true. Sentence 2 always implies sentence 1.
- C) 1 false, 2 true. Sentence 1 always implies sentence 2.
- D) 1 true, 2 false. Sentence 2 always implies sentence 1.

> [!success]- Answer
> **D.** Sentence 1: $b$ is loved by $a$, $c$ by $b$, $a$ by $c$. True.
> Sentence 2: each object loves only one other object, so no one loves all three. False.
> If one $x$ loves everyone, then everyone is loved by someone (that $x$). So 2 implies 1. This world shows that 1 does not imply 2.
> - A: The values are right, but this world is a counterexample to "1 implies 2".
> - B: This swaps the quantifier order when evaluating: each sentence gets the value of the other.
> - C: This swaps the quantifier order, and the implication direction too.

### Q12 (PredicateLogic, slide 11, duality)

Which sentence is equivalent to $\neg\forall x\,\exists y\ \mathit{Loves}(x,y)$ ("not everyone loves someone"), with the $\neg$ directly in front of $\mathit{Loves}$?

- A) $\forall x\,\exists y\ \neg\mathit{Loves}(x,y)$
- B) $\exists x\,\forall y\ \neg\mathit{Loves}(x,y)$
- C) $\exists x\,\exists y\ \neg\mathit{Loves}(x,y)$
- D) $\forall x\,\forall y\ \neg\mathit{Loves}(x,y)$

> [!success]- Answer
> **B.** Move $\neg$ inwards. Each quantifier it passes flips:
> $\neg\forall x\,\exists y\ L\equiv\exists x\,\neg\exists y\ L\equiv\exists x\,\forall y\ \neg L$.
> In English: there is someone who loves nobody.
> - A: This moves $\neg$ past both quantifiers without flipping them. It says everyone fails to love someone.
> - C: This flips only $\forall x$ and forgets to flip $\exists y$.
> - D: This uses $\neg\forall=\forall\neg$. It says nobody loves anybody, which is much stronger.

## 5. Translating English, equality (slides 12-13)

### Q13 (PredicateLogic, slide 12)

$\mathit{Parent}(p,c)$ means "$p$ is a parent of $c$". $\mathit{Grandparent}(x,y)$ means "$x$ is a grandparent of $y$". Which sentence says "a grandparent is a parent of one's parent"?

- A) $\forall x,y\ \mathit{Grandparent}(x,y)\Leftrightarrow\forall z\ \mathit{Parent}(x,z)\land\mathit{Parent}(z,y)$
- B) $\forall x,y\ \mathit{Grandparent}(x,y)\Leftrightarrow\exists z\ \mathit{Parent}(x,z)\land\mathit{Parent}(z,y)$
- C) $\forall x,y\ \mathit{Grandparent}(x,y)\Leftrightarrow\exists z\ \mathit{Parent}(x,z)\Rightarrow\mathit{Parent}(z,y)$
- D) $\forall x,y\ \mathit{Grandparent}(x,y)\Leftrightarrow\exists z\ \mathit{Parent}(z,x)\land\mathit{Parent}(y,z)$

> [!success]- Answer
> **B.** It is a definition, so $\Leftrightarrow$. The person in the middle is "some $z$" with $x$ a parent of $z$ and $z$ a parent of $y$: $\exists$ with $\land$.
> Check in the world with domain $\{g,p,c,o\}$ and $\mathit{Parent}=\{(g,p),(p,c),(g,o)\}$: only $\mathit{Grandparent}(g,c)$ should hold, and B gives exactly that.
> - A: $\forall z$ asks every object to be a child of $x$ and a parent of $y$. In the check world nobody is a grandparent.
> - C: $\exists$ with $\Rightarrow$ is true as soon as some $z$ is not a child of $x$. In the check world all 16 pairs become grandparents.
> - D: The arguments are swapped. This says $y$ is a grandparent of $x$. It gives $(c,g)$ instead of $(g,c)$.

### Q14 (PredicateLogic, slide 13)

Definition: $\forall x,y\ \mathit{Sibling}(x,y)\Leftrightarrow[\neg(x=y)\land\exists m\,\exists f\ \neg(m=f)\land\mathit{Parent}(m,x)\land\mathit{Parent}(f,x)\land\mathit{Parent}(m,y)\land\mathit{Parent}(f,y)]$. World: domain $\{m,f,k_1,k_2\}$, $\mathit{Parent}=\{(m,k_1),(f,k_1),(m,k_2)\}$. Which statement is right?

- A) $\mathit{Sibling}(k_1,k_2)$ is false, but it becomes true if $\neg(m=f)$ is removed.
- B) $\mathit{Sibling}(k_1,k_2)$ is true, because $k_1$ and $k_2$ share the parent $m$.
- C) $\mathit{Sibling}(k_1,k_2)$ is false, and it stays false if $\neg(m=f)$ is removed.
- D) $\mathit{Sibling}(k_1,k_1)$ is true, because $k_1$ has two different parents.

> [!success]- Answer
> **A.** We need two different objects that are parents of both $k_1$ and $k_2$. $k_2$ has only the parent $m$, so no pair works: false.
> Without $\neg(m=f)$ both variables may refer to the object $m$. Then all four $\mathit{Parent}$ facts hold, and the sentence becomes true.
> - B: One shared parent is not enough. The definition asks for two different shared parents.
> - C: Two different variables do not mean two different objects. Only $\neg(m=f)$ says that.
> - D: $\neg(x=y)$ is false for $x=y=k_1$, so the right side is false.

## 6. Substitutions, numbers, sets, lists (slides 14-16)

### Q15 (PredicateLogic, slide 14)

$S=P(x,f(y),z)$ and $\sigma=\{x=y,\ y=g(z),\ z=A\}$. The bindings are applied simultaneously. What is $S\sigma$?

- A) $P(g(A),f(g(A)),A)$
- B) $P(y,f(g(A)),A)$
- C) $P(g(z),f(g(z)),A)$
- D) $P(y,f(g(z)),A)$

> [!success]- Answer
> **D.** Replace every variable of the original $S$ once: $x\to y$, $y\to g(z)$, $z\to A$. This gives $P(y,f(g(z)),A)$.
> The new $y$ and $z$ came in through the bindings, so they are not replaced again. $S\sigma$ is not ground.
> - A: This applies the bindings one after another, from left to right. Every new variable is replaced again.
> - B: This replaces the new $z$ inside $g(z)$ a second time.
> - C: This replaces the new $y$ in the first argument a second time.

### Q16 (PredicateLogic, slide 15)

Axioms: (1) $+(0,m)=m$, (2) $+(S(m),n)=S(+(m,n))$. You compute $2+1$, that is $+(S(S(0)),S(0))$, by applying one axiom per step. What is the term after the second step?

- A) $S(+(S(0),S(0)))$
- B) $S(S(+(0,S(0))))$
- C) $S(S(S(0)))$
- D) $+(0,S(S(S(0))))$

> [!success]- Answer
> **B.** Step 1, axiom 2 with $m=S(0)$, $n=S(0)$: $S(+(S(0),S(0)))$.
> Step 2, axiom 2 with $m=0$, $n=S(0)$: $S(S(+(0,S(0))))$.
> Step 3 (axiom 1) then gives $S(S(S(0)))$, the number 3.
> - A: This is the term after step 1.
> - C: This is the term after step 3.
> - D: This uses a wrong axiom $+(S(m),n)=+(m,S(n))$ twice. It moves the $S$ into the second argument instead of outside.

### Q17 (PredicateLogic, slide 16)

Lists: $[A,B,C]=\mathit{Cons}(A,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil})))$. Rule 1: $\forall x,l\ \mathit{Member}(x,\mathit{Cons}(x,l))$. Rule 2: $\forall x,y,l\ \mathit{Member}(x,l)\Rightarrow\mathit{Member}(x,\mathit{Cons}(y,l))$. Rule 1 gives $\mathit{Member}(B,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil})))$. Which substitution in rule 2 then gives $\mathit{Member}(B,[A,B,C])$?

- A) $\{x=A,\ y=B,\ l=\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))\}$
- B) $\{x=B,\ y=A,\ l=\mathit{Cons}(C,\mathit{Nil})\}$
- C) $\{x=B,\ y=A,\ l=\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))\}$
- D) $\{x=A,\ y=B,\ l=\mathit{Cons}(C,\mathit{Nil})\}$

> [!success]- Answer
> **C.** The conclusion $\mathit{Member}(x,\mathit{Cons}(y,l))$ must equal $\mathit{Member}(B,\mathit{Cons}(A,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))))$. So $x=B$, $y=A$, $l=\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil}))$.
> The premise is then $\mathit{Member}(B,\mathit{Cons}(B,\mathit{Cons}(C,\mathit{Nil})))$, the fact from rule 1.
> - A: This swaps $x$ and $y$. The conclusion is about $A$, and the premise $\mathit{Member}(A,[B,C])$ is not known.
> - B: $l$ is one element too short. The conclusion is $\mathit{Member}(B,[A,C])$, and the premise $\mathit{Member}(B,[C])$ is not known.
> - D: Both slips: $x$ and $y$ swapped and $l$ too short. The conclusion is $\mathit{Member}(A,[B,C])$.

## 7. Using a FOL knowledge base (slides 17-20)

### Q18 (PredicateLogic, slide 17)

A KB contains only $\mathit{Brother}(\mathit{Richard},\mathit{John})$, $\mathit{Brother}(\mathit{John},\mathit{Richard})$ and $\mathit{King}(\mathit{John})$. `Ask(KB, S)` returns the substitutions $\sigma$ with $KB\models S\sigma$. What does `Ask(KB,` $\mathit{Brother}(x,y)\land\mathit{King}(y)$`)` return?

- A) $\{x=\mathit{Richard},\ y=\mathit{John}\}$
- B) $\{x=\mathit{John},\ y=\mathit{Richard}\}$
- C) Both $\{x=\mathit{Richard},\ y=\mathit{John}\}$ and $\{x=\mathit{John},\ y=\mathit{Richard}\}$
- D) No substitution, because the query has free variables

> [!success]- Answer
> **A.** $y$ must be a king, and only $\mathit{King}(\mathit{John})$ is in the KB. So $y=\mathit{John}$, and $\mathit{Brother}(\mathit{Richard},\mathit{John})$ gives $x=\mathit{Richard}$.
> - B: This makes Richard the king. $\mathit{King}(\mathit{Richard})$ is not entailed.
> - C: This checks only the $\mathit{Brother}$ part and forgets $\mathit{King}(y)$.
> - D: Ask exists to bind the variables of a query. The binding list tells the agent which object makes the query true.

### Q19 (PredicateLogic, slide 18, percept)

Rules: $\forall t,s,b,m,c\ \mathit{Percept}([s,b,\mathit{Glitter},m,c],t)\Rightarrow\mathit{Glitter}(t)$ and $\forall t\ \mathit{Glitter}(t)\Rightarrow\mathit{BestAction}(\mathit{Grab},t)$. The agent tells the KB $\mathit{Percept}([\mathit{Stench},\mathit{Breeze},\mathit{Glitter},\mathit{None},\mathit{None}],5)$. Which substitution makes the first rule fire, and what does the KB then entail?

- A) $\{s=\mathit{Stench},b=\mathit{Breeze},m=\mathit{None},c=\mathit{None},t=5\}$, then $\mathit{Action}(\mathit{Grab},5)$
- B) $\{s=\mathit{Stench},b=\mathit{Breeze},m=\mathit{None},c=\mathit{None},t=5\}$, then $\mathit{BestAction}(\mathit{Grab},5)$
- C) $\{s=\mathit{Stench},b=\mathit{Breeze},m=\mathit{Glitter},c=\mathit{None},t=5\}$, then $\mathit{BestAction}(\mathit{Grab},5)$
- D) $\{s=\mathit{Stench},b=\mathit{Breeze},m=\mathit{Glitter},c=\mathit{None},t=5\}$, then $\mathit{Action}(\mathit{Grab},5)$

> [!success]- Answer
> **B.** Match position by position. The third place is the constant $\mathit{Glitter}$ in both, so it needs no binding. $m$ and $c$ are in places 4 and 5: both $\mathit{None}$.
> The rule gives $\mathit{Glitter}(5)$. The reflex rule with $t=5$ gives $\mathit{BestAction}(\mathit{Grab},5)$.
> - A: The match is right, but the rules conclude $\mathit{BestAction}$, not $\mathit{Action}$ (the slide's own mismatch).
> - C: This skips the constant slot and gives the values to the variables in order, so $m$ gets the third value.
> - D: Both slips: the constant slot skipped and $\mathit{Action}$ instead of $\mathit{BestAction}$.

### Q20 (PredicateLogic, slide 18, arrow)

Successor state axiom: $\forall t\ \mathit{HaveArrow}(t+1)\Leftrightarrow[\mathit{HaveArrow}(t)\land\neg\mathit{Action}(\mathit{Shoot},t)]$. $\mathit{HaveArrow}(3)$ is true. The agent shoots at $t=3$ and does not shoot at $t=4$ or $t=5$. What are $\mathit{HaveArrow}(4)$ and $\mathit{HaveArrow}(5)$?

- A) $\mathit{HaveArrow}(4)$ true, $\mathit{HaveArrow}(5)$ true
- B) $\mathit{HaveArrow}(4)$ false, $\mathit{HaveArrow}(5)$ true
- C) $\mathit{HaveArrow}(4)$ false, $\mathit{HaveArrow}(5)$ false
- D) $\mathit{HaveArrow}(4)$ true, $\mathit{HaveArrow}(5)$ false

> [!success]- Answer
> **C.** $t=3$: true $\land\neg$true = false, so $\mathit{HaveArrow}(4)$ is false.
> $t=4$: false $\land\neg$false = false, so $\mathit{HaveArrow}(5)$ is false. A lost arrow never comes back.
> - A: This is off by one: it uses the action at $t+1$ instead of $t$, so the shot at $t=3$ is never counted.
> - B: This drops $\mathit{HaveArrow}(t)$ from the right side. Then not shooting at $t=4$ seems to bring the arrow back.
> - D: This forgets the $\neg$ in front of $\mathit{Action}$.

### Q21 (PredicateLogic, slide 19)

Axioms: $\mathit{Adjacent}([x,y],[a,b])\Leftrightarrow(x=a\land(y=b-1\lor y=b+1))\lor(y=b\land(x=a-1\lor x=a+1))$, and $\forall s\ \mathit{Breezy}(s)\Leftrightarrow\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)$. The grid is $4\times4$ with squares $[1,1]$ to $[4,4]$. The agent knows $\neg\mathit{Breezy}([1,1])$. What follows, and which direction of the $\Leftrightarrow$ is used?

- A) No pit in $[1,2]$, $[2,1]$, $[2,2]$. Direction $\mathit{Breezy}(s)\Rightarrow\exists r\dots$
- B) No pit in $[1,2]$, $[2,1]$. Direction $\mathit{Breezy}(s)\Rightarrow\exists r\dots$
- C) No pit in $[1,2]$, $[2,1]$, $[2,2]$. Direction $\exists r\dots\Rightarrow\mathit{Breezy}(s)$
- D) No pit in $[1,2]$, $[2,1]$. Direction $\exists r\dots\Rightarrow\mathit{Breezy}(s)$

> [!success]- Answer
> **D.** The direction $[\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)]\Rightarrow\mathit{Breezy}(s)$, used as its contrapositive: not breezy, so no adjacent square has a pit.
> Adjacent to $[1,1]$ inside the grid: $[1,2]$ and $[2,1]$. $[2,2]$ differs in both coordinates, so it is not adjacent.
> - A: Diagonal squares are not adjacent, and the other direction says nothing when $\mathit{Breezy}$ is false.
> - B: The squares are right, but $\mathit{Breezy}(s)\Rightarrow\dots$ has a false premise here, so it gives nothing.
> - C: The direction is right, but $[2,2]$ is diagonal to $[1,1]$, not adjacent.

### Q22 (PredicateLogic, slide 20)

In a $4\times4$ wumpus world, how many propositional sentences like $B_{1,1}\Leftrightarrow(P_{1,2}\lor P_{2,1})$ are needed to say "a square is breezy exactly when an adjacent square has a pit"? How many FOL sentences (not counting the definition of $\mathit{Adjacent}$)?

- A) 16 propositional, 1 FOL
- B) 16 propositional, 16 FOL
- C) 24 propositional, 1 FOL
- D) 24 propositional, 16 FOL

> [!success]- Answer
> **A.** Propositional logic has no variables for squares, so it needs one sentence per square: $4\cdot4=16$.
> FOL needs one: $\forall s\ \mathit{Breezy}(s)\Leftrightarrow\exists r\ \mathit{Adjacent}(r,s)\land\mathit{Pit}(r)$. It does not grow with the grid.
> - B: The variable $s$ covers every square, so FOL needs no sentence per square.
> - C: 24 is the number of pairs of adjacent squares. The sentences are per square, not per pair.
> - D: Both slips: one sentence per adjacent pair, and one FOL sentence per square.

## Part B. Deterministic planning (deck DeterministicPlanning)

## 8. Blocks world and states (slides 1-5)

### Q23 (DeterministicPlanning, slide 1)

Slide 1 compares the search algorithms of Chapter 3 with the logical agent of Chapter 7. What does classical planning take from logic, and what does it take from search?

- A) From logic: general inference rules for each step. From search: one hand-written heuristic per domain.
- B) From logic: one fixed language for states and actions. From search: exploring states with algorithms like A*.
- C) From logic: one fixed language for states and actions. From search: one hand-written heuristic per domain.
- D) From logic: general inference rules for each step. From search: exploring states with algorithms like A*.

> [!success]- Answer
> **B.** Logic gives a standard representation: states and actions are written in one fixed language. Search gives the idea of exploring the state space with domain-independent algorithms.
> Because the representation is always the same, even the heuristics can be domain-independent.
> - A: Both parts are the weak points. Logical inference was too slow (huge numbers of rules), and a hand-written heuristic per domain is what planning wants to avoid.
> - C: The first part is right, but a hand-written heuristic per domain is the weakness of Chapter 3 search, not what planning keeps.
> - D: The second part is right, but planning does not take the slow general inference. It takes the fixed language.

### Q24 (DeterministicPlanning, slide 2)

Slide 2 says that $n$ blocks give more than $n!$ states. Count the physically possible states for $n=3$ blocks a, b, c. Positions on the table are ignored. The hand is empty or holds one block.

- A) 13
- B) 6
- C) 19
- D) 22

> [!success]- Answer
> **D.** Hand empty: all on the table 1, one tower of two plus a single block $3\cdot2=6$, one tower of three $3!=6$. That is $13$.
> Hand holds a block: 3 choices. The other two are both on the table, or one is on the other (2 ways): 3 arrangements. $3\cdot3=9$.
> Total $13+9=22>3!=6$.
> - A: This counts only the states with an empty hand.
> - B: This counts only the towers of three, which is $3!$.
> - C: This counts $3\cdot2=6$ states with a held block. It forgets the case where the other two blocks are both on the table.

### Q25 (DeterministicPlanning, slide 3)

The planning language has no function symbols but unlimited variable symbols. Why do infinitely many variables not make the number of states infinite?

- A) A state holds only ground atoms, and these are built from the finitely many predicates and constants.
- B) The number of variables used is bounded by the number of blocks, so only finitely many atoms exist.
- C) A search with loop-checking visits each state at most once, so the number of states stays finite.
- D) Each variable can only take values from the finitely many function symbols of the language.

> [!success]- Answer
> **A.** A state is a set of ground atoms. A ground atom has no variables. It is built from predicates and constants only, and both sets are finite. Variables appear only in operators and are replaced by constants before an action is executed.
> - B: The language allows unlimited variables. The reason is that states contain no variables at all.
> - C: This turns the order around. Loop-checking ends the search because the number of states is finite, not the other way round.
> - D: The language has no function symbols. Variables are replaced by constants.

### Q26 (DeterministicPlanning, slide 4)

The constant symbols are the blocks a to e. Which of (a) `on(c,a)`, (b) `on(c,x)`, (c) `handempty`, (d) $\neg$`clear(a)` are ground atoms? How many ground instances does `on(c,x)` have (count every constant, also c)?

- A) (a), (c) and (d). 5 ground instances.
- B) (a), (c) and (d). 4 ground instances.
- C) (a) and (c). 5 ground instances.
- D) (a) and (c). 4 ground instances.

> [!success]- Answer
> **C.** (a) is an atom without variables: ground. (b) has the variable $x$: not ground. (c) is a predicate with zero arguments: a ground atom. (d) is a negative literal, not an atom.
> `on(c,x)` has one instance per constant: `on(c,a)`, ..., `on(c,e)`, so 5. `on(c,c)` is a correct ground atom. It is only never true in a real situation.
> - A: (d) is the negation of the ground atom `clear(a)`. A negated atom is a literal, not an atom.
> - B: Two slips: (d) is not an atom, and `on(c,c)` was dropped although it belongs to the language.
> - D: The atoms are right, but `on(c,c)` is a ground instance too.

### Q27 (DeterministicPlanning, slide 5)

Three blocks: b is on a, a and c are on the table, the hand is empty. Which set is the state by the definition of slide 5 (a set of the ground atoms that are true)?

- A) $\{$`ontable(a)`, `on(b,a)`, `clear(a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$
- B) $\{$`on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$
- C) $\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$
- D) $\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`$\}$

> [!success]- Answer
> **C.** a: `ontable(a)`, not clear because b is on it. b: `on(b,a)`, `clear(b)`. c: `ontable(c)`, `clear(c)`. Hand: `handempty`.
> `clear(a)` is not in the set, so it is false.
> - A: `clear(a)` is false: b is on a.
> - B: `ontable(a)` is missing. A block under another block still stands on the table.
> - D: `handempty` is missing. The hand is empty, so this atom is true and must be in the set.

## 9. Operators and actions (slides 6-11)

### Q28 (DeterministicPlanning, slide 6)

Look at this operator. Head `lift(x)`. Precond: `on(x,y)`, `clear(x)`, `handempty`. Effects: $\neg$`on(x,y)`, `holding(x)`, `clear(y)`. Which rule of slide 6 does it break?

- A) The head must list all variables. With $y$ missing, `lift(c)` does not say which block c leaves.
- B) An effect may only add atoms that are in the preconditions. Here `holding(x)` is a new atom.
- C) Preconditions may only contain ground atoms. Here `on(x,y)` contains two variables.
- D) Each variable may appear in only one precondition. Here $x$ appears in two preconditions.

> [!success]- Answer
> **A.** Slide 6: the parameter list must include all of the operator's variables. An action is made by replacing the parameters by constants. Without $y$, the preconditions and effects of `lift(c)` are not fixed. The correct head is `lift(x,y)`, the `unstack(x,y)` of slide 7.
> - B: No such rule. Most effects add atoms that were false before; that is what actions do.
> - C: This mixes up operators and actions. An operator has variables; only an action (a ground instance) is ground.
> - D: No such rule. `unstack(x,y)` also uses $x$ in two preconditions.

### Q29 (DeterministicPlanning, slide 7)

The operators are `unstack(x,y)`, `stack(x,y)`, `pickup(x)`, `putdown(x)`. There are 5 blocks. How many ground actions do the four operators have, if every parameter can be any of the 5 constants (also $x=y$)?

- A) 20
- B) 50
- C) 30
- D) 60

> [!success]- Answer
> **D.** Two parameters: $5\cdot5=25$ each for `unstack` and `stack`. One parameter: 5 each for `pickup` and `putdown`. $25+25+5+5=60$.
> - A: This gives the two-parameter operators only 5 instances each: $5+5+5+5$.
> - B: This drops the 10 actions with $x=y$, like `unstack(a,a)`. The question counts them.
> - C: This counts unordered pairs, $\binom52=10$, for `stack` and `unstack`. But `stack(a,b)` and `stack(b,a)` are different actions.

### Q30 (DeterministicPlanning, slide 8)

Operator `stack(x,y)`: Precond `holding(x)`, `clear(y)`. Effects $\neg$`holding(x)`, $\neg$`clear(y)`, `on(x,y)`, `clear(x)`, `handempty`. Which is the action `stack(b,c)`?

- A) Precond `holding(c)`, `clear(b)`. Effects $\neg$`holding(c)`, $\neg$`clear(b)`, `on(c,b)`, `clear(c)`, `handempty`.
- B) Precond `holding(b)`, `clear(c)`. Effects $\neg$`holding(b)`, $\neg$`clear(c)`, `on(b,c)`, `clear(b)`, `handempty`.
- C) Precond `holding(b)`, `clear(c)`. Effects $\neg$`holding(c)`, $\neg$`clear(b)`, `on(c,b)`, `clear(c)`, `handempty`.
- D) Precond `holding(c)`, `clear(b)`. Effects $\neg$`holding(b)`, $\neg$`clear(c)`, `on(b,c)`, `clear(b)`, `handempty`.

> [!success]- Answer
> **B.** The substitution is $\{x=\text{b},\ y=\text{c}\}$. Every $x$ becomes b and every $y$ becomes c, in the preconditions and in the effects.
> - A: This uses $\{x=\text{c},\ y=\text{b}\}$ everywhere. That is the action `stack(c,b)`.
> - C: The preconditions are right, but the effects use the swapped substitution.
> - D: The effects are right, but the preconditions use the swapped substitution.

### Q31 (DeterministicPlanning, slide 9)

Action `putdown(e)`: Precond `holding(e)`. Effects $\neg$`holding(e)`, `ontable(e)`, `clear(e)`, `handempty`. $S^+$ holds the atoms that appear positively in $S$, $S^-$ those that appear negatively. What are $\text{precond}^-$ and $\text{effects}^-$?

- A) $\text{precond}^-=\emptyset$, $\text{effects}^-=\{$`holding(e)`$\}$
- B) $\text{precond}^-=\{$`holding(e)`$\}$, $\text{effects}^-=\{$`holding(e)`$\}$
- C) $\text{precond}^-=\emptyset$, $\text{effects}^-=\{$`ontable(e)`, `clear(e)`, `handempty`$\}$
- D) $\text{precond}^-=\{$`holding(e)`$\}$, $\text{effects}^-=\emptyset$

> [!success]- Answer
> **A.** The only precondition `holding(e)` is positive, so $\text{precond}^-=\emptyset$. The only negative effect is $\neg$`holding(e)`, so $\text{effects}^-=\{$`holding(e)`$\}$ (the set holds the atom without the $\neg$).
> - B: `holding(e)` is a positive precondition. Its negation is an effect, not a precondition.
> - C: This is $\text{effects}^+$. The positive and negative effects are swapped.
> - D: $\neg$`holding(e)` was put into $\text{precond}^-$ instead of $\text{effects}^-$. It is an effect: the action makes `holding(e)` false.

### Q32 (DeterministicPlanning, slide 10)

$s=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. Preconditions: `unstack(x,y)`: `on(x,y)`, `clear(x)`, `handempty`. `pickup(x)`: `ontable(x)`, `clear(x)`, `handempty`. `stack(x,y)`: `holding(x)`, `clear(y)`. Which statement is correct?

- A) `pickup(a)` is executable in $s$: `ontable(a)` and `handempty` are in $s$.
- B) `stack(c,b)` is executable in $s$: `clear(b)` and `clear(c)` are in $s$.
- C) `unstack(b,a)` is executable in $s$: `on(b,a)`, `clear(b)`, `handempty` are in $s$.
- D) `unstack(b,a)` is not executable in $s$: `clear(a)` is not in $s$.

> [!success]- Answer
> **C.** All preconditions are positive, so the test is $\text{precond}^+(a)\subseteq s$. `on(b,a)`, `clear(b)` and `handempty` are all in $s$.
> - A: The precondition `clear(a)` was not checked. It is not in $s$ (b is on a), so `pickup(a)` is not executable.
> - B: The precondition `holding(c)` was not checked. The hand is empty, so `stack(c,b)` is not executable.
> - D: `unstack(b,a)` does not need `clear(a)`. It needs b to be clear, and it makes a clear.

### Q33 (DeterministicPlanning, slide 11)

$s=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. `pickup(x)`: Precond `ontable(x)`, `clear(x)`, `handempty`; Effects $\neg$`ontable(x)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)`. `stack(x,y)`: Precond `holding(x)`, `clear(y)`; Effects $\neg$`holding(x)`, $\neg$`clear(y)`, `on(x,y)`, `clear(x)`, `handempty`. With $\gamma(s,a)=(s-\text{effects}^-(a))\cup\text{effects}^+(a)$, what is $s_2=\gamma(\gamma(s,$`pickup(c)`$),$`stack(c,b)`$)$?

- A) $\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `holding(c)`$\}$
- B) $\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(b)`, `clear(c)`, `handempty`$\}$
- C) $\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(c)`, `holding(c)`, `handempty`$\}$
- D) $\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(c)`, `handempty`$\}$

> [!success]- Answer
> **D.** `pickup(c)`: delete `ontable(c)`, `clear(c)`, `handempty`, add `holding(c)`. $s_1=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `holding(c)`$\}$.
> `stack(c,b)` is executable in $s_1$. Delete `holding(c)`, `clear(b)`. Add `on(c,b)`, `clear(c)`, `handempty`. Result: the tower c on b on a.
> - A: In `stack(c,b)` the add and delete lists are swapped. Nothing to delete is in $s_1$, so $s_1$ comes back unchanged.
> - B: `clear(b)` was not deleted. c now sits on b. (Slide 11 prints this same slip.)
> - C: `holding(c)` was not deleted. The hand cannot hold c and be empty.

## 10. Plans, problems, complexity (slides 12-16)

### Q34 (DeterministicPlanning, slide 12)

$s_0=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. Operators `pickup(x)` and `stack(x,y)` as in Q33 (`pickup` needs `ontable(x)`, `clear(x)`, `handempty`; `stack` needs `holding(x)`, `clear(y)`). Is $\pi=($`pickup(c)`, `stack(c,b)`, `pickup(a)`$)$ executable in $s_0$?

- A) Yes. $\gamma(s_0,\pi)=\{$`on(b,a)`, `on(c,b)`, `clear(c)`, `holding(a)`$\}$.
- B) No. `pickup(a)` needs `clear(a)`, so $\gamma(s_0,\pi)$ is not defined.
- C) No. `pickup(c)` deletes `clear(b)`, so `stack(c,b)` cannot be executed.
- D) Yes. The last action changes nothing, so $\gamma(s_0,\pi)$ is the tower c, b, a.

> [!success]- Answer
> **B.** After `pickup(c)` and `stack(c,b)` the state is $\{$`ontable(a)`, `on(b,a)`, `on(c,b)`, `clear(c)`, `handempty`$\}$. `pickup(a)` needs `clear(a)`, which is not there. One action fails, so the plan is not executable and $\gamma(s_0,\pi)$ is undefined.
> - A: This applies the formula to `pickup(a)` without the executability test.
> - C: `pickup(c)` deletes `ontable(c)`, `clear(c)`, `handempty`. It does not touch `clear(b)`.
> - D: An action that is not executable is not a "do nothing" step. It makes the whole plan not executable.

### Q35 (DeterministicPlanning, slide 13)

$s_0=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `handempty`$\}$. Slide 13 executes `unstack(c,a)`, `putdown(c)`, `pickup(b)`, `stack(b,a)`. After `unstack(c,a)`: $s_1=\{$`ontable(a)`, `clear(a)`, `ontable(b)`, `clear(b)`, `holding(c)`$\}$. `pickup(x)` needs `ontable(x)`, `clear(x)`, `handempty`. What happens if `putdown(c)` and `pickup(b)` change places?

- A) `pickup(b)` is executable in $s_1$, but then `stack(b,a)` fails because a is not clear.
- B) `pickup(b)` is not executable in $s_1$: block b is not clear after `unstack(c,a)`.
- C) The new order is executable and gives the same final state as the plan of the slide.
- D) `pickup(b)` is not executable in $s_1$: the hand holds c, and `handempty` is missing.

> [!success]- Answer
> **D.** After the swap, `pickup(b)` comes right after `unstack(c,a)`, in $s_1$. It needs `handempty`, but $s_1$ has `holding(c)` instead. Not executable: the same actions in another order are not an executable plan.
> - A: `pickup(b)` already fails, and in $s_1$ block a is clear anyway.
> - B: `clear(b)` is in $s_1$. Nothing was ever on b.
> - C: The order matters, because the effects of one action are the preconditions of the next.

### Q36 (DeterministicPlanning, slide 14)

$s_4=\{$`ontable(a)`, `on(b,a)`, `clear(b)`, `ontable(c)`, `clear(c)`, `handempty`$\}$. A goal is a set of literals. Is $s_4$ a goal state for $g_1=\{$`on(b,a)`, `clear(c)`$\}$? For $g_2=\{$`on(b,a)`, $\neg$`handempty`$\}$?

- A) $g_1$: yes. $g_2$: no.
- B) $g_1$: no. $g_2$: no.
- C) $g_1$: yes. $g_2$: yes.
- D) $g_1$: no. $g_2$: yes.

> [!success]- Answer
> **A.** $g_1$: both atoms are in $s_4$, so $s_4$ satisfies $g_1$. A goal need not describe the whole state.
> $g_2$: $\neg$`handempty` asks that `handempty` is not in the state. It is in $s_4$, so $s_4$ does not satisfy $g_2$.
> - B: $g_2$ is right, but $g_1$ does not have to list the other atoms of $s_4$.
> - C: $g_1$ is right, but the negative literal $\neg$`handempty` was ignored.
> - D: Both parts are wrong. $g_1$ does not have to list every atom, and the negative literal of $g_2$ fails.

### Q37 (DeterministicPlanning, slide 15)

$s_0=\{$`ontable(a)`, `on(c,a)`, `clear(c)`, `ontable(b)`, `clear(b)`, `handempty`$\}$, $g=\{$`on(a,b)`$\}$. `unstack(x,y)`: Precond `on(x,y)`, `clear(x)`, `handempty`; Effects $\neg$`on(x,y)`, $\neg$`clear(x)`, $\neg$`handempty`, `holding(x)`, `clear(y)`. `pickup` and `stack` as in Q33. Is $\pi'=($`unstack(c,a)`, `stack(c,b)`, `pickup(a)`, `stack(a,c)`$)$ a solution?

- A) No. `pickup(a)` is not executable after `stack(c,b)`.
- B) Yes. It is executable, and a ends up above b in one tower.
- C) No. It is executable, but the final state has `on(a,c)`, not `on(a,b)`.
- D) No. `stack(a,c)` is not executable, because c is not clear.

> [!success]- Answer
> **C.** After `unstack(c,a)`, `stack(c,b)` the state has `ontable(a)`, `clear(a)`, `handempty`, so `pickup(a)` is executable. Then `stack(a,c)` needs `holding(a)` and `clear(c)`: both there.
> Final state: $\{$`ontable(b)`, `on(c,b)`, `on(a,c)`, `clear(a)`, `handempty`$\}$. It does not contain `on(a,b)`, so $\pi'$ is executable but not a solution.
> - A: `pickup(a)` finds `ontable(a)`, `clear(a)` and `handempty` in the state.
> - B: `on(a,b)` means directly on b. "Above b in the tower" is not the goal atom.
> - D: `stack(c,b)` adds `clear(c)`, so c is clear.

### Q38 (DeterministicPlanning, slide 16)

With function symbols, "does a planning problem have a solution?" is undecidable. But "is there a solution of length $k$ or less?" is decidable. Why?

- A) Only plans of at most $k$ actions must be examined, so the search can stop at depth $k$ with "no".
- B) Function symbols are removed before the search, so the state space becomes finite again.
- C) Loop-checking visits each state at most once, so the search ends after finitely many steps.
- D) For a fixed $k$ the problem is in NP, and every problem in NP has a polynomial algorithm.

> [!success]- Answer
> **A.** The bound limits the search depth. Finitely many actions apply in each state, so the plans of length at most $k$ are finitely many. After depth $k$ the answer "no" is safe.
> - B: Nothing is removed. The state space stays infinite, only the search depth is limited.
> - C: With infinitely many states the search can go on forever without repeating a state. Loop-checking does not help.
> - D: It is not known that every NP problem has a polynomial algorithm. Decidable does not mean polynomial.

## 11. Forward search (slide 17)

### Q39 (DeterministicPlanning, slide 17)

Two blocks: $s_0=\{$`ontable(a)`, `ontable(b)`, `clear(a)`, `clear(b)`, `handempty`$\}$, $g=\{$`on(a,b)`$\}$, the four blocks-world operators (`pickup` and `stack` as in Q33, `putdown` as in Q31, `unstack` as in Q37). How many states can be reached from $s_0$, and how long is the plan that breadth-first forward search finds?

- A) 3 states, a plan of 2 actions
- B) 5 states, a plan of 2 actions
- C) 5 states, a plan of 4 actions
- D) 3 states, a plan of 4 actions

> [!success]- Answer
> **B.** States with an empty hand: both on the table, a on b, b on a (3). States with a held block: holding a, holding b (2). Total 5.
> In $s_0$ only `pickup(a)` and `pickup(b)` are executable. Depth 2 from $\{$`ontable(b)`, `clear(b)`, `holding(a)`$\}$: `stack(a,b)` gives `on(a,b)`. Plan (`pickup(a)`, `stack(a,b)`).
> - A: This counts only the states with an empty hand.
> - C: The state count is right, but no block covers a here. The `unstack`, `putdown` steps of the slide-15 plan are not needed.
> - D: Both slips: the held states are forgotten and the 4-step plan of slide 15 is copied.

## 12. Graphplan and the planning graph (slides 18-32)

### Q40 (DeterministicPlanning, slide 18)

Slide 18 says that 450 blocks give more than $10^{1000}$ states. Slide 2 says $n$ blocks give more than $n!$ states. Use $\log_{10}(n!)=\sum_{i=1}^{n}\log_{10}i$. What do you get for $450!$?

- A) $450!\approx10^{135}$, so the claim fails.
- B) $450!\approx10^{998}$, so the claim fails.
- C) $450!\approx10^{1194}$, so the claim holds.
- D) $450!\approx1.7\cdot10^{1000}$, so the claim holds.

> [!success]- Answer
> **D.** $\sum_{i=1}^{450}\log_{10}i\approx1000.24$, so $450!\approx10^{1000.24}\approx1.7\cdot10^{1000}>10^{1000}$.
> - A: This is $2^{450}$ ($450\cdot\log_{10}2\approx135.5$), not $450!$.
> - B: This is Stirling's formula $(n/e)^n$ without the factor $\sqrt{2\pi n}$, which adds about $1.7$ to the exponent.
> - C: This is $450^{450}$ ($450\cdot\log_{10}450\approx1193.9$). Every factor of $n!$ is replaced by $n$.

### Q41 (DeterministicPlanning, slides 19-20)

Graphplan checks at level $k$ whether all goals are present and no two are mutex. The slides call this "a necessary (but insufficient) condition for plan existence". What does this mean for the algorithm?

- A) If the condition holds at level $k$, a $k$-level plan exists, so the extraction always succeeds.
- B) If the condition fails at level $k$, a $k$-level plan may still exist, so extraction must still run.
- C) If it fails at level $k$, no $k$-level plan exists. If it holds, extraction must run and may fail.
- D) If the condition holds at level $k$, no shorter plan exists, and extraction only prints the plan.

> [!success]- Answer
> **C.** Necessary: every $k$-level plan makes the condition true, so if it fails, Graphplan can skip the search and expand the graph. Insufficient: the condition can hold without a plan, so solution extraction must run, and it may fail.
> - A: This reads "necessary" as "sufficient". The check uses pairs only, so it can hold without a plan.
> - B: This turns "necessary" around. When the condition fails, no $k$-level plan exists.
> - D: The extraction is a real search with backtracking. It can fail, and then Graphplan expands further.

### Q42 (DeterministicPlanning, slides 21-22)

Shopping problem: $s_0=\{$money$\}$, $g=\{$food, ticket$\}$. `buyFood`: needs money, gives food, $\neg$money. `buyTicket`: needs money, gives ticket, $\neg$money. `work`: needs nothing, gives money. State-level 0 holds money, $\neg$food, $\neg$ticket. How many nodes does action-level 1 have (no-ops included), and how many literals does state-level 1 have?

- A) 3 action nodes, 6 literals
- B) 6 action nodes, 6 literals
- C) 3 action nodes, 4 literals
- D) 6 action nodes, 4 literals

> [!success]- Answer
> **B.** Real actions: `buyFood`, `buyTicket` (money is in state-level 0) and `work` (no precondition). No-ops: one for each of the 3 literals of state-level 0. $3+3=6$.
> State-level 1 = all effects: food, $\neg$money, ticket, money, and from the no-ops money, $\neg$food, $\neg$ticket. That is all 6 literals.
> - A: The no-op nodes are forgotten, but their literals are still counted in state-level 1.
> - C: Without no-ops, only food, ticket, $\neg$money, money reach state-level 1.
> - D: The no-op nodes are counted, but their effects $\neg$food and $\neg$ticket are dropped from state-level 1.

### Q43 (DeterministicPlanning, slide 23)

Shopping problem as in the previous question, action-level 1. Mutex rules for actions: inconsistent effects (an effect of one negates an effect of the other), interference (one deletes a precondition of the other), competing needs. Why is (a) `buyFood` mutex with `buyTicket`, and (b) `buyFood` mutex with `work`?

- A) (a) interference, (b) interference
- B) (a) inconsistent effects, (b) inconsistent effects
- C) (a) interference, (b) inconsistent effects
- D) (a) inconsistent effects, (b) interference

> [!success]- Answer
> **C.** (a) `buyFood` deletes money, a precondition of `buyTicket` (and the other way round): interference. Both give $\neg$money, and the same effect twice is not inconsistent.
> (b) `buyFood` gives $\neg$money, `work` gives money: inconsistent effects. `work` has no precondition, so nothing of it is deleted.
> - A: (a) is right, but `work` has no precondition that `buyFood` could delete.
> - B: (b) is right, but in (a) the two actions share the effect $\neg$money. That does not negate anything.
> - D: Both rules are mixed up: a shared effect taken as inconsistent, and an opposite effect taken as interference.

### Q44 (DeterministicPlanning, slide 24)

Dinner date: $s_0=\{$garbage, cleanHands, quiet$\}$. `cook`: needs cleanHands, gives dinner. `wrap`: needs quiet, gives present. `carry`: no precondition, gives $\neg$garbage, $\neg$cleanHands. `dolly`: no precondition, gives $\neg$garbage, $\neg$quiet. Two literals are mutex if every pair of their producers is mutex. At level 1, is `carry` mutex with `dolly`? Are the literals $\neg$garbage and dinner mutex?

![[cmpe421_lecture_07_deterministicplanning_p024.jpg]]

- A) `carry`, `dolly`: not mutex. $\neg$garbage, dinner: not mutex.
- B) `carry`, `dolly`: mutex. $\neg$garbage, dinner: not mutex.
- C) `carry`, `dolly`: mutex. $\neg$garbage, dinner: mutex.
- D) `carry`, `dolly`: not mutex. $\neg$garbage, dinner: mutex.

> [!success]- Answer
> **A.** `carry` and `dolly` both give $\neg$garbage. No effect negates another, and they have no preconditions: not mutex.
> $\neg$garbage comes from `carry` or `dolly`, dinner only from `cook`. `carry`, `cook` is mutex (interference), but `dolly`, `cook` is not. One non-mutex pair is enough: not mutex.
> - B: The literal part is right, but sharing the effect $\neg$garbage does not make two actions mutex.
> - C: A shared effect is not inconsistent, and only one producer pair of the literals is mutex.
> - D: Only the pair `carry`, `cook` was checked. The pair `dolly`, `cook` is not mutex.

### Q45 (DeterministicPlanning, slide 25)

Shopping problem ($s_0=\{$money$\}$, $g=\{$food, ticket$\}$, `buyFood` and `buyTicket` each need money and give $\neg$money, `work` gives money). Food and ticket are both in state-level 1. Graphplan starts solution extraction at the first level where both goals are present and not mutex. Which level is it?

- A) Level 2
- B) Level 4
- C) Level 1
- D) Level 3

> [!success]- Answer
> **D.** Level 1: food comes only from `buyFood`, ticket only from `buyTicket`, and these interfere: mutex.
> Level 2: all four producer pairs are mutex (interference, or competing needs because money is mutex with food and with ticket in state-level 1).
> Level 3: in state-level 2 money and ticket are no longer mutex (`work` with `no-op(ticket)`), so `buyFood` and `no-op(ticket)` are not mutex. Extraction starts at level 3, the real plan length.
> - A: At level 2 the no-ops of food and ticket still have mutex preconditions (competing needs).
> - B: This counts state-level 0 as level 1.
> - C: Food and ticket are present at level 1, but the mutex test was skipped.

### Q46 (DeterministicPlanning, slide 26)

Dinner date (`cook`: needs cleanHands, gives dinner; `wrap`: needs quiet, gives present; `carry`: no precondition, gives $\neg$garbage, $\neg$cleanHands). Solution extraction is called with $g=\{\neg$garbage, dinner, present$\}$ at level 2. It chooses `carry` for $\neg$garbage, `no-op(dinner)` for dinner and `wrap` for present. What is the new goal set $g'$ for level 1?

- A) $\{$cleanHands, quiet$\}$
- B) $\{\neg$garbage, dinner, present$\}$
- C) $\{$quiet$\}$
- D) $\{$dinner, quiet$\}$

> [!success]- Answer
> **D.** $g'$ is the set of the preconditions of the chosen actions. `carry`: none. `no-op(dinner)`: dinner. `wrap`: quiet. The next call is solution-extraction($\{$dinner, quiet$\}$, 1).
> - A: This uses the precondition of `cook`. But dinner was chosen through `no-op(dinner)`.
> - B: These are the effects of the chosen actions (the old goals), not their preconditions.
> - C: The no-op has a precondition too: the literal it carries forward.

### Q47 (DeterministicPlanning, slide 27)

Dinner date as in Q44. At level 1, `carry` is mutex with `cook` and `dolly` is mutex with `wrap` (interference). The goals $\neg$garbage, dinner, present are all in state-level 1 and no two are mutex. Still no plan with one level exists. Why?

- A) Each pair of goals has non-mutex producers, but no single choice works for all three goals.
- B) The goal $\neg$garbage is not yet in state-level 1, so one level is too short for it.
- C) `cook` and `wrap` are mutex at level 1, so dinner and present cannot be reached together.
- D) Graphplan checks mutexes only between literals, so mutex actions can still be chosen.

> [!success]- Answer
> **A.** Pairs: `dolly`, `cook` for $\neg$garbage and dinner; `carry`, `wrap` for $\neg$garbage and present; `cook`, `wrap` for dinner and present. None is mutex.
> All three goals need `cook`, `wrap` and one of `carry`, `dolly`, and each of those two clashes with one of them. Mutex is a test on pairs, so the check passes but extraction fails.
> - B: $\neg$garbage is in state-level 1, produced by `carry` and `dolly`.
> - C: `cook` and `wrap` have different preconditions and effects. They are not mutex.
> - D: Graphplan also marks mutex actions, and extraction never chooses two of them together.

### Q48 (DeterministicPlanning, slide 29)

Dinner date as in Q44, after the second expansion (figure). State-level 0 has 5 literals, state-level 1 has all 10 literals. Each action-level has the 4 real actions plus one no-op per literal of the previous state-level. How many nodes does action-level 2 have? Are $\neg$quiet and present (mutex in state-level 1) still mutex in state-level 2?

![[cmpe421_lecture_07_deterministicplanning_p029.jpg]]

- A) 14 nodes. Still mutex.
- B) 14 nodes. No longer mutex.
- C) 9 nodes. Still mutex.
- D) 9 nodes. No longer mutex.

> [!success]- Answer
> **B.** Action-level 2: 4 real actions + 10 no-ops = 14.
> In level 2, present has a second producer, `no-op(present)`. `dolly` (gives $\neg$quiet) and `no-op(present)` are not mutex: no negated effect, `dolly` does not delete present, and `dolly` has no precondition. One non-mutex pair is enough.
> - A: The count is right, but the level-1 mutexes were copied. Mutexes can only shrink from level to level.
> - C: Both slips: no-ops for state-level 0 only, and the mutex copied from level 1.
> - D: This gives no-ops only for the 5 literals of state-level 0, as in action-level 1.

### Q49 (DeterministicPlanning, slides 30-31)

Dinner date, extraction at state-level 2. $\neg$garbage has 3 producers (`carry`, `dolly`, `no-op(`$\neg$`garbage)`), dinner has 2 (`cook`, `no-op(dinner)`), present has 2 (`wrap`, `no-op(present)`): 12 combinations. The mutex pairs at action-level 2 among these actions are `carry` with `cook` and `dolly` with `wrap`. How many of the 12 combinations contain a mutex pair?

- A) 2
- B) 8
- C) 4
- D) 6

> [!success]- Answer
> **C.** Combinations with `carry` and `cook`: present can be `wrap` or `no-op(present)`, 2. Combinations with `dolly` and `wrap`: dinner can be `cook` or `no-op(dinner)`, 2. $2+2=4$ fail, and $12-4=8$ look OK.
> - A: Only the combinations made of real actions were counted, one per mutex pair.
> - B: This is the number of combinations without a mutex pair.
> - D: This also takes `carry` and `no-op(dinner)` as mutex. But `carry` deletes cleanHands, which is not a precondition of `no-op(dinner)`.

### Q50 (DeterministicPlanning, slide 32)

Dinner date: $s_0=\{$garbage, cleanHands, quiet$\}$, $g=\{$dinner, present, $\neg$garbage$\}$. `cook` needs cleanHands, gives dinner. `wrap` needs quiet, gives present. `carry` gives $\neg$garbage, $\neg$cleanHands. The extraction gives level 1: `cook`, level 2: `carry`, `wrap`. Which sequence is a correct plan?

- A) `carry`, `cook`, `wrap`
- B) `cook`, `wrap`, `carry`
- C) `wrap`, `carry`, `cook`
- D) `carry`, `wrap`, `cook`

> [!success]- Answer
> **B.** Level 1 comes first, then the level-2 actions in any order. $\{$garbage, cleanHands, quiet$\}$; `cook` adds dinner; `wrap` adds present; `carry` removes garbage and cleanHands. Result $\{$quiet, dinner, present$\}$: goal holds.
> - A: `carry` removes cleanHands first, so `cook` is not executable.
> - C: `carry` comes before `cook`, so cleanHands is gone when `cook` needs it.
> - D: Same slip: `cook` is placed after `carry`, which deletes its precondition.

## 13. Heuristics and FastForward (slides 33-38)

### Q51 (DeterministicPlanning, slide 33)

Initial state: blocks a1 to a500 all on the table and clear, hand empty. Goal $\{$`on(a1,a2)`, `on(a2,a3)`$\}$. Blocks-world operators as in Q33 (`pickup`, `stack`), Q31 (`putdown`) and Q37 (`unstack`). In the initial state exactly the 500 `pickup` actions are executable. How many actions does a shortest plan have, and how many of the 500 first actions start a shortest plan?

![[cmpe421_lecture_07_deterministicplanning_p033.jpg]]

- A) 2 actions, 1 starts a shortest plan
- B) 4 actions, 2 start a shortest plan
- C) 4 actions, 1 starts a shortest plan
- D) 2 actions, 2 start a shortest plan

> [!success]- Answer
> **C.** Shortest plan: `pickup(a2)`, `stack(a2,a3)`, `pickup(a1)`, `stack(a1,a2)` (4 actions). Each `stack` needs `holding(x)`, so a `pickup` comes before it.
> Only `pickup(a2)` starts it: if a1 goes on a2 first, a2 is no longer clear and cannot be picked up. The other 497 blocks are not in the goal.
> - A: This counts only the two `stack` actions. Each needs a `pickup` before it, because the hand starts empty.
> - B: The length is right, but `pickup(a1)` is not a good start. Then a1 must be put down again, or stacked on a2, which covers a2. Both make the plan longer.
> - D: Both slips: the `pickup` actions are not counted, and `pickup(a1)` is taken as a good start.

### Q52 (DeterministicPlanning, slide 34)

Graphplan has two parts: graph expansion and solution extraction. A heuristic $h(s)$ is computed for every state that forward search generates. Which part can $h$ use, and why?

- A) Graph expansion, because it takes polynomial time and $h$ is called very often.
- B) Graph expansion, because it is the only part of Graphplan that uses mutexes.
- C) Solution extraction, because it gives the exact number of steps to the goal.
- D) Solution extraction, because the expansion alone gives no number for a state.

> [!success]- Answer
> **A.** Expansion is polynomial. Extraction is a backward search with backtracking, exponential in the worst case. $h$ runs thousands of times, so it must be cheap.
> - B: The extraction also uses mutexes (it backtracks on mutex pairs). The reason is the running time.
> - C: Extraction from $s$ would solve the whole problem from $s$, and then forward search is not needed. It is far too slow for a heuristic.
> - D: The expansion does give a number: the first level where the goals are possibly achieved.

### Q53 (DeterministicPlanning, slide 35)

$\Delta^g(s_0,g)$ is the first level of the planning graph where all goals are present and no two are mutex. Shopping: $s_0=\{$money$\}$, $g=\{$food, ticket$\}$, `buyFood`/`buyTicket` need money and give $\neg$money, `work` gives money. Dinner date: the goals $\neg$garbage, dinner, present are present and pairwise non-mutex in state-level 1; the real plan has 3 actions in 2 levels. What is $\Delta^g$ for shopping and for the dinner date?

- A) Shopping 1, dinner 1
- B) Shopping 3, dinner 1
- C) Shopping 3, dinner 2
- D) Shopping 1, dinner 2

> [!success]- Answer
> **B.** Shopping: food and ticket are present from level 1 but mutex at levels 1 and 2, not mutex at level 3: $\Delta^g=3$, equal to the real cost.
> Dinner: the goals are present and pairwise non-mutex at level 1: $\Delta^g=1\le3$. A lower bound, not tight.
> - A: The dinner value is right, but for shopping the mutex between food and ticket was ignored.
> - C: The shopping value is right, but $\Delta^g$ uses the pairwise test of the graph, not the real number of plan levels.
> - D: Both slips: the shopping mutex ignored, and the real plan's levels used instead of the graph test.

### Q54 (DeterministicPlanning, slide 35, level heuristics)

Shopping problem as in the previous question. The level cost of a literal is the first state-level where it appears. Max-level is the largest level cost of the goals, level-sum is their sum, and set-level is the first level where all goals are present and no two are mutex. Give max-level, level-sum and set-level.

- A) Max-level 2, level-sum 1, set-level 3
- B) Max-level 2, level-sum 1, set-level 1
- C) Max-level 1, level-sum 2, set-level 1
- D) Max-level 1, level-sum 2, set-level 3

> [!success]- Answer
> **D.** Food and ticket both appear in state-level 1, so each level cost is 1. Max-level $=\max(1,1)=1$. Level-sum $=1+1=2$. Set-level $=3$: food and ticket stop being mutex at level 3.
> Only set-level uses the mutexes, so only it sees that the money is enough for one purchase.
> - A: Max-level and level-sum are swapped (max taken for the sum and the sum for the max).
> - B: Both slips: max and sum swapped, and the mutex ignored.
> - C: Max-level and level-sum are right, but the set-level ignores the mutex.

### Q55 (DeterministicPlanning, slide 36)

Shopping problem ($s_0=\{$money$\}$, $g=\{$food, ticket$\}$; `buyFood`: needs money, gives food, $\neg$money; `buyTicket`: needs money, gives ticket, $\neg$money; `work`: gives money). FastForward's $h(s)$ is the number of actions of a relaxed plan from $s$ in which all delete lists are removed. What are $h(s_0)$ and $h(\{$food$\})$?

- A) $h(s_0)=2$, $h(\{$food$\})=2$
- B) $h(s_0)=3$, $h(\{$food$\})=2$
- C) $h(s_0)=2$, $h(\{$food$\})=1$
- D) $h(s_0)=3$, $h(\{$food$\})=1$

> [!success]- Answer
> **A.** Without delete lists, money is never used up. From $\{$money$\}$: `buyFood`, `buyTicket`, so $h=2$ (the real cost is 3).
> From $\{$food$\}$: there is no money, so `work`, then `buyTicket`: $h=2$.
> - B: This keeps the delete lists for $s_0$ and gives the real cost 3, not the relaxed one.
> - C: `buyTicket` was counted without checking its precondition money. `work` is needed first.
> - D: Both slips: the real cost for $s_0$, and the missing `work` for $\{$food$\}$.

### Q56 (DeterministicPlanning, slide 37)

Shopping problem without the action `work`: $s_0=\{$money$\}$, $g=\{$food, ticket$\}$, `buyFood` and `buyTicket` each need money and give $\neg$money. FastForward goes greedily from $s_0$ to $\{$food$\}$ by `buyFood`. What happens next?

- A) FF does a breadth-first search from $\{$food$\}$ until it finds a state with a smaller $h$.
- B) FF goes back to $s_0$, tries `buyTicket` instead, and from there reaches the goal.
- C) No action is executable in $\{$food$\}$ and the goal is not reached: a dead end, so FF fails.
- D) `buyTicket` is still possible in the relaxed problem, so FF executes it and reaches the goal.

> [!success]- Answer
> **C.** Both actions need money, and nothing produces money. In $\{$food$\}$ no action is executable and the goal does not hold: a dead end. FF fails.
> Here no solution exists at all (two purchases, money for one), so failing is correct.
> - A: The breadth-first step is for a local minimum, where children exist. A dead end has no children to search.
> - B: FF does not keep other branches, and from $\{$ticket$\}$ the food cannot be bought either.
> - D: The relaxed problem is only used to compute $h$. Real actions still need their real preconditions.

### Q57 (DeterministicPlanning, slide 38)

The slide says about FastForward: "large variation in how good or bad its plans were, but it found them very quickly". Which design decisions explain "very quickly" and "large variation"?

- A) Quickly: $h$ runs full extraction on a relaxed problem. Variation: greedy steps never compare complete plans.
- B) Quickly: $h$ uses only graph expansion, and no big frontier is kept. Variation: greedy steps never compare complete plans.
- C) Quickly: $h$ uses only graph expansion, and no big frontier is kept. Variation: relaxed plans may hold non-executable actions.
- D) Quickly: $h$ runs full extraction on a relaxed problem. Variation: relaxed plans may hold non-executable actions.

> [!success]- Answer
> **B.** Quickly: the heuristic uses only the polynomial expansion, and the greedy search follows one path without an A*-style frontier.
> Variation: the greedy search takes the child that looks best and never compares whole plans. Nothing keeps the plan short; plateaus and detours make it long.
> - A: Full solution extraction is the exponential part. FF avoids it for speed.
> - C: The plan FF returns is made of real executed steps. Relaxed plans are only used to count $h$.
> - D: Both reasons are wrong: extraction is avoided, and the returned plan is always executable.

## Part C. Extra deck: inference in predicate logic

## 14. Instantiation and propositionalization (inference deck, slides 1-8)

### Q58 (inference deck, slides 1-2)

Slide 1 lists two results of Gödel, one from 1930 and one from 1931. What do they say?

- A) 1930: no complete proof procedure for FOL exists. 1931: a complete procedure exists for arithmetic.
- B) 1930: a complete proof procedure for FOL exists. 1931: a complete procedure exists for arithmetic.
- C) 1930: a complete proof procedure for FOL exists. 1931: no complete procedure exists for arithmetic.
- D) 1930: no complete proof procedure for FOL exists. 1931: no complete procedure exists for arithmetic.

> [!success]- Answer
> **C.** 1930 (completeness theorem, $\exists$ on the slide): every entailed FOL sentence can be proved by one procedure.
> 1931 (incompleteness theorem, $\neg\exists$ on the slide): for arithmetic no such procedure exists. Some true sentences about numbers cannot be proved.
> - A: This turns both results around.
> - B: The 1931 result is negative. Arithmetic is the case where completeness is lost.
> - D: The 1930 result is positive. Resolution (slides 45-48) is such a complete procedure for FOL.

### Q59 (inference deck, slide 3)

The KB has the constants $\mathit{Ann}$ and $\mathit{Bob}$, the function $\mathit{mother}$, and the sentence $\forall x\ \mathit{Student}(x)\Rightarrow\mathit{Reads}(x,\mathit{mother}(x))$. Universal instantiation (UI) replaces $x$ by one ground term $g$. Which sentence does UI give?

- A) $\mathit{Student}(\mathit{mother}(\mathit{Ann}))\Rightarrow\mathit{Reads}(\mathit{mother}(\mathit{Ann}),\mathit{mother}(\mathit{mother}(\mathit{Ann})))$
- B) $\mathit{Student}(y)\Rightarrow\mathit{Reads}(y,\mathit{mother}(y))$
- C) $\mathit{Student}(\mathit{mother}(\mathit{Bob}))\Rightarrow\mathit{Reads}(\mathit{mother}(\mathit{Bob}),\mathit{mother}(\mathit{mother}(\mathit{Ann})))$
- D) $\mathit{Student}(\mathit{mother}(\mathit{Ann}))\Rightarrow\mathit{Reads}(\mathit{mother}(\mathit{Ann}),\mathit{mother}(\mathit{Ann}))$

> [!success]- Answer
> **A.** $\mathit{mother}(\mathit{Ann})$ is a ground term. With $\{x=\mathit{mother}(\mathit{Ann})\}$ every $x$ is replaced, also the $x$ inside $\mathit{mother}(x)$. This gives $\mathit{mother}(\mathit{mother}(\mathit{Ann}))$.
> - B: $y$ is a variable, not a ground term. This is only a renaming of the rule.
> - C: One substitution replaces every $x$ by the same term. Here $x$ became $\mathit{mother}(\mathit{Bob})$ in two places and $\mathit{mother}(\mathit{Ann})$ in the third.
> - D: The last argument must be $\mathit{mother}(x)$ with $x=\mathit{mother}(\mathit{Ann})$. $\mathit{mother}(\mathit{Ann})$ comes from $x=\mathit{Ann}$, a different substitution.

### Q60 (inference deck, slide 4)

The KB contains $\mathit{Cat}(\mathit{Tom})$, $\mathit{Hungry}(\mathit{Tom})$ and $\exists x\ \mathit{Cat}(x)\land\mathit{Owns}(\mathit{Mary},x)$. What is the correct result of existential instantiation (EI) on the last sentence?

- A) $\mathit{Cat}(\mathit{Tom})\land\mathit{Owns}(\mathit{Mary},\mathit{Tom})$
- B) $\mathit{Cat}(x)\land\mathit{Owns}(\mathit{Mary},x)$, with $\exists$ simply dropped
- C) $\mathit{Cat}(C_1)\land\mathit{Owns}(\mathit{Mary},C_1)\land\mathit{Hungry}(C_1)$, $C_1$ new
- D) $\mathit{Cat}(C_1)\land\mathit{Owns}(\mathit{Mary},C_1)$, with $C_1$ a new constant

> [!success]- Answer
> **D.** EI names the promised object with a new constant (a Skolem constant). Nothing else is known about $C_1$, so nothing false is added.
> - A: $\mathit{Tom}$ already appears in the KB. Mary owns some cat, maybe a different one. With A we could "prove" that Mary owns a hungry cat.
> - B: A variable without a quantifier is read as universal. This would say that Mary owns every cat.
> - C: $\mathit{Hungry}(C_1)$ does not follow. Only $\mathit{Tom}$ is known to be hungry.

### Q61 (inference deck, slide 5)

The limit proof on slide 5 has two steps: (1) "let $\epsilon$ be any number $>0$" and (2) "let $\delta_1>0$ be such that ...". Which inference rule is each step?

- A) (1) universal instantiation, (2) universal instantiation
- B) (1) universal instantiation, (2) existential instantiation
- C) (1) existential instantiation, (2) universal instantiation
- D) (1) existential instantiation, (2) existential instantiation

> [!success]- Answer
> **B.** "Any $\epsilon$" removes $\forall\epsilon$: UI. It can be repeated with other values.
> "Let $\delta_1$ be such that" names the object that $\exists\delta$ promises: EI. $\delta_1$ is a Skolem constant, used once.
> - A: $\delta_1$ is not chosen freely. It is the one value whose existence the definition promises.
> - C: This swaps the two. "Any" means every value, so it removes a $\forall$.
> - D: $\epsilon$ may be any positive number. The proof must work for all of them.

### Q62 (inference deck, slide 6)

Propositionalize this KB: $\forall x\ \mathit{Cat}(x)\Rightarrow\mathit{Mammal}(x)$, $\mathit{Cat}(\mathit{Tom})$, $\mathit{Dog}(\mathit{Rex})$. How many instances of the rule are there, and does the new KB entail $\mathit{Mammal}(\mathit{Rex})$?

- A) 1 instance. $\mathit{Mammal}(\mathit{Rex})$ is not entailed.
- B) 2 instances. $\mathit{Mammal}(\mathit{Rex})$ is entailed.
- C) 1 instance. $\mathit{Mammal}(\mathit{Rex})$ is entailed.
- D) 2 instances. $\mathit{Mammal}(\mathit{Rex})$ is not entailed.

> [!success]- Answer
> **D.** There are 2 constants and no functions, so 2 instances: $\mathit{Cat}(\mathit{Tom})\Rightarrow\mathit{Mammal}(\mathit{Tom})$ and $\mathit{Cat}(\mathit{Rex})\Rightarrow\mathit{Mammal}(\mathit{Rex})$.
> $\mathit{Cat}(\mathit{Rex})$ is not known, so Modus Ponens cannot give $\mathit{Mammal}(\mathit{Rex})$. Only $\mathit{Mammal}(\mathit{Tom})$ is entailed.
> - A: Propositionalization uses every constant, not only those whose premise is known to hold.
> - B: An instance is a rule, not a fact. Its conclusion needs its premise $\mathit{Cat}(\mathit{Rex})$.
> - C: Both slips: one instance too few, and the conclusion taken without its premise.

### Q63 (inference deck, slide 7)

A KB without functions has $p=4$ predicates, each of arity $k=3$, and $n=20$ constants. How many ground atoms can be built?

- A) $32\,000$
- B) $8000$
- C) $512\,000$
- D) $240$

> [!success]- Answer
> **A.** Each predicate has $n^k$ argument tuples, so there are $p\cdot n^k$ ground atoms.
> $4\cdot20^3=4\cdot8000=32\,000$.
> - B: $20^3=8000$ counts the tuples of only one predicate. Multiply by $p=4$.
> - C: $(p\cdot n)^k=80^3=512\,000$ puts $p$ inside the power.
> - D: $p\cdot k\cdot n=240$ multiplies by $k$ instead of raising $n$ to the power $k$.

### Q64 (inference deck, slide 8)

A KB has one constant $A$ and one binary function $g$ (two arguments). A constant has depth 0, and $g(s_1,s_2)$ has depth one more than the deeper of $s_1,s_2$. How many ground terms have depth $\le3$?

- A) $4$
- B) $15$
- C) $26$
- D) $21$

> [!success]- Answer
> **C.** Let $t_n$ be the number of terms of depth $\le n$. A term is $A$ or $g(s_1,s_2)$ with $s_1,s_2$ of depth $\le n-1$: $t_n=1+t_{n-1}^2$.
> $t_0=1$, $t_1=1+1^2=2$, $t_2=1+2^2=5$, $t_3=1+5^2=26$.
> - A: $1+t_{n-1}$ gives $1,2,3,4$. That treats $g$ as a function of one argument.
> - B: $1+2t_{n-1}$ gives $1,3,7,15$. The two arguments are chosen independently, so the counts multiply, they do not add.
> - D: $26-5=21$ counts only the terms of depth exactly 3 and forgets the 5 shallower terms.

## 15. Unification and Generalized Modus Ponens (inference deck, slides 9-20)

### Q65 (inference deck, slide 9)

KB: $\forall x\ \mathit{Bird}(x)\land\mathit{Small}(x)\Rightarrow\mathit{Flies}(x)$, $\mathit{Bird}(\mathit{Tweety})$, $\forall y\ \mathit{Small}(y)$, $\mathit{Friend}(\mathit{Rex},\mathit{Bob})$. Generalized Modus Ponens needs a $\theta$ that makes each fact equal to its premise. Which $\theta$ infers $\mathit{Flies}(\mathit{Tweety})$ in one step?

- A) $\{y=\mathit{Tweety}\}$
- B) $\{x=\mathit{Tweety},\ y=\mathit{Bob}\}$
- C) $\{x=y\}$
- D) $\{x=\mathit{Tweety},\ y=\mathit{Tweety}\}$

> [!success]- Answer
> **D.** $\mathit{Bird}(x)\theta=\mathit{Bird}(\mathit{Tweety})$ and $\mathit{Small}(x)\theta=\mathit{Small}(y)\theta=\mathit{Small}(\mathit{Tweety})$. Both premises match, and $\mathit{Flies}(x)\theta=\mathit{Flies}(\mathit{Tweety})$.
> Propositionalization would first create 3 $\mathit{Small}$ sentences, one per constant. Unification creates only the one needed.
> - A: $x$ stays unbound, so $\mathit{Bird}(x)$ does not match $\mathit{Bird}(\mathit{Tweety})$.
> - B: $\mathit{Small}(\mathit{Bob})$ and $\mathit{Small}(\mathit{Tweety})$ are different, so the second premise does not match.
> - C: This matches only the $\mathit{Small}$ premise. $\mathit{Bird}(y)$ is not the fact $\mathit{Bird}(\mathit{Tweety})$.

### Q66 (inference deck, slides 10-12)

Find a unifier or say that none exists: (a) $\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(y,\mathit{Pizza})$, (b) $\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(\mathit{Bob},y)$.

- A) (a) $\{y=\mathit{Ann},\ x=\mathit{Pizza}\}$. (b) No unifier.
- B) (a) $\{x=\mathit{Ann},\ y=\mathit{Pizza}\}$. (b) $\{x=y\}$.
- C) (a) $\{y=\mathit{Ann},\ x=\mathit{Pizza}\}$. (b) $\{x=y\}$.
- D) (a) $\{x=\mathit{Ann},\ y=\mathit{Pizza}\}$. (b) No unifier.

> [!success]- Answer
> **A.** (a) Compare position by position. First arguments: $\mathit{Ann}$ and $y$, so $y=\mathit{Ann}$. Second: $x$ and $\mathit{Pizza}$, so $x=\mathit{Pizza}$. Both become $\mathit{Likes}(\mathit{Ann},\mathit{Pizza})$.
> (b) The first arguments are the two different constants $\mathit{Ann}$ and $\mathit{Bob}$. A substitution changes only variables: no unifier.
> - B: Both slips of C and D together.
> - C: $\{x=y\}$ handles only the second arguments. $\mathit{Ann}$ and $\mathit{Bob}$ still differ.
> - D: Each variable gets the constant in the same position of the other literal. $x$ stands opposite $\mathit{Pizza}$, not $\mathit{Ann}$.

### Q67 (inference deck, slide 13)

Unify $\mathit{Teaches}(\mathit{Ali},x)$ with $\mathit{Teaches}(y,\mathit{course}(y))$. What is the finished unifier?

- A) $\{y=\mathit{Ali},\ x=\mathit{course}(y)\}$
- B) $\{x=\mathit{Ali},\ y=\mathit{course}(\mathit{Ali})\}$
- C) No unifier: $y$ occurs inside $\mathit{course}(y)$.
- D) $\{y=\mathit{Ali},\ x=\mathit{course}(\mathit{Ali})\}$

> [!success]- Answer
> **D.** First arguments: $y=\mathit{Ali}$. Apply it to the second arguments: $x$ against $\mathit{course}(\mathit{Ali})$, so $x=\mathit{course}(\mathit{Ali})$. Both become $\mathit{Teaches}(\mathit{Ali},\mathit{course}(\mathit{Ali}))$.
> - A: The earlier binding $y=\mathit{Ali}$ was not applied. Used at once, A turns the first literal into $\mathit{Teaches}(\mathit{Ali},\mathit{course}(y))$, which still contains $y$.
> - B: This binds $x$ to the constant opposite $y$ and $y$ to the term opposite $x$: the positions are swapped.
> - C: The occur check fails only when a variable is bound to a term that contains this same variable. Here $x$ (not $y$) is bound to $\mathit{course}(\mathit{Ali})$.

### Q68 (inference deck, slides 14-15)

$\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(x,\mathit{Pizza})$ come from two different sentences. Rename the $x$ of the second literal to $x_2$ (standardizing apart). What is the result?

- A) $\{x=\mathit{Ann},\ x_2=\mathit{Pizza}\}$
- B) No unifier: $x$ cannot be $\mathit{Ann}$ and $\mathit{Pizza}$ at once.
- C) $\{x_2=\mathit{Ann},\ x=\mathit{Pizza}\}$
- D) No unifier: the renaming changes what the second sentence says.

> [!success]- Answer
> **C.** After renaming: $\mathit{Likes}(\mathit{Ann},x)$ and $\mathit{Likes}(x_2,\mathit{Pizza})$. First arguments: $x_2=\mathit{Ann}$. Second arguments: $x=\mathit{Pizza}$.
> - A: The bindings are put on the wrong variables. $x$ stands opposite $\mathit{Pizza}$, $x_2$ opposite $\mathit{Ann}$.
> - B: This is the result without renaming. After standardizing apart there are two variables.
> - D: Each sentence has its own $\forall$. $\forall x\ \mathit{Likes}(x,\mathit{Pizza})$ and $\forall x_2\ \mathit{Likes}(x_2,\mathit{Pizza})$ mean the same.

### Q69 (inference deck, slide 16)

Which pairs unify? (a) $P(x,x)$ and $P(y,f(y))$, (b) $P(x,g(x))$ and $P(g(y),y)$, (c) $P(x,f(y))$ and $P(z,f(z))$.

- A) all three
- B) (b) and (c)
- C) none of them
- D) only (c)

> [!success]- Answer
> **D.** (a) $x=y$, then $y$ against $f(y)$: $y$ occurs inside $f(y)$, the occur check fails.
> (b) $x=g(y)$, then $g(g(y))$ against $y$: $y$ occurs inside, fail.
> (c) $x=z$, then $f(y)$ against $f(z)$: $y=z$. Both become $P(z,f(z))$.
> - A: Without the occur check, (a) and (b) give $y=f(y)$ and $y=g(g(y))$. No finite term solves these.
> - B: In (b) $y$ is two levels deep inside $g(g(y))$. The occur check must search the whole term, not only its top level.
> - C: Two different variables can be unified: $y=z$ is a correct binding.

### Q70 (inference deck, slide 17)

$\alpha=P(x,f(y))$ and $\beta=P(g(z),w)$ have the mgu $\theta=\{x=g(z),\ w=f(y)\}$. What is $\theta_3=\{x=g(A),\ z=A,\ w=f(y)\}$?

- A) Not a unifier, because $\alpha\theta_3\ne\beta\theta_3$.
- B) A unifier, not an mgu: $\theta_3$ is $\theta$ applied first, then $\{z=A\}$.
- C) A unifier, not an mgu: $\theta_3$ is $\{z=A\}$ applied first, then $\theta$.
- D) A unifier and also an mgu: it binds only these variables.

> [!success]- Answer
> **B.** $\alpha\theta_3=P(g(A),f(y))=\beta\theta_3$: a unifier. Nothing forces $z=A$, so it is not most general.
> $\theta\{z=A\}$: apply $\{z=A\}$ to the right sides of $\theta$ ($x=g(A)$, $w=f(y)$) and add $z=A$. This is $\theta_3$.
> - A: Both sides become $P(g(A),f(y))$, so they are equal.
> - C: Wrong order. First $\{z=A\}$ leaves $x$ alone, then $\theta$ gives $x=g(z)$, so $x$ ends as $g(z)$, not $g(A)$.
> - D: The mgu result $P(g(z),f(y))$ cannot be reached from $P(g(A),f(y))$: a constant cannot turn back into a variable.

### Q71 (inference deck, slide 18)

Run the unification algorithm on $Q(x,g(x),y)$ and $Q(A,z,f(z))$, from left to right. Before each argument the current $\theta$ is applied. What is the final $\theta$?

- A) $\{x=A,\ z=g(A),\ y=f(g(A))\}$
- B) $\{x=A,\ z=g(x),\ y=f(z)\}$
- C) $\{x=A,\ z=g(A),\ y=f(z)\}$
- D) Fail: $A$ meets $f(g(A))$ in argument 3.

> [!success]- Answer
> **A.** Argument 1: $x=A$. Argument 2: $g(A)$ and $z$, so $z=g(A)$. Argument 3: $y$ and $f(g(A))$, so $y=f(g(A))$.
> Both literals become $Q(A,g(A),f(g(A)))$.
> - B: The current $\theta$ was never applied. The bindings still contain bound variables.
> - C: $\theta$ was applied before argument 2 but not before argument 3.
> - D: This happens for the first literal $Q(x,g(x),x)$. Here the third argument is $y$, a free variable.

### Q72 (inference deck, slide 19)

Facts: $\mathit{Child}(\mathit{Ece})$, $\mathit{Likes}(w,\mathit{IceCream})$, $\mathit{Sweet}(\mathit{IceCream})$. Rule: $\mathit{Child}(x)\land\mathit{Likes}(x,y)\land\mathit{Sweet}(y)\Rightarrow\mathit{Wants}(x,y)$. GMP needs $p_i'\theta=p_i\theta$ for every fact $p_i'$ and premise $p_i$. Give $\theta$ and the rule as a definite clause.

- A) $\theta=\{x=\mathit{Ece},\ y=\mathit{IceCream}\}$. Clause $\neg\mathit{Child}(x)\lor\neg\mathit{Likes}(x,y)\lor\neg\mathit{Sweet}(y)\lor\mathit{Wants}(x,y)$.
- B) $\theta=\{x=\mathit{Ece},\ w=\mathit{Ece},\ y=\mathit{IceCream}\}$. Clause $\neg\mathit{Child}(x)\lor\neg\mathit{Likes}(x,y)\lor\neg\mathit{Sweet}(y)\lor\neg\mathit{Wants}(x,y)$.
- C) $\theta=\{x=\mathit{Ece},\ w=\mathit{Ece},\ y=\mathit{IceCream}\}$. Clause $\neg\mathit{Child}(x)\lor\neg\mathit{Likes}(x,y)\lor\neg\mathit{Sweet}(y)\lor\mathit{Wants}(x,y)$.
- D) $\theta=\{x=\mathit{Ece},\ y=\mathit{IceCream}\}$. Clause $\neg\mathit{Child}(x)\lor\neg\mathit{Likes}(x,y)\lor\neg\mathit{Sweet}(y)\lor\neg\mathit{Wants}(x,y)$.

> [!success]- Answer
> **C.** $\mathit{Child}(\mathit{Ece})$ and $\mathit{Child}(x)$: $x=\mathit{Ece}$. $\mathit{Likes}(w,\mathit{IceCream})$ and $\mathit{Likes}(\mathit{Ece},y)$: $w=\mathit{Ece}$, $y=\mathit{IceCream}$. $\mathit{Sweet}$ matches. Conclusion $\mathit{Wants}(\mathit{Ece},\mathit{IceCream})$.
> $p_1\land p_2\land p_3\Rightarrow q$ is $\neg p_1\lor\neg p_2\lor\neg p_3\lor q$: exactly one positive literal.
> - A: The variable $w$ of the fact is not bound, so $\mathit{Likes}(w,\mathit{IceCream})\theta\ne\mathit{Likes}(x,y)\theta$.
> - B: The conclusion keeps its sign. With four negative literals the clause has no positive literal and is not definite.
> - D: Both slips of A and B together.

### Q73 (inference deck, slide 20)

The soundness proof of GMP for $\mathit{King}(\mathit{John})$, $\mathit{Greedy}(y)$, $\mathit{King}(x)\land\mathit{Greedy}(x)\Rightarrow\mathit{Evil}(x)$ uses $\theta=\{x=\mathit{John},\ y=\mathit{John}\}$. Step 1 applies UI to the facts, step 2 applies UI to the rule. Which propositional rule is step 3, and what does "sound" mean?

- A) Modus Ponens. Sound: every entailed sentence can be derived.
- B) Modus Ponens. Sound: every derived sentence is entailed.
- C) Modus Tollens. Sound: every derived sentence is entailed.
- D) Modus Tollens. Sound: every entailed sentence can be derived.

> [!success]- Answer
> **B.** After steps 1 and 2 we have $\mathit{King}(\mathit{John})$, $\mathit{Greedy}(\mathit{John})$ and $\mathit{King}(\mathit{John})\land\mathit{Greedy}(\mathit{John})\Rightarrow\mathit{Evil}(\mathit{John})$. Modus Ponens gives $\mathit{Evil}(\mathit{John})$.
> Sound: GMP derives only sentences that the premises entail, because it only does several correct steps at once.
> - A: "Every entailed sentence can be derived" is completeness, not soundness.
> - C: Modus Tollens goes from $\neg q$ to $\neg p$. Here the premises are known and the conclusion is wanted.
> - D: Both slips of A and C together.

## 16. Forward chaining in FOL (inference deck, slides 21-31)

### Q74 (inference deck, slides 21-23)

Slide 23 asks what the problem is with the axiom $\forall x\ \mathit{LawSays}(x)\Rightarrow x$. What is it?

- A) $x$ is a free variable, so the formula is not a sentence.
- B) It is valid FOL, but it should use $\exists x$, since only some laws exist.
- C) It is valid FOL, but forward chaining cannot use it, since it is not a definite clause.
- D) $x$ is used as a term and as a sentence. FOL variables stand only for objects.

> [!success]- Answer
> **D.** $x$ is the argument of $\mathit{LawSays}$, so it is a term, and it also stands alone as a sentence. In FOL a sentence is not a term. Quantifying over sentences needs higher-order logic.
> So each law is written directly as its own FOL rule.
> - A: $x$ is bound by $\forall x$. The problem is the kind of thing $x$ stands for.
> - B: The quantifier is not the problem. With $\exists$ the formula still uses $x$ as a sentence.
> - C: It is not valid FOL at all, so the question of definite clauses does not arise.

### Q75 (inference deck, slide 24)

Translate in the style of slide 24: "All of Nono's tanks were given to it by Ivan."

- A) $\forall x\ \mathit{Tank}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Gave}(\mathit{Ivan},x,\mathit{Nono})$
- B) $\forall x\ \mathit{Tank}(x)\land\mathit{Owns}(\mathit{Nono},x)\land\mathit{Gave}(\mathit{Ivan},x,\mathit{Nono})$
- C) $\exists x\ \mathit{Tank}(x)\land\mathit{Owns}(\mathit{Nono},x)\land\mathit{Gave}(\mathit{Ivan},x,\mathit{Nono})$
- D) $\forall x\ \mathit{Gave}(\mathit{Ivan},x,\mathit{Nono})\Rightarrow\mathit{Tank}(x)\land\mathit{Owns}(\mathit{Nono},x)$

> [!success]- Answer
> **A.** "All" becomes $\forall$ with $\Rightarrow$. The conditions "is a tank" and "Nono owns it" are the premises, the gift is the conclusion.
> - B: $\forall$ with $\land$ says that every object is a tank, owned by Nono and given by Ivan.
> - C: $\exists$ says only that at least one such tank exists. That is "some", not "all".
> - D: The direction is reversed: it says that everything Ivan gave to Nono is a tank.

### Q76 (inference deck, slide 25)

In FOL-FC-Ask a new conclusion $q'$ is added only if it is "not a renaming of a sentence already in KB or new". Why is this test needed?

- A) It standardizes apart the variables of the new facts.
- B) Without it the same facts return in every iteration and the loop never stops.
- C) It makes the result independent of the order of the rules.
- D) A renaming of a fact can be false in some models, so it must not be added.

> [!success]- Answer
> **B.** Without the test, every iteration would derive the old facts again. `new` would never be empty, and the loop would never reach the fixed point. A renaming such as $\mathit{Likes}(y,\mathit{IceCream})$ for $\mathit{Likes}(x,\mathit{IceCream})$ is the same fact.
> - A: The test only refuses sentences. It does not rename any variable.
> - C: Independence of the rule order comes from collecting conclusions in `new` and adding them at the end of the iteration.
> - D: A renaming means exactly the same as the original, so it is true in the same models. It is refused because it adds nothing.

### Q77 (inference deck, slides 26-27)

Rules: R1 $\mathit{Parent}(x,y)\Rightarrow\mathit{Ancestor}(x,y)$, R2 $\mathit{Parent}(x,y)\land\mathit{Ancestor}(y,z)\Rightarrow\mathit{Ancestor}(x,z)$. Facts: $\mathit{Parent}(\mathit{Ali},\mathit{Can})$, $\mathit{Parent}(\mathit{Can},\mathit{Ece})$, $\mathit{Parent}(\mathit{Ece},\mathit{Naz})$. Forward chaining adds the new facts of an iteration at its end. In which iteration is $\mathit{Ancestor}(\mathit{Ali},\mathit{Naz})$ derived, and how many $\mathit{Ancestor}$ facts are there at the fixed point?

- A) Iteration 3. 6 $\mathit{Ancestor}$ facts.
- B) Iteration 3. 9 $\mathit{Ancestor}$ facts.
- C) Iteration 2. 6 $\mathit{Ancestor}$ facts.
- D) Iteration 2. 9 $\mathit{Ancestor}$ facts.

> [!success]- Answer
> **A.** Iteration 1: only R1 fires: $\mathit{Ancestor}(\mathit{Ali},\mathit{Can})$, $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$, $\mathit{Ancestor}(\mathit{Ece},\mathit{Naz})$.
> Iteration 2: R2 gives $\mathit{Ancestor}(\mathit{Ali},\mathit{Ece})$ and $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$.
> Iteration 3: R2 with $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$ gives $\mathit{Ancestor}(\mathit{Ali},\mathit{Naz})$. Iteration 4 adds nothing: $3+2+1=6$ facts.
> - B: 9 counts the 3 $\mathit{Parent}$ facts too. There are 9 facts in total but 6 $\mathit{Ancestor}$ facts.
> - C: $\mathit{Ancestor}(\mathit{Can},\mathit{Naz})$ is found in iteration 2. It can be used only from iteration 3 on.
> - D: Both slips of B and C together.

### Q78 (inference deck, slide 28)

![[cmpe421_lecture_8_inference_predicatelogic_p028.jpg]]

Remove the fact $\mathit{Enemy}(\mathit{Nono},\mathit{America})$ from the KB of the figure. Which facts does forward chaining still derive?

- A) $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$, $\mathit{Hostile}(\mathit{Nono})$
- B) None: the proof of $\mathit{Criminal}(\mathit{West})$ fails, so nothing is added.
- C) $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$, $\mathit{Criminal}(\mathit{West})$
- D) $\mathit{Weapon}(M_1)$ and $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$ only

> [!success]- Answer
> **D.** Iteration 1 still gives $\mathit{Weapon}(M_1)$ (from $\mathit{Missile}(M_1)$) and $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$ (from $\mathit{Missile}(M_1)$ and $\mathit{Owns}(\mathit{Nono},M_1)$). Iteration 2 adds nothing: fixed point.
> - A: The only rule for $\mathit{Hostile}$ needs an $\mathit{Enemy}$ fact.
> - B: Forward chaining has no goal to fail. It fires every rule whose premises are known.
> - C: The crime rule needs all four premises (the arc). No fact matches $\mathit{Hostile}(z)$.

### Q79 (inference deck, slide 29)

The ancestor KB of Q77 is Datalog: $p=2$ predicates of arity $k=2$ and $n=4$ constants. What is the bound $p\cdot n^k$ on the number of ground atoms, which guarantees that forward chaining stops?

- A) $8$
- B) $64$
- C) $16$
- D) $32$

> [!success]- Answer
> **D.** $p\cdot n^k=2\cdot4^2=2\cdot16=32$. Every iteration except the last adds at least one new fact, so there are at most 32 such iterations. The trace needed only 4 and reached 9 facts.
> - A: $p\cdot n=8$ forgets that each of the $k=2$ arguments takes $n$ values.
> - B: $(p\cdot n)^k=8^2=64$ puts $p$ inside the power.
> - C: $n^k=16$ counts the tuples of only one predicate.

### Q80 (inference deck, slide 30)

The Colonel West KB has four rules: the crime rule $\mathit{American}(x)\land\mathit{Weapon}(y)\land\mathit{Sells}(x,y,z)\land\mathit{Hostile}(z)\Rightarrow\mathit{Criminal}(x)$, the Sells rule $\mathit{Missile}(x)\land\mathit{Owns}(\mathit{Nono},x)\Rightarrow\mathit{Sells}(\mathit{West},x,\mathit{Nono})$, the Weapon rule $\mathit{Missile}(x)\Rightarrow\mathit{Weapon}(x)$ and the Hostile rule $\mathit{Enemy}(x,\mathit{America})\Rightarrow\mathit{Hostile}(x)$. Iteration 1 of forward chaining adds $\mathit{Weapon}(M_1)$, $\mathit{Sells}(\mathit{West},M_1,\mathit{Nono})$ and $\mathit{Hostile}(\mathit{Nono})$. Slide 30 says: a rule needs to be checked in iteration $k$ only if a premise was added in iteration $k-1$. Which rules are checked in iteration 2?

- A) All four rules, because the KB has changed.
- B) Only the Sells rule and the Weapon rule, whose conclusions are new.
- C) Only the crime rule, whose premises include the new predicates.
- D) No rule, because each rule has already fired once.

> [!success]- Answer
> **C.** The crime rule has the premises $\mathit{Weapon}$, $\mathit{Sells}$ and $\mathit{Hostile}$, all added in iteration 1. The other rules have the premises $\mathit{Missile}$, $\mathit{Owns}$, $\mathit{Enemy}$, and nothing new has these predicates.
> - A: The other three rules can only repeat their old conclusions.
> - B: The test looks at premises, not conclusions. New conclusions are why these rules need no new check.
> - D: A rule may fire again with new facts. The crime rule had not fired at all yet.

### Q81 (inference deck, slide 31)

The map-colouring rule has 6 variables $wa,nt,sa,q,nsw,v$ and 9 $\mathit{Diff}$ premises, one per pair of neighbours: $wa$-$nt$, $wa$-$sa$, $nt$-$sa$, $nt$-$q$, $sa$-$q$, $sa$-$nsw$, $sa$-$v$, $q$-$nsw$, $nsw$-$v$. There are 3 colours. How many colour assignments exist, and how many satisfy all premises?

- A) $216$ assignments, $6$ satisfy all premises
- B) $729$ assignments, $6$ satisfy all premises
- C) $729$ assignments, $3$ satisfy all premises
- D) $216$ assignments, $3$ satisfy all premises

> [!success]- Answer
> **B.** Each of the 6 variables takes 3 colours: $3^6=729$.
> $wa,nt,sa$ touch each other: $3!=6$ ways. Then $q$ must take the colour of $wa$, $nsw$ that of $nt$, $v$ that of $wa$. So $6$ assignments.
> - A: $6^3=216$ swaps base and exponent. It should be colours to the power of variables.
> - C: $3$ fixes only the colour of $wa$. $nt$ and $sa$ can still swap their two colours: $3\cdot2=6$.
> - D: Both slips of A and C together.

## 17. Backward chaining and Prolog (inference deck, slides 32-44)

### Q82 (inference deck, slide 32)

$\theta\theta'$ is the composition: first $\theta$, then $\theta'$. Compute it for $\theta=\{x=f(y)\}$ and $\theta'=\{y=A,\ z=B\}$.

- A) $\{x=f(y),\ y=A,\ z=B\}$
- B) $\{x=f(A),\ y=A,\ z=B\}$
- C) $\{x=f(A)\}$
- D) $\{y=A,\ z=B\}$

> [!success]- Answer
> **B.** Apply $\theta'$ to the right side of $\theta$: $x=f(A)$. Then add the bindings of $\theta'$: $y=A$, $z=B$.
> Test on $P(x,y,z)$: $\theta$ gives $P(f(y),y,z)$, then $\theta'$ gives $P(f(A),A,B)$. The composition gives the same in one step.
> - A: $\theta'$ was not applied to the right side of $\theta$. In one step A gives $P(f(y),A,B)$. This is the wrong order $\theta'\theta$.
> - C: The bindings of $\theta'$ for $y$ and $z$ must be added too.
> - D: The bindings of $\theta'$ do not replace $\theta$. The binding of $x$ is kept.

### Q83 (inference deck, slides 33-35)

![[cmpe421_lecture_8_inference_predicatelogic_p035.jpg]]

The figure starts from the query $\mathit{Criminal}(\mathit{West})$. Suppose the query is $\mathit{Criminal}(w)$ with a variable $w$. The KB is the one listed in the figure, and subgoals are proved from left to right. How does the top of the tree change?

- A) The head unifies by $\{x=w\}$. $\mathit{American}(w)$ then matches the fact by $\{w=\mathit{West}\}$.
- B) The head cannot be unified: $\mathit{Criminal}(w)$ and $\mathit{Criminal}(x)$ contain only variables and no constant.
- C) The head unifies by $\{x=\mathit{West}\}$, because $\mathit{American}(\mathit{West})$ is the only American in the KB.
- D) The head unifies by $\{x=w\}$. $\mathit{American}(w)$ then fails, since $w$ is unbound and no fact has a variable.

> [!success]- Answer
> **A.** $\mathit{Criminal}(w)$ and $\mathit{Criminal}(x)$ unify by $\{x=w\}$. The first subgoal is $\mathit{American}(w)$. It unifies with the fact $\mathit{American}(\mathit{West})$ by $\{w=\mathit{West}\}$. Backward chaining finds out who the criminal is.
> - B: Two variables unify: one is bound to the other.
> - C: Unification uses only the two literals. It does not look at the other facts of the KB.
> - D: A variable in a goal can be bound to a constant of a fact. This is how the query variable gets its value.

### Q84 (inference deck, slides 36-37)

![[cmpe421_lecture_8_inference_predicatelogic_p037.jpg]]

Suppose the KB also has the fact $\mathit{Missile}(M_2)$, written before $\mathit{Missile}(M_1)$, and no fact $\mathit{Owns}(\mathit{Nono},M_2)$. Backward chaining is depth-first, tries the KB sentences in the order of the figure and proves subgoals from left to right. What does it do?

- A) It skips $M_2$ and takes $y=M_1$ at once, because only $\mathit{Owns}(\mathit{Nono},M_1)$ is in the KB.
- B) It takes $y=M_2$, fails on $\mathit{Owns}(\mathit{Nono},M_2)$, then retries $\mathit{Missile}(y)$ with $y=M_1$.
- C) It takes $y=M_2$, fails on $\mathit{Owns}(\mathit{Nono},M_2)$, and the whole query fails.
- D) It keeps both $y=M_2$ and $y=M_1$ and follows the two branches side by side, level by level.

> [!success]- Answer
> **B.** The first match of $\mathit{Missile}(y)$ is $\{y=M_2\}$. The goal $\mathit{Sells}(\mathit{West},M_2,z)$ needs $\mathit{Missile}(M_2)$, which holds, and $\mathit{Owns}(\mathit{Nono},M_2)$, which fails. The algorithm backtracks to the last choice, $\mathit{Missile}(y)$, and takes $\{y=M_1\}$. The work for $M_2$ is lost.
> - A: Backward chaining does not look ahead. It takes the first match and finds the problem later.
> - C: A failed branch is not the end. The caller tries its next sentence: backtracking.
> - D: Following all branches at once is breadth-first search. Backward chaining keeps one path.

### Q85 (inference deck, slides 38-39)

The KB in this order: R1 $\mathit{Parent}(x,y)\Rightarrow\mathit{Ancestor}(x,y)$, R2 $\mathit{Parent}(x,y)\land\mathit{Ancestor}(y,z)\Rightarrow\mathit{Ancestor}(x,z)$, then $\mathit{Parent}(\mathit{Ali},\mathit{Can})$, $\mathit{Parent}(\mathit{Can},\mathit{Ece})$. Backward chaining on $\mathit{Ancestor}(\mathit{Ali},\mathit{Ece})$ tries sentences in this order and subgoals from left to right. Where is the dead end, and which rule proves the subgoal $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$?

- A) Dead end: $\mathit{Parent}(\mathit{Ali},\mathit{Ece})$ from R1. $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$ is proved by R2.
- B) Dead end: $\mathit{Parent}(\mathit{Ali},y_2)$ from R2. $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$ is proved by R1.
- C) Dead end: $\mathit{Parent}(\mathit{Ali},\mathit{Ece})$ from R1. $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$ is proved by R1.
- D) Dead end: $\mathit{Parent}(\mathit{Ali},y_2)$ from R2. $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$ is proved by R2.

> [!success]- Answer
> **C.** R1 first: subgoal $\mathit{Parent}(\mathit{Ali},\mathit{Ece})$. No fact or head unifies: dead end, backtrack.
> R2: subgoals $\mathit{Parent}(\mathit{Ali},y_2)$ and $\mathit{Ancestor}(y_2,\mathit{Ece})$. The first matches $\mathit{Parent}(\mathit{Ali},\mathit{Can})$ with $\{y_2=\mathit{Can}\}$.
> $\mathit{Ancestor}(\mathit{Can},\mathit{Ece})$: R1 is tried first and gives the fact $\mathit{Parent}(\mathit{Can},\mathit{Ece})$. Proved.
> - A: R1 comes first and already succeeds, so R2 is never tried for this subgoal.
> - B: A goal with a variable can match a fact: $\{y_2=\mathit{Can}\}$.
> - D: Both slips of A and B together.

### Q86 (inference deck, slide 40)

The KB has the rule $\mathit{Even}(\mathit{plus2}(x))\Rightarrow\mathit{Even}(x)$ and the fact $\mathit{Even}(\mathit{Zero})$, in this order. The query is $\mathit{Even}(\mathit{One})$. What is the third goal of backward chaining, and does the check "is the current goal already on the stack?" stop the loop?

- A) $\mathit{Even}(\mathit{One})$ again. The check stops the loop.
- B) $\mathit{Even}(\mathit{plus2}(\mathit{plus2}(\mathit{One})))$. The check stops the loop.
- C) $\mathit{Even}(\mathit{plus2}(\mathit{plus2}(\mathit{One})))$. The check does not stop the loop.
- D) $\mathit{Even}(\mathit{One})$ again. The check does not stop the loop.

> [!success]- Answer
> **C.** $\mathit{Even}(\mathit{One})$ unifies with the head $\mathit{Even}(x)$: new goal $\mathit{Even}(\mathit{plus2}(\mathit{One}))$. Again: $\mathit{Even}(\mathit{plus2}(\mathit{plus2}(\mathit{One})))$.
> Each goal is different from all goals before it, so the stack check never finds a repeat. The search goes down forever.
> - A: The goal does not come back. That happens for $P(x)\Rightarrow P(x)$, and there the check does stop it.
> - B: The check looks for an identical goal. Here each goal is new.
> - D: If the same goal came back, the check would see it and cut the branch.

### Q87 (inference deck, slide 41)

The ancestor KB is written in Prolog. One line reads `Parent(ali, Can).` What is wrong with it?

- A) Capitalized words are variables: `Parent` is not a predicate name, and `Can` is a variable.
- B) Only `Parent` is wrong. `Can` is a correct constant, because it names a person.
- C) Only `Can` is wrong. A predicate name may start with a capital letter, as in logic.
- D) Nothing is wrong. Prolog ignores capitals and only checks the final full stop.

> [!success]- Answer
> **A.** In Prolog a capitalized word is a variable, the opposite of our logic notation. `Parent` is read as a variable, so the line is not a valid clause. `Can` is a variable too: `parent(ali, Can).` would say "ali is a parent of everything". Correct: `parent(ali, can).`
> - B: A name of a person is a constant in Prolog only if it starts with a lower-case letter.
> - C: Predicate names must start with a lower-case letter too.
> - D: Capitals carry meaning in Prolog: they mark variables.

### Q88 (inference deck, slide 42)

Prolog program: `bird(tweety).` `bird(pingu).` `penguin(pingu).` `flies(X) :- bird(X), not penguin(X).` The FOL version has the facts $\mathit{Bird}(\mathit{Tweety})$, $\mathit{Bird}(\mathit{Pingu})$, $\mathit{Penguin}(\mathit{Pingu})$ and the rule $\forall x\ \mathit{Bird}(x)\land\neg\mathit{Penguin}(x)\Rightarrow\mathit{Flies}(x)$. What does Prolog answer to `flies(tweety)`, and does the FOL KB entail $\mathit{Flies}(\mathit{Tweety})$?

- A) Prolog: yes. FOL: entailed.
- B) Prolog: no. FOL: not entailed.
- C) Prolog: yes. FOL: not entailed.
- D) Prolog: no. FOL: entailed.

> [!success]- Answer
> **C.** Prolog: `bird(tweety)` succeeds. `penguin(tweety)` cannot be proved, so `not penguin(tweety)` succeeds: yes. This is negation as failure (closed world).
> FOL: the KB does not say $\neg\mathit{Penguin}(\mathit{Tweety})$, so the rule cannot fire. There are models where Tweety is a penguin.
> - A: FOL has no closed-world assumption. "Not provable" is not the same as "false".
> - B: Prolog does not need a proof that Tweety is not a penguin. Failing to prove that it is one is enough.
> - D: This swaps the two systems.

### Q89 (inference deck, slide 43)

Clause: `avg(Y,Z,X) :- X is (Y + Z)/2.` What does Prolog do with `avg(7,3,X)` and with `avg(Y,3,5)`?

- A) `X = 5`. For `avg(Y,3,5)` it answers `Y = 7`.
- B) `X = 5`. For `avg(Y,3,5)` it gives no `Y = 7`, usually an error.
- C) `X = (7+3)/2`, a term. For `avg(Y,3,5)` it answers `Y = 7`.
- D) `X = (7+3)/2`, a term. For `avg(Y,3,5)` it gives no `Y = 7`, usually an error.

> [!success]- Answer
> **B.** `Y = 7`, `Z = 3` are bound by unification with the head. `is` evaluates the right side: $(7+3)/2=5$.
> In `avg(Y,3,5)` the right side contains the unbound `Y`. `is` only evaluates, it does not solve for `Y`. Pure logic would allow `Y = 7`; this is why the slide calls `is` a kludge.
> - A: `is` works in one direction only, from right to left.
> - C: Both slips of A and D together.
> - D: `is` evaluates the arithmetic. Only `=` would bind `X` to the unevaluated term.

### Q90 (inference deck, slide 44)

The two clauses are `append([],Y,Y).` and `append([X|L],Y,[X|Z]) :- append(L,Y,Z).`, in this order. How many answers does `append(A,B,[1,2,3])` give, and which comes first?

- A) 3 answers. First: `A = [1,2,3]`, `B = []`.
- B) 4 answers. First: `A = [1,2,3]`, `B = []`.
- C) 3 answers. First: `A = []`, `B = [1,2,3]`.
- D) 4 answers. First: `A = []`, `B = [1,2,3]`.

> [!success]- Answer
> **D.** There is one answer for each place where the list can be cut: before 1, after 1, after 2, after 3. That is 4.
> Clause 1 is tried first and matches at once: `A = []`, `B = [1,2,3]`. Then `[1]`/`[2,3]`, `[1,2]`/`[3]`, `[1,2,3]`/`[]`.
> - A: Both slips of B and C together.
> - B: The clauses are tried from top to bottom, so the empty first list comes first, not last.
> - C: A list of 3 elements has 4 cut places, including both ends.

## 18. Resolution in FOL (inference deck, slides 45-48)

### Q91 (inference deck, slide 45)

Resolve $\neg\mathit{Likes}(x,\mathit{IceCream})\lor\mathit{Happy}(x)$ with $\mathit{Likes}(\mathit{Ece},y)\lor\neg\mathit{Sweet}(y)$. What is the resolvent?

- A) $\mathit{Happy}(\mathit{Ece})\lor\neg\mathit{Sweet}(\mathit{IceCream})$
- B) $\mathit{Happy}(x)\lor\neg\mathit{Sweet}(y)$
- C) $\neg\mathit{Happy}(\mathit{Ece})\lor\mathit{Sweet}(\mathit{IceCream})$
- D) $\mathit{Happy}(\mathit{Ece})\lor\neg\mathit{Sweet}(y)$

> [!success]- Answer
> **A.** Unify $\mathit{Likes}(x,\mathit{IceCream})$ with $\mathit{Likes}(\mathit{Ece},y)$: $\theta=\{x=\mathit{Ece},\ y=\mathit{IceCream}\}$. Remove the pair, join the rest, apply $\theta$.
> In words: if ice cream is sweet, Ece is happy.
> - B: $\theta$ was not applied to the remaining literals.
> - C: The remaining literals keep their signs. Only the complementary pair is removed.
> - D: $\theta$ must be applied to all remaining literals, also to those of the second clause.

### Q92 (inference deck, slide 46)

Convert to CNF: "it is not true that every student has a kind friend", $\neg\forall x\ [\mathit{Student}(x)\Rightarrow\exists y\ (\mathit{Friend}(x,y)\land\mathit{Kind}(y))]$. Which clauses result ($S_1$, $S_2$, $F$ are Skolem symbols)?

- A) $\neg\mathit{Student}(x)\lor\mathit{Friend}(x,F(x))$ and $\neg\mathit{Student}(x)\lor\mathit{Kind}(F(x))$
- B) $\mathit{Student}(S_1)$ and $\neg\mathit{Friend}(S_1,S_2)\lor\neg\mathit{Kind}(S_2)$
- C) $\mathit{Student}(S_1)$ and $\neg\mathit{Friend}(S_1,y)\lor\neg\mathit{Kind}(y)$
- D) $\mathit{Student}(x)$ and $\neg\mathit{Friend}(x,y)\lor\neg\mathit{Kind}(y)$

> [!success]- Answer
> **C.** Remove $\Rightarrow$, then move $\neg$ inwards: $\neg\forall x$ becomes $\exists x\,\neg$, and $\neg\exists y$ becomes $\forall y\,\neg$:
> $\exists x\ [\mathit{Student}(x)\land\forall y\ (\neg\mathit{Friend}(x,y)\lor\neg\mathit{Kind}(y))]$.
> $\exists x$ is inside no $\forall$: Skolem constant $S_1$. Drop $\forall y$.
> - A: This is the CNF of the sentence without the outer $\neg$.
> - B: After moving $\neg$ inwards, $y$ is universal. It must stay a variable.
> - D: $\neg\forall x$ becomes $\exists x$, so $x$ needs a Skolem constant, not a free variable.

### Q93 (inference deck, slide 47)

Convert "between two numbers there is always a third" to CNF: $\forall x\ \forall y\ [\mathit{Less}(x,y)\Rightarrow\exists z\ (\mathit{Less}(x,z)\land\mathit{Less}(z,y))]$.

- A) $\neg\mathit{Less}(x,y)\lor\mathit{Less}(x,C)$ and $\neg\mathit{Less}(x,y)\lor\mathit{Less}(C,y)$
- B) $\neg\mathit{Less}(x,y)\lor\mathit{Less}(x,M(x,y))$ and $\neg\mathit{Less}(x,y)\lor\mathit{Less}(M(x,y),y)$
- C) $\neg\mathit{Less}(x,y)\lor\mathit{Less}(x,M(x))$ and $\neg\mathit{Less}(x,y)\lor\mathit{Less}(M(x),y)$
- D) $\neg\mathit{Less}(x,y)\lor\mathit{Less}(x,M(x,y))\lor\mathit{Less}(M(x,y),y)$, one clause

> [!success]- Answer
> **B.** Remove $\Rightarrow$: $\neg\mathit{Less}(x,y)\lor\exists z\,(\dots)$. $\exists z$ is inside $\forall x$ and $\forall y$, so $z$ becomes $M(x,y)$.
> Distribute $\lor$ over $\land$: $(a\land b)\lor c$ becomes $(a\lor c)\land(b\lor c)$. Two clauses.
> - A: A constant $C$ says that one fixed number lies between every pair $x<y$. That is false.
> - C: $z$ depends on both $x$ and $y$. The Skolem function needs all enclosing universal variables.
> - D: The $\land$ inside the bracket was turned into $\lor$. Distribution keeps it as two clauses.

### Q94 (inference deck, slide 48)

KB: $\forall x\ \exists y\ \mathit{Mother}(y,x)$ and $\forall x\ \forall y\ \mathit{Mother}(x,y)\Rightarrow\mathit{Parent}(x,y)$. Clauses: (1) $\mathit{Mother}(m(x_1),x_1)$ with the Skolem function $m$, (2) $\neg\mathit{Mother}(x,y)\lor\mathit{Parent}(x,y)$, (3) the negated query $\neg\mathit{Parent}(w,\mathit{Ali})$. Resolve (3) with (2), then the result with (1). Which value of $w$ does the proof find?

- A) $w=m(x_1)$
- B) $w=M$, a Skolem constant
- C) $w=\mathit{Ali}$
- D) $w=m(\mathit{Ali})$

> [!success]- Answer
> **D.** (3) with (2): $\theta=\{x=w,\ y=\mathit{Ali}\}$, resolvent (4) $\neg\mathit{Mother}(w,\mathit{Ali})$.
> (4) with (1): unify $\mathit{Mother}(w,\mathit{Ali})$ with $\mathit{Mother}(m(x_1),x_1)$: $x_1=\mathit{Ali}$, $w=m(\mathit{Ali})$. Empty clause.
> So $w=m(\mathit{Ali})$: "the mother of Ali".
> - A: The binding $x_1=\mathit{Ali}$ of the same unifier was not applied to $w=m(x_1)$.
> - B: This Skolemizes $y$ as a constant $M$, so clause (1) becomes $\mathit{Mother}(M,x_1)$. But $y$ is inside $\forall x$ and needs the Skolem function $m(x)$. A constant would say that one person is the mother of everybody.
> - C: This takes the constant of the query. In $\mathit{Mother}(w,\mathit{Ali})$ the child is $\mathit{Ali}$; $w$ stands opposite $m(x_1)$, not opposite $x_1$.
