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
	2. Return average $\frac{1}{B}\sum_{j=\{1,\dots,B\}}y_j$ for regression, or majority vote for classification
## KNN
D: Training Data, 
K: # of neighbors that classification is based on, 
$\hat{x}$: Test instance with unknown class in {-1; +1}
![[Pasted image 20260308194308.png]]
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
**Hyperplane:** 
- Def: A cut that separates a $D$-dimensional space into two spaces. (e.g. in a 2D space, it is a line, in 3D space it is a plane.)
- In D-dimensions space, it is a $(D-1)$ dimensional hyperplane
- Defined by an outward pointing normal vector $w\in \mathbb{R}^D$, where $w$ is **orthogonal** to any vector lying on the hyperplane
- Hyperplane passes through the origin, unless we also define a $bias$ term b

**Binary classification via hyperplanes:**
1. Let's assume that the decision boundary is a hyperplane
2. Then, training consists in finding a hyperplane $w$ that separates positive from negative examples
3. At test time, we check on what side of the hyperplane examples fall $\hat{y}=sign(w^Tx+b)$

**Function Approximation with Perceptron:**
Problem setting:
- Set of possible instances $X$
	- each instance $x \in X$ is a feature vector $x=[x_{1},\dots,x_D]$
- Unknown target function $f: X\to Y$
	- $Y$ is binary valued $\{-1;+1\}$
- Set of function hypotheses $H=\{h|h:X\to Y\}$
	- Each hypothesis $h$ is a hyperplane in D-dimensional space
Input:
- Training examples $\{(x^{(1)},y^{(1)}),\dots,(x^{(N)},y^{(N)})\}$ of unknown target function $f$
Output:
- Hypothesis $h \in H$ that best approximates target function $f$

**Perception: Prediction Algo**
![[Pasted image 20260309013512.png]]

**Perceptron Training Algorithm:**
![[Pasted image 20260309013545.png]]

**Practical considerations:**
- The order of training examples matters: Random is better
- Early stopping: Good strategy to avoid overfitting
- Simple modifications dramatically improve performance: Voting or averaging

**Advanced Perceptron:**
1. The voted perceptron: acts like a committee of different models.
	- The logic: Every weight vector created during training $(w^{(1)},\dots,w^{(K)}$ gets to vote on the final classification
	- The weighting: The vote of each weight vector is multiplied by its survival time $c^{(k)}$
$$\hat{y}=sign(\sum_{k=1}^{K}c^{(k)}sign(w^{(k)}\cdot \hat{x}+b^{(k)}))$$
2. The averaged perceptron: more computationally efficient than voted perceptron
	- The logic: Instead of taking a sign(vote) for every intermediate weight, it calculates a weighted average of all the weight vectors first, and then makes one single prediction
$$\hat{y}=sign(\sum_{k=1}^{K}c^{(k)}(w^{(k)}\cdot \hat{x}+b^{(k)}))$$
- Require keeping track of "survival time" of weight vectors $c^{(1)},\dots,c^{(K)}$. Survival time counts how many training examples a specific weight vector $\Omega^{(k)}$ correctly classified before it made a mistake and had to be changed

**Convergence of Perceptron:**
- The perceptron has converged if it can classify every training example correctly
- If the training data $D=\{(x_{1},y_{1}),\dots,(x_N, y_N)\}$ is **linearly separable** with margin $\gamma$ by a unit norm hyperplane $w_*(||w_*||=1)$ with $b=0$, Then **perceptron training converges after $\frac{R^2}{\gamma^2}$ errors** during training (assuming $(||x||<R)$ for all $x$)

**Margin of a dataset $D$**
![[Pasted image 20260309020659.png]]

**Practical Implications:**
- Sensitivity to noise:
	- If the data is not linearly separable due to noise, no guarantee of convergence or accuracy
- Linear separability in practice
	- Data may be linearly separable in practice
	- Especially when # features >> # examples
- Risk of overfitting mitigated by
	- Early stopping
	- Averaging

## Binary Classification with Linear Models
**Definition:**
- Task: Separate data points in D-dimension with a hyperplane
- Given:
	1. An input space $X$
	2. An unknown distribution $D$ over $X\times\{-1,1\}$
- Compute: A function $f$ minimizing: $\mathbb{E}_{(x,y)\textasciitilde D}[f(x)\neq y]$

**Equations:**
1. Simple loss function
	- Defines learning as a pure minimization of training errors
	- 0-1 Loss: It uses an Indicator function $\mathbb{I}(\cdot)$, which simply counts mistakes. It returns 1 if the point is misclassified (where the sign of the prediction doesn't match the label $y_n$ and 0 if it is correct
	- Goal: This objective only cares about fitting the training data as perfectly as possible.
$$\min_{w,b} \sum_{n} \mathbb{I}[y_n(w \cdot x_n + b) < 0]$$
2. Loss function with regularization
	- Still measure how well the classifier fits the training data, but adds a second component to optimization problem
	- Regularizer $R(w,b)$: This term punishes overly complex solutions. It expresses a preference for models that are simpler (e.g. have smaller weights), which are statistically more likely to generalize well to unseen data
	- Hyperparameter $\lambda$: This controls the trade-off. A high $\lambda$ prioritizes simplicity (preventing overfitting), while a low $\lambda$ prioritizes training accuracy.
$$\min_{w,b} L(\mathbf{w}, b) = \min_{w,b} \sum_{n=1}^{N} \mathbb{I}(y_n(w^T x_n + b) < 0) + \lambda R(\mathbf{w}, b)$$
	- Where $R^{cnt}(w,b)=\sum_{d=1}^D\mathbb{I}(w_d\neq 0)$. In practice, because raw count is too slow (NP-hard), we use approximations like L1 norm (Lasso) or L2 norm (Ridge)

## Gradient descent
![[Pasted image 20260309052932.png]]

**Practical questions:**
- When to stop?
	- When the gradient gets close to zero
	- When the objective stops changing much 
	- When the parameters stop changing much
	- When performance on held-out dev set plateaus
- How to choose the step size?
	- Start with large steps, then take smaller steps

**Example for Gradient Calculation:**
1. Derivative with respect to $b$
$$\begin{aligned} \frac{\partial \mathcal{L}}{\partial b} &= \frac{\partial}{\partial b} \sum_{n} \exp \left[ -y_n(w \cdot x_n + b) \right] + \frac{\partial}{\partial b} \frac{\lambda}{2} \|w\|^2 & (6.12) \\ &= \sum_{n} \frac{\partial}{\partial b} \exp \left[ -y_n(w \cdot x_n + b) \right] + 0 & (6.13) \\ &= \sum_{n} \left( \frac{\partial}{\partial b} -y_n(w \cdot x_n + b) \right) \exp \left[ -y_n(w \cdot x_n + b) \right] & (6.14) \\ &= -\sum_{n} y_n \exp \left[ -y_n(w \cdot x_n + b) \right] & (6.15) \end{aligned}$$

2. Gradient with respect to $w$
$$\begin{aligned} \nabla_w \mathcal{L} &= \nabla_w \sum_{n} \exp \left[ -y_n(w \cdot x_n + b) \right] + \nabla_w \frac{\lambda}{2} \|w\|^2 & (6.16) \\ &= \sum_{n} \left( \nabla_w -y_n(w \cdot x_n + b) \right) \exp \left[ -y_n(w \cdot x_n + b) \right] + \lambda w & (6.17) \\ &= -\sum_{n} y_n x_n \exp \left[ -y_n(w \cdot x_n + b) \right] + \lambda w & (6.18) \end{aligned}$$**Subgradient**
- Problem: some objective functions are not differentiable everywhere (e.g. hinge loss, l1 norm)
- Solution: subgradient optimization
$$\begin{aligned} & \partial_w \max\{0, 1 - y_n(w \cdot x_n + b)\} & (6.22) \\ &= \partial_w \begin{cases} 0 & \text{if } y_n(w \cdot x_n + b) > 1 \\ -y_n(w \cdot x_n + b) & \text{otherwise} \end{cases} & (6.23) \\ &= \begin{cases} \partial_w 0 & \text{if } y_n(w \cdot x_n + b) > 1 \\ -\partial_w y_n(w \cdot x_n + b) & \text{otherwise} \end{cases} & (6.24) \\ &= \begin{cases} 0 & \text{if } y_n(w \cdot x_n + b) > 1 \\ -y_n x_n & \text{otherwise} \end{cases} & (6.25) \end{aligned}$$
![[Pasted image 20260309193005.png]]
## Bays
**Bayes' rule:**
$P(A|B)=\frac{P(B|A)*P(A)}{P(B)}$, we call $P(A)$ the "prior", and $P(A|B)$ the "posterior" 
$P(A)=P(A|B)P(B)+P(A|B')P(B')$ 

**Bayes Optimal Classifier:**
- Assume that we know the data generating distribution $D$ (That is, we know the distribution $P(y)$ and $P(x)$)
- We define the **Bayes Optimal classifier** as $f^{(BO)}(\hat{x})=arg \underset{\hat{y}\in Y}\max D(\hat{x},\hat{y})$
- Theorem: Of all possible classifiers, the Bayes Optimal classifier achieves the smallest zero/one loss
- Bayes error rate
	- Defined as the error rate of the Bayes optimal classifier
	- Best error rate we can ever hope to achieve under zero/one loss

**Training:**
- What does "training" mean in probabilistic setting?
- Training = estimating $D$ from a finite training set
	- We assume that training examples are Independently and Identically Distributed (IID) (i.e. as we draw a sequence of examples from $D$, the n-th draw is independent from the pervious n-1 sample)
	- We typically assume that $D$ comes from a specific family of probability distributions. (e.g., Bernouilli, Gaussian, etc)
	- Learning means inferring parameters of that distributions (e.g., mean and covariance of the Gaussian)

**Maximum Likelihood Estimates:**
- Assume we are dealing a coin flip situation
- Each coin flip yields a Boolean value for X $X \sim Bernouilli: P(X) = \theta^X(1-\theta)^X$
- Given a dataset D of IID flips, which contains $\alpha_{1}$ ones and $\alpha_{0}$ zeros
  $P_\theta(D)=\theta ^{\alpha_1}(1-\theta)^{\alpha_{0}}$
  $\hat{\theta}_{MLE}=arg\max_\theta P_\theta(D)=\frac{\alpha_{1}}{\alpha_{1}+\alpha_{0}}$

**Naive Bayes Assumption:**
$P(X_{1},X_{2},\dots,X_d|Y)=\prod_{i=1}^dP(X_i|Y)$
i.e., that $X_i$ and $X_j$ are conditionally independent given Y, for all $i \neq j$

**Conditional Independence**
- Def: $X$ is conditionally independent of $Y$ given $Z$, if $P(X|Y\cap Z)=P(X|Z)$ (In English, once you know Z, learning Y doesn't provide extra info about X)
- Recall that $X$ is independent of $Y$ if $P(X|Y)=P(X)$

**Naive Bayes Classifier**
- Why we need it: To use the exact Bayes Optimal Classifier, you need to calculate the likelihood: $P(X_{1},X_{2},\dots,X_d|Y)$. Means you need to calculate all possible combination of all features, if there are 20 binary features, there are $2^{20}$ combinations.
- Naive Bayes solve this by making a unrealistic assumption: every feature is conditionally independent of every other feature. Instead of calculating one massive joint probability, it calculate the probability of each feature individually and multiply them together
$$
\begin{align}
\hat{y} &= argmax_y P(Y=y|X=x) \\
&= argmax_y P(Y=y)P(X=x|Y=y) \\
&= argmax_y P(Y=y)\prod_{i=1}^dP(X_i=x_i|Y=y)
\end{align}
$$
**Convex**
- Def: A "bowl" shape function, if you draw a straight line segment between any two points on the curve, that line will lie entirely on or above the curve. This property is critical because it guarantees that **any local minimum is also the global minimum**.
- Convex test:
	1. Second derivative test:
	   If the loss function $f(x)$ is twice differentiable, this is the easiest test:
		- Single variable: Calculate the second derivative. If $f''(x) \ge 0$ for all possible inputs, it is convex. (For example, with squared loss $f(x) = x^2$, the second derivative is $2$, which is strictly positive).
		- Multivariable: Calculate the Hessian matrix (the grid of second-order partial derivatives). If the
	2. The tangent line test:
	   If the function is differentiable, the curve must sit entirely on or above all of its tangent lines for all points $x$ and $y$:
$$f(y) \ge f(x) + \nabla f(x)^T(y - x)$$

## Logistic Regression
Binary Classifier, by calculating the probability $P$ for a given data point $X^{(i)}$ belongs to class 1 or 0, given the model's learned parameters $\theta$

$P(Y^{(i)}=1|X^{(i)},\theta)=g(<\theta,X^{(i)}>)$
$P(Y^{(i)}=0|X^{(i)},\theta)=1-g(<\theta,X^{(i)}>)$
Note the term $<\theta,X^{(i)}>$ is the dot product of the weight $\theta$ and the input feature $X$

**Sigmoid**:
Map any real number into range 0-1, used to convert the raw activation into the probability
$g(z) = \frac{1}{1+\exp(-z)}$

**Cross-entropy loss:**
Logistic regression try to find the parameter $\theta$ to minimize the loss. We use gradient descent on this function
$\underset{\theta}{max} \sum_{i=1}^{N}Y^{(i)}\log g(<\theta,X^{(i)}>)+(1-Y^{(i)})\log(1-g(<\theta,X^{(i)}>))$
## Multiclass Classification:
**OVR (One-Versus-All):**
- Train K-many (where k is the # of classes) binary classifiers
- Classifier k predicts whether an example belong to class k or not
![[Pasted image 20260408210013.png]]
**AVA (All-Versus-All):**
- Train $\frac{K(k-1)}{2}$-many binary classifier
- Classifier k predicts whether an example belong to class k or not
![[Pasted image 20260408210024.png]]


$L_{\text{hinge}} = \sum_i \max(0,\ 1-y_i s_i)$