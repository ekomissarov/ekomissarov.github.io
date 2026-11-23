+++
title = 'Confidence Intervals: Exact and Asymptotic'
date = 2026-11-23
draft = false
description = 'Review notes on coverage, exact and asymptotic confidence intervals, prediction intervals, and intervals for means and variances.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 6*

These notes revisit confidence interval coverage and the assumptions behind exact and asymptotic constructions. They continue [Estimator Properties, Bias–Variance Trade-off, and Efficiency](/posts/estimator-properties-bias-variance-and-efficiency/).

## Coverage and Interpretation

A confidence interval $[L(X),U(X)]$ with confidence level at least $1-\alpha$ satisfies

$$
P_\theta\bigl(L(X)\le\theta\le U(X)\bigr)\ge1-\alpha
\qquad\text{for all }\theta\in\Theta.
$$

The parameter is fixed; the endpoints are random before the data are observed. Coverage is the proportion of intervals that contain the true parameter over repeated applications of the same procedure. Once a particular interval has been calculated, the frequentist interpretation does not assign a probability of $0.95$ to the parameter lying inside it.

| Type of interval | Guarantee |
| :--- | :--- |
| Exact coverage | The coverage probability equals $1-\alpha$ at a finite sample size under the assumed model |
| Conservative | Coverage is at least $1-\alpha$ |
| Asymptotic | The required coverage is attained in the limit as the sample size increases |

Comparing procedures requires considering both coverage and interval length, such as expected length. A narrower interval in a single realization or a point estimator with lower variance does not, by itself, establish that a procedure is optimal.

**Notation.** $z_p$, $t_{p,\nu}$, $\chi^2_{p,\nu}$, and $F_{p,\nu_1,\nu_2}$ denote quantiles at probability level $p$, defined using the cumulative probability from the left. All intervals below have nominal confidence level $1-\alpha$, $0\lt \alpha\lt 1$. For $95\%$ confidence, use $\alpha=0.05$ and $z_{0.975}\approx1.96$.

[Explore confidence interval coverage](/visualizations/confidence-interval-coverage/). Compare the number of intervals containing the true mean across repeated samples.

## Construction Using a Statistic with a Known Distribution

If the distribution of $T(X,\theta)$ does not depend on unknown parameters, choose bounds $a,b$ such that $P_\theta(a\le T\le b)=1-\alpha$, then invert the inequality to obtain bounds for $\theta$. Asymptotic intervals use the limiting distribution of $T$.

For i.i.d. observations, write

$$
\bar X=\frac1n\sum_{i=1}^nX_i,\qquad
s^2=\frac1{n-1}\sum_{i=1}^n(X_i-\bar X)^2,\qquad n\ge2.
$$

## The Mean: Normal-Model and Asymptotic Intervals

| Assumptions | Interval for $\mu$ | Status |
| :--- | :--- | :--- |
| i.i.d. $N(\mu,\sigma^2)$, known $\sigma$, $n\ge1$ | $\bar X\pm z_{1-\alpha/2}\sigma/\sqrt n$ | Exact |
| i.i.d. $N(\mu,\sigma^2)$, unknown $\sigma$, $n\ge2$ | $\bar X\pm t_{1-\alpha/2,n-1}s/\sqrt n$ | Exact |
| i.i.d., $0\lt \sigma^2\lt \infty$, $n\to\infty$ | $\bar X\pm z_{1-\alpha/2}s/\sqrt n$ | Asymptotic |

All normal models assume $\sigma^2>0$. The exact Student's t result follows from the independence of $\bar X$ and $s^2$ and the distributions

$$
\frac{\sqrt n(\bar X-\mu)}{\sigma}\sim N(0,1),\qquad
\frac{(n-1)s^2}{\sigma^2}\sim\chi^2_{n-1},\qquad
\frac{\sqrt n(\bar X-\mu)}s\sim t_{n-1}.
$$

**Slutsky's theorem:** if $A_n\xrightarrow{P}c$ and $B_n\xrightarrow{d}B$, then $A_nB_n\xrightarrow{d}cB$. The factors need not be independent. With finite positive variance, $s\xrightarrow{P}\sigma$, so

$$
\frac{\sqrt n(\bar X-\mu)}s
=\frac{\sigma}s\frac{\sqrt n(\bar X-\mu)}\sigma
\xrightarrow{d}N(0,1).
$$

For the same $s>0$, the interval using a $t$ quantile is wider than the one using a $z$ quantile; the quantiles approach each other as $n\to\infty$. Comparison with an interval using the true $\sigma$ depends on the relationship between $s$ and $\sigma$.

The normal interval has half-width $z_{1-\alpha/2}\sigma/\sqrt n$: quadrupling $n$ halves it. Increasing the confidence level while holding the data fixed widens these intervals.

## Confidence and Prediction Intervals

A confidence interval estimates a parameter. A prediction interval is intended for a new random observation; its endpoints may also depend on the data.

For an i.i.d. sample from $N(\mu,\sigma^2)$ with unknown parameters and a new independent observation from the same model:

| Target | Exact interval for $n\ge2$ |
| :--- | :--- |
| Mean $\mu$ | $\bar X\pm t_{1-\alpha/2,n-1}s/\sqrt n$ |
| New observation $X_{n+1}$ | $\bar X\pm t_{1-\alpha/2,n-1}s\sqrt{1+1/n}$ |

As $n$ increases, uncertainty about the mean decreases, while the variability of a new observation remains.

## The Delta Method

For i.i.d. observations with $0\lt \sigma^2\lt \infty$, if $g$ is continuously differentiable near $\mu$ and $g'(\mu)\ne0$:

$$
\sqrt n\bigl(g(\bar X)-g(\mu)\bigr)
\xrightarrow{d}N\bigl(0,\sigma^2[g'(\mu)]^2\bigr).
$$

An asymptotic interval for $g(\mu)$ is

$$
g(\bar X)\pm z_{1-\alpha/2}\lvert g'(\bar X)\rvert\frac{s}{\sqrt n}.
$$

When $g'(\mu)=0$, this formula does not yield the usual nondegenerate normal approximation.

## Difference of Means for Independent Samples

Suppose the samples $X_1,\dots,X_n$ and $Y_1,\dots,Y_m$ are independent of each other, with i.i.d. observations within each group. Write $\widehat\Delta=\bar X-\bar Y$, $\Delta=\mu_X-\mu_Y$. Then

$$
\operatorname{Var}(\widehat\Delta)=\frac{\sigma_X^2}{n}+\frac{\sigma_Y^2}{m}.
$$

| Assumptions | Interval for $\Delta$ | Status |
| :--- | :--- | :--- |
| Normal samples, known variances | $\widehat\Delta\pm z_{1-\alpha/2}\sqrt{\sigma_X^2/n+\sigma_Y^2/m}$ | Exact |
| Normal samples, unknown equal variances | $\widehat\Delta\pm t_{1-\alpha/2,n+m-2}s_p\sqrt{1/n+1/m}$ | Exact |
| Normal samples, unknown variances without an equality assumption | $\widehat\Delta\pm t_{1-\alpha/2,\nu}\sqrt{s_X^2/n+s_Y^2/m}$ | Welch's approximation |
| Finite positive variances, $n,m\to\infty$ | $\widehat\Delta\pm z_{1-\alpha/2}\sqrt{s_X^2/n+s_Y^2/m}$ | Asymptotic |

When variances are estimated, $n,m\ge2$ are required. The pooled variance and Welch–Satterthwaite degrees of freedom are

$$
s_p^2=\frac{(n-1)s_X^2+(m-1)s_Y^2}{n+m-2},
$$

$$
\nu=\frac{(s_X^2/n+s_Y^2/m)^2}
{(s_X^2/n)^2/(n-1)+(s_Y^2/m)^2/(m-1)}.
$$

Welch's approximation allows noninteger $\nu$. With unknown unequal variances, the exact distribution of its statistic generally depends on the unknown variance ratio: this is the Behrens–Fisher problem. Normality of the data alone does not make Welch's approximation exact.

## Paired Samples

For dependent measurements within a pair, use $D_i=X_i-Y_i$. Pairs must be independent and identically distributed across subjects:

$$
\bar D=\bar X-\bar Y,\qquad
\operatorname{Var}(D_i)=\sigma_X^2+\sigma_Y^2-2\operatorname{Cov}(X_i,Y_i).
$$

For normally distributed differences with positive variance, the exact interval for $\Delta=E[D_i]$ is

$$
\bar D\pm t_{1-\alpha/2,n-1}\frac{s_D}{\sqrt n},\qquad
s_D^2=\frac1{n-1}\sum_i(D_i-\bar D)^2.
$$

For nonnormal differences with finite positive variance, the corresponding interval with a $z$ quantile is asymptotic. Positive covariance reduces the variance of the difference relative to independent measurements with the same marginal distributions. For the exact interval, normality is required of the differences themselves.

## Variance: Exact and Asymptotic Intervals

For an i.i.d. sample from $N(\mu,\sigma^2)$:

| Mean | Sum of squares $Q$ | Degrees of freedom $\nu$ |
| :--- | :--- | :--- |
| Known $\mu$ | $\sum_i(X_i-\mu)^2=n\hat\sigma_0^2$ | $n$ |
| Unknown $\mu$ | $\sum_i(X_i-\bar X)^2=(n-1)s^2$ | $n-1$ |

In both cases, $Q/\sigma^2\sim\chi^2_\nu$, giving the exact interval

$$
\left[\frac Q{\chi^2_{1-\alpha/2,\nu}},\quad
\frac Q{\chi^2_{\alpha/2,\nu}}\right].
$$

For the standard deviation, take the square roots of the endpoints. The $\chi^2$ distribution is asymmetric: the lower quantile cannot be obtained by changing the sign of the upper quantile. These intervals are sensitive to departures from normality.

For an arbitrary i.i.d. distribution with a finite fourth moment and $\mu_4-\sigma^4>0$:

$$
\sqrt n(s^2-\sigma^2)\xrightarrow{d}N(0,\mu_4-\sigma^4),\qquad
\mu_4=E[(X-\mu)^4].
$$

Using $\hat\mu_4=n^{-1}\sum_i(X_i-\bar X)^4$ and $\hat\sigma_n^2=n^{-1}\sum_i(X_i-\bar X)^2$ gives the asymptotic interval

$$
s^2\pm z_{1-\alpha/2}\sqrt{\frac{\hat\mu_4-(\hat\sigma_n^2)^2}{n}}.
$$

When $\mu_4=\sigma^4$, the limiting distribution under $\sqrt n$ scaling is degenerate, so this construction of a normal interval is not justified. In the normal model, $\mu_4=3\sigma^4$, giving an asymptotic variance of $2\sigma^4/n$ for $s^2$.

## Ratio of Variances

For two independent normal i.i.d. samples of sizes $n,m\ge2$ with positive variances:

$$
\frac{s_X^2/\sigma_X^2}{s_Y^2/\sigma_Y^2}\sim F_{n-1,m-1}.
$$

Writing $R=s_X^2/s_Y^2$, the exact interval for $\sigma_X^2/\sigma_Y^2$ is

$$
\left[\frac R{F_{1-\alpha/2,n-1,m-1}},\quad
\frac R{F_{\alpha/2,n-1,m-1}}\right].
$$

Taking reciprocals in a positive inequality reverses the order of the bounds. The quantiles satisfy

$$
F_{p,d_1,d_2}=\frac1{F_{1-p,d_2,d_1}}.
$$

If the interval contains one, the corresponding two-sided test does not reject equality of variances; this does not prove equality. Normality within each group does not replace independence between groups.

## The Uniform Distribution: An Interval Based on the Maximum

Let $X_i\overset{\mathrm{i.i.d.}}\sim\operatorname{Unif}(0,\theta)$, $\theta>0$, and $M=\max_iX_i$. For any i.i.d. sample, $P(M\le x)=F(x)^n$. In the uniform model,

$$
P_\theta(M/\theta\le u)=u^n,\qquad 0\le u\le1.
$$

The identity $P_\theta(\alpha^{1/n}\le M/\theta\le1)=1-\alpha$ yields the exact intervals

$$
\theta\in\left[M,\frac M{\alpha^{1/n}}\right],\qquad
\mu=\frac\theta2\in\left[\frac M2,\frac M{2\alpha^{1/n}}\right].
$$

For $n=10$, $M=8$, and $\alpha=0.05$, the interval for $\mu$ is approximately $[4;\ 5.40]$.

Choosing an upper bound of $1$ for $M/\theta$ minimizes the length of the parameter interval among procedures obtained by inverting fixed bounds $a\le M/\theta\le b$ with $b^n-a^n=1-\alpha$. This does not establish optimality among all possible procedures. The known lower endpoint $0$ is an essential model assumption.

## Conditions for Applicability

Exactness refers to coverage under the assumed model at finite $n$. Asymptotic results require the assumptions of the relevant limit theorem; there is no universal sample-size threshold. The absence of visible outliers does not guarantee finite moments or independence. For the mean, finite positive variance is sufficient; the asymptotic variance interval given above requires a finite fourth moment and a nondegenerate limiting distribution.

Failure to reject a normality test does not establish normality. The model should be justified using the nature of the data, the sampling design, and diagnostics; a preliminary normality test is not a mandatory condition for using a $t$ interval.
