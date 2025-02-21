---
title: About
math: true
---

## Introduction

The Praxis Learning Research Collective is a group of people dedicated to exploring the proposal of **praxis learning (PL)**,

> Automated synthesis of artifacts that improve unassisted human performance from machine representations of perfect or near-perfect game-theoretic strategy.

We can take a look at this informally on a part-by-part basis.

* **Synthesis of artifacts.** Creating data of a special form.

* **Unassisted performance.** The ability to make good choices without a computer.

* **Machine representations.** Representations bound for computer processing.

* **Perfect strategy.** Specification of how to achieve the best outcome.

Hence, _praxis_ in _praxis learning_ refers to the What of learning, not the How; in PL, a praxis is learned automatically, but learning is not happening via some other meaning of 'praxis.' Likewise, _learning_ refers to the overall objective of coming to an effective praxis. This may seem obvious, but it is easy to think that _learning_ refers to some specific methodology.

### Dual Perspective

The process of creating things that help humans understand machine-bound strategy necessarily involves some form of **automated understanding**. Creating a process that carries out automated understanding is a worthy objective per se, which suggests an alternative proposal,

> Automated analysis of the intuition within perfect game-theoretic strategy.

This description does not involve the synthesis of artifacts which communicate this intuition to humans. As a result, the original formulation of the proposal is 'product oriented,' whereas this perspective attends to the core of why PL is valuable.

## Objective

In the landscape of objectives, PL can be characterized as an unusual form of machine learning (ML), where instead of only modeling functions as closely as possible, we add the upfront constraint that the model must be **inherently interpretable**.

### Area of Concern

PL also constrains the set of models under consideration to functions which approximate a game-theoretic strategy. The term **policy function**, which is adopted from reinfocement learning (RL), describes such functions.

Praxis learning is **not concerned with optimizing policy functions** under the objective of utility maximization. There are many incredibly successful fields of research dedicated to this objective under different constraints. Instead, the optimization objective of PL is to have a model closely approximate an existing policy function **with access to it**.

Therefore, while PL can be characterized as ML, its purpose is strictly translation across model families. With all this, PL could be characterized as transfer learning (TL) for game-theoretic AI interpretability. (Although this is deceiving, as the methods of TL differ.)

## Methods

Much like RL has a typical scenario framing, the appropriate environment for praxis learning involves the following elements being present.

* A **game** $G$, with a set of action histories $H$.
* A specified **state space** $S$, which is a set.
* An implicit **abstraction** $\alpha : H \to S$ connecting $H$ to $S$.
* An **optimal policy function** $\pi^* : S \to S$ (an oracle).

The ask is then to find the following.

* An alternative **state space** $S'$.
* An efficient **abstraction** $\phi : S \to S'$.
* A **model** $\pi' : S' \times \Theta \to S'$.
* Optimal parameters $\theta^* \in \Theta$.

> [!NOTE]
> Please search [this article](https://www.maxfierro.me/representation-concepts-in-game-theoretic-systems/) for contextualized definitions of some of these terms.

Here, the choice of $\pi'$ must make $\theta^\*$ interpretable, which can be aided by an appropriate choice of $\phi$. Importantly, no assumption is made about the functional form of $\pi^\*$.

### Model Selection

In general, there is no general method to PL, because most of it relies on model selection and design. This is because the main desideratum of PL is to have interpretability. The functional form of $\pi'$ and the abstract state space $S'$ are the only degrees of freedom available to achieve this objective.

### Optimization

Once selections are made, however, traditional ML techniques can be employed to find an optimal $\theta^* \in \Theta$ which makes $\pi'_{\theta^\*}$ most closely resemble $\pi^\*$. But in general, the availability of gradient methods is uncertain.

## Statements

### Mission

Given that the idea of praxis learning is unexplored, the utmost priority is to first motivate, assess, and communicate its tractability via comprehensive studies. Later, focus can shift to connecting a taxonomy of topics within PL to motivate additional research and future work. This is all under the objective of AI interpretability constrained to game-theoretic settings.

### Collaboration

In the spirit of a collective, we aspire for our work to be characterized by seamless reproducibility, proactive transparency, and clear exposure. This is in the interest of accelerating our mission and improving our intellectual community.
