---
title: Scenario visualization tool
permalink: /docs/minesim-overview/scenario-visualization-tool/
description: none
---

It is crucial for researchers to quickly iterate on a new planning algorithm and closely investigate the algorithm's performance. Therefore, MineSim provides two Scenario Visualization Tools: the 2D Visualizer and the 3D Visualizer.

### 2D Visualizer

The **2D Visualizer**, developed using the Python Matplotlib, renders top-down views of each frame of a single scenario based on the simulation log file. Researchers can easily add scenario information and other information for the algorithm to the API as needed, as follow.

e.g. Scenario ID:dapai_intersection_1_3_4. Planning Method: Sampling Planner using Predefined Maneuver Modes(SPPMM).
<p align="center">
  <strong>Gif 1: Scenario ID dapai_intersection_1_3_4, SPPMM Method</strong>
  <img style="display:block; width:100%; height:auto;"
    src="https://raw.githubusercontent.com/byChenZhifa/archive/main/minesim//gifs/web-dapai_intersection_1_3_4-sppmm.gif"
    alt="Example GIF">
</p>
            
 


### 3D Visualizer

The **3D Visualizer**, developed using ROS, renders 3D views of each frame in a scenario based on the simulation log file. It allows for an intuitive observation of vehicle interactions and longitudinal speed planning information for the ego vehicle, as follow.

*Note: Currently, the 3D Visualizer does not support the "static obstacle avoidance scenario test".*

e.g. Scenario ID:dapai_intersection_1_3_4. Planning Method: SPPMM. An example of a collision occurring.
<p align="center">
    <strong>Video 1: Scenario ID dapai_intersection_1_3_4, SPPMM Method</strong>
    <video style="display:block; width:100%; height:auto;" autoplay="autoplay" muted loop="loop" controls playsinline>
        <source
        src="https://raw.githubusercontent.com/byChenZhifa/archive/main/minesim/3D-dapai_intersection_1_3_4.mp4"
        type="video/mp4" />
</p>        

 
The 3D Visualizer, developed using ROS, renders 3D views of each frame in a scenario based on the simulation log file. see Code Repository: [MineSim-3DVisualTool](https://github.com/BUAA-TRANS-Mine-Group/MineSim-3DVisualTool)

