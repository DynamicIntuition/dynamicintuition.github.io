---
linkTitle: Strategy
title: Software Strategy for 2025
weight: 1
---

In accordance to our research mission, projects are distributed with the objective of assessing the tractability of Praxis Learning. See our [About page](/about) for a fundamental perspective on our work.

## Summary

Motivated by the profound differences across strategic situations, we attempt to maximize the number of samples available for testing our methods during experimentation.

We specifically seek to have standardized access to many provably correct strategies. By focusing on the deterministic case of game strategy, we restrict potential shortcomings in performance to our methods, increasing the statistical efficiency of our sample strategies.

As such, our projects facilitate consistent production of datasets associated with provably correct strategies in this case of games. We incorporate design decisions that accelerate this process and take on projects that address this objective.

## Roadmap

{{% steps %}}

### Graphor

Graphor is our plugin-based platform for dataset generation from a primitive of implicit graphs, which the aforementioned games are a case of. It is designed for language interoperability and quick experimentation, with diminished emphasis on the size of graphs it can support.

### Nova

Nova is the post-Graphor future of [GamesmanNova](https://github.com/GamesCrafters/GamesmanNova), an all-in-one Rust system for search in sequential games. Since Graphor will be an integrated system, Nova will simply become a collection of Graphor plugins.

### Plugins

Once Graphor and Nova are compatible, contributions will be open for Nova. A series of game Graphor plugins will be added to the system together with feature definitions.

### Hermes

With plugins available for dataset generation, feasability studies will be conducted on the proposal of Hermes, an end-to-end system for generating textual descriptions of game-theoretic strategies.

{{% /steps %}}

## Timeline

* Graphor and Nova are set to be compatible by mid-May.

* Significant progress is expected on Graphor by late March.

* Preparations for accepting contributions to plugins will be done during the Summer.

* Plugin additions will be accepted during the Fall, concurrent to the development of learned strategy representations.

* A minimum viable product for Hermes is tentative for early 2026.
