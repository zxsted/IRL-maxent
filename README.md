# Maximum Entropy Inverse Reinforcement Learning in GridWorld

## Credits:
The codebase used for the experiment has been ported from [Matthew Alger]
(matthew.alger@anu.edu.au) and then modified and trimmed for the purposes of this experiment. 
His github codebase can be found [here](https://github.com/MatthewJA/Inverse-Reinforcement-Learning)

## Contents
- [Experiments](#experiments)
    - [Experiment 1 - Effect of Random Starts](#experiment-1)
    - [Experiment 2 - Effect of Trust factor of trajectories](#experiment-2)
    - [Experiment 3 - Effect of Trajectory Length](#experiment-3)
    - [Experiment 4 - Effect of Varied Experts](#experiment-4)
- [Requirements](#requirements)
- [Run Instruction](#run-instructions)

## Experiments:
### Experiment 1
**Effect of Random Starts vs Fixed Point Starts on Trajectory Generation**

**Random Start**

![Random](/images/random=True.png) 

**Fixed Start at (0,0)**

![Fixed](/images/random=False.png)

#### Observation 1 
Fixed start biases the learning towards certain trajectories that are created from the fixed state. The trajectories from state never visited (or rarely visited) gets learnt less, and thus the model stays confused about their rewards distribution.

#### Observation 2 
Starting from same location and moving towards the positive reward location, can make the agent develop a notion of pseudo negative reward for the start location, as it is moving away from that location all the time. This can be seen in the magnitude of the start location to be very negative for the fixed start state situation, while this is effectively evened out with (0,0) being very close to 0 for the start state in random start case.

### Experiment 2 
**Effect of trust factor over trajectories generated**

**Trust = 0.1**

![Trust=0.1](/images/trust=0.1.png)

**Trust = 0.5**

![Trust=0.5](/images/trust=0.5.png)

**Trust = 0.9**

![Trust=0.9](/images/trust=0.9.png)

#### Observation
A IRL agent is as good as an expert. If each expert trajectory can only be trusted by a certain amount, as in if the trajectory is random a certain percentage of time in all the trajectories, then the IRL agent can get easily confused. It is visible in case when the expert chooses an action randomly 90 % of time. While there is a gradual improvement in performance for situations when expert chooses randomly only 50% and 10% of time respectively.

### Experiment 3 
**Effect of trajectory length**

#### High Trust Situation (trust = 0.9)

Trajectory Length = 5

![trajectory_length=5](/images/trajectory_length=5.png)

Trajectory Length = 10

![trajectory_length=10](/images/trajectory_length=10.png)

Trajectory Length = 15

![trajectory_length=15](/images/trajectory_length=15.png)

#### Observation
Shorter trajectories that are not enough to reach the goal makes the IRL agent falsely think that the location of the goal, and thus leads to poor result. This improves as the trajectory length increases. Even if the trajectory length is enough to reach the goal for proper reinforcement to happen, higher trajectory_length is still better as it makes the RL agent robust to random actions taken by experts. As now it has more good transitions. 

#### Low Trust Situation (trust = 0.1)

Trajectory Length = 15

![trajectory_length=15](/images/poor_trust_traj=15.png)

Trajectory Length = 25

![trajectory_length=25](/images/poor_trust_traj=25.png)

#### Observation
We can see that there is not much improvement in result from trajectory = 10 to trajectory = 15 when the experts are pretty good (trust = 0.9), but when the experts are very poor, it helps to have more trajectories, as we cans see a huge visible improvement form trajectory_length=15 to trajectory_length=25 for a poor ensemble of experts (trust = 0.1)


### Experiment 4 
**Effect of High variance in skills of expert assuming each trajectory generated by a unique expert**

**Varied Skill Experts**

![variant](/images/ensemble=0.5-(0.2,0.5,0.8).png)

**Uniform Skill Experts**

![uniform](/images/all=0.5.png)

Note - Both set of experts have same skill in expectation (=0.5)

#### Observation
We can see that the ensemble performs better than the set of experts that perform equally for each expert, as the first set of experts perform in expectation. This result is interesting, and shows that the flow in trajectory is very important in IRL. For the set where every expert performs equally, the trajectory is uniformly noisy, and thus lacks flow. However, for the case when there are experts who are pretty good, there trajectories are very good, and thus leads to better flow. Thus trajectory flow is important for Maximum entropy IRL.  

It initially does not appear to be intuitive as the feature construction takes an expectation across trajectories. But it should be notes that experts only visit good states that lead to goal sooner, while bad experts explore all states randomly. Thus even though in expectation the bad states are equal as if all the experts were equally bad, but the good states are disproportionately high for good experts, and relatively low (close to random) for bad experts. Thus in expectation it is better than the uniformly bad case. Hence it improves the feature expectation. 

## Requirements

- NumPy
- SciPy
- CVXOPT
- Theano
- MatPlotLib (for examples)

## Run Instructions
```
sh run_all_experiments.sh
```

You can take a look inside the above script to see how individual experiments are performed.