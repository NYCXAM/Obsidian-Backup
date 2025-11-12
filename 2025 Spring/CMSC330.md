## Exam1:
#### OCaml Data and Typing:
`::` vs `@` -> `element :: [list]` vs `['a] @ ['a]`

In OCaml, all values are expressions but not all expressions are values.
All expressions also have a data type.

Bool -> `&&, ||, not`
Int -> `+, -, *, mod, /`
Folat -> `+., -., mod., /.`
String -> `^`

|     |                             |
| --- | --------------------------- |
| =   | Structural equality test.   |
| <>  | Structural inequality test. |
| ==  | Physical equality test.     |
| !=  | Physical inequality test.   |

#Examples 
`fun a b c d -> if c || d then b *. b else if c then b else a`
1. Since `c` and `d` use `||` so they must be bool. and `b` uses `*.` so it must be float.
2. Check the if statement, if true, return a float, so it must also return a float for false. so a must be float.
3. (float -> float -> bool -> bool -> float)

`fun b d c a -> ((not d) || (d && d)) || a -. b = if c then b else a`
1. since d uses not, ||, and &&, d must be bool.
2. since it used `if c`, then c must be bool
3. since the return for true is b, false is a, b and a must be the same type
4. since it used `-.` for a and b, a and b must be float.
5. (float -> bool -> bool -> float -> float)

`fun a b c d e -> (e :: d, c a b)`
1. it used `::` to connect e and d, so d is a `'e list` and e is an element of type `e`.
2. it used c as a function, so c is a function that take in a and b. 
3. ('a -> 'b -> ('a -> 'b -> 'k) -> 'e list -> 'e -> 'e list * 'k)

`fun a b c d e f g -> g > f || if e < d then not c else b > a`
1. it used not c so c must be bool
2. it used > or < between g f, e d, b a. so g f is the same type, e d is the same type, b a is the same type
3. ('a -> 'a -> bool -> 'd -> 'd -> 'f -> 'f -> bool)

`(int -> bool) -> int -> bool list`
fun a b -> [a (b+1) && true]

`('a list -> 'b) -> 'a -> 'b -> 'a -> 'b`
fun f a b c -> if true b else f [a; c]

`('a -> 'a list) -> ('a -> 'a) -> 'a -> 'a * 'b list -> 'a`
fun a b c (d,e) -> 
	if b c = c then 
		List.hd(c::(a d)) 
	else if e@e = e then 
		c else d

#### Higher Order Functions:

**Map**: Applies a function to each item in a structure to change node's value **without changing the structure itself**.
```OCaml
let rec list_map f l =
	match l with
	| [] -> []
	| h::t -> (f h)::(map f t)
```

**Fold:** Applies a function to each item in a structure and combine the value to the accumulator, it can change the shape of structure.
```OCaml
let rec tree_fold f a t =
	match t with
	| Leaf -> a
	| Node(l, x, r) -> f x (fold_tree f a t) (fold_tree f a t)
```


#Examples 
`list_sum_product list`
	Type: `int list -> int * int * bool`
	Description: Write a function that takes in an `int list` and returns an `int * int * bool` tuple of the following form:
		- The first element is the sum of the even indexed element
		- The second element is the product of the odd indexed element
		- The third element is a boolean that will be true if the sum and the product are equal, otherwise false.

```OCaml
let list_sum_product list =
	let (sum, prod, count) = 
		fold(fun (sum, prod, count) elem ->
			if count mod 2 = 0 then
				(sum + elem, prod, count + 1)
			else
				(sum, prod * elem, count + 1)
		) (0,1,0) list
	in

	(sum, prod, sum=prod)
```

#### Imperative OCaml
In OCaml, states are immutable once created (updates create a new state)
While variables are immutable, it has references, mutable fields, and arrays which are mutable

**References:**
Three reference operators
1. `ref`
	TypeL `'a -> 'a ref`
	Allocates a reference
	`ref <expression>`
	
	- Evaluates `<expression>` to a value `v`
	- Allocates a new location in memory to hold `v`
	- Stores `v` in that location
	- Returns that location (type `a ref`)

2. `!`
	Type: `'a ref -> 'a`
    Reads a reference
    `!<expression>`
    
    - Evaluates `<expression>` to a _location_
    - Returns contents of memory at that location

3. `:=`
   Type: `'a ref -> 'a -> unit`
	Changes a value stored in a reference
    `e₁ := e₂`
    - Evaluates e₂ to a value v₂
    - Evaluates e₁ to a _location_
    - Store v₂ in contents of memory at that _location_
    - Return `unit ()`

**Sequences:**
Syntax:
	`<expression 1> ; <expression 2>`
	- Evaluates `<expression 1>` to a value $v_1$
	- Evaluates `<expression 2>` to a value $v_2$
	- Returns $v_{2}$
		- Note that this throws away $v_{1}$ - so `<expression 1>` is only ever useful if it has side effects
		`let x = print_string "a" ; 3`

#Examples 
```ocaml
let f b =
     let a = ref 0 in
         a := !a + b; !a
         in (f 5) + (f 5)
```
Answer = 10

```OCaml
let f =
     let a = ref 0 in
         fun b -> a := !a + b; !a
         in (f 5) + (f 5)
```
Answer = 15

```OCaml
let rec f a =
    let x = ref "hi" in
        fun b -> a := !x ^ b; 4
```
Answer = `string ref -> string -> int`

```OCaml
let rec f (x::xs) b = 
    x := !b + !x
```
Answer = `int ref list -> int ref -> unit`


**Functions with Imperative**

- First consider two simple examples:

```OCaml
    let x = print_int 5;;
    (* expression x is not a function, it has type unit *)
    (* after the first call, x has the value (), so future calls won't execute the print, will only return () *)
    x;;
    x;;
    x;;
    
    let y _ = print_int 5;;
	(* this is a function called y and takes any arguments, then calls print 5 *)
    let y = fun _ -> print_int 5;;
    (* same thing, different way to write it *)
    
    (* Will not print 5 - the value is the whole function *)
    y;;
    y;;
    y;;
    
    (* Will print 5 - the function is excuted bc it's given an argument *)
    y ();;
    y 4;;
    y "hehe";;
```

#### Property Based Testing:
The idea that we can test code based on properties they must uphold rather than specific inputs

Eg: A function that reverse a list -> if I do `reverse (reverse list)` then the result should be the same as `list`

When given questions regarding PBT, there are 3 aspects that we must consider:
    - the validity of the **property itself** in describing what the function is supposed to do (the correct implementation of the function would uphold the property)
    - the validity of the **implementation** of the property (implementation of property represents property)
    - whether the implementation of the **function** upholds the property (the code that may or may not be buggy will always return true when tested against the property)
    - 
These are mostly independent of each other! Consider we are given 4 elements:
    - The property `p`
    - The goal of a function `f` (what `f` is supposed to do)
    - The implementation of `f`, which we can call `I(f)` (the actual code written in an attempt to obtain `f` -> may or may not be buggy)
    - The implementation of `p`, which we can call `I(p)`

When asked about the validity of the property, we only consider `p` and `f`

When asked about the validity of the implementation of the property, we only consider whether `I(p)` matches `p

When asked whether the implementation of the code upholds the property (or will the property "catch bugs"), we only consider how `p` correlates to `I(f)`

#### RegEx:
A pattern that describes a set of strings
Defines a regular language, which can be created from a finite state machine
- Creating regular expressions
    - `ab` -> a **and** b
    - `a|b` -> a **or** b
    - `[abc]` -> a **or** b **or** c
    - `[^abc]` -> anything except a, b, c
    - `[a-z]` -> all lowercase letters
    - `[A-Z]` -> all uppercase letters
    - `[0-9]` -> every digit from 0 to 9
        - Observe, `[r1-r2]` is a range specification**What does [A-z] match?**
    - `(cs|ece)` -> capture "cs" **or** "ece"
        - **What if we did "[cs|ece]"?**
    - `.` -> any character
    - `*` -> zero or more repetitions of the preceding character or group
    - `+` -> one or more repetitions of the preceding character or group
    - `?` -> zero or one repetitions of the preceding character or group
    - `{n}` -> exactly n repetitions of the preceding character or group
    - `{m, n}` -> at least m and at most n repetitions of the preceding character or group
    - `{n,}` -> at least n repetitions of the preceding character or group}
    - `\s` -> matches to any whitespace
    - `\d` -> matches to any singular numeric character. Equivalent to `[0-9]`
    - `\w` -> matches to any alphanumeric character including underscore. Equivalent to `[A-Za-z0-9_]`
    - `^` -> begins with
    - `$` -> ends with

- Remember to escape special characters
    - Ex. `\(`, `\)`, `\.`, `\?`, `\+`, `\/`, `\*`, etc.

- Commonly made mistakes:
    - Using `[]` instead of `()`
    - `(hi|bye)` - "match 'hi' or 'bye'"
    - `[hi|bye]` - "match 'h' or 'i' or '|' or 'b' or 'y' or 'e'"
    - Forgetting how `|` scoping works
    - Forgetting how `*` and `+` scoping works
    - Forgetting that `^` works differently inside `[]` and outside
    - Forgetting to escape `.` and `(` and `)` and `\` and `*` and `+`
    - Trying to be clever about ASCII values\

#### Finite State Machines, NFA-DFA:
FSMs can be represented by a 5-element tuple:
    - **Set** of all **possible states**
    - A **starting state** — There can be only 1 starting state
    - **Set** of **final or accepting states**
    - **Set** of **transitions** — (initial_state, letter from alphabet, ending_state)
    - **Alphabet** — set of all symbols that representing valid transitions

Note all RegEx can be represented as FSMs and vice versa


Differences between DFA and NFA:
- **DFA** (Deterministic Finite Automata)
    - Exact output state from any given input state and symbol is pre-_determined_.
    - This means at any given state with any given input, there is **no ambiguity** as to what state comes next.
        - DFAs **cannot have explicit $\epsilon$ transitions**. If they did, then it would be ambiguous whether the machine should be at the current state or at a state it can lead to using an epsilon transition.
        - DFAs **cannot have more than one transition with the same symbol out of the same state**. If they did, then it would be ambiguous which path the machine should follow when given that symbol as input.
- **NFA ( Nondeterministic Finite Automata )**
    - The exact output state from any given input state and symbol may **not** be pre-determined.
    - This means at any given state with any given input, there **may be ambiguity** as to what state comes next.
    - NFAs are considered a superset of DFAs. That is, **every DFA is also considered an NFA, but not every NFA is a DFA**.
        - In short, "All DFAs are NFAs, but not all NFAs are DFAs".

E-Closure:
	Take as input an NFA and the current state
	Return the set of all the states that we can visit from the current state using any number of $\epsilon$ transitions (including 0)

Move:
	Take as input an NFA, the current state, and a symbol
	Return the set of all the states that we can visit from the current state using exactly one transitions on that symbol.


**Converting NFA to DFA:**
Conversion algorithm:

| As seen in disucussion 5              | In other words...                                                                                                                                                                                    |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Input: NFA(Σ,Q,q0,Fn,δ)               | Input: NFA                                                                                                                                                                                           |
| Output: DFA(Σ,R,r0,Fd,δ′)             | Output: DFA                                                                                                                                                                                          |
| Let r0 = ε-closure(δ,q0), add it to R | Let the starting state of the DFA (r0) equal the ε-clousre of the starting state of the NFA (q0). Add r0 to the list of states R.                                                                    |
| While ∃ an unmarked state r∈R:        | Process every state r in the list of states R.                                                                                                                                                       |
| Mark r                                | We are now processing r. Mark it as having been processed.                                                                                                                                           |
| For each σ∈Σ                          | For each symbol σ in the alphabet:                                                                                                                                                                   |
| Let E=move(δ,r,σ)                     | Call _move_ on the currently-being-processed state r and the current symbol σ. Name the result E.                                                                                                    |
| Let e=ε-closure(δ,E)                  | Call _ε-clousre_ on E. Name the result e.                                                                                                                                                            |
| If e∉R then let R=R∪e                 | Add e to the set of states R. R is a set, and should not have duplicates. Also, the empty list does not need to be added to R.                                                                       |
| Let δ′=δ′∪{r,σ,e}                     | Add the transition (r,σ,e) to the DFA's set of transitions.                                                                                                                                          |
| Let Fd={r∣∃s∈r with s∈Fn}             | Once you have processed every state in R, get the list of final states Fd for the DFA. These are the set of states from R which have a non-zero intersection with the final states of the input NFA. |