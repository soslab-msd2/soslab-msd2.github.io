---
title: "Quick Start"

sidebar:
    nav: "mapping_localization_pathplanning"
---

<br/>


# 1. 개요

2D 맵핑은 라이다 토픽을 퍼블리시 하는 부분과 라이다 토픽을 서브스크라이브하여 맵을 생성하는 부분으로 나뉩니다.

- 라이다 토픽 퍼블리시
- 맵핑 노드 구동

**"220413_mapping_localization_path_v1.0.0"** 프로그램은 rosbag을 포함하고 있으며, rosbag을 이용하여 쉽게 테스트 구동을 해볼 수 있습니다. 

프로그램의 기본 설정으로 구동 시에 실행 노드와 퍼블리시/서브스크라이브 토픽은 아래와 같습니다. 

| 구동 노드             | Subscribe                    | Publish                            |
| :---:                | :---:                        | :---:                              | 
| rosbag2              | ---                          | /scan_front <br/> /scan_back       | 
| merge_two_laser_ros2 | /scan_front <br/> /scan_back | /merged_pointcloud                 | 
| cartographer_node    | /merged_pointcloud           | /submap_list                       | 
| occupancy_grid_node  | /submap_list                 | /map                               | 

<br/>




# 2. 라이다 토픽 퍼블리시

