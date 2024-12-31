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
    <img src="../../../assets/img/system-architecture-minesim-2.png" alt="Logo" width="1000">
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














