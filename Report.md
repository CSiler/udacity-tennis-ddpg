
# Tennis Game Report
## Learning Algorithm
### DDPG basics
This project's approach to solving the Tennis environment is based on the Deep Deterministic Policy Gradient (DDPG) algorithm as described in [this paper.](https://arxiv.org/pdf/1509.02971.pdf)

DDPG is an off-policy actor-critic network with the actor producing action values and the critic evaluating these actions. 

In order to avoid negative feedback preventing convergence toward optimal parameter values, DDPG uses the Double Q-network concept where a target 
and a local copy of the network are maintained. The local copy of the actor is used to produce the stream of actions the agent uses to navigate the environment while the target copy is used to determine a hypothetical next action to be evaluated by the critic. 

The Q-value the critic assigns to the current state and such an action serves as the surrogate to be used for training the actor network with gradient ascent.

In order to improve and train the critic network, a target copy of the critic is used to produce the future (t+1) Q-value which is discounted and added to the observed 
reward. This serves as the benchmark for a Q-value estimated by the local copy of the critic. The difference defines the loss to update the local copy of the critic 
with gradient descent.

Target parameter values for both actor and critic networks are softly updated over time with their local copies.

### Prioritized Experience Replay Buffer
DDPG uses a replay buffer that collects a history of observations including state, action, reward and next state. DDPG can be further improved by using a prioritized replay buffer as described in [this paper](https://arxiv.org/pdf/1511.05952.pdf).

The Tennis environment is hard to solve without prioritized sampling of experiences. When an untrained agent enters the game, positive rewards are rare as it is not sufficient to just hit the ball, the racket must also be moving in the right direction to drive the ball across the net. In the early stages of training the replay buffer will therefore be dominated by negative or zero reward experiences. In order to avoid such bias, priority is given to experiences with the highest learning impact, measured by difference between forecasted and actual Q-value. 

This algorithm shares experiences between agents. 

### Noise
DDPG adds noise to the actor's output to allow for exploration. An auto-correlated noise function as defined by Uhlenbeck & Ornstein is used, adjusted to be centered around 0.

<img src="https://github.com/CSiler/udacity-tennis-ddpg/assets/6819661/be34d620-9ee2-4721-aa76-4abd5d6d3962" width="50%">

## Hyper-parameters
```
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 5e-3              # for soft update of target parameters
LR_ACTOR = 3e-3         # learning rate of the actor 
LR_CRITIC = 3e-3        # learning rate of the critic
WEIGHT_DECAY = 0        # L2 weight decay
```

Uhlenbeck & Ornstein noise:
```
theta = 0.06
sigma = 0.2
```
Actor and critic networks:
```
Fully connected layer 1 nodes: 256
Fully connected layer 2 nodes: 128
```
## Model Architecture
### Actor
The actor networks use three fully connected layers, with the first two layers' output subject to batch normalization:
```
self.fc1 = nn.Linear(state_size, fc1_units)
self.bn1 = nn.BatchNorm1d(num_features=fc1_units)
self.fc2 = nn.Linear(fc1_units, fc2_units)
self.bn1 = nn.BatchNorm1d(num_features=fc2_units)
self.fc3 = nn.Linear(fc2_units, action_size)
```
### Critic
The critic passes the environment state vector through three fully connected layers and adds the action vector on the second layer. 
The output of the first two layers are subject to batch normalization:
```
self.fcs1 = nn.Linear(state_size, fcs1_units)
self.bn1 = nn.BatchNorm1d(num_features=fcs1_units)
self.fc2 = nn.Linear(fcs1_units+action_size, fc2_units)
self.bn2 = nn.BatchNorm1d(num_features=fcs2_units)
self.fc3 = nn.Linear(fc2_units, 1)
```

### Collaborative vs. non-collaborative gameplay
Different approaches for rewarding agents were examined, including rewarding each agent only for its own points, or rewarding each agent for the maximum score of either agent or the sum of both agents' score. 


## Evaluation
Best results were achieved when rewarding each agent with the sum of scores of both agents. The target average of .5 over the last 100 episodes was reached after 686 episodes of training.


<img src="https://github.com/CSiler/udacity-tennis-ddpg/assets/6819661/d94aca11-3c57-4ed9-845b-c2cf608d0cdb" width="50%">

## Ideas for Improvements

DDPG seemed like a well fit algorithm to solve the Tennis environment as the action space is continuous. However it is noteworthy that DDPG as it is implemented here reverts to actions at the edges of the action space, as can be seen in the following chart representing the complete history of actions during training:

<img src="https://github.com/CSiler/udacity-tennis-ddpg/assets/6819661/e129d76f-c684-4fa5-8efe-40e5e47b3152" width="50%">

If the environment can be solved without exhausting the continuity of the action space, as the chart implies, then an algorithm for discrete action spaces might also be used successfully.

As such, DeepQ or AlphaZero might outperform DDPG in the exploration of the state space and could lead to a faster convergence of the Q-function. While DDPG uses noise to try out new actions at random, AlphaZero for example intelligently chooses new actions based on calculated potential reward. If the gameplay follows a pattern that has been encountered and successfully managed before, AlphaZero should be able to reproduce its previous successful move, while the noise in DDPG's action selection process will prevent it from doing so.
