**Admissible:**
A heuristic $h(n)$ is admissible if $h(n) \leq h^*(n)$, where $h^*(n)$ is the true optimal cost from $n$ to the goal, meaning it never overestimates the remaining cost

**Consistent:**
A heuristic $h(n)$ is consistent if for every node $n$ and every successor $n'$, $h(n)\leq \cos t(n, n') + h(n')$ and also $h(goal) = 0$, meaning the heuristic follow triangle inequality

**Rational:**
- Goals are expressed in terms of the utility of outcomes
- World is uncertain, so we'll use expected utility
Being rational means acting to maximize your expected utility

**Reflex:**
- Acts based only on the current percept
- Follows condition-action rules, such as "if apple: move forward"
## Lecture 3:
Planning: Finding a sequence of actions that leads from an initial state to a goal state
- Complete planning: guaranteed to find s solution if one exist
- Optimal planning: Guaranteed to find the best solution if one exist
Search (in planning): systematically exploring states or actions to find a sequence of steps (plan) that leads to a goal

 A search problem consists of:
 - A state space
 - A successor function (with actions and costs)
 - A start state
 - A goal test

**State Space Graph:**
Def: A mathematical representation of a search problem
- Nodes are (abstracted) world configurations
- Arrows represent successor
- The goal test is a set of goal nodes (maybe only one)
- In a state space graph, each state occurs only once
![[Pasted image 20260207215652.png]]
**Search Tree:**
- A "what if" tree of plans and their outcomes
- The start state is the root node
- Children correspond to successors
- Nodes show states, but correspond to plans that achieve those states

**DFS Properties:**
- If m is finite, takes time $O(b^m)$, where m is the maximum depth of the search tree
- Takes $O(bm)$ space
- Could be complete if we prevent cycles
- Not optimal, finds the "leftmost" solution
**BFS Properties:**
- If s is finite, takes time $O(b^s)$, where the depth of shallowest solution be s
- Takes $O(b^s)$ space
- s must be finite if a solution exists, so yes
- Optimal only if costs are all 1
**Uniform Cost Search:**
Def: expand a cheapest node first. fringe is a priority queue
How does UCS expand:
- Processes all nodes with cost less than cheapest solution
- If that solution cost C* and arrows cost at least $\epsilon$, then the "effective depth" is roughly C*/$\epsilon$
- Takes time $O(b^{C^*/\epsilon})$
- Take $O(b^{C^*/\epsilon})$
- Assuming best solution has a finite cost and minimum arrow cost is positive, yes
- Is optimal
**Bidirectional Search:**
Def: Instead of only searching from the start state, we search from both start and goal state, and meet in the middle


## Lecture 4: Informed Search
Pervious search algos are all uninformed search:
- Use only the information given in the problem definition
- No heuristics
- Examples: BFS, DFS, UCS, iterative deepening
Informed Search:
- Uses extra knowledge (a heuristic)
- Estimates closeness to goal
- Examples: Greedy best-first, A*, hill-climbing

**Heuristic:**
Def: A function that estimate how close a state is to a goal, designed for a particular search problem 

Idea: Greedy Search
Expand a node that seems closest to the goal, let $g(n) =$ cost from the start node to $n$, $h(n) =$ heuristic estimate from $n$ to the goal. $f(n) = g(n) + h(n)$. We call this A* search

- Inadmissible heuristics break optimality by trapping good plans on the fringe
- Admissible heuristics slow down plans but never outweight true costs

A heuristic $h$ is admissible if: $0 \leq h(n) \leq h^*(n)$ where $h^*(n)$ is the true cost to a nearest goal

When should A* terminate?
Loop:
1. Select a node
2. Goal test (Only terminate when test = True here, even if the goal node is seen earlier, goal test is only run when the node is selected)
3. Expand

## Lecture 10:

**Entailment:** 
- $KB\models\alpha$
- Means: In every possible scenario (model) where the $KB$ is true, the sentence $\alpha$ must also be true.
- Focus: It doesn't care how you find the answer, only cares that the answer is a mathematical necessity based on the facts.
**Derivation:**
- $KB\vdash_i\alpha$
- Means: There exists a specific algorithm or set of inference rules that can mechanically manipulate the symbols in the $KB$ to produce the sentence $\alpha$
- Focus: It is entirely about the process.

**Decidable:** The procedure is guaranteed to halt with an answer (T or F)
**Soundness:** Procedure i is sound if whenever $KB\vdash_i\alpha$, it is also true that $KB\models\alpha$; unsound procedures can conclude untrue statements: If $KB\vdash_i\alpha$ then $KB\models\alpha$
**Completeness:** $i$ is complete if whenever $KB\models\alpha$, it is also true that $KB\vdash_i\alpha$; a complete procedure is able to derive any sentence that is entailed. i.e., procedure will answer any question whose answer follows from what is known by the $KB$. If $KB\models\alpha$ then $KB\vdash_i\alpha$ 

**Model:** In logic, a model is simply a "possible world", meaning a specific assignment of True of False to every variable in your system. 
**Valid (Tautology):** True in all possible models. No matter what the facts are, this sentence can not be false. 
**Satisfiable:** True in at least on model, it could be true given the right circumstances.
**Unsatisfiable (Contradiction):** True in zero models, it is mathematically impossible for this to be true. 
**Deduction Theorem:** $KB \models\alpha$ if and only if the statement ($KB\to \alpha$) is a valid sentence. $KB\models\alpha \iff M(KB)\subseteq M(\alpha)$

**Logical inference is a kind of search:**
- State: The facts of the knowledge base (KB)
- "Search": Inference via Entailment: $KB\models\alpha$ (The Deduction Theorem)
- Successor Function: Application of the possible rules of inference.

## Propositional Logic:
Syntax: Elements of the language:
- Primitive propositions (statements like):
	- Bob loves Alice -> P
	- Alice loves Bob -> Q
- Compound Propositions:
	- Bob loves Alice and Alice loves Bob -> $P \wedge Q$
Connectives: 
- $\neg$ not
- $\wedge$ and
- $\vee$ or
- $\to$ implies
- $\leftrightarrow$ equivalent (iff)

**Various Proof Methods:**
- Model Checking ($\models$, proof as SAT):
	- Truth table "inference by enumeration" (exponential in n)
	- Improved backtracking (still searching over assignments)
	- Heuristic search of assignments in model space (sound but incomplete, much like CSP solving)
- Application of inference rules ($\vdash$, proof as search):
	- Proof is a sequence of rule applications
	- Easier to do if logics are in structured form

**Pros and Cons of Propositional Logic:**
Pro:
- Propositional logic is declarative: pieces of syntax correspond to facts
- Propositional logic allows partial/disjunctive/negated information (unlike most data structures and databases)
- Propositional logic is compositional: meaning of $B_{1,1} \wedge P_{1,2}$ is derived from meaning of $B_{1,1}$ and of $P_{1,2}$
- Meaning in propositional logic is context-independent (unlike natural language, where meaning depends on context)
Con:
- Propositional logic has very limited expressive power (unlike natural language) E.g., can't say "all houses have a color" except by writing one sentence for each

## First-Order Logic
Unlike propositional logic assumes world contains only facts, first-order logic (like natural languages) assumes the world contains:
- Objects: People, house, numbers...
- Relations: Red, bigger than, brother of...
- Functions: Father of, best friend, end of...
![[Pasted image 20260330023336.png]]



|                    | Propositional                          | First Order Logic                                                                 | Horn Clause Logic                                                                             |
| ------------------ | -------------------------------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| Ontology           | Fact(P, Q)                             | Objects, Relations, Variables                                                     | Herbrand universe (constants, functions, relations); closed world assumption                  |
| Syntax             | Atomic sentences + Connectives         | Connectives, Quantifiers, Constants, Variables, Predicates & Relations, Functions | Definite clauses: $A \leftarrow B_1,\dots,B_n$                                                |
| Semantics (Models) | Truth Tables                           | Much more complex (binding of objects)                                            | Least Herbrand model; minimal model making all clauses ture                                   |
| Inference          | DPLL, GSAT, WalkSAT for model checking | Unification and Resolution; automated theorem provers                             | Selective Linear Definite clause resolution (SLD-resolution) via unification and backtracking |
| Complexity         | NP-Complete                            | Semi-Decidable                                                                    | Polynomial-time for Horn-SAT potentially exponential in procedural search                     |
**Syntax of FoL:**
![[Pasted image 20260330024900.png]]
**Possible worlds:**
A possible world for FoL consists of:
- A non-empty set of objects
- For each k-ary predicate in the language, a set of k-tuples of objects (i.e., the set of tuples of objects that satisfy the predicate in this world)
- For each k-ary function in the language, a mapping from k-tuples of objects to objects
- For each constant symbol, a particular object (can think of constant as 0-sary functions)

## The Horn Clause:
A Horn clause is a disjunction of literals of which at most one is positive, Mathematically, that looks like this:

$$\neg P_1 \lor \neg P_2 \lor \neg P_3 \lor C$$

Using De Morgan's Laws, we can factor out the negatives:

$$\neg (P_1 \land P_2 \land P_3) \lor C$$

By the definition of logical implication ($\neg A \lor B \equiv A \implies B$), this becomes:

$$(P_1 \land P_2 \land P_3) \implies C$$
E.g., $\{\neg \text{lawyer}(x),\text{rich}(x)\}$ or rich(x) :- lawyer(x)
means only one of the two can be true, if $\neg \text{lawyer}(x)$ is true, then $\text{rich}(x)$ is false; if $\text{rich}(x)$ is true, then $\neg \text{lawyer}(x)$ is false

Hence, every Horn clause is an implication whose premise is a conjunction of positive literals w/ a conclusion that is a single positive literal, e.g., noted as $C :- P1, P2, P3, ..., Pn$
Horn clauses with exactly one positive literal are called definite clauses.

A definite clause with no negative literals simply asserts a given proposition - called a fact. 