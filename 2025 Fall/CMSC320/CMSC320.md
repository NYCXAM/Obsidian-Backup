# Lecture 1

#### Writing documentations:
**Style**
- Same font, color
- Headers to divide sections
- Italics: Put emphasis on a specific thing.
- Bold: For things like specific terms. You can also use it for warnings. 
- Don't use `!`
- Avoid abusing bullet points
- Write heading by sections before filling details
**Report structure**:
- Introduction: explain relevant context, overview of what you’ve done, and a brief summary of results.  
- Background: extended summary of the data used and what it contained.  
- Methodology: How you did your analysis  
- Findings: Interesting results you produced  
- Future Work: Things needs to be done (optional)  
- Appendix: All the boring details that someone might need to look up later
**Wording**:
- Hard -> Difficult  
- Also -> In addition to  
- Right -> Correct
- Avoid first person and no unnecessary words

``
This question was chosen, as the relationship between someone's gender, background, and belief is still unclear. Recent news [some references] report male data  shows more conservative tendency, whereas female data show alignment with liberal ideologies. 

This questions was chosen and investigated to understand how parents' political believe affect their children

1 Data Issues
1.1: Optional Response
The data were collected with an optional survey experiment, resulting in the inconsistency of dataset. 

# Lecture 2  09/12/2025
**Visualization:**
Focus on:
1. Readability (rounding the numbers, not overly crowded, don't blind people with ugly color, font, etc)
2. Looks good (color and style)
3. Consistency

Consider:
1. Different heat maps might look the same due to population distribution
2. Seeing correlation between different event doesn't mean they are 100% correlated

## Lecture 5 09/16

**Types of Data:**
- Tabular: CSV/TSV
- Graph: JPG/PNG
- Geo:
- Raw: JSON/XML
- Hierarchies
- Text: TXT
- Time series: 
- Audio: WAV/MPG/DSD/FLAC


## Lecture 4:
**Pandas**:
- apply()
- group_by()
- hist()
- value_counts()
- Selecting entries based on key value matching (eg. df[df["col name"] >10] ...)
**Issues of Survey:**
- Wording could introduce bias
- Order of questions
- Desire of respondents to please
- Participants distracted when answering
- etc


## Lecture 5:
**Polling**:
- Control group: Simulates the normal situation where test case X didn't occur
- Treatment group: control group + test case X occurred 
**Replication Crisis:**
- Many scientific papers cannot be reproduced to verify the reliability and accuracy  




## Lecture 6:
**Distributions**:
- Uniform: Flat probability across values
- Normal(Gaussian): Bell curve, symmetric around mean
- Poisson: Counts events over time/space
- Bernoulli: Anything that has set probability of   happening
- Binomial: Successes in fixed trials
- Num. - Inflated distribution: looks like a type of distribution but a peak at Num.

**Central Limit Theorem**:
- Sampling: 

Summary stats are for after understanding the data

**Means**:
- Arithmetic Mean: Your typical average  
- Geometric Mean: A measure of central tendency less sensitive to outliers  
- Harmonic Mean: Primarily used for rates; included here so that if someone mentions it you can nod knowingly instead of looking confused
![[Pasted image 20250923144847.png]]

**Median and Mode**:
- Median: The value in the dataset that has an equal number of items greater than and less than it
- Mode: The most common item in your dataset

**Measures of Variance**:
- Sample Variance:
  $s^2 = \frac{\sum(x-\bar{x})^2}{n-1}$
- Sample Standard Deviation:
  $s = \sqrt{\frac{\sum(x-\bar{x})^2}{n-1}}$

**Descriptors**:
- Skew (tail)
  ![[Pasted image 20250923150245.png]]
## Lecture 7:
**Terms**:
- Population: total # of samples we can choose from 
- Population Mean: the actual mean of the population
- Sample Mean: the mean of samples we selected
- Null hypothesis: sample mean = population mean
- Alternative hypothesis: sample mean $\neq$ population mean
- P-value: the probability that we would have these observations under the null hypothesis 
- One sample T-test: takes a sample mean and observations and tells you how likely it is to make those observations if the sample mean is the same as the population mean.
- Chi Squared test: check if two sets of samples come from the same distribution
- Anova test: determine if there are statistically significant differences between the means of three or more independent groups
- Type I Error (False Positive): Occurs when you incorrectly reject a true null hypothesis (α).  
- Type II Error (False Negative): Occurs when you fail to reject a false null hypothesis (β).


## Lecture 10/07

**Z-Score:**
The Z-Score measures how far something is from the mean $Z = \frac{x-\mu}{\sigma}$
$Z =$ standard score
$x =$ observed value
$\mu =$ mean of the sample
$\sigma =$ standard deviation of the sample

**Default Values:**
Sometimes datasets use some default value (0, or some random number) to replace missing entries

**Boundary Conditions:**
If the value is greater than the boundary, there will be a clear cut off at the boundary

**Data Types:**
- Numeric
- Ordinal (GPA, number of stars, etc)
- Categorical (State, Major, hat color)
- Int, float, dates, strings

**Number of ways to edit columns:**  
- ``df[“column”].astype(someType)  ``
- ``df[“column”].apply(conversion_function)``
- ``df["column"].to_datetime()``
- ``df["column"].drop()

**Missing Values:**
- Hot Deck imputation: Find a similar row and fill in with that value


## Lecture 10/09
**Transforms:** Applying a function to certain cols, have two types: linear and non-linear

**Normalization:** Scale the sample value to range(0, 1)
$x_{scaled} = \frac{x-x_{min}}{x_{max}-x_{min}}$

**Standardization:** apply this formula and set the mean to zero and standard deviation to one
$z = \frac{x_i-\mu}{\sigma}$

**Log Transforms:** Move the outliers closer in, make the dataset looks more like a normal distribution 

**Hot Encoding:** ML models don't work well with categorical data, but if you do (Teacher = 0, Student = 1, Actor = 2 ...) the model will try to find the numerical relationships.

Hot encoding helps with categorical but sometimes can make the col size too big
![[Pasted image 20251009142043 1.png]]

**Binning:** taking a continuous feature and introducing a column that categorizes it.

**Clustering**: [k-means](https://www.naftaliharris.com/blog/visualizing-k-means-clustering/)
1. Assign k random centroid
2. Categorize points by closest centroid
3. For each group, calculate actual centroid of these points
4. Recalculate the distance and adjust the group of points
![[Pasted image 20251009144702 1.png]]

**Dimensionality Reduction:** 
- ==Covariance (ON EXAM)==
  ![[Pasted image 20251009145333 1.png]]

**Principal Component Analysis (PCA):**
- A tool for dimensionality reduction
- PCA on a dataset means: an ordered series of vectors along which the most cariance lies

## Lecture 10/21
**Classification Problem:**
- A target variable you want to predict (called "class" or "label")
- A set of data that target label is known
- New data that target label is unknown
- A model is a mathematical object that assign label to data without label


**K Nearest Neighbor (KNN):**
- Find the neartest k points to the new data
- Take the weighted avg of the labels based on the euclidean distance
- Assign that label

K values:
- K = 1: the model will be lack of the ability to generalize (e.g. on more complicated dataset)
- K = N: the model will just predict the mode of data


## Lecture 10/23

Smaller is better:
- Fewer rules: More generalized
- Many rules: More specificity

**Entropy Gain:**
The amount of entropy difference between different layers ![[Pasted image 20251023143705.png]]
**Decision Tree Training:**
- We want to get as close as possible to cutting the data in *half* with every split (to get the smallest tree)
- Means we want the splits with the most differences in **information** before and after the split



## Lecture 10/30 Classification
**Overfitting**:
Too good at predicting the training set, making decision based on entries that can't be generalized. (e.g. deciding if people can get the loan by that day's weather)

**Underfitting**:
Too generalized, it remember one rule and is applying it everywhere (e.g. only consider gpa for college application)


|                | Predict Pos | Predict Neg |
| -------------- | ----------- | ----------- |
| **Actual Pos** | True Pos    | False Neg   |
| **Actual Neg** | False Pos   | True Neg    |
**Accuracy:** What fraction of predictions were correct?
	=  $\frac{(TP + TN)}{(TP + TN + FP + FN} = \frac{\text{Correct predictions}}{\text{All predictions}}$

**Precision:** Of all the cases predicted as positive, how many are true positive?
	= $\frac{TP}{TP + FP}$

**Recall:** Of all the true positive, how many did we correctly identify
	= $\frac{TP}{TP + FN} = \frac{\text{True Positive}}{\text{All Positives}}$

## Lecture 11/04 Regression
**Do not use typical regression for time on x-axis**

**Linear Regression:**
*Needs:*
- Linearity: The data must have a linear relationship
- Autocorrelation: Errors must have no correlation
- Homoscedasticity: Variance in the outliers must be constant

We want to find the coefficients of the line: $f(x)=\beta_1x$ 
Which minimize:
$Error=\frac{1}{n}\sum_{i=1}^{n}(Y_i-f(X_i))^2$
![[Pasted image 20251104142540.png]]

==(IMPORTANT)**Gradient Descent:**==
Calculate the vector, follow the vector that allow us move down the most

How fast you go down( size of the small step): ==Learning rate==
![[Pasted image 20251104142855.png]]

*Pros:*
- Simple and effective for ML
- Scalable
*Cons:*
- Only applies to smooth functions (differentiable)
- Might be trapped inside local minimal instead reaching of the global minimal
**Examples:**
- Education affecting wages
- Weight and height
- Grade based on study habits

Exam extra credit:
Talked about not using regressions to time related tasks in class

**Polynomial Regression:**
Easy to overfit
Generally use it only for small degree polynomials

**Regularization:**


## Lecture 11/06 Time Series Data
**Time Series:** Always one to one, like stock price. Can't use normal regressions because you need to maintain the temporal ordering

**Autocorrelation**: Each datapoint in influenced by the pervious ones

**Plot:**
- Trend: upward or downward pattern that might affect future
- Periodicity: Repetition of behavior in a regular pattern
- Seasonality: Periodic behavior with a known period
- Stationarity: the mean and variance remain constant over time
- Heteroskedasticity: changing variance

**Missing Values:**
- Linear Imputation: good enough unless the gap is too big![[Pasted image 20251106144404.png]]
- $\text{info gain} = H(parent) - H(chi ld)$
- $\text{entropy} = -(1-p)\log_2(1-p)$ (Binary)
- or
- $\text{entorpy} = -\sum_x p(x)\log_2(p(x))$ (General)

## Lecture 11/18, 11/20 Neural Network
**Sigmoid:**

**Soft max:**

**Perceptron:**

**Multi-layer Perceptron:**


**Convolutional Neural Network:**
Light weight model for image classification,
	- Object classification: Classify what object is in the image
- Object localization: Classification + the location of that object in image
- Semantic segmentation: Label all pixels


**Transform learning:**



## Lecture 12/02 Transformer

**Natural Language Processing (NLP):**
Tokenize the input string -> Convert words into vector by their meaning (the closer two words are, the closer their vector are in the embedding space) -> 


**Embedding Space:**
You feed different sentences that missing one word into a neural network, and let it predict what word go into the blank (I like ___ because they are cute), and neural network will notice all words go in this blank are related to cute. If you do this with a rich training set then it will end up with a embedding space that encode the similarity between different words


## Lecture 12/04 Graph/Reviews

**Perceptron:**
- Definition: it is a linear binary classifier 
- Example: why binary classification only, not three or more classes?
  Because you can't represent more than two classes with only two signs (+ & -)
$$f(\mathbf{x}) = \begin{cases} 1 & \text{if } \mathbf{w} \cdot \mathbf{x} + b > 0 \\ 0 \text{ (or -1)} & \text{otherwise} \end{cases}$$
- Given input vector $x$, weight vector $w$, and bias $b$
  $w*x$: Dot product of weights and inputs
**Logistic Regression:**
- Definition: it is a linear classifier that predicts probabilities, output continuous value from 0 to 1 indicating the likelihood. This is done by passing the linear weighted sum through the sigmoid function.
  $\hat{y} = \sigma(w*x+b)$
- Difference with perceptron: Perceptron only updates weight when it makes a mistake, where logistic regression updates weights by gradient descent.

**Multilayer Perceptron:**
- Definition: it is a neural network with at least one hidden layer between the input and output. 
- Forward pass: two steps:
	- Calculate weighted sum with $z=(\sum x_i*w_i)+b$.  
	  $x_i$: input from previous layer, $w_i$: Weight on the connection, $b$: Bias of the current neuron. 
	- Apply Activation: Pass the weighted sum through the activation function (Sigmoid or ReLU)

**Activation Functions:**
- Sigmoid:
	  An S-shaped activation function 
	  $\sigma(z)=\frac{1}{1+e^{-z}}$
- ReLU:
	  Piecewise function that outputs the original input if it is positive, otherwise output 0
	  $f(z) = max(0,z)$

![[Pasted image 20251204144047.png]]
**Softmax:**
- It solves multi-class classification, where you can have multiple outputs, and you want to convert the raw output scores (logits, can be neg or pos or large) into a normalized probability distribution.
- Difference between normalize: softmax works great on negative values, and 
- ==Formula (memorize)==
  $P(y=i)=\frac{e^{z_i}}{\sum_{j=1}^{K}e^{z_j}}$
  For a vector of **K** raw scores (logits) $z$, this formula give value for the $i$-th class.
  Numerator ($e^{z_i}$): Exponentiates the individual score (makes everything positive)
  Denominator ($\sum e^{z_j}$): The sum of all exponentiated scores (normalizes the result)
- Properties:
  1. Sum of all outputs add up to 1, each output range from 0 to 1
  2. "Soft" Max: It highlights the largest value but doesn't completely zero out the others 
  3. Amplification: Because of the exponential, even small differences in the raw score result in large differences in probability.

**Batch:**

**Convolutional Neural Network:**
- Description: A deep learning method to use convolutional layers to scan through input images to find local features. CNN is able to learn complex patterns by combining local fields, weight sharing, and polling.
- **Convolutional Layers:**
	- Parameters:
	  **Stride**: How much overlap between different layers, Smaller value means higher overlap, capture more details but is more expensive and slow; Higher values means smaller overlap, more cheap and fast but can miss details.
	  **Zero Padding (adding border of 0s to the image):** It allow the center of layer to reach the very edge pixels of the input, and make sure every layer has the same size.
	  **Batch (Training parameter):** Controls how many samples are processed before the model updates its weights
	  **Local Receptive Field (Filter size):** Controls how many pixles each neuron in a layer "looks at" at once.
- **Pooling Layers:**
	- **Description**: A "downsampling" operation used to reduce the spatial dimensions of the feature map while keeping the depth intact. Usually done by max pooling (takes the maximum value in a window)
	- **Purpose**:
	  1. Reduce computation: fewer pixels to process in later layers
	  2. Translation Invariance: makes the network robust to small shifts. If a feature move slightly, the max value in the windows remain the same.
- **Fully Connected Layers:**
	- **Description**: These appear at the very end of the CNN. They take the high-level features learned by the conv layers (3D volumes) and flatten them into a 1D vector. 
	- **Latent Space:** This flattened vector represents the latent space. It is a compressed, abstract representation of the input image where "meaning" is stored (e.g. a specific combination of numbers in this vector represents "cat-like" features)
	- **Purpose:** Conv Layers are feature extractors, they don't make decisions. The FC layers act as the classifier. They look at the latenet space representation and combine those features to predict the final class. 
- **Auto Encoders:**
	- **Purpose:** Unsupervised learning used for dimensionality reduction, denoising, or compression.
	- **How they work:** 
	  1. Encoder: Compress the input down into a small latent space.
	  2. Decoder: Attempts to reconstruct the original input purely from that compressed latent representation.

**Word Embeddings:**
- **Description**: A technique that map words to vectors of real numbers. Instead of a sparse "one-hot encoding" (which is huge and has no meaning), an embedding is a dense vector where similar words have similar values (or in similar location)
- **Why do we use them and what they do**: 
	  1. Semantic meaning: it captures relationships between words. e.g. King and Queen will be closer than King and Apple.
	  2. Dimensionality Reduction: Compresses a large vocabulary into a manageable vectors for the model to process efficiently.
- **Methods of training:**
	- 1. Skip-gram: Predicts the context words based on the target word. e.g. Input "sat" $\to$ Predict ["The", "cat", "on", "mat"]. _Note:_ Skip-gram generally works better for infrequent words.
	- 2. CBOW (Continue Bag of Words): Predicts the target words based on the context (surrounding words). e.g. Input ["The", "cat", "on", "mat"] $\to$ Predict "sat".
- **Cosine Similarity**: We measure similarity by the angle between two vectors, not the Euclidean distance. 

**Transformers:**
- Overview: Transformer discards Recurrent Neural Networks entirely and relies on the Attention Mechanism. It processes the entire sequence of input data simultaneously (parallelization), rather than word-by-word (sequential).
- **Attention:**
	- **Why we need it:** RNNs struggle with long-range dependencies. If a sentence is very long, the network forgets the beginning by the time it reaches the end.
	- Attention allows the model to "look at" every other word in the sentence at the same time and decide which one are relevant to the current word being processed. 
- **RLHF** (Reinforcement Learning from Human Feedback):
	- **How/Why**: used to align the model with human intent (helpfulness, safety) after initial pre-tranning.
		1. Humans rank model outputs.
		2. Train a "Reward Model" to predict these ranking.
		3. Use RL to update the LLM to maximize the score form the Reward Model
	- **Sycophancy:** The model agrees with the user's wrong beliefs just to please them to get higher score.
- **System Prompt:**
	- Purpose: The "hidden" instruction given to the model before the user conversation starts. It defines the models' persona, boundaries, and rules. 
- **Interpretability Theory**:
	- ==Monosemanticity==: The ideal state where one neuron corresponds to exactly one concept. (e.g. a "husky" neuron that only activate for that dog)
	- ==Superposition==: The reality where neural networks store more concepts than they have neurons. They compress multiple unrelated concepts into a single neuron (using different linear combinations), making interpretability difficult because one neuron fires for "husky" and "ancient Roman houses".
- **MLPs as Fact Storage:**
	  In a transformer, the attention layers handle the grammar and relationships between words.
	  The Multi-Layer Perceptron (MLP) blocks are theorized to act as Key-Value Memories where facts are stored.
	  Input: "Capital of France" -> MLP Lookup -> Output: "Paris"
Know about RLHF, System Prompts

**Graphs:**
- **Link Prediction as Supervised Learning:** 
	- Definition: Predicting whether an edge (link) exists between two specific nodes that are currently unconnected (e.g. Should Facebook suggest this person as a friend)
	  To turn this into a classification problem, we treat pairs of nodes as our data points.
	  1. Data Creation:
	     - Positive Examples ($y=1$): Existing edges in the graph. (You might hide some edges during training to test if the model can "rediscover" them)
	     - Negative Examples ($y=0$): Pairs of nodes that do not have an edge between them (usually generated by random sampling, as non-edges are majority)
	  2. Feature Engineering:
	     - For every pair of nodes ($u,v$), we extract features (e.g. number of common neighbors, Jaccard coefficient, or distance between embeddings) 
	  3. Training:
	     - Train a standard classifier (like Logistic Regression or Random Forest on these pairs to predict the binary label (Edge/No Edge)
	     
- **Node Embeddings:**
	- Definition: Mapping each node in a graph to a low-dimensional vector (similar to word embedding)
	- Goal: To encode the structure of the graph into the vector space so that "similar" nodes are close together in that space
	- How They are generated:
		  1. Random Walks:
		     - The algorithm perfomrs random walks starting from a node to generate sequences of nodes (similar to sentences in NLP)
		     - It then uses Skip-gram to learn embedding  where nodes that appear often in the same walk are considered similar
		  2. Graph Neural Netowrk:
			  - Iteratively aggregate information from a node's neightbors to update its vector representation

- Structural Equivalence vs. Network Homophily:

|**Feature**|**Network Homophily**|**Structural Equivalence**|
|---|---|---|
|**Core Concept**|"Birds of a feather flock together."|"Similar roles in different places."|
|**Definition**|Nodes are similar if they are **connected** to each other or share the same neighbors.|Nodes are similar if they have the **same network topology** (local structure) around them, regardless of distance.|
|**Example**|Two students in the same study group who are friends. They are close in the graph.|The **CEO** of Amazon and the **CEO** of Google. They may not know each other (no edge), but they look the same mathematically (both connected to many distinct clusters of employees).|
|**Embedding Goal**|Embeddings should be close if nodes are **neighbors**.|Embeddings should be close if nodes perform the **same function** (e.g., hubs, bridges).|