There are several common and useful equations (or formulas) for calculating summations. These are frequently encountered in algebra, calculus, computer science (especially in algorithm analysis), and other areas of mathematics.

Here are some of the most common ones:

1.  **Sum of a Constant:**
    If $c$ is a constant, then:
    $$\sum_{i=1}^{n} c = c \cdot n$$
    * **Explanation:** You are adding the constant $c$ to itself $n$ times.

2.  **Sum of the First $n$ Integers (Arithmetic Series):**
    $$\sum_{i=1}^{n} i = 1 + 2 + 3 + \dots + n = \frac{n(n+1)}{2}$$
    * **Explanation:** This formula sums the first $n$ positive integers. It's a specific case of an arithmetic series.
3.  **Finite Geometric Series:**
    For a series $a + ar + ar^2 + \dots + ar^{n-1}$ (which has $n$ terms, or summing $ar^k$ from $k=0$ to $n-1$):
    $$\sum_{k=0}^{n-1} ar^k = a \frac{r^n-1}{r-1}$$
    Or, if summing $ar^k$ from $k=0$ to $n$:
    $$\sum_{k=0}^{n} ar^k = a \frac{r^{n+1}-1}{r-1}$$


**Properties of Summation (Linearity):**

These are not formulas for specific series but are rules for manipulating summations:

* **Sum/Difference Rule:**
    $$\sum_{i=m}^{n} (a_i \pm b_i) = \sum_{i=m}^{n} a_i \pm \sum_{i=m}^{n} b_i$$
* **Constant Multiple Rule:**
    If $c$ is a constant:
    $$\sum_{i=m}^{n} c \cdot a_i = c \sum_{i=m}^{n} a_i$$

These formulas and properties are foundational for working with series and sequences. Knowing them can greatly simplify many mathematical problems.

**Nested Sum:**
last_index - first_index + 1, so
$$
\begin{align}
\sum_{i=0}^{n-2}\sum_{j=i+1}^{n-1}a &= \sum_{i=0}^{n-2}a((n−1)−(i+1)+1)\\
&= \sum_{i=0}^{n-2}a(n-i-1) \text{ Then you can seperate the sum}\\
&= \sum_{i=0}^{n-2}an - \sum_{i=0}^{n-2}ai - \sum_{i=0}^{n-2}a \\
&= \dots
\end{align}

$$

- [ ] Recurrence Relation (dig down)
      24s q10
- [x] Master Theorem
- [x] Time complexity for each algo
- [x] Summation equations
- [x] P&NP
- [x] Binary Search