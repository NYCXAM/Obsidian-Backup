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
You feed different sentences that missing one word into a neural network, and let it predict what word go into the blank (I like ~~cats~~ because they are cute), and neural network will notice all words go in this blank are related to cute. If you do this with a rich training set then it will end up with a embedding space that encode the similarity between different words

 