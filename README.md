# Reinforcement-Learning-Actor-Critic-Methods

## Introduction
This project demonstrates the implementation of the Advantage Actor-Critic (A2C) algorithm to solve the CartPole-v1 environment. A2C is a type of actor-critic method that uses separate networks to approximate the policy (actor) and the value function (critic). This approach balances the need to explore the action space (actor) with the requirement to evaluate actions and states effectively (critic).

## Environment Description

### CartPole-v1:
The CartPole-v1 environment is a classic control problem available in Gymnasium. The goal is to balance a pole on a moving cart by applying force to move the cart left or right.

#### Action Space:
- The action space has 2 possible actions:
  - `0`: Push the cart to the left.
  - `1`: Push the cart to the right.

#### Observation Space:
The observation space consists of 4 features:
- **Cart Position:** The position of the cart on the track.
- **Cart Velocity:** The speed of the cart.
- **Pole Angle:** The angle of the pole with the vertical.
- **Pole Angular Velocity:** The speed at which the pole is rotating.

#### Termination Criteria:
The episode terminates when:
- The cart's position is outside the range (-2.4, 2.4).
- The pole's angle exceeds ±12 degrees.

#### Rewards:
- The agent receives a reward of +1 for each time step the pole remains balanced.

#### Solved Criteria:
- The environment is considered solved when the agent consistently achieves a score of 500.

## Advantage Actor-Critic (A2C)

### Overview:
A2C utilizes two networks:
- **Actor Network:** Learns the policy by outputting the probability distribution over actions given an observation.
- **Critic Network:** Estimates the value function, guiding the actor network on how good the taken action is.

### Advantage Calculation:
The advantage value used in A2C is calculated as:
\[ \text{Advantage} = \text{Reward} + \gamma \cdot V(\text{next state}) - V(\text{current state}) \]

### Loss Functions:
- **Actor Loss:** 
  \[ -\log(\pi(a|s)) \cdot \text{Advantage} \]
- **Critic Loss:** 
  \[ \text{MSE}(\text{Reward} + \gamma \cdot V(\text{next state}), V(\text{current state})) \]

## Network Architectures

### Actor Network:
- **Input Layer:** 4 neurons (for the observation space).
- **Hidden Layer 1:** 128 neurons, ReLU activation.
- **Hidden Layer 2:** 64 neurons, ReLU activation.
- **Output Layer:** 2 neurons (for the action probabilities), Softmax activation.

### Critic Network:
- **Input Layer:** 4 neurons (for the observation space).
- **Hidden Layer 1:** 64 neurons, ReLU activation.
- **Output Layer:** 1 neuron (for the state value), Linear activation.

## Implementation Details

The A2C algorithm was implemented to balance the cart and pole system in the CartPole-v1 environment. Key aspects include:
- **Policy Learning:** The actor network learns to predict the optimal action probabilities.
- **Value Estimation:** The critic network approximates the value of the state-action pairs.

### Results:
- The agent successfully learned to achieve the maximum reward of 500 in each episode.
- During training, the agent occasionally forgot the optimal policy. Implementing an **early-stopping** criterion ensured robust learning and prevented overfitting by stopping training after achieving a max reward of 500 for 10 consecutive episodes.

![image](https://github.com/imalhotra15/Reinforcement-Learning-Actor-Critic-Methods/assets/118845522/bb4228a3-4a4b-4209-bfdb-e1aafe4eb570)

### Discussion:
The early-stopping mechanism played a crucial role in:
- **Stabilizing Learning:** Preventing the agent from drastically forgetting the learned optimal policy.
- **Improving Efficiency:** Reducing unnecessary computational resources by terminating training once the agent reached a satisfactory performance level.
- **Enhancing Generalization:** Ensuring the agent did not overfit to specific episodes by stopping training upon achieving consistent high rewards.

### Applying A2C for Acrobot:

![image](https://github.com/imalhotra15/Reinforcement-Learning-Actor-Critic-Methods/assets/118845522/c666bcde-20bb-4882-8eb5-c5a19f0480e5)

![image](https://github.com/imalhotra15/Reinforcement-Learning-Actor-Critic-Methods/assets/118845522/ab38bfe2-f5fd-4be4-a05d-ddb9afacb201)


### Summary
Two A2C algorithms were developed in this assignment, one for discrete action space and one
for continuous action space. The discrete algorithm was tested on CartPole and Acrobot
whereas the continuous A2C was implemented on BiPedal, MountainCar and Pusher.

#### A2C discussion for Discrete Environment:

A2C algorithm was successful to solve the different discrete environments that we tried
(Cartpole, Acrobot). After training, the greedy-policy followed was by choosing the action with
the highest probability from the trained actor-network.
The challenges faced during training were due to learning instability. The algorithm would train
to get the optimal policy but would immediately ‘forget’ the policy and the rewards would drop
suddenly. This issue is mitigate development by incorporating early-stopping while training.

#### A2C discussion for Continuous Environment:

The inability of A2C to explore due to shrinking of standard deviation led to learning of a
particular action instead of a generalized solution. This also occurred because of exploring poor
transitions (Transition with high negative reward) a lot more frequently than good
transitions(Transition with positive rewards), which cause the network to learn to find a solution
of local minima and not explore high rewarding future states and action. This is one of the major
challenges of the policy gradient method using Actor Critic methods, which have been solved by
adding replay buffers to store high priority transitions and sample good actions frequently in the
DDPG algorithm. Consequently, A2C struggled to discover the optimal policy in continuous
environments like Pusher, MountainCar continuous, and Bipedal

# Notes

This was done as a part of the coursework of CSE 568 at the University at Buffalo. The source code is not available publicly to avoid academic integrity violations. Please feel free to contact the author if you wish access to the source code.
