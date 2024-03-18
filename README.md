# Keep the Tennis Ball in the Air with DDPG Reinforcement Learning
## Project Details
This project uses Udacity's Tennis environment which based on the Unity engine. Subject of this challenge is to control two agents playing a version of Tennis with each other in which the ball should never touch the net or the ground.


<img src="https://github.com/CSiler/udacity-tennis-ddpg/assets/6819661/2b428c50-306d-4b9f-905c-6ac572ed604e" width="50%" >


The action space is continuous and consists of two parameters assuming values from -1 to +1 representing thrust applied to the tennis racket of each agent in x- and y-directions. The environment responds with 24 state variables, representing three consequetive time frames whith 8 variables each, including velocity and location of ball and racket.

An agent receives a reward of .1 when it plays the ball across the net and a reward of -.1 when it lets the ball drop to the ground in its field. 

In order to solve the environment, episodes are counted with the maximum of each player's points. The average of that maximum score must reach .5 over the last 100 episodes.

## Getting Started

Create a new conda environment via
```
conda create -n mlagents python=3.10.12 && conda activate mlagents
```
Run these commands to install Unity requirements. 
```
git clone --branch release_21 https://github.com/Unity-Technologies/ml-agents.git
cd ml-agents
python -m pip install ./ml-agents-envs
python -m pip install ./ml-agents
python -m pip install mlagents_envs==1.0.0
```
Download the [requirements.txt](https://github.com/CSiler/udacity-ddpg/blob/main/requirements.txt) from this repository and install as follows:
```
python -m pip install -r requirements.txt
```



## Instructions
This project uses a version of the Tennis environment with two agents. To obtain the corresponding executable 
- Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
- Mac OSX: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip)
- Windows (32-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86.zip)
- Windows (64-bit): [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Windows_x86_64.zip)

For a headless version for Linux, [click here.](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux_NoVis.zip)
