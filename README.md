# Reinforcement Learning and the Racetrack Problem

An implementation and analysis of three reinforcement learning algorithms applied to the racetrack control problem.  
Developed for **CSCI 446: Artificial Intelligence** at Montana State University.

---

## Overview

The goal of this project was to implement and evaluate three reinforcement learning algorithms on the racetrack problem, a stochastic control task where an agent must learn to navigate from a starting line to a finish line as efficiently as possible.

The environment is modeled as a grid-based track where the agent’s state consists of its position and velocity, and actions correspond to accelerations in the x and y directions. Each action has a **20% chance of failure**, introducing stochasticity into the environment.

We evaluated the algorithms on three different track layouts, **2-Track**, **U-Track**, and **W-Track**, under two crash scenarios:
- **NRST**: reset to the nearest valid track position
- **STRT**: reset to the original starting position

This project explored the tradeoffs between model-based and model-free reinforcement learning in terms of optimality, learning speed, stability, and scalability.

---

## Algorithms Implemented

1. **Value Iteration (VI)** –  
   A model-based reinforcement learning algorithm that computes the optimal policy by iteratively applying the Bellman optimality equation. Requires full knowledge of the transition model and guarantees optimal solutions for finite MDPs.

2. **Q-Learning (QL)** –  
   An off-policy, model-free reinforcement learning algorithm that learns an action-value function through sampled experience. Uses an ε-greedy exploration strategy and converges to the optimal policy given sufficient exploration.

3. **SARSA** –  
   An on-policy, model-free reinforcement learning algorithm that updates action values based on the agent’s actual behavior policy. Tends to learn safer and more conservative policies compared to Q-Learning.

All algorithms were implemented from scratch in Python without using external reinforcement learning libraries.

---

## Results
Across all tracks and crash types, we saw consistent levels of performance for all three
algorithms. The biggest differences depended on whether it was a model-based or model-free
learning efficiency. Overall, Value Iteration produced the fastest and most reliable solutions
across every Track and crash type. Q-Learning and SARSA required significantly more
time training to reach similar success rates, and their learning dynamics varied substantially
depending on the track complexity and crash rules. On average SARSA and Q-Learning
also needed more steps than Value Iteration.

Value Iteration has full access to the model of the track, Value Iteration consistently
reached a 100 percent success rate with extremely less average steps than both SARSA and
Q-Learning. For example, only `10.1` steps on W-Track (STRT) compared to over `200` steps
for Q-Learning under the same conditions. (Table 6). However, Value Iteration does need
to complete multiple sweeps over the entire race track. until convergence, which becomes
costly on larger or more complex tracks such as W-Track.

Q-Learning and SARSA scaled similarly, since they both rely on sampled interactions
rather than full track sweeps. Q-Learning converged faster in deterministic settings but
was significantly slower when exploration was high. Q-Learning uses a greedy policy, this
leads to reapeatedly overestimating values early on in the process. We can see this out-
line with visible spikes in the learning curve. (Figure 2). SARSA did something a little
different. It maintained a smoother and more stable learning throughout. SARSA is more
sensitive to the behavior policy. This resulted in more cautious value estimates that avoided
the dramatic overshooting seen in Q-Learning. As a result, SARSA performed better in
environments where exploration leads to worse transitions.

Overall, our findings support the initial hypothesis. Value Iteration performed by far the
strongest due to the full model of the race tracks being available. Value Iteration achieved
the shortest paths and perfect success rates. Q-Learning and SARSA both scale better
to large state spaces where a model is unavailable, but they showed long learning times
and sensitivity to parameters like track geometry. Q-Learning is more aggressive and can
converge faster in some cases, while SARSA is more stable and risk-averse. Together, these
differences highlight the core trade-off between model-based and model-free reinforcement
learning: VI achieves optimality through computation, whereas Q-Learning and SARSA
achieve competence through experience.

---

## Files

- `CSCI_446_Project4.ipynb` – Complete implementation of Value Iteration, Q-Learning, and SARSA  
- `CSCI446_Project4_Report.pdf` – Full report paper with experimental results and analysis  

---

## License

MIT License – see [LICENSE](./LICENSE) for details.
