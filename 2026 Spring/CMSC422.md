## KNN
**Two approaches for learning:**
1. Eager learning (e.g. decision tree)
   - Induce an abstract model from data
   - Apply learned model to new data
2. Lazy learning (e.g. KNN)
   - Just store the data in memory
   - Compare new data to stored data

**Components of a KNN classifier:**
- Distance metric:
	- How do we measure the distance between instances?
	- Determines the layout of the example space (e.g. L1 is manhattan distance, l2 is euclidean distance)
	
- The k hyperparameter:
	- How large a neighborhood should we consider?
	- Determines the complexity of the hypothesis space


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
	3. Else, accept with probability: $\exp(-\frac{E-E_i}{T_i})$
	4. $T = T* \alpha$

**Genetic Algorithm:**
1. Make a random initial population
2. Apply population fitness function
3. While generations < num_generations
	1. For ($\frac{population\_size}{2}$):
		- Choose 2 parents according to population fitness function
		- Generate children
		- Mutate
	2. Add the new routes to the population
	3. Choose the best routes to stay in the population (maintaining population size)
	4. Update fitness function
4. Return the best solution from the population
![[Pasted image 20260217130159.png]]
Population fitness function:
- For each solution in the population:
	- Fitness = maximum cost - solution cost
	- Fitness probability = solution cost / $\sum$ population fitness
- Select parents according to their fitness probabilities

## Exam 1 Review:
**Complete:** An algorithm is complete if it is guaranteed to find a solution whenever at least one exists, and correctly reports failure when no solution exists.
**Finite search graphs:** A state space consisting of a limited, finite number of distinct nodes and transitions.