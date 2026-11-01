+++
title = 'What Does Probability Mean?'
date = 2026-10-31
draft = false
description = 'Frequentist and Bayesian perspectives on probability, uncertainty, and statistical inference.'
tags = ['Probability', 'Statistics', 'Frequentist Statistics', 'Bayesian Statistics']
+++

*Frequentist and Bayesian perspectives on uncertainty.*

In the [previous article](/posts/does-randomness-really-exist/), we explored why probabilistic models can be useful even when the underlying processes are deterministic.

This raises another question: if probability does not necessarily describe fundamental randomness, what exactly does it mean?

Suppose we say that a user has a 30% probability of making a purchase.

Does this describe a property of a repeatable process? A degree of uncertainty about an individual outcome? Or something else?

Probability theory provides a common mathematical framework for reasoning about uncertain events, but the interpretation of probability is not unique.

Two particularly influential approaches are frequentist and Bayesian probability.

Understanding their differences helps explain why statistical methods that use the same mathematical foundations can lead to different forms of inference.

## Probability and Its Interpretation

Consider a simple experiment: rolling a six-sided die.

We assign a probability of $1/6$ to each face, assuming the die and throwing procedure are sufficiently symmetric.

But what does this probability represent?

One possible answer is that if we repeat the experiment many times under comparable conditions, approximately one-sixth of the outcomes will be sixes.

Another answer is that $1/6$ represents our uncertainty about the outcome before observing it.

These interpretations are related, but they are not identical.

The first emphasizes patterns across repeated experiments.

The second emphasizes uncertainty given available information.

Both can be expressed using the same mathematical probability theory. The distinction lies in what the probabilities are understood to mean.

## The Frequentist Perspective

The frequentist interpretation connects probability to the long-run behavior of repeatable experiments.

Suppose we roll a die repeatedly under comparable conditions.

Let $N$ be the number of rolls and $N_6$ the number of times we observe a six.

The relative frequency is:

$$
\hat{\theta}_N=\frac{N_6}{N}
$$

Under suitable assumptions, the Law of Large Numbers tells us that this relative frequency converges toward the underlying probability as the number of trials increases.

For a fair die, that probability is $1/6$.

Importantly, the Law of Large Numbers is a theorem within probability theory, not a definition of probability.

Defining probability purely through limiting frequencies introduces mathematical and philosophical difficulties, including questions about how those limits are established and which sequences of trials qualify.

### Historical Background

Frequency-based interpretations of probability predate modern statistical inference. In the nineteenth century, John Venn argued for understanding probability through frequencies in classes of events. In the twentieth, Richard von Mises tried to formalize this idea with the concept of *collectives*.

Frequentist statistical methodology is a related but separate story. Ronald Fisher developed maximum likelihood, significance testing, and experimental design. Jerzy Neyman and Egon Pearson framed hypothesis testing as a decision procedure with controlled long-run error rates.

The two traditions differ in spirit: for Fisher, a p-value was a measure of evidence against a null hypothesis, while for Neyman and Pearson a test was a rule with specified error properties. Modern frequentist statistics draws on both, and is better seen as a family of methods than as a single doctrine.

## Fixed Parameters and Random Data

One defining feature of frequentist inference is the distinction between parameters and observations.

Suppose we want to estimate the conversion probability of a website.

Let $\theta$ denote the unknown conversion probability under a specified statistical model.

In the frequentist framework, $\theta$ is treated as fixed but unknown.

The observed data are modeled as random.

For example, if we observe $n$ independent users, each with conversion probability $\theta$, the number of conversions can be modeled as:

$$
K\sim\operatorname{Binomial}(n,\theta)
$$

This model does not claim that each user flips a coin when deciding to buy. It says that, given what we know, we cannot tell the users apart, so we treat them identically and independently. In the language of the previous article, the randomness here belongs to our description of the data, not necessarily to the users themselves.

An estimator of the conversion probability is:

$$
\hat{\theta}=\frac{K}{n}
$$

The estimator varies across hypothetical repeated samples. The parameter does not.

This distinction determines how frequentist uncertainty is evaluated.

Rather than assigning a probability distribution to the unknown parameter, frequentist methods study the sampling distribution of the estimator.

### Confidence Intervals

Suppose we construct a 95% confidence interval for the conversion probability.

The frequentist interpretation is that the interval construction procedure has 95% coverage under the assumed model.

If we repeatedly collected samples and calculated an interval using the same procedure, approximately 95% of those intervals would contain the true parameter.

Once a particular interval has been calculated, the parameter is still treated as fixed.

The frequentist framework does not assign a 95% probability to the parameter lying within that realized interval.

This is a subtle but important distinction.

The probability statement concerns the procedure across repeated samples, not the unknown parameter after observing a particular dataset.

### Hypothesis Testing

Suppose we want to test whether a new product feature changes conversion.

We formulate a null hypothesis:

$$
H_0:\theta_A=\theta_B
$$

and an alternative hypothesis:

$$
H_1:\theta_A\neq\theta_B
$$

A **p-value** is the probability, assuming the null hypothesis and statistical model are correct, of obtaining a test statistic at least as extreme as the one observed.

For a right-tailed test, this can be written as:

$$
p_{\mathrm{value}} =
P(T_{\mathrm{stat}}\geq t_{\mathrm{obs}}\mid H_0)
$$

For two-sided tests, the definition of extremeness depends on the chosen statistic and testing procedure.

A p-value is not the probability that the null hypothesis is true.

In a Neyman–Pearson testing framework, we can construct a decision procedure that controls the probability of a Type I error at a predefined significance level.

For example, choosing $\alpha=0.05$ means that, under the null hypothesis and the relevant assumptions, the procedure rejects the null in no more than 5% of repeated applications.

These guarantees are valuable in experimental design because they allow us to evaluate statistical procedures before observing the results.

## The Bayesian Perspective

The Bayesian approach begins with a different question.

Suppose we want to estimate an unknown conversion probability.

Before collecting data, we may already have some information about plausible values.

Perhaps similar products usually have conversion rates between 5% and 15%.

Then we observe new data.

How should our uncertainty change?

Bayesian inference provides a formal framework for updating probability distributions as evidence becomes available.

### Bayes and Laplace

The historical roots of Bayesian inference include the work of Thomas Bayes and Pierre-Simon Laplace.

Bayes developed an early result concerning inference about an unknown probability from observed outcomes.

Laplace subsequently made major contributions to inverse probability and probabilistic reasoning.

Interestingly, this is the same Laplace associated with the famous thought experiment of a perfectly informed intelligence capable of predicting the evolution of a deterministic universe.

In his *Essai philosophique sur les probabilités*, Laplace connected probability with the limitations of human knowledge.

For him, a deterministic conception of the physical world was compatible with probabilistic reasoning about events whose causes were not fully known.

This connection illustrates why probability need not be interpreted exclusively as a property of fundamental physical randomness.

### Updating Uncertainty

Bayesian inference represents uncertainty about unknown parameters using probability distributions.

Bayes' theorem provides the relationship:

$$
p(\theta\mid D) =
\frac{p(D\mid\theta)p(\theta)}{p(D)}
$$

where:

- $\theta$ is an unknown parameter;
- $D$ represents observed data;
- $p(\theta)$ is the prior distribution;
- $p(D\mid\theta)$ is the likelihood;
- $p(\theta\mid D)$ is the posterior distribution.

The prior represents uncertainty before observing the current data.

The likelihood describes how compatible different parameter values are with the observations.

The posterior represents updated uncertainty after incorporating the evidence.

Here, lowercase $p$ denotes a probability density or mass function, while uppercase $P$ denotes the probability of an event.

**New evidence changes the probability distribution representing our uncertainty about an unknown quantity.**

## A Bayesian Conversion Rate

Consider a website with an unknown conversion probability $\theta$.

We observe $K$ conversions among $n$ users, and denote these data by $D$.

Assume:

$$
K\mid\theta\sim\operatorname{Binomial}(n,\theta)
$$

A convenient prior distribution for $\theta$ is the Beta distribution:

$$
\theta\sim\operatorname{Beta}(a,b)
$$

After observing the data, the posterior distribution is:

$$
\theta\mid D
\sim
\operatorname{Beta}(a+K,b+n-K)
$$

This is an example of a conjugate Bayesian model.

Let's make the calculation concrete.

### A Reproducible Example

Suppose we choose the prior:

$$
\theta\sim\operatorname{Beta}(2,18)
$$

Its mean is:

$$
E[\theta]=\frac{2}{2+18}=0.10
$$

The prior therefore centers our uncertainty around a conversion probability of 10%.

Now suppose we observe 14 conversions among 100 users:

$$
K=14,\qquad n=100
$$

The observed conversion rate is:

$$
\hat{\theta}=\frac{14}{100}=0.14
$$

The posterior distribution becomes:

$$
\theta\mid D
\sim
\operatorname{Beta}(16,104)
$$

Its mean is:

$$
E[\theta\mid D] =
\frac{16}{120}
\approx 0.1333
$$

Our posterior mean conversion rate is approximately **13.33%**.

We can also calculate the probability that the true conversion rate exceeds 10%:

$$
P(\theta>0.10\mid D)
\approx 0.8632
$$

Under the specified model and prior, there is approximately an **86.32% posterior probability** that the conversion rate exceeds 10%.

The equal-tailed 95% credible interval is approximately:

$$
[0.0788,\ 0.1994]
$$

or **[7.88%, 19.94%]**.

These results can be reproduced using SciPy:

```python
from scipy.stats import beta

# Prior: Beta(2, 18)
a, b = 2, 18

# Observed data
n = 100
k = 14

# Posterior parameters
a_post = a + k
b_post = b + n - k

# Posterior mean
posterior_mean = a_post / (a_post + b_post)

# Probability that conversion exceeds 10%
prob_above_10 = beta.sf(0.10, a_post, b_post)

# Equal-tailed 95% credible interval
credible_interval = beta.ppf(
    [0.025, 0.975],
    a_post,
    b_post,
)

print(f"Posterior mean: {posterior_mean:.2%}")
print(f"P(theta > 10%): {prob_above_10:.2%}")
print(
    "95% credible interval: "
    f"[{credible_interval[0]:.2%}, "
    f"{credible_interval[1]:.2%}]"
)
```

The example illustrates how Bayesian inference combines prior information with observed data.

The posterior mean lies between the prior mean of 10% and the observed conversion rate of 14%.

The posterior distribution also allows us to make direct probability statements about the unknown parameter.

These statements remain conditional on the model and prior assumptions.

### Credible Intervals vs. Confidence Intervals

A Bayesian credible interval contains a specified amount of posterior probability.

For example, a 95% credible interval for $\theta$ contains 95% of its posterior probability under the model.

A frequentist confidence interval is characterized instead by the long-run coverage of its construction procedure.

Although the numerical intervals may sometimes be similar, their interpretations differ.

## Does Bayesian Mean Subjective?

Bayesian inference is often associated with subjective probability.

The connection is understandable: prior distributions can represent information or beliefs held before observing the current data.

But Bayesian statistics is not synonymous with unrestricted personal opinion, and the Bayesian tradition spans a range of views.

At one end, Bruno de Finetti and Leonard Savage developed the subjective view, in which probability expresses an individual's coherent degree of belief.

At the other, E. T. Jaynes, whom we encountered in the previous article, developed an approach to probability as an extension of logic under incomplete information. He explored principles such as maximum entropy for assigning probability distributions from limited information, and his work is associated with objective Bayesian methods, although the meaning and feasibility of objectivity remain debated.

In practice, priors are often informed by previous experiments, scientific knowledge, or structural constraints.

Nevertheless, prior assumptions matter, particularly when data are limited.

The likelihood also depends on assumptions about how the data were generated.

Bayesian inference provides a coherent framework for updating uncertainty, but it does not guarantee that the model accurately represents reality.

Frequentist methods face a related limitation: their guarantees also depend on the validity of statistical assumptions.

Neither framework eliminates the need to understand the data-generating process.

## Two Different Questions

Consider an unknown conversion probability.

A frequentist may ask:

*How would my estimation procedure behave if I repeatedly collected samples under the same conditions?*

A Bayesian may ask:

*Given the data I have observed and my prior assumptions, what is the probability distribution of the unknown conversion rate?*

These are different questions.

The frequentist perspective emphasizes the repeated-sampling properties of statistical procedures.

The Bayesian perspective emphasizes the updating of uncertainty about unknown quantities.

Both use probability theory, and both can produce useful estimates, intervals, and decisions.

Neither requires the conversion decision of an individual user to be fundamentally random.

## Randomization Inference: Where the Randomness Comes From

There is another important connection between probability and statistical inference.

In the previous article, we considered randomized experiments in which every participant has fixed potential outcomes:

$$
Y_i(0),\qquad Y_i(1)
$$

The uncertainty comes from the assignment of participants to treatment and control groups.

This idea provides the foundation for **Fisher's randomization test**.

Suppose we want to test the sharp null hypothesis that the treatment has no effect on any participant:

$$
H_0:Y_i(1)=Y_i(0)
\quad\text{for every }i
$$

Under this hypothesis, each participant's outcome would be the same regardless of treatment assignment.

We can therefore determine what the test statistic would have been under every assignment allowed by the experimental design.

For example, if the experiment randomly assigns a fixed number of users to treatment, we can consider all assignments with that same group size.

For each possible assignment, we calculate the test statistic.

Together, these values form a **randomization distribution**.

The observed statistic can then be compared with this distribution to calculate a p-value.

In practice, the number of possible assignments is usually enormous, so we sample random assignments instead of listing them all. This is the same idea as re-running the assignment with different salts, as in the previous article.

The important point is where the probability distribution comes from.

**It comes from the experimental assignment mechanism, not from a probability model for how individual outcomes are generated.**

The potential outcomes are treated as fixed.

The randomness comes entirely from the assignment procedure.

This is a particularly clear example of statistical inference that does not require us to assume fundamental randomness in individual behavior.

It also illustrates why understanding the experimental design matters as much as understanding the statistical test.

## What Does This Mean for A/B Testing?

Suppose we run an A/B test comparing two product variants.

We want to know whether the treatment improves conversion.

In a frequentist analysis, we might construct a confidence interval for the treatment effect and perform a hypothesis test with a predefined Type I error rate.

In a Bayesian analysis, we might calculate the posterior probability that the treatment effect is positive:

$$
P(\theta_B>\theta_A\mid D)
$$

We could also calculate the posterior probability that the effect exceeds a practically meaningful threshold.

These quantities answer different questions.

A frequentist p-value is not equivalent to a Bayesian posterior probability.

A 95% confidence interval is not interpreted in the same way as a 95% credible interval.

However, the choice between frequentist and Bayesian methods should not be reduced to a preference for one interpretation of probability.

Practical considerations also matter.

For example, an experiment may require strict control of false-positive decisions, while another decision problem may benefit from incorporating reliable prior information.

The appropriate method depends on the scientific question, the experimental design, the available information, and the consequences of different decisions.

One particularly important consideration is **when and how the experiment is evaluated**.

Repeatedly checking a conventional fixed-horizon p-value and stopping when it becomes significant can invalidate its nominal Type I error guarantee.

Sequential testing methods address this problem by explicitly accounting for repeated evaluation.

This is a separate topic, but it demonstrates an important principle: statistical guarantees depend not only on the formula we calculate, but also on the procedure through which data are collected and decisions are made.

## Conclusion

Probability provides a mathematical language for reasoning about uncertainty.

But the meaning we assign to probability influences how we construct statistical inference.

The frequentist approach evaluates procedures through their behavior under repeated sampling.

The Bayesian approach represents uncertainty about unknown quantities through probability distributions that are updated as evidence becomes available.

Randomization inference provides another useful perspective: a probability distribution can arise directly from the experimental assignment mechanism, even when all potential outcomes are treated as fixed.

These approaches are not simply competing answers to the same question.

They offer different ways to formulate problems, express uncertainty, and evaluate evidence.

Back to our 30% purchase probability. A frequentist reads it as the long-run frequency of purchases in a repeatable process. A Bayesian reads it as a degree of uncertainty given the available information and the model. In a randomized experiment, probabilities can also come from the assignment procedure itself. The same number can carry different meanings, and knowing which one is intended is part of reading any statistical result.

For an analyst, understanding these differences is more valuable than treating frequentist and Bayesian statistics as competing ideologies.

The objective is not to choose a statistical philosophy once and for all.

It is to understand what a method assumes, what its results mean, and whether those results answer the question we actually care about.

**Statistical methods are tools for reasoning under uncertainty. Their usefulness depends not only on the mathematics, but also on how carefully we connect that mathematics to the world.**

## What's Next?

A/B testing provides a natural setting for exploring how statistical guarantees depend on experimental procedures.

In a future article, we'll examine why repeatedly checking conventional p-values can lead to misleading conclusions, and how sequential testing methods allow experiments to be monitored without losing control of statistical error rates.
