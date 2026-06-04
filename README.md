# Title

## Introduction
Learned branching policies replace traditional heuristics in branch and bound by utilizing machine learning to imitate optimal branching patterns at a fraction of the overhead. However, they are heavily constrained.

These models are generally trained on a specific MILP problem instance, which makes them very effective at accelerating solving of instances within their training class. However, their performance is terrible when solving any out-of-distribution instances with their predictions being akin to random noise.

This leads to some important questions: what if two problem instances are different but are structurally similar? Could a model trained on one be helpful to solve the other? Conversely, if an instance is structurally far from the training instance would the learned policy become even less reliable?

This article details an independent project that utilizes Maudet Distances to discuss whether structural similarity between MILP instances can predict transferability of a learned branching policy across problem classes. I train a Learn2Branch style graph neural network branching policy on set-cover instances, then evaulate how its performance changes on other MILP problem classes as their Maudet distances increase.

## Definitions
Before we get started on the actual problem, I'll go over some of the ideas that we will use in this article. I'll be very brief, for which I linked material that can be used for a hollistic understanding of each topic

### Mixed-Integer Linear Programming


Following the formulation given in the Conforti, Cornejouls and Zambelli textbook, A Mixed-Integer Linear Program are problems in the form:



![Standard MILP formulation](https://raw.githubusercontent.com/skanda-vyas-srinivasan/Branch-Learning-Distance-Threshold/main/assets/milp-formulation.svg)


Here, **x** represents the integer decision variables and **y** represents the continous decision variables. **c** and **h** are vectors that define the linear objective, and the matrices **A**, **G**, and **b** define the linear constraints.


Some great resources on Linear and Integer Programming are listed down below:
- Alberto Del Pia, [Linear Optimization Playlist](https://www.youtube.com/playlist?list=PLeO_PhASIA0Ot69TqANAnNxoykHGOQp2Y)
- Alberto Del Pia, [Integer Optimization Playlist (Incomplete)](https://www.youtube.com/playlist?list=PLeO_PhASIA0NtvLCAZXLC8HACOgVD9Y32)
- Dimitris Bertsimas and John N. Tsitsiklis, [Introduction to Linear Optimization](http://athenasc.com/linoptbook.html)
- Michele Conforti, Gérard Cornuéjols, and Giacomo Zambelli, [Integer Programming](https://link.springer.com/book/10.1007/978-3-319-11008-0)




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

## Experiment Setup

The experiment uses a learned-branching policy trained on one set-cover distribution: instances with 500 rows, 1000 columns, and desnity 0.05. The policy follows the Learn2Branch-style graph neural network architecture, where each MILP state is represented as a bipartite gra








