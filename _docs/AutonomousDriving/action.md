---
title: 액션
permalink: /docs/AD5/
---

##### Table of Contents  
[1. 경로 보간](#pathInterpolation)  
[2.경로 추종(모션제어)](#pathTracking)  
[3.힘제어](#forceControl)  


경로보간, 경로추종(모션제어), 힘제어  


<a name="pathInterpolation" />  
## 1. 경로 보간  
-   



<a name="pathTracking" />  
## 2. 경로 추종(모션 제어)
- Pure Pursuit Control (기하학적 접근방법, 다양한 동력학적 변수가 제외됨)
  - 차량이 진행할 경로의 목표점을 잇는 원호의 곡률을 기하학적으로 계산
- Stanley 방법
- PID 방법 (closed loop, 게인값에 따라서 오실레이션이 있을 수 있다.)
- Model Predictive Control(모델예측 접근 방법, 현재정보+예측정보 활용)



<a name="forceControl" />  
## 3. 힘 제어  
