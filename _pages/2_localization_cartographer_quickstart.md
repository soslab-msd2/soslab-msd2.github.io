---
title: "Quick Start"

permalink: /2_localization_cartographer_quickstart

sidebar:
    nav: "2_mapping_localization_pathplanning"
---

<br/>




# 1. 개요

2D 위치측위는 사전에 이미 만들어 놓은 맵(`map.pbstream`)을 이용하여 수행합니다. 

위치측위는 라이다 토픽을 퍼블리시 하는 부분과 라이다 토픽을 서브스크라이브하여 위치측위를 수행하는 부분으로 나뉩니다.

- 라이다 토픽 퍼블리시
- 위치측위 노드 구동

**"220413_mapping_localization_path_v1.0.0"** 프로그램은 **rosbag**을 포함하고 있으며, **rosbag**을 이용하여 쉽게 테스트 구동을 해볼 수 있습니다. 

프로그램의 기본 설정으로 구동 시에 실행 노드와 퍼블리시/서브스크라이브 토픽은 아래와 같습니다. 

| 구동 노드 | Subscribe | Publish |
| :---: | :---: | :---: |
| rosbag2 | --- | /scan_front <br/> (sensor_msgs::msg::LaserScan) <br/> /scan_back <br/> (sensor_msgs::msg::LaserScan) |
| merge_two_laser_ros2 | /scan_front <br/> (sensor_msgs::msg::LaserScan) <br/> /scan_back <br/> (sensor_msgs::msg::LaserScan) | /merged_pointcloud <br/> (sensor_msgs::msg::PointCloud2) |
| cartographer_node | /merged_pointcloud <br/> (sensor_msgs::msg::PointCloud2) | /submap_list |
| occupancy_grid_node | /submap_list | /map <br/> (nav_msgs::msg::OccupancyGrid) |

<br/>




# 2. 라이다 토픽 퍼블리시

## (1) ROSBAG 실행

아래 명령어를 실행하여 **rosbag**을 구동합니다.

```bash
$ cd ${Project DIR}
$ ros2 bag play rosbag2_2022_04_11-20_09_20
```

> `${Project DIR}`는 **“220413_mapping_localization_path_v1.0.0”** 프로그램 폴더의 경로를 의미하며, 해당 폴더의 경로로 수정하여 실행함
{: .notice--info}

`rosbag2_2022_04_11-20_09_20`은 이동체에 2D 라이다(GL310) 2대를 앞뒤로 설치하여 취득한 데이터 입니다.

각각 `/scan_front`, `/scan_back` 토픽 이름으로 `sensor_msgs::msg::LaserScan` 데이터를 퍼블리시 합니다.


## (2) `merge_two_laser_ros2` 노드 실행

`rosbag2_2022_04_11-20_09_20`은 2대의 라이다 데이터를 각각 퍼블리시합니다.

따라서 맵핑에 이용하기 위해 하나의 데이터로 병합합니다.

아래 명령어를 실행하여 **launch** 파일을 통해 **merge_two_laser_ros2** 노드를 실행합니다.

```bash
$ cd ${Project DIR}/merge_two_laser_ros2-master/launch
$ ros2 launch ./merge_two_laser_ros2.py
```

> `${Project DIR}`는 **“220413_mapping_localization_path_v1.0.0”** 프로그램 폴더의 경로를 의미하며, 해당 폴더의 경로로 수정하여 실행함
{: .notice--info}

**merge_two_laser_ros2** 노드는 두 개의 센서 `sensor_msgs::msg::LaserScan` 데이터를 하나의 포인트클라우드 `sensor_msgs::msg::PointCloud2` 데이터로 병합합니다. 

> - **launch** 파일은 **rviz**를 자동실행하여 `/merged_pointcloud` 데이터를 가시화함  
> - **rviz** 중복 실행 시에는 `merge_two_laser_ros2.py` 파일의 `ld.add_action( rviz_node )` 부분을 주석처리하여 **rviz**가 중복하여 실행하지 않도록 주의가 필요함
{: .notice--info}

<br/>




# 3. 위치측위 노드 구동

## (1) `cartographer_node` & `occupancy_grid_node` 노드 실행

아래 명령어를 실행하여 **launch** 파일을 통해 **cartographer_node** 노드와 **occupancy_grid_node** 노드를 실행합니다.

```bash
$ cd ${Project DIR}/mapping_2d_cartographer_ros2-master/launch
$ ros2 launch ./localization_2d_cartographer.py
```

> - 앞서 [2D Mapping / Quick Start / 6. .pbstream 저장]({{site.url}}/2_mapping_cartographer_quickstart#6-pbstream-저장)의 과정 수행을 통해 launch 폴더 내에 생성한 `map.pbstream` 파일이 있다고 가정함  
> - `localization_2d_cartographer.py` launch 파일은 **rviz** 가시화를 포함하고 있으므로 중복 실행에 주의가 필요함
{: .notice--info}

<br/>




# 4. 구동 결과

**rosbag2** 노드, **merge_two_laser_ros2.py** launch 파일, **localization_2d_cartographer.py** launch 파일을 모두 실행하면 아래와 같은 결과를 확인할 수 있습니다.

{% include video id="kgX6rKFhKsM" provider="youtube" %}

> **사용센서 :** GL310 20Hz 2대 (앞, 뒤)  
{: .notice--info}

