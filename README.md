# Solving Udacity's Reacher Challenge With DDPG Reinforcement Learning
## Project Details
This project uses the Reacher environment based on the Unity engine. The agent which is coded in this project controls a robot arm consisting of two joints. The goal is to keep the end of the arm inside a target sphere which circles around the base of the arm at varying speeds and direction. 

![reacher](https://github.com/CSiler/udacity-ddpg/assets/6819661/b3a14597-0c25-473d-96b7-4f62cc0c6565)

The action space therefore consists of four parameters controlling torque applied to each joint in the range -1 to +1. The environment responds with 33 state variables describing position, rotation, velocity and angular velocity of the arm.

The environment is considered solved when an average score of at least 30 points has been reached over 100 consecutive periods.

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
This project uses a version of the Reacher environment with one agent. To obtain the corresponding executable for Linux please click [here.](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux.zip)

Alternatively please [click here for a headless version.](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Linux_NoVis.zip)

If you are using a different operating system or would like to use the version with 20 agents instead of one, please consult [this GitHub repository](https://github.com/MathiasGruber/ReacherAgent-PyTorch).

To train the agent run the Jupyter notebook [training.ipynb](https://github.com/CSiler/udacity-ddpg/blob/main/training.ipynb) from this repository. Before you start, please make sure the link to the Reacher environment inside the notebook points to the right location on your drive.






