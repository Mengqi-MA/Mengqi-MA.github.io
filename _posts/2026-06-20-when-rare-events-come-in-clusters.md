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

The total number of events is the sum of the cluster sizes. In compact mathematical notation, we can write

$$
W = X_1 + X_2 + \cdots + X_N,
$$

where $N$ is the number of clusters and $X_1, X_2, \ldots$ are their sizes. This distinction matters: two weeks might have the same total number of earthquakes, while one contains several unrelated events and the other is dominated by a single sequence of aftershocks.

Compound Poisson models are useful whenever the data look like rare events with local bursts. Earthquake sequences are one example; insurance claims, network failures, and some reliability problems can have a similar structure.

## A model is only useful if we know when to trust it

Choosing a plausible model is only half the job. The harder question is how close that model is to the process it represents. If the assumptions behind a compound Poisson model are not a reasonable description of the data, the resulting probabilities can be misleading.

This is where Stein's method enters the picture. Rather than asking only whether a compound Poisson distribution is convenient, Stein's method provides a way to bound the difference between the real distribution of interest and the approximation. In plain language, it turns "this model seems reasonable" into a question with a quantitative answer: how much error might the approximation introduce?

## A small case study in South-West Australia

In this project, I explored compound Poisson approximation using earthquake data from South-West Australia from 2014 to 2015. The goal was to approximate the distribution of the total number of earthquakes in a week.

The key modelling decision was to treat events that occurred nearby in time and space as potentially related. These local relationships formed the clusters. I then used two approaches to compound Poisson approximation and applied Stein's method to obtain error bounds for the resulting models.

This was a deliberately small case study, with limited historical data and simplifying assumptions. Its value was not a ready-made earthquake forecast. Instead, it illustrated a more general modelling lesson: a useful approximation should reflect the structure we see in the data, and it should come with an honest assessment of its uncertainty.

## What I took away

This project changed the way I think about probability models. A good model is not simply one that produces a number. It makes its assumptions visible, explains what features of the data it captures, and tells us when we should be cautious.

For rare events that come in clusters, compound Poisson approximation provides one such framework. It keeps the simplicity of Poisson modelling while making room for the fact that, in the real world, events often arrive together.
