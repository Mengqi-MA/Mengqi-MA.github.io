---
title: "When Rare Events Come in Clusters"
date: 2026-06-20
permalink: /posts/2026/06/when-rare-events-come-in-clusters/
tags:
  - probability
  - stochastic modelling
  - earthquakes
  - Stein's method
---

Earthquakes are rare events, but they do not always arrive one at a time. The 1976 Tangshan earthquake sequence is a stark example. At 3:42 a.m. on 28 July, a magnitude 7.6 earthquake struck the Tangshan region. About three and a half hours later, a magnitude 6.2 aftershock followed; that evening, the region experienced a second magnitude 7.0 earthquake. A long aftershock sequence continued afterwards, including twelve earthquakes of magnitude 6 or greater.

This devastating sequence shows why a simple model that treats earthquakes as isolated events can miss something essential: rare events may come in clusters. [Wikipedia has further background on the 1976 Tangshan earthquake](https://en.wikipedia.org/wiki/1976_Tangshan_earthquake).

How can we describe this kind of burst mathematically? This post is about modelling clustered event counts, not predicting individual earthquakes.

## A natural starting point: the Poisson model

The Poisson distribution is a natural baseline for counting rare events over a fixed period of time. It has been used to model counts of events such as earthquakes, insurance claims, and equipment failures. In its simplest form, it assumes that events occur independently and that the average rate stays constant.

That can be a useful first approximation. Suppose we observe $n$ earthquakes across $m$ comparable time intervals, such as weeks. A natural estimate of the average number of earthquakes per interval is

$$
\widehat{\lambda} = \frac{n}{m}.
$$

If $N$ denotes the number of earthquakes in one such interval, the Poisson model says that the probability of observing exactly $k$ earthquakes is

$$
\Pr(N = k) = \frac{e^{-\lambda}\lambda^k}{k!}, \qquad k = 0, 1, 2, \ldots
$$

This gives a compact way to translate an average event rate into probabilities for different total counts.

But the Tangshan sequence points to its limitation. A major earthquake can be followed by aftershocks, so one event may make nearby events in time and space more likely. When events arrive in bursts, treating every earthquake as an isolated arrival leaves out part of the mechanism we want to describe.

## From individual events to clusters

A compound Poisson model extends the ordinary Poisson idea. Rather than modelling each event separately, it models two layers:

1. How many clusters occur in a given period?
2. How many events occur within each cluster?

Suppose we observe $c$ clusters across $m$ comparable time intervals. We can estimate the average number of clusters per interval by

$$
\widehat{\lambda} = \frac{c}{m}.
$$

We also need to estimate the cluster-size distribution. If $c_j$ of the observed clusters have size $j$, then a natural estimate of the probability that a cluster has size $j$ is

$$
\widehat{\mu}_j = \frac{c_j}{c}.
$$

The total number of events is the sum of the cluster sizes. In compact mathematical notation, we can write

$$
W = X_1 + X_2 + \cdots + X_N,
$$

where $N$ is the number of clusters and $X_1, X_2, \ldots$ are their sizes. Once $\lambda$ and the cluster-size distribution $\mu$ have been estimated, the probability of observing exactly $k$ events can be calculated as

$$
\Pr(W = k) = e^{-\lambda}\sum_{r=0}^{\infty} \frac{\lambda^r}{r!}\Pr(X_1 + \cdots + X_r = k).
$$

This formula averages over the possible number of clusters $r$ and the possible sizes of those clusters. For $r = 0$, the sum is zero, so this term contributes only when $k = 0$. The model is more flexible than the ordinary Poisson formula because it allows a single cluster to contribute more than one event. Two weeks might therefore have the same total number of earthquakes, while one contains several unrelated events and the other is dominated by a single sequence of aftershocks.

Compound Poisson models are useful whenever the data look like rare events with local bursts. Earthquake sequences are one example; insurance claims, network failures, and some reliability problems can have a similar structure.

## How close is the approximation?

Choosing a plausible model is only half the job. The harder question is how close that model is to the process it represents. If the assumptions behind a compound Poisson model are not a reasonable description of the data, the resulting probabilities can be misleading.

One way to make this question precise is the total variation distance. If $W$ is the true event count and $Z$ is the count predicted by the compound Poisson model, then

$$
d_{\mathrm{TV}}\bigl(\mathcal{L}(W), \mathcal{L}(Z)\bigr)
= \sup_{A \subseteq \mathbb{Z}_{\geq 0}}
\left|\Pr(W \in A) - \Pr(Z \in A)\right|.
$$

Here, $\mathcal{L}(W)$ means the probability distribution of $W$. The total variation distance considers every possible collection of event counts $A$ and records the largest difference between the two probabilities. A small value means that the model assigns similar probabilities to the true process across all events we might care about.

This is where Stein's method enters the picture. Rather than calculating the unknown distribution of $W$ directly, Stein's method uses a characterising equation for the target distribution $Z$. It rewrites the difference between the two distributions in a form that can be bounded using what we do know about event rates, cluster sizes, and local dependence.

In short, Stein's method turns "the compound Poisson model seems plausible" into an explicit upper bound on the total variation distance. It gives us a way to judge not only whether a model is convenient, but whether it is accurate enough for the question we want to answer.
