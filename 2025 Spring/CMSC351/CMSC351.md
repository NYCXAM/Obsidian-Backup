## Lecture 1 01/29
### Coin Change 
**Greedy: $O(n^2)$**
		- Always try the biggest coin first then go to the next biggest one until find the solution
		- Fast but **don't guarantee the best solution:**
		  Have: 25,10,1 Need:330
		  Greedy: 25, 1, 1, 1, 1, 1
		  Best: 10, 10, 10
**DP: O(n)**
		- Always give the best solution but slower than greedy
		- Make a memory table for later use,
		eg. 	We have 25, 10, 5, 1

| Target          | 1   | 2   | ... | 10  | 11  | ... | 20  | 29  | 30  |
| --------------- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| # Coin          | 1   | 2   | ... | 1   | 2   | ... | ?   | ?   | ?   |
| $ of First coin | 1   | 1   | ... | 10  | 10  | ... | ?   | ?   | ?   |
Find 20:
	1. 20 > 25 thus use_25 == False
	2. 20 = 2 * 10 thus lookup pervious found solution for 10 in table -> 10: 1
	3. Given 20 = 2 * 10, and we can't use 25, solution is 2 * table.get(10)
Find 29:
	

In exam:
given x, y. Ask for the minimal coins needed for x + n or y + n



## Lecture 3 2/3
Notation:
- Big $O(n)$[:](https://www.youtube.com/watch?v=7NC-iyZ7vpQ) Upper bound for the time complexity
	$T(n)$ is $O(f(n))$ if and only if there exists positive constants, $n_0$ and $c$, such that:
	$T(n) \leq C f(n)$ for any $n \geq n_0$

- Big $\Omega(n)$[:](https://www.youtube.com/watch?v=BeDnyutk2-E&t=624s) Lower bound of the time complexity
	$T(n)$ is $\Omega(f(n))$ if and only if there exists positive constants, $n_0$ and $B$, such that
	$T(n) \geq B f(n)$ for any $n \geq n_0$ 
	
- Big  $\Theta(n)$: Two non-negative functions $f(n)$ and $g(n)$. $f(n) = \Theta(g(n))$ if and only if $c_1 *g(n) \leq f(n) \leq c_2 * g(n)$ for all $n \geq n_0$ and $c_1, c_2$ and $n_0$ are constants.

#Examples 
- $T(n) = 3n^2 + 2n + 1$
	$O(n):$ 
	1. Let C be 6 and find the $n_o$
	2. When C = 6, $n_o = 1$, For all $n > n_o$ , $3n^2 + 2n + 1 \leq 6n^2$
	3. Thus $3n^2 + 2n + 1 = O(n^2)$ for $c = 6, n_0 = 1$
	$\Omega(n):$
	4. Let C be 3 and find the $n_o$
	5. When C = 3, $n_o = 1$, For all $n > n_o$ , $3n^2 + 2n + 1\geq 3n^2$
	6. Thus $3n^2 + 2n + 1 = \Omega(n^2)$ for $c = 3, n_0 = 1$
	$\Theta(n):$
	7. let g(n) be $n^2$, need to find $c_1$ and $c_2$
	8. $c_1:$ Notice that $3n^3 \leq 3n^2 + 2n + 1$ for $n\geq 1$
	9. $c_2:$ Notice that $6n^2 \geq 3n^2 + 2n + 1$ for $n \geq 1$
	10. Thus $3n^2 + 2n + 1 = \Theta(n^2)$ for $c_1 = 3, c_2 = 6,$ and $n_0 = 3$
## Lecture 4 2/5
**Limit theorem:**
	Suppose $f(x)$ and $g(x)$ are functions, then:
	1. $\lim_{ n \to \infty } \frac{f(n)}{g(n)} \neq \infty \to f(n) = O(g(n))$
	2. $\lim_{ n \to \infty } \frac{f(n)}{g(n)} \neq 0 \to f(n) = \Omega(g(n))$
	3. $\lim_{ n \to \infty } \frac{f(n)}{g(n)} \neq 0 \text{ and } \neq \infty \to f(n) = \Theta(g(n))$


## Lecture 5 2/7
**Maximum Contiguous Sum**:
	Brute Force $\Theta (n^2)$:
		Loop through every possible possible sub-list and find the largest result. 
	Divide and Conquer $\Theta(n\lg(n))$:
		Recursively divide the list into left and right, then compute the MCS that cross the midpoint, then return the max of Left, Right, and Center result.
	Dp: Kadne's Algorithm ($\Theta(n)$):
		Break the problem to -> for each element, to maximize the value of list[i] + buffer[i-1], should I include the current element or not? Then note the biggest value for this element to buffer list and track the biggest value so far.

| Input List  | 2   | -3  | 5   | -1  | 2   | 4   | -1  |
| ----------- | --- | --- | --- | --- | --- | --- | --- |
| Buffer List | 2   | -1  | 5   | 4   | 6   | 10  | 9   |
| int Result  | 2   | 2   | 5   | 5   | 6   | 10  | 10  |

## Lecture 6 2/10
**Bubble sort (Time: $\Theta(n^2)$ Memory: $O(1)$, Stable, In Place):**
	Loop through the list and if the current element is bigger than the next element, swap them, if same value then no action, until all element is sorted.

Pseudo-code
```
for i = 0 to n - 1
	for j = 0 to n - 2 - i
		if list[j] > list[j+1]
			swap list[j] and list[j+1]
		end
	end
return list;
```

## Lecture 7 2/12
**Selection Sort (Time:  $O(n^2)$, Memory: $\Theta(1)$, Not Stable, In Place):**
	Find the index of the smallest element and swap it with A[0]. Now A[0] is correct.
	Then find the smallest element in A[1,...,n] and swap it with A[1]. Now A[0, 1] are correct.
	Until sort A[0,..,n-1], since A[n-1] is sorted, the last element A[n] is the biggest element
Pseudocode:
```
for i = 0 to n - 2
	minindex = i
	for j = i+1 to n-1
		if A[j] < A[minindex]
			minindex = j
		end
	end
	swap A[i] with A[minindex]
end
```
Lets say the if statement take C seconds
$$
\begin{align}
T(n) &= \sum_{i=0}^{n-2}\sum_{j=i+1}^{n-1}C \\
&= C * \sum_{i=0}^{n-2}\sum_{j=i+1}^{n-1}1 \\
&= C * \sum_{i=0}^{n-2} n - 1 - (i+1) + 1 \\
&= C * \sum_{i=0}^{n-2} (n-1-i) \\
&= C * \sum_{i=0}^{n-2}(n-1) - C * \sum_{i=0}^{n-2} i \\
&= C(n-2-0+1)(n-1) - C(\frac{(n-2)(n-1)}{2}) \\
&= \Theta(n^2)
\end{align}
$$

Note that $\sum_{i=1}^{a} = \frac{a(a+1)}{2}$ Thus $\sum_{i=0}^{n-2} i = \frac{(n-2)(n-1)}{2}$

**Stable**: The order of element that have same value will keep the same, in this case, the blue 20 will remain on the left side of red 20 after the sort:
![[Pasted image 20250210212309.png]]
**In Place**: The algorithm doesn't create a new list while sorting.

## Lecture 8 2/14
**Insertion Sort:**
Worst $O(n^2)$ List is reverse sorted at start
Best $O(n)$ List is already sorted at start
Avg $O(n^2)$
Memory: $O(1)$
In place, stable
	Work from left to right, each step take 1 element on right and insert it to the correct place in left side. Which means left hand side is always sorted, each step take one element from right and add it to the sorted list.
Pseudocode:
```
for i = 1 to n-1
	key = A[i]
	j = i-1
	while j >= 0 and key < A[j]
		A[j+1] = A[j]
		j = j-1
	end
	A[j+1] = key
end
```


## Lecture 9 2/17
**Binary Search:**
Worst: $O(\log n)$ if the target is the first element checked
Best: $O(1)$ if the target is not in the list
Avg: $O(\log n)$:
	Each iteration reduce the search space by half:
	1st: $\frac{n}{2}$ elements left
	2nd: $\frac{n}{4}$ elements left
	kth: $\frac{n}{2^k}$ elements left
	For list with n elements, the average case need to process $\frac{n}{2}$ elements, so it is $\frac{n}{2} + \frac{n}{4} + \dots + \frac{n}{2^{\frac{n}{2}}}$

Pseudocode:
```
function binarysearch(A, TARGET)
	L = 0
	R = n-1
	while L <= R
		C = floor((L+R)/2)
		if A[C] == TARGET
			return C
		else if TARGET < A[C]
			R = C-1
		else if TARGET > A[C]
			L = C+1
		end
	end while
	return FAIL
end
```


## Lecture 10 2/19
**Exam:**
- [x]  Tell the difference between similar algorithm
	- Selection sort find the smallest element from the unsorted part of list and swap with the first element from unsorted list, then grow the sorted list by 1 (same as put it at the end of sorted list) **There is not swap between the sorted element**
	- Insertion sort take the first element in the unsorted list and continue swap it with the element inside sorted list until the new element find the right place. **There are swaps between sorted elements**
- [x]  Running time
- [x]  Read pseudocode, no write
	- Find the # of prints of some added print statement
- [x]  Coin change dp table
- [x] **Selection/Insertion Sort (At least 1 question)**
- [x] **Definition of $O(n) \Omega(n) \Theta(n)$**
- [ ] Prove by definition (find B/C or Limit theory)
- [ ] Formula:
	1. $\frac{d}{dx} b^x = (\ln b) b^x$
	2. $\log(2^x) = x\log2$
	3. L'H rule
	4. Arithmetic Series: $1+2+3+\dots+(n-1)+n = \frac{n(n+1)}{2}$
	5. Geometric Series: $1+a+a^2+\dots+a^{(n-1)}+a^n = \frac{a^{n-1}}{a-1}$
- [ ] Intuition $x^2 +x\log x + 4^x + 8x + 7$
- [x] T/F
	- Bubble sort is $O(n^4)$ T
	- $f \neq \Omega(g(n))$ then $f=O(g(n))$ F
- [x] Rigorous Time
	1. Analyse pseudocode
	2. Impact of conditions(for&while) / If statements
- [x] MCS
	1. Brute Force
	2. Divide and Conquer
	3. DP
- [x] Proofs
	- Induction 

## Lecture 11 2/24
#### Recurrence Relation:
Goal: We need a new tool to analyze algorithm
- Binary Search: It checks the middle (constant time) and then check the sublist that has half the length. So $T(n) = T(\frac{n}{2}) + C$
- Bobble Sort: It iterates and do $n-1$ comparison + (swap) (constant time each) and then repeats with a list of length $n-1$. So $T(n) = T(n-1) + C(n-1)$
- MCS (Divide and Conquer): It recursively calls to both halves of the list and find the straddling sum (linear) and compares all 3 (Lmax, Smax, Rmax). So $T(n) = 2T(\frac{n}{2})+cn+d$

Definition:
	A recurrence relation for a function $T(n)$ is an equation for $T(n)$ in terms of "smaller" values and pass some other functions of n

**Solving Recurrence Relation:**
Three Ideas:
1. Can we find specific $T()$ values?
2. Can we find a "closed" formula for $T(n)$? (meaning a normal function, not a RR)
3. Can we jump straight from RR -> $O, \Omega, \Theta$?

#Examples 
Specific Value
$$
\begin{align}
T(n) = T( \lfloor   \frac{n}{3} \rfloor  ) + n + 1 \text{ with } T(0)=5 \\
\text{then } T(1) &= T( \lfloor \frac{1}{3} \rfloor )+1+1 = T(0)+2=7 \\
\text{and } T(2)&=T(\lfloor  \frac{2}{3}\rfloor )+2+1 = T(0)+3=8
\end{align}
$$
#Examples 
Digging down for a formula
$\text{Suppose we have } T(n) = T\left( \frac{n}{2} \right)+3 \text{ with } T(1)=5$
$$
\begin{align}
T(n) &= T\left( \frac{n}{2} \right)+3\\
T(n) &= \left[ T\left( \frac{n}{4} \right)+3 \right]+3\\
\\
\\
T(n) &= T\left( \frac{n}{4} \right)+6\\
T(n) &= T\left[ T\left( \frac{n}{8} \right)+3 \right]+6\\
T(n) &= T(\frac{n}{8})+9\\
\end{align}
$$
What's the pattern?
$$
\begin{align}
T(n) &= T(\frac{n}{2^k})+3k\\
\text{and given that } T(1) &= 5\\
2^k = n \text{ so } k&=\log n\\
\text{Plug into the pattern } T(n)&=T(1)+3\log n\\ \\
\\
\text{so } T(n)&=5+3\log n\\
\end{align}
$$

#Examples 
$$
\begin{align}
T(n) &= T(n-1)+n \text{ with } T(0) = 3\\
T(n) &= T(n-1)+n\\
T(n) &= [T(n-2)+n-1]+n\\
T(n) &= T(n-2) + n + (n-1)\\
T(n) &= T(n-3) + n + (n-1) + (n-2)\\ \\
\dots\\
\text{In general } T(n) &= T(n-k) + n+(n-1)+(n-2)+\dots+(n-(k-1)) \\
\text{stop when we got } T(0) \\
T(n) &= T(0) + n+(n-1)+(n-2)+\dots+(1) \\
T(n) &= 3 + \frac{n(n+1)}{2} \\ \\
\\
\text{thus }T(n) &= \frac{1}{2} n^3 + \frac{1}{2} n + 3\\
\end{align}
$$

## Lecture 12 2/27
#### Recurrence Relation + Tree:
Trees for values:
	Consider this RR $T(n) = 2T(\frac{n}{3})+5n+1$ with $T(1)=7$
	Think T(9) as the only node in a tree
	![[Pasted image 20250227010644.png]]From this tree, $T(9) = 5(9)+1+2[5(3)+1]+4[7]\dots$
For unknown n, level k contains $T(\frac{n}{3^k})$ and it ends at T(1) so that's when $\frac{n}{3^k}=1$ thus $3^k=1$ thus $k=\log_3n$
![[Pasted image 20250227011534.png]]![[Pasted image 20250227012612.png]]

## Lecture 13 2/28 
#### The Master Theorem:
Purpose: Find $\Theta$ for R.R easily and quickly

Theorem:
$T(n) = a T(\frac{n}{b}) +f(n)$
where a, b $\in Z$ with $a \geq 1$ and $b \geq 2$
$f(n) \geq 0$ for large enough $n$

Then we have:
	Case 1: If $f(n) = O(n^c)$ for some $c\geq 0$ and $\log_b a > c$ then $T(n) = \Theta(n^{\log_b a})$
	Case 2: If $f(n) = \Theta(n^c)$ for some  $c \geq 0$ and $\log_b a = c$ then $T(n) = \Theta(n^{\log_b a} \log n)$
	Case 2f: If $f(n) = \Theta(n^c\log^kn)$ ... and $\log_b a =c$ then $T(n) = \Theta(n^{\log_b a}\log^{k+1}n)$
	Case 3: If $f(n) = \Omega(n^c)$ ... and $\log_b a <c$ then $T(n) = \Theta (f(n))$

#Examples 
3
Ask: $f(n) = \Theta(n^c)$ for which c? Compare $\log_b a$ and c

Eg1:
$T(n) = 8$ and $T(\frac{n}{2}) + n^2 +1$
$$
\begin{align}
f(n) &= n^2 + 1 = \Theta(n^2)\\
\text{Then } \log_b a &= \log_2 8 = 3\\
\text{Thus } T(n) &= \Theta(n^{\log_b a}) = \Theta(n^3)\\
\end{align}
$$

Eg2:
$$
\begin{align}
T(n) &= 9T(\frac{n}{3}) + n^2 +n\log\\
f(n) &= \Theta(n^2)\\
\text{Then } \log_3 9 &= 2\\
\text{Thus } T(n) &= \Theta(n^{\log_3 9} \log n)\\
\end{align}
$$
Eg3:
$$
\begin{align}\\
T(n) &= 125T(\frac{n}{5}) + n^3\log n+17\\
f(n) &= \Theta(n^3\log n)\\
\log_5 125 &= 3\\
\text{Thus } T(n) &= \Theta(n^{\log_5 125}\log^2 n) = \Theta(n^3\log^2 n)\\
\end{align}
$$

Eg4:
$$
\begin{align}
T(n) &= 5T\left( \frac{n}{25} \right)+n+1\\
f(n) &= \Theta(n)\\
\log_{25} 5 &= \frac{1}{2}\\
T(n)&=\Theta(f(n))=\Theta(n+1)=\Theta(n)\\
\end{align}
$$
#Examples 
Tricker ones:
Eg1:
$T(n) = 3T(\frac{n}{2})+\log n$

$$
\begin{align}
\text{notice }f(n) &= \Theta(\log n) \text{ apply to case 2f} \\
\text{with } f(n)  &= \Theta(n^0 \log^1 n)\\
\text{but }\log_2 3 &= 1.xxx \neq 0 \text{ whic means it doesn't meet the requirement for case 2f}\\
\text{but notice }f(n) &= \log n=O(n) \text{ which is case1}\\
\text{and } \log_2 3 &= 1.xxx > 1 \text{ which meet the requirement for case1}\\
\text{thus case 1 applies: } T(n) &= \Theta(n^{\log_2 3})\\
\end{align}
$$

Eg2:
$T(n) = 16T(\frac{n}{2})+n^3\log n$
$$
\begin{align}
\text{notice it might work for case 1: } f(n) &= n^3\log n = O(n^4)\\
\text{but } \log_{2}16 & \not> 4\\
\text{check case 3: }f(n)&=n^3\log n=\Omega(n^3)\\
\text{but } \log_{2}16 & \not<  3\\
\text{notice: }n^3\log n&=O(n^c) \text{ for any} c > 3\\
\text{so } f(n) &= n^3\log n=O(n^3.5)\\
\text{trying case 1 again: } \log_{2}16 &> 3.5 \text{ true!}\\
\text{thus: }T(n)&=\Theta(n^{\log_b a})=\Theta(n^4)\\
\end{align}
$$
Doesn't apply:
1. $T(n) = /2T(n-1)/ + \log n$
2. $T(n) = /2T\left( \frac{n}{3} \right) + T(\frac{n}{4})/ + n^2$
3. $T(n) = /1.5/T(\frac{n}{2})+n$
4. $T(n) = 16T(\frac{n}{4})+f(n) \text{ and } f(n) = O(n^2)$
   but $\log_4 16 \not> 2=c \text{ so case 1 fails}$


## Lecture 14 3/3
#### Merge Sort:
All case $T(n) = O(n\log n)$
$S(n) = \Theta(n)$
Not in-place
Stable

Intro: Merge sort is a divide and conquer sorting algorithm

The Merge Process:
	Suppose we have a divided list "A" that is sorted on each half like this: 
	[2, 3, 5, 10 | 1, 4, 7, 20] with l = 0, r = 4
1. We create a empty list "temp" with the same length and index i = 0;
2. Compare A[l] with A[r], pick the smallest and copy to the temp list. 
3. Increase i, then increase l or r based on which side was the bigger element in. Repeat.
```
1.
A: [2, 3, 5, 10 | 1, 4, 7, 20]
	^             ^
temp: [1], i = 0

2.
A: [2, 3, 5, 10 | 1, 4, 7, 20]
	^                ^
temp: [1, 2], i = 1

3.
A: [2, 3, 5, 10 | 1, 4, 7, 20]
	   ^             ^
temp: [1, 2, 3], i = 2

...

End.
A: [2, 3, 5, 10 | 1, 4, 7, 20]
              ^             ^
temp: [1, 2, 3, 4, 5, 7, 10, 20], i = 7
```
Note:
	If we finish one side first, copy the rest of other side to temp (since each side is sorted)
	Time (for this step, not the algo): $\Theta(n)$
	Space (for this step, not the algo): $\Theta(n)$

Pseudocode:
```
function Merge(arr, start1, end1, start2, end2)
	temp = new array of same size as arr
	l = start1, r = start2, i = start1

	//main merge part
	while l <= end1 and r <= end2
		if arr[l] <= arr[r]
			temp[i] = arr[l];
			l++, i++;
		else
			temp[i] = arr[r];
			r++, i++;
		end if
	end while

	//if one side finished first,
	//copy the remains of others
	while l <= end1
		temp[i] = arr[l];
		l++, i++;
	end while
	while r <= end2
		temp[i] = arr[r]
		r++, i++;
	end while

	//copy temp back to arr
	for j = start1 to end 2 inclusive
		arr[j] = temp[j]\
	end for
end function
```

Pseudocode
```
function MergeSort(arr, start, end)
	if start < end
		//find the middle
		middle = (start+end) / 2                     <- Theta(1)
	
		//apply mergesort to each half
		MergeSort(arr, start, middle)                <- T(1/2)
		MergeSort(arr,middle+1,end)                  <- T(1/2)
	
		//merge the two halves back on top of arr
		Merge(arr,start,middle,middle+1,end)         <- Theta(n)
	end if
end function
```

Time Complexity:
	Suppose it take time $T(n)$ for a list of length n. Then we have:
$$
\begin{align}
T(n)&=\Theta(1)+2T\left(  \frac{n}{2} \right) + \Theta(n) \text{ if } n>1\\
T(1) &= \Theta(1)\\
T(n) &= 2T\left( \frac{n}{2} \right)+\Theta(n)+\Theta(1)\\ \\
\text{Think: } T(n)&=2T(\frac{n}{2}) + f(n)\\
\text{By Master Theorem: } f(n) &= \Theta(n')\\
\log_b a &= \log_2 2 = 1 \text{ so } c=1\\
&\text{So case2}\\
T(n) &= \Theta(n^{\log_b a}\log n) = \Theta(n^1 \log n) = \Theta(n\log n)\\
\end{align}
$$
Space Complexity:
$$
\begin{align}
S(n) &= \Theta(1) + 2S\left( \frac{n}{2} \right) = \Theta(n)\\
\Theta(1) &\text{: find middle}\\
2S\left( \frac{n}{2} \right) &\text{: 2 rec calls to merge left\&right}\\
\Theta(n) &\text{: merge}\\ \\
\text{by Master Theorem: } S(n) &= S(\frac{n}{2})+f(n) \text{ with } f(n) = \Theta(n)\\
\log_2 1 &= 0 \text{ so } \log_b a < c \to \text{ case 3}\\
\text{so } S(n) &= \Theta(f(n)) \text{ so } S(n) = \Theta(n)\\
\end{align}
$$

## Lecture 14&15 3/5&7
#### Max Heap:
Complete Binary Tree: A CBT is a binary tree in which each level is full expect the lowest level, in the lowest level has all nodes to the left

Node Indexing: 
1. If the node with index i has kids, the kids have indices 2i and 2i+1
2. If the node with index i has a parent, it will have index $\left\lfloor  \frac{i}{2}  \right\rfloor$
3. If the tree has n nodes then the index of the maximum index node with kids is $\left\lfloor  \frac{n}{2}  \right\rfloor$
4. The nodes with kids are those with indices $1,2,\dots,\left\lfloor  \frac{n}{2}  \right\rfloor$ ![[Pasted image 20250307151331.png]]

Level Indexing: 
1. We 0-index the level:
2. The leftmost node in level k has index $2^k$
3. The node with index i is in the level $\lfloor \log i \rfloor$
4. If the tree has n nodes then it has levels: $0,1,\dots,\lfloor \log n \rfloor$![[Pasted image 20250307152050.png]]

Max Heap:
- Definition: A MH is a CBT where the key stored in each node is $\geq$ the keys stores in its kids' nodes, if it has kids.
- Max heaps are useful for priority queues in which we store and deal with jobs each of within has an "importance" 

Converting: 
- Given a CBT can we rearrange the keys to form a MH?
  Yes:
1. **Maxheapify** 
   Best: $\Theta(1)$ if no swapping is needed
   Worst: $\Theta(\log n)$ if we called it one the root and had to swap all the way to the bottom, note that if a specific node is given, the worst case may not be $\Theta(\log n)$
   
   We only ever call maxheapify on a node whose child subtrees (if any) are max heaps in their own right.
   Given a node we compare the key to the child keys. If $\geq$ both, stop.
   Else swap with largest child key (either if =) then recurse-do it again with the same key. Continues until the key stops swapping.![[Pasted image 20250307155812.png]]![[Pasted image 20250307162340.png]]
2. **Converting to max heap:**
   Best: $\Theta(1)$ per node so overall $\Theta(n)$
   Worst: $O(\log n)$ per node so overall $O(n\log n)$
   
   Take a CBT and converts it to a MH: Call maxheapify on all nodes with kids, in reverse order. (Recall the max index with kids is $\left\lfloor  \frac{n}{2}  \right\rfloor$![[Pasted image 20250307211611.png]]


#### Heap Sort:
- Algorithm: 
	  Starting with a list, we know it represent a CBT
	  First run convert to max heap on it!
	  Eg: A = [10, 20, 2, 5, 7, 15]
	  ![[Pasted image 20250307211851.png]]
	  ![[Pasted image 20250307211927.png]]
	  Then: for i = n, n - 1, ..., 2 we do:
	  1. Swap keys at indices 1 and i
	  2. Chop off index i (ignore it going forward)
	  3. Call maxheapify on the root.
	    Eg:![[Pasted image 20250307212144.png]]
- Time complexity:
  Recall: maxheapify: Best: $\Theta(1)$ Worst:$O(n\log n)$ 
  Convert to maxheap: Best: $\Theta(n)$
  Look at heap sort overall:

  Worst: Run convert to max heap: $\Theta(n)$, iterate n-1 times: Swap chop $\Theta(1)$ and maxheapify $O(\log n)$. In combine, $O(n\log n)$
  
  Best: 
  A. If the list is sorted then convert to max heap messes it all up, then must fix. End result $O(n\log n)$
  B. If the list is already a max heap, the "heap sort part" is still $O(n\log n)$
  C. If the list is all the same element then maxheapify never does any swapping. Result is $O(n)$.
  D. If the list is an reverse order to start, still $O(n\log n)$

## Lecture 16&17 3/10&12
#### Quick sort intro:
 **Intro**: Quick sort is a fairly fast general purpose sorting algorithm. Great for random data but not always the best option. 
 
 **High level overview:** 
 Suppose we have a list, we'll pick an element in the list called pivot key (pk).
 
 We then partition the list so that:
 - Everything to the lest of pk is $\leq$ pk, to the right is $\geq$ pk
 - In result the pk will be in its sorted position
>  Eg: Chose 3 as pk: [2, 5, 4, 1, 0, 3] -> [x, x, x, 3, x, x] 
>  Left is 0,1,2 in some order and right is 4,5 in some order, but 3 is in its right place.

Then we repeat recursively with each sublist, stop when the list.len = 1

**The Partition Process: **
1. There are many ways to do this, we'll use a fairly standard approach.
2. High level: suppose we have a list and we've chosen pk. Then:
   1. Take the left most element > pk
   2. Take the first subsequent element $\leq$ pk
   3. Swap
   4. Repeat until we cannot
> Eg. 
> [2, *5*, 4, *1*, 0, 3] pk = 3
> [2, 1, *4*, 5, *0*, 3]
> [2, 1, 0, *5*, 4, *3*]
> [2, 1, 0, 3, 4, 5] Cannot do it now, finish


```
function partition(A, L, R)
	\\ To use a different pivot key
	\\ Swap it with A[R] here.
	pk = A[R]
	t = L             \\ t is looking for the leftmost element that > pk
	for i = L to R-1   \\ i is looking for the first subsquence that <= pk
		if A[i] <= pk
			swap(A[t], A[i])
			t++
		end if 
	end for
	swap(A[t], A[R])
	return t
end
```
```
[2, 5, 4, 1, 0, 3] L = 0, R = 5, pk = 3
t,i               -> A[i] <= pk? Yes, useless swap
[2, 5, 4, 1, 0, 3]
   t,i            -> A[i] <= pk? No
[2, 5, 4, 1, 0, 3]
    t     i       -> A[i] <= pk? Yes, swap
[2, 1, 4, 5, 0, 3]
       t     i    -> A[i] <= pk? Yes, swap
[2, 1, 0, 5, 4, 3]    
          t     i -> loop stops because we only go to i = R-1

Final swap after loop:
[2, 1, 0, 3, 4, 5]    
          t       -> this is returned : t = 3(index)
```

**Choose of pk:**
- Can we use pk = median (the median of values in the list, not the middle index)? This would split the list nicely. Can be done in $\Theta(n)$
- Estimate the median: Can be done in $\Theta(1)$
- Random choice: most implementation do this, pretty good. 

#### Quick sort:
**In Place**
**Not Stable**

**Time Complexity:**
Best: $T(n) =\Theta(nlgn)$
Worst: $T(n) = \Theta(n^2)$
Avg: $O(nlgn)$

**Space Complexity:**
Best: $S(n)=s(\frac{n}{2})+\Theta(1)$
Worst: $S(n)=\Theta(n)$

After understanding the partition, quick sort is then easy:
```
function quicksort(A, L, R):
	if L < R:
		rpi = partition(A, L, R)
		quicksort(A, L, rpi-1)
		quicksort(A, rpi+1, R)

Initial call: quicksort(A, 0, len(A)-1)
```

**Time complexity:**
Best Case:
	We (somehow) choose pk = median every time then our sublists are essentially half the length.
	Then we have: $T(n) = \underbrace{2T(\frac{n}{2})}_\text{2 sublists half as long} + \underbrace{\Theta(n)}_\text{partitioning}$ 
	Notice this is the case 2 of the Master theorem with result: $T(n) = \Theta(n\log n)$ Note: This might not be the case for an already sorted list.

Worst Case:
	We (unfortunately) chose pk = smallest or largest element every time. Then:
	$T(n) = \underbrace{T(n-1)}_\text{one sublist or length n-1} + \underbrace{\Theta(n)}_\text{partitioning}$ With $T(0)=T(1)=\text{const}$
	By digging down: $T(n)=\Theta(n^2)$
Average Case:
	For a list of length n that pk has a $\frac{1}{n}$ chance of ending up at each index.
	So then we do an expected value calculation:
	$$
	T(n) = \sum_{k=0}^{n-1} \frac{1}{n}[\underbrace{T(k)}_\text{Left Sublist} + \underbrace{T(n-k-1)}_\text{Right Sublist} + \underbrace{\Theta(n)}_\text{Partition}]
	$$
	Step1: Show $T(n) = \frac{2}{n}[\sum_{k=0}^{n-1}T(k)]+n$
	Step2: Show $\frac{T(n)}{n+1}\leq \sum_{k=2}^{n}\frac{1}{k}$
	Step3: Recall: $\sum_{k=2}^{n} \frac{1}{k} \leq \int_{1}^{\infty}{\frac{1}{k}}$
	Step4: Conclude: $T(n)=O(n\log n)$


## Lecture 18 3/25
#### Limitation on Comparison Based Sorting Algorithms:
Definition: A CBSA works by comparing elements and performing actions (generally swapping)

**1. Intro: Consider the Worst Cases of our sorting algos:**
Bubble: $\Theta(n^2)$ all lists
Selection: $\Theta(n^2)$ all lists
Insertion: $\Theta(n^2)$ reverse sorted lists
Merge: $\Theta(nlgn)$ all lists
Quick: $\Theta(n^2)$ when you always chose the smallest/largest element as pivot
Heap: $\Theta(nlgn)$ list already sorted

Notice all of these are $\Omega(nlgn)$

Q: Can we build a CBSA that has worst case better than $\Omega(nlgn)$?
A: No

**2. Using Decision Trees:**
A CBSA compare two elements, do some actions, then repeat until list is fully sorted. This could been shown by a tree.

Eg. Consider a list of length n = 2. We can sort it by asking: is A[0] < A[1]? If so, stop; else swap.
Here is the tree:
![[Pasted image 20250326032843.png]]

Eg. Now consider n = 3, Bubble sort works by:
- Compare A[0] < A[1] Swap if not
- A[1] < A[2]
- A[0] < A[1] Swap if not
![[Pasted image 20250326033204.png]]
If we are sorting a list with n elements and we draw the tree, it must have $n!$ leaves, How high must it be?:

A prefect binary tree with height h has $2^h$ leaves. Our decision trees may not be prefect.
We have $L \leq 2^h$ but our trees has $n!$ leaves, so: $2^h > n!$
So: $h\geq lg(n!)$
Each "height" corresponds to a comparison we make, If takes $\Theta(1)$ time for our algo to sort at least some of the list
So out time is $\Omega(lg(n!))$ for some lists. So out CBSA in the worst case is $\Omega(nlgn)$
This gives us our result

Basic Idea:
$$
\begin{align}
lg(n!) &= lg(n(n-1)(n-2)\dots(2)(1)\\
&= lgn + lg(n-1) + lg(n-2) + \dots + lg(2) + lg(1)\\
&\geq \text{first half}\\
&\geq \frac{n}{2}lg(\frac{n}{2})\\
&\end{align}

$$
So if a function is $\Omega(lg(n!))$ it's $\Omega(\frac{n}{2}lg(\frac{n}{2}))$ which is $\Omega(nlgn)$

## Lecture 19 3/26
#### Counting Sort:
**Intro:** 
	Counting sort is a non-comparison based sorting algo which sorts lists of non negative ints. And can sometimes be faster than other algos.

#Examples 
Consider: 
`A = [3, 0, 2, 1, 2*, 0*] *to seperate elem with same value`

Construct a list pos (non-cumulative version). 
Pos has length k+1 where k= max value in list A. And pos[i] = # of i's in A.

$$\text{pos} = [\underbrace{2}_\text{A contains 2 0's}, \underbrace{1}_\text{A has 1 1's}, \underbrace{2}_\text{2 2's}, \underbrace{1}_{1 3's}]$$

Make pos cumulative - add previous values, iteratively.
`pos = [2, 3, 5, 6]`
Now, what could list A look like if it were sorted(stable)?
`A* = [0, 0*, 1, 2, 2*, 3]`
So observe: pos[i] = position(index - 1) of the last i in the sorted list.
```
[0, 0*, 1, 2, 2*, 3]
	^   ^     ^   ^
   2-1 3-1   5-1  6-1

eg: pos[2] = 5, and in the sorted list the 2's end at position 5
```
So now we'll iterate through list A backwards and we'll use pos to "copy" the elements from A to A*
Note that A* will start empty and end with a sorted A

Here goes:
```
A = [3, 0, 2, 1, 2*, 0*]
pos = [2, 3, 5, 6]
A* = [_, _, _, _, _, _]

A[5] = 0*, pos[0*] = 2 so put 0* in position 2: index 1
A* = [_, 0*, _, _, _, _] pos = [1, 3, 5, 6]
								^

A[4] = 2*, pos[2*] = 5 so put 2* in pos 5: index 4
A* = [_, 0*, _, _, 2*, _] pos = [1, 3, 4, 6]
									   ^

A[3] = 1, pos[1] = 3 so put 1 in pos 3: index 2
A* = [_, 0*, 1, _, 2*, _] pos = [1, 2, 4, 6]
									^
									
A[2] = 2 pos[2] = 4 so put 2 in pos 4: index 3
A* = [_, 0*, 1, 2, 2*, _] pos = [1, 2, 3, 6]
									   ^

A[1] = 0 pos[0] = 1 so put 0 in pos 1: index 0
A* = [0, 0*, 1, 2, 2*, _] pos = [0, 2, 3, 6]
								 ^

A[0] = 3 pos[3] = 6 so put 3 in pos 6: index 5
A* = [0, 0*, 1, 2, 2*, 3] pos = [0, 2, 3, 5]
										  ^

Last step: copy A* back on top of A
```

**Pre-Pseudocode Notes:**
1. We go through list A backwards to ensure stability.
2. In our original  Pos why not use it to just create a new list with 2 0's, 1 1's, 2 2's, and 1 3's?
   Because we want to think of the elements in A as items which need to be moved/sorted

**Pseudocode:**
```
\\ A is a list of non negative ints with len = n and maximum = k

//init
POS = list of zeros of length k+1
A* = list of zeros of length n

//creates non-cumulative version
for i = o to n-1
	POS[A[i]] = POS[A[i]] + 1
end

//makes it cumulative
for i = 1 to k
	POS[i] = POS[i] + POS[i-1]
end

//actual work
for i = n-1 down to 0
	A*[POS[A[i]]-1] = A[i]
	POS[A[i]] = POS[A[i]] - 1
end

//copy back to A
for i = 0 to n-1
	A[i] = A*[i]
end
```

**Notes:**
- Time complexity: $\Theta(n+k)$
   Suppose, as n gets bigger, k remains constant.
   Then this is $\Theta(n)$
   But perhaps k grows as n does?
   eg:  our list of length n contains non negative int between O and $n^2$
   Then it's $\Theta(n+n^2) = \Theta(n^2)$
 
- Space: $\Theta(n+k)$ as well because of A* and pos same points as time
- Stable
- Not Inplace
- Can be bad for large k even if constant because pos wll take up memory and our second loop will take a long time.
- Comparison Sort can sometimes be used to sort other lists if we can modify those lists.
  eg: a non negative list but one digit after decimal like 1.2. We could mult all by 10 then sort, then divide by 10


## Lecture 20 3/28
#### Radix sort (end of exam materials on sorting algos)

**Intro**: 
> To apply RS we need a list such that:
> - Each item has the same # of digits/characters(we can pad if necessary)
> - The digits/chars must be sortable
> 
> Eg: [582, 102, 063, 111, 849, 000]
> Eg: [Help, CMSC, Pain, love]

**How it works:**
> Start by choosing a stable sorting method which is used as the "underlying method".
> 
>  Then: 
>  - Start by least signify digit (right most), using the stable sorting method we chose.
>  - Then the next
>  - Etc
> 
> Eg: 
> 	[312^, 321^, 633^, 313^, 623^, 223^, 213^]
> 	[32^1, 31^2, 63^3, 31^3, 62^3, 22^3, 21^3]
> 	[3^12, 3^13, 2^13, 3^21, 6^23, 2^23, 6^33]
> 	Result: [213, 223, 312, 313, 321, 623, 633]
> Note: Do the most signify digit first doesn't work

**Note:**
- All items must have same # of digits, we can padding the elements if needed
- Underlying sort must be stable, otherwise it will mess up the value of elements

Time:
> Suppose the items we're sorting have d digits
> Suppose the underlying sort is $\Theta(f(n))$
> 
> Then Radix Sort will be $\Theta(df(n))$
> Note: d may not be constant
> Eg: 
> 	d = 3 with Merge Sort: $\Theta(3nlgn) = \Theta(nlgn)$
> 	d = 5 with Bubble Sort: $\Theta(5n^2) = \Theta(n^2)$
> 	d= 7 with Quick Sort: Don't work! Because QS is NOT STABLE

An "obvious" choice of the underlying sort algo is counting sort:

Eg:  d = 6 with counting sort:
	Given that Counting Sort is $\Theta(n+k)$ and in this case $k$ is the maximum digit

- So d = 6 with # in decimal then $k=9$ (digits 0-9) so we get $\Theta(6(n+9))=\Theta(n)$
- So d = 8 with # in binary then k = 1 (digits 0-1)
  so we get $\Theta(8(n+1)) = \Theta(n)$
- So d = 10with words using A-Z the nif we treat as 0-25 we get $\Theta(10(n+25))=\Theta(n)$

Note 1: k will essentially not change (max digit)
Note 2: d could change as n changes

Eg:
	Sps A is a list of length n with all values b, between 0 and $2^n - 1$ inclusive, in binary.
	Q: How many binary digit are needed to represent all # between $0, 2^n -1$ inclusive?
	A: It takes $n$ digits
	So here d = n, so time with counting sort as underlying sort is $\Theta(n(n+1))=\Theta(n^2)$
	Might be better to throw out Radix sort and use something else.

**In Place**: If and only if underlying sort is.

**Space:** Essentially same as the underlying sort
**Stable:** Yes.


## Lecture 21 3/31
Karatsuba's Algorithm (for integer multiplication)

**Intro:**
Consider it's the case that 
- Adding two single-digit numbers 7+5 is $\Theta(1)$
- Multiplying two single digit number 7*5 is also $\Theta(1)$
Questions:
- How about adding two n0digits numbers? 
  It's $\Theta(n)$, we do schoolbook addition:
```
  82
+ 98
------
 180
```
- How about mult two n-digit numbers?
```
		73
	*   46
	--------
		438
		2920
	---------
	3358
```
- with school book mult it is $\Theta(n^2)$ because of the $n^2$ mults, the additions don't make it worse
- To make it better, the goal is to reduce the # of SDMs (Significant k-digit mult)

**Pattern:**
> Consider two n=2 digit numbers
> Suppose we have $A = a_{1}a_{0}$ and $B =b_{1}b_{0}$
> eg: If A = 73 and B = 46 then $a_{1} = 7,a_{2}=3,b_{1}=4,b_{0}=6$
> we want to calculate A * B
> First observe that $A = 10a_{1}+a_{0}$ and same for B,
> So: 
$$
\begin{align}
AB &= (10a_{1}+a_{0})(10b_{1}+b_{0})\\
&= 10^2a_{1}b_{1}+10(a_{0}b_{1}+a_{1}b_{0}) + a_{0}b_{0}\\ \\
&\text{now observe separately: }\\
(a_{1}+a_{0})(b_{1}+b_{0}) &= a_{1}b_{1}+a_{0}b_{1}+a_{1}b_{0}
a_{0}b_{1} + a_{1}b_{0}\\
&= (a_{1}+a_{0})(b_{1}+b_{0})-a_{1}b_{1}-a_{0}b_{0}\\
&\text{Plug this into * to get: }\\
AB &= 10^2 a_{1}b_{1} + 10[(a_{1}+a_{0})(b_{1}+b_{0})-a_{1}b_{1}-a_{0}b_{0}]+a_{0}b_{0}\\
\end{align}


$$
> While * is not necessarily a SDM, it's certain a product of smaller numbers than AB
> Lets pretend it is a SDM for bookkeeping
> So we have:
> - 3 SDM: $a_{0}b_{0}, a_{1}b_{1}, (a_{1}+a_{0})(b_{1}+b_{0})$
> - 2 + 1 decimal shifts: the mult by $10^2$ and 10
> - 6 +/- each with $\leq 4$ digits

With two $n=4$ digit numbers.
> Think $A=A_{1}A_{0}$ and $B=B_{1}B_{0}$
> eg: if $A=8613, B=4097$ then $A_{1}=86, A_{0}=13, B_{1}=40, B_{0}=97$
> So really $AB = 10^4A_{1}B_{1}+10^2[(A_{1}+A_{0})(B_{1}+B_{0})-A_{1}B_{1}-A_{0}B_{0}]+A_{0}B_{0}$
> So we have:
> - 3 two digit mult
> - 4+2 decimal shifts
> - 6 +/- each with $\leq 8$ digits

With two n-digit # s with n  = even
> We get: $A=A_{1}A_{0}$ and $B=B_{1}B_{0}$ where $A_{1}, A_{0}, B_{1}, B_{0}$ each have $\frac{n}{2}$ digits
> $AB = 10^nA_{1}B_{1} + 10^{\frac{n}{2}}[(A_{1}+A_{0})(B_{1}+B_{0})-A_{1}B_{1}-A_{0}B_{0}]+A_{0}B_{0}$
> Here we have:
> - 3 mult with $\frac{n}{2}$ digits each
> - $n+\frac{n}{2}$ decimal shifts
> - 6 +/- each with $\leq 2n$ digits

**Building an Algorithm:**
To mult two n-digits # s we apply
this reduce two problem to (essentially) 3 mult with $\frac{n}{2}$ digits each and the decrease shifts and the +/-
We recurse down to SDMs which we do:
If turns out that $n+\frac{n}{2}$ decrease shifts is $\Theta(n)$ and the 6+/- with $\leq 2n$ digits is also $\Theta(n)$.
So if $T(n) =$ time required to mult two n-digits # s then:
$$T(n) = \underbrace{3T(\frac{n}{2})}_\text{3 mult with n/2 digits}+ \underbrace{\Theta(n)}_\text{shifts and +/-}$$
Solve with Master Theorem:
$$
\begin{align}
\log_2 3 > 1 = c \text{ so case 1}\\
\text{with solution:}\\
T(n) = \Theta(n^{\log_2 3}) \approx \Theta(n^1.58)\\
\end{align}
$$
This is better than schoolbook

## Lecture 22 4/2
#### Exam Review:
1. Overall comments:
	1. Don't need to memorize pseudocode but do need to understand them
	2. Understand how algos work at various level
	3. Trace with examples
2. Binary Search
	1. All of 1.
	2. Time: 
	   Best case: $\Theta(1)$ 
	   Worst case: $\Theta(lgn)$ 
	   Average case: $\Theta(lgn)$
	3. List must be sorted (but you can assume this on exam)
3. Recurrence Relations
	1. Definition + examples:
$$
\begin{align}
T(n) &= 2T\left( \frac{n}{2} \right)+n+1\\
T(n) &= 3T(\left\lfloor  \frac{n}{2}  \right\rfloor )+n\\
T(n) &= 3T(n-1)+n^2 \\
T(n) &= 2T(n-1)+T(n-2)+n \\
&\text{etc}
\end{align}
$$
	2. Need to calculate specific values
	3. Understand how they represent algos
4. Digging Down
	1. Iterate, find a pattern, end with a $T(?)$ in it, like $T(\frac{n}{2^k}), T(\text{base case})$ will tell you $k$, then put $k$ into your formula
	2. See where it ends (base case)
	3. Construct a formula
	4. Do a bunch of example questions
5. Trees
	1. Use to represent $T(\text{specific values})$
	2. Going trees -> Recurrence Relation
	3. Using tree -> table -> formula
	```
			n
		   / \
	T(n/3)     T(n/3)
	table had one tow per level, etc
	...
			n
		   / \
		base cases
	```
6. Master Theorem:
	1. eg: $T(n)=aT(\frac{n}{b})+f(n)$ $a,b\in Z, a\geq {1}, b\geq {2}$
	2. Know the cases
	3. Do example questions
	4. sneaky examples: using facts like $n^2lgn = O(n^{2.1})$ $n^2lgn=\Omega(n^2)$ etc
	5. Know when does it not apply
7. Merge sort
	1. All of 1.
	2. Recurrence Relation: $T(n) = 2T(\frac{n}{2})+\Theta(n)$ <- MT case 2
	3. Space: $S(n)=\Theta(n)$ <- Via MT
	4. Stable
	5. Not in place
8. Heaps
	1. Complete binary tree
	2. Know index + level facts
	3. max heap: parent key $\geq$ child keys
	4. maxheapify
	5. conver to maxheap CBT(Complete Binary Tree) -> MH(Max Heap)
9. Heapsort
	1. List: treat as CBT, convert to maxheap
	2. Swap, chop, on maxiheapify on root (repeat)
	3. Time: 
	   Worst case: $\Theta(nlgn)$
	   Best case: $\Theta(n)$
	4. Space: $\Theta(1)$
	5. Not stable
	6. In pace
10. Quicksort
	1. How partition works (pivot key - last elt unless stated) (high-level + low-level)
	2. How QS works (divide + conquer) on top of that
	3. Time:
	   Best case: $\Theta(nlgn)$ via $T(n)=2T(\frac{n}{2})+\Theta(n)$
	   Worst case: $\Theta(n^2)$ via $T(n)=T(n-1)+\Theta(n)$
	   Average case: $\Theta(nlgn)$ via homework
	4. Space:
	   Best case: $\Theta(lgn)$
	   Worst case: $\Theta(n)$
	5. Stable
	6. In place
11. Limitation of Comparison based sorting algos
	1. Nothing on the exam
12. Counting Sort
	1. Sorts lists of non negative int between $0,\underbrace{k^{\text{max}}}_\text{might depend on n}$
	2. Sets up POS (non-cumulative)
	3. Then POS (cumulative)
	4. Use POS to build a new from A
	5. Time: $\Theta(n+k)$
	6. Space: $\Theta(n+k)$
	7. Stable
	8. Not in place
13. Radix sort
	1. All entries have same # digits/char
	2. Sort rightmost digit first, then go left. With any stable sort algo
	3. Time: $\Theta(df(n))$ with $f(n)=\text{time of the sort algo used}$


## Lecture 23 4/7

#### Karatsuba - continued:
**Recall**: For A, B, with n (even digits each we split them up as $A = A_1A_{0}$ and $B=B_{1}B_{0}$, then: $AB = 10^nA_{1}B_{1}+10^{\frac{n}{2}}[(A_{1}+A_{0})(B_{1}+B_{0})-A_{1}B_{1}-A_{0}B_{0}] + A_{0}B_{0}$
So how does this manifest as an algorithm?
Loosely speaking, given A, B we split them up tp $A_{1},A_{0},B_{1},B_{0}$ we then calculate:
- $A_{1}B_{1}$
- $A_{0}B_{0}$
- $(A_{1}+A_{0})(B_{1}+B_{0})$
- This 3 is called * in the following note
We plug those into the pervious formula to calculate AB. The sneaky bit B that * are done recursively. Base case is when EITHER is a single-digit number

**Pseudocode**:
```
\\A,B are the list representation of number

function karatsuba(A, B)
	if either A or B is single-digit
		return (A*B)
	else 
		sp = floor((minimum number of digits in A, B)/2
		A1, A0 = split A, p digits from the right
		B1, B0 = split B, sp digits from the right
		k1 = karatsuba(A1, B1)
		k2 = karatsuba(A1+A0, B1+B0)
		k3 = karatsuba(A0, B0)
		// The powers of 10 should be thought of as shifts.
		r = 10^(2*sp)*k1 + 10^(sp)*(k2-k3-k1) + k3
		return(r)
	end
end
```
splitting ex. to clarify sp, etc
Consider (13521)(2143)
Note: sp = floor((min # of digits in A, B)/2) = floor(4/2) = 2
So:
	A = 13521 = A1 A0 with A1 = 135 A0 = 21 } 
	B = 2143 = B1B0 with B1 = 21 B0 = 43     }
	Split both 2 digits from the right

Key point: 
	$A =135*10^2+21$
	$B = 21 * 10^2 + 43$

**Counting the # of SPMs**:
We can draw a tree diagram to do this
![[Pasted image 20250411193917.png]]
Total # of SDM = 14 SDMs
If we did schoolbook mult. We'd have $5*4=20$ SDMs 

**Closing Notes:**
A. In extreme cases we want reduce the # of SDMs
![[Pasted image 20250411195628.png]]

B. A couple more splitting examples:
(8461529|7)(46|3)
(15|68)(43|21)

C. No special cases other than the base cases:
(456)(0)
(456)(1000)

## Lecture 24 4/9
**Basic Terminology:**
Graph: A graph is a collection of vertices/nodes connected by edges.
ex:
![[Pasted image 20250411200234.png]]Adjacent: We say two vertices are adjacent if there is a edge between them.

Degree: The degree of a vertex is the # of "edge ends" at that vertex.
![[Pasted image 20250411200432.png]]

Loop: A loop is an edge which joins a vertex to itself.
![[Pasted image 20250411200617.png]]

Edges: A graph has multiple edges if we allow more than one edge between two vertices.
![[Pasted image 20250411201057.png]]

Simple: A graph is simple if it has no loops, no multiple edges. (Some people might use slightly different definition) Essentially all our graphs will be simple.

Weighted: A graph is weighted if each edge has a number (weight, cost) associated to it. Negative and zero are allowed, most of our weighted graphs will only have positive weights
![[Pasted image 20250411201944.png]]

Directed: A graph is directed if the edges have directions
![[Pasted image 20250411202619.png]]

Notation We'll use 
	V = # of vertices (sometimes n is used)
	E = # edges
	Time complexity of graph algos of ten depends on both
	Eg: We might get a time complexity like $\Theta(V+E)$

**Connections:**
Walk: A walk is a sequence of the form: vertex, edge, vertex, edge,..., vertex. Which starts at a vertex and ends at a vertex. How we write this down might depend on how the graph is given. Repeating vertices and edges is allowed.
![[Pasted image 20250411234651.png]]

Trail: A trail is a work with no repeated edges. Repeated vertices are fine.
![[Pasted image 20250412112942.png]]

Path: A path is a walk with no repeated edges or vertices.
Eg: In above 1-2-3-4-5

Cycle: A cycle is a path in which we allow + force the starting and ending vertices to be the same.
Eg: In above 2-5-4-3-2

Connected: A graph is connected if for any two vertices, there is a path between them. 
![[Pasted image 20250412113029.png]]

Tree: A tree is a connected graph with no cycles
![[Pasted image 20250412113055.png]]
**Graph storage**: 
Q: How might we store these?
1.  Notes + Panters: Not that useful for us
2. Adjacency Matrix
   For an unweighted, undirected simple graph with V vertices, the adjacent matrix is the V * V matrix A where `A[i][j]` = 0 if vertices i,j are not adjacent, and 1 if they are 
![[Pasted image 20250412114435.png]]
Note: versions exist for directed graphs, weighted graphs, etc
3. Adjacency List:
   The adj. list for a graph is a list of lists AL where `AL[i] = list` of vertices i is adj to ![[Pasted image 20250412114548.png]]
## Lecture 25 4/11
#### The shortest path algorithm
**Goal:** 
Given a simple, undirected, unweighted, connected graph, and a starting vertex, find the shortest path (and distance) to every other vertex.

**Idea:**
If s is the starting vertex - we start at s, then visit:
- all  vertices one edge away from s.
- all " two edges " " ".
- etc

**More Detail:**
We'll have a queue Q and start with s on Q. We dequeue it and visit all vertices adj, to s, enqueuing those as well.
We dequeue those and visit all* vertices adj. to those, etc.
all* which have not been visited already.
As we go, then, we need to keep track of:
- Visited vertices
- Distance
- Paths

**Examples:**
Consider the graph:
```
Initialize
Q = [1]                     //just s
D = [inf, 0, inf, inf, inf] //distance
//D[1] = 0 because distance to vertex 1 is 0.
P = [N, N, N, N, N]         //predecessor will store path 
```
![[Pasted image 20250412171414.png]]

Iterate: 
	X = Q dequeue = 1
	look at all $\infty$-vert (not visited) adj. to x = 1
	Enqueue them, set their distance = 1 (1's dist +1)
	And set their pred. to 1
	$S_{1}$ Q = [0,2] D = $[1, 0, 1, \infty, \infty]$ P = [1, N, 1, N, N]

Iterate:
	X = Q dequeue = 0
	Look at all $\infty$-vertices adj to x = 0
	Enqueue them, set their distance = 2 (0's dist + 1)
	And set their pred. to 0
	So Q = [2, 3] $D = [1, 0, 1, 2, \infty]$ P = [1, N, 1, 0, N]

Iterate:
	X = Q dequeue = 2
	Look at all $\infty$-vertex adj to x = 2
	Enqueue them, set their dist = 2 (2's dist + 1)
	And set their perd to 2
	So Q = [3, 4] D = [1, 1, 1, 2, 2] P = [1, N, 1, 0, 2]

Iterate:
	X = Q dequeue = 3
	look at all $\infty$-vert adj to X = 3, None.
	So Q = [4] D, P Same

Iterate:
	X = Q dequeue = 4
	look at all $\infty$-vert adj to X = 4, None.
	So Q = [], D, P Same

Note: When we enqueue multiple adj vertices, the order is up to the implementation.

**Pseudocode:**
```
function shortestpath(G, s, t)
	dist = distance array of size V full of inf
	pred = predecessor array of size V full of NULL
	Q = empty queue
	dist[s] = 0
	Q.enqueue(s)
	while Q is nonempty
		X = Q.dequeue
		for each infinity vertex y adjacent to x
			dist[y] = dist[x] + 1
			pred[y] = x
			if y == t
				return(pred)
			end
			Q.enqueue(y)
		end
	end
end
```

**Time:**
	With a target the best case would be if t is adj to s. Then time is $\Theta(V)$ for init, and $\Theta(1)$ because we essentially get t next.
	So lets look at the non-target version.
	All cases are the same.
	
**All Cases:**
	Before the while loop it's $\Theta(V)$
	The while loop iterates V times (once for each vertex)
	Then:
- If graph is stored as an aj matrix then to find all $\infty$-vert adj to x (in the code)
  We want to scan row x in two adj matrix.
  That's V steps.
  So the for loop iterates V times at $\Theta(1)$ each time.
  Time: $\underbrace{\Theta(V)}_\text{init} + \underbrace {V}_\text{dequeue while loop}[\Theta(1) + \underbrace {V}_\text{for loop}(\underbrace{\Theta(1)}_\text{inside for loop})] = \Theta(V^2)$
- If two graph is stored as an adj list... without the for loop: $\Theta(V)+V[\Theta(1)] = \Theta(V)$
  But over the course of the entire algorithm, the body of the for loop runs twice for each edge, once when x is at one end ant y at the other and once when those are reversed.
  The body of the for loop then run 2E times. Each time is $\Theta(1)$
  So total time is $\Theta(V+2E)$
  Some $\infty$ $\Theta(V+E)$

## Lecture 26 4/14
#### Graph Traversals:
Idea: Given a graph, we wish to visit all the vertices in some orders. There are many different ways to do this.

**Breadth-First Traverse:**
Idea:
	From a starting vertex, visit the closer vertices first.![[Pasted image 20250415010848.png]]

Note:
	This is just like the SPA.
	- Use a queue
	- Don't need DIST, replace by VISITED = boolean list which tells us if a vertex has been visited.
	- don't need PRED, replace by VORDER = list which stores the order we visit the nodes.

Pseudocode:
```
function bft(G, s)
	QUEUE = [s]
	VISITED = list of FALSE of length V\
	VISITED[s] = TRUE
	VORDER = [s]
	while QUEUE is not empty:
		x = QUEUE.dequeue
		for all y adjacent to x
			if VISITED[y] == FALSE
				QUEUE.enqueue(y)
				VISITED[y] = TRUE
				VORDER.append(y)
			end
		end
	end
	return(VORDER)
end
```

![[Pasted image 20250415011518.png]]![[Pasted image 20250415011530.png]]
Notes:
	- VORDER gives the orders
	- When we looked at adj vert, we did so in inc order

**Time:**
	Time before the while loop is $\Theta(V)$ to set up the list. While loop iterates V times (once for each vertex)

> Adj. Matrix: 
> 	The for loop runs V times to look through row x of the matrix to see which y are adj.
> 	Inside the for loop is $\Theta(1)$
> 	Also x = Q.deq is $\Theta(1)$. And return is $\Theta(1)$
> 	So: Time = $\underbrace{\Theta(V)}_{\text{before while}} + \underbrace{V}_\text{while loop V times}(\underbrace{\Theta(1)}_\text{dequeue} + \underbrace{V*\Theta(1)}_\text{for loop}) +\underbrace{\Theta(1)}_\text{return}$
> 	Time = $\Theta(V^2)$
> 
> Adj. List: 
> 	Without the for loop, time = $\Theta(V)+V(\Theta(1)+\text{no for loop})+\Theta(1)=\Theta(V)$
> 	Consider the body of the for loop. For each edge the body runs twice - once when x is one end and y is the other, and once for the reverse.
> 	Thus the body of the for loop runs twice for each edge over the course of the entire run. Thus total time:
> 	Time = $\Theta(V)+\underbrace{2E}_\text{body of tor loop runs 2E times}(\underbrace{\Theta(1)}_{\text{takes }  \Theta(1)\text{ per}}$
> 	Time = $\Theta(V)+\Theta(2E)=\Theta(V+E)$

**Depth-First Traberse:**
**Idea:**
	We have a starting vertex. Want to traverse as far away as possible, and only "back up" when forced to![[Pasted image 20250415013539.png]]
**Implementation:**
	Two classic ways:
> 	A: Recursion
> 	B: Using a stack
> 		Basic idea: start with s on the stack
> 		Pop a vertex
> 		Push all the adj. vertices
> 		only visited when we pop![[Pasted image 20250415013721.png]]

## Lecture 27 4/16
#### Depth-First Traverse:
**Reminder:**
	The general approach to DFT is:
	- Use a stack
	- When we pop a vertex, mark it as visited and push adjacent vertices.

But there's an issue to contend with!
**Consider:** 
	Suppose we go 0 -> 1
	at 1 we push 2, 3 onto the stack because they're adjacent to 1
	Suppose we now go 0 -> 1 -> 2
	at 2 we see 3, 4 adj. 
	Do we push 3 onto the stack again?![[Pasted image 20250416230019.png]]
**3 Ideas:**
> 	A. If a vertex is on the stack, don't push it on again. However this actually may not lead to DFT.
> 	B. Allow vertices on the stack multiple times, but if we pop a vertex that has already been visited, ignore it.
> 	Here's the pseudocode:
```
VORDER = []
VISITED = list of length V full of FALSE\
STACK = [s]
while STACK is not empty:
	x = STACK.pop()
	if VISITED[x] == FALSE:
		VISITED[x] = TRUE
		VORDER.append(x)
		for all nodes y adjacent to x:
			if VISITED[y] == FALSE:
				STACK.push(y)
			end if
		enf for
	end if
end while
```
> 	Issue: we don't know how many times the while loop will run.
> 	We end up with best + worst cases.
> 	We get:
> 	Best case:
> 		AM $\Theta(V^2)$
> 		AL $\Theta(V+E)$
> 	Worst case:
> 		AM $\Theta(V^2)$
> 		AL $\Theta(V^2+E)$ and $O(V^2)$
> 	C. if a vertex is on the stack and we go to push it again, remove the earlier occurrence
> 	Now each vertex is on the stack once so the while loop runs exactly V times!
> 	Pseudocode:
```
VORDER = []
VISITED = list of length V full of FALSE
STACK = [s]
while STACK is not empty:
	x = STACK.pop()
	VISITED[x] = TRUE
	CORDER.apend(x)
	// order must be given
	for all nodes y adjacent to x:
		if VISITED[y] == FALSE:
			if y on STACK
				STACK.remove(y)
			end if
			STACK.push(y)
		end if
	end for
end while
```

S = [0] V0=[] V1=[F, F, F, F, F, F]

S = S.pop = 0, adj unvisited 1, 2, 3 so:
S = [1, 2, 3] V0 = [0] V1 = [T, F, F, F, F]

x = S.pop = 3, adj unvisited: 1 so:
S = [2, 1] V0 = [0, 3], V1 = [T, F, F, T, F]
    ^ notice here 1 removed
    
x = S.pop = 1, adj unvisited: 4 so
S = [2, 4] V0 = [0, 3, 1] V1 = [T, T, F, T, F]

x = S.pop = 4, adj unvisited: none, so
S= [2] V0 = [0, 3, 1, 4] V1 = [T, T, F, T, T]

x = S.pop = 2, adj unvisited: none, so
S = [] V0 = [0, 3, 1, 4, 2] V1 = [T, T, T, T, T]
![[Pasted image 20250416232206.png]]

Time complexity: Imagine removal is $\Theta(1)$
Then this is exactly like BFT, so:
- Adj matrix: $\Theta(V^2)$ <- because each loop runs V times.
- Adj list: $\Theta(V+E)$ <- because without for loop it is $\Theta(V)$ but the body of the for loop runs 2E times over the entire algo. For each edge, once when x = one end, y = other once for the reverse.
  Thus the body of the for loop is $\Theta(2E)$ overall So $\Theta(V)+\Theta(2E)=\Theta(V+E)$

Last hurdle: How do we remove a vertex from the stack in $\Theta(1)$?
Implement the stack as a doublely-linked list
We can push/pop with a pointer to the end  (this is $\Theta(1)$
Then we have a list PTR indexed by vertex,
so for a vertex x, PTR[x] points to:\
	if x $\in$ stack, points to that element in the DLL
	if x $\in$ stack, NULL
So now to see if some y is on the stack, check PTR[y]. If not NULL we simply remove it from the DLL by concatenating points, then push y onto the stack, update PTR[y]
![[Pasted image 20250416234525.png]]

## Lecture 28 4/18
#### Dijkstra's Algorithm
**Intro:**
	Given a weighted (positive weight), simple, undirected connected graph and a starting vertex s, find the shortest path (lowest weight)
	Basically, this is like the SPA but with weights.
Issue: We may encounter a vertex "later" in the algo and find a shorter path to it. So distances may need to be updated.

Goal in visual ![[Pasted image 20250418233403.png]]

**General Idea:**
	pred = " " " " " NULL
	S = empty set, once a vertex is in S we are done with it.
	Iterations:
	- Pick a vertex x of min.dist which is not yet in S (if several exist, pick one)\
	- Put x in S, done with it
	- For every y adjacent to x, ask:
		  Is dist[x] + w(x, y) < dist[y]?
		  If so, assign dist[y] = dist[x] + w(x, y), assign pred[y] = x
	Keep going until all vertices are in S.

Eg:
![[Pasted image 20250419001740.png]]
P = [N, N, N, N, N] S = {}
Iterate, pick a vertex of min.dist not in the S. Thats:
> x = 2: Put it in S, look at ad:
> 	y = 1: Is d[2] + w(2, 1) < d[1]? ] d[1] = 10
> 	        0   +   10      < $\infty$      ] p[1] = 2
> 	y = 3: Is d[2] + w(2, 3) < d[3]? ] d[3] = 30
> 			0   +   30       < $\infty$      ] p[3] = 2  
![[Pasted image 20250419011854.png]]
P = [N, 2, N, 2, N] S = {2}
Iterate, Pick:
> x = 1: Put it in S. Look at adj:
> 	y = 2: Is d[1] + w(1, 2) < d[2]
> 			10   +    10   <  0 ? (false)
> 	y = 3 Is d[1] + w(1, 3) < d[3]             ] d[3] = 25
> 		    10   +    15   <  30 ? (true) ] p[3] = 1
> 	y = 4 Is d[1] + w(1, 4) < d[4]             ] d[4] = 50
> 			10   +    40  <  $\infty$  ? (true) ] p[4] = 1

![[Screenshot 2025-04-19 at 01.43.26.png]]
P = [N, 2, N, 1, 1] S = {2, 1}
> x = 3. Put it in S, look at adj:
> 	y = 0: Is d[3] + w(3, 0) < d[0]          ] d[0] = 45
> 		    25  +  20       < $\infty$ ?(true) ] p[0] = 3
> 	y = 1: Is d[3] + w(3, 1) < d[1]
> 			25  +  15      < 10 ?(false)
> 	y = 2: Is d[3] + w(3, 2) < d[2]
> 			25  +  30       < 0 ?(false)

![[Screenshot 2025-04-19 at 01.51.11.png]]
P = [3, 2, N, 1, 1] S = {2, 1, 3}
Iterate: Pick:
x = 0: Put it in S, Adj: y =3, no update.
Iterate: Pick:
x = 4: Put it in S, Adj: y = 1, no update.
Done

**Pseudocode:**
```
def dijkstra(G, start):
	// from here
	dist = [inf, ..., inf] of length V
	pred = [NULL, ..., NULL] of length V
	S = []
	dist[start] = 0
	// to here, time is Theta(v)
	
	while length(S) != V
		x = vertex in G-S with smallest distance // Theta(v)
		for each vertex y connected to x
			// body of For loop is Theta(1)
			if dist[x] + (Weight of Edge x, y) < dist[y]:
				dist[y] = dist[x] + (Weight of Edge x, y)
				pred[y] = x
			end
		end
		append x to S // Theta(1)
	end
end
```

**Time if G is stored as an... :**
	A. Adj Matrix: For loop runs V times always. So total:
		Total = $\underbrace{\Theta(V)}_\text{init} + \underbrace{V}_\text{while}(\underbrace{\Theta(V)}_\text{find x}+\underbrace{V}_\text{for}(\underbrace{\Theta(1)}_\text{if}+\underbrace{\Theta(1)}_\text{append}=\Theta(V^2)$
	B. Adj List: Without the For loop at all:
		$\Theta(V)+V(\Theta(V))=\Theta(V^2)$
		The body of the for loop, over the entire algo, runs 2E times at $\Theta(1)$ per time, so
		Total = $\Theta(V^2)+\Theta(2E)=\Theta(V^2+E)$
Note: Other data struct (min heaps, fibonacci trees) can be used, too, and if done well, we can get a time complex 

## Floyd's Algorithm
**Goal:**
	Given a simple, connected, directed, weighted graph, for every pair of vertices, find the shortest path between them. 
	Note that we allow zero and negative weights but no neg cycles

**The Algorithm:**
	Start with two matrices.
	A. Distance matrix (aka a variation on the adj matrix)
		d[i, j] = {weight of edge from i to j if on exists, $\infty$ otherwise
		d[i, i] = 0
	B. Predecessor matrix
		p[i, j] = { i if there is an edge from i to j, N otherwise}
		p[i, i] = i

Ex:
	![[Screenshot 2025-04-21 at 17.00.51.png]]Interpret:
	d[i, j] = 
		length of shortest path from i to j,
		Not perimitting any intermediate vertices
	p[i, j] = 
		assuming there is a path: i to j
		Not  ... vertices,
		This is the predecessor of j
	Iterate: Suppose we allow 0 as an intermediate vertex, what changes?
		In code we'll check
		For all i, j is d[i, o] + d[o, j] < d[i, j]?
		If so: d[i, j] = d[i, o] + d[o, j]
		and: p[i, j] = p[o, j]
	
![[Screenshot 2025-04-21 at 17.24.27.png]]
	BecauseL i = 2, j = 1, gets better, nothing else improves.
	This is the "pass by o" step.
	Iterate: Suppose we now allow all 0, 1, 2 as int vert. This is the "pass by 2" step. I.e. Allowing all of 0, 1, 2 as int vert. Code: same but replace 0 by 2
In our ex.
![[Screenshot 2025-04-21 at 17.36.09.png]]
	Because i = 1, j = 0 gets better.
	Done. How to interpret:
	A. d[i, j] 
		= shortest dist, form i to j if a path exists.
		= $\infty$ if not possible
	B. To reconstruct the path from i to j:
	Find:
		p[i, j]
		p[i, p[i, j]]
		p[i, p[i, p[i, j]]]
		p[i, p[i, p[i, p[i, j]]]]
		...
		Until we get i.
		Then the path is this list in reverse order

**Pseudocode:**
```
d= adjacency matrix for a graph
p = n x n array all null
for each edge (u, v)
	p[u, v] = u
end for
for each vertex v:
	p[v, v] = v
end for

for k = 1 to n:
	for i = 1 to n:
		for j = 1 to n:
			if d[i, k] + d[k, j] < d[i, j]:
				d[i, j] = d[i, k] + d[k, j]
				p[i, j] = p[k, j]
			end if
		end for
	end for
end for
```
**Time:** is $\Theta(V^3)$ with V = # vertices
Slow but it give us all shortest paths, and with negative weights

## Lecture 29 4/23
#### Spanning Trees and more:
1.
	Given a simple, connected, undirected graph, a spanning tree is a subgraph which is a tree and includes every vertex. Typically there are many:![[Screenshot 2025-04-23 at 13.50.33.png]]
	Use case: reinforcing connections in a network 
2.
	Given a simple, connected, undirected, weighted graph, each spanning tree has a total weight.
	A minimal spanning tree (MST) is a spanning tree with min total weight![[Screenshot 2025-04-23 at 13.56.18.png]]
	There are 4 classic algo to do this.

Prim's Algo:
	Works as follow: It "grows" a tree:
		A. Pick any vertex
			Define T = the subtree of G (the graph) consisting of just that vertex.
		B. Iterate:
			Look at all edges which connect a vertex in T with a vertex not in T. Pick one with min weight. Include it and the vertex not in T.
		C. Repeat B. until we have all vertex
	Note: Because our tree will have V-1 edges, step B will iterate V-1 times.

Example:
![[Screenshot 2025-04-23 at 14.13.02.png]]

Note: This algo is greedy - makes the best choice at each step with no long term planning. We know not all greddy algos are optimal (coin-changing for example) but this one is.

Pseudocode:
This depends strongly on the structure we use to store + search through edges.
However, we can say this about the time complexity:
- Easy to do in $O(V^3)$
	V-1 steps, look in an adj matrix, thats $V^2$ etc
- Can be modified easily to get $O(V^2)$ still with adj matrix
- Can be done with more care (using a min heap to store edges by weight) to get $O(E\log V)$ or $O(E+V\log V$). The log is because of the heaps.

## Lecture 30 4/25
#### Kruskal's Algorithm
Intro: Kruskal's Algorithm is another greedy algo for finding a minimal spanning tree.

Algo: 
	Given a simple, connected, weighted, undirected graph, do the following: let T = the graph but with no edges (all the vertices).
	Iterate: 
		Pick a min. weight edge from the original graph such that if we add it to T, we don't get  a cycle. Add it
		Do this V - 1 times, the result is a MST.

![[Screenshot 2025-04-25 at 12.46.09.png]]
We did it 8 times because there are 9 vertices.
Note1: Typically many choices at each step
Note2: Unlike prim, we don't start with a specific vertex
Note3: Until the end we may just have a bunch of disconnected bits

Pseudocode + Note:
	A. Idea: Let T = our graph with no edges (aka a set of V vertices)
		Iterate V - 1 times:
		- Pick min weight wedge whose inclusion in T doesn't from a cycle.
		- Include it in T.
		Note: could be better to think:
		let : = []
		Iterate V - 1 times
		- Pick min weight edge whose inclusion in L doesn't from a cycle
		- L.append(that edge)
		at the end, L contains the edges we want.
		Suppose we had all the edges in a list.
		Iterating through them takes E steps.
		But how can we check if including an edge forms a cycle?
		idea: DFT can be used to find cycles - if we do DFT and encounter a pre-visited vertex, we have a cycle. 
		Total Time = $\underbrace{(V-1)}_\text{num of times}\underbrace{E}_\text{look through all edges} * \Theta(\underbrace{V^2}_\text{DFT with Adj. matrix}) = \underbrace{\Theta((V-1)EV^2)}_\text{slow}$
	B. If we use some basic heaps and simple structures this to $O(E\log E)$
	C. But there is a faster way: 
		There is a data structure called a disjoint set data structure, aka Union-find data structure
		If we use a DSDS then this question: 
		Given a graph with no cycles and an edge, if we include the edge, does this form a cycle.
		Can be answered "very fast": the amortized time is $O(\alpha(V))$
		- Amortized time usually asks: if we do something some number of iterations, in the worst-case, what is the avg per-iteration time?
		- $\alpha$ is the inverse Ackerman function![[Screenshot 2025-04-25 at 14.31.39.png]]
	So provided $V \leq 10^600$ then $\alpha(V)\leq 4$ which means it's basically constant.
	Essentially we can detect if adding an edge forms a cycle in const time ($\alpha(V)$. So if our list of edges is sorted, then we iterate through it and simply check if each would form a cycle, and if not, ad it.

## Lecture 31 4/29
#### Huffman Coding (not on exam 3)
Intro:
Suppose we have a word like `HEYYY`
We wish to encode it - assign a binary string to each letters.
One idea is: 
	H = 00
	E = 01
	Y = 10
	-> HEYYY = 0001101010 (10 bits)
Could we do this with fewer bits?
	H = 0
	E = 1
	Y = 01
	-> HEYYY = 01101010 (8 bits)
	But note this is ambiguous -> HEEHEHEH
Issue: In this case one binary string, 1, is a prefix of another, 10, which means we don't know how to interpret it.

This:
	H = 0
	E = 10
	Y = 11
	-> HEYYY = 0101111111 (9 bits not ambiguous)

Some definitions and question:
	Fixed-length Code: every letter is assigned the same # of bits
	Prefix Code: No binary string is a prefix of another, 
	Note: 
		Prefix code $\not \to$ FL code
		FL code $\to$ Prefix code
	Q: Given a specific word, how can we construct a prefix code such that the word uses as few bits as possible?
	A: Below

Huffman Coding:
	Intro: Letters which appear more frequently should use fewer bits, letter which appear less frequently should use more bits.
	Ex: HEYYY
		Construct a tree for each letter with its count attached:
		H: 1, E: 1. Y: 3
		Iterate: Pick two tress whose counts are as small as possible, join them with a new root whose count is the sum.
		![[Pasted image 20250429194530.png]]
	Ex: WEEKEND:
		W:1, E:3, K:1, N:1, D:1
		![[Pasted image 20250429194701.png]]
		So:
			W = 000
			E = 1
			K = 001
			N = 010
			D = 011
			-> WEEKEND = 000110011010011 (15 bits)
	Time Complexity of building the tree:
		Suppose we start with our string,
		let p = # of letters in the string
		n = # different chars
		-> WEEKEND: p = 7, n = 5
		First we need to count the # of each letter. This can be done with a linked list, in $O(pn)$ time because the Linked List will have n items and we scan the string of length p to build it:
		Ex: [W:1]->[E:3]->[K:1]->[N:1]->[D:1]
		Then separate from the linked list:
			[W:1], [E:3], [K:1], [N:1], [D:1]
			Put all in a min heap (can be done in $O(n)$). Extract the top node, and then again: $O(\log n)$ twice, join them reinsert as a new node: $O(\log n)$
			We do this n - 1 times, of which point our min heap has one node and that's the final tree.
			All together: $O(np)+O(n)+O(n\log n)=O(np+n\log n)$

##  Lecture 32 4/30
#### Exam 3 review:
1. Karatsuba:
	- Goal: minimize SDM
	- Rewrote the mult via: split # s in half, removing one mult so we're left with 3 (instead of 4)
	- RR: $T(n) = 3T(\frac{n}{2})+\Theta(n)$
	- Q: suppose you broke your # s into 3 pieces each and removed some mult, what would the RR be?
	- A: RR analyzed via MT, Draw a tree to count the SDM
2. Basic Graph Stuff:
	- Graph, edge, vertex
	- Loops, mult edges. Simple graphs
	- (un)weighted
	- (un)directed
	- walk, trail, path, cycle, connected
	- adj matrix + list (representing graphs)
3. Alg - in general
	- What type of graphs?
	- Trace them across data structures
	- What type(s) of data structures are involved
	- What does each yield? How to interpret?
	- Time complexities.
4. Shortest Path Algorithm
	- Has a starting vertex
	- Finds SP to every other vertex
	- Unweighted, undirected graphs
	- Can have a target (not on the exam)
	- Used a Q
	- Produced dist, pred lists
	- Time: Adj matrix: $\Theta(V^2)$ Adj list: $\Theta(V+E)$
5. BFT:
	- Has a starting vertex s
	- Unweighted, undirected
	- Visits vertices close to s first
	- Vorder, etc
	- Did with a Q but also with no particular pseudocode
	- Time - same as Shortest Path Algorithm
6. DFT:
	- Has an s
	- Un weighted/ Un directed
	- Travels as far away as possible, only backing up when it have to
	- Vorder, etc
	- Did with a Stack:
		  V1: Allow pushing more than once. If we pop a vertex already visited, ignore.
		  V2: If pushing a vertex already on the stack remove earlier 
	- Time: For V2 same as SPA, BFT. For V1 don't worry
7. Dijkstra:
	- Shortest Path Algorithm for weighted graphs
	- Pred, dist list, set S
	- Starting vertex
	- Both pred, dist might need to be updated later in the algorithm
	- Time: Adj Matrix: $\Theta(V^2)$ Adj List: $\Theta(V^2+E)$
	- Weighted, undirected
8. Floyd
	- Finds all shortest paths
	- No starting vertex
	- Only works on Adj matrix, via pass-by approach
	- Time: $\Theta(V^3)$
	- Know what "pass-by" means
	- Dist, pred matrices
	- directed, weighted graphs. Neg weights allowed, but no neg cycles
9. Spanning Tree:
	- What is a ST?
	- What is a MST?
10. Prim's Algorithm:
	- Begins at some S
	- "Grows" a tree by iteratively choosing a vertex not already included which is connected by a min weight edge. Does this V - 1 times
11. Kruskal's Algorithm:
	- Throw out all edges first
	- Iteratively include an edge of min weight which doesn't form a cycle, V - 1 times
	- Tree only guaranteed at the end

## Lecture 31 5/5
Pand NP: Informalities, Decision and Non-decision problems
1. Intro: This last part of the course is about the categorization of two types of problems in terms of their O time complexity.
2. Informalities: Consider the O time for some things:
	- Time to sort a list of length n: $O(n^2)$
	- Time to determine of a list is sorted: $O(n)$
	- Given a list with n elements (think of it as a set), is a non-empty subset which adds to )?
	  Brute force: There are $2^n-1$ nonempty subsets and it takes $O(n)$ to check each, so that's $O(n*(2^n-1))$
	  Can we do this faster?
	- Given a list with n elements and a nonempty subset, does it add to 0? $O(n)$
	- Sudoku exists in all sizes $n^2 * n^2$ for $n \geq 1$![[Screenshot 2025-05-05 at 16.15.16.png]]
	  Given a partially filled $n^2 * n^2$ sudoku board, find a solution.
	  Each row has at most $(n^2)!$ possible arrangements $n^2$ rows so at most $(n^2)(n^2)!$ possibilities.
	  Try each one + test is slow
	- Given a filled board + a potential solution, check is it a solution? Easier, and time slightly better
3. Decision Problems:
	- Informally a decision problem is a Yes/No problem
	- Formally: A D.P. is a function denoted Q which takes an input called an instance and output either Y or N.
	  Ex:
		  Q: Given a list of length n, is it sorted?
		  if L = {3,2,1,4} then Q(L) = No
		  if L = {1, 2, 4, 5, 7} then Q(L) = Yes
4. Proofs/witnesses/certificates
	- Definition: If we have a Q and an I with Q(I) = Yes, a proof/witness/certificate is some evidence of that.
	  Ex:
		  Q: Given a set with n elements, is there a nonempty subset which adds to 0?
		  If I = {8, 6, -3, -1, 9, -4, 2} then Q(I) = Yes
		  and a proof is {8, -3, -1, -4} add up to 0
	- Note1: Just because Q(I) = Yes doesn't mean it's easy to find a proof.
	- Note2: When we say proof/witness we generally mean a valid proof/witness - we might have an invalid proof/witness. Having such a thing tells us nothing.
5. DP + non-DP
	- Point - often a non-DP problem will have one or more DP which are equivalently difficult.
	- Ex:
		  Non-DP: Given a set of n integers, find a nonempty subset which adds to O, if one exists. 
		  DP: Does one exist?

## Lecture 32 5/7
#### P and NP
1. Reminder: A decision problem Q takes an instance I and outputs either Yes or No.
2. Turing Machines:
	- A deterministic Turing Machine (DTM) is essentially our average everyday computer **Important**
	- A non-deterministic Turing Machine (NTM) is a theoretical computer which can branch infinitely as it processes
3. Definition of P:
	- DefinitionL P = set of all DPs such that we can write an algo. Which makes the decision (output Y or N) and that algo runs in polynomial time ($O(n^k)k \in Z$ on a DTM (everyday computer)
	- Ex: 
	  Q: Given a weighted, directed, simple, connected graph G with v vertices, and an integer $k \geq 0$. Are there two vertices x,y such that $dist(x, y) \geq k$?
	  Algo: Run Floyd which is $O(v^3)$. Check every pair, $O(v^2)$ say Y if we can find a pair, N otherwise.
	  Algo runs in $O(v^3) k \in Z$
	- Ex:
	  Q: Given a partially filled $n^2*n^2$ sudoku board, is there a solution?
	  Algo: Check every possible fill, but this is not in polynomial time.
	  So it don't exist a algo that run within the time limit and validate.
4. Definition of NP:
	- Original Definition:
	  NP = set of all DPs such that we can write an algo, which makes the decision (outputs Y or N) and that algo runs in polynomial time $O(n^k) k \in Z$ on a NTM.
	  This definition is confusing because NTM are confusing. There are more modern definitions.
	- Verifier definition:
	  We say $Q \in NP$ if we can write a polynomial time algo (called a verifier) on a DTM which takes both an instance $I$ and a potential witness/proof $X$ and then two things are true
	  1. If $V(I, X)$ outputs Y then there is a valid witness (not necessary X)
	  2. If $V(I, X)$ outputs N then $X$ is not a valid witness.
	- Ex:
	  Q: Given a set with n elements, is one of them $\geq 42$?
	  Verifier gets: 
		  $I$ = set with n elements
		  $X$ = some # in the set
		for out algo: check all elements ($O(n)$), say Y if one is $\geq 42$. N otherwise. We ignore X
		So Now:
		- If $I$ = {1, 10, 17, 48} and $X = 48$ outputs Y (Case 1.)
		- If $I$ = {1, 10, 17, 48} and $X = 17$ outputs Y (Case 2.)
		- If $I$ = {1, 10, 17} and $x = 17$ outputs N
		So $Q \in NP$

## Lecture 33 5/9
#### P vs NP
1. Theorem: $P \in NP$
	   Proof: Suppose $Q \in P$. Define V as follows: Given $I$ and $X$, ignore $X$, use the polynomial time algo for $Q$ to say Y or N, so then:
	- If $V(I, X)$ outputs Y then we know there is a valid witness (we don't know what it is) because we actually solved the problem and got Y, So (i) is satisfied
	- If $V(I, X)$ outputs N then there is no valid witness/proof (because if there were, we'd got T), so $X$ is not a valid witness.
2. Q: Is $NP \in P$ ? 
	   Nobody knows

#### Polynomial Reducibility
1. Intro:
	Suppose you want an algo such that given some n $\geq$ 0, it will tell you if Justin have at most $n in Justin's wallet, you have the function $djh(x)$ which says Y if Justin has exactly $X. N otherwise. (don't care how fast it runs)
	Here is the algo
```
function djhatmostn(n){
	for x = 0 to n inclusive{
		if djh(x) == Yes: return Y
	}
	return N
}
```
Here djhatmostn may not run in polynomial time but it runs in polynomial time as a "wrapper" around djh. If djh $\in \Theta(1)$ then djhatmostn would run in polynomial time.
Definition: We say djhatmostn is polynomially reducible to djh
	
2. Another example: Want a function which determines if a string ends with at least one X. Suppose we have a function which tells us if a string ends with at least two x's. No idea how fast it runs. (ex: endswxx)
```
function endswx(s){
	if endswxx(s+"x") {
		return Y
	} else {
		return N
	}
}	
```
Runs in polytime as a "wrapper" around endswxx. So endswx is polynomially reduciable to endswxx.
3. Fun math analogy:
   is $x^2 + x + 2$ a poly? Y
   is $(\sin x)^2+(\sin x)+2$  a poly? N
   is $(\sin x)^2+(\sin x)+2$  a poly function of $\sin x$? Y
4. Set theoretic Approach
	- Point: sets are all essentially decision problems. 
	  Given a set WT we simply ask - 
	  Given an x, is $x \in T$ or is $x \not \in T$? That's a Y/N question. 
	  Suppose we have two sets A and B. 
	  Suppose we have some way of telling if things are in B ( we don't care how fasts or how its done)
	  Can we use this to detect if things are in A?
	  Definition: Given setes A and B we say A is poly reducible to B if there is a function f which runs in poly time and has the property that $x \in A \iff f(x) \in B$  ($\iff$ means if and only if)
	- Ex:
		X = {strings which ends in at least one x}
		XX = {strings which ends in at least two x's}
		Show X is poly reducible to XX.
		Definition: f which takes a string s and appends an X
	  So $f(s)$+"x", lets prove:
			First:
				Prime if s $\in$ X then $f(s) \in$ XX.
				If $s \in X$ then $s = s_{0}x$ for some string $s_{0}$.
				Then $f(s)=$ s + "x" = $s_{0}x x \in XX$
			Second: 
				Prime if $f(s) \in XX$ then $s \in X$
				If $f(s) \in XX$ then $f(s) = s_{0}x x$ for some string $s_{0}$
				But $f(s)$ = s + "x" form the definition of $f$
				So s+"x" = $s_{0}x x$ so $s=s_{0}x \in X$
	- Ex:
		Let A = {x | x = 2j + 7 for some $j \in Z$}
		Let B = {x | x = 6k - 1 for some $k\in Z$}
		Show A is poly reducible to B.
		Definition: $f(x) = 3x-22$
		Lets prove $x\in A \iff f(x)\in B$
		-> Suppose $x\in A$ = $f(2j + 7)  = 3(2j+7)-22 = 6j+21-22 = 6j-1\in B$
		<- Suppose $f(x) \in B$ so $f(x) = 6k-1$ for some $k \in Z$. But also $f(x) = 3x-22$ by the definition of x
		So
$$
\begin{align}
3x-22 &=6k-1\\
3x &=6k+21\\
x &=2k+7\in A\\ \\
QED
\end{align}

$$
	  
## Lecture 33 5/12
#### Final Exam:
- Focus first on old finals, then exams, then Hw
- Focus on your weaknesses
- Focus on problems which arise frequently
- Less focus on quirky problems
- Final: 2hrs and 2x exam length

Algo in general:
- What does it do?
- What's the input and output
- High-levelL how does it work?
- Understand pseudocode - provided if needed
- Data structures involved
- Time complexity
- Run through examples
- What do the DS look like during the algo?
- What does ^ say about the object we're processing
  (Ex: sps pass-by-1 matrix for a graph looks like xxx, what does that tell you about the graph?)
Specific categories of Algos:
- Sorting - Aux space, stability, in-place
- Graph - what sort of graphs do they work on
- Katatsuba - How to split
- Binary Search - must be a sorted list
- etc
Broad Categories of Topics:
- Sorting - Bubble, select, insert, merge, heaps, quick, counting, radix
- Graph - SPA, BFT, DFT, Dijkstra, Floyd, Kruskal, Prim
- Other Algos - 
	  Coin changing (greedy and optimal dp version)
	  MCS (Brute force, D+C, Kadane)
	  Binary search
	  Karatsuba
	  Huffman
- Other topics -
	  $O, \Theta, \Omega$ definition
	  Limit theorems
	  Recursive Relations - eval values, digging down
	  Master Theorem
	  Limitation of Comparison Based Sorting Algorithm
	  Graph definition
P and NP:
- Definition of decision problems + non-decision problems
- Connections between the two
- Definition of P (Via construction of an algo which decides Q)
- Definition of NP (Via construction of an algo which obeys the two tules)
- Polynomial Reduce: With algos + with sets (with function)

Before this course:
- Basic logs + exponentials
- Basic limits + L'hop rule + derivatives
- Sums: $\sum_{i=1}^n 1, \sum_{i=1}^n i, \sum_{i=0}^n n^i$ + tweaks
- Proofs: Familiar, rearrange fragments, induction
