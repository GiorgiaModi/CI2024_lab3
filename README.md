# CI2024_lab3
This repository contains an implementation of two different algorithms to solve the **n²-1 puzzle**. The puzzle consists of a square grid (with a dimension of n x n) with tiles numbered from 1 to n²-1, and an empty space (0) that allows the tiles to slide.

The goal of the puzzle is to arrange the tiles in a particular order by sliding them into the empty space. The challenge is to find the sequence of moves that leads from a scrambled initial configuration to the goal configuration, using the fewest number of moves.

For example, for a 4x4 puzzle (n = 4), the goal configuration is:

  ```
  1  2  3  4
  5  6  7  8
  9  10 11 12
  13 14 15 0
  ```

## Solution Strategies Implemented

Two search algorithms were implemented to solve the puzzle: **A\* Search** and **Depth-First Search with Bound (Iterative Deepening DFS)**. Both algorithms find the sequence of moves needed to transition from an initial scrambled state to the goal state.

### Algorithm 1: A* Search
A* is a popular **informed search algorithm** that uses both the current cost (g-score) and a heuristic (h-score) to prioritize the most promising states to explore. 

- **g-score**: The cost of reaching the current state from the start.
- **h-score**: A heuristic estimate of the cost from the current state to the goal. In this case, the heuristic used is the **Manhattan Distance**, which calculates the sum of the vertical and horizontal distances between the current position and the target position of each tile.
- **f-score**: The sum of the g-score and h-score (`f = g + h`), which is used to prioritize which states to explore first. States with the lowest f-score are expanded first.

**Steps**:
1. Start from the initial scrambled state.
2. Use a priority queue (min-heap) to store states, where the priority is determined by the f-score.
3. Expand the state with the lowest f-score, calculate its neighbors, and add them to the queue.
4. Repeat the process until the goal state is found or no solution is possible.


A* Search garantees the optimal solution with the least number of moves, but it's more memory-intensive due to storing all explored states and their f-scores.

### Algorithm 2: Depth-First Search with Bound (Iterative Deepening DFS)
Depth-First Search (DFS) is an uninformed search algorithm that explores the search space by going as deep as possible down one path before backtracking. The algorithm with a bound places a limit on the depth of the search to prevent it from going infinitely deep.

**Iterative Deepening DFS** is an enhancement of DFS that gradually increases the depth limit, performing DFS for each depth limit starting from 0 until a solution is found or the maximum depth is reached.

**Steps**:
1. Start from the initial scrambled state.
2. Perform DFS within the first depth limit.
3. If the solution is not found, increment the depth limit and repeat the process.
4. Continue until the goal state is reached or the maximum depth is reached.

Depth-First Search with Bound is more memory-efficient but can take longer to find a solution compared to A*.
