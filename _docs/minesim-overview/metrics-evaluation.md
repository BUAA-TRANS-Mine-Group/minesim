---
title: Metrics evaluation
permalink: /docs/minesim-overview/metrics-evaluation/
description: none
---

### TODO：

- [x] Complete Metrics Evaluation System Introduction；
- [ ] Add Metrics Evaluation tool code and Tutorial；



### 1. Metrics Evaluation System

To comprehensively evaluate the planning algorithm's performance across various scenarios, our scoring system assesses four key categories of metrics: safety, efficiency, smoothness, and task completion. These metrics are aggregated to produce an overall score for the proposed algorithm, providing a holistic assessment of its capabilities.

#### Safety Metrics:

This metric evaluates the vehicle's ability to avoid collisions with obstacles and road boundaries. Given the irregular characteristics of unstructured roads, the vehicle must maintain a sufficient safety margin from the boundaries. Specifically, the lateral distance between the ego vehicle and the road boundary must exceed a predefined threshold to ensure safety throughout the scenario.

#### Efficiency Metrics: 
Efficiency is measured by how quickly the autonomous vehicle reaches the target area, provided it completes the scenario safely. Faster completion times indicate better performance.

#### Smoothness Metrics: 
Unlike passenger vehicles, where algorithms focus on optimizing ride comfort based on passengers' subjective experience, the primary goal for mining vehicles is driving smoothness.  This metric is assessed by calculating the minimum and maximum longitudinal and lateral accelerations, as well as centripetal acceleration during turns to evaluate stability. Smoothness is crucial for ensuring stability and safety in challenging mining environments.

#### Task Completion: 
 Task completion  is evaluated for both scenarios in which the vehicle reaches the target area and those in which it does not. If the target is not reached, the completion score is determined by the vehicle’s progress along the route, specifically by comparing the final driving distance to the total route distance. If the vehicle reaches the target, the quality of task completion is also assessed by evaluating the final yaw deviation, which measures the vehicle's alignment with the road at the goal based on the yaw difference from the target orientation.

*Note："Dynamic Obstacle Avoidance Scenario Testing"and "Dynamic Obstacle Avoidance Scenario Testing" is different.*



#### Other Metrics:

When testing the performance of the provided algorithm across a wide range of scenarios, key metrics like task completion rate and collision rate can also serve as important indicators, depending on the metrics prioritized.



#### Dynamic Obstacle Avoidance Scenario Testing

<p align="center">
  <strong>Table 1 Metric Design for Dynamic Obstacle Avoidance Scenario Testing.</strong>
</p>

| Metric Categories           |          **MetricName**           |           **Metric Score Calculation Description**           | **Points** |
| --------------------------- | :-------------------------------: | :----------------------------------------------------------: | :--------: |
| Safety (40 points)          |    Collisions  with Obstacles     |      Penalty:  Safety score is 0 if a collision occurs.      |  0 or 40   |
|                             | Collisions  with Road  Boundaries |      Penalty:  Safety score is 0 if a collision occurs.      |  0 or 40   |
|                             |    Lateral Safety  (Roadside)     | Penalty:  Deduct points based on the duration below the lateral safety distance  threshold over total driving time. |    0-40    |
| Efficiency (10 points)      |         Driving Duration          | If the safety score is  0, this score is also 0; otherwise, score is calculated as min (1.0, total  driving time / expected time) * 10. |    0-10    |
| Smoothness (30 points)      |              Lateral              | Penalty: min  (1.0, duration exceeding lateral acceleration threshold / total driving  time) * 10. |    0-10    |
|                             |           Longitudinal            | Penalty: min  (1.0, duration exceeding longitudinal acceleration threshold/total driving  time) * 10. |    0-10    |
|                             |              Turning              | Penalty: min  (1.0, duration exceeding centripetal acceleration threshold/total driving  time) * 10. |    0-10    |
| Task Completion (20 points) |    Progress  along Route  Path    | If the ego  vehicle reaches the goal, the score is 20; otherwise,  it is calculated as min (1.0, final driving distance along the  route path / overall route path distance) * 20. |    0-20    |
|                             |       Final Yaw  Deviation        | Penalty: If  the ego vehicle does not reach the goal, score is 0; otherwise, score is min  [1.0, cos (final yaw deviation/0.5*pi)] * 10. |    0-10    |


#### Static Obstacle Avoidance Scenario Testing
The metrics design for the **static scenario planning task** is shown in Table 3 and differs slightly from the Task completion metrics used for the **dynamic scenario planning task**.

<p align="center">
  <strong>Table 3: Metric Design for Static Obstacle Avoidance Scenario Testing. </strong>
</p>

| Metric Categories           |          **MetricName**           |           **Metric Score Calculation Description**           | **Points** |
| --------------------------- | :-------------------------------: | :----------------------------------------------------------: | :--------: |
| Safety (40 points)          |    Collisions  with Obstacles     |      Penalty:  Safety score is 0 if a collision occurs.      |  0 or 40   |
|                             | Collisions  with Road  Boundaries |      Penalty:  Safety score is 0 if a collision occurs.      |  0 or 40   |
|                             |    Lateral Safety  (Roadside)     | Penalty:  Deduct points based on the duration below the lateral safety distance  threshold over total driving time. |    0-40    |
| Efficiency (10 points)      |         Driving Duration          | If the safety score is  0, this score is also 0; otherwise, score is calculated as min (1.0, total  driving time / expected time) * 10. |    0-10    |
| Smoothness (30 points)      |              Lateral              | Penalty: min  (1.0, duration exceeding lateral acceleration threshold / total driving  time) * 10. |    0-10    |
|                             |           Longitudinal            | Penalty: min  (1.0, duration exceeding longitudinal acceleration threshold/total driving  time) * 10. |    0-10    |
|                             |              Turning              | Penalty: min  (1.0, duration exceeding centripetal acceleration threshold/total driving  time) * 10. |    0-10    |
| Task Completion (20 points) |    Progress  along Route  Path    | If the ego  vehicle reaches the goal, the score is 20; otherwise,  it is calculated as min (1.0, final driving distance along the  route path / overall route path distance) * 20. |    0-20    |
|                             |       Final Yaw  Deviation        | Penalty: If  the ego vehicle does not reach the goal, score is 0; otherwise, score is min  [1.0, cos (final yaw deviation/0.5*pi)] * 10. |    0-10    |









### 2. Metrics Evaluation Tool

TODO ......


