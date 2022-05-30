# How to compare two distributions

[TOC]

## Kullback-Leibler divergence

- is a measure of how much one **probability distribution** Q differs from a second one
- is NOT symmetric, i.e. $D_{KL}(P,Q)!=D_{KL}(Q,P)$

![image-20220415113756559](20220415_comparing_distributions.assets/image-20220415113756559.png)

## F-test

- is [any statistical test](https://en.wikipedia.org/wiki/F-test) where the test statistic has an F-distribution under the null hypothesis ?!

- however, it seems that the term often refers to the most widely known F-test, which tests whether the **means** of normal distributions are the same

  - plays an important role in the analysis of variance (ANOVA)

## Kolmogorov–Smirnov statistic

- is a nonparametric test of the equality of one-dimensional **probability distributions**
- uses the [empirical (cumulative )distribution function (ECDF)](https://en.wikipedia.org/wiki/Empirical_distribution_function)
  - ![image-20220415114531169](20220415_comparing_distributions.assets/image-20220415114531169.png)
- measures the maximum distance between two ECDFs
  - ![image-20220415114621718](20220415_comparing_distributions.assets/image-20220415114621718.png)
  - ![image-20220415114724236](20220415_comparing_distributions.assets/image-20220415114724236.png)
- It rejects the null hypothesis at level $\alpha$ if...
  - ![image-20220415114915459](20220415_comparing_distributions.assets/image-20220415114915459.png)
- is explained here
  - https://towardsdatascience.com/how-to-compare-two-distributions-in-practice-8c676904a285

## Standardized Wasserstein distance

- represents the average distance between samples in distribution A and samples in distribution B
  - ![img](20220415_comparing_distributions.assets/1K12JZ29uq4q-texa647DFw.png)
  - In this case the Wasserstein distance is $(2.8+1.6+4.0) / 3 = 2.8$
- If distributions have different number of samples, empirical cumulative distribution is compared
  - ![img](20220415_comparing_distributions.assets/1JG-CVjiRCoJ4-6Ripb7JMg.png)
- does not use p-values in contrast to the F-test and the Kolmogorov Smirnov distance ?!
- is [implemented in scipy](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.wasserstein_distance.html)
- is explained here
  - https://towardsdatascience.com/statistical-tests-wont-help-you-to-compare-distributions-d829eefe418
    - argues that the standardized wasserstein distance (SWD) is often a better metric than the f-test (f) and the Kolmogorov-smirnov test (ks). See plots below, where numbers represent the measure of difference with respect to the blue distribution.
    - with 20 datapoints
      - ![img](20220415_comparing_distributions.assets/1SSWEKHyHK7JFzAGMo-_nFg.png)
    - with 1000 data points
      - ![img](20220415_comparing_distributions.assets/1k_tT2upGmxQ2hRZYl157KQ.png)
  - The author proposes a _standardized_ Wasserstein distance in the following way:
    - `standardized_ws=wasserstein_distance(a,b) / np.std(np.concatenate([a,b]))`
    - the value may be greater than 1!
    - it may be interpreted as the answer to the following question: “By how many standard deviations do we need to move each point of one group to obtain the other group?”
  - The author also shows that the (standardized) Wasserstein distance of a feature correlates with the feature importance of a trained catboost classifier on many keras datasets.
