---
title: 개요
permalink: /docs/AD1/
---

##### Table of Contents  
[1.들어가면서](#intro)  
[2.배경기술](#baseTech)  
[3.센싱](#sensing)  
[4.인지(perception)](#perception)  
[5.판단](#decision)  
[6.액션](#action)  

<a name="intro" />  

## 1. 들어가면서  
<br/>  
#### 1.1. 참고 자료  
[자율주행 기술 로드맵-중소벤처기업부](http://smroadmap.smtech.go.kr/)  
[K-MOOC-자율주행자동차기술-국민대](http://www.kmooc.kr/courses/course-v1:KMUk+CK-KMUK_02+2021_1/about)  
[K-MOOC-자율주행 인공지능 시스템-국민대](http://www.kmooc.kr/courses/course-v1:NGV+NGV01+2021_A2/about)  
자율주행 데이터 셋 : BDD100K, KITTI  
[강의- AI in ME 1 - KAIST](https://kooc.kaist.ac.kr/mechanical-engineering-ai)  
<br/>  
[실습-터틀봇 홈페이지](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/#overview)  
[블로그-설명 및 실습](https://soohwan-justin.tistory.com/42?category=1019796)  
<br/>  
[블로그-slam](http://jinyongjeong.github.io/tag/SLAM/)  
[블로그-RVO](https://ropiens.tistory.com/category/keep9oing/robotics)  
<br/>  
[tutorial - PCL](https://pcl.gitbook.io/tutorial/)  


#### 1.2. 기술의 구성  
- 배경 기술  
  - 통계
- 센싱  
    - 필터링
- 인지(perception)  
    - 로컬라이제이션,맵핑, SLAM, 객체인지, 객체추적
- 판단  
    - 경로계획, 예측, 회피  
- 액션  
    - 경로보간, 경로추종(모션제어), 힘제어  
<br/>  

#### 1.3. Traditional GNC 기술 (결정론적 접근, missile 계산 시...)  
> KAIST 김지완 교수 intelligent vehicle 참조  

- Vehicle  
  - Space vehicles, Aerial vehicles, Ground vehicles, Marine vehicles  
- Unmanned Robotic Vehicles  
  - Unmanned Aerial Vehicle (UAV), Unmanned Ground Vehicle (UGV), Unmanned Marine Vehicle (UMV)
- Unmanned Vehicles vs Autonomous Vehicles
  - 무인 이동체는 대부분 원격조정 등이 있어 완전 무인이 아니다. 이중 자율 판단을 추구해나아가는 것이 automous vehicles인 것이다.  
  - 이를 위해 intelligent한 소프트웨어 기술이 필요하다.  

- Intelligent vehicle의 기본 기술 GNC (Guidance, Navigation and Control)  
  - Guidance (planning)  
    To compute the desired path or trajectory from the vehicle's current location to a designate location.  
    Classical guidance laws
      - pursuit guidance(PG), constant bearing guidance, proportional navigation(PN), line-of-sight guidance(LOS)
      - Optimal guidance, way-point guidance
  - Navigation(sensing & perception)  
    To determine the vehicles's motion variables including its position and attitude at a given time.  
    Inertial/dead-reckonig navigarion,GPS navigation, radar navigation, sonar navigation, radio navigation  
    Geophysical navigation : terrain-referecned, geomagnetic, gravimetric  
    Vision-based navigation  
  - Control  
    To determine the necessary forces and moments to satisfy given control objectives (often tracking a guidance trajectory)  
    Classical control (frequency domain techniques)  
    Modern/optimal control (state-space techniques)  
    Robust control, adaptive control, nonlinear control, etc.

- Classical Guidance Examples  
  - Pursuit Guidance  
    - The interceptor remains pointed at the targer at all times  
    - Effective for stationary or slow moving targets but may not be effective against moving targets  
  - Proportional Navigation  
    - The turn rate of the interceptor's heading is proportional to the turn rate of the LOSto the target.  
    - Known to be effective for maneuvering(기동) targets  
- Navigation Filter Algorithms  
  - Complementary filtering  
  - Fixed gain observer  
  - Kalman filter  
  - Particle filter  
- Classical Control  
  - PID  

- 더 높은 수준의 GNC를 위한 AI 적용  
  - 높은 수준의 판단을 위해  
  - 많은 정보를 담고 있는 비전 정보를 딥러닝 방법으로 시도하고 있음  
  - 복잡한 시스템을 강화학습을 통해 효과적으로 제어함 




-----  
<a name="baseTech" />  
## 2. 배경기술  
### 2.1. bayesian
> bayesian 방법 : 확률로 본다는 개념  

bayesian filter는 베이즈 정리와 마코프가정 그리고 전확률 법칙으로 유도된다.  
이 필터는 이전 상태와 제어값을 이용하여 현재 상태를 예측하고 베이즈 정리를 이용하여 센서 입력값을 현재 상태에 업데이트한다.  
![image](https://user-images.githubusercontent.com/57220434/169835082-ae206769-ede7-442e-b615-506411d6cc58.png)  
##### bayesian filter를 만든 이유?  
우리는 베이즈 정리를 통해 추론을 할 수 있다.  
이 원리는 모형(likelihood)을 알고있는 시스템에서 증거(prior)가 주어진다면 상태(posterior)를 추론할 수 있다는 것이다.  
더 쉬운 말로 시스템의 전달함수를 알고있고 결과 값 y가 주어진다면 입력 x를 알 수 있다는 것이다.  
결국 y=ax에서 a와 y를 알면 x를 알 수 있다는 말이다.  단지 x,y,a가 확률분포라는 이야기일 뿐...  
이것을 통해 센서 측정 값으로 현재 상태를 추론하게 된다.
또한 베이즈 필터는 결과 값을 다시 입력으로 넣어주면서 재귀적으로 반복 시켜 과거 값을 계속 반영하도록 만들었는데  
이렇게 하면 이동평균처럼 노이즈를 제거하는 효과를 얻을 수 있다.  
여기서는 평균이 아니라 확률분포를 곱한 것이고, 이것은 이상 값이 들어오면 바로 그값을 취하지 않게되어 노이즈 필터가 된다.  
과거 데이터 기반으로 노이즈를 제거하는 방법의 예로 시계열 분석이 있다.  
마코프가정은 바로 직전 값만을 고려하는 방법이다.  
정리하면 베이즈 필터는 센서 측정 값으로 현재 상태를 추정하고 과거 데이터를 기반으로 노이즈를 제거하는 역할을 수행한다.  
##### bayesian filter의 문제점  
확률분포를 함수로 나타내기 어려워 적분하기가 힘들다.  
그래서 확률분포를 가우시안 분포로 가정하거나 랜덤샘플링 방법으로 계산 가능하도록 한다.  
이 두가지를 각각 가우시안 필터, 파티클 필터 라고 부른다.  








- DDS (Data Distribution Service)  
  - <img src="https://www.researchgate.net/profile/Takuya-Azumi/publication/309128426/figure/fig1/AS:416910068994049@1476410514667/ROS1-ROS2-architecture-for-DDS-approach-to-ROS-We-clarify-the-performance-of-the-data.png" width="60%" height="60%" title="타이틀" alt="image"/>  
  - image from : Exploring the Performance of ROS 2 <https://www.researchgate.net/publication/309128426_Exploring_the_performance_of_ROS2>
  - DDS(Data Distribution Service)는 OMG(http://www.omgwiki.org/dds/)에서 국제 표준으로 정한 실시간 데이터 분배 미들웨어 입니다.  
- GPG (GNU Privacy Guard, GnuPG 또는 GPG)

-----  
<a name="sensing" />  
## 3. 센싱

-----  
<a name="perception" />  
## 4. 인지(perception)  
### 4.1. 로컬라이제이션  

### 4.3 SLAM  
> [Velodyne 공급사홈페이지-라이다를 활용한 SLAM](http://www.lumisol.co.kr/sub/reference/lidar.asp?mode=view&bid=4&s_type=&s_keyword=&s_cate=&idx=212&page=1)  
> [석사학위논문 - 자율 주차를 위한 Around View Monitor(AVM) 기반 Visual SLAM](https://s-space.snu.ac.kr/bitstream/10371/174850/3/000000164251.pdf)  

-----  
<a name="decision" />  
## 5. 판단  
### 5.1 경로 계획  
> [논문-세타*: 동적 가중치를 가진 계층 구조식 경로계획법](https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=DIKO0014017925&dbt=DIKO)  
> [학술-A Survey of Path Planning Algorithms for Mobile Robots](https://www.mdpi.com/2624-8921/3/3/27/pdf)  
> [블로그-Application of hybrid A star](https://miracleyoo.tistory.com/21)  
> [블로그-ROS2 - RRT알고리즘 python](https://velog.io/@delicate1290/ROS2-RRT-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)  



-----  
<a name="action" />  
## 6. 액션
### 6.2. 경로추종(모션제어)  
- 


