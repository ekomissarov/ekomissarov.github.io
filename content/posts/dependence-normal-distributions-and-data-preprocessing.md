+++
title = 'Dependence, Normal Distributions, and Data Preprocessing'
date = 2026-11-08
draft = false
description = 'Review notes on independence, correlation, normal distributions, missing values, transformations, and feature preprocessing.'
tags = ['Probability', 'Statistics', 'Review Notes']
ShowToc = true
+++

*Statistics Review Notes · Part 2*

A compact refresher on dependence, normal distributions, and data preprocessing for readers who have studied introductory probability and statistics. These notes continue [Distributions and Descriptive Statistics](/posts/distributions-and-descriptive-statistics/).

## Independence and Uncorrelatedness

**Independence of events:** $P(A\cap B)=P(A)P(B)$. When $P(B)>0$, this is equivalent to $P(A\mid B)=P(A)$.

**Independence of random variables:**

| Distribution description | Criterion |
| :--- | :--- |
| Joint CDF | $F_{X,Y}(x,y)=F_X(x)F_Y(y)$ for all $x,y$ |
| Discrete distribution | $P(X=x_i,Y=y_j)=P(X=x_i)P(Y=y_j)$ for all $i,j$ |
| Joint density | $f_{X,Y}(x,y)=f_X(x)f_Y(y)$ almost everywhere |

Joint CDF: $F_{X,Y}(x,y)=P(X\le x,Y\le y)$. The density criterion requires a joint density to exist; a mismatch at a single point does not establish dependence.

With finite second moments:

$$
X\perp Y\ \Longrightarrow\ \operatorname{Cov}(X,Y)=0,
\qquad\text{the converse is false in general.}
$$

**Counterexample:** $X\in\{-1,0,1\}$ with probability $1/3$ for each value, and $Z=|X|$. Then $E(X)=E(XZ)=0$ and $E(Z)=2/3$, so $\operatorname{Cov}(X,Z)=0$, but $P(X=0,Z=0)=1/3\ne1/9=P(X=0)P(Z=0)$.

If $X$ is symmetric about zero and $g$ is even, then $\operatorname{Cov}(X,g(X))=0$ as well, provided $E|X|$, $E|g(X)|$, and $E|Xg(X)|$ are finite.

## Covariance and Correlation

[Explore covariance as the average signed area](/visualizations/covariance/). Drag the points to see how their positions relative to the sample mean affect covariance.

### Population Quantities

Finite second moments are assumed below; correlation additionally requires positive variances.

$$
\operatorname{Cov}(X,Y)=E[(X-E(X))(Y-E(Y))]=E(XY)-E(X)E(Y).
$$

$$
\rho_{XY}=\frac{\operatorname{Cov}(X,Y)}{\sigma_X\sigma_Y},
\qquad -1\le\rho_{XY}\le1.
$$

| Property | Formula |
| :--- | :--- |
| Symmetry | $\operatorname{Cov}(X,Y)=\operatorname{Cov}(Y,X)$ |
| Relationship to variance | $\operatorname{Cov}(X,X)=\operatorname{Var}(X)$ |
| Constant | $\operatorname{Cov}(X,c)=0$ |
| Shifting and scaling | $\operatorname{Cov}(aX+b,cY+d)=ac\operatorname{Cov}(X,Y)$ |
| Additivity | $\operatorname{Cov}(X+Z,Y)=\operatorname{Cov}(X,Y)+\operatorname{Cov}(Z,Y)$ |
| Variance of a sum or difference | $\operatorname{Var}(X\pm Y)=\operatorname{Var}(X)+\operatorname{Var}(Y)\pm2\operatorname{Cov}(X,Y)$ |
| Transformation of correlation | $\operatorname{Corr}(aX+b,cY+d)=\operatorname{sign}(ac)\rho_{XY}$, $a,c\ne0$ |

- Covariance depends on the units of measurement; correlation is dimensionless.
- $\rho=\pm1$ means that $Y=aX+b$ almost surely, with the sign of $a$ matching that of $\rho$.
- $\rho=0$ does not rule out nonlinear dependence. Correlation with a constant is undefined.
- Correlation does not establish causation: an association may reflect a common factor, a time trend, or chance.

### Sample Counterparts and Spearman Correlation

For paired observations $(X_i,Y_i)$, let $A_{XY}=\sum_i(X_i-\bar X)(Y_i-\bar Y)$.

$$
S_{XY}=\frac{A_{XY}}n,\qquad
s_{XY}=\frac{A_{XY}}{n-1},\qquad
r_{XY}=\frac{A_{XY}}{\sqrt{\sum_i(X_i-\bar X)^2\sum_i(Y_i-\bar Y)^2}}.
$$

$s_{XY}$ is an unbiased estimator of covariance for an i.i.d. sample of pairs with finite second moments and $n\ge2$. For $r_{XY}$, both variables must have nonzero sample variation.

**Spearman correlation:** Pearson correlation between ranks. Tied values are assigned their average rank. With no ties:

$$
r_s=1-\frac{6\sum_i d_i^2}{n(n^2-1)},\qquad d_i=R(X_i)-R(Y_i).
$$

| | Pearson | Spearman |
| :--- | :--- | :--- |
| Association measured | Linear | Monotonic |
| Data used | Original values | Ranks |
| Outliers | Sensitive | The influence of extreme magnitudes is limited |

A strictly increasing functional relationship gives $r_s=1$, and a strictly decreasing one gives $-1$, provided the ranks have nonzero variation. With ties, use correlation between ranks rather than the simplified formula involving $d_i$.

### Outcome Counts: The Die-Rolling Result

Let $X_i$ count occurrences of outcome $i$ across $n\ge1$ independent trials with the same outcome probabilities $p_i$:

$$
\operatorname{Var}(X_i)=np_i(1-p_i),\qquad
\operatorname{Cov}(X_i,X_j)=-np_ip_j\quad(i\ne j).
$$

$$
\operatorname{Corr}(X_i,X_j)=-\sqrt{\frac{p_ip_j}{(1-p_i)(1-p_j)}},\qquad 0\lt p_i,p_j\lt 1.
$$

For $K\ge2$ equally likely outcomes, $\rho=-1/(K-1)$; for a fair six-sided die, it is $-1/5$. The correlation does not depend on $n$. The constraint $\sum_iX_i=n$ is affine; the centered counts are linearly dependent, and their covariance matrix is singular.

## Normal Distributions and Distribution Shape

For $X\sim N(\mu,\sigma^2)$ with $\sigma>0$:

$$
f_X(x)=\frac1{\sigma\sqrt{2\pi}}e^{-(x-\mu)^2/(2\sigma^2)},
\qquad Z=\frac{X-\mu}{\sigma}\sim N(0,1).
$$

$$
P(a\lt X\le b)=\Phi\!\left(\frac{b-\mu}{\sigma}\right)-\Phi\!\left(\frac{a-\mu}{\sigma}\right).
$$

$\Phi$ is the standard normal CDF. For $a\ne0$, $aX+b\sim N(a\mu+b,a^2\sigma^2)$. A sum of independent normal variables is normal, with means and variances adding.

| Interval | Probability under a normal distribution |
| :--- | :--- |
| $\mu\pm\sigma$ | $68.27\%$ |
| $\mu\pm2\sigma$ | $95.45\%$ |
| $\mu\pm3\sigma$ | $99.73\%$ |

For i.i.d. observations with finite positive variance, the CLT justifies a normal approximation for **centered and appropriately scaled sums and means**, not normality of the individual observations.

**Skewness and excess kurtosis:** for $U=(X-\mu)/\sigma$,

$$
\operatorname{Skew}(X)=E(U^3),\qquad
\operatorname{Excess}(X)=E(U^4)-3.
$$

These require $0\lt \sigma^2\lt \infty$ and a finite third absolute moment or fourth moment, respectively.

- Symmetry implies zero skewness; the converse is false. Positive skewness typically corresponds to a longer right tail, and negative skewness to a longer left tail.
- A normal distribution has excess kurtosis $0$; zero excess kurtosis does not establish normality.
- Positive excess kurtosis means a larger fourth standardized moment than that of a normal distribution; negative excess kurtosis means a smaller one. It does not uniquely determine peak height or tail shape.

## Multivariate Normal Vectors

$$
X\sim N_d(\mu,\Sigma),\qquad
\mu=E(X),\qquad
\Sigma=E[(X-\mu)(X-\mu)^T].
$$

- $\Sigma_{ii}=\operatorname{Var}(X_i)$; $\Sigma_{ij}=\operatorname{Cov}(X_i,X_j)$.
- $\Sigma=\Sigma^T$ and $v^T\Sigma v\ge0$ for all $v$.
- In the nondegenerate case, $\Sigma\succ0$: $v^T\Sigma v>0$ for $v\ne0$, $\det\Sigma>0$, and $\Sigma^{-1}$ exists.

Density of the nondegenerate distribution:

$$
f_X(x)=\frac{\exp[-\tfrac12(x-\mu)^T\Sigma^{-1}(x-\mu)]}{(2\pi)^{d/2}\sqrt{\det\Sigma}}.
$$

**Geometry:** contours of a bivariate density are ellipses; when $\Sigma=\sigma^2I$, they are circles. The quadratic form $(x-\mu)^T\Sigma^{-1}(x-\mu)$ is the squared Mahalanobis distance.

**Joint normality:** zero covariance between components is equivalent to their independence; a diagonal $\Sigma$ implies independence of all components. Normality of each marginal distribution alone is not sufficient.

**Linear combination:**

$$
Z=a^TX+b\sim N(a^T\mu+b,\ a^T\Sigma a).
$$

With zero variance, the result is a constant. For two components:

$$
\operatorname{Var}(aX+bY+c)=a^2\operatorname{Var}(X)+b^2\operatorname{Var}(Y)+2ab\operatorname{Cov}(X,Y).
$$

Example: with $\mu=(1,-4)^T$ and $\Sigma=\begin{pmatrix}3&-1\\-1&4\end{pmatrix}$, the variable $Z=X-3Y-6$ follows $N(7,45)$; $P(Z>5)\approx0.6172$.

## Kernel Density Estimation (KDE)

$$
\hat f_h(x)=\frac1{nh}\sum_{i=1}^nK\!\left(\frac{x-X_i}{h}\right),\qquad h>0.
$$

The kernel satisfies $K\ge0$ and $\int K(u)\,du=1$ and is usually symmetric. Examples: Gaussian, $K(u)=e^{-u^2/2}/\sqrt{2\pi}$; rectangular, $K(u)=\mathbb I(|u|\le1/2)$.

| Bandwidth relative to the scale of the data | Result |
| :--- | :--- |
| Large | Strong smoothing, loss of modes and detail, increased bias |
| Small and positive | Sensitivity to random fluctuations, increased variance |

KDE does not depend on fixed bin boundaries. Smoothness is inherited from the kernel: Gaussian KDE is infinitely differentiable. The true density need not be smooth. Methods for selecting $h$ include Silverman's rule of thumb.

## Missing Values and Outliers

### Missingness Mechanisms and Imputation

| Mechanism | Dependence of missingness |
| :--- | :--- |
| MCAR | Independent of the data values |
| MAR | Independent of unobserved values conditional on the observed data |
| MNAR | Depends on unobserved values even after conditioning on the observed data |

| Treatment | Main limitation |
| :--- | :--- |
| Deleting rows | Reduces sample size; may bias conclusions under MAR and MNAR |
| Dropping features | Discards potentially important information |
| Mean, median, or mode imputation | Alters the distribution and associations; does not account for imputation uncertainty |
| k-NN, regression, MICE | Results depend on the model and missingness assumptions |

Mean imputation preserves the mean of the **observed** values but reduces sample variance and does not guarantee an unbiased estimate of the population mean.

### Outlier Detection

| Method | Bounds | Qualification |
| :--- | :--- | :--- |
| Three-sigma rule | $\bar X\pm3s$ | The probability $99.73\%$ applies to a normal distribution with the true $\mu,\sigma$; sample bounds are sensitive to outliers |
| IQR | $[Q_1-kIQR,\ Q_3+kIQR]$ | $k=1.5$ is the usual threshold; $k=3$ is more conservative and flags fewer points |

An unusual value is not necessarily an error; automatic deletion can distort the distribution. Computing the mean and standard deviation does not require normality. The asymptotic breakdown point is $50\%$ for the median and approximately $25\%$ for quartiles and the IQR.

## Box–Cox Transformation

For $x>0$:

$$
T_\lambda(x)=\begin{cases}
(x^\lambda-1)/\lambda,&\lambda\ne0,\\
\ln x,&\lambda=0.
\end{cases}
$$

- $\lambda=1$ gives the shift $x-1$; $\lambda=0$ gives the logarithm; the transformation is continuous in the limit $\lambda\to0$.
- For nonpositive data, use $T_\lambda(x+\alpha)$, ensuring $x+\alpha>0$ for all values being processed.
- Choose $\lambda$ using likelihood or correlation with theoretical normal quantiles on a Q–Q plot.
- The transformation may reduce skewness and stabilize variance but does not guarantee normality. Lognormal $X$ yields normal $\ln X$; for an exponential variable, taking the logarithm can produce left skewness.
- The tradeoff is a less interpretable scale. Linear regression does not generally require normal features or a normal response; a normal error model concerns the conditional errors.

## Scaling and Categorical Features

### Feature Scaling

| Method | Transformation | Limitation |
| :--- | :--- | :--- |
| Standardization | $(x-\bar x)/s$ | $s>0$; sensitive to outliers; does not produce normality |
| Min-Max | $(x-x_{\min})/(x_{\max}-x_{\min})$ | $x_{\max}>x_{\min}$; new data may fall outside $[0,1]$ |
| Robust scaling | $(x-\operatorname{Med})/IQR$ | $IQR>0$; resistance to individual outliers is not absolute |

Estimate parameters on the training data and apply them to new data without refitting. Scaling changes Euclidean distances and nearest neighbors, making it important for distance-based algorithms. `StandardScaler` uses variance with divisor $n$ (`ddof=0`).

### Encoding Categorical Features

- **Nominal categories** have no order; **ordinal categories** have a natural order.
- **Ordinal encoding** is appropriate when the ordering is meaningful; for nominal categories, numerical codes can impose artificial distances.
- **One-hot encoding:** a separate indicator for each category. Missing values require an explicitly chosen encoding; the zero vector is not a universal convention.
- **Dummy variable trap:** with an intercept, the sum of all $K$ indicators duplicates the column of ones. A linear model typically retains $K-1$ indicators and a reference category.
- Without an intercept, the full one-hot matrix has rank $K$ if every category is represented. The covariance matrix of the centered indicators is singular because their sum is zero.
- After dropping the reference category, zeros identify that category; if missing values are also encoded as zeros, the two are indistinguishable without a separate indicator.
