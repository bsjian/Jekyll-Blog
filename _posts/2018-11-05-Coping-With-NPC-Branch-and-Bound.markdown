---
title: "Coping With NP-Completeness - Branch and Bound"
layout: post
date: 2018-11-05 10:00
image: /assets/images/markdown.jpg
headerImage: false
tag:
- NP-Complete
- algorithms
- proof
- problems
category: blog
author: boshengjian
description: Coping with NP-Completeness
# jemoji: '<img class="emoji" title=":ramen:" alt=":ramen:" src="https://assets.github.com/images/icons/emoji/unicode/1f35c.png" height="20" width="20" align="absmiddle">'
---

## Summary:

This post will introduce branch and bound method. 
- [Introduction](#introduction)
- [Pseudo-Code](#pseudo-code)
- [Travelling-Salesman-Problem](#travelling-salesman-problem)
   * [TSP-Bounding](#tsp-bounding)
   * [Naive-Bounding](#naive-bound)
   * [Reduced-Cost-Matrix](#reduced-cost-matrix)
   * [Dynamic-MST-Bounding](#dynamic-mst-bounding)
- [TSP-Branching](#tsp-branching)

---

## Introduction

Branch and Bound method is an enhancement of [Backtracking](http://www.boshengjian.com/General-Solution-to-Backtracking-Questions/#formula)

Facts on Branch and Bound:
- Applicable to optimization problems (assume we're minimizing).
- Keep track of the BSET solution found so far (upper bound on optimal)
- For each node (partial solution), computes a lower bound LB on the value of the objective function for all descendants of node. (removing all of its child, pruning part of the tree)
- Use the bound for: 
  + ruling our certain nodes as "non-promising" to prune the tree. 
  + guiding the search through state-space as a measure of “promise”

### Pseudo Code

<center><img src="/assets/blogs/coping/branch_algo.png" alt="drawing"/></center>

### Travelling Salesman Problem
[Travelling Salesman Problem (TSP)](https://en.wikipedia.org/wiki/Travelling_salesman_problem): Given a complete graph G=(V,E), a cost function w:E->N, and an integer k, is there a cycle C going through each vertex once and only once, with ∑<sub>e∈C</sub> w(e) ≤ k ?

<center><img src="/assets/blogs/coping/TSP.png" alt="drawing"/></center>

- Partial Solution (a, T, b): a path from a start node a to b, going through nodes T (same as HamCycle). 
- Expand: choose an edge from b to V-T-{a,b} (same as HamCycle)
- Choose: what can be the best-first criteria?
  + The partial assignment with smallest lower bound(most promising of
having a short TSP tour)
- Check:
  + Same feasibility checks as HamCycle to detect dead-end
  + How do we compute a Lower Bound given a partial solution (a,T,b)?


#### TSP Bounding 

##### Naive Bounding

<center><img src="/assets/blogs/coping/TSP_bound_1_1.png" alt="drawing"/></center>

Because a tour must leave every vertex exactly once, a lower bound on the length of a tour is the sum of the minimum cost of leaving every vertex. 

Every vertex must be entered and exited exactly once.
For a given edge (u, v), think of half of its weight as the exiting cost of u, and half of its weight as the entering cost of v 
- The total length of a tour = the sum of costs of visiting (entering and exiting) every vertex exactly once. 
- a lower bound on the length of a tour is the sum of the lower bound on the cost of entering and leaving every vertex. 

##### Reduced Cost Matrix

Step 1 to reduce: Search each row for the smallest value
<center><img src="/assets/blogs/coping/reduced_cost_row.png" alt="drawing"/></center>

Step 2 to reduce: Search each column for the smallest value
<center><img src="/assets/blogs/coping/reduced_cost_col.png" alt="drawing"/></center>

The total cost of 84+12=96 is subtracted. Thus, we know the lower bound
of feasible solutions to this TSP problem is 96.

##### Dynamic MST Bounding
If the TSP is symmetric, then our path will include a Minimum Spanning Tree.

Given a partial solution (a,T,b)
We have a path from a to b using vertices T ⊆ V −{a, b}
A lower bound is the sum of:
- The partial path we have
- A lower bound on exiting a and b (their shortest edge to a vertex in V-T-{a,b})
- A lower bound on visiting the remaining nodes (The cost of the Minimum Spanning Tree for the subgraph over nodes in V-T-{a,b} )

#### TSP Branching

Different ways to explore the same search space.
<!-- Binary Choice:
(a, T, b): 
include (b,c) -> (a, T U{b} , c)
exclude (b,c) -> (a, T, b) Yi(b,c) , include(b,d) or exclude(b,d)

Multiple Choice: -->

Consider Reduced Cost Matrix, if we take the edge (4,6), Lower Bound won't change; if not taking that edge, add 32 to our Lower Bound.

<center><img src="/assets/blogs/coping/TSP_decision_tree.png" alt="drawing"/></center>
<center>High-Level State-Space Tree</center>

Then, again reduce the current matrix by one row and one column to get the current Lower Bound. Set the cost (6,4) to infinity (so that we 're not going back to 4).

