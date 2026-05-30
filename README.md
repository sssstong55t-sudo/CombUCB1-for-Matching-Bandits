# CombUCB1 for Matching Bandits

This repository contains a reproduction and extension study of **CombUCB1 for matching bandits**. The project focuses on online learning in matching markets, where an algorithm repeatedly selects matchings under uncertainty and learns from observed rewards.

## Project Overview

The goal is to study how **Combinatorial Upper Confidence Bound (CombUCB1)** can be applied to matching bandit problems. The repository includes:

- A reproduction of the baseline CombUCB1 matching bandit setting
- Extension 1: many-to-one student-college matching with college capacities
- Extension 2: non-linear reward matching bandits with diversity and interaction terms
- Experimental results and analysis
- A final report summarizing the method, implementation, and findings

## Repository Structure

```text
.
├── Reproduction.ipynb      # Reproduction of the original CombUCB1 setting
├── EXTENSION1.ipynb        # Many-to-one matching extension
├── EXTENSION2_2.ipynb      # Non-linear reward extension
├── FinalReport.pdf         # Final project report
└── README.md
```

## Method

CombUCB1 is a bandit algorithm designed for combinatorial action spaces. In this project, each action corresponds to a matching. The algorithm balances:

- **Exploitation**: choosing matchings with high estimated rewards
- **Exploration**: trying uncertain matchings using confidence bounds

At each round, the algorithm computes UCB scores for possible student-college edges and selects a matching that maximizes the estimated optimistic reward.

## Reproduction

The reproduction notebook implements the basic CombUCB1 setting for matching bandits. The learner does not know the true reward of each edge at the beginning. It repeatedly selects a matching, observes stochastic rewards from the selected edges, updates the estimated means, and tracks cumulative regret over time.

## Extension 1: Many-to-One College Matching

The first extension changes the matching setting from a simple one-to-one matching problem to a **many-to-one student-college matching problem**.

In the basic setting, each college can usually be matched with only one student. However, in many real matching markets, one college can accept multiple students. Therefore, Extension 1 adds **college capacities**.

For example, if a college has capacity 2, then two students can be assigned to that college. To solve this using the Hungarian algorithm, the implementation converts each college into several **virtual slots**. Each slot represents one available seat in that college.

Example:

```text
College A with capacity 3
→ A_slot_1, A_slot_2, A_slot_3
```

After this transformation, the many-to-one problem becomes a standard one-to-one bipartite matching problem over students and virtual college slots. The algorithm can then still use the Hungarian algorithm as the matching oracle.

The purpose of this extension is to show that CombUCB1 can be adapted to more realistic matching markets where institutions have different capacities.

## Extension 2: Non-Linear Reward Matching Bandit

The second extension changes the reward model. In the original setting, the total reward of a matching is usually **linear**, meaning it is just the sum of the rewards of all selected edges.

Extension 2 studies a more complex case where the value of a matching is not only determined by individual student-college rewards, but also by the structure of the whole matching.

The reward contains three parts:

```text
Total reward =
edge reward
+ diversity bonus
+ pairwise interaction reward
```

This means a matching can be better not only because each individual pair is good, but also because the selected set of matches is diverse or has useful interactions between pairs.

The notebook compares:

1. **Linear CombUCB1 baseline**  
   This method only considers edge-level UCB scores and ignores non-linear terms.

2. **Non-linear UCB oracle**  
   This method still uses UCB estimates for unknown edge rewards, but the oracle also includes known diversity and interaction terms when choosing the matching.

The purpose of this extension is to test whether a matching bandit algorithm can handle more realistic objectives where the quality of a matching depends on the selected set as a whole, not just independent edge rewards.

The experiments evaluate cumulative regret, sensitivity to the strength of the non-linear reward, scaling behavior, ablation of reward components, and oracle runtime.

## How to Run

Open the notebooks in Jupyter Notebook or JupyterLab:

```bash
jupyter notebook
```

Then run the notebooks in the following order:

1. `Reproduction.ipynb`
2. `EXTENSION1.ipynb`
3. `EXTENSION2_2.ipynb`

## Requirements

The project uses Python and common scientific computing libraries:

```text
numpy
matplotlib
scipy
networkx
```

Install dependencies with:

```bash
pip install numpy matplotlib scipy networkx
```

## Results

The experiments compare the behavior of CombUCB1 under different matching bandit settings. The main evaluation focuses on cumulative regret and the quality of the learned matching policy over time.

For full details, see `FinalReport.pdf`.

## Authors

Course project by:

- Sai Sharan
- Shaotong Sun
- Yiqi Li
- Bole Yi

