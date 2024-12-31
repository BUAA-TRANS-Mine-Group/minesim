---
title: Scenario task description in MineSim
permalink: /docs/scenario-description/scenario-task-description/
description: none
---


### Unified Scenario Description
The data sources of test scenarios fall into two categories: scenarios converted from real-world logs and virtually generated scenarios. A unified scenario description is essential for consistent storage. As shown in Fig. 2, we define the unified scenario description format as a JSON file. A complete scenario description consists of three main parts: 1) The descriptions of static and dynamic obstacles within the scenario; 2) The setup of the scenario's planning problem; 3) The High-Definition (HD) Map of the corresponding area for the scenario.

<p align="center">
    <img src="../../../assets/img/system-architecture-minesim-2.png" alt="Logo" width="1000">
</p>
<p align="center">
    <strong>JSON file structure for each scenario.</strong>
</p>

Unlike the complex and open urban roads filled with various cars, bicycles, and pedestrians, the primary traffic participants in open-pit mines are autonomous mining trucks and various static and dynamic obstacles. Following common practices from open-source autonomous driving datasets CommonRoad(Althoff et al., 2017), NuPlan(Caesar et al., 2022), and  Waymo-Open-Motion-Dataset(Ettinger et al., 2021), traffic participants are stored in a time-frame-based format. The “Dynamic obstacles” field contain complete trajectory information, and any frame drop issues are addressed during the data processing stage. The “Static obstacles” field stores details such as obstacle type (e.g., parked vehicles, rocks, construction areas, unknown), bounding box coordinates, and whether the obstacle is virtually generated. The ego vehicle's parameter information is stored in the “planning problem” field. In addition to ego parameters, this field also includes the ego vehicle's start state, goal state, and the maximum allowed runtime for the test task. Based on the scenario descriptions above, MineSim supports automated testing for various planning tasks of autonomous mining vehicles in open-pit mines. Two clearly defined scenario libraries are provided:
(1) Scenario Library 1: This library focuses on testing obstacle avoidance in mixed traffic at mining intersections. Its scenarios feature unstructured intersections with varying slopes and irregular shapes, where autonomous mining trucks interact with other vehicles. The main goal is to assess the trucks' smooth obstacle avoidance.  
(2) Scenario Library 2: This library is designed to test obstacle avoidance with static obstacles on mining roads. It records scenarios where the trucks face static obstacles of various sizes on roads with different curves and widths. The focus is on assessing the ability of autonomous mining trucks to plan smooth, efficient trajectories.  

> Althoff, M., Koschi, M., Manzinger, S., 2017. CommonRoad: Composable Benchmarks for Motion Planning on Roads, in: 2017 IEEE Intelligent Vehicles Symposium. Presented at the 2017 IEEE Intelligent Vehicles Symposium, IEEE, Los Angeles, CA, USA. https://doi.org/10.1109/IVS.2017.7995802
> Caesar, H., Kabzan, J., Tan, K.S., Fong, W.K., Wolff, E., Lang, A., Fletcher, L., Beijbom, O., Omari, S., 2022. NuPlan: A closed-loop ML-based planning benchmark for autonomous vehicles. https://doi.org/10.48550/arXiv.2106.11810
> Ettinger, S., Cheng, S., Caine, B., Liu, C., Zhao, H., Pradhan, S., Chai, Y., Sapp, B., Qi, C.R., Zhou, Y., Yang, Z., Chouard, A., Sun, P., Ngiam, J., Vasudevan, V., McCauley, A., Shlens, J., Anguelov, D., 2021. Large Scale Interactive Motion Forecasting for Autonomous Driving: The Waymo Open Motion Dataset. Presented at the Proceedings of the IEEE/CVF International Conference on Computer Vision, pp. 9710–9719.


### Scenario 每个字段含义

todo ......

