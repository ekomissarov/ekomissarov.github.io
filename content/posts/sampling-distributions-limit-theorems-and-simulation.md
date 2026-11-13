+++
title = 'Sampling Distributions, Limit Theorems, and Simulation'
date = 2026-11-13
draft = false
description = 'Review notes on exact sampling distributions, modes of convergence, the law of large numbers, the central limit theorem, and inverse transform sampling.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 3*

These notes revisit sampling distributions, the law of large numbers, the central limit theorem, and simulation. They build on [Dependence, Normal Distributions, and Data Preprocessing](/posts/dependence-normal-distributions-and-data-preprocessing/).

## Distributions Related to the Normal Distribution

In the constructions below, $Z,Z_1,\ldots,Z_k\sim N(0,1)$, $U\sim\chi^2(k_1)$, and $V\sim\chi^2(k_2)$. The variables used in each construction are **independent**; the degrees of freedom are positive integers.

| Distribution | Construction | Main properties |
| :--- | :--- | :--- |
| Chi-square $\chi^2(k)$ | $\sum_{i=1}^k Z_i^2$ | Nonnegative; $E(X)=k$, $\operatorname{Var}(X)=2k$ |
| Student's $t(k)$ | $Z/\sqrt{W/k}$, where $W\sim\chi^2(k)$ | Symmetric about zero; heavier tails than the standard normal; converges in distribution to $N(0,1)$ as $k\to\infty$ |
| Fisher's $F(k_1,k_2)$ | $(U/k_1)/(V/k_2)$ | Positive; the degrees of freedom correspond to the numerator and denominator, in that order |

**Additivity:** the sum of independent $\chi^2(k_1)$ and $\chi^2(k_2)$ variables follows $\chi^2(k_1+k_2)$.

**Degrees of freedom:** the number of free coordinates after accounting for constraints. Deviations from the sample mean satisfy $\sum_i(X_i-\bar X)=0$, leaving $n-1$ degrees of freedom for estimating variance.

### Exact Sampling Distributions for a Normal Sample

Let $X_1,\ldots,X_n\overset{\mathrm{i.i.d.}}{\sim}N(\mu,\sigma^2)$, $n\ge2$, $\sigma^2>0$. Define the sample variance by

$$
s^2=\frac1{n-1}\sum_{i=1}^n(X_i-\bar X)^2.
$$

Then

$$
\frac{(n-1)s^2}{\sigma^2}\sim\chi^2(n-1),
\qquad
\frac{\sqrt n(\bar X-\mu)}s\sim t(n-1).
$$

The sample mean $\bar X$ and sample variance $s^2$ are independent. The first statistic supports inference about variance; the second supports inference about the mean when variance is unknown. Replacing $\sigma$ with $s$ in an arbitrary sample does not guarantee an exact Student's t distribution.

For two **independent i.i.d. normal samples**, $n_1,n_2\ge2$:

$$
\frac{s_1^2/\sigma_1^2}{s_2^2/\sigma_2^2}\sim F(n_1-1,n_2-1).
$$

The unscaled ratio $s_1^2/s_2^2$ follows this distribution when $\sigma_1^2=\sigma_2^2>0$. In ANOVA, the F-statistic is the ratio of between-group to within-group mean squares under the relevant assumptions and null hypothesis.

## Three Modes of Convergence

Convergence in probability and almost sure convergence require the variables to be defined on the same probability space; convergence in distribution does not.

| Mode | Definition | What it controls |
| :--- | :--- | :--- |
| In probability: $X_n\xrightarrow P X$ | $P(\lvert X_n-X\rvert\ge\varepsilon)\to0$ for every $\varepsilon>0$ | The probability of a deviation at step $n$ |
| Almost surely: $X_n\xrightarrow{a.s.}X$ | $P(\{\omega:X_n(\omega)\to X(\omega)\})=1$ | The limit along almost every sample path |
| In distribution: $X_n\xrightarrow d X$ | $F_{X_n}(x)\to F_X(x)$ at every continuity point of $F_X$ | Probability laws |

$$
X_n\xrightarrow{a.s.}X\ \Longrightarrow\ X_n\xrightarrow P X\ \Longrightarrow\ X_n\xrightarrow d X.
$$

- The reverse implications are false in general. Exception: $X_n\xrightarrow d c$ for a constant is equivalent to $X_n\xrightarrow P c$.
- Convergence in probability does not guarantee that a sample path eventually stays within an $\varepsilon$-band. The probability of a deviation need not decrease monotonically.
- Under almost sure convergence, for every $\varepsilon>0$, almost every sample path stays within the band from some index $N(\omega,\varepsilon)$ onward.
- Convergence in distribution does not imply closeness of realizations or convergence of densities. CDF values converge in the ordinary sense, not via `p-lim`.

### Operations on Limits in Probability

If $X_n\xrightarrow P X$ and $Y_n\xrightarrow P Y$, then

$$
X_n+Y_n\xrightarrow P X+Y,\qquad
X_nY_n\xrightarrow P XY,\qquad
cX_n\xrightarrow P cX.
$$

For a quotient, additionally require $P(Y=0)=0$ and that $X_n/Y_n$ be well-defined; for example, $P(Y_n=0)=0$ for every $n$ is sufficient.

**Continuous mapping:** $g(X_n)\xrightarrow P g(X)$ if $g$ is measurable and continuous at $X$ almost surely. In particular, $\bar X_n\xrightarrow P\mu$ implies $\bar X_n^2\xrightarrow P\mu^2$.

**Consistency:** an estimator $\hat\theta_n$ is consistent if $\hat\theta_n\xrightarrow P\theta$.

### Borel–Cantelli and a Counterexample

- If $\sum_nP(A_n)\lt \infty$, then, with probability $1$, only finitely many of the events $A_n$ occur. Independence is not required.
- If the events are independent and $\sum_nP(A_n)=\infty$, they occur infinitely often with probability $1$.
- Divergence of the series alone is insufficient: the intervals $A_n=(0,1/n)$ on $(0,1)$ have probabilities $1/n$, but each point belongs to only finitely many intervals.

**Convergence in probability without almost sure convergence:** for independent events with $P(A_n)=1/n$, set $X_n=n^2\mathbb I(A_n)$. For fixed $\varepsilon>0$ and sufficiently large $n$:

$$
P(|X_n|\ge\varepsilon)=1/n\to0.
$$

Yet almost every sample path contains infinitely many zeros and values of $n^2$, so it does not converge. The pattern of rare deviations across steps matters; marginal probabilities alone do not determine almost sure convergence.

## Law of Large Numbers

Let $\bar X_n=n^{-1}\sum_{i=1}^nX_i$.

| Version | Sufficient conditions | Conclusion |
| :--- | :--- | :--- |
| Chebyshev's weak LLN | Pairwise independence, identical distributions, $E(X_i)=\mu$, $\operatorname{Var}(X_i)=\sigma^2\lt \infty$ | $\bar X_n\xrightarrow P\mu$ |
| Kolmogorov's strong LLN | i.i.d., $E\lvert X_i\rvert\lt \infty$, $E(X_i)=\mu$ | $\bar X_n\xrightarrow{a.s.}\mu$ |

Pairwise independence is weaker than the mutual independence assumed by i.i.d. Finite variance is not required for the stated i.i.d. version of the strong LLN.

With common mean, equal finite variances, and pairwise independence:

$$
E(\bar X_n)=\mu,\qquad
\operatorname{Var}(\bar X_n)=\frac{\sigma^2}{n},\qquad
\operatorname{SD}(\bar X_n)=\frac{\sigma}{\sqrt n}.
$$

Chebyshev's inequality yields the weak LLN:

$$
P(|\bar X_n-\mu|\ge\varepsilon)\le\frac{\sigma^2}{n\varepsilon^2}\to0.
$$

- For every finite $n$, the sample mean remains random; the LLN does not guarantee an exact result.
- For a sample proportion, $\operatorname{Var}(\hat p)=p(1-p)/n$. Other things being equal, smaller groups yield more variable proportions and means.
- A large sample does not remedy dependence, selection bias, or a negative expected value: for example, an insurer that collects 100 rubles per policy and pays out 11,000 rubles with probability 0.01 has expected profit $E(X)=100-0.01\cdot11000=-10$ rubles per policy.

## Central Limit Theorem

**Lindeberg–Lévy:** for i.i.d. observations with $E(X_i)=\mu$ and $0\lt \operatorname{Var}(X_i)=\sigma^2\lt \infty$:

$$
Z_n=\frac{\sqrt n(\bar X_n-\mu)}\sigma
=\frac{\sum_{i=1}^nX_i-n\mu}{\sigma\sqrt n}
\xrightarrow d N(0,1).
$$

For large but finite $n$:

$$
\bar X_n\approx N(\mu,\sigma^2/n),\qquad
\sum_{i=1}^nX_i\approx N(n\mu,n\sigma^2).
$$

These are **approximations**, not limiting laws that depend on $n$. The sample mean $\bar X_n$ converges to the constant $\mu$; the standardized error $Z_n$ has a nondegenerate normal limit.

| | LLN | CLT |
| :--- | :--- | :--- |
| Question | What does the sample mean approach? | How is the normalized error distributed? |
| Limit | The constant $\mu$ | The law $N(0,1)$ for $Z_n$ |
| Convergence | In probability or almost surely | In distribution |

### Conditions and Limitations

- A single outlier does not invalidate the CLT. The conditions concern the distribution and dependence structure, not the appearance of a particular sample.
- The classical i.i.d. CLT does not apply when variance is infinite. A Pareto distribution has finite variance when its shape parameter satisfies $\alpha>2$; heavy tails alone do not rule out the CLT.
- The theorem does not guarantee an accurate normal approximation in the far tails for finite $n$.
- Histogram spikes and departures from normality do not establish manipulation: rounding, scale limits, and mixtures of groups are possible explanations.

For independent centered variables $Y_i$ with finite variances, let $s_n^2=\sum_{i=1}^nE(Y_i^2)>0$. **Lindeberg's condition** is

$$
\frac1{s_n^2}\sum_{i=1}^nE\left[Y_i^2\mathbb I(|Y_i|>\varepsilon s_n)\right]\to0
\quad\text{for every }\varepsilon>0.
$$

It is sufficient for $\sum_{i=1}^nY_i/s_n\xrightarrow dN(0,1)$. Feller's condition, $\max_{i\le n}\operatorname{Var}(Y_i)/s_n^2\to0$, states that individual variance contributions are small, but is not sufficient on its own.

## Random Variate Generation

- **TRNG:** a physical process supplies entropy.
- **PRNG:** a deterministic algorithm; an initial state, set by a seed, produces a reproducible sequence.
- **CSPRNG:** a generator for tasks requiring unpredictability and protection of secrets. The statistical properties of an ordinary PRNG do not guarantee cryptographic security.

Mersenne Twister (MT19937) and PCG64 are examples of PRNGs. The period of MT19937 is $2^{19937}-1$.

### Forward and Inverse Transforms

| Direction | Conditions | Result |
| :--- | :--- | :--- |
| Forward: $X\mapsto F(X)$ | $X$ has a continuous CDF $F$ | $F(X)\sim U(0,1)$ |
| Inverse: $U\mapsto Q(U)$ | Any CDF $F$, $U\sim U(0,1)$ | $Q(U)$ has CDF $F$ |

$$
Q(u)=\inf\{x\in\mathbb R:F(x)\ge u\},\qquad 0\lt u\lt 1.
$$

The inverse method does not require a density, continuity, or ordinary invertibility of the CDF. The key equivalence is

$$
Q(u)\le x\iff u\le F(x)
\quad\Longrightarrow\quad
P(Q(U)\le x)=F(x).
$$

For continuous $F$, the identity $F(Q(u))=u$ holds; it may fail at jumps. At the endpoints $u=0,1$, the quantile requires separate conventions and may be infinite.

**Exponential distribution**, rate $\lambda>0$:

$$
F(x)=1-e^{-\lambda x}\quad(x\ge0),\qquad
Q(u)=-\frac{\ln(1-u)}\lambda.
$$

In the ideal model, $-\ln U/\lambda$ has the same distribution. In computation, zero must be handled: for $u\in[0,1)$, `-log1p(-u) / rate` is safe, whereas `log(u)` is undefined at $u=0$. A finite-precision PRNG only approximates a continuous distribution.

**Monte Carlo:** numerical methods that use random simulation for integration, distribution estimation, and other tasks. Simulation helps check calculations and explore approximations, but does not prove a limit theorem and may miss rare events or conceal numerical errors.
