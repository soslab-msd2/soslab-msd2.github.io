---
title: "Quick Start"

permalink: /2_mapping_cartographer_quickstart

sidebar:
    nav: "2_mapping_localization_pathplanning"
---

<br/>




# 1. 개요

2D 맵핑은 라이다 토픽을 퍼블리시 하는 부분과 라이다 토픽을 서브스크라이브하여 맵을 생성하는 부분으로 나뉩니다.

- 라이다 토픽 퍼블리시
- 맵핑 노드 구동

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

**merge_two_laser_ros2** 노드는 두 개의 `sensor_msgs::msg::LaserScan` 데이터를 하나의 포인트클라우드 `sensor_msgs::msg::PointCloud2` 데이터로 병합합니다. 

> - **launch** 파일은 **rviz**를 자동실행하여 `/merged_pointcloud` 데이터를 가시화함  
> - **rviz** 중복 실행 시에는 `merge_two_laser_ros2.py` 파일의 `ld.add_action( rviz_node )` 부분을 주석처리하여 **rviz**가 중복하여 실행하지 않도록 주의가 필요함
{: .notice--info}

<br/>




# 3. 맵핑 노드 구동

## (1) `cartographer_node` & `occupancy_grid_node` 노드 실행

아래 명령어를 실행하여 **launch** 파일을 통해 **cartographer_node** 노드와 **occupancy_grid_node** 노드를 실행합니다.

```bash
$ cd ${Project DIR}/mapping_2d_cartographer_ros2-master/launch
$ ros2 launch ./mapping_2d_cartographer.py
```

**cartographer_node** 노드와 **occupancy_grid_node** 노드 구동을 통해 포인트클라우드 `sensor_msgs::msg::PointCloud2` 데이터를 서브스크라이브하여 맵 `nav_msgs::msg::OccupancyGrid` 데이터를 퍼블리시 합니다.

> `mapping_2d_cartographer.py` launch 파일은 **rviz** 가시화를 포함하고 있으므로 중복 실행에 주의가 필요함
{: .notice--info}

<br/>




# 4. 구동 결과

**rosbag2** 노드, **merge_two_laser_ros2.py** launch 파일, **mapping_2d_cartographer.py** launch 파일을 모두 실행하면 아래와 같은 결과를 확인할 수 있습니다.

{% include video id="nt6b2z_FLeo" provider="youtube" %}

> **사용센서 :** GL310 20Hz 2대 (앞, 뒤)  
{: .notice--info}

<br/>




# 5. Occupancy grid map 저장

맵핑 완료 후에 맵 이미지를 저장하기 위해서는 **occupancy grid map**을 저장합니다.

## (1) Nav2 map server 설치

사전에 **nav2 map server**가 설치되어 있지 않다면 아래 명령어를 통해 설치합니다.

```bash
$ apt install -y ros-dashing-nav2-map-server
```

## (2) 맵 이미지 저장

맵 저장을 위해 아래 명령어를 실행합니다.

```bash
$ ros2 run nav2_map_server map_saver
```

위 명령어를 실행하면 현재 생성된 맵 이미지를 `map.pgm`, `map.yaml` 파일로 저장하며, `map.pgm`은 맵 이미지이며, `map.yaml`은 생성된 맵 이미지에 대한 정보를 담고 있습니다.

rosbag 구동 시에 `map.yaml` 파일의 예시는 아래와 같습니다.

```javascript
image: map.pgm
mode: trinary
resolution: 0.05
origin: [-36.9, -14.3, 0]
negate: 0
occupied_thresh: 0.65
free_thresh: 0.25
```

rosbag 구동 시에 `map.pgm` 파일의 예시는 아래와 같습니다.

![]({{ site.url }}/assets/images/2/mapping/map.jpg){: .align-center} 

> 저장한 맵 이미지 `map.pgm`는 우분투에서 **GIMP** 등의 프로그램을 통해 열어볼 수 있으며, **jpg** 형태로 저장이 가능함  
{: .notice--info}

<br/>




# 6. .pbstream 저장

Cartographer를 이용하여 생성한 맵으로 위치측위를 수행하기 위해서는 `.pbstream` 파일 형태로 상태 저장이 필요합니다.

아래 명령어를 통해 맵핑 결과에 대한 상태 저장을 하며, 이때 생성되는 `map.pbstream` 파일은 Cartographer를 이용한 위치측위에 활용됩니다.

```bash
$ ros2 service call /finish_trajectory cartographer_ros_msgs/srv/FinishTrajectory  
$ ros2 service call /write_state cartographer_ros_msgs/srv/WriteState "filename: 'map.pbstream'"  
```

위 명령어를 실행하면 `${Project DIR}/mapping_2d_cartographer_ros2-master/launch` 폴더 내에 `map.pbstream` 파일이 생성됩니다.
