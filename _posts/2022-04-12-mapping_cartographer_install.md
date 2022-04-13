---
title: "Installation"

sidebar:
    nav: "mapping_localization_pathplanning"
---

<br/>

> - **"220413_mapping_localization_path_v1.0.0"** 프로그램을 기준으로 설명함
> - 해당 프로그램은 **2D 맵핑, 위치측위, 경로생성**을 위한 프로그램을 포함하고 있으며, `${ROS2 workspace}/src` 폴더에 설치 또는 복사하여 사용함
{: .notice--info}

<br/>


# 1. 구동 환경

- x86_64
- Ubuntu 18.04 LTS
- ROS2 Dashing 

<br/>



# 2. 2D 맵핑 패키지

- **220413_mapping_localization_path_v1.0.0 / mapping_2d_cartographer_ros2-master**

![]({{ site.url }}/assets/images/220412/mapping/220412_mapping_files.jpg){: .align-center} 

## (1) 개요

Cartographer를 기반으로 맵핑을 수행합니다.

해당 패키지는 GL 라이다에 적합한 파라미터를 적용하였습니다. 


## (2) 필요 요소 설치

아래 명령어를 실행하여 필요 요소를 설치합니다. 

```bash
$ cd ${ROS2 workspace}/src/220413_mapping_localization_path_v1.0.0/mapping_2d_cartographer_ros2-master
$ ./install_dependency.sh
```

> `${ROS2 workspace}`는 ROS2 workspace 폴더의 경로로 수정하여 실행
{: .notice--info}


## (3) 2D 맵핑 패키지 컴파일

일반적인 ROS2 패키지와 동일하게 컴파일을 수행하여 패키지를 설치합니다.

```bash
$ cd ${ROS2 workspace}
$ colcon build --symlink-install 
```

