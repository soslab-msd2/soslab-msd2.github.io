---
title: "Customize"

permalink: /2_pathplanning_customize

sidebar:
    nav: "2_mapping_localization_pathplanning"
---

<br/>




# 1. 실시간 라이다 데이터 퍼블리시

앞서 [Path Planning / Quick Start]({{site.url}}/2_pathplanning_quickstart)에서는 rosbag을 이용하여 구동을 해보았습니다.

실제 시스템에서 구동하기 위해서는 실시간 라이다 데이터를 퍼블리시 해야 합니다.

## (1) GL 드라이버 패키지 설치

GL 시리즈의 라이다 데이터 퍼블리시를 위해서는 [GL (2D LiDAR)]({{site.url}}/1_gl)을 참고하여 해당 GL 드라이버 패키지를 설치하여 구동합니다.

## (2) 라이다 토픽 이름 수정 및 구동

이때, GL 드라이버 패키지의 **launch** 파일에서 라이다 토픽 이름을 `/scan_front` 또는 `/scan_back`으로 변경하여 구동합니다.

```python
            {'pub_topicname_lidar': 'scan'},
```

<br/>




# 2. 목표 위치 서브스크라이브 설정

경로생성 노드는 목표 위치를 두 가지 방법으로 서브스크라이브 합니다.

- **rviz**의 **2D Nav Goal** 버튼을 이용한 목표 위치 설정
- **다른 노드**에서 목표 위치 퍼블리시를 통한 목표 위치 설정

## (1) **rviz**의 **2D Nav Goal** 버튼을 이용한 목표 위치 설정

`path_planning_ros2.py` launch 파일에서 아래 부분 수정을 통해 rviz 버튼을 이용한 목표 위치 설정 여부를 수정합니다.

```python
            {'rviz_goal_onoff'        :True               },
```

기본적으로 ``True``로 설정하여 rviz를 통해 목표 위치를 설정할 수 있으며, 필요할 경우 `False`로 설정하여 해당 기능을 끄도록 합니다.

## (2) 다른 노드에서 목표 위치 퍼블리시를 통한 목표 위치 설정

다른 노드에서 목표 위치를 퍼블리시하여, 해당 데이터를 `path_planning_ros2` 노드에서 서브스크라이브하여 목표 위치를 설정할 수 있습니다.

목표 위치 토픽 타입은 `geometry_msgs::msg::PoseStamped`이며, `path_planning_ros2.py` launch 파일에서 아래 부분 수정을 통해 토픽 이름을 변경하여 사용할 수 있습니다.

```python
            {'goal_sub_topicname'     :'goal'             },
```

<br/>




# 3. 다른 노드에서 현재 위치 퍼블리시를 통한 현재 위치 설정

기본 설정된 `path_planning_ros2` 노드는 `base_link`와 `map` TF 사이의 차이로부터 현재 위치를 계산하여 경로를 생성합니다.

다른 노드에서 현재 위치를 계산하여 퍼블리시하고, `path_planning_ros2` 노드에서 서브스크라이브하여 현재 위치를 사용할 수 있습니다.

현재 위치 토픽 타입은 `geometry_msgs::msg::PoseStamped`이며, `path_planning_ros2.py` launch 파일에서 아래 부분 수정을 통해 토픽 이름을 변경하여 사용할 수 있습니다.

```python
            {'pos_sub_topicname'      :'localization_pose'},
            {'use_external_pos'       :False              },
```

기본적으로 `use_external_pos`은 `False`로 설정되어 있으며, 다른 노드에서 퍼블리시하는 현재 위치를 사용할 경우에는 `True`로 설정합니다.

<br/>




# 4. 실시간 라이다 데이터를 활용한 경로생성

경로생성 시에 맵 정보만 가지고 경로를 생성할 수도 있으며, 실시간 라이다 측정 데이터를 활용하여 경로생성을 할 수 있습니다.

`path_planning_ros2.py` launch 파일에서 아래 부분을 수정하여 실시간 데이터 사용 여부를 결정할 수 있습니다.

```python
            {'lidar_onoff'            :True               },
```

실시간 라이다 측정 데이터를 활용하면 사람 등의 움직이는 물체를 회피하여 경로생성을 할 수 있습니다.

{% include video id="Kn4cYO7U0PE" provider="youtube" %}

<br/>




# 5. 경로생성 관련 파라미터

|Name|Type|Range|Description|
|:---:|:---:|:---:|---|
|radius_of_wall_collision|double|0.0 ~ inf [m]|로봇이 맵과 충돌가능한 범위 설정|
|radius_of_wall_expansion|double|0.0 ~ inf [m]|맵 기반 Cost map 최대 확장 범위 설정|
|value_of_wall_expansion|int|0 ~ 254|맵 정보 기반 Cost map의 최소 값 설정|
|number_of_wall_layers|int|0 ~ inf|맵의 확장 레이어의 개수 설정|
|radius_of_obstacle_collision|double|0.0 ~ inf [m]|로봇이 실시간 라이다 기반 물체와 충돌가능한 범위 설정|
|radius_of_obstacle_expansion|double|0.0 ~ inf [m]|실시간 라이다 기반 Cost map 최대 확장 범위 설정|
|value_of_obstacle_expansion|int|0 ~ 254|라이다 정보 기반 Cost map 최소 값 설정|
|number_of_obstacle_layers|int|0 ~ inf|라이다 정보의 확장 레이어 개수 설정|
|radius_of_goal_detection|double|0 ~ inf [m]|목표점이 접근 불가 영역일 경우 주변 새로운 목표점 스캔 범위|
|value_of_goal_detection|int|0 ~ inf|목표점이 접근 불가 영역일 경우 주변 새로운 목표점 설정 가능 Cost 값|
|robot_length|double|0.0 ~ inf [m]|로봇의 한 변의 길이 설정(정사각형)|
|threshold_wall|int|0 ~ 100|Cartographer에서 넘어온 맵정보를 필터링하는 역치 값|
|threshold_path|double|0.0 ~ inf [m]|경로 업데이트 역치<br/>(기존 경로와 현재 경로의 길이 차이)|
|cost_factor|double|0.0 ~ inf [m]|다익스트라 알고리듬에서 각 Cost가 반영되는 Weight|