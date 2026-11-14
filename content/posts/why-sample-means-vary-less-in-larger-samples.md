+++
title = 'Why Sample Means Vary Less in Larger Samples'
date = 2026-11-14
draft = false
description = 'An interactive experiment with simulated salaries and the sampling distribution of the mean.'
tags = ['Statistics', 'Sampling', 'Visualization']
+++

Draw a sample of 10 people and calculate their average salary. Draw another sample from the same population and the average will usually change. Repeat this 1,000 times and the resulting averages trace out a sampling distribution.

In the interactive plot, select **Mean**, leave **With replacement** checked, and run the simulation with a sample size of 10. Then try 100 with the same population. Compare **Std. dev. of statistic** across the two runs. [Open the salary sampling visualization](/visualizations/salary-sampling/).

For draws with replacement, the standard deviation of the sample mean is $\sigma/\sqrt{n}$, where $\sigma$ is the population standard deviation. Going from 10 to 100 observations should bring the spread of the means down to about one-third of what it was. [Sampling distributions and limit theorems in the review notes](/posts/sampling-distributions-limit-theorems-and-simulation/).
