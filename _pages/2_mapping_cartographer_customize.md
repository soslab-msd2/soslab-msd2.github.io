---
title: "Customize"

permalink: /2_mapping_cartographer_customize

sidebar:
    nav: "2_mapping_localization_pathplanning"
---

<br/>




# 1. 실시간 라이다 데이터 퍼블리시

앞서 [2D Mapping / Quick Start]({{site.url}}/2_mapping_cartographer_quickstart)에서는 rosbag을 이용하여 구동을 해보았습니다.

실제 시스템에서 구동하기 위해서는 실시간 라이다 데이터를 퍼블리시 해야 합니다.

## (1) GL 드라이버 패키지 설치

GL 시리즈의 라이다 데이터 퍼블리시를 위해서는 [GL (2D LiDAR)]({{site.url}}/1_gl)을 참고하여 해당 GL 드라이버 패키지를 설치하여 구동합니다.

## (2) 라이다 토픽 이름 수정 및 구동

이때, GL 드라이버 패키지의 **launch** 파일에서 라이다 토픽 이름을 `/scan_front` 또는 `/scan_back`으로 변경하여 구동합니다.

```python
            {'pub_topicname_lidar': 'scan'},
```

<br/>




# 2. 라이다 설치 위치 설정

라이다 설치 위치에 대한 설정을 위해서는 `${Project DIR}/merge_two_laser_ros2-master/launch` 폴더 내의 `merge_two_laser_ros2.py` 파일에서 아래 값을 수정합니다. 

```python
            {'laser1_offset_x': 0.150},
            {'laser1_offset_y': 0.0},
            {'laser1_offset_yaw': -0.004},

            {'laser2_offset_x': -0.250},
            {'laser2_offset_y': 0.0},
            {'laser2_offset_yaw': 0.000},
```

> - x축, y축에 대한 위치 값은 [m] 단위 입니다.  
> - 회전(yaw) 값은 [rad] 단위 입니다.
{: .notice--info}

각 변수의 의미는 아래 그림과 같습니다.

![]({{ site.url }}/assets/images/2/mapping/sensor_install.jpg){: .align-center} 

이동체의 중심이 되는 `base_link` 좌표계로부터 각 센서의 위치(x축, y축)와 방향(yaw)을 설정합니다.

> 센서의 위치 설정 값이 맵핑 또는 위치측위 결과에 큰 영향을 주므로 최대한 정확한 설정이 필요함
{: .notice--info}