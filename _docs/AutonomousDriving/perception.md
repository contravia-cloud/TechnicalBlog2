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


### SLAM 아닌 SLAM 기술 영역  
#### Structure from Motion  
- 2차원 사진에서 3차원 모델인 포인트 클라우드를 생성하는 기술  
- 상용프로그램에서 널리 사용되고 있음  
- feature detection / matching / relating images(로봇 움직임 추정) /projective reconstruction  
![image](https://user-images.githubusercontent.com/57220434/166128044-a2c5f85b-751a-4f2d-816f-624f3edf3cd7.png)  

#### Odometry  
- 


<iframe src="//www.slideshare.net/slideshow/embed_code/key/ujCD0bKgXipdtb" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/HyunggiChang/master-visualslam-in-2-hours" title="Master Visual-SLAM in 2 hours" target="_blank">Master Visual-SLAM in 2 hours</a> </strong> from <strong><a href="//www.slideshare.net/HyunggiChang" target="_blank">Hyunggi Chang</a></strong> </div>
