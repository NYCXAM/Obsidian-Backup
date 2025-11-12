---
share_link: https://share.note.sx/b1kzjjuc#Oe9YVj1XSOSVBDuLoafNb4b7LcqDsXoRODrkWSWFW9o
share_updated: 2024-12-08T03:56:21-05:00
---
##  Lecture 25: Tuesday 11/19/2024 
### Inferential Statistics

**Probability vs. Inferential Statistics**

- **Probability:** Given that data comes from a particular distribution. How likely is a particular collection of a outcomes?

- **Inferential Statistics:** Say something meaningful about the population based on a sample.

#Examples

- We have coin. Is it fair?
- We have a company that claims that their batteries last 5 years. Is it reasonable based on sample data?

### Key Concepts

**Sample:** A subcollection of the population (entire collection of objects we want to study)
_**Definition**_: A random sample of size $n$ is collection  $x_{1}, x_{2}, \dots x_{{n}}$ of independent identically distribution random variables (iid rv)
![[Diagram 2.svg]]

**Statistic:** Some sort of summary based on out chosen sample.
_**Definition:**_ A function defined on the sample.

#Examples 
Let $x_{1}, \dots, x_{n}$ be a sample size of $n$.
- **Sample Avg:** $\overline{X}_n = \frac{\sum_{i=1}^n X_i}{n} = T(X_1, \dots, X_n)$ 
- **Sample Variance:** $S^2_n = \frac{1}{n-1}\sum_{i = 1}^{n}(x_{i} - \overline {x})^2 (= U(x_1, \dots, x_n))$
- **General Statistic:** For any function $V$,$$V(X_1,\dots,X_n) = f(X_{1},\dots ,X_n)$$
**Notes:** 
1) A statistic is a random variable and as such has its own distribution.
2) Central Limit Theorem talks about the approximate distribution of $\overline x_n$ for large $n$.
3) For precise definitions:
		- A sample must consist of observable quantities
		- A statistic must be calculable from the sample data

**Observation:** 
- Based on experience we know that $\overline x$ is useful as well as $S^2$. The statistic $V(x_1, \dots, x_n)$ is less so.


### Point Estimators

_Question:_ What makes a statistic "good"? 

Let $\theta$ be some parameter associated with the population. This parameter is fixed but unknown. Our goal is use the data (realized sample/observations) to determine the value of $\theta$.

_**Definition**_: An estimator for $\theta$, is a statistic used to approximate $\hat{\theta}$. This lead to a question:
What makes $\hat{\theta}$ a good estimator for $\theta$?

#Examples 
- $\overline X$ is a good estimator for $\mu$ but not for $\sigma$
$$
\begin{align}
E(\overline x) &= E(\frac{x_{1}+x_{2}+\dots+x_n}{n}) \\
&= \frac{1}{n}E(x_{1}+\dots+x_n)\\
&= \frac{1}{n}(E(x_{1})+\dots+E(x_n)) \\
&= \frac{1}{n} \cdot n\mu \\
&= \mu
\end{align}
$$
- $S^2$ is a good estimator for $\sigma^2$ but not for $\mu$
An estimator $\hat{\theta}$ for $\theta$ is said to be unbiased if $E(\hat{\theta}) = \theta$ for possible values of $\theta$.

### Computing the bias of $\hat{\sigma}^2$ as an estimator for $\sigma^2$:
We want to find E($\hat{\sigma}^2 - \sigma^2)$
Note that $\hat{\sigma}^2 = \frac{n-1}{n}S^2$
Therefore:
$$
\begin{align}
E(\hat{\sigma}^2) &= E(\frac{n-1}{n}S^2) \\
&= \frac{n-1}{n}E(S^2) \\
&= \frac{n-1}{n}\sigma^2
\end{align}
$$
Thus, the bias is:
$$
\begin{align}
bias(\hat{\sigma}^2) &= \frac{n-1}{n}\sigma^2-\sigma^2 \\
&=-\frac{\sigma^2}{n}
\end{align}
$$
_**Definition:**_ A sequence of estimator {$\hat{\theta_n}$} for $\theta$ is said to be consistent if for every $\epsilon > 0$ 
$$
\lim_{n} P(|\hat{\theta}_n-\theta|>\epsilon) = 0 
$$


## Lecture 26: Thursday 11/21/2024

**Three main parts of inference:**
1) Point estimation
2) Confidence intervals
3) Hypothesis testing

### Consistency:
Applies to a sequence of estimators

_**Definition**_: A sequence $\{\hat{\theta_n}\}_{n\in \mathbb{N}}$ of estimators for $\hat{\theta}$ is said to be consistent if for all $\epsilon > 0$ 
$$
\lim_{n} P(|\hat{\theta_n} - \theta| > \epsilon) = 0
$$
#Examples 
The sequence $(\overline x_n)_{n \in \mathbb N}$ is consistent for $\mu$. This is known as the Weak Law of Large Numbers.

Suppose you have three estimators for $\mu$:
Which estimator would you use if the sample $x_{1},\dots,x_n$ has $E(X_i) = \mu$ and $Var(X_i)=\sigma^2$?
- $X_{1}$
$$
E(X_{1}) = \mu
$$
- $\overline X_n$
$$
E(\overline X_n)=\mu
$$
- $\overline X_n^* = \frac{1}{n-2} \sum _{i = 2} ^{n-2} X_i$ (means don't count the first and last sample)
$$
E(\overline X_n ^*) = E\left( \frac{1}{n-2}\sum _{i=2} ^{n-2}X_i \right) = \frac{1}{n-2}\sum_{i = 2} ^{n-2} E(X_i) = \frac{1}{n-2}\sum _{i=2} ^{n-2}\mu = \mu
$$
So they all are unbiased estimators for $\mu$. But unbiasdness by itself is not good enough to give us the best estimator, although they are better than biased ones.

Recall the topic: **Consistency**
- $X_{1}$ only check the first sample so it is not consistent.
- $\overline X_n$ consistent, it is the average of entire sample space
- $\overline X _n ^*$ consistent

### Mean Square Error:

_**Definition:**_ Let $\hat{\theta}$ be an estimator. The **Mean Square Error (MSE)** of $\hat{\theta}$ represents the average squared deviation from $\hat{\theta}$ 
$$
MSE(\hat{\theta}) = E((\hat{\theta} - \theta)^2)
$$


**Comparing Estimators:** Given two estimators $\hat{\theta_1}$ and $\hat{\theta_2}$ for $\theta$, we say $\hat{\theta_1}$ is better than $\hat{\theta_2}$ if:
$$
MSE(\hat{\theta_1}) < MSE(\hat{\theta_2})
$$
**Facts:*
- $MSE(\hat{\theta}) = bias(\hat{\theta})^2 + Var(\hat{\theta})$
- For unbiased estimators, $MSE(\hat{\theta} = Var(\hat{\theta})$ since $bias(\hat{\theta}) = 0$

When choosing between two unbiased estimators, we choose the one with lower variance (this is known as the Minimum Variance Unbiased Estimator principle, or MVUE)

**Note:**
$$
\begin{align}
Var(\overline X_n) &= Var\left(  \frac{1}{n}\sum _{i=1} ^{n} X_i \right) \\
&= \frac{1}{n^2} Var\left( \sum _{i=1} ^{n} X_i \right) \\
&= \frac{1}{n^2} \sum _{i=1} ^{n} Var(X_i)\ (Since\ X_i\ are\ independent) \\
&= \frac{\sigma^2}{n}
\end{align}
$$
#Examples 
**Question:** Suppose you gave a coin that comes up head with probability $p$. Suggest a way to construct an estimator $\hat{p}$ for $p$.
- Toss the coin a number of times $n$, then $\hat{p} = \frac{\#\ of\ heads}{n}$ 

**Question:** How do we construct estimators if we are not familiar with the parameter?

Let $x_{1},\dots x_n$ be a sample of size $n$ with $x_i~Ber(p)$ where $0<p<1$ is fixed but unknown ($E(X_{1}) = p$)
$$
\hat{p} = \frac{1}{n} \sum _{i=1} ^{n} X_i
$$
- **Maximum Likelihood Estimators:** Find the value for the parameter that maximizes the probability of observing the given sample
- **Method of Moments Estimators:** 



_**MLE**_: let $f(x, \theta)$ be the pdf/pmf of the population. We define $L(\theta) = \prod _{i=1} ^{n}f(X_i, \theta)$ 
	Note: if $f$ is a pmf, 
$$
\begin{align}
P(X_{1} &= x_{1}, X_{2} = x_{2},\dots, X_n = x_n) \\
&= P(X_{1}=x_{1}, \theta) \cdot P(X_{2} = x_{2}, \theta)\cdot\dots\cdot P(X_n=x_n,\theta) \\
&= \prod _{i=1} ^{n} f(x_i,\theta)
\end{align}
$$

## Lecture 27: Tuesday 11/26/2024

**Two steps of applying MLE:**
1) Find $L(\theta)$
2) Find the value for $\theta, \theta_0$ such that $L(\theta_0 \geq L(\theta)$ for all possible values of $\theta$
	   - Critical point
	   - $1^{st} \text{or}\ 2^{nd}$ derivative test 
	   - $\hat{p}_{MLE} = \frac{\sum _{i=1} ^{n} X_i}{n}$ ($n$: # of toss, $X_i$: # of heads)

#Examples #Important
Let $x_{1},\dots,x_n$ be a sample of $n$ where $X_{i} \sim Ber(p)$ where $p$ is fixed but unknown
$$
f(x;p)=
\begin{cases}
p^x(1-p)^{1-x} & \text{x=0,1} \\
0 & \text{otherwise}
\end{cases}
$$
1. We need to find $L(\theta)$
Recall the likelihood formula:
$$
\begin{align}
L(p) &= \prod _{i=1} ^{n} f(x_i;p) \\
&= \prod _{i=1} ^{n} p^{x_i}(1-p)^{1-x_i}
\end{align}
$$
2. Find the value for $\theta, \theta_0$ that maximize value, let $X_{1}, X_{2}, X_{3}$ be sample of size 3 in this case, then:
$$
\hat{p}_{MLE} = \frac{1}{3}\sum _{i=3} ^{3} x_i
$$
Suppose $x_{1}=0, x_{2}=1, x_{3}=1$:
$$
\hat{p}_{MLE} = \frac{{0+1+1}}{3} = \frac{2}{3}
$$
#Examples 
Let this be the exponential distribution with parameter $\theta$:
$$
f(x;\theta)=
\begin{cases}
\theta e^{-\theta x} & x\geq0 \\
0 & \text{otherwise}
\end{cases}
$$
Let $\theta>0$ be fixed but unknown. Given a random sample $X_{1},\dots,X_n$ of size $n$ where each $X_i$ has pdf $f(x;\theta)$, find $\hat{\theta}_{MLE}$ 

_Solution:_
1. Find the $L(\theta)$
$$
L(\theta) = \prod _{i=1} ^{n} f(x_i;\theta) = \prod _{i=1} ^{n}\theta e^{-\theta x_i}
$$
Taking the natural logarithm:

$$
\begin{align}
l(\theta) &= \ln(L(\theta))  \\
&= \ln\left( \prod_{i=1}^{n} \theta e^{-\theta x_i} \right)  \\
&= \sum_{i=1}^{n} \ln\left(\theta e^{-\theta x_i}\right)  \\
&= \sum_{i=1}^{n} \left( \ln(\theta) - \theta x_i \right)  \\
&= n \ln(\theta) - \theta \sum_{i=1}^{n} x_i
\end{align}
$$
Taking the derivative:

$$
l'(\theta) = \frac{n}{\theta} - \sum_{i=1}^{n} x_i
$$

Setting $l'(\theta) = 0$ and solving:
$$
\frac{n}{\theta} - \sum_{i=1}^{n} x_i = 0
$$
$$
\theta = \frac{n}{\sum_{i=1}^{n} x_i}
$$
Therefore: 
$$
\hat{\theta}_{MLE} = \frac{n}{\sum_{i=1}^{n} x_i}
$$
To verify this is a maximum:
$$
l''(\theta) = - \frac{n}{\theta^2} < 0
$$
Since $l''(\theta) < 0$ for all $\theta > 0$, by the second derivative test, $l(\theta)$ is a maximum.

#Examples 
Multiple parameters:
Let $x_{1},\dots,x_n$ , $X_i \sim N(\mu, \sigma^2)$ with both $\mu$ and $\sigma^2$ unknwon?
$$
f(x; \mu, \sigma) = \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x_i-\mu)^2}{2\sigma^2}}
$$

$$
L(\mu,\sigma) = \prod _{i=1} ^{n} \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x_i-\mu)^2}{2\sigma^2}}
$$

$$
\begin{align}
l(\mu,\sigma) &= \ln \left( \prod _{i=1} ^{n} \frac{1}{\sqrt{2\pi}\sigma} \right)e^{\frac{-(x_i-\mu)^2}{2\sigma^2}} \\
&= \sum _{i=1} ^{n} \ln \left( \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x_i-\mu)^2}{2\sigma^2}} \right) \\
&= \dots \\
&= -\frac{n}{2} \ln(2\pi) - n \ln(\sigma) - \frac{1}{2\sigma^2} \sum_{i=1}^{n} (x_i - \mu)^2
\end{align}
$$

## Lecture 28: Tuesday 12/03/2024

### Final Exam:
- 12/12 10:30 - 12:30
- 9 Questions, at least 3 of them will be materials after midterm 2
- 2hrs

#Examples MLE
Let $x_{1},\dots,x_n$ be a sample of size $n$, with $x_i \sim N(\mu, \sigma^2)$ where $\mu$ and $\sigma^w$ are both unkown. Find the MLE for $\mu$ and $\sigma$

$$
f(x;\mu, \sigma) = \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x-\mu)^2}{2\sigma^2}}
-\infty<x<\infty
$$

Step1: Find the likelihood function:
$$
\begin{align}
L(\mu,\sigma) &= \prod _{i=1} ^n f(x_i;\mu,\sigma) \\
&= \prod _{i=1} ^n \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x_i-\mu)^2}{2\sigma^2}} \\
 \\
l(\mu,\sigma) &= \ln(L(\mu,\sigma)) \\
&= \sum _{i=1} ^n \ln\left( \frac{1}{\sqrt{2\pi}\sigma}e^{\frac{-(x_i-\mu)^2}{2\sigma^2}}\right) \\
&= \ln\left( \frac{1}{\sqrt{2\pi}} \right)+\ln\left( \frac{1}{\sigma} \right)+\ln\left( e^{\frac{-(x_i-\mu)^2}{2\sigma^2}} \right) \\
&= -n \ln(\sqrt{2\pi}) - n\ln(\sigma)-\frac{1}{2\sigma^2}\sum _{i=1} ^n (x_i - \mu)^2 \\
 \\
\frac{\partial l}{\partial \mu} = 0  \\
\frac{\partial l}{\partial\sigma} = 0
\end{align}
$$
Find the derivative
$$
\begin{align}
\dots\\
\frac{\partial l}{\partial \mu} = \frac{1}{\sigma^2}\sum _{i=1} ^n (x_i-\mu)\\
\text{Set: }  \frac{\partial l}{\partial \mu} &= 0 \text{\ to find MLE for }\mu\\
\mu &=\frac{\sum _{i=1} ^nx_i}{n}
\end{align}
$$

$$
\begin{align}
\dots \\
\\\frac{\partial l}{\partial \mu} = -\frac{n}{\sigma} + \frac{1}{\sigma ^3} \sum _{i=1} ^n (x_i - \mu)^2 \\
\text{Set: } \frac{\partial l}{\partial \sigma} &= 0 \text{ to find MLE for }\overline x \\
\overline x &= \frac{\sum _{i=1} ^n x_i}{n}
\end{align}
$$
$$
\begin{align}
\hat{M}_{MLE} &= \frac{\sum _{i=1} ^n X_i}{n}\\
\hat{\sigma}_{MLE} &= \sqrt{\frac{1}{n}\sum_{i=1}^n (x_i - \overline x)^2}
\end{align}
$$
Similar to CLT, for MLE:
$$
\frac{\hat{\theta}_{MLE}-\theta}{\sqrt{V(\hat{\theta}_{MLE})}}
$$
### Method of Moments Estimator (MM): 

Strategy based on Laws of Large Number:
$$
\frac{\sum_{i=1}^n X ^k _i}{n} \approx \overset{\text{k-th moment}}{E\left( X_i^k \right)} = \overline X
$$
Parameters are related to moments.

#Examples #Important
Suppose $x_{1},\dots,x_n$ is a sample of size $n$ with $X_i \sim U[0,\theta]$ where $\theta>0$ is fixed but unknown. Compute the MM estimator for $\theta$.
- Calculate the necessary moment:
  $E(X) = \frac{\theta}{2}$
- Use Law of Large Numbers:
$$
\begin{align}
E(X) &\approx \frac{\sum _{i=1} ^{n}x_i} {n}\\
\frac{\theta}{2} &\approx \frac{\sum _{i=1} ^{n}x_i} {n}\\
\hat{\theta}_{MM} &= \frac{2}{n}\sum_{i=1}^n x_i
\end{align}
$$
#Examples 
$x_{1},\dots, x_n$ is a sample of size $n$ with 
$$
f(x;\theta) = 
\begin{cases}
\theta e^{-\theta x} & x\geq 0 \\
0 & \text{otherwise}
\end{cases}
$$
$$
X_i \sim \exp(\theta)
$$
Given MLE for $\theta = \frac{n}{\sum _{i=1} ^n X_i}$ for convenience
_Find MM for $\theta$:_
$$
\begin{align}
E(X) &= \frac{1}{\theta}\\
\text{from LLN: } E(X) &\sim  \frac{\sum _{i=1} ^n X_i}{n} \\
\frac{1}{\hat{\theta}_{MM}} &= \frac{\sum _{i=1}^nX_i}{n} \\
\hat{\theta}_{MM} &= \frac{n}{\sum_{i=1}^n X_i} = \hat{\theta}_{MLE}
\end{align}
$$