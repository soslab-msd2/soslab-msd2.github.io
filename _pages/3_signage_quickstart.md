---
title: "Quick Start"

permalink: /3_signage_quickstart

sidebar:
    nav: "3_signage"
---

<br/>

> - **"220507_gl_signage_v3.0.0"** 프로그램을 기준으로 설명함
{: .notice--info}

<br/>




# 1. 2D 라이다(GL) 셋팅

## (1) 센서 설치

![]({{ site.url }}/assets/images/3/install_sensor.jpg){: .align-center} 

센서를 스크린의 하단 또는 상단에 설치합니다.

> - 센서의 아래 면을 벽으로 향하도록 설치합니다. 
{: .notice--info}

![]({{ site.url }}/assets/images/3/install_sensor2.jpg){: .align-center} 

센서는 12~24V 전원을 사용하며, 컴퓨터와 통신을 위해 랜 케이블을 이용하여 연결합니다. 


## (2) 네트워크 설정

![]({{ site.url }}/assets/images/3/network_setting.jpg){: .align-center} 

"제어판\네트워크 및 인터넷\네트워크 연결"에서 해당 이더넷 속성을 변경합니다.

> - 컴퓨터 IP의 기본 값은 `10.110.1.3`이며, 서브넷 마스크는 `255.255.255.0`으로 설정합니다.
{: .notice--info}

<br/>




# 2. 프로그램 실행

![]({{ site.url }}/assets/images/3/main_window.jpg){: .align-center} 

**220507_gl_signage_v3.0.0/gl_signage.exe** 파일을 실행하여 프로그램을 실행합니다.


## (1) 2D 라이다(GL) 설정

### a. LiDAR IP/Port, PC IP/Port 설정

|설정 이름|기본 값|
|:---:|:---:|
|LiDAR IP|10.110.1.2|
|LiDAR Port|2000|
|PC IP|10.110.1.3|
|PC Port|3000|

라이다의 IP/Port와 PC의 IP/Port를 설정합니다.

### b. 라이다 연결

**연결 버튼**을 통해 라이다와 연결을 수행합니다.

라이다와 연결된 상태에서만 다른 기능을 수행할 수 있습니다. 

- LiDAR 연결 성공

연결이 성공적으로 이뤄지면 연결된 라이다의 시리얼 넘버가 출력됩니다.

![]({{ site.url }}/assets/images/3/lidar_product_num.jpg){: .align-center}

- LiDAR 연결 실패

![]({{ site.url }}/assets/images/3/lidar_is_not_responding.jpg){: .align-center}


### c. 라이다 구동 테스트

![]({{ site.url }}/assets/images/3/lidar_test.jpg){: .align-center}

**구동 테스트 버튼**을 누르면 라이다 데이터를 가시화 합니다.

> - 하나의 격자(grid)는 1m를 의미합니다.
{: .notice--info}

가시화의 종료, 확대/축소, 위아래/좌우 반전을 위해서 아래 단축키를 사용합니다.

|단축키|설명|
|:---:|:---:|
|'ESC' or 'q'|가시화 창 종료|
|'-'|1m 단위로 축소|
|'+'|1m 단위로 확대|
|'v'|vertical 방향으로 반전|
|'h'|horizontal 방향으로 반전|


## (2) 스크린 설정

스크린 설정 부분에서 현재 연결된 스크린을 확인하고 사용하고자 하는 스크린을 설정합니다.

### a. 스크린 확인

**스크린 확인 버튼**을 누르면 현재 연결된 스크린을 모두 확인할 수 있으며, 각 스크린의 왼쪽 상단에 번호를 가시화하여 확인이 가능합니다.

### b. 스크린 선택

**스크린 선택**에서 사용하고자 하는 스크린을 선택하여 설정합니다.


## (3) 라이다-스크린 캘리브레이션 설정

### a. 센서 위치

![]({{ site.url }}/assets/images/3/install_sensor.jpg){: .align-center}

센서의 설치 위치에 따라 상단/하단을 선택합니다.

### b. Screen Offset

![]({{ site.url }}/assets/images/3/offset_size.jpg){: .align-center}

**Screen Offset x** 값은 센서로부터 스크린 왼쪽 면까지의 길이를 의미하며, **Screen Offset y** 값은 센서로부터 스크린 상단 또는 하단 면까지의 길이를 의미합니다. 

### c. Screen Size

**Screen Size Width** 값은 스크린의 가로 길이를 의미하며, **Screen Size Height** 값은 스크린의 세로 길이를 의미합니다. 

### d. Margin

![]({{ site.url }}/assets/images/3/margin.jpg){: .align-center}

스크린에 터치 제외 영역을 설정하기 위해 사용합니다.

스크린에서 margin에 해당하는 부분을 제외한 영역만 터치가 가능합니다.


## (4) 터치 설정

### a. Distance Thr.

![]({{ site.url }}/assets/images/3/distance_thr.jpg){: .align-center}

**Distance Thr.** 값은 멀티 터치 시에 두 포인트 구분을 위한 최소 거리입니다.

멀티 터치 시에 두 포인트 간의 거리가 distance thr. 값 보다 작을 경우 하나의 포인트로 인식하며, 클 경우에 두 개의 포인트로 인식합니다. 

### b. 터치 시작

|단축키|설명|
|:---:|:---:|
|'F8'|터치 시작|
|'F9'|터치 종료|

**터치 시작 버튼** 또는 **단축키**를 통해 터치를 구동합니다. 

### c. 구동 테스트

![]({{ site.url }}/assets/images/3/touch_test.jpg){: .align-center}

터치 위치 등을 테스트 하기 위해 사용합니다.

구동 테스트에서 빨간점 부분이 터치가 되는 부분이며, 실제 터치 위치와 상이할 경우에는 설정 값 등을 확인해야 합니다. 

<br/>

