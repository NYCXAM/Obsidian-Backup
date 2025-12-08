## Lecture 3:
We want to find the components inside a 

**Strong Component Def:**
Two vertices $u + v$ are in the same **strong component** if $\exists$ path $u$ to $v$ + $v$ to $u$ 
![[Pasted image 20250917230247.png]]

If we "==collapse==" all the vertices in each strong component, we obtain the ==component diagram==
![[Pasted image 20250917230500.png]]
Claim: The ==component diagram== is a ==DAG==

**Strong Components Algorithm (G = (V,E)):**
- Compute $G^R$
- Run DFS($G^R$) + record finish times
- Sort vertices reversely by finish times (magic order)
- Run DFS(G)
	- Whenever we start a new tree, use the above order
- Output DFS trees as Strong Components

## Lecture 4:

**Shortest Path Problem:**
Def: Given a diagraph with numeric edge weights, compute ==shortest paths==

Notation: G =(V,E) - The graph n = |V|, m = |E|
$w(u, v):$ weight of edge $(u, v) \in E$
==cost== of a path = sum of edge weights
==distance==  from $u$ to $v$ ($\infty$ if no path), Denoted $\delta(u,v)$ Note: $\delta(u, u) =0$
==single-source==: for a given source $s \in V$, compute $\delta(s,u)$ for all $u \in V$ (Also encode shortest path info)

**Preliminaries:**
- If edge weights are uniform (e.g. $w(u,v)=1, \forall(u,v)\in E$)
  fastest algorithm is breadth first search (==BFS==), $O(n+m)$ time
- Negative edge weights?
	- Used, eg, when edges model ==financial transactions== (Buy and Sell)
	- Shortest path are well defined ($\delta(s,u)$ may be negative) provided there are ==no negative-cost cycles==
- Path?
	  pred[u] points to ==prior vertex== on path from s to u. (pred[s] = null)
	  Follow these back = ==reverse path==
#### Bellman-Ford Algorithm:
- Solves the single source shortest path problem for arbitrary edge weights. (No neg. cost cycles)
- Runs in time $O(n*m)$
- Old and simple
**Basic:** Maintain for all $v \in V$
- d[v] = current ==distance estimate==
	  There is a path $s\to v$ of this cost but might not be the shortest $d[v] \geq \delta(s,v)$
- pred[v] = ==predecessor== based on d-value
	  If $u = pred[v]$ then $d[v] = d[u]+w(u,v)$
**Relaxation:** 
- Propagates shortest path forward one edge at a time
- $relax(u,v)$:
  if $d[u] + w(u,v) < d[v]$
  then $d[v] \leftarrow d[u] + w(u,v)$
	  $pred[v] \leftarrow u$

**Idea:** $relax(u,v), \forall(u,v) \in E$ ==Repeat until convergence (no value is getting smaller)==
![[Pasted image 20250918011516.png]]
**Pseudocode**:
```
belleman-ford(G=(V,E), w(weight), s(source))
	//init
	for each u in V
		d[u] = inf
		pred[u] = null
	d[s] = 0
	while converged = false
		converged = true
		for each (u,u) in E
			relax(u,v)
			if d[v] changed
				converge = false
	[pred links define an inverted shortest path tree]
```

**Runtime**: n =|V|  m = |E|
- Each ==repeat loop== takes $O(m)$ time
- Will show ==convergence== within n-1 iterations
- Total time: $O(nm)$

**Correctness**:
- Consider any shortest path
- Each iteration of the repea4t loop propagates distance one more edge

**Dijkstra's Algorithm:**
- Simple greedy algorithm for single-source shortest path
- Requires edge weights to be $\geq 0$
- The best from a worst-case perspective

## Lecture 5:
**Quiz 1:**
1. Graph combinative (num of edges, vertices, degree, etc)
2. Asymptotic (PP1) $n^2 = n + n^2 < n^3$
3. Bellman-Ford analysis
4. DFS and strong components. Discovery (d[v]), Finish (f[v]), back edge, forward edge, cross edge, strong components
5. Design algorithm DFS -> like alternating path (PP2), explanation, justification, run time

**Discrete Optimization:**
Compute a ==discrete structure== of a given class (eg. subset, tree, path, partition) to ==maximize/minimize== a given ==objective function== (eg., cost, distance, size) subject to a given set of ==constraints== (eg, disjointness, connectedness, completeness)

**Feasible solution:**
A feasible solution satisfies all constraints, An optimal solution is feasible and max/minimizes the objective function
Eg: Min. Spanning TreeL Given a connected, weighted graph compute:
- Structure: subset of edges
- Objective: min. total weight
- Constraints: connected, acyclic, cover all vertices

**Common Strategies:**
- ==Brute force search==: Try all possibilities
- ==Local search==: Find an init. feasible solution + repeatedly make small improvements
- ==Dynamic programming:== future lectures
- ==Greedy==: Build a solution by repeated additions  (never revoked/reversed) each based on best choice subject to constraints
  Eg: Kruskal's MST algorithm (add min weight edge that doesn't cause a cycle)

#### ==Interval scheduling:==
Given a set $R = {r_{1}\dots r_{n}}$ of requests, each being an interval $r_i = [s_i, f_i]$ compute a subset of ==non-overlapping requests= of max. cardinality==
Application: Scheduling events at a facility
![[Pasted image 20250921212917.png]]
**Greedy Approach:** Select a request that does not conflict with prior selection + min.some measure:
- Earliest Start: request with ==smallest start, $s_i$==
- Earliest Finish: request with ==smallest finish, $f_i$==
- Shortest Duration: request with ==smallest duration, $f_i-s_i$==
- Min conflicts: ==Overlaps fewest== among remaining requests
Optimal:
- ==Earliest Finish== = Latest start

Earliest Finish First (EFF) Pseuducode:
```
greedy-interval-schedule(s,f):
	sort requests by f-time
	# init empty schedule
	schedule = []
	prevFinish = inf
	for i in range(len(s))
		# if no conflict, schedule it
		if s[i] > prevFinish
			append i to schedule
			prevFinish = f[i]
```
![[Pasted image 20250921221926.png]]

**Running Time:**
- Sort: $O(n\log n)$ 
- Remaining processing: $O(n)$
- Total: $O(n\log n)$

**Correctness:** Must show
- Feasibility - A valid schedule (no conflicts)
  Easy - No tasks is scheduled until after prev request finishes
  
- Optimality- Maximizes # of tasks
  Not so easy:
  Let $O=[x_{1}, \dots, x_k]$ be any optimal schedule (may be many)
  Let $G = [g_{1}, \dots, g_k]$ be EFF greedy schedule ( $k' \leq k)$
  If G = O we're done
  Otherwise, let j be smallest index s.t. $s_j \neq g_j$
	  - By definition of EFF, $f[g_j] \leq f[x_j]$
	  - From a new schedule O' by replacing $x_j$ by $g_j$ - Still feasible + same size
	  - ![[Pasted image 20250921225114.png]]
	  - Repeart (hidden induction) until $O^{nm} = G \implies G$ is optimal
  


#### ==Interval partioning==:
Suppose we have $\infty$ many resources, but they cost $
Want to satisfy all requests with fewest resources
**Problem**:
Given a set $R = \{r_{1}, \dots , r_n\}$ of requests, each with start/finish times $s_i$ / $f_i$, partition R into the smallest num of sets $R_{1}, \dots , R_d$ such that all requests in $R_i$ are pairwise non-conflicting

Eg
![[Pasted image 20250921233133.png]]
This is an example coloring - Given a set of objects + a conflict relation, partition into smallest number of conflict free subset
==Number of subsets = coloring numbers==

An example of coloring: Graph coloring:
![[Pasted image 20250922003635.png]]
Greedy Strategy for Interval Partitioning Lowest Available Color
```
# s: start, f: finish
greedy-interval-partition(s,f):
	sort by start times
	for i in range(n):
		# X: excluded colorfs
		X = []
		for j in range(i-1)
			if [s[j], f[j]] overlaps [s[i], f[i]]:
				add color[j] to X
			color[i] = smallest color not in X
		
	return color array
```
![[Pasted image 20250922014951.png]]
Correctness: Need to show
- Feasibility: Avoid conflicting colors
- Optimality Approach:
	  - Define a statistic: depth
	  - Lower bound: any solution must use $\geq$ depth colors
	  - Upper bound: Greedy algorithm uses $\leq$ depth colors

- Depth: 
  Given task set $R = \{r_{1},\dots,r_b\},r_i = [s_i, f_i]$ + time t define
  $depth_R(t) =$ num. intervals overlap t = $|\{i|t \in [s_i, f_i]\}|$

- ==Schedule to minimize lateness==

## Lecture 6:
#### Scheduling to Minimize Lateness:
- Take rather than requests - $X = \{x_{1}, \dots, x_n\}$
	- Execution time = $t_i$
	- Deadline = $d_i$
- Application: homework assignments

**Objective:**
- Compute start times $S = \{s_{1}, \dots, s_n\}$ s.t.
	- Tasks do not overlap $[s_i, f_i] \cap[s_j, f_j] = \emptyset$ where $f_i = s_i + t_i$
	- Minimize lateness: max deadline excess $l_i = max(O) \underbrace{f_i, d_i}_\text{How far beyond the deadline did you finish}$ 
	- Maximize lateness: $L(S) = max(l_i) \underbrace{1\leq i\leq n}_\text{your worst deadline excess (not sum)}$
**Greedy approach:** Schedule based on some criterium 
- Shortest duration first - sort by $t_i$ (Not gonna work)
- Earliest deadline first - sort by $d_i$  (Will work)
- Smallest slack first - sort by $d_i - t_i$

**Pseudocode:**
```
greedy-lateness-scheduled(t, d):
	sort by deadlines
	# when prev task finished
	prev_finish = 0
	for i in range len(t):
		# start after preve finish
		s_i = prev_finish
		
		# update finish time
		prev_finish = f_i = s_i + t_i
		
		# lateness
		l_i = max(O, f_i - d_i)
	
	# return the start time
	return [s_1, ..., s_n] L = max(l_i)
	
```

**Time:**
- Sorting: $O(n\log n)$
- Processing: $O(n)$
- Total: $O(n\log n)$

#### K-center Clustering:
- Clustering: Given a set of points P + distance function, partition it into similar groups, called clusters $\{C_{1},\dots,C_k\}$

**Center-based clustering:**
- Compute a set of cluster centers $\{C_1,\dots,C_k\}$ and clusters are implicitly defined by distance $N(c_i) = \text{subset of P closed to } c_i$

Two varieties:
- Centers must be chosen from P  (discrete clustering)
- Centers can be any point in space

Three Common Center-Based Clustering:
![[Pasted image 20250923115835.png]]

**Which one is best?** 
- Depends on application (e.g., senstity to outliers)
- Helps to understand the single-cluster case (k=1)
- 1-median: 
	- 1-D -> median 
	- d-D -> too hard
![[Pasted image 20250923120638.png]]

- 1-means: 
	- 1-D -> mean
	- d-D -> centroid (center of mass)
	- Easy to compute in any dimension! Take mean coordinate value in each dimension
	- k-Means is very popular - Lloydi Algorithm (or k-Means algorithm)
- 1-center:
	- 1-D -> midpoint of min + max
	- d-D -> center of min enclosing ball (can compute in $O(n)$ time, but tricky algorithm)

**Metric Space:**
- Distance function $\delta: P$

## Lecture 7:
