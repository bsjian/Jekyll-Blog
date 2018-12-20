---
title: "P, NP and NP-Complete"
layout: post
date: 2018-9-25 23:18
image: /assets/images/markdown.jpg
headerImage: false
tag:
- NP-Complete
- proof
- algorithms
- problems
category: blog
author: boshengjian
description: My understanding on P, NP and NP-Complete
---

## Summary

Computers are designed to compute. And they are built to help people solve problems.

There are tons of problems in our natural world. Those problems differs in terms of computability, i.e. some are harder to compute and some are easier. So we have P, NP, NP-Complete and NP-Hard class to differentiate those problems. 

---

## Contents
- [Definition](#definition)
- [Reduction](#reduction)
- [Relation of P, NP and NPC](#relation-of-p-np-and-npc)
- [Establish of NP-Completeness](#establish-of-np-completeness)

---

## Definition

Among all the problems, the easiest ones are called P problems, which can be solved in polynomial time. [Wikipedia](https://en.wikipedia.org/wiki/P_(complexity)) defines P problems as "all decision problems can be solved by a deterministic Turing machine using a polynomial amount of computation time, or polynomial time".

Famous NP problem: [Travelling Salesman Problem (TSP)](https://en.wikipedia.org/wiki/Travelling_salesman_problem)

![Screenshot](/assets/blogs/P_NP_NPC/TSP.png)
- For each two cities, an integer cost is given to travel from one of the
two cities to the other. The salesperson wants to make a minimum
cost circuit visiting each city exactly once.
- TSP: Given a complete graph G=(V,E), a cost function w:E->N, and an
integer k, is there a cycle C going through each vertex once and only
once, with ∑<sub>e∈C</sub> w(e) ≤ k ?

NP problems are defined as problems whose solution can be verified in polynomial time. NP is the "set of all decision problems for which the instances where the answer is 'yes' have efficiently verifiable proofs.More precisely, these proofs have to be verifiable by deterministic computations that can be performed in polynomial time" [Wikipedia](https://en.wikipedia.org/wiki/NP_(complexity)). 

More harder problems are NP-Hard problems, they're at least as hard as the hardest problems in NP. And NP-Complete problems are defined as problems belonged to both NP and NP-Hard (i.e. A problem Y is NP-hard if X <=<sub>p</sub> Y for all X ∈ NP)

---

## Reduction
 
Reducible is a very important concept here. Given two problems X and Y, reduction from X to Y is showing that we can solve X using the
algorithm that solves Y. In other words, Y is harder to solve than X.

X is reducible to Y can be written as **X <=<sub></sub> Y**

### Polynomial Reduction

- There exists a function f that converts the input of X to inputs of Y in
polynomial time
- X(i) = YES <=> Y(f(i)) = YES 
- X is polynomial reducible to Y can be written as **X <=<sub>p</sub> Y**

![Screenshot](/assets/blogs/P_NP_NPC/reduction.png)

---

## Relation of P, NP and NPC

To sum up, NP problems are problems we don't know if we can solve them in polynomial time, but given a candidate solution, the solution can be verified in polynomial time. Some problems in NP has been proved that they can be solved in polynomial time, so we call those problems P problems since they're easy. However, what about other NP problems? Are they also polynomial solvable? Will one day in the future someone discovered an algorithm to solve all NP problems in polynomial time? Are all NP problems are actually P problems but human being didn't find out? 

To prove NP = P is difficult, so we come up with NP-Complete problems. By definition of reduction, if any NP-Complete problem can be solved in polynomial time, then all NP problems can be solved in polynomial time, so NP = P. But until now no one has proved that.

The difference between NP-Hard and NP-Complete is that for NP-Hard, it's difficult to verify the candidate solution in polynomial time or it cannot be verified in polynomial time, thus NP-Hard includes NP-Complete. These are some problems which in NP-Hard are not NP-Complete, e.g. [Halting Problem](https://en.wikipedia.org/wiki/Halting_problem).

![Screenshot](/assets/blogs/P_NP_NPC/P_np_np-complete_np-hard.svg)

---

## Establish of NP-Completeness

Recipe to establish NP-completeness of problem Y.
- Step 1. Show that Y is in NP.
- Step 2. Choose an NP-complete problem X.
- Step 3. Prove that X <= <sub>p</sub> Y (poly-time reduction).

![Screenshot](/assets/blogs/P_NP_NPC/problems.png)
<center>How NP-C problems are discovered</center>

Here is a [list](https://en.wikipedia.org/wiki/List_of_NP-complete_problems) of NP-Complete problems.



