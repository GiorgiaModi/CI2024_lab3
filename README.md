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

## Results
Execution time increases significantly as the size of the puzzle increases. For sizes larger than 3x3, the execution time is highly variable. For example for 4x4, the time can range from 1-2 minutes to up to 30 minutes, while for 5x5, it can take more than two hours. 

Execution time significantly depends on the number of RANDOMIZED_STEPS used to generate the initial configuration. For 5x5 puzzles and larger, reducing RANDOMIZED_STEPS to 100 yields a solution in a reasonable amount of time. However,  even adding just a small amount to RANDOMIZED_STEPS results in a dramatic increase in execution time, making larger puzzles infeasible. Additionally, for DFS, increasing the `max_depth` to 25 has been tested to better handle the complexity of these puzzles. 

A new heuristic function, **linear conflict**, was also implemented but did not improve performance compared to the Manhattan distance heuristic.

In general, the results are **very** dependent on the initial configuration, so the results shown in this table are only indicative. For DFS with bounds, it can happen that no solution is found within the given depth limit, so the depth limit should be increased accordingly.

| Method | Size | Time         | Initial Step                                                                                                   | Final Step                                                                                                     | Notes                                        |
|--------|------|--------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| A*     | 2x2  | 0.01s        | `[[3 1]` <br> `[2 0]]`                                                                                        | `[[1 2]` <br> `[3 0]]`                                                                                        | RANDOMIZED_STEPS = 10,000, <br> #steps = 4                    |
|        | 3x3  | 0.01s        | `[[2 6 4]` <br> `[5 7 3]` <br> `[0 1 8]]`                                                                     | `[[1 2 3]` <br> `[4 5 6]` <br> `[7 8 0]]`                                                                     | RANDOMIZED_STEPS = 10,000, <br> #steps = 16                    |
|        | 4x4  | 1m 38.6s     | `[[13 14 7 6]` <br> `[3 0 1 8]` <br> `[2 12 9 15]` <br> `[11 10 4 5]]`                                         | `[[1 2 3 4]` <br> `[5 6 7 8]` <br> `[9 10 11 12]` <br> `[13 14 15 0]]`                                         | RANDOMIZED_STEPS = 10,000, <br> #steps = 52                    |
|        | 5x5  | 1.5s         | `[[7 3 4 5 10]` <br> `[2 0 1 8 9]` <br> `[6 12 13 19 14]` <br> `[11 16 18 24 15]` <br> `[21 17 22 23 20]]`       | `[[1 2 3 4 5]` <br> `[6 7 8 9 10]` <br> `[11 12 13 14 15]` <br> `[16 17 18 19 20]` <br> `[21 22 23 24 0]]`       | RANDOMIZED_STEPS = 100, <br> #steps = 24                       |
|        | 6x6  | 1.5s         | `[[1 15 2 10 11 6]` <br> `[3 0 4 5 9 12]` <br> `[7 8 14 16 17 18]` <br> `[13 20 21 28 22 24]` <br> `[19 26 33 27 23 30]` <br> `[25 31 32 34 29 35]]` | `[[1 2 3 4 5 6]` <br> `[7 8 9 10 11 12]` <br> `[13 14 15 16 17 18]` <br> `[19 20 21 22 23 24]` <br> `[25 26 27 28 29 30]` <br> `[31 32 33 34 35 0]]` | RANDOMIZED_STEPS = 100, <br> #steps = 36                     |
| DFS    | 2x2  | 0.01s        | `[[3 1]` <br> `[2 0]]`                                                                                        | `[[1 2]` <br> `[3 0]]`                                                                                        | RANDOMIZED_STEPS = 10,000, <br> Solution at Depth: 4, <br> #steps = 4          |
|        | 3x3  | 0.01s        | `[[2 6 4]` <br> `[5 7 3]` <br> `[0 1 8]]`                                                                     | `[[1 2 3]` <br> `[4 5 6]` <br> `[7 8 0]]`                                                                     | RANDOMIZED_STEPS = 10,000,<br> Solution at Depth: 18, <br> #steps = 16         |
|        | 4x4  | 4.1s         | `[[13 14 7 6]` <br> `[3 0 1 8]` <br> `[2 12 9 15]` <br> `[11 10 4 5]]`                                         | -                                                                                                              | RANDOMIZED_STEPS = 10,000,<br> No solution found (limit: 20) |
|        | 5x5  | 1m 13.5s     | `[[7 3 4 5 10]` <br> `[2 0 1 8 9]` <br> `[6 12 13 19 14]` <br> `[11 16 18 24 15]` <br> `[21 17 22 23 20]]`       | `[[1 2 3 4 5]` <br> `[6 7 8 9 10]` <br> `[11 12 13 14 15]` <br> `[16 17 18 19 20]` <br> `[21 22 23 24 0]]`       | RANDOMIZED_STEPS = 100, <br> Solution at Depth: 24, <br> #steps = 24           |
|        | 6x6  | 11m 37.7s    | `[[1 15 2 10 11 6]` <br> `[3 0 4 5 9 12]` <br> `[7 8 14 16 17 18]` <br> `[13 20 21 28 22 24]` <br> `[19 26 33 27 23 30]` <br> `[25 31 32 34 29 35]]` | -                                                                                                              | RANDOMIZED_STEPS = 100, <br> No solution found (limit: 25)   |

`RANDOMIZED_STEPS` refers to the number of random moves (or randomization steps) used to generate the initial configuration of the puzzle before the algorithm begins solving it.

`#steps` refers to the number of steps (i.e., individual moves or edits) taken by the algorithm to reach the goal solution, where each step represents a change in the matrix configuration.
