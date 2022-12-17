# Reinforcement-learning-for-variable-speed-limit-control

In this project, we propose an approach providing both macroscopic and microscopic control on mixed autonomy traffic for variable speed limits and CAVs using the multi-agent reinforcement learning model and Evolution Strategy. We introduce the graph convolution network with Graph Attention Mechanism (GATs) into Deep Q networks for vehicles' decision making and design the architecture of the VSL network using the Evolution Strategy to provide real-time speed limit. A dedicated reward function has been implemented to consider both the actions and speed limit. We conduct extensive experiments in the Bottleneck network in *Flow*, an open-source deep reinforcement learning framework. Experimental results show that our approach has demonstrated superior performance compared with other baselines in terms of several metrics like throughput, average speed, and safety.



The project hierarchy is as follows:

Project Hierarchy


-----BaseLine  

|  

|  

|------ only_dgn_no_RL.py        (**baseline model: IDM controller**)   

|  

|  

|------ only_gipps.py            (**baseline model: Gipps controller**)  

|  

|  

—----buffer.py                	 (**ReplayBuffer of Deep Q Learning**)  

|  

|  

—----config.py			         (**config file**)  

|  

|  

—----DGN_Env.py		             (**RL Environment config file**)  

|  

|  

—----DGN.py			             (**DGN network**)  

|  

|  

—----ES_VSL.py		             (**Evolution Strategy network**)  

|  

|  

—----ring_main_DGN-ES.py         (**DGN with Variable Speed Limit control**)  

|  

|  

—----ring_main_DGN.py	         (**DGN without Variable Speed Limit control**)  



### Dataset
We choose the “Cooperate control for variable speed limit for CAVs using MARL and Evolution Strategy ” as the topic of our final project. As is known that Reinforcement Learning(RL) concerns with agents interacting with the environment in order to maximize the accumulated reward and achieve optimal results. In this process, a labelled dataset is not needed since agents are able to adjust their actions according to the reward provided by the environment and learn to optimize their decision-making strategy based on the current knowledge. Therefore, we do not include any public datasets in our experiment.

### Experiment Setup

We conduct our experiment in Flow, an open-source deep reinforcement learning framework that supports mixed-autonomy control. Since the merging lane area can better evaluate the performance of cooperation and overall throughput, we use the Bottleneck network from the Flow environment. To simulate the fleet of vehicles, we use SUMO (Simulation of Urban Mobility), an open source multi-model traffic simulation package which can model large traffic systems. The horizon of each episode is 3000 and the total number of vehicles is 22.

### Controller
***Intelligent Driver Model (IDM)***. The intelligent driver model is a commonly used adaptive cruise control method for vehicles that automatically adjusts the acceleration based on distance and velocity information to maintain a safe distance from the leading vehicle. IDM is commonly used to model human-driving behavior in traffic simulators. In this experiment, we use IDM to control human driving cars. We consider adding 0.2 random noise to the action to model the uncertainty of human-driving behavior.

***RL Controller***. RL controller is a controller to receive the actions from your RL algorithms and to control the acceleration/deceleration of CAVs. It does not need any specific parameters.
SimLaneChange Controller. SimLaneChange Controller controls the lane change whenever the lanes become narrow. Both human-driving cars and RL cars use it to change lanes.

***Gipps model***. Gipps model is a mathematical model for describing car-following behaviour by motorists, which is extensively used in road traffic studies. It is of practical importance to power the UK Transport Research Laboratory highway simulation package SISTM. The model tries to get the velocity dynamics and calculate the appropriate acceleration based on speed limit and distance. We consider setting the internal speed limit of 10 and limiting the acceleration boundary between -3 and 3.

Besides, we should set up the car following the params. There are several speed modes such as “aggressive”, “all-checks”, etc. The “aggressive” means the car will follow the action's output regardless of safety while “all-checks” will change the actions and stop automatically to prevent accidents. In order to evaluate the results in an objective way, we set “all-checks” to human driving cars and “aggressive” to RL cars.

In order to better compare the results among several models, we tend to keep the initial observation of vehicles the same, that is keep the same position with the same initial velocity. Therefore, we set the “uniform” rather than “random” in the InitialConfig.
