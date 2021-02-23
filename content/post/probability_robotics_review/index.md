---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Probability for Robotics Review"
subtitle: ""
summary: ""
authors: []
tags: 
- probability
- robotics

categories:
- theory

date: 2021-02-22T11:00:47-05:00
lastmod: 2021-02-22T11:00:47-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
#projects: []
---

## Introduction

In this blog post, I will go over some of the fundamental statistics that every roboticist will encounter when working or studying. I am assuming the audience will have knowledge of high school statistics and are familiar with the basic probability theory. At first, probability definitions are introduced and will continue to introduce common definitions and end with the Bayes probability theory. I am pumped, let's get started!

## Motivation 

No sensor is perfect! Imagine Rami bot, a romba like robot with a sonar distance sensor attached. Using the sensor we wanted to predict where the obstacles are with respect to the world frame (start point of the Rami). Since the sensor is not perfect, it might be noisy and predict the obstacle distance to be 1ft short than what it actually is. To help improve Rami's obstacle estimates being able to quantify the obstacles location is important not to crash. For example, it would be helpful to Rami if we can say that the obstacle is +/- 0.5 ft with a confidence of 90% is helpful to predict and plan a route compared to the deterministic value we get from the noisy measurements. This framework can be achieved using probability theory. Enough of the imagination, let's get to the cake.

## Axiomatic Probability Theory

`sample space`: universe of all possible outcomes of the experiments
$S = \cup s_i$ 

Let $s_i$ denote a possible outcome of an experiment. The collection of $s_i$ denoted by $S$ should be such that $s_i$ are 

- Mutually Exclusive: $A_i \cap A_j = 0; i \neq j$
- Exhaustive: Need to cover all possibilities of the outcome


`Event`: is any subset of the sample space $S$ 
Example: coin flip 
possible samples in the experiment are either heads or tails

$S =$ { tails; heads} 


$A =$ event heads -> {heads} 

`Random Variable`: Mapping from a sample space to a number. Can take multiple values 

$X : S \rightarrow \mathbb{R}$ 

Notation: In literature, it is conventional to use capital letters to indicate capital letters 

Example: Coin flip 

$X =$ {heads; tails}

If it takes a specific value, we indicate by $P(X=$ heads) i.e probability of getting heads 

properties: 
- The probability distribution must sum to 1 

  $\sum_{x} P(X = x) = 1$

- always non-negative 

  $P(X = x) \geq 0$

for simplicity, instead of using $P(X=x)$ from here, I will use $P(x)$

## Probability density 

$f(x) = \frac{ \partial F(x)}{\partial x}$
where $F(x)$ is the probability distribution function

In the robotics community, Gaussian/normal density function is widely used. 

parametrized by $\mu:$ mean, $\sigma^{2}:$ variance 

$$
p(X=x)=p(x)=\frac{1}{\sqrt{2 \pi \sigma^{2}}} e^{-\frac{(x-\mu)^{2}}{2 \sigma^{2}}}
$$

Multi-variate case: 
here $\boldsymbol{x}$ is vector instead of scalar 

$$
P(\boldsymbol{x})=\frac{1}{\sqrt{(2 \pi)^{N} \operatorname{det}(\mathbf{\Sigma})}} \exp \left(-\frac{1}{2}(\boldsymbol{x}-\boldsymbol{\mu})^{T} \boldsymbol{\Sigma}^{-1}(\boldsymbol{x}-\boldsymbol{\mu})\right)
$$

$\boldsymbol{\Sigma}$ is called covariance matrix 
properties: 
- Positive semi-definite
- Symmentric 

Don't worry if the above equation looks daunting, I am sure you will be seeing it in various literature and you will get comfortable with it. Takeaways are properties of the covariance matrix, and we need two parameters: mean, and variance is enough to get the normal distribution for a state $x$ 

## Conditional Probability

Often random variables carry information about other random variables. For example, LIDAR measurements can carry information about the robot's position.


$p(x\mid y) = p(X = x\mid Y = y)$ i.e probability of x given y 

$p(y) > 0$, $p(x\mid y) = \frac{p(x,y)}{p(y)}$

If x and y are independent $p(x,y) = p(x) * p(y)$

$p(x\mid y) = \frac{p(x) * p(y)} {p(y)} = p(x)$

make sense right? if $y$ has no information about $x$ then $y$ won't contribute anything by querying.

## Most Used Theorems

Below are the most used theorems in solving probabilistic proofs and problems in various books:


### Total probability:

$p(x) = \sum_{Y} p(x\mid y) p(y)$

{{< figure src="blog_post_1.png" title="Total probability disjoint sets"  numbered="true">}}

$B$ is the disjoint union of the intersection $(B \cap A_{i})$ i.e 

$B = \cup_{i = 3}^{6}$  $A_{i}$

$p(B) = p(\cup_{i = 3}^{6}$  $(B \cap A_{i}))$

$= \sum_{i = 3}^{6}$ $p (B, A_{i})$

$= \sum_{i = 3}^{6}$ $p (B\mid A_{i})$ $p (A_{i})$




### Chain rule 

$$
p\left(x_{1}, \ldots, x_{n}\right)=p\left(x_{n} \mid x_{n-1}, \ldots, x_{1}\right) p\left(x_{n-1}, \ldots, x_{1}\right)
$$

generally we can formulate as follows: 


$$
p\left(\cap_{k=1}^{n} x_{k}\right)=\prod_{k=1}^{n} p\left(x_{k} \mid \cap_{j=1}^{k-1} x_{j}\right)
$$

### Bayes rule 

$p(x\mid y) = \frac{p(y\mid x) * p(x)}{p(y)} = \frac{p(y\mid x) * p(x)}{\sum_{x'} p(y\mid x') p(x')}$ = $\frac{\text{likelihood} * \text{prior}}{\text{total probability}}$

sometimes the above can be written as follows: 

$p(x\mid y) = \eta * p(y\mid x) * p(x)$

the Bayes rule can also be used for more than one random variable 

$$
p(x \mid y, z)=\frac{p(y \mid x, z) p(x \mid z)}{p(y \mid z)}=\frac{p(z \mid x, y) p(x \mid y)}{p(z \mid y)}
$$

### Conditional Independence  


$$
p(x, y \mid z) = \frac{p(x, y, z)}{p(z)} = p(x\mid z)
$$

## Expectation

$E[X] = \sum_{x} x p(x)$

property: 
- linearity 

  $E[aX + b] = a * E[X] + b$


## Variance 

measures squared expected deviation $\sigma$

$$
\begin{array}{c}
\boldsymbol{\Sigma}(X, Y)=E[(X-E[X])(Y-E[Y])] \\
\Sigma(X, Y)=\left[\begin{array}{cc}
\sigma^{2}(x) & \sigma(x, y)=\sigma(x) \sigma(y) \\
\sigma(y, x)=\sigma(y) \sigma(x) & \sigma^{2}(y)
\end{array}\right]
\end{array}
$$

properties: 

- Bilinear: $\sigma(ax + by, z) = a\sigma(x,z) + b\sigma(y,z)$
- Symmentric: $\sigma(x,y) = \sigma(y,x)$ (very handy)
- Positive semi-definite: $\sigma^2(x) = \sigma(x,x) \geq 0$ $\forall$ random variable

If $\sigma(x,x) = 0$ then random variable is constant 


If a vector of random variable transformed by linear transform $A$ then covariance is also transformed as $\Sigma(Ax) = A \Sigma(x) A^T$

## Entropy

measures information content 

$H(X) = E[ -log_{2} p(X)]$

$= -\Sigma_{x} p(x) log_{2} p(x)$

can be very useful for estimating the gain of the information when our Rami takes a specific action. Usually, we seek to minimize the entropy to increase the gain of information 

## Bayes Filter 

`Complete state`: $x_t$ is a complete state if it encompases the necessary information about the past states in order to predict future states. Therefore, $x_t$ will be independent from the past events like past measurements $Z_{0:t-1}$, actions $u_{0:t-1}$, and previous states $x_{0:t-1}$


`Belief`: 

$bel(x_t) = p(x_t \mid z_{1:t}, u_{1:t})$


## sources:

- Probabilistic Robotics: https://mitpress.mit.edu/books/probabilistic-robotics
- http://bibing.us.es/proyectos/abreproy/70437/fichero/4_Capitulo2.pdf
- ENAE788K UMD class: Dr. Robert Sanner 