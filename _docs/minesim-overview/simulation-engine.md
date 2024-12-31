---
title: Simulation engine
permalink: /docs/minesim-overview/simulation-engine/
description: none
---



The MineSim system is primarily designed for testing autonomous driving planning tasks. The Simulation Engine serves as the core of MineSim and includes the following components: Prediction Algorithms, Planning Algorithms, Ego Simulation, and Agents Simulation. 

#### **(1) Summary of Methods for Core Components in the Simulation Engine**

To clarify the scalability of each component in the dynamic and static obstacle avoidance tests, we have provided **Tables 2 and 3** as references. The details are outlined below:



<p align="center">
  <strong>Table 2: Core Component Configuration for Dynamic Obstacle Avoidance Test</strong>
</p>



| **Components**                                 | **Current  Methods**                                         | **Explanation**                                              |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Prediction  Algorithms                         | 1)  Perfect prediction;<br> 2)  Constant velocity and yaw rate kinematics prediction;<br>3)  Multimodal Trajectory Prediction model | It is recommended to use Method 1 and Method 2. The  network parameters for Method 3 currently cannot be modified. |
| Planning  Algorithms                           | 1)  Simple longitudinal planner based Intelligent Driving Model (IDM);<br>2)  Sampling Planner based on Predefined Maneuver Modes (SPPMM) | MineSim provides two benchmark algorithms.                   |
| Ego  simulation  (Ego  open-loop simulation)   | Perfect  track planned trajectory                            | /                                                            |
| Ego  simulation  (Ego  Closed-loop simulation) | Ego  Motion Controller:<br>  1)  LQR-based Controller;<br>   2)  iLQR-based Controller;<br>   3)  Pure Pursuit Controller | /                                                            |
|                                                | Ego  Update Model:<br>   1)  Kinematic Bicycle Model (KBM);<br>   2)  Kinematic Bicycle Model with Response Lag (KBM-wRL);<br>   3)  Kinematic Bicycle Model with Response Lag and Road Slope (KBM-wRLwRS) | /                                                            |
| Agents simulation   (non-reactive agents)      | Replay Policy                                                | /                                                            |
| Agents simulation  (reactive agents)           | 1)  IDM-based Reactive Policy;<br>  2) Improved IDM-based  Reactive Policy;<br>  3)  Multimodal Trajectory Prediction-based Reactive Policy; | /                                                            |



<p align="center">
  <strong>Table 3: Core Component Configuration for Static Obstacle Avoidance Test</strong>
</p>


| **Components**                                 | **Current  Methods**                                         | **Explanation**                            |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------ |
| Prediction  Algorithms                         | /                                                            | /                                          |
| Planning  Algorithms                           | 1)  Simple longitudinal planner based Intelligent Driving Model (IDM);<br> 2)  Sampling Planner based on Predefined Maneuver Modes (SPPMM) | MineSim provides two benchmark algorithms. |
| Ego  simulation  (Ego  open-loop simulation)   | Perfect  track planned trajectory                            | /                                          |
| Ego  simulation  (Ego  Closed-loop simulation) | Ego  Motion Controller: <br>1)  LQR-based Controller; <br>2)  iLQR-based Controller; <br>3)  Pure Pursuit Controller | /                                          |
|                                                | Ego  Update Model: <br>1)  Kinematic Bicycle Model (KBM); <br>2)  Kinematic Bicycle Model with Response Lag (KBM-wRL);<br>3)  Kinematic Bicycle Model with Response Lag and Road Slope (KBM-wRLwRS) | /                                          |
| Agents simulation   (non-reactive agents)      | Replay Policy                                                | /                                          |
| Agents simulation  (reactive agents)           | 1)  IDM-based Reactive Policy; <br>2) Improved IDM-based  Reactive Policy; <br>3)  Multimodal Trajectory Prediction-based Reactive Policy; | /                                          |



#### **(2) Detailed Description of Methods for Core Components in the Simulation Engine**

##### **Prediction Algorithms:**

For the dynamic obstacle avoidance test, the Prediction Algorithms component is essential. We provide three prediction methods: 

- **Perfect Prediction**: This method analyzes the state information of the vehicle recorded in the scene file over a fixed time horizon and directly sends the predicted trajectory to the downstream planning module.
- **Constant Velocity and Yaw Rate Kinematics Prediction**: This physics-based prediction algorithm has a lower prediction accuracy compared to the others.
- **Multimodal Trajectory Prediction Model**: This method utilizes a lightweight Multimodal Trajectory Prediction model based on a CNN backbone (Li et al., 2024). However, since our “open-pit mining scenario motion prediction dataset” has not yet been released, the network parameters for this model cannot currently be retrained or modified, but it can still be used directly."



##### **Planning Algorithms:** 

The **Planning Algorithms** component is the primary testing task supported by the MineSim system. Currently, two benchmark algorithms are provided for both dynamic and static obstacle avoidance tests, as detailed in Sections 4.3.1 and 5.3.1 of the manuscript. Users can modify parameters based on the existing planning algorithms and are encouraged to develop additional planning algorithms suitable for unstructured open-pit mining road scenarios.

 

##### **Ego Simulation:**

The **Ego Simulation** component receives the results from the planning algorithms and updates the ego vehicle's state. This component supports both **open-loop** and **closed-loop simulation** modes:

-  **Open-loop Simulation Mode**: In this mode, the vehicle's next-step states are updated directly according to the trajectory output from the planning algorithm (referred to as "Perfect Track Planned Trajectory").

- **Closed-loop Simulation Mode**: This mode involves two stages of control: the **Ego Motion Controller** and the **Ego Update Model**, which work together to update the ego vehicle's state. This approach is common in other autonomous driving simulations, such as NuPlan (Caesar et al., 2022), ScenarioNet, and CommonRoad (Althoff et al., 2017).

  - The **Ego Motion Controller** tracks the desired trajectory from the planning algorithm. We provide three vehicle controllers **1) LQR-based Controller; 2) iLQR-based Controller; 3) Pure Pursuit Controller.** These controllers allow users to modify parameters and add their own motion control algorithms.

  - The **Ego Update Model** simulates real-world features like nonlinearity, response lag, inertia, and road slopes in heavy mining vehicles. We currently provide three vehicle state update models: **1) Kinematic Bicycle Model (KBM); 2) Kinematic Bicycle Model with Response Lag (KBM-wRL); 3) Kinematic Bicycle Model with Response Lag and Road Slope (KBM-wRLwRS).** Users can select one of these models, but we do not recommend modifying parameters or adding new models, as the choice of update model significantly impacts the control algorithms. Since MineSim's primary focus is to test planning algorithms, the choice of the update model does not significantly affect the planning tests.



##### **Agents simulation:**

**Agents Simulation** Also known as the **Agent Update Policy**. The Agents Simulation component is a necessary component for the dynamic obstacle avoidance test, primarily used to simulate the interactive behavior of other vehicles, ensuring thorough testing of the obstacle avoidance planning algorithm in mixed-traffic scenarios. In open-pit mines, other traffic agents primarily refer to obstacle vehicles, and MineSim provides a Replay Policy and three types of Reactive Policies. 

This component supports both **non-reactive agents** and **reactive agents** modes:

- non-reactive agents: Supports a **Replay Policy**. This policy involves replay testing based on logs recorded from real-world scenarios, which are replayed step-by-step according to the simulation cycle.

- reactive agents: The Reactive Policy addresses how multiple agents in a scenario can achieve realistic and reasonable state updates, making it an essential research focus in the field of scenario generation, with some challenges emerging at the research forefront. MineSim provides three types of Reactive Policies. 
  - **1) IDM-based Reactive Policy**. This rule-based longitudinal Intelligent Driving Model (IDM) is typically used for traffic flow simulation. It enables behaviors such as cruising, following, and emergency stopping along a predefined path. Additionally, various driving styles, i.e. "conservative," "moderate," and "aggressive" can be configured by adjusting IDM parameters. The IDM in MineSim is designed based on the model described in the reference (Treiber et al., 2000).
  > Treiber, M., Hennecke, A., Helbing, D., 2000. Congested traffic states in empirical observations and microscopic simulations. Phys. Rev. E 62, 1805–1824. https://doi.org/10.1103/PhysRevE.62.1805
  
    
  $$
  s^*(v_{\mathrm{agent}},v_{\mathrm{lead}})=s_0+v_{\mathrm{agent}}\cdot T+\frac{v_{\mathrm{agent}}\cdot (v_{\mathrm{agent}}-v_{\mathrm{lead}})}{2\cdot \sqrt{a\cdot b}}
    \\
    \dot{x}_{lon}=v_{\mathrm{agent}}
    \\
    \dot{v}_{\mathrm{agent}}={\dot{v}^{\mathrm{free}}}_{\mathrm{agent}}+{\dot{v}^{\mathrm{interact}}}_{\mathrm{agent}}=a_{\mathrm{agent}}\left( 1-\left( \frac{v_{\mathrm{agent}}}{v_0} \right) ^{\delta} \right) -a_{\mathrm{agent}}\left( \frac{s^*}{s_{\alpha}} \right) ^2
  $$
  

  where $v_0$ is the desired velocity the vehicle would drive at in free-flowing traffic, $s_0$ is the minimum safety distance if another vehicle is present ahead, and *T*  is the desired time headway in such situations.  $a$ represents the maximum vehicle acceleration, and  $b$ is the comfortable braking deceleration. The acceleration exponent, $\delta$, is typically set to 4. The agent's acceleration can be divided into a free-road term and an interaction term. The free-road term governs the vehicle's acceleration on open roads, while the interaction term adjusts the vehicle's behavior based on the distance and speed difference relative to the vehicle ahead.
  
  
  
  - **2) Improved IDM-based Reactive Policy**. In MineSim, we have improved the input strategy of the IDM model. Since the roads in open-pit mines lack clear lane markings, vehicles do not necessarily follow the reference path, which exists only on our map. Therefore, when determining the corresponding lead vehicle for each agent, the logic must consider the predicted information of other agents. The Constant Velocity Constant Yaw Rate (CVCYR) prediction method is used to predict the future states of other agents, projecting these states onto the agent’s matched path. The agent’s state is then updated according to the IDM model.
  
    
  
  - **3) Multimodal Trajectory Prediction-based Reactive Policy.** It is worth noting that another area of focus in scenario generation involves Neural Network-based state update models for single-agent or multi-agent systems. These models are typically scenario-centric and use trajectory prediction networks to simultaneously update the states of individual agents or multiple agents. They enable the simulation of more diverse and realistic behaviors, facilitating the creation of more generalized and authentic closed-loop traffic scenarios. This is a key advantage over traditional IDM-based methods, which primarily focus on longitudinal behavior and are unable to capture the full range of agent interactions. However, these trajectory prediction models have notable drawbacks, particularly in real-time simulations involving a large number of agents. They require significant computational resources and may occasionally produce rare or difficult-to-interpret simulation results, an issue that warrants further investigation and improvement. Despite these challenges, such models are crucial for the training and testing of Reinforcement Learning or End-to-End autonomous driving algorithms. In MineSim, we provide a lightweight Multimodal Trajectory Prediction model (Li et al., 2024) to update single-agent states, allowing for more diverse and realistic lateral and longitudinal state updates in mining scenarios. As illustrated in Fig. 6, the architecture of the prediction model uses the target agent's historical 3-second trajectory and renders its shape contour onto a mask raster map. A CNN-based backbone network then encodes the rendered images to extract features, and the model generates 𝑘 possible trajectories, each with associated probability scores. Further details on the model can be found in the reference (Li et al., 2024).
  > Li, L., Chen, Z., Wang, J., Zhou, B., Yu, G., Chen, X., 2024. Multimodal trajectory prediction for autonomous driving on unstructured roads using deep convolutional network. https://doi.org/10.48550/arXiv.2409.18399  
    
    
    <p align="center">
        <img src="../../../assets/img/F6-agent-states-update.png" alt="agent states update" width="1000">
    </p>
  
  

#### **(3) Recommended Test Configurations for Simulation Engine**

We recommend testing the newly proposed planning algorithms using the following configurations:

|        | Test Mode                                                    | **Applicable Scenario** | **Explanation**                                              |
| ------ | ------------------------------------------------------------ | ----------------------- | ------------------------------------------------------------ |
| Mode 1 | **Replay Test Mode**  (Ego Closed-loop with Non-reactive  Agents) | Dynamic                 | - Prediction Algorithms: Perfect Prediction;<br>- Ego Closed-loop Simulation:  LQR-based controller + KBM-wRL Ego Update Model;<br>- Agents Simulation: Replay Policy. |
| Mode 2 | **Interactive**  **Test**  **Mode**   **(**Ego Closed-loop with Reactive Agents) | Dynamic                 | - Prediction Algorithms: Constant Velocity and Yaw Rate Kinematics Prediction;<br>- Ego Closed-loop Simulation: LQR-based controller + KBM-wRL Ego Update Model;<br>- Agents Simulation: Improved IDM-based Reactive Policy. |
| Mode 3 | **Static Replay Test Mode**                                  | Static                  | - Ego Closed-loop Simulation: LQR-based controller + KBM-wRL Ego Update Model; |



