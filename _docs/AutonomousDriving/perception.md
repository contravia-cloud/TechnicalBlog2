---
title: 인지
permalink: /docs/AD3/
---

##### Table of Contents  
[1. 로컬라이제이션](#localisation)  
[2. 매핑](#mapping)  
[3. SLAM](#slam)  



<a name="localisation" />  
## 1. 로컬라이제이션  
- local 로컬라이제이션 : 상대적으로 빠르다. 리소스 적게필요  
- global 로컬라이제이션 : external reference (인공위성, 랜드마크 등 활용)  

### 1.1. GNSS 기반 로컬라이제이션  

### 1.2. wheel odometry 기반 로컬라이제이션  
- 외부참조 정보가 필요하지 않다.  
- 상대적 로컬라이제이션  

### 1.3. INS(inertia navigation system) 로컬라이제이션  
- 외부참조 정보가 필요없음
- IMU 센서는 가속도계, 자이로스코프, 지자기센서로 이뤄진 경우가 많음  

### 1.4. 외부참조 정보를 이용한 로컬라이제이션  
- 패시브 디바이스 (표지판) 또는  
  액티브 디바이스 (와이파이,블루투스 비콘) 사용  

### 1.5. LiDAR 기반 로컬라이제이션  
- 빌딩, 벽, 나무와 같은 자연적인 랜드마크 사용
- 라이다 기반 로컬라이제이션에는 보통 스캔 매칭을 이용한다.  
  - 두 스캔 결과가 최적으로 중첩될 수 있는 기하학적 정렬을 탐색하는 기법  
  - 지도 구축 시 오도메트리 교정에 효과적
  
#### 스캔매칭  
- 가장 많이 인용된 스캔 매칭 기법은 ICP(interative closet point)이다.  
- ICP는 두 스캔 사이의 점 대 점 거리를 최소화하도록 하는 반복 알고리즘이다.  
  - 참조 스캔의 각 지점에 대해, 개체 스캔의 가장 가까운 지점이나 가장 가까운 주변 지점을 골라 대응을 탐색한다.  
  - 참조 스캔과 개체 스캔에 대한 모든 대응 쌍의 제곱 평균 오차를 최소화하는 가체 변환을 계산한다.  
- PLICP(점 대 선 거리 사용) : 수렴 속도 향상, but no robust  
- 상관스캔매칭 : 데이터 관측 성공 **확률** 최대화하는 강체 변환을 탐색    
- 특징대 특징 : FLIRT (fast laser interest region transform, 고속 레이저 관심 지역 변환)  
  - 지역적으로 구분할 수 있는 랜드마크가 많은 환경에서는 부분적으로 효과적일 수 있음  
  - 일반적으로 점 기반 접근 방식보다 환경을 컴팩트하게 표현할 수 있음  
- 레이더를 오도메트리와 매핑에 활용하는 방식  

### 1.6. 카메라 기반 로컬라이제이션  
- 모노, 스테레오, RGB-D 카메라 활용  
- Visual Odometry : 시작점으로부터 추정한 자세를 점진적으로 갱신하는 방식으로 차량의 궤적을 획득한다.  
- 

<a name="mapping" />  
## 2. mapping

<a name="slam" />  
## 3. SLAM  
### 3.1. 서론
- SLAM 구분(2020.11)  
  - 크게 LiDAR SLAM (대표적 알고리즘 : Lidar Odometry and Mapping(LOAM))  
  - RGB-D SLAM  
  - IMU를 사용하는 Visual Inertial SLAM 혹은 Visual Inertial Odometry  
  - Visual SLAM 으로 나뉜다.(모노카메라 Direct Sparse Odometry(DSO), 스테레오 카메라Stereo Direct Sparse Odometry(Stereo DSO))  
- Visual SLAM의 과정(2020.11)  
  - 특징점 탐색 : FAST+BRIEF = ORB, 따라서 ORB SLAM이 있었다.  
  - Data Association : Nearest Neighbor(NN), Multi-Hypothesis Tracker(MHT), Joint Compatibility Branch and Bound(JCBB), Simple Online and Realtime Tracking(SORT) 등의 방법이 있다. 최근에는 CNN을 활용한 Deep SORT가 뛰어난 성능을 보이고 있다.
  - tracking : 비용함수(reprojuction error, photometric error) 정의, Iterative Closest Point(ICP) 등으로 최적화  
  - 성능평가 : Absolute Trajectory Error(ATE)  

#### Visual SLAM  
- monocular SLAM  
  - ORB-SLAM2 및 LSD-SLAM이 있다.  
  - 카메라 자세추정 : 서로 다른 시점에서 촬영된 이미지로 3차원 위치 추정  
  - 카메라 트래킹 : 특징점을 이용해서 카메라가 지도 내에 어디 있는지 추정  
- ORB-SLAM2 vs LSD-SLAM 
  - ORB-SLAM2
    - sparse한 map을 만들지만 내 위치는 엄청 잘 잡는다.
    - CPU에서 돌아갈만큼 가볍다
    - gloval optimization + loop closure 가 들어가 있다.
  - LSD-SLAM (Large-Scale Direct monocular SLAM)
    - 꽤 Dense한 map을 만들고 내 위치도 잘 잡힌다.
    - CPU에서 돌아가긴 하지만 알고리즘이 꽤 무겁다.
    - 그래서 - global optimization + loop closure 가 없다.  

- 기술 : 특징 추출, 특징 매칭, 카메라 자세추정(삼각측량법-> 초기지도작성), 카메라 트래킹
- 특징 매칭 시 강건성 확보 알고리즘: 순환 일관성(cycle consistency), epipolar 제약, 호모그라피
- 전역지도관리
  - 국소지도확장 -> 오차 보정 -> 전역지도정합(전역지도 최적화)  
    : 번들 조정(정합), 루프결합, 자세그래프 최적화 그리고 카메라 자세 놓쳤을 때 최적화카메라 위치 재조정  
    
- 비전 지식  
  - camera calibration  
  - feature detection / feature matching  
  - optical flow  
  - epipolar geometry / fundamental matrix / essential matrix / triangulation  
  - bundle adjustment / levenberg=marquardt method / gauss-newton method
  - perspective N points

- Deep-SLAM  
  - 최신 분야  
  - 3D Object Detection  
  - Object segmentation  
  - image retrieval (loop closure를 위한..)
  - 6D pose localization & estimation  
  - real time performance 중요  
    - model compression & quantization  
    - inference optimization  
    - hardware
  - CPU/GPU  
    - Nvidia Jetson Nano GPU
    - Google Coral TPU
    - Intel Neural Compute Stick VPU

아래글은 다음 참조함  
(https://www.slideshare.net/HyunggiChang?utm_campaign=profiletracking&utm_medium=sssite&utm_source=ssslideview)  

#### SLAM의 정의에 대한 두가지 견해  
  (https://www.slideshare.net/HyunggiChang?utm_campaign=profiletracking&utm_medium=sssite&utm_source=ssslideview)  
  - Stachniss : 연속된 실시간 센서데이터로 위치추정과 지도작성을 동시에 하면 SLAM 이다.
    - Classical robotics 관점  
    - 많은 LiDAR SLAM이 이 정의를 따름
  - Scaamuzza : 연속된 실시간 센서데이터로 위치추정과 지도작성을 도시에 하면서, 내가 만든 지도 안에서 항상 정확한 위치추정이 되어야한다.  
    - Modern robotics 관점 - large scale mapping
    - 최신 Visual SLAM 이 이 정의를 따름  

#### 기존의 localisation 방식의 한계  
  - Monte-Carlo localisation (particle filter)
    - configuration space(예 map) 정보를 알 때, particle filter를 이용해서 위치추정을 수행하는 방법  
    - Map의 정보가 정확하지 않을 때, 정확한 localisation에 실패하게 됨.  
    - Map 이 없다면 사용할 수 없음.  
    - Discrete multi modal 추정이기 때문에 비슷한 맵 구간이 많으면 헷갈릴 수 있다.
  - Kalman filter (+ Extended Kalman Filter)  
    - gaussian 분포의 motion model 과 sensor measurement 값의 correlation 을 구해 최적의 위치를 추정하는 방법.  
    - Kalma filter는 선형 시스템에서만 적용되기 때문에 비선형 시스템을 선형근사하는 Extended Kalman Filter를 사용한다.
    - 계산해야하는 state가 많아질수록 covariance matrix가 quadratic 하게 증가하기 때문에 실시간 환경에서 모든 센서 데이터를 다 사용할 수 없음.
    - EKF의 경우 선형근사를 하기 때문에 매 state update 마다 에러가(drift error) 쌓일 수 밖에 없음  

#### SLAM 이 어려운 문제인 이유  
  - 기존의 단일 localisation / mapping 기법들로 해결 불가능  
  - 완벽한 state-estimation 이 나올 수 없음  
    - Monte-Carlo localisation 의 경우 sampling을 통한 particle selection이기 때문에 optimal value를 얻을 수 없음  
    - KF/EKF의 경우 선형 근사를 하기 때문에 근사 에러가 생길 수 밖에 없음
  - 시간이 지날수록 에러가 누적되며 추정된 global 위치 값에 큰 오차를 줌

#### SLAM 의 해결 방법  
  - 연속된 데이터로부터 가장 정확한 localisation 값과 가장 정확한 mapping 값을 도출해낼 수 있는 최적화 문제로(least squares problem) 문제를 재정의함  
  - least-squares란  
    - over determined system의 해를 구하기 위한 방법(거의 모든 상황으 SLAM문제는 over determined system임)
    - over determined system == 미지수의 수 보다 식의 수가 더 많은 경우 (state의 수 보다 센서 값의 수가 더 많음)
    - 실제 센서 값과 현재 state 값에서 예상되는 센서 값의 차이를 최소화
    - ![image](https://user-images.githubusercontent.com/57220434/166134877-6952785d-1024-477d-9b22-4aef815f38ca.png)  
    - 현재 로봇의 센서 값과 예상 센서 값을 최소화 할 수 있는 로봇의 위치 + 맵의 위치 값은 무엇인가
    - LiDAR SLAM : iterative closet point(ICP)
    - Visual SLAM : Bundle Adjustment (BA)  

#### Graph based SLAM  
  - 로봇의 움직임고 위치를 node와 edge로 구성  
  - 로봇이 odometry 정보로 움직이면서 motion constraint를 구성  
  - 추가 constaint(예 loop closure)등이 생성되면 최적화(least-squares) 수행 가능
  - least-squares 문제를 구성하기 위해 graph based SLAM 이 사용됨 (즉 의존성의 관계를 형성하여 최적화 한다.)  
  - ![image](https://user-images.githubusercontent.com/57220434/166136610-fbf6f7b6-b7f3-4ff0-a063-89f2676f084e.png)  
  - global graph optimisatin이 없으면 odometry  
  - global graph optimisatin이 있으면 SLAM (Scaramuzza의 SLAM 정의)  







### SLAM 아닌 SLAM 기술 영역  
#### Structure from Motion  
- 2차원 사진에서 3차원 모델인 포인트 클라우드를 생성하는 기술  
- 상용프로그램에서 널리 사용되고 있음  
- feature detection / matching / relating images(로봇 움직임 추정) /projective reconstruction  
![image](https://user-images.githubusercontent.com/57220434/166128044-a2c5f85b-751a-4f2d-816f-624f3edf3cd7.png)  

#### Odometry  
- 


<iframe src="//www.slideshare.net/slideshow/embed_code/key/ujCD0bKgXipdtb" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/HyunggiChang/master-visualslam-in-2-hours" title="Master Visual-SLAM in 2 hours" target="_blank">Master Visual-SLAM in 2 hours</a> </strong> from <strong><a href="//www.slideshare.net/HyunggiChang" target="_blank">Hyunggi Chang</a></strong> </div>
