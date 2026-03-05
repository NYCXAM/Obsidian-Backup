## Recap
**Dot Product (Similarity):**
$a \cdot b = \sum a_ib_i$, the dot product measure alignment, if two vectors point in the same direction, their dot product is large

**Norm:**
Norms measure the size or length of a vector:
- $L_{2}$ Norm (Euclidean):$||x||_2=\sqrt{\sum x^2_i}=||a||||b||\cos(\theta)$.
- $L_{1}$ Norm (Manhattan): $||x||_1=\sum|x_i|$

## Decision Trees:
**Representation:**
- Each internal node tests a feature
- Each branch corresponds to a feature value
- Each leaf node assigns a classification (or a probability distribution over classifications)
- Decision trees represent functions that map examples in X to classes in Y, $f: \text{<outlook, Humidity, Wind>} \to \text{PlayTennis?}$

**Function Approximation with Decision Trees:**
Problem setting
- Set of possible instances X
	- Each instance $x \in X$ is a feature vector $x =[x_{1},\dots ,x_n]$
- Unknown target function $f: X\to Y$
	- $Y$ is discreate valued
- Set of function hypotheses $H=\{h|h: X\to Y\}$
	- Each hypothesis $h$ is a decision tree
Input
- Training examples $\{(x^{(1)}, y^{(1)}),\dots,(x^{(N)},y^{(N)})\}$ of unknown target function $f$
Output
- Hypothesis $h \in H$ that best approximates target function $f$

**Decision Tree Learning:**
- Finding the hypothesis $h \in H$
	- That minimizes training error
	- Or maximizes training accuracy
- How?
	- $H$ is too large for exhaustive search
	- We will use a heuristic search algo which:
		- Picks questions to ask, in order
		- Such that classification accuracy is maximized
## KNN
**Two approaches to learning:**
- Eager learning:
	- Induce an abstract model from data
	- Apply learned model to new data
- Lazy learning:
	- Just store data in memory
	- Compare new data to stored data

**Hyperparameter K:**
- Tunes the complexity of the hypothesis space:
	- If k = 1, every training example has its own neighborhood
	- if  k = N, the entire feature space is one neighborhood
- Higher k yields smoother decision boundaries

**Epsilon Ball NN:**
- Same general idea as KNN, but change the method for selecting which training examples vote
- Instead of using K nearest neighbors, we use all examples x such that $dist(\hat{x},x)\le \epsilon$
$$
$$
## Perceptron
$$
$$
