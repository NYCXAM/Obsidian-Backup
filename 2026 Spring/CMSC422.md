## Lecture 1 2/3
Wording:
label = target
feature = vectors = observations 

**Hill-climbing:**
1. Start with a random solution (a randomly created route)
2. While new solution better than old solution:
	1. Find solution "neighbors"
	2. Get the best neighbor
	3. If best neighbor is better than solution: solution = best neighbor

**Simulated annealing:**
1. Set the temperature (T)
2. Set an initial random solution
3. While (T > 0):
	1. Get a random "neighbor"
	2. If neighbor better than solution: accept as new solution
	3. Else, accept with probability: $$