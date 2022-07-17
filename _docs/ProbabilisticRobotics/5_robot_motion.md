---
title: 05 Robot Motion
permalink: /docs/PR5/
---

### 1. 알고리즘 
![image](https://user-images.githubusercontent.com/57220434/179227602-c80adbac-56d6-4f70-b3c4-3607b35b2b69.png)  
<br>
- motoin model에는 velocity motion model과 odometry motion model이 있다.  
- 이 둘은 제어값을 어디로부터 얻는지에 따라 구분되는데 velocity model은 IMU 등을 이용한 모델이고 odometry model은 엔코더를 이용한 모델이다.  
- 각 모델들은 닫힌 형태 또는 샘플링 형태로 구현된다.  

##### 알고리즘 의미  
- motion model은 $p(x_{t}\vert u_{t},x_{t-1})$ 를 구하는 것이다.  
- 이를 위해 제어값 $u_{t}$에 직접 샘플링 노이즈를 반영하여 $x_{t}$의 분포를 구하는 것이 샘플링 알고리즘이고  
- 닫힌 형태는 $x_{t}$와 $x_{t-1}$을 이용하여 노이즈가 반영된 $\hat{u}$를 계산하고 $\hat{u}$로 $x_{t}$의 분포를 구한다.  
- 속도 모델에서 제어값 $u_{t}$로 다음 로봇 위치를 알 수 있지만 오돔 모델에서 제어값 $u_{t}$는 이동 후 얻어지는 값으로 모션 플래닝에 사용할 수 없다.  
-  
