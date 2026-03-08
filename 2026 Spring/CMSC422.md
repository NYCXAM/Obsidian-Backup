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

**Entropy:**
Entropy $H(X)$ of a random variable $X$, where $H(X)$ is the expected number of bits needed to encode a randomly drawn value of $X$ (under most efficient code)
$H(X) = -sum_{i=1}^{n}P(X=i)\log_2P(X=i)$

**Conditional Entropy:**
Conditional Entropy $H(Y|X)$ of a random variable $Y$ conditioned on a random variable $X$
$H(Y|X) = -\sum_{j=1}^{v}P(X=x_j)\sum_{i=1}^{k}P(Y=y_j|X=x_j)\log_2P(Y=y_i|X=x_j)$
![[Pasted image 20260304203343.png]]

**Information Gain:**
Decrease in entropy (uncertainty) after splitting, $IG(X)=H(Y)-H(Y|X)$

**Inductive bias:**
Decision tree algos typically prefer certain types of trees over others, such as shorter trees or trees with high information gain. It prefer a shorter tree because a short hypothesis that fits the data is less likely to be a statistical coincidence

**Overfitting:**
- Consider a hypothesis $h$ and its:
	- Error rate over training data $error_{train}(h)$
	- True error rate over all data $error_{true}(h)$
- We say $h$ overfits the training data if 
  $error_{train}(h) < error_{true}(h)$
- Amount of overfitting = 
  $error_{true}(h) - error_{train}(h)$

**Evaluating on test data:**
- Problem: we don't know $error_{true}(h)$
- Solution:
	- We set aside a test set (some examples that will be used for evaluation)
	- We don't look at them during training
	- After learning a decision tree, we calculate $error_{test}(h)$

**Bootstrapping:**
Bootstrapping is to make an inference about an estimate of statistic
Given:
- Observations $x_1,\dots,x_n$ and estimates $y=\frac{1}{n}\sum_{i=1}^nx_i$
- What can we say about the standard error of y? $S.E. = \frac{\sigma}{\sqrt{n}}$

**Bagging:** **B**ootstrap **ag**gregation
- Resampling a training set of size $n$ via the bootstrap:
	- Sample with replacement $n$ elements
- General scheme for random forests:
	1. Create B bootstrap samples, $\{Z_{1},Z_{2},\dots,Z_B\}$
	2. Build B decision trees, $\{T_{1},T_{2},\dots,T_B\}$, from $\{Z_{1},Z_{2},\dots,Z_B\}$
- Classification/Regression:
	1. Each tree $T_j$ predicts class/value $y_j$
	2. Return average $\frac{1}{B}\sum_{j=\{1,\dots,B\}}y_j$
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

## Perceptron

