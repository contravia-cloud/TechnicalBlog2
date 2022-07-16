---
title: 05 Robot Motion
permalink: /docs/PR5/
---

### 1. 알고리즘 
![image](https://user-images.githubusercontent.com/57220434/179227602-c80adbac-56d6-4f70-b3c4-3607b35b2b69.png)  
<br>
- motoin model에는 velocity motion model과 odometry motion model이 있다.  
- 이 둘은 제어값을 어디로부터 반영하는지에 따라 구분되는데 velocity model은 IMU 등을 이용한 모델이고 odometry model은 엔코더를 이용한 모델이다.  
- 각 모델들에서는 
