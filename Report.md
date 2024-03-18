
# Reacher Game Report
## Learning Algorithm
### DDPG basics
This project's approach to solving the Reacher environment is based on the Deep Deterministic Policy Gradient (DDPG) algorithm as described in [this paper.](https://arxiv.org/pdf/1509.02971.pdf)

DDPG uses an actor-critic network with the actor producing action values and the critic evaluating these actions. 

In order to avoid negative feedback preventing convergence toward optimal parameter values, DDPG uses the Double Q-network concept where a target 
and a local copy of the network are maintained. The local copy of the actor is used to produce the stream of actions the agent uses to navigate the 
environment while the target copy is used to determine a hypothetical next action to be evaluated by the critic. 

The Q-value the critic assigns to the current state and such an action serves as the surrogate to be used for training the actor network with gradient ascent.

In order to improve and train the critic network, a target copy of the critic is used to produce the future (t+1) Q-value which is discounted and added to the observed 
reward. This serves as the benchmark for a Q-value estimated by the local copy of the critic. The difference defines the loss to update the local copy of the critic 
with gradient descent.

Target parameter values for both actor and critic networks are softly updated over time with their local copies.

### Replay Buffer
DDPG uses a replay buffer that collects a history of observations including state, action, reward and next state. When training the network, 
batches of observations are randomly sampled from the buffered history.

### Noise
DDPG adds noise to the actor's output to allow for exploration. An auto-correlated noise function as defined by Uhlenbeck & Ornstein is used.

## Hyper-parameters
Environment state vectors are scaled by a factor 1/10 to lie within a range of -1.5 to 1.5 before they are fed into any actor or critic network.

Further hyper-parameters used:
```
BUFFER_SIZE = int(1e5)  # replay buffer size
BATCH_SIZE = 256        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 5e-3              # for soft update of target parameters
LR_ACTOR = 1e-4         # learning rate of the actor 
LR_CRITIC = 1e-4        # learning rate of the critic
WEIGHT_DECAY = 0        # L2 weight decay
```

Uhlenbeck & Ornstein noise:
```
theta = 0.03
sigma = 0.02
```
Actor and critic networks:
```
Fully connected layer nodes: 512
```
## Model Architecture
### Actor
The actor networks use three fully connected layers, with the first layer's output subject to batch normalization:
```
self.fc1 = nn.Linear(state_size, fc1_units)
self.bn1 = nn.BatchNorm1d(num_features=fc1_units)
self.fc2 = nn.Linear(fc1_units, fc2_units)
self.fc3 = nn.Linear(fc2_units, action_size)
```
### Critic
The critic passes the environment state vector through three fully connected layers and adds the action vector on the second layer. 
The output of the first layer is subject to batch normalization:
```
self.fcs1 = nn.Linear(state_size, fcs1_units)
self.bn1 = nn.BatchNorm1d(num_features=fcs1_units)
self.fc2 = nn.Linear(fcs1_units+action_size, fc2_units)
self.fc3 = nn.Linear(fc2_units, 1)
```
## Evaluation
The training agent managed to receive an average reward of 30 over the previous 100 episodes after 139 evaluations, i.e. 239 training cycles.
Training took 15 min on an Nvidia RTX 4080.

![image](https://github.com/CSiler/udacity-ddpg/assets/6819661/8bf6172a-4cb7-4099-85fc-37055d413946)



## Ideas for Improvements
Further tuning of hyper-parameters will likely reduce training time.

Prioritizing rarely seen state-action pairs during training would avoid inherent bias from unequally distributed observations.

A technical training speedup might result from using the 20 agents version of the Reacher environment and suitable algorithms, e.g. TD3 or A3C.
