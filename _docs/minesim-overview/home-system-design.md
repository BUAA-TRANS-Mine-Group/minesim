---
title: System design
permalink: /docs/minesim-overview/home-system-design/
# Home 文件，会直接跳转到该处
# redirect_from: /docs/minesim-overview/home-system-design.html
description: none
---



<p align="center">
    <img src="../../../assets/img/logo_minesim_big.png" alt="Logo" width="500">
    <!-- <h1 align="center">Scenario-Based Simulator for Autonomous Truck Planning in Open-Pit Mines</h1> -->
</p>


**MineSim: Scenario-Based Simulator for Autonomous Truck Planning in Open-Pit Mines**

MineSim system offers a simulation testing environment for motion planning tasks in open-pit mining. It includes both dynamic and static scenario libraries, built from real-world driving data collected from mining sites. Users can evaluate proposed motion planning algorithms for autonomous mining trucks by testing them in numerous scenarios. 

### To Do

- [ ] Complete the repo code for [MineSim-Static](https://github.com/buaa-trans-mine-group/minesim-static).
- [ ] Complete the repo code for [MineSim-Dynamic](https://github.com/buaa-trans-mine-group/minesim-dynamic), 2025--01-05
- [x] Complete the project home info,  2024-11-20
- [x] Summit paper, 2024-10-16
- [x] Initial project home and repo, 2024-10-05



### System Architecture

Figure 1 illustrates the architecture of MineSim simulation testing system. This system standardizes the representation of mining truck driving scenarios and facilitates large-scale testing of planning algorithms. 

<p align="center">
    <img src="../../../assets/img/system-architecture-minesim.png" alt="Logo" width="1000">
</p>
MineSim is primarily designed for testing autonomous driving planning tasks. Specifically, MineSim currently includes two defined planning tasks: **dynamic obstacle avoidance planning** and **static obstacle avoidance planning**. However, other components are also essential for comprehensive autonomous driving testing, such as **prediction algorithm** for dynamic obstacle vehicles and **Motion controller** for the ego vehicle. Additionally, the **Metrics Evaluation** component is designed to be customizable and allows for modification and combination of various performance indicators.

### MineSim Components List

To elaborate, we have provided a more comprehensive list of components in the **MineSim** system that users can modify and extend, as shown in **Table 1**. 

<p align="center">
  <strong>Table 1: Components in MineSim that Support User Modification and Extension</strong>
</p>


| **Component**                                                | **Supports Parameter Modification** | **Supports Methods Extension** | **Explanation**                                              |
| :----------------------------------------------------------- | ----------------------------------- | ------------------------------ | ------------------------------------------------------------ |
| Simulation Engine: Environment Manager                       | No                                  | No                             | Loads and parses scenarios from the scenario library, and manages the entire simulation cycle. |
| Simulation Engine: Prediction  Aalgorithms                   | Yes                                 | Yes                            | Supports  extension of advanced learning-based prediction algorithms. |
| Simulation Engine: Planning  Aalgorithms                     | Yes                                 | Yes                            | Designed  primarily for testing autonomous driving planning tasks. |
| Simulation Engine: Ego  simulation                           | Yes                                 | Yes                            | Includes  Ego Motion Controller and Ego Update Model for closed-loop simulation. |
| Simulation Engine: Agents  simulation (Agents Update Policy) | Yes                                 | Yes                            | Used  to simulate interactive behavior of other vehicles, ensuring comprehensive  testing of obstacle avoidance algorithms. |
| Test Logger                                                  | Yes                                 | /                              | The Test Logger component is used to record simulation information at each simulation cycle, automatically storing it in a local folder. |
| Metrics Evaluation                                           | Yes                                 | Yes                            | Users can combine metrics like Safety, Efficiency,  Smoothness, and Task Completion according to task priorities. |
| Scenario Visualization                                       | /                                   | Yes                            | Supports user customization for adding relevant  information. |



###  MineSim Components Description

MineSim consists of three main components: the *Simulation Engine*, the *Metric* *Evaluation* component*,* and the *Scenario Visualization* tool. 

#### **(1) Simulation Engine:**

The *Simulation Engine* serves as the core of MineSim. It includes multiple components: *Environment Manager*，*Prediction Algorithm*, *Planning Algorithm*, *Motion Controller*, *Ego Update Model*, *Agent Update Policy*,  and *Test Logger*.

- **Environment Manager**: Loads and parses scenarios from the scenario library, and manages the entire simulation cycle, detecting the state of the simulation: start, normal operation, ego vehicle collision with road boundaries, ego vehicle collision with other agents, and task completion when the goal is reached.
- **Prediction Algorithms:** The Prediction Algorithms component is essential for dynamic obstacle avoidance testing. It is used to predict or directly read the future states of dynamic agents, and MineSim users can extend this component by incorporating more advanced learning-based prediction algorithms.
- **Planning Algorithms:** The Planning Algorithms component, which is described in detail in the manuscript, supports the primary testing tasks in MineSim. Users can modify parameters based on existing algorithms and are encouraged to develop new planning algorithms suitable for open-pit unstructured road scenarios. 
- **Ego Simulation:** The Ego Simulation component receives the output from the planning algorithms and performs the state updates for the ego vehicle.It includes the **Ego Motion Controller** and **Ego Update Model** in a closed-loop simulation mode. This component also supports parameter modifications and the extension of additional methods.
- **Agents Simulation** Also known as the **Agent Update Policy**. The Agents Simulation component is a necessary component for the dynamic obstacle avoidance test, primarily used to simulate the interactive behavior of other vehicles, ensuring thorough testing of the obstacle avoidance planning algorithm in mixed-traffic scenarios.
- **Test Logger：** The Test Logger component is used to record simulation information at each simulation cycle, automatically storing it in a local folder. It serves two main purposes: providing data for the Metric Evaluation component to perform performance assessments and supplying information to the Scenario Visualization tools for 2D and 3D visualization of the test scenarios.

#### **(2) Metric Evaluation:**

The Metric Evaluation component enables users to create a customized evaluation framework focused on specific performance after a set of scenario tests. It includes four main categories: safety, efficiency, smoothness, and task completion. Users can combine these metrics based on the specific performance priorities of their tasks and can also define additional performance indicators, such as **collision rates** across multiple scenarios.

#### **(3) Scenario Visualization:**

The Scenario Visualization tool offers both **2D views** and **3D views** of the test scenarios, aiding users in better understanding and demonstrating the performance of the proposed algorithms. It also supports users in adding other relevant information to the visualization interface. This component is fully open-source and easily modifiable.



### Simulation Engine Key Methods



#### **(1) Summary of Methods for Core Components in the Simulation Engine**

The MineSim system is primarily designed for testing autonomous driving planning tasks. The Simulation Engine serves as the core of MineSim and includes the following components: Prediction Algorithms, Planning Algorithms, Ego Simulation, and Agents Simulation. To clarify the scalability of each component in the dynamic and static obstacle avoidance tests, we have provided **Tables 2 and 3** as references. The details are outlined below:



<p align="center">
  <strong>Table 2: Core Component Configuration for Dynamic Obstacle Avoidance Test</strong>
</p>

| **Components**                                 | **Current  Methods**                                         | **Explanation**                                              |
| ---------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Prediction  Algorithms                         | 1)  Perfect prediction;<br>  2)  Constant velocity and yaw rate kinematics prediction;<br>  3)  Multimodal Trajectory Prediction model | It is recommended to use Method 1 and Method 2. The  network parameters for Method 3 currently cannot be modified. |
| Planning  Algorithms                           | 1)  Simple longitudinal planner based Intelligent Driving Model (IDM);<br>  2)  Sampling Planner based on Predefined Maneuver Modes (SPPMM) | MineSim provides two benchmark algorithms.                   |
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

##### Agents simulation

**Agents Simulation** Also known as the **Agent Update Policy**. The Agents Simulation component is a necessary component for the dynamic obstacle avoidance test, primarily used to simulate the interactive behavior of other vehicles, ensuring thorough testing of the obstacle avoidance planning algorithm in mixed-traffic scenarios. In open-pit mines, other traffic agents primarily refer to obstacle vehicles, and MineSim provides a Replay Policy and three types of Reactive Policies. 

This component supports both **non-reactive agents** and **reactive agents** modes:

- non-reactive agents: Supports a **Replay Policy**. This policy involves replay testing based on logs recorded from real-world scenarios, which are replayed step-by-step according to the simulation cycle.

- reactive agents: The Reactive Policy addresses how multiple agents in a scenario can achieve realistic and reasonable state updates, making it an essential research focus in the field of scenario generation, with some challenges emerging at the research forefront. MineSim provides three types of Reactive Policies. 
  - **1) IDM-based Reactive Policy**. This rule-based longitudinal Intelligent Driving Model (IDM) is typically used for traffic flow simulation. It enables behaviors such as cruising, following, and emergency stopping along a predefined path. Additionally, various driving styles, i.e. "conservative," "moderate," and "aggressive" can be configured by adjusting IDM parameters. The IDM in MineSim is designed based on the model described in the reference (Treiber et al., 2000).
  
    
  $$
  s^*(v_{\mathrm{agent}},v_{\mathrm{lead}})=s_0+v_{\mathrm{agent}}\cdot T+\frac{v_{\mathrm{agent}}\cdot (v_{\mathrm{agent}}-v_{\mathrm{lead}})}{2\cdot \sqrt{a\cdot b}}
    \\
    \dot{x}_{lon}=v_{\mathrm{agent}}
    \\
    \dot{v}_{\mathrm{agent}}={\dot{v}^{\mathrm{free}}}_{\mathrm{agent}}+{\dot{v}^{\mathrm{interact}}}_{\mathrm{agent}}=a_{\mathrm{agent}}\left( 1-\left( \frac{v_{\mathrm{agent}}}{v_0} \right) ^{\delta} \right) -a_{\mathrm{agent}}\left( \frac{s^*}{s_{\alpha}} \right) ^2
  $$
  

  where $v_0$ is the desired velocity the vehicle would drive at in free-flowing traffic, $s_0$ is the minimum safety distance if another vehicle is present ahead, and *T* is the desired time headway in such situations.  $a$ represents the maximum vehicle acceleration, and  $b$ is the comfortable braking deceleration. The acceleration exponent, $\delta$, is typically set to 4. The agent's acceleration can be divided into a free-road term and an interaction term. The free-road term governs the vehicle's acceleration on open roads, while the interaction term adjusts the vehicle's behavior based on the distance and speed difference relative to the vehicle ahead.
  
  
  
  - **2) Improved IDM-based Reactive Policy**. In MineSim, we have improved the input strategy of the IDM model. Since the roads in open-pit mines lack clear lane markings, vehicles do not necessarily follow the reference path, which exists only on our map. Therefore, when determining the corresponding lead vehicle for each agent, the logic must consider the predicted information of other agents. The Constant Velocity Constant Yaw Rate (CVCYR) prediction method is used to predict the future states of other agents, projecting these states onto the agent’s matched path. The agent’s state is then updated according to the IDM model.
  
    
  
  - **3) Multimodal Trajectory Prediction-based Reactive Policy.** It is worth noting that another area of focus in scenario generation involves Neural Network-based state update models for single-agent or multi-agent systems. These models are typically scenario-centric and use trajectory prediction networks to simultaneously update the states of individual agents or multiple agents. They enable the simulation of more diverse and realistic behaviors, facilitating the creation of more generalized and authentic closed-loop traffic scenarios. This is a key advantage over traditional IDM-based methods, which primarily focus on longitudinal behavior and are unable to capture the full range of agent interactions. However, these trajectory prediction models have notable drawbacks, particularly in real-time simulations involving a large number of agents. They require significant computational resources and may occasionally produce rare or difficult-to-interpret simulation results, an issue that warrants further investigation and improvement. Despite these challenges, such models are crucial for the training and testing of Reinforcement Learning or End-to-End autonomous driving algorithms. In MineSim, we provide a lightweight Multimodal Trajectory Prediction model (Li et al., 2024) to update single-agent states, allowing for more diverse and realistic lateral and longitudinal state updates in mining scenarios. As illustrated in Fig. 6, the architecture of the prediction model uses the target agent's historical 3-second trajectory and renders its shape contour onto a mask raster map. A CNN-based backbone network then encodes the rendered images to extract features, and the model generates 𝑘 possible trajectories, each with associated probability scores. Further details on the model can be found in the reference (Li et al., 2024).
    
    
    
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





### Data collection

todo

For the MineSim dataset we collect  ...  

*Note: Scenario data will continue to be expanded in the future.*





### [optional] Clone MineSim-Dynamic Repository







#### [optional] Clone MineSim-Static Repository





#### Download



