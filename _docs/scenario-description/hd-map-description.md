---
title: HD map description
permalink: /docs/scenario-description/hd-map-description/
description: none
---

### HD Map Descriptions in Open-pit Mines

The critical differences between unstructured roads in open-pit mines (akin to off-road terrain) and structured roads such as urban highways are the lack of clear lane markings, irregular road boundaries, and uneven road widths. Additionally, these roads often feature steep slopes and frequent gradient changes. As industrial environments, mines have constantly changed operational areas, such as loading and unloading zones, where drivable areas shift frequently. As shown in Fig. 3, these are typical mining roads provided in MineSim. Drawing on an open-source map format (Holger Caesar et al., 2020), we designed an HD map to accurately represent these characteristics of unstructured roads in open-pit mines. The HD map consists of two components: a raster map and a semantic map. The scenario description file in Fig. 2 contains a "link_map" field that stores the map information covering the entire area. The Simulation Engine automatically indexes the corresponding HD map file, which is described in a separate JSON file.


<p align="center">
  <img src="../../../assets/img/F3-abc-jinagtong.png" alt="Logo" width="1000">
</p>
<p align="center">
  <strong>Fig. 3. Real open-pit HD map of unstructured roads in MineSim. (a-c) Mines snapshot, orthophoto image, and raster map at Jiangtong open-pit, Jiangxi Province, China.</strong>
</p>

<p align="center">
  <img src="../../../assets/img/F3-def-dapai.png" alt="Logo" width="1000">
</p>
<p align="center">
  <strong>Fig. 3. Real open-pit HD map of unstructured roads in MineSim. (d-f) Mines snapshot, orthophoto image, and raster map at Dapai open-pit, Guangdong Province, China.</strong>
</p>

The HD map consists of two components: a raster map and a semantic map.

#### (1) Raster Map

The high-resolution mask map data is used to represent the drivable areas of open-pit mining roads. This format preserves the maximum detail for all drivable areas within the entire area, as illustrated in Fig. 3 (c) and (f). In the raster map, each pixel contains a value of either 0 or 1, indicating non-drivable and drivable areas, respectively. The pixel coordinates are precisely mapped to absolute geographical coordinates, with a scale of 10 pixels per meter.

#### (2) Semantic Map

The high-precision semantic map consists of geometric and semantic layers within the entire area, with precise location data for each layer. The geometric layers include "*node*," "*node_block*," and "*polygon*," recording various geometric elements like points and surfaces that compose the semantic map. The semantic layers represented by geometric polygons include "*road*," "*intersection*," "*loading_area*," "*unloading_area*," and "*road_block*." These layers divide drivable areas of the entire mines into smaller regions such as road segments, intersections, loading areas, and unloading areas, each associated with different potential traffic conflicts. The *semantic layers* represented by geometric lines include "*dubins_pose*," "*reference_path*," and "*borderline*." The *geometric* and *semantic* *layers* are managed by a unique identifier called a "*token*", ensuring the uniqueness of each element within the map system. The meanings of the fields and additional details are as follows:

- "*node*" or "*node_block*": A geometric layer that records geometric point elements composing various semantic layers on the map.
- “*polygon*”: A geometric layer that records the geometric polygon elements that form various semantic layers on the map. It also stores association information with other semantic layers such as "*reference_path*," "*borderline*," and "*dubins_pose*."
- “*road*”: A semantic layer that records the semantic segmentation of all road segments in a specific area.
- “*intersection*”: A semantic layer that records the semantic segmentation of all intersections (or junctions) in a specific area.
- “*loading_area*”: A semantic layer that records the semantic segmentation of all loading areas (used for loading ore onto ming trucks) in a specific area.
- “*unloading_area*”: A semantic layer that records the semantic segmentation of all unloading areas (used for unloading ore from mining trucks) in a specific area.
- “*road_block*”: A semantic layer that records the semantic segmentation of all road block areas in a specific region.
- “*dubins_pose*”: A semantic layer used as control points for generating reference paths. It includes fields such as "*dubinspose_type*" and "*link_polygon_token*." The "*dubinspose_type*" records the control point types (as shown in Fig. 21a) : 
  - "*normal*": A standard control point with one upstream and one downstream path; 
  - "*split*": One upstream path and multiple downstream paths; 
  - "*merge*": Multiple upstream paths and one downstream path.

<p align="center">
  <img src="../../../assets/img/F21-a.png" alt="Logo" width="800">
</p>
<p align="center">
    <strong>Fig. 21a. Differences between types in the "dubins_pose," and "reference_path" layer.</strong>
</p>

- “*reference_path*”: A semantic layer that records reference path segments, generated using Dubins curves from dubins_pose points. It includes the following fields: related polygons ("*link_polygon_tokens*"), related dubins_pose ("*link_dubinspose_tokens*"), upstream paths ("*incoming_tokens*"), downstream paths ("*outgoing_tokens*"), whether the start of the path is blocked ("*is_start_blocked*"), and whether the endpoint is blocked ("*is_end_blocked*"). Additionally, it contains "*waypoints*" with details such as [x coordinate, y coordinate, yaw, elevation, slope], discretely sampled at approximately 0.2-meter intervals. There exist two types of reference paths (as shown in Fig. 21.a): 
  - "*connector_path*" represents internal paths within intersections connecting road segments; 
  - "*base_path*" represents the basic path within a road, similar to the centerline of a two-way lane in urban roads.

- “*borderline*”: A semantic layer that records road boundary information. The boundary line is generated from offline data collected by mapping vehicles and corresponds to the black-and-white boundary of the raster map (*bitmap_mask.png*). This layer is associated with the polygon specified by the token using "*link_polygon_token*." Additionally, it contains "*borderpoints*" with details such as [x coordinate, y coordinate, elevation], discretely sampled at approximately 0.5-meter intervals. The borderline layer has two types (as shown in Fig. 21 b): 
  - "*edge*" represents the outer boundaries of the road, usually non-closed; 
  - "*inner*" represents internal dividers such as walls or roundabouts, usually closed within the road.
  <p align="center">
    <img src="../../../assets/img/F21-b.png" alt="Logo" width="400">
  </p>
  <p align="center">
      <strong>Fig. 21b. Differences between types in the "borderline" layer.</strong>
  </p>



<p align="center">
  <img src="../../../assets/img/F22-hdmap.png" alt="Logo" width="800">
</p>
<p align="center">
    <strong>Fig. 22. Illustration of the different semantic layers in MineSim’s HD map as represented in the real world.</strong>
</p>

### Other
TODO ......