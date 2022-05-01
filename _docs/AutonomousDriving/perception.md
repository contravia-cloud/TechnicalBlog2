---
title: 인지
permalink: /docs/AD3/
---

##### Table of Contents  
[들어가면서](#intro)  
[0.배경기술](#baseTech)  
[1.센싱](#sensing)  
[2.인지(perception)](#perception)  
[3.판단](#decision)  
[4.액션](#action)  


## SLAM  

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

- ORB-SLAM2 vs LSD-SLAM  
  - ORB-SLAM2
    - sparse한 map을 만들지만 내 위치는 엄청 잘 잡는다.
    - CPU에서 돌아갈만큼 가볍다
    - gloval optimization + loop closure 가 들어가 있다.
  - LSD-SLAM
    - 꽤 Dense한 map을 만들고 내 위치도 잘 잡힌다.
    - CPU에서 돌아가긴 하지만 알고리즘이 꽤 무겁다.
    - 그래서 - gloval optimization + loop closure 가 없다.  

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

- SLAM의 정의에 대한 두가지 견해  
  (https://www.slideshare.net/HyunggiChang?utm_campaign=profiletracking&utm_medium=sssite&utm_source=ssslideview)  
  - Stachniss : 연속된 실시간 센서데이터로 위치추정과 지도작성을 동시에 하면 SLAM 이다.
    - Classical robotics 관점  
    - 많은 LiDAR SLAM이 이 정의를 따름
  - Scaamuzza : 연속된 실시간 센서데이터로 위치추정과 지도작성을 도시에 하면서, 내가 만든 지도 안에서 항상 정확한 위치추정이 되어야한다.  
    - Modern robotics 관점 - large scale mapping
    - 최신 Visual SLAM 이 이 정의를 따름  

- 기존의 localisation 방식의 한계  
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

- SLAM 이 어려운 문제인 이유  
  - 기존의 단일 localisation / mapping 기법들로 해결 불가능  
  - 완벽한 state-estimation 이 나올 수 없음  
    - Monte-Carlo localisation 의 경우 sampling을 통한 particle selection이기 때문에 optimal value를 얻을 수 없음  
    - KF/EKF의 경우 선형 근사를 하기 때문에 근사 에러가 생길 수 밖에 없음
  - 시간이 지날수록 에러가 누적되며 추정된 global 위치 값에 큰 오차를 줌
- SLAM 의 해결 방법  
  - 연속된 데이터로부터 가장 정확한 localisation 값과 가장 정확한 mapping 값을 도출해낼 수 있는 최적화 문제로(least squares problem) 문제를 재정의함  
  - least-squares란  
    - over determined system의 해를 구하기 위한 방법(거의 모든 상황으 SLAM문제는 over determined system임)
    - over determined system == 미지수의 수 보다 식의 수가 더 많은 경우 (state의 수 보다 센서 값의 수가 더 많음)
    - 실제 센서 값과 현재 state 값에서 예상되는 센서 값의 차이를 최소화
    - ![image](https://user-images.githubusercontent.com/57220434/166134877-6952785d-1024-477d-9b22-4aef815f38ca.png)  
    - 현재 로봇의 센서 값과 예상 센서 값을 최소화 할 수 있는 로봇의 위치 + 맵의 위치 값은 무엇인가
    - LiDAR SLAM : iterative closet point(ICP)
    - Visual SLAM : Bundle Adjustment (BA)



### SLAM 아닌 SLAM 기술 영역  
#### Structure from Motion  
- 2차원 사진에서 3차원 모델인 포인트 클라우드를 생성하는 기술  
- 상용프로그램에서 널리 사용되고 있음  
- feature detection / matching / relating images(로봇 움직임 추정) /projective reconstruction  
![image](https://user-images.githubusercontent.com/57220434/166128044-a2c5f85b-751a-4f2d-816f-624f3edf3cd7.png)  

#### Odometry  
- 


<iframe src="//www.slideshare.net/slideshow/embed_code/key/ujCD0bKgXipdtb" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/HyunggiChang/master-visualslam-in-2-hours" title="Master Visual-SLAM in 2 hours" target="_blank">Master Visual-SLAM in 2 hours</a> </strong> from <strong><a href="//www.slideshare.net/HyunggiChang" target="_blank">Hyunggi Chang</a></strong> </div>
