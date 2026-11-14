+++
title = 'Why Covariance Changes When You Change Units'
date = 2026-11-14
draft = false
description = 'An interactive example of how converting meters to centimeters changes covariance while leaving correlation unchanged.'
tags = ['Statistics', 'Covariance', 'Visualization']
+++

Open the interactive plot and click **Units of X: meters** to switch to centimeters. The points stay in place, but the sample covariance is multiplied by 100. The sample correlation stays the same. [Open the covariance visualization](/visualizations/covariance/).

The plot represents each pair of observations as a signed rectangle drawn from the sample mean. Its signed area is the product of the two deviations from the mean. Expressing $X$ in centimeters multiplies every horizontal deviation by 100, so the average signed area is also multiplied by 100.

Correlation divides covariance by the product of the two standard deviations. The standard deviation of $X$ picks up the same factor of 100, which cancels in the ratio. [Covariance and Correlation in the review notes](/posts/dependence-normal-distributions-and-data-preprocessing/#covariance-and-correlation).
