# RL+LLM City Simulation System

This project implements city-scale agent behaviors for game engine environments using a hybrid RL+LLM approach to build realistic simulations. The goal is to be able to more easily have agents available for simulations for a wide range of purposes, especially in urban and transportation planning and supply chain logistics. I left off Unity development when it became prohibitively expensive, so this codebase may be generally useful for continued work in Unity, or more simplified implementations may be found in my [three-mlagents](https://lukehollis.github.io/three-mlagents/) library, based on the Unity-MLAgents library of similar name.



https://github.com/user-attachments/assets/d730fd1c-5b08-4699-ad8f-e1a0e8aa44e2

_Digital twin of the train system for the Bay Area Rapid Transit Link 21 team._

Individual characters can be controlled by RL policy, running inference in engine, and interpret and plan actions with a language model, then visualize their inner state in game.


## Instatiating Characters in Environments

You can generate a diverse population of characters representing your required demographic landscape based on realworld data inputs. The character creation process synthesizes data from multiple sources, including US Census Bureau demographics, Bureau of Labor Statistics employment data, and local transportation studies.

Each generated character includes contextual details including their personality, age, profession, education level, commute patterns, family relationships, backstory (generalized and with episodic highlights), and other information. 

![NPC Spawn](https://iiif.mused.com/bart_simulation_spawning_npcs.jpg/full/800,/0/default.jpg)

The NPC Spawner Tool provided is an Editor tool that easily allows you to customize your NPC creation process on your navmesh within a bounding box. 

The generation process begins by establishing family units, then populating them with individual characters whose attributes are derived from weighted distributions matching specified input demographics. Educational backgrounds influence personality assignments, while employment roles and industries are selected to mirror the region's actual workforce composition.

## Mode shift

The Mode Shift module simulates how individual agents make transportation choices in urban environments. Each agent uses a multi-factor decision model to choose between available transportation modes (car, public transit, walking, cycling) based on:

* Travel time and distance
* Cost considerations
* Weather conditions
* Individual agent preferences
* Time of day
* Current traffic conditions
* Transit service availability

https://github.com/user-attachments/assets/ae6daae7-9fb0-4321-ac94-e252f89cb40c

Example usage in the BART train system simulation https://github.com/lukehollis/bart-3d


### Implementation
Agents use a combination of:

* Reinforcement learning policy for immediate reactions to environment changes
* Slower language model reflection for longer-term transportation planning
* Historical behavior patterns that influence future choices



## SEIR Simulation

The SEIR (Susceptible, Exposed, Infectious, Recovered) module integrates epidemiological modeling with agent-based simulation to model disease spread in urban environments. This integration allows for realistic modeling of how transportation patterns and urban mobility affect disease transmission.

https://github.com/user-attachments/assets/38f2ad40-e490-4d00-9eb2-269f6b6e0596



## Cognitive Architecture

![cognitive_architecture](https://github.com/user-attachments/assets/b823d896-94b3-4d85-9550-dce7df5bfe28)

Inspired by the [CoALA Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427), the NPCs in the simulations build on a similar version of the language agents but only implement the language model for rational reflection on actions inferred from policy--so an impulse from the policy and then reflection from the language model. 

https://github.com/user-attachments/assets/c42531ca-64e2-4ea9-92a5-a44fab5b04c9



## Networked State Management System

Inspired by projects like Photon for Unity networked state management, the state management system orchestrates persistent offline networked states through interconnected components in Unity and the backend (Python/Django connected via websocket) that work together to create a cohesive game world.



#### Conversations
The conversation system tracks all character-player interactions by maintaining a detailed message history with timestamps. It supports different types of communication (SPATIAL, DIGITAL) and maintains visibility settings to control information flow between characters and players. Each conversation is tied to specific game sessions and users, allowing for contextual interactions that persist across sessions.

If time-period appropriate, characters have the option to use simulated digital means of communication that are a simplified version of our own. They can do 1-1 agent dialog, group communication, or public posting on "The Feed," similar to a social network. Other characters can choose to check the Feed as one of their behaviors and like/dialog with public posts. 


#### Memory System
Characters maintain memories through a hierarchical storage system with three priority levels: high, medium, and low. Currently in dialog, the most recent ten high priority memories, five medium priority memories, and last three background memories provide general context. Each memory record contains a description, location, timestamp, priority level (ranked separately with a language model), and associations with specific characters, users, and game sessions.


https://github.com/user-attachments/assets/68d52aa0-2de4-4895-8844-9ac24141851f


#### World State
The world state maintains comprehensive data about the game environment, including the time period, environmental conditions, and the relations and data of characters. The backend contains a basic represetnation of spatial information in the Unity 2d or 3d environments while managing the progression of quests and storylines. This creates a persistent environment for characters -- which responds to player and character actions between play sessions.



https://github.com/user-attachments/assets/6887b20e-4f93-4ed8-85f2-05cdb81efdd1

BART train system simulation at https://github.com/lukehollis/bart-3d



## Getting Started

### Prerequisites
- Python 3.10+
- Unity ML-Agents 
- PyTorch
- OpenAI Gym

### Installation
```bash
pip install -r requirements.txt
``` 

### Data Collection
```bash
python src/collect.py
```


# In Unity

## Getting Started
0. Add the Agentics package to your Unity project
0.a If working in 2D, you may get further with the 2D specific version in this repo
1. Add the `AgenticController` component to your character, and configure with CharacterController, NavMeshAgent, and any other components you need
2. Set an initial day plan and waypoints for the agent
3. Set up training configuration with the python directory in the root of this repo
4. Add example plans in Data directory and configure with NetworkingController [coming soon]


# Citations

```
@inproceedings{Park2023GenerativeAgents,  
author = {Park, Joon Sung and O'Brien, Joseph C. and Cai, Carrie J. and Morris, Meredith Ringel and Liang, Percy and Bernstein, Michael S.},  
title = {Generative Agents: Interactive Simulacra of Human Behavior},  
year = {2023},  
publisher = {Association for Computing Machinery},  
address = {New York, NY, USA},  
booktitle = {In the 36th Annual ACM Symposium on User Interface Software and Technology (UIST '23)},  
keywords = {Human-AI interaction, agents, generative AI, large language models},  
location = {San Francisco, CA, USA},  
series = {UIST '23}
}
```


```
@misc{sumers2023cognitive,
      title={Cognitive Architectures for Language Agents}, 
      author={Theodore Sumers and Shunyu Yao and Karthik Narasimhan and Thomas L. Griffiths},
      year={2023},
      eprint={2309.02427},
      archivePrefix={arXiv},
      primaryClass={cs.AI}
}
```

