---
title: "Coping With NP-Completeness - Local Search"
layout: post
date: 2018-11-13 12:00
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
    + [Search-Process](#search-process)
    + [Several-Concepts](#several-concepts)
        * [TSP](#tsp)
- [Hill-Climbing](#hill-climbing)
- [Stochastic-Local-Search](#stochastic-local-search)
    + [Random-restart-hill-climbing](#random-restart-hill-climbing)
    + [The-WalkSAT-Algorithm](#the-walksat-algorithm)
- [Simulated-Annealing](#hill-climbing)

---

## Introduction

NP-Complete problems are very difficult and naive algorithms is typically of exponential run time complexity, which is very bad when the input is large. 

For Combinatorial Optimization Problems (such as TSP, 0-1 Knapsack, Assignment problem, Bin packing problem, SAT, Eight queens puzzle, etc.), local search can quickly find a solution for which you cannot give any quality guarantee (but which might often be good in practice on real problem instances)

### Search Process

Local Search can be generalized as:
- start from initial position
- iteratively move from current position to one of neighboring
positions
- use evaluation function to choose among neighboring positions

![LocalSearch](/assets/images/blogs/coping/local_search_metaphor.png)

### Several Concepts

Take SAT and TSP as examples, local search would be:

- **Search Space S:** 
    + SAT: set of all complete truth assignments to propositional variables (all “potential solutions”)
    + TSP: set of all permutations of vertices (all “potential
solutions”)
- **Solution Set S'⊆S:** 
    + SAT: all satisfying assignments for given formula
    + TSP: the tours of minimum length
- **Neighborhood Relation N(S)∈S :**
    + A way to move from one potential solution to another
    + SAT: neighboring variable assignments differ in the truth
    + TSP: neighboring tour differ in several edges
- **Evaluation Function g:** 
    + SAT: number of clauses satisfied under given assignment
    + TSP: length of tour

#### TSP 

![Screenshot](/assets/images/blogs/coping/tsp_neighboring.png)

Search Space: all permutations of the cities (each defines a cycle)
3-opt – delete 3 edges and reconnect fragments into 1 cycle
k-opt – delete k edges and reconnect fragments into 1 cycle


---

## Hill Climbing

Hill Climbing is basically choosing the neighbor with the largest improvement as the next state until a local optimal solution is reached. It's very fast and works well for certain problems.

![Screenshot](/assets/images/blogs/coping/hill_climbing.png)
![Screenshot](/assets/images/blogs/coping/hill_climbing_algo.png)

However, this local search only finds the local optimization, which is not guaranteed to be the overall optimal solution, as there might be multiple hills. And it is prone to be misguided by evaluation/objective function.

## Stochastic Local Search

Hill Climbing is a Greedy Algorithm which search a local optimization.

Compared to Hill Climbing, Stochastic Local Search is that when we make our decisions, instead of choosing the neighbor with the largest improvement as the next state (go up), we not only randomize initialization step but also choose the next state **randomly** such that suboptimal/worsening steps are
allowed.

Simple SLS methods include :

- Random Search (Blind Guessing): In each step, randomly select one element of the search space.
- (Uninformed) RandomWalk: In each step, randomly select one of the neighboring positions of the search space and move there.


### Random restart hill climbing

Steps:

- Start at random solution
- Hill-climb until local optima
- Start at another random position

![Screenshot](/assets/images/blogs/coping/random_hill_climbing.png)

### The WalkSAT Algorithm

```
WalkSAT(CNF, max-tries, max-flips, p) {
    for i ← 1 to max-tries do
        solution = random truth assignment
        for j ← 1 to max-flips do
            if all clauses in CNF satisfied then
                return solution
            c ← random unsatisfied clause in CNF with probability p
                flip a random variable in c
            else
                flip variable in c that maximizes number of satisfied clauses
return failure
}
```


## Simulated Annealing 

### SA Analogy

Annealing is process of heating and cooling metals in order to
improve strength
- Idea: Controlled heating and cooling of metal
- When hot, atoms move around
- When cooled, atoms find configuration with lower internal energy (i.e.
makes metal stronger)
Analogy:
- Temperature = probability of accepting worse neighboring solution
- When temperature is high, likely to accept worse neighboring solutions
(but may lead to better overall solution)
- Analogous to atoms wandering around
- Cooling represents shrinking probability of accepting worse solutions

### SA Pseudo code

Select a neighbor at random.
- If better than current state, go there (improving move).
- Otherwise, go there with some probability (worsening move).
- Probability goes down with time (similar to temperature cooling

![Screenshot](/assets/images/blogs/coping/sa_algo.png)

Acceptance criterion (Metropolis condition): choose new solution s' over old solution s with probability (maximization)

![Screenshot](/assets/images/blogs/coping/sa_formula.png)

- Initial temperature T0
- Annealing (cooling) schedule: how to update the
temperature
- E.g. T = a T with a =0.95 (geometric schedule)
- Number of iterations at each temperature (e.g. multiple of the
neighbourhood size)
- Stopping criterion
- E.g. no improved solution found for a number of iterations (or
number of temperature values)

