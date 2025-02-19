---
linkTitle: Graphor
title: "Dataset Generation Over Implicit Graphs"
weight: 10
math: true
sidebar:
  open: true
---

Graphor is a set of software containing interfaces for representing [implicit graphs](https://en.wikipedia.org/wiki/Implicit_graph) through specific functional forms and utilities for computing features associated with their vertices for its downstream analysis.

Concretely, `graphor` is a [Rust](https://www.rust-lang.org/) [library crate](https://doc.rust-lang.org/book/ch07-01-packages-and-crates.html) which implements a plugin system where actual implementations of implicit graphs can be dynamically linked. So, the Graphor _project_ also maintains packages with `graphor` _pre-baked_ into them that conveniently expose `graphor` in useful ways.

## Primitives

There are two objects in primary focus.

* **Implicit Graphs (IGs)**. Graph representations in terms of an initial vertex $v \in V$ and a _transition_ function $t : V \to \mathcal{P}(V)$.
* **Abstractions**: Adjacency-preserving [Morphisms](https://en.wikipedia.org/wiki/Morphism) (functions) of the form $a : V \to V'$, from vertices in a [lift](https://en.wikipedia.org/wiki/Covering_graph) $G = (V, E)$ to a possible correspondant $G' = (V', E')$.

{{< callout type="info" >}}
  This is a lot; see [this article](https://www.maxfierro.me/representation-concepts-in-game-theoretic-systems/) for a contextualized explanation.
{{< /callout >}}

Every other library item in `graphor` is dedicated to efficiently and exhaustively traversing IGs, producing artifacts as a function of (possibly abstracted) graph vertices, and storing them for later analysis.

As such, the library is split into 1) traits that represent IGs and abstractions and 2) utilities that consume these definitions in useful ways, supporting different algorithmic procedures. In accordance to the role of Graphor in PLRC's project strategy, these are mainly dynamic programming algorithms.

## Stories

The `graphor` library crate can be useful in many ways. We will explain this through three different user stories of a fictional Joe User.

{{< tabs items="Plugin Author,Library Author,Binary Author" >}}
  {{< tab >}}
  {{% steps %}}

  ### Situation

  Joe is a mathematician making a rewrite system to see how many theorems he can prove by brute-forcing formal rewrites.

  ### Problem

  Joe's project only involves one formal system with well-defined rewrite rules. He can write code, but does not have the technical know-how to manage the amount of permutations that his system will explore during exploration.

  ### Solution

  Joe finds the Graphor project, and notices that it already has an implementation of a shortest-paths algorithm. His solution is to write a `graphor` plugin representing his formal rewrite system.

  ### Implementation

  Joe writes Rust code implementing the right interfaces for his rewrite system. He then compiles his code as a dynamic library, allowing it to be used as a plugin.

  ### Usage

  Joe then uses the `graphor-cli` binary crate of the Graphor project to register his plugin and run the shortest-paths algorithm on a few different input theorems.

  Joe could have also used the `graphor` Python package to do the same through a Python script (and have programatic access to artifacts generated in the process).

  {{% /steps %}}
  {{< /tab >}}
  {{< tab >}}
  {{% steps %}}

  ### Situation

  Joe is a computer scientist implementing a toolchain for [formal grammar](https://en.wikipedia.org/wiki/Formal_grammar) checkers (programs that check whether or not a string belongs to a formal language) that work via [recursive descent](https://en.wikipedia.org/wiki/Recursive_descent_parser).

  ### Problem

  Recursive descent can be very expensive, and Joe does not want to spend too much time making optimizations.

  ### Solution

  Joe finds the Graphor project, and notices that it already has an implementation of a vertex-inclusion graph algorithm. His solution is to simply write a wrapper around the `graphor` library crate under the semantics of different types of formal grammars.

  ### Implementation

  Joe writes Rust wrapper traits (interfaces) that, once implemented, automatically give users' items access to `graphor` traits. He does this via [blanket implementations](https://doc.rust-lang.org/reference/glossary.html#blanket-implementation).

  ### Usage

  Joe then publishes his toolchain as a Rust library crate. This allows others to write Rust code that implements Joe's interface definitions, compile their code as a plugin, and use it through any `graphor` frontend (such as the `graphor-cli` binary crate).

  Joe could even write a wrapper (or extension) around the `graphor` CLI utility (which `graphor-cli` calls directly), and publish his own appropriately-themed binary with a dedicated UI. (Or write an entire user interface from scratch.)

  {{% /steps %}}
  {{< /tab >}}
  {{< tab >}}
  {{% steps %}}

  ### Situation

  Joe is a classical AI afficionado who likes to [solve](https://en.wikipedia.org/wiki/Solved_game) small games and publish his results.

  ### Problem

  Joe would like to spend his time working on implementing game rules, not on building infrastructure.

  ### Solution

  Through the Graphor project, Joe observes that he can represent many games he is interested in as IGs and apply well-known algorithms to solve them.

  ### Implementation

  Joe implements `graphor`'s interfaces for a lot of games, then self-registers them in a binary program (without compiling dynamic libraries). His program includes a user interface that allows him to interact with is implementations.

  ### Usage

  Joe publishes his binary as a standalone program which comes pre-loaded with a lot of games, and accepts user commands that allow it to generate solutions to the games he coded.

  When he has the time, Joe will make it so that, in addition to being able to solve all of the pre-loaded games in the binary, users will be able to write `graphor` plugins that his binary can register and also solve. This will be possible with minimal effort due to `graphor`'s architecture.

  {{% /steps %}}
  {{< /tab >}}
{{< /tabs >}}
