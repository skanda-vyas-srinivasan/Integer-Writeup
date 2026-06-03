# Title

## Introduction
Learned branching policies replace traditional heurisitcs in branch and bound by utilizing machine learning to imitate optimal branching patterns at a fraction of the overhead. However, they are heavily constrained.

These models are generally trained on a specific MILP problem instance, which makes them very effective at accelerating solving of instances within their training class. However, their performance is terrible when solving any out-of-distribution instances with their predictions being akin to random noise.

This leads to some important questions: what if two problem instances are different but are structurally similar? Could a model trained on one be helpful to solve the other? Conversely, if an instance is structurally far from the training instance would the learned policy become even less reliable?

This article details an independent project that utilizes Maudet Distances to discuss whether structural similarity between MILP instances can predict transferability of a learned branching policy across problem classes. I train a Learn2Branch style graph neural network branching policy on set-cover instances, then evaulate how its performance changes on other MILP problem classes as their Maudet distances increase.

## Definitions
Before we get started on the actual problem, I'll go over some of the ideas that we will use in this article. I'll be very brief, for which I linked material that can be used for a hollistic understanding of each topic

### Mixed-Integer Linear Programming


Following the standard formulation in Conforti et al., a mixed-integer linear program (MILP) can be written as

\[
\min \{ c^\top x + h^\top y : Ax + Gy \leq b,\; x \in \mathbb{Z}^n,\; y \in \mathbb{R}^p \}.
\]

Here, \(x\) represents the integer decision variables, \(y\) represents the continuous decision variables, \(c\) and \(h\) define the linear objective, and \(A\), \(G\), and \(b\) define the linear constraints. The presence of integer variables makes MILPs much harder than ordinary linear programs, so solvers typically rely on methods such as branch-and-bound.

A mixed integer-linear program according to 
 ### Machine-Learning Augmented Branching
 Generally in a branch and bound MILP solver, branching helps decide which variable to split on when the relaxation is fractional. ML - Augmented branching replaces a traditional heuristic choosing of variables with a learned model that guides the choice by scoring candidate variables. In this project, the learned model does not solve the MILP directly. It only influences the branching decisions made inside SCIP.
 
Some great resources if you want to learn more about this topic:
- https://www.youtube.com/watch?-v=SEZC03h6cIs&t=1938s
- https://www.youtube.com/watch?v=_PfCnScRVYA
- Scavuzzo et al., [Machine learning augmented branch and bound for mixed integer linear programming](https://link.springer.com/article/10.1007/s10107-024-02130-y), Mathematical Programming, 2024.



### Maudet Distances


Maudet distances detail a way to compare MILP instances by their structure. In this project, I use this distance to measure how close a test instance is to the training set, i.e. set cover isntances. For each test instance, I compute its distance to all the instances in a reference set of the set-cover problems. I then average all of those distances to get one structural distance score for that test instance.

For a hollistic understanding of the Maudet distance, I implore you to read the original paper:
[Maudet and Danoy, *A Distance Metric for Mixed Integer Programming Instances*](https://arxiv.org/abs/2507.11063)









