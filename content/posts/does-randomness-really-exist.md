+++
title = 'Does Randomness Really Exist?'
date = 2026-10-30
draft = false
description = 'Determinism, uncertainty, and why statistical models do not require a fundamentally random world.'
tags = ['Probability', 'Statistics', 'Philosophy', 'Causal Inference']
+++

*Determinism, uncertainty, and the data-generating process.*

Statistical reasoning does not require the world to be fundamentally random. Much of the uncertainty we encounter arises from the gap between the complexity of reality and the limits of our knowledge.

## A Roll of the Dice

Imagine rolling a six-sided die.

Before it lands, we usually say that each face has a probability of $1/6$. We describe the outcome as random, rarely questioning what that actually means.

But is the outcome truly random?

The die follows the laws of physics. Its trajectory depends on its initial position, velocity, angular momentum, the properties of the surface, air resistance, and numerous other physical conditions.

In an idealized classical model, the outcome is determined by these initial conditions and the laws governing the die's motion. If we knew the relevant parameters with sufficient precision and could calculate the resulting trajectory, we could, in principle, predict which face would end up on top.

The result would follow from a particular initial state evolving according to deterministic laws.

So why do we assign probabilities to the possible outcomes?

The familiar value of $1/6$ is not a direct consequence of classical mechanics. It depends on assumptions about the die, the throwing procedure, and the distribution of initial conditions.

When we assign equal probabilities because we have no reason to prefer one outcome over another, we appeal to a version of the *principle of indifference*.

But this principle is not universally reliable. Bertrand's paradox illustrates the problem: different seemingly reasonable ways of choosing a random chord of a circle produce different probabilities for the same geometric event.

The physical mechanism matters.

The same question arises with another familiar example of randomness: tossing a coin.

In *Dynamical Bias in the Coin Toss*, Persi Diaconis, Susan Holmes, and Richard Montgomery investigated a mechanical model of coin tossing. Their analysis predicts a small bias toward the side initially facing upward, with a probability of approximately 51% under the model's assumptions.

Even familiar examples of supposedly fair random events can conceal systematic properties of their physical mechanisms.

This leads to a deeper question:

**If an outcome is determined by physical conditions, what exactly does its probability describe?**

## Laplace's Demon

In the early nineteenth century, the French mathematician Pierre-Simon Laplace formulated one of the most famous thought experiments in the history of probability.

Imagine an intelligence that knows the positions and motions of every particle in the universe, together with all the forces acting upon them.

If this intelligence also possesses unlimited analytical capabilities, it could calculate the future and reconstruct the past.

For such an intelligence, nothing would be uncertain.

This hypothetical observer later became known as *Laplace's Demon*.

The thought experiment represents an extreme form of classical determinism: the idea that the state of the world at one moment, together with the laws governing its evolution, uniquely determines its state at every other moment.

Under this view, uncertainty need not be a property of the physical world itself. It may arise from the limited information available to an observer.

Of course, Laplace's Demon is a philosophical idealization.

Even in classical physics, deterministic systems can exhibit chaotic behavior. Tiny differences in initial conditions may produce dramatically different outcomes over time.

Perfect prediction would therefore require not only enormous computational resources but also extraordinarily precise knowledge of the initial state.

**Determinism does not imply practical predictability.**

A system may evolve according to deterministic laws while remaining effectively unpredictable to any realistic observer.

### The Gap Between Reality and Knowledge

In practice, we observe only fragments of reality. We simplify complex mechanisms, ignore variables that cannot be measured, and construct mathematical models that capture the aspects of a process relevant to our questions.

Consider the die again.

If we know little about how it is thrown, assigning equal probabilities to its six faces may be a useful approximation under suitable symmetry assumptions.

If we observe its initial position and velocity, we may be able to make a more informed prediction.

If we possess a detailed physical model and precise measurements, our uncertainty may decrease further.

Let $Y$ represent the outcome of the die roll, and let $X$ represent the information available to us.

Our uncertainty can be described by the conditional distribution:

$$
P(Y \mid X)
$$

As the information contained in $X$ changes, this distribution may change as well.

In a deterministic model, the outcome is fixed by the complete initial state. What changes as we acquire information is not the underlying mechanism, but our probability distribution over its possible outcomes.

If the available information were sufficient to determine the outcome exactly, the conditional distribution would collapse to a single point.

This suggests an important distinction:

**A probabilistic description of an event does not imply that the underlying physical process must be fundamentally random.**

The physicist and probability theorist E. T. Jaynes discussed a closely related idea through what he called the *mind projection fallacy*: confusing uncertainty in our descriptions of reality with uncertainty inherent in reality itself.

This perspective treats probability, in some contexts, as a tool for reasoning under incomplete information.

It is an epistemic interpretation of uncertainty, although it is not the only possible interpretation of probability.

### A Note on Quantum Mechanics

Our discussion so far assumes classical physics.

Quantum mechanics complicates the picture. Standard quantum theory assigns probabilities to measurement outcomes, while Bell's theorem places strong constraints on local hidden-variable explanations.

Some interpretations, including Bohmian mechanics and many-worlds, retain deterministic dynamics.

Fortunately, our argument does not depend on resolving this debate. Statistical models can be useful regardless of whether the underlying world is fundamentally deterministic.

## From Reality to Data-Generating Processes

The same reasoning applies when we move from physical examples to data analysis.

Consider a user visiting an online store.

Will they make a purchase?

Their decision may depend on their needs, available money, previous experience, the price, the interface, the time of day, competing alternatives, and many other factors.

Some of these factors are observable, while others remain unknown to the analyst.

The mechanism producing the observations is what we attempt to represent through a **data-generating process (DGP)**.

A DGP is the mechanism, real or hypothesized, that produces the data we analyze.

Statistical models describe selected aspects of this mechanism, but the model is not the process itself.

For example, in product analytics, we often represent a purchase decision as a Bernoulli random variable:

$$
Y \sim \operatorname{Bernoulli}(\theta)
$$

where $Y=1$ represents a purchase, $Y=0$ represents no purchase, and $\theta$ is the conversion probability under the model.

But this statistical representation does not explain why the user makes a purchase.

To describe the mechanism more explicitly, we can use a structural equation of the kind found in **structural causal models (SCMs)**:

$$
Y=f(X,U)
$$

where:

- $Y$ is the observed outcome;
- $X$ represents observed factors;
- $U$ represents unobserved factors;
- $f$ describes the mechanism producing the outcome.

The function $f$ can be entirely deterministic.

Yet if we do not know the exact values of $U$, we may represent our uncertainty about them using a probability distribution.

For a binary outcome, suppose $X=x$ is fixed by an external intervention and the distribution of $U$ remains unchanged. The resulting conversion probability is:

$$
\theta(x)=P_U(f(x,U)=1)
$$

Here $\theta$ is the overall conversion probability, while $\theta(x)$ is the conversion probability when the observed factors are set to $x$.

This equation illustrates the central idea.

**A deterministic mechanism can produce a probabilistic model when some of its inputs are unknown.**

The probability represents the set of hidden states leading to a purchase, weighted by their assigned probabilities.

There is, however, an important distinction between setting $X=x$ and merely observing $X=x$.

Under an intervention, we evaluate the mechanism using the original distribution of the unobserved factors:

$$
P(Y=1\mid\operatorname{do}(X=x)) =
P_U(f(x,U)=1)
$$

When we condition on observing $X=x$, the distribution of hidden factors may be different:

$$
P(Y=1\mid X=x) =
P_{U\mid X=x}(f(x,U)=1)
$$

The difference is fundamental: **an intervention changes the value of a variable, while observation selects cases in which that variable already has a particular value.**

This distinction takes us directly from probabilistic modeling to causal inference.

## Observation Is Not Intervention

Suppose a company sends promotional offers to selected users. Here the factor of interest is a treatment, which we denote by $T$:

- $T=1$ means that a user receives an offer;
- $T=0$ means that they do not;
- $Y$ represents whether the user makes a purchase.

We might observe that users who receive offers have higher conversion rates.

But does this mean that sending an offer increases conversion?

Perhaps offers are preferentially sent to users who are already more likely to purchase.

The observed relationship may therefore reflect the assignment mechanism rather than the causal effect of the offer.

Using the intervention notation introduced by Judea Pearl, we distinguish:

$$
P(Y\mid T=t)
$$

from:

$$
P(Y\mid\operatorname{do}(T=t))
$$

The first describes the distribution of outcomes conditional on observing treatment status $T=t$.

The second describes the distribution of outcomes under an intervention that sets treatment to $t$.

These distributions are not generally equal. Their relationship depends on the underlying causal structure.

A statistical model may accurately predict which users are likely to purchase without identifying whether a promotional offer causes them to purchase.

Prediction and causal explanation are different objectives.

Understanding the data-generating process is essential when moving from one to the other.

## Randomization and Causal Inference

This is where randomized experiments become particularly valuable.

In an A/B test, we deliberately introduce randomness through treatment assignment.

This does not make users' behavior fundamentally random. Instead, randomization creates a known assignment mechanism that helps separate treatment effects from other factors influencing outcomes.

### Fixed Outcomes, Random Assignment

Consider a finite population of users participating in an experiment.

For each user $i$, define two potential outcomes:

$$
Y_i(0),\qquad Y_i(1)
$$

The first is the outcome that would occur under control, and the second is the outcome that would occur under treatment.

In the classical finite-population framework of randomization inference, we treat these potential outcomes as fixed.

Each user has two fixed potential outcomes, although we can observe only one of them.

Let $T_i\in\{0,1\}$ represent treatment assignment.

The observed outcome is:

$$
Y_i^{\mathrm{obs}} =
T_iY_i(1)+(1-T_i)Y_i(0)
$$

Here is the crucial point:

**The potential outcomes are fixed. The randomness comes from which treatment each user receives.**

We do not need to assume that users' responses are generated by some fundamentally random process.

Instead, the experiment creates a probability distribution over possible assignments.

That distribution allows us to reason statistically about treatment effects.

Because assignment is random, the probability that a user receives treatment does not depend on that user's potential outcomes or characteristics:

$$
P(T_i=1\mid Y_i(0),Y_i(1),X_i)=P(T_i=1)
$$

This is what allows us to estimate causal effects without observing every factor that influences user behavior.

We do not need to understand every reason why a particular user converts. We only need an assignment mechanism that we control and understand.

Randomization does not solve every problem, but it gives us a known assignment mechanism on which statistical inference can rest.

### Even the Randomization Can Be Deterministic

There is another interesting detail.

In digital products, treatment assignment is often implemented through a deterministic pseudorandom mechanism.

For example, an experimentation platform may combine a user identifier with an experiment-specific salt, calculate a stable hash, and map the result to an experimental group.

Given the same identifier and salt, the assignment is reproducible.

So where is the randomness? Not in any single assignment, which is fixed once the salt is chosen. It lies in the choice of salt: with a different salt, the same users would land in different groups. The distribution over such hypothetical re-randomizations is what our inference relies on.

A deterministic hash alone does not guarantee a valid experiment, so the quality of the hashing and the experimental design still matter.

Nevertheless, the example is revealing.

**Even the mechanism we use to introduce experimental randomness can be implemented deterministically.**

What matters for statistical inference is not whether a computer generates metaphysically random bits, but whether the assignment mechanism supports the probabilistic guarantees on which our conclusions depend.

## Conclusion: Is the World Random?

We began with a simple question.

When we roll a die, is the outcome truly random?

Within an idealized deterministic model of classical mechanics, the result follows from the initial conditions and physical laws.

Our inability to predict it does not establish that the physical process is fundamentally random.

But this does not mean that probability is merely an illusion.

Deterministic systems can be unpredictable. Our measurements are limited, and our models are incomplete.

Probability provides a mathematical language for describing uncertainty, whether that uncertainty arises from incomplete information, a deliberately randomized procedure, or other features of a statistical model.

The example of randomized experiments makes this particularly clear.

We can treat every user's potential outcomes as fixed and still construct a valid statistical inference procedure based on randomized assignment.

The randomness required for that inference comes from the experimental design, not from an assumption that individual behavior is fundamentally indeterminate.

This is a useful reminder that statistical randomness and physical randomness are not the same question.

**Statistics does not require the world to be fundamentally random. It requires us to be clear about where our probabilities come from: our ignorance, or our design.**

## What's Next?

If probability can describe uncertainty rather than fundamental randomness, what exactly does it mean to assign a probability to an event?

In the next article, *What Does Probability Mean?*, we'll explore two influential perspectives on this question: frequentist and Bayesian interpretations of probability.

