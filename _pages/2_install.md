---
title: "Installation"

permalink: /2_install

sidebar:
    nav: "2_mapping_localization_pathplanning"
---

<br/>

> - **"220504_mapping_localization_path_v1.0.1"** 프로그램을 기준으로 설명함
> - 해당 프로그램은 **2D 맵핑, 위치측위, 경로생성**을 위한 프로그램을 포함하고 있으며, `${ROS2 workspace}/src` 폴더에 설치 또는 복사하여 사용함
{: .notice--info}

<br/>


# 1. 구동 환경

- x86_64 / aarch64
- Ubuntu 18.04 LTS / Jetson (L4T r32.4.4)
- ROS2 Dashing 

<br/>


# 2. 구성 패키지

## (1) 2D 맵핑/위치측위 패키지

- **220504_mapping_localization_path_v1.0.1 / mapping_2d_cartographer_ros2-master**

![]({{ site.url }}/assets/images/2/mapping/mapping_files.jpg){: .align-center} 

Cartographer를 기반으로 2D 라이다만을 이용한 맵핑/위치측위를 수행합니다.

맵핑 패키지와 위치측위 패키지는 동일 패키지이며, 실행 시에 설정만 다르게 구동합니다.

해당 패키지는 GL 라이다에 적합한 파라미터를 적용하였습니다. 



## (2) 경로생성 패키지

- **220504_mapping_localization_path_v1.0.1 / path_planning_ros2-master**

![]({{ site.url }}/assets/images/2/pathplanning/pathplanning_files.jpg){: .align-center} 

맵, 현재 위치, 목표 위치를 입력 받아 현재 위치부터 목표 위치까지의 경로를 생성합니다.



## (3) 라이다 데이터 융합 패키지

- **220504_mapping_localization_path_v1.0.1 / merge_two_laser_ros2-master**

![]({{ site.url }}/assets/images/2/merge/merge_files.jpg){: .align-center} 

하나 또는 두 개의 라이다 데이터를 받아 하나의 포인트클라우드 데이터로 융합합니다.

<br/>





# 3. 필요 요소 설치

아래 명령어를 실행하여 필요 요소를 설치합니다. 

```bash
$ cd ${ROS2 workspace}/src/220504_mapping_localization_path_v1.0.1/mapping_2d_cartographer_ros2-master
$ ./install_dependency.sh
```

> `${ROS2 workspace}`는 ROS2 workspace 폴더의 경로로 수정하여 실행함
{: .notice--info}

<br/>


# 4. 패키지 컴파일

일반적인 ROS2 패키지와 동일하게 컴파일을 수행하여 패키지를 설치합니다.

```bash
$ cd ${ROS2 workspace}
$ colcon build --symlink-install 
```

