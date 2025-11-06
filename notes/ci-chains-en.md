---
title: "Tutorial for Computer Scientists: Substitution Models"
---

* TOC
{:toc}


## Markov Chains (Markovove reťazce)

- A Markov chain is a simpler model than a hidden Markov model (HMM), as it contains only states and transition probabilities between them, but does not consider any emissions.
- Formally, a **Markov chain** is a sequence of random variables
  $X_0, X_1, \dots$
  such that
  $\Pr(X_t|X_0,\dots,X_{t-1}) = \Pr(X_t|X_{t-1})$,
  i.e., the state at time $t$ depends only on the state at time $t-1$ and not on any earlier states (states are the possible values of $X_{t}$).
- We are interested in **homogeneous** Markov chains, where
  $\Pr(X_t|X_{t-1})$
  does not depend on $t$.
- We also consider only chains where the random variables $X_t$ take values from a finite set.
- Example 1: weather model, states are rain / sunshine, $X_t$ is the weather on day $t$.
- Example 2: states A, C, G, T, can be used to generate random DNA or to track mutations at a specific position in a chromosome. In the latter case, $X_t$ is the base at that position at time $t$ (analogous to the weather).

### Transition Probability Matrix (matica pravdepodobnosti prechodu)

- Transition probabilities between states can be organized as a matrix $M$, where the element $M[x,y]$
  denotes the probability of transition from state $x$ to state $y$, i.e.,
  $M[x,y]=\Pr(X_t=y|X_{t-1}=x)$.
- The sum of each matrix row must be 1, and all elements must be non-negative.
- Independent coin tosses are also a special case of a Markov chain. What does its matrix look like?

Example (see also notebook):
```
0.767 0.233      0.067 0.933     0.30 0.70
0.100 0.900      0.400 0.600     0.30 0.70
--------------------------------------------------
Markov           Markov          Independent
chain            chain           coin tosses
with extended    with shortened  with Pr(X=1)=0.7
periods of       periods of
the same         the same
weather          weather
```

* All three matrices generate sequences with approximately 70% ones.
* Average length of consecutive zeros: 4.3, 1.1, 1.4.
* Average length of consecutive ones: 10, 2.5, 3.3.

### Computing the Matrix for Multiple Steps

- For a chain with matrix $M$, let us compute $\Pr(X_2=y \| X_0=x)$ (what is the distribution of weather two days from now given today's weather). We try all values $z$ at time 1 (tomorrow).
- We get $\sum_z \Pr(X_1=z \| X_0=x)\cdot\Pr(X_2=y \| X_1=z) = \sum_z M[x,z]\cdot M[z,y]$.
- This is the product of matrix $M$ with itself, i.e., $M^2$.
- Similarly, $\Pr(X_{t_0+t}=y \| X_{t_0}=x)$ is the entry $M^t[x,y]$ from the matrix $M^t$ (weather $t$ days later given today's weather).
- What is the running time of computing $M^t$ depending on $t$ and the number of states $n$?

```
1.000 0.000   0.767 0.233   0.611 0.389   0.507 0.493  ...  0.312 0.688
0.000 1.000   0.100 0.900   0.167 0.833   0.211 0.789  ...  0.295 0.705
t=0           t=1           t=2           t=3               t=10
```

### Stationary Distribution (stacionárne rozdelenie)

- A distribution $\pi$ over the set of states is called **stationary** for a Markov chain $M$ if for every $y$ it holds that $\sum_{x}\pi(x)M[x,y] = \pi(y)$ (or in matrix notation $\pi M = \pi$)
- If the matrix $M$ satisfies certain conditions (is ergodic), there exists exactly one stationary distribution $\pi$. Moreover, for every $x$ and $y$, $\lim_{t\to\infty} M^{t}[x,y] = \pi(y)$.
- This means that if we raise matrix $M$ to a very large power, all rows of the matrix approach the stationary distribution.
- After many steps, it no longer matters much which state we started in (the weather a year from now is not much affected by whether it rains today).
- The stationary distribution is also called the equilibrium of the Markov chain.

### Ergodic Markov Chains

- We say that matrix $M$ is **ergodic** if $M^t$ for some $t>0$ has all entries nonzero.
- Examples of non-ergodic and ergodic matrices:

```
  1 0          0.5 0.5          0 1             0.5 0.5
  0 1          0   1            1 0             1   0
  disconnected weakly connected periodic        ergodic
```


## Substitution Models

- Let $\Pr(a\stackrel{t}{\rightarrow} b)$ be the probability that if we start with base *a*, after time *t* we have base *b*, i.e., $\Pr(X_{t_0+t}=b \| X_{t_0}=a)$.
- For a given *t*, such probabilities can be arranged into a $4\times4$ matrix $S(t)$ (if we study DNA):

$S(t) = \left(\begin{array}{cccc} 
\Pr(A\stackrel{t}{\rightarrow} A) & \Pr(A\stackrel{t}{\rightarrow} C) & \Pr(A\stackrel{t}{\rightarrow} G) & \Pr(A\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(C\stackrel{t}{\rightarrow} A) & \Pr(C\stackrel{t}{\rightarrow} C) & \Pr(C\stackrel{t}{\rightarrow} G) & \Pr(C\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(G\stackrel{t}{\rightarrow} A) & \Pr(G\stackrel{t}{\rightarrow} C) & \Pr(G\stackrel{t}{\rightarrow} G) & \Pr(G\stackrel{t}{\rightarrow} T) \\\\\\\\
\Pr(T\stackrel{t}{\rightarrow} A) & \Pr(T\stackrel{t}{\rightarrow} C) & \Pr(T\stackrel{t}{\rightarrow} G) & \Pr(T\stackrel{t}{\rightarrow} T) 
\end{array}\right)$

- Rows correspond to the original base $a$, columns to the new base $b$.
- The sum of each row is 1.

### Discrete Time

- If we consider only integer values of time $t$, we can express the substitution model using a Markov chain with some matrix $M$, which will represent $S(1)$.
- The matrix $S(t)$ is obtained by raising $M$ to the power $t$, i.e., $S(t) = M^t$.

Example: for time t=1, each change (e.g., from A to C) has probability 1%, and no change thus 97%.

```
1 0 0 0   0.97 0.01 0.01 0.01  0.9412 0.0196 0.0196 0.0196
0 1 0 0   0.01 0.97 0.01 0.01  0.0196 0.9412 0.0196 0.0196
0 0 1 0   0.01 0.01 0.97 0.01  0.0196 0.0196 0.9412 0.0196
0 0 0 1   0.01 0.01 0.01 0.97  0.0196 0.0196 0.0196 0.9412
S(0)      S(1)                 S(2)

0.914 0.029 0.029 0.029  0.749 0.084 0.084 0.084  0.470 0.177 0.177 0.177
0.029 0.914 0.029 0.029  0.084 0.749 0.084 0.084  0.177 0.470 0.177 0.177
0.029 0.029 0.914 0.029  0.084 0.084 0.749 0.084  0.177 0.177 0.470 0.177
0.029 0.029 0.029 0.914  0.084 0.084 0.084 0.749  0.177 0.177 0.177 0.470
S(3)                     S(10)                    S(30)
```

* For small values of $t$, the probability of change is roughly $t$ times higher than for time 1.
* For example, in one unit of time, 1% of As change to Cs. In the next unit of time, another 1% change, but from the previous Cs, a small part changes to something else, so overall we see slightly less than 2% changes from A to C.
* As $t$ increases, we move away from this linear trend because the chance of multiple mutations increases and we see only one.
* What will be the stationary distribution for this matrix?


### Continuous Time

- In evolutionary studies, time $t$ is usually considered as a real number, which has several advantages (for example, we can differentiate the probability of a certain change with respect to $t$).
- We use a continuous-time Markov chain.
- For any $t$, we want to compute $S(t)$.
- It should still hold that $S(t_1+t_2) = S(t_1)\cdot S(t_2)$.

### Jukes-Cantor Substitution Model

- This model assumes that all substitutions are equally probable, so the matrix must look like this:

$S(t) = 
\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)$


- We want to derive the formula for $s(t)$, which we saw in the lecture. Let us compute the derivative of this function.
- By definition of the derivative, $s'(t) = \lim_{\epsilon\to 0} \frac{s(t+\epsilon)-s(t)}{\epsilon}$.
- $s(t+\epsilon)$ is computed from the product of matrices $S(t+\epsilon) = S(t)S(\epsilon)$.
- $\left(\begin{array}{cccc}
1-3s(t) & s(t) & s(t) & s(t) \\\\\\\\
s(t) & 1-3s(t) & s(t) & s(t) \\\\\\\\
s(t) & s(t) & 1-3s(t) & s(t) \\\\\\\\
s(t) & s(t) & s(t) & 1-3s(t) 
\end{array}\right)\cdot 
\left(\begin{array}{cccc}
1-3s(\epsilon) & s(\epsilon) & s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & 1-3s(\epsilon) & s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & s(\epsilon) & 1-3s(\epsilon) & s(\epsilon) \\\\\\\\
s(\epsilon) & s(\epsilon) & s(\epsilon) & 1-3s(\epsilon) 
\end{array}\right)$
- We get
  $s(t+\epsilon) = (1-3s(t))s(\epsilon) + s(t)(1-3s(\epsilon))+s(t)s(\epsilon)+s(t)s(\epsilon)$.
- After simplification
  $s(t+\epsilon) = s(\epsilon) +s(t) - 4s(t)s(\epsilon) = s(t)+s(\epsilon)(1-4s(t))$.
- $s'(t) = \lim_{\epsilon\to 0} \frac{s(\epsilon) (1-4s(t))}{\epsilon} = (1-4s(t))\lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon}= (1-4s(t))s'(0)$.
- Let $\alpha = \lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon}$ ($\alpha$ is a constant, independent of $\epsilon$ and $t$).
- We get $s'(t) = \alpha (1-4s(t))$. This is a differential equation; we seek a function $s(t)$ that satisfies it.
- The solution is $s(t) = 1/4+c e^{-4\alpha t}$ for any constant $c$.
- We can verify by substitution into the equation, where
  $s'(t) = -4 c \alpha e^{-4\alpha t}$.
- The value $c=-1/4$ is calculated from the initial condition $s(0)=0$.
- We also verify that $\lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon}=\alpha$. (L'Hospital's rule may be useful.)
- Since $s(0)=0$, we have  $\alpha = \lim_{\epsilon\to 0} \frac{s(\epsilon)}{\epsilon} =  \lim_{\epsilon\to 0} \frac{s(0+\epsilon)-s(0)}{\epsilon} = s'(0)$. 


### Properties of the Solution

- So we have the matrix:
$S(t)=
\left(\begin{array}{cccc}
(1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 \\\\\\\\
(1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1- e^{-4\alpha t})/4 & (1+3e^{-4\alpha t})/4
\end{array}\right)$

- When $t\rightarrow \infty$, all elements of the matrix become 1/4, i.e., $\lim_{t\to \infty}s(t)=\lim_{t\to \infty}1-3s(t)=1/4$.
- $\alpha=s'(0)$ expresses the rate at which mutations accumulate. It is the probability of a specific change per unit time, considering very short times.
- Models are usually normalized so that the average number of substitutions per unit time is 1. In the case of the Jukes-Cantor model, this is when $\alpha=1/3$.
- After such normalization, our "unit of time" is the time during which, on average, one mutation occurs at one position.

### Rate Matrix

- The substitution model is usually expressed by a rate (intensity) matrix (transition rate matrix, substitution rate matrix).
- It is a $4\times4$ matrix $R$, where $R[x,y]$ for $x\ne y$ is the rate at which a change from $x$ to $y$ occurs.
- More precisely, $R[x,y] = \\lim_{t\\rightarrow 0}\\frac{\Pr(x\\stackrel{t}{\rightarrow} y)}{t}$.
- Diagonal elements $R[x,x]$ are calculated so that the sum of each row is always 0; it is thus the rate at which base $x$ is lost by mutations.
- For the J-C model, the rate matrix is
$R=
\left(\begin{array}{cccc}
-3\alpha & \alpha & \alpha & \alpha \\\\\\\\
\alpha & -3\alpha & \alpha & \alpha \\\\\\\\
\alpha & \alpha & -3\alpha & \alpha \\\\\\\\
\alpha & \alpha & \alpha & -3\alpha
\end{array}\right)$

- For very small times, $S(t)\approx I+Rt$.
- $S(t+\epsilon) = S(t)S(\epsilon) \approx S(t)(I+R\epsilon)$ and thus $(S(t+\epsilon)-S(t))/\epsilon \approx S(t)R$
- In the limit, we get $S(t)R = \lim_{\epsilon\rightarrow 0} (S(t+\epsilon)-S(t))/\epsilon = S'(t)$.
- We obtained the differential equation $S'(t) = S(t)R$, initial state $S(0)=I$.
- If we substitute $R$ for the J-C model, we get the same differential equation as before.

### Jukes-Cantor Model, Summary

- $S(t)$ is a $4\times4$ matrix, where the entry $S(t)[a,b]=\Pr(a\stackrel{t}{\rightarrow} b)$ is
  the probability that if we start with base $a$, after time $t$ we have
  base $b$.
- The Jukes-Cantor model assumes that this probability is the same
  for any two bases $a\ne b$.
- For a given time $t$, we have $s(t)$ everywhere except the diagonal, and $1-3s(t)$ on the diagonal.
- Rate matrix $R$: for the Jukes-Cantor model, $\alpha$ everywhere except the diagonal, $-3\alpha$ on the diagonal.
- For very small time $t$, $S(t)$ is approximately $I-Rt$.
- The rate $\alpha$ is thus the probability of change per unit time, considering very short times, i.e., the derivative of *s(t)* with respect to *t* at point 0.
- Solving the differential equations for the Jukes-Cantor model, we get
  $s(t) = (1-e^{-4\alpha t})/4$.
- The rate matrix is usually normalized so that, on average, one substitution occurs per unit time, which is achieved if
  $\alpha=1/3$.


### Application to Estimating Evolutionary Distance

- At time $t$, the probability of observing a changed base is
  $D(t) = 3s(t) = \frac{3}{4}(1-e^{-4\alpha t})$
- In practice (computing the distance matrix for the neighbor-joining method), we have two aligned sequences, between which we see $d\%$
  changed bases, and we want to estimate $t$
- We thus calculate $t$ that would correspond to $D(t)=d$.
- We get the formula for distance, which we saw in the lecture
  $t=-\frac{3}{4} \log\left(1-\frac{4}{3}d\right)$
- If $d\rightarrow 0.75$, then $t\rightarrow \infty$
- Why did we derive the formula this way? In fact, we want to find
  the most likely value of $t$, i.e., one for which $\Pr(data|t)$
  is maximized. Coincidentally, it comes out this way.

### More Complex Substitution Models

- In more general models, different types of mutations occur at different rates, so after time $t$ we may get a different probability for A->C than for A->T.
- We thus have a more general rate matrix $R$
- $R = \\left( \\begin{array}{cccc}
  -\\mu_A & \\mu\_{AC} & \\mu\_{AG} & \\mu\_{AT}\\\\\\\\
  \\mu\_{CA} & -\\mu_C & \\mu\_{CG} & \\mu\_{CT}\\\\\\\\
  \\mu\_{GA} & \\mu\_{GC} & -\\mu_G & \\mu\_{GT}\\\\\\\\
  \\mu\_{TA} & \\mu\_{TC} & \\mu\_{TG} & -\\mu_T
  \\end{array} \\right)$
- The value $\mu_{xy}$ thus denotes the rate at which
  a particular base x changes to another base y.
- As we saw, the diagonal is calculated so that the sum of each row is 0, i.e., $\mu_x = \sum_{y\ne x}\mu_{xy}$
- Often, models are used where all necessary rates are calculated from a smaller number of parameters.

The **K80** model (Kimura 1980), for example, captures that purines more often change to other
purines (A and G) and pyrimidines to other pyrimidines (C and T).
- Transition is a change within a group (C<->T, A<->G), transversion is a change between groups {C, T } <-> {A, G}.
- The model has two parameters: transition rate $\alpha$, transversion rate $\beta$.
$R=\left(\begin{array}{cccc}
-2\beta-\alpha & \beta & \alpha & \beta \\\\\\\\
\beta & -2\beta-\alpha & \beta & \alpha \\\\\\\\
\alpha & \beta & -2\beta-\alpha & \beta \\\\\\\\
\beta & \alpha & \beta & -2\beta-\alpha 
\end{array}\right)$

The **HKY85** model (Hasegawa, Kishino & Yano, 1985) also allows for different probabilities of A, C, G, and T at equilibrium.
- If we set time in the evolutionary model to infinity, it does not matter which base we started from, the frequency of occurrence of individual bases stabilizes in the stationary distribution (equilibrium).
- In the J-C and K80 models, the probability of any base at equilibrium is 1/4.
- In HKY85, we also choose the frequencies of individual nucleotides at equilibrium $\pi_A,\pi_C, \pi_G, \pi_T$ with sum 1.
- Parameter $\kappa$: ratio of transitions to transversions ($\alpha/\beta$)
- Rate matrix:
  - $\mu_{x,y} =  \kappa \pi_y$ if mutation x->y is a transition,
  - $\pi_y$ if mutation x->y is a transversion

### Computing Probabilities from the Rate Matrix

- For complex models, we cannot derive an explicit formula for $S(t)$ as we had for the Jukes-Cantor model
- But in general, for a rate matrix $R$, we get $S(t)=e^{Rt}$.
  - The exponential function of a matrix $A$ is defined as
  $e^A = \sum_{k=0}^\infty{1 \over k!}A^k.$
  - If we diagonalize the rate matrix $R$ (which can be done for
  symmetric $R$) $R = U D U^{-1}$, where $D$ is a diagonal matrix (its diagonal will contain the eigenvalues of $R$), then
  $e^{Rt} = U e^{Dt} U^{-1}$, i.e., the exponential function
  is applied only to the diagonal elements of matrix $D$.
- Note the first two terms of the expansion $S(t) = e^{Rt} = \sum_{k=0}^\infty{1 \over k!}(Rt)^k$. We get $I + Rt$, which is our approximation of $S(t)$ for small $t$. With more terms, we get a better approximation.
