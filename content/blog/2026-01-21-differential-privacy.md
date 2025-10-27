---
title: "Differential Privacy"
description: An exploration of some common differential privacy methods.
date: 2026-01-23
tags: [privacy, security, algorithms]
---

Differential privacy, in a broad sense, is the ability to provide guarantees
about the data in a dataset such that any given record in the dataset is
anonymous to within some bounded probability. Formally, we define differential
privacy as
$ P\left[\mathcal{M}(D)\in S\right]\leq e^\epsilon P\left[\mathcal{M}(D')\in S\right] $

Let's walk through that:

$P\left[\mathcal{M}(D)\in S\right]$ denotes the probability that the result of
some gaussian process $\mathcal{M}$ applied to the dataset $D$ results in some
value in $S$. The gaussian process we want to apply can be some sort of query,
like a selection query or finding the mean of some value in the dataset, the
result of which is captured in S.

On the right side of the equation, instead of using $D$, we use $D'$, where $D'$
is a neighbouring dataset. To be neighbouring, our datasets can only differ by
at most one (1) entry.

The term $e^\epsilon$ denotes our degree of acceptable difference between the
two probability distributions. We want the true result (the result of computing
the gaussian process on $D$) to be within some bounded difference of the value
of the result on the modified dataset.

To actually use this definition, we need to know some more information about our
queries to the dataset.

## The Sensitivity of a Query

In privacy, we define the sensitivity of any given query to a dataset by the
difference caused to the output of a query when we remove the most statistically
significant element of the dataset. Formally, we say
$ \Delta f = \text{max}_{D,\, D'}\left|\left| f(D) - f(D') \right|\right| $
where $\Delta f$ is the sensitivity.

This sensitivity measure for a given query provides useful information that we
can apply to our differential privacy equation to determine the 'correct' amount
of noise that we need to apply to our dataset in order to maintain the privacy
of the members.

## Applying noise to the Dataset

The easiest method to apply noise to the dataset is to use a Laplace
distribution with the parameters $\text{Lap}(0, \Delta f / \epsilon)$. That is
to say that we need a laplace distribution with a mean of zero and variance of
our sensitivity over the privacy parameter.

Adding the noise to this dataset or to the query will result in the analyst
obtaining data that obeys our definition of differential privacy above.

## Applying Differential Privacy to Machine Learning

In 2016, [this paper](https://arxiv.org/pdf/1607.00133 "Differential Privacy in Machine Learning")
provided a method to apply differential privacy to deep learning by, at every
epoch, sampling some of the data (the authors call this a lot, which is
distinct from a batch) on which to apply privacy correcting terms. With the lot
selected, the authors then compute the gradient of the lot, clip the gradient
within some bound (a hyperparameter), and then add noise to the computed
gradient with a gaussian noise function (you could use laplace as well).

Once the authors apply the privacy-preserving system to a single lot of the
data, they can then perform gradient descent and move to the next lot of
examples.

The authors suggest that the lot size may be much larger than the batch size. So
within their differentially-private SGD, when we compute the gradient we do so
in batches. Then we apply the noise to the entire computed gradient after the
fact, continuing until we are trained.

## Extensions

There are a number of extensions to differential privacy, such as ($\epsilon$-
$\delta$)-differential privacy and Renyi-Differential privacy, both of which
loosen the strictness of differential privacy to provide easier analysis.
However, you can consult
[this video](https://www.youtube.com/watch?v=oQzaA5KG3pM "Renyi Differential Privacy")
for some details from the author of the ($\epsilon$-$\delta$)-differential
privacy work on why some people use it wrong for analysis (it boils down to that
epsilon-delta does not actually both compose and allow for catestrophic failure
all at once, but Renyi-DP does).

## Why do we care?

There are myriad explanations online. Go look them up.

## Links

- [Video](https://www.youtube.com/watch?v=QJ3D4koSc6A "Differential Privacy")
  explaining the fundamental mathematics behind differential privacy.
- [Wikipedia](https://en.wikipedia.org/wiki/Differential_privacy "Wikipedia")
  article on differential privacy.
- [Video](https://www.youtube.com/watch?v=p6p9i1Hbcns "Differential Privacy in ML")
  on differential privacy in machine learning
- [Video](https://www.youtube.com/watch?v=mO0NmkVGapM "Adding noise") on Adding
  noise to your dataset to provide a differentially private query
