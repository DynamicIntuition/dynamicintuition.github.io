---
linkTitle: N-Gram Markovian Modeling of Optimal Policy Under Interpretable Abstractions
title: N-Gram Markovian Modeling of Optimal Policy Under Interpretable Abstractions
description: "Using classifier-style abstractions which map states to semantically interpretable labels, we show an approach to policy approximation through the construction of discrete Markov models of optimal policy dynamics within each class space."
math: true
---

## Metadata

```bibtex
@misc{fierro2025markov,
  author = {Fierro, Max},
  title = {N-Gram Markovian Modeling of Optimal Policy Under Interpretable Abstractions},
  institution = {EECS Department, University of California, Berkeley},
  howpublished = {Online},
  url = {http://plrcollective.github.io/reports/markov_models/},
  year = {2025},
  month = {February},
  day = {21},
  abstract = {...}
}
```

## Abstract

In praxis learning, choosing an interpretable functional form as a policy approximation model is paramount. Equally important is to ensure that it is well-defined defined over interpretable domains. Many such interpretable domains are the result of class-valued abstractions of the observable state space, as visual classification is a task that humans excel at. Motivated by this fact, we provide an interpretable functional form that is valid over multiclass spaces in the form of an $n$-gram Markov model approximation of dynamics under optimal policy.

## Background

$n$-gram models were developed as a rudimentary statistical model of language. By assuming an $n^{th}$-order [Markov property](https://en.wikipedia.org/wiki/Markov_property) on the probability of observing a word $w_{t + 1}$ at discrete time $t + 1$ given a history sequence $\langle w_i \rangle_{i \in [1, \\, t]}$,

\[
\begin{equation}
  P(w_1, \, \ldots, w_{t + 1}) =
  P(w_1, \dots, w_{t - n - 1}) \prod_{i = 0}^{n - 1} P(w_{t+1} \mid w_{t-n}, \dots, w_t),
\end{equation}
\]

straightforward [maximum likelihood estimation](https://en.wikipedia.org/wiki/Maximum_likelihood_estimation) shows that this probability is the proportion of times that the sequence $\langle w_{t-n}, \\, \ldots, w_t \rangle$ appears before $w_{t + 1}$ in observations. This is can be seen as frequentist inference, making the probability measure intuitive.

When applied to a set of symbols (words) $S$, such a model implies a Markov chain over the product $S^n = S \times \cdots \times S$. It follows that the chain's [stochastic matrix](https://en.wikipedia.org/wiki/Stochastic_matrix) $\Pi$ is an element of $\mathbb{R}^{k \times k^n}$ with $k = |S|$, meaning that the number of learnable parameters grows exponentially with the order of the model for a fixed $S$.

As a result of upholding the Markov property, $n$-gram models are stationary, meaning that the probabilities they assign to a sequence are invariant with respect to shifts in the time index. This flaw makes them incompatible with natural language to any useful extent.

### Rules of Thumb

Many heuristics taught in strategic decision-making can be described to be conditionals over the result of classification exercises. For example, there is a rule of thumb in Chess which calls for protecting one's own king if it is open.

When implementing this heuristic, a player necessarily performs a classification task via a mapping $\phi : S \to \\{\text{Yes}, \\, \text{No}\\}$ from the set of board states to an answer to the heuristic's condition, where experience insists that if a player's $\phi$ is sufficiently close to ground truth, they obtain a performance improvement.

Naturally, the complexity involved in evaluating a classification $\phi_h(s)$ for some state $s \in S$ should be minimal so that its heuristic $h$ can be implemented without computer assistance. In many cases, their simplicity to humans (i.e., how intuitive they are) directly translates to the simplicity of implementing them in other models of computation; it is generally easy to program such functions.

However, humans can obtain an _unexplainable_ intuitive understanding of a game. In such cases, the classification exercises they carry out for their expert heuristics are mappings onto a set of abstract characteristics (e.g., area 'crowdedness' in Chess). This can be seen loosely as a human version of [feature learning](https://en.wikipedia.org/wiki/Feature_learning).

But even in these cases, it is relatively simple to train a model which replicates a human's capacity to perform classification for expert heuristics via human-labeled datasets. Hence, one can generally assume access to efficient classifiers for human-interpretable features.

### Abstract Strategy

Given a morphism (an abstraction) $\alpha : S \to \tilde{S}$ over a state set $S$, the lack of an injectivity constraint could produce a situation where for an arbitrary policy $\pi : S \to S$ satisfying $\pi(s) = a$ and $\pi(s^\prime) = b$ with distinct $a, \\, b, \\, s, \\, s^\prime \in S$,

$$
\begin{equation}
  \alpha(s) = \alpha(s^\prime) \;\; \text{and} \;\; \alpha(a) \neq \alpha(b).
\end{equation}
$$

Therefore, attempting to obtain a counterpart $\tilde{\pi} : \tilde{S} \to \tilde{S}$ (an 'abstract strategy') which preserves the information in $\pi$ is oftentimes not possible, as $\tilde{\pi}(\alpha(s)) = \tilde{\pi}(\alpha(s^\prime))$ would have to 'remember' the distinct $\pi(s) = a$ and $\pi(s^\prime) = b$.

## Model

Let $\langle \phi^{(\alpha)} : S \to S^{(\alpha)} \rangle_{\alpha \in \Alpha}$ be a collection of abstractions enumerated in $\Alpha$, and $\pi : S \to S$ a policy over $S$. Observing $(2)$, we propose modeling class-conditional transition probability distributions,

$$
\begin{equation}
  P^{(\alpha)}_{t+1}(k) = P[\phi^{(\alpha)}(\pi^{t + 1}(s)) = k \; | \; \phi^{(\alpha)}(\pi^t(s)) = k_t, \, \ldots, \, \phi^{(\alpha)}(\pi^0(s)) = k_0],
\end{equation}
$$

of the elements $k_i \in S^{(\alpha)}$ via an $n$-gram model. This effectively establishes sequences in the set $\phi^{(\alpha)}(S)$ via repeated aplication of $\pi$ within $S$ (following the dynamics of $\pi$), so that in the above equation $\pi^t(s) = \pi_t(\pi_{t-1}(\ldots\pi_1(s)))$.
This yields a collection of stochastic matrices $\langle  \Pi^{(a)} \rangle_{\alpha \in \Alpha}$ with

$$
\Pi^{(\alpha)}_{i, j} = P[\, i \text{ is observed at time } t \; | \; j \text{ is observed immediately before}\,],
$$

where $i \in S^{(\alpha)}$ and $j \in (S^{(\alpha)})^n$. The amount of learnable parameters (i.e., the size) of such a model $M = \langle  \Pi^{(a)} \rangle_{\alpha \in \Alpha}$ is therefore

$$
\begin{equation}
  |M| = \sum_{\alpha \in \Alpha} |S^{(\alpha)}|^n \, (|S^{(\alpha)}| - 1).
\end{equation}
$$

## Training

The parameter space for a model $M$ of order $n$ is precisely

$$
\begin{equation}
  \Theta = \large{\times_{\alpha \in \Alpha}} \left[ \bold{S}^{|S^{(\alpha)}|^n} \right]^{|S^{(\alpha)}|},
\end{equation}
$$

(where $\bold{S}^k$ denotes the $k$-dimensional unit sphere). Finding an optimal $\theta^* \in \Theta$ follows the same procedure as any $n$-gram model. Hence, we simply provide the generic closed-form solution written in terms of the objects at hand,

$$
\begin{equation}
 \Pi^{(\alpha)}_{i, j} = \frac{1}{N}
 \sum_{s \in S} I^{(\alpha)}_{i,j}(\pi^n(s), \langle \pi^i(s) \rangle_{i \in [0, \, n)}),
\end{equation}
$$

where

$$
\begin{equation*}
 I^{(\alpha)}_{i,j}(a, \langle b_i \rangle_{i \in [0, \, n)}) =
 \begin{cases}
       1 & \text{if } \; \phi^{(\alpha)}(a) = i \; \text{ and } \; \phi^{(\alpha)}(b) = j, \\
       0 & \text{otherwise},
  \end{cases}
\end{equation*}
$$

and $N$ is the number of length-$(n + 1)$ contiguous subsequences in the dynamics of $\pi$, which can be easily sketched while computing the sum in $(5)$.

### Sources
The nature of the policy operator $\pi$ is such that there exists some $s \in S$ wihtout an $s^\prime$ such that $\pi(s^\prime) = s$. Here, $s$ is called a source within the dynamics of $\pi$. This constitutes a problem, as the start $s_0$ of the game for which $S$ is a state space is necessarily a source (which may not be unique); therefore, an attempt to find an $n$-length sequence of moves leading up to a state less than $n$ applications of $\pi$ away from a source in its dynamics may fail.

This is important because it is a step necessary to compute the $\Pi^{(\alpha)}_{i, j}$$^{\text{th}}$ parameter of the model, where $i$ is the parameter that is too close to a source to have a valid $n$-gram history. A solution which does not significantly alter transition distributions of $\Pi^{(\alpha)}$ is to sample missing elements of $n$-gram histories from a uniform distribution while computing its entries. If this measure is taken, $N$ can be set to $|S|$ in $(5)$, avoiding the need for sketching proportions.

### Sinks

In many traditional definitions of a policy $\pi$, there may exist elements $s^\prime_i$ of $S$ over which $\pi$ is not defined, as they are terminal in the game under representation. These are sinks in the dynamics of $\pi$, and should never be considered as part of a history while computing model parameters.

## Inference

When at a state $s \in S$, a human player can consider the set of next possible states $t(s)$ (where $t : S \to \mathcal{P}(S)$ is a transition function). Ideally, they would perform combinatorial optimization across all elements $s^\prime \in t(s)$ under the objective of maximizing the probability that their action is observed across all abstract state space transitions $S^{(\alpha)}$. Essentially, this is maximum likelihood estimation.

While this is possible to an extent due to the simplicity of the abstractions in consideration (which map onto small sets of classes, reducing maximization objectives during MLE), the true value of the model is in the subjective analysis of each $\Pi^{(\alpha)}$. Additionally, quantitative techniques (such as finding the static distribution and convergence rate of these matrices) may illustrate interpretable patterns in the dynamics of $\pi$, depending on $\langle \phi^{(\alpha)} \rangle$.

## Remarks

Establishing an approximation of optimal policy in the form of a Markov process provides an interpretable functional representation that is able to work with intuitive abstractions. Thus, it constitutes a valid representation of a praxis.

### Explorations

The following are left as potential avenues of analysis relating to this model family.

* Smoothing techniques, and an analysis of their benefit in the context of optimal policy.
* Non-interpretability of $n$-gram model successors; in particular transformer attention.
* Skip-gram models as an extension of this family.
