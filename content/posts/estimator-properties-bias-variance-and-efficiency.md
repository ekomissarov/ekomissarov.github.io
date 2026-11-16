+++
title = 'Estimator Properties, Bias–Variance Trade-off, and Efficiency'
date = 2026-11-16
draft = false
description = 'Review notes on unbiasedness, consistency, the bias–variance trade-off, regularization, and the Cramér–Rao bound.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 5*

These notes compare point estimators by their bias, variance, and behavior as the sample grows. They build on [Point Estimation, Confidence Intervals, and Hypothesis Testing](/posts/point-estimation-confidence-intervals-and-hypothesis-testing/).

## Unbiasedness and Consistency

An **estimator** $\hat\theta_n=T(X_1,\dots,X_n)$ is a rule that uses a random sample to produce an estimate. Before the data are observed, $\hat\theta_n$ is a random variable. Evaluating it on the observed data gives a numerical estimate. The symbols $E_\theta$ and $P_\theta$ refer to expectation and probability under the sampling distribution at the true parameter $\theta$.

| Property | Criterion for each $\theta$ |
| :--- | :--- |
| Unbiasedness at fixed $n$ | $E_\theta[\hat\theta_n]=\theta$ |
| Asymptotic unbiasedness | $E_\theta[\hat\theta_n]\to\theta$ |
| Consistency | $\hat\theta_n\xrightarrow{P}\theta$, meaning $P_\theta(\lvert\hat\theta_n-\theta\rvert\ge\varepsilon)\to0$ for every $\varepsilon>0$ |

All four combinations are possible for an i.i.d. sample from $N(\mu,1)$.

[Explore bias and consistency](/visualizations/bias-and-consistency/). Increase the sample size and compare which distributions narrow and where they concentrate.

| | Unbiased estimator of $\mu$ | Biased estimator of $\mu$ |
| :--- | :--- | :--- |
| Consistent | $\bar X_n$ | $\bar X_n+1/n$ |
| Inconsistent | $X_1$ | $\bar X_n+5$ |

The variability of $X_1$ persists as $n$ grows. The probability limit of $\bar X_n+5$ is $\mu+5$. For $\operatorname{Unif}(0,\theta)$ with $\theta>0$, the corresponding four examples are $2\bar X_n$, $2\bar X_n+1/n$, $2X_1$, and $\bar X_n$.

Convergence in probability allows rare but large errors. If $Z_n=n$ with probability $1/n$ and $Z_n=0$ otherwise, then $Z_n\xrightarrow{P}0$ even though $E[Z_n]=1$ and $\operatorname{Var}(Z_n)=n-1$.

[Explore how rare errors can drive up MSE](/visualizations/rare-costly-error/). Increase $n$ to see the probability of a nonzero error fall while the mean squared error grows.

## Sample Mean and Sample Variance

Assume i.i.d. observations with $E[X_i]=\mu$ and finite variance $\sigma^2$. The corrected sample variance $s^2$ is defined for $n\ge2$.

| Estimator | Formula | Expected value | Probability limit |
| :--- | :--- | :--- | :--- |
| $\bar X_n$ | $\frac1n\sum_iX_i$ | $\mu$ | $\mu$ |
| $\hat\sigma_n^2$ | $\frac1n\sum_i(X_i-\bar X_n)^2$ | $\frac{n-1}{n}\sigma^2$ | $\sigma^2$ |
| $s^2$ | $\frac{n}{n-1}\hat\sigma_n^2$ | $\sigma^2$ | $\sigma^2$ |

The identity $\hat\sigma_n^2=\frac1n\sum_iX_i^2-\bar X_n^2$ gives a short derivation. Since $E[X_i^2]=\sigma^2+\mu^2$ and $E[\bar X_n^2]=\mu^2+\sigma^2/n$, we get $E[\hat\sigma_n^2]=(1-1/n)\sigma^2$. Dividing the sum of squared deviations by $n-1$ removes the bias. The deviations from the mean sum to zero, leaving $n-1$ degrees of freedom.

By the law of large numbers, $\frac1n\sum_iX_i^2\xrightarrow{P}E[X_i^2]$ and $\bar X_n\xrightarrow{P}\mu$. Subtracting the squared sample mean gives the limit $\sigma^2$. Since $n/(n-1)\to1$, both variance estimators are consistent.

## Mean Squared Error and Regularization

For an estimator $\hat\theta$ with a finite second moment, expected squared loss decomposes as

$$
b(\theta)=E_\theta[\hat\theta]-\theta,\qquad
\operatorname{MSE}_\theta(\hat\theta)
=E_\theta[(\hat\theta-\theta)^2]
=\operatorname{Var}_\theta(\hat\theta)+b(\theta)^2.
$$

The cross term vanishes because $E_\theta[\hat\theta-E_\theta[\hat\theta]]=0$. For unbiased estimators, comparing MSE is equivalent to comparing variances. A biased estimator has lower MSE than an unbiased estimator if the reduction in variance exceeds the squared bias.

With finite second moments, $\operatorname{MSE}_\theta(\hat\theta_n)\to0$ is equivalent to both bias and variance tending to zero. Markov’s inequality then gives consistency. The example with $Z_n$ above shows that consistency alone can coexist with growing MSE.

**Variance estimation.** For i.i.d. normal observations with $n\ge2$ and $\sigma^2>0$,

$$
\operatorname{MSE}(\hat\sigma_n^2)=\frac{2n-1}{n^2}\sigma^4
\lt \frac{2}{n-1}\sigma^4=\operatorname{MSE}(s^2).
$$

[Compare variance estimators with divisors $n$ and $n-1$](/visualizations/variance-divisor/). Start with a small sample and compare how the reduction in variance outweighs the squared bias.

**Shrinking the mean toward zero.** Given three i.i.d. observations $Y_i$ with mean $\mu$ and variance $\sigma^2>0$, consider $\hat\mu_\gamma=\gamma\sum_{i=1}^3Y_i$. The estimator is unbiased at $\gamma=1/3$. For fixed $\mu$ and $\sigma^2$, the MSE and its minimizer are

$$
\operatorname{MSE}(\hat\mu_\gamma)=3\gamma^2\sigma^2+(3\gamma-1)^2\mu^2,
\qquad
\gamma_*=\frac{\mu^2}{\sigma^2+3\mu^2}.
$$

For predicting a new independent observation $Y_4$ from the same distribution, the expected squared prediction error is larger by $\sigma^2$. The optimal $\gamma$ stays the same.

With a quadratic penalty and $\lambda\ge0$, the minimizer is

$$
\hat\mu_\lambda
=\arg\min_a\left\{\sum_{i=1}^{3}(Y_i-a)^2+\lambda a^2\right\}
=\frac{\sum_{i=1}^{3}Y_i}{3+\lambda}.
$$

At $\lambda=0$, the estimator is $\bar Y_3$. Increasing $\lambda$ reduces variance and shrinks the estimate toward zero. For $\mu\ne0$, MSE is minimized at $\lambda_*=\sigma^2/\mu^2$. This optimum depends on the unknown population parameters. If $\mu=0$, the minimum is approached as $\lambda\to\infty$.

## Fisher Information and the Cramér–Rao Bound

[Explore the Cramér–Rao bound for Bernoulli trials](/visualizations/cramer-rao-bernoulli/). Increase the number of trials to see how the variance of the sample proportion follows the bound and how it compares with using just the first trial.

For one observation $X$ with density or probability mass function $f_\theta$, the score and Fisher information are

$$
S_\theta(X)=\partial_\theta\log f_\theta(X),\qquad
I_1(\theta)=E_\theta[S_\theta(X)^2].
$$

In a regular model, $E_\theta[S_\theta]=0$. When differentiation under the integral or sum is valid through second order, $I_1(\theta)=-E_\theta[\partial_\theta^2\log f_\theta(X)]$. Information adds across independent observations. For an i.i.d. sample, $I_n(\theta)=nI_1(\theta)$.

For an i.i.d. sample from a regular scalar-parameter model, an estimator with finite variance and bias $b(\theta)$ satisfies

$$
\operatorname{Var}_\theta(\hat\theta)
\ge\frac{[1+b'(\theta)]^2}{nI_1(\theta)}.
$$

The standard derivation requires valid differentiation under the integral or sum and finite positive information. With adequate smoothness, a common support avoids boundary terms. For $\operatorname{Unif}(0,\theta)$, the support boundary depends on $\theta$, so the standard derivation does not apply.

For an unbiased estimator, the numerator is $1$. Attaining the bound for every $\theta$ proves that the estimator is uniformly minimum-variance among unbiased estimators. A variance above the bound leaves the question of optimality open.

| Model for an i.i.d. sample | Score for one observation | $I_1$ | Estimator attaining the bound |
| :--- | :--- | :--- | :--- |
| $\operatorname{Bernoulli}(p)$, $0\lt p\lt 1$ | $(X-p)/[p(1-p)]$ | $1/[p(1-p)]$ | $\hat p=\bar X_n$, $\operatorname{Var}(\hat p)=p(1-p)/n$ |
| $N(\mu,1)$ | $X-\mu$ | $1$ | $\hat\mu=\bar X_n$, $\operatorname{Var}(\hat\mu)=1/n$ |
