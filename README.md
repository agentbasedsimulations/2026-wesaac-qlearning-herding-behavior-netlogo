Q-Learning Sheep Herding in NetLogo

Agent-based simulation of autonomous herding using reinforcement learning — no pre-programmed heuristics, just spatial Q-Learning.
This repository contains two NetLogo simulations that apply Q-Learning to the classic sheep herding problem. A single dog agent learns to guide sheep toward a target gate by discovering spatial rules through trial and error, using a low-complexity discrete state space based on relative distance, angle, and alignment.

📁 Repository Structure
plain

├── Sheepherding-Simulation-Static.nlogo      # Single sheep, fixed spawn, stationary target  
│     
├── Sheepherding-Simulation-Dynamic.nlogo     # Multiple sheep, random spawn, moving targets   
│    
└── README.md  

🧠 What is Q-Learning Herding?

The herding problem is a classic benchmark in autonomous control and multi-agent systems. The challenge lies in indirectly manipulating a non-cooperative agent (the sheep) through a driving agent (the dog). The dog cannot simply walk to the gate—it must learn to position itself behind the sheep, aligned with the gate, at exactly the right distance to trigger the sheep's flight response in the desired direction.
Instead of hard-coding these geometric rules, the dog agent learns them autonomously by:
Observing a discretized spatial state (distance, angle, alignment)
Choosing from five discrete actions (move forward, turn left, turn right, orbit, tangent movement)
Receiving shaped rewards based on progress toward the gate
Updating a Q-table via the Bellman equation

📊 Simulations

1. Static Herding

Feature	Description  
Sheep	1, completely stationary  
Spawn	Fixed positions every episode  
Environment	Fully deterministic  
Convergence	Very fast — optimal policy learned in a few thousand episodes  

Best for: Understanding the core Q-Learning mechanics and verifying that the state-action abstraction works correctly before adding complexity.
What to notice: The steps-per-episode and reward plots flatline quickly. Once trained, the dog repeats the exact same efficient trajectory every time.

2. Dynamic Herding

Feature	Description  
Sheep	Multiple (configurable)  
Spawn	Random positions outside the corral every episode  
Environment	Stochastic — sheep move randomly and flee from the dog  
Convergence	Slower and noisier, but demonstrates robust generalization  

Best for: Evaluating policy stability under varying environmental complexity and testing whether the agent can adapt its strategy to new spatial configurations on the fly.
What to notice: The cumulative completed-episodes plot trends upward linearly despite randomness, confirming the Q-matrix adapts without catastrophic forgetting.

🎮 How to Use

Install NetLogo (version 6.x recommended).  
Open either .nlogo file.  
Click Setup to initialize the environment.  
Click Go to start training.  
Watch the plots to monitor learning progress.  

Interface Controls

Control	Function
Setup	Initialize agents and clear plots  
Go	Run continuous training episodes  
Go Once	Execute a single simulation tick  
Run Episode	Run one episode using the current policy  

Key Plots

Completed Episodes: Cumulative successes over total simulation ticks  
Average Steps (per 1,000 episodes): Measures path efficiency   
Average Reward (per 1,000 episodes): Measures policy quality   

⚙️ State, Action & Reward Design

State Space (2 × 8 × 4 = 64 states)  
Distance: near / far relative to the sheep's perception zone  
Angle: 8 sectors of 45° around the sheep  
Alignment: 4 discrete positions relative to the sheep-to-gate axis  
Action Space  
Move Forward  
Turn Left / Turn Right  
Orbit Trigger (circular path around the sheep)  
Tangent Movement (lateral repositioning)  

Reward Function

Event	Reward  
Sheep enters gate	+10  
Sheep gets closer to gate	+4  
Sheep moves away from gate	−0.5  
Dog too far from sheep (>7 patches)	−0.5  
Time penalty (every step)	−0.1  
Dog stuck / oscillating	−1  

🔬 Hyperparameters

Parameter	Value	Description  
Learning Rate (α)	0.4	Balance between new and past experience  
Discount Factor (γ)	0.7	Weight of long-term vs. immediate rewards  
Initial Epsilon (ε)	0.5	Initial exploration probability  
Epsilon Decay	0.9994	Gradual shift from exploration to exploitation  
These values were empirically tuned using NetLogo's BehaviorSpace.  

📈 Expected Results

Metric	Static	Dynamic

Steps trend	Rapid flatline at minimum	Gradual decline to stable baseline  
Reward trend	Rapid flatline near maximum	Stabilization after initial volatility  
Policy behavior	Identical trajectory every episode	Adaptive path per spawn configuration  

📚 References

This project is based on:
Applying Q-Learning to Herding Behavior: A Single-Agent Approach in NetLogo (2026)

Key sources:

Watkins, C. J. C. H., & Dayan, P. (1992). Q-learning. Machine Learning, 8(3–4), 279–292.  
Strömbom, D., et al. (2014). Solving the shepherding problem. Proceedings of the Royal Society B, 281(1787), 20140719  
Lien, J.-M., et al. (2004). Shepherding behaviors with multiple shepherds. IEEE International Conference on Robotics and Automation (ICRA).  
Macal, C. M., & North, M. J. (2010). Tutorial on agent-based modelling and simulation. Journal of Simulation, 4(3), 151–162.  

📄 License
This project is provided for academic and educational purposes. Please cite the original paper if you use or extend this work.
