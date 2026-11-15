+++
title = 'Point Estimation, Confidence Intervals, and Hypothesis Testing'
date = 2026-11-15
draft = false
description = 'Review notes on point estimation, confidence intervals, hypothesis testing, and the delta method.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 4*

These notes revisit how to estimate a parameter, construct a confidence interval, and test a hypothesis. They build on [Sampling Distributions, Limit Theorems, and Simulation](/posts/sampling-distributions-limit-theorems-and-simulation/).

## From Samples to Statistical Inference

1. Specify the data model, the sampling design, and the parameter of interest $\theta$.
2. Compute a point estimate $\hat\theta$ from the sample.
3. Find the exact sampling distribution of the statistic or justify an asymptotic approximation.
4. Construct a confidence interval or test a hypothesis. In A/B testing, the parameter is often a difference in mean outcomes or conversion rates.

| Estimator property | Definition |
| :--- | :--- |
| Unbiasedness | $E_\theta[\hat\theta]=\theta$ |
| Consistency | $\hat\theta_n\xrightarrow{P}\theta$ |
| Minimum variance | Compare variances among unbiased estimators. Under regularity conditions, the Cramér–Rao bound gives a lower bound that may be unattainable. |

A **sampling distribution** describes how a statistic varies across repeated samples. For a finite population of four measurements $(150,160,180,190)$ cm, all six unordered pairs are equally likely under simple random sampling without replacement. Their means are $155,165,170,170,175,185$. Thus $E[\bar X_2]=170$ and $\operatorname{Var}(\bar X_2)=250/3$ cm². With independent draws with replacement from the same population, the variance of the sample mean is $125$ cm².

## The Sample Mean, the CLT, and Slutsky’s Theorem

Expectation is linear when the expectations are finite. With finite second moments, the variance of $aX+bY$ is $a^2\operatorname{Var}(X)+b^2\operatorname{Var}(Y)+2ab\operatorname{Cov}(X,Y)$. Zero covariance is enough for the variances to add.

For i.i.d. observations with $E[X_i]=\mu$ and $0\lt \sigma^2=\operatorname{Var}(X_i)\lt \infty$, we have

$$
E[\bar X_n]=\mu,\qquad
\operatorname{Var}(\bar X_n)=\frac{\sigma^2}{n},\qquad
\frac{\sqrt n(\bar X_n-\mu)}{\sigma}\xrightarrow{d}N(0,1).
$$

The law of large numbers gives $\bar X_n\xrightarrow P\mu$. For large finite $n$, the sampling distribution of the mean is approximately $N(\mu,\sigma^2/n)$. For normally distributed observations under this i.i.d. model, the distribution is exact for every $n$.

$$
s^2=\frac1{n-1}\sum_{i=1}^{n}(X_i-\bar X_n)^2,\qquad
\frac{s}{\sigma}\xrightarrow P1,\qquad
\frac{\sqrt n(\bar X_n-\mu)}s\xrightarrow{d}N(0,1).
$$

The last convergence follows from the CLT and Slutsky’s theorem. The sample variance $s^2$ is unbiased for $\sigma^2$. Its square root $s$ is generally biased as an estimator of $\sigma$.

## Method of Moments

For the selected moment orders $k$, solve $m_k(\hat\theta)=\hat m_k$, where

$$
m_k(\theta)=E_\theta[X^k],\qquad
\hat m_k=\frac1n\sum_{i=1}^nX_i^k.
$$

For i.i.d. observations with $E_\theta\lvert X\rvert^k\lt \infty$, the sample moment converges to the population moment. Consistency follows when the selected moments uniquely identify the parameter and the inverse moment map is continuous at the true vector of moments.

| Model | Informative moments | Estimate |
| :--- | :--- | :--- |
| $N(\mu,\sigma^2)$ | $E[X]=\mu$, $E[X^2]=\sigma^2+\mu^2$ | $\hat\mu=\bar X$, $\hat\sigma^2=\hat m_2-\bar X^2$ |
| $\operatorname{Unif}(-\theta,\theta)$, $\theta>0$ | $E[X]=0$ for every $\theta$, $E[X^2]=\theta^2/3$ | $\hat\theta=\sqrt{3\hat m_2}$ |
| $\operatorname{Geom}(p)$, the trial number of the first success | $E[X]=1/p$ | $\hat p=1/\bar X$ |
| $\operatorname{Pois}(\lambda)$ | $E[X]=\operatorname{Var}(X)=\lambda$ | $\hat\lambda=\bar X$ |

The normal model uses **raw moments**. The method-of-moments variance estimator divides by $n$ and has expectation $(n-1)\sigma^2/n$.

## Confidence Intervals

The confidence level $1-\alpha$ describes coverage of a fixed parameter across repeated samples. For an asymptotic interval, coverage approaches the nominal level under the stated assumptions. For the symmetric intervals below, the half-width is the critical quantile times the standard error or its estimate.

Let $z=z_{1-\alpha/2}$ be the corresponding quantile of $N(0,1)$. Use $z\approx1.96$ for $95\%$ and $z\approx3.2905$ for $99.9\%$. The interval from $-3$ to $3$ contains about $99.73\%$ of the standard normal distribution.

| Target | Two-sided interval | Conditions |
| :--- | :--- | :--- |
| Mean, known $\sigma$ | $\bar X\pm z\,\sigma/\sqrt n$ | Exact for an i.i.d. normal sample. Asymptotic under the CLT assumptions. |
| Mean, unknown $\sigma$ | $\bar X\pm t_{n-1,\,1-\alpha/2}\,s/\sqrt n$ | Exact for an i.i.d. normal sample with $n\ge2$ and $\sigma>0$. |
| Mean, unknown $\sigma$, large $n$ | $\bar X\pm z\,s/\sqrt n$ | Asymptotic for i.i.d. observations with finite positive variance. |
| Difference in means | $(\bar X-\bar Y)\pm z\sqrt{s_X^2/n+s_Y^2/m}$ | Independent groups, i.i.d. observations within each group, and finite positive variances. Both sample sizes grow. |
| Poisson rate | $\hat\lambda\pm z\sqrt{\hat\lambda/n}$ | i.i.d. Poisson observations with fixed $\lambda>0$. |
| Difference in Poisson rates | $(\hat\lambda_2-\hat\lambda_1)\pm z\sqrt{\hat\lambda_1/n+\hat\lambda_2/m}$ | Two independent i.i.d. Poisson samples with positive rates. Both sample sizes grow. |
| Proportion | $\hat p\pm z\sqrt{\hat p(1-\hat p)/n}$ | i.i.d. Bernoulli observations with fixed $0\lt p\lt 1$. |
| Difference in proportions | $(\hat p_2-\hat p_1)\pm z\sqrt{\hat p_1(1-\hat p_1)/n+\hat p_2(1-\hat p_2)/m}$ | Two independent i.i.d. Bernoulli samples with $0\lt p_1,p_2\lt 1$. Both sample sizes grow. |

- For a difference of dependent sample means, the variance includes $-2\operatorname{Cov}(\bar X,\bar Y)$. With independent pairs, an interval can be built from the within-pair differences.
- Wald intervals for proportions can have poor coverage with small samples or rare successes or failures. For one proportion, the interval collapses to a point when $\hat p=0$ or $1$. A Wilson interval is one option in this setting. A Wald interval for a Poisson rate can have a negative lower bound and collapses to a point when $\hat\lambda=0$.
- For i.i.d. Bernoulli observations, $\operatorname{Var}(\hat p)=p(1-p)/n$. With unknown $p$, use $\lceil z^2/(4\Delta^2)\rceil$ for a target Wald interval half-width of $\Delta$, since $p(1-p)\le1/4$. The actual estimation error can exceed $\Delta$. An A/B test power calculation requires specifying the test and the target effect size.

## Hypothesis Testing

- Specify $H_0$, the direction of $H_1$, and the significance level $\alpha$. Use the statistic’s distribution under $H_0$ to calculate the probability of a result at least as extreme as the observed one, using the tail or tails specified by the alternative.
- Reject $H_0$ when the $p$-value is at most $\alpha$. A larger value gives insufficient evidence to reject $H_0$.
- A two-sided interval for a difference and the corresponding two-sided Wald test give the same decision about zero when they use the same standard error and matching confidence and significance levels. A one-sided test uses the corresponding one-sided confidence bound.

**One-sample $t$-test.** Two independent normal measurements $(150,190)$ cm with unknown population variance give $\bar X=170$, $s/\sqrt2=20$, and one degree of freedom. The exact $95\%$ $t$-interval is approximately $[-84.12,424.12]$ cm. Under $H_0:\mu=200$, the statistic is $t=-1.5$, with $p\approx0.187$ for $H_1:\mu\lt 200$ and $p\approx0.374$ for $H_1:\mu\ne200$. At $\alpha=0.05$, both tests fail to reject $H_0$.

**Comparing Poisson rates.** Two independent event streams were observed for $100$ days each. The sample means were $\hat\lambda_1=4$ and $\hat\lambda_2=5$ events per day. The $95\%$ Wald confidence interval for $\lambda_1$ is $[3.608,4.392]$. For $\lambda_2-\lambda_1$, the estimated standard error is $0.3$ and the interval is $[0.412,1.588]$. The interval for the difference excludes zero.

## Delta Method

Suppose $\sqrt n(\hat\theta_n-\theta)\xrightarrow{d}N(0,V)$ and $g$ is differentiable at $\theta$. The first-order approximation replaces $g(\hat\theta_n)-g(\theta)$ with $g'(\theta)(\hat\theta_n-\theta)$, yielding

$$
\sqrt n\,[g(\hat\theta_n)-g(\theta)]\xrightarrow{d}
N\bigl(0,[g'(\theta)]^2V\bigr).
$$

For $g'(\theta)\ne0$, the estimated standard error is $\lvert g'(\hat\theta_n)\rvert\sqrt{\hat V/n}$ when $V$ and the derivative are estimated consistently. The approximation is centered at $g(\theta)$. Convergence in distribution alone does not imply convergence of moments.

- **Reciprocal of the sample mean.** For $X_i\sim\operatorname{Unif}(2,8)$, we have $\mu=5$ and $\sigma^2=3$. For $g(x)=1/x$, the derivative at $5$ is $-1/25$. This gives $1/\bar X\approx N(0.2,3/(625n))$. For finite $n$, the expectation is strictly greater than $0.2$ by Jensen’s inequality.
- **Probability of a zero Poisson count.** The target is $p_0=e^{-\lambda}$ and its estimate is $\hat p_0=e^{-\bar X}$. The limiting variance of the scaled error is $\lambda e^{-2\lambda}$ and the estimated standard error is $e^{-\bar X}\sqrt{\bar X/n}$. Center the Wald interval at $\hat p_0$.
- **Zero derivative.** The first-order limit is degenerate. If $g'(\theta)=0$, $g''(\theta)\ne0$, and a second-order expansion is available, then $n[g(\hat\theta_n)-g(\theta)]\xrightarrow{d}\tfrac12g''(\theta)V\chi_1^2$ under the stated normal limit.
