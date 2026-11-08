+++
title = 'Distributions and Descriptive Statistics'
date = 2026-11-08
draft = false
description = 'Review notes on probability distributions, numerical characteristics, sample statistics, and visualization.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 1*

These notes are for readers who have studied introductory probability and statistics and want a compact refresher. They summarize key definitions, formulas, and conditions.

## Probabilistic Model and Sample

- **DGP (Data-Generating Process)** — the process that generates the data, including the observation and sampling mechanisms.
- **Random variable** $X$ — the numerical outcome of an experiment; $x$ is a particular value.
- **Population** — the collection of objects or distribution about which conclusions are drawn; **sample** — the observations obtained.
- **Frequentist interpretation of probability** — the limiting frequency under repeated trials; **Bayesian interpretation** — a degree of belief given the available information.
- **i.i.d.** — independent and identically distributed observations: $X_1,\ldots,X_n\overset{\mathrm{i.i.d.}}{\sim}F$.
- **Statistic** $T=g(X_1,\ldots,X_n)$ — a function of the sample that does not depend on unknown parameters. It is random before the data are observed and takes a particular value for the observed sample.

Random sampling within an accessible group does not guarantee representativeness of the entire population. A large sample does not eliminate selection bias.

## Probability Distributions: PMF, PDF, CDF

The **cumulative distribution function (CDF)** is defined for every random variable:

$$
F_X(x)=P(X\le x),\qquad P(a\lt X\le b)=F_X(b)-F_X(a).
$$

Properties: nondecreasing, right-continuous, $0\le F_X\le1$; the limits as $x\to-\infty$ and $x\to+\infty$ are $0$ and $1$, respectively.

| | Discrete distribution | Absolutely continuous distribution |
| :--- | :--- | :--- |
| Specification | PMF: $p_i=P(X=x_i)$ | PDF: density $f_X(x)$ |
| Normalization | $p_i\ge0$, $\sum_i p_i=1$ | $f_X\ge0$, $\int_{-\infty}^{\infty}f_X(t)\,dt=1$ |
| Relationship to the CDF | $F_X(x)=\sum_{x_i\le x}p_i$ | $F_X(x)=\int_{-\infty}^x f_X(t)\,dt$ |
| Point probability | The CDF jumps by $p_i$ at $x_i$ | $P(X=a)=0$ |
| Interval probability | Sum of the probabilities of values in the interval | $P(a\le X\le b)=\int_a^b f_X(t)\,dt$ |

**Density is not probability:** it can exceed $1$; probability is the area under the density curve. For a distribution with a density, $F_X'=f_X$ almost everywhere; the CDF need not be smooth or S-shaped. Mixed and singular distributions also exist.

## Numerical Characteristics of a Distribution

### Expectation, Variance, Standard Deviation

$$
\mu=E(X)=\sum_i x_i p_i
\quad\text{or}\quad
\mu=\int_{-\infty}^{\infty}x f_X(x)\,dx.
$$

$$
\operatorname{Var}(X)=E[(X-\mu)^2]=E(X^2)-\mu^2,
\qquad \sigma=\sqrt{\operatorname{Var}(X)}.
$$

Finiteness conditions: $E|X|\lt \infty$ for the expectation, $E(X^2)\lt \infty$ for the variance. The expectation and standard deviation have the same units as the variable; the variance has squared units.

| Property | Formula | Condition |
| :--- | :--- | :--- |
| Linearity of expectation | $E(aX+bY+c)=aE(X)+bE(Y)+c$ | Finite expectations; independence is not required |
| Expectation of a product | $E(XY)=E(X)E(Y)$ | Independence and finite expectations |
| Scaling and shifting | $\operatorname{Var}(aX+c)=a^2\operatorname{Var}(X)$ | Finite variance |
| Variance of a sum or difference | $\operatorname{Var}(X\pm Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)$ | Independence and finite variances |

The expected signed deviation $E(X-E(X))=0$, so it does not measure spread.

### Quantiles, Median, Mode

- **Quantile at level $\gamma$:** $Q_\gamma=\inf\{x:F_X(x)\ge\gamma\}$, $0\lt \gamma\lt 1$.
- **Median $m$:** $P(X\le m)\ge1/2$ and $P(X\ge m)\ge1/2$. It need not be unique; $Q_{0.5}$ selects one of the medians.
- **Mode:** a most probable discrete value; for a density, a point of local maximum. There may be several modes.

The equality $F_X(Q_\gamma)=\gamma$ does not hold universally: the CDF may jump over level $\gamma$. It holds when the CDF is continuous.

## Common Distributions

The table gives PMFs for discrete models and PDFs for continuous models; they are zero outside the specified range.

| Distribution | Values | PMF or PDF | $E(X)$ | $\operatorname{Var}(X)$ |
| :--- | :--- | :--- | :--- | :--- |
| Bernoulli $\operatorname{Bern}(p)$ | $k\in\{0,1\}$ | $p^k(1-p)^{1-k}$ | $p$ | $p(1-p)$ |
| Binomial $\operatorname{Bin}(n,p)$ | $k=0,\ldots,n$ | $\binom nk p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ |
| Geometric $\operatorname{Geom}(p)$ | $k=1,2,\ldots$ | $(1-p)^{k-1}p$ | $1/p$ | $(1-p)/p^2$ |
| Poisson $\operatorname{Pois}(\lambda)$ | $k=0,1,\ldots$ | $\lambda^ke^{-\lambda}/k!$ | $\lambda$ | $\lambda$ |
| Exponential $\operatorname{Exp}(r)$ | $x\ge0$ | $re^{-rx}$ | $1/r$ | $1/r^2$ |
| Uniform $U[a,b]$ | $x\in[a,b]$ | $1/(b-a)$ | $(a+b)/2$ | $(b-a)^2/12$ |
| Normal $N(\mu,\sigma^2)$ | $x\in\mathbb R$ | $\dfrac1{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/(2\sigma^2)}$ | $\mu$ | $\sigma^2$ |

Parameters: $0\lt p\lt 1$, $n$ is a positive integer, $\lambda,r>0$, $a\lt b$, $\mu\in\mathbb R$, $\sigma>0$. Binomial coefficient: $\binom nk=n!/[k!(n-k)!]$.

**Relationships and interpretations:**

- Bernoulli describes a single binary outcome; the sum of $n$ independent $\operatorname{Bern}(p)$ variables follows $\operatorname{Bin}(n,p)$.
- Geometric describes the trial number of the first success in independent trials with constant success probability $p$. If failures before the first success are counted instead, the expectation is $(1-p)/p$.
- In a homogeneous Poisson process with rate $r$, the event count over time $t$ follows $\operatorname{Pois}(rt)$, and interarrival times follow $\operatorname{Exp}(r)$. **$r$ is the event rate; $\lambda=rt$ is the expected event count.**
- Exponential distribution: $F(x)=1-e^{-rx}$ for $x\ge0$, $F(x)=0$ for $x\lt 0$. Memorylessness: $P(X>s+t\mid X>s)=P(X>t)$, $s,t\ge0$. The scale $\beta=1/r$ is the reciprocal of the rate.

**Central limit theorem:** for i.i.d. observations with $E(X_i)=\mu$ and $0\lt \operatorname{Var}(X_i)=\sigma^2\lt \infty$:

$$
\frac{\sum_{i=1}^nX_i-n\mu}{\sigma\sqrt n}\xrightarrow{d}N(0,1).
$$

The normal approximation applies to centered and appropriately scaled sums and means; the individual observations need not be normally distributed.

## Sample Statistics

Ordered sample: $X_{(1)}\le\cdots\le X_{(n)}$; $X_{(i)}$ is the $i$th order statistic.

| Statistic | Formula |
| :--- | :--- |
| Mean | $\bar X=\frac1n\sum_{i=1}^nX_i$ |
| Variance with divisor $n$ | $S^2=\frac1n\sum_{i=1}^n(X_i-\bar X)^2=\overline{X^2}-\bar X^2$ |
| Bessel-corrected variance, $n\ge2$ | $s^2=\frac1{n-1}\sum_{i=1}^n(X_i-\bar X)^2=\frac n{n-1}S^2$ |
| Standard deviation | $S=\sqrt{S^2}$ or $s=\sqrt{s^2}$ |
| Median for odd $n$ | $X_{((n+1)/2)}$ |
| Median for even $n$ | $\big(X_{(n/2)}+X_{(n/2+1)}\big)/2$ |
| Interquartile range | $IQR=Q_3-Q_1$, where $Q_1=\hat Q_{0.25}$, $Q_3=\hat Q_{0.75}$ |

For an i.i.d. sample with finite variance:

$$
E(S^2)=\frac{n-1}{n}\sigma^2,\qquad E(s^2)=\sigma^2.
$$

- Divisor $n$: describing the spread of the observations; divisor $n-1$: unbiased estimation of the distribution's variance.
- $S^2$ underestimates the variance **on average across repeated samples**, not in every sample.
- The mean and standard deviation are sensitive to outliers; the median and IQR are more robust.
- Sample quantiles depend on the interpolation convention. With ties, the interval between the quartiles need not contain exactly $50\%$ of the observations.

## Empirical Distribution and Visualization

### Empirical CDF

$$
\hat F_n(x)=\frac1n\sum_{i=1}^n\mathbb I(X_i\le x).
$$

$\mathbb I$ is the indicator of the condition. A value occurring $m$ times creates a jump of $m/n$.

**Glivenko–Cantelli theorem:** for an i.i.d. sample,

$$
\sup_{x\in\mathbb R}|\hat F_n(x)-F(x)|\xrightarrow[n\to\infty]{\text{a.s.}}0.
$$

### Histogram and KDE

**Density histogram:** for a bin of width $h_j$ containing $n_j$ observations, the height is $\hat f_j=n_j/(nh_j)$ and the area is $n_j/n$. The total area is $1$ if every observation is counted exactly once. With unequal bin widths, frequencies are compared using areas rather than heights.

**Kernel density estimation (KDE):**

$$
\hat f_h(x)=\frac1{nh}\sum_{i=1}^nK\!\left(\frac{x-X_i}{h}\right).
$$

$h>0$ is the bandwidth; $K$ is a nonnegative kernel integrating to $1$, such as the standard normal density. Wide histogram bins or a large $h$ hide detail; narrow bins or a small $h$ amplify sampling noise.

### Boxplot

- **Box:** from $Q_1$ to $Q_3$; the line inside marks the median; the box spans the IQR.
- **Tukey fences:** $L=Q_1-1.5IQR$, $U=Q_3+1.5IQR$.
- **Whisker endpoints:** the most extreme observations within $[L,U]$, rather than the fences themselves.
- **Separate points:** observations beyond the fences; not necessarily data errors.

The CDF shows cumulative proportions, histograms and KDE show density shape, and boxplots show center, spread, and extreme values. Distributions with the same mean and variance can have different shapes.
