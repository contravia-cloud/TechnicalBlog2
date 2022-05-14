---
title: 판단
permalink: /docs/AD4/
---


##### Table of Contents  
[1. Path planning](#pathPlanning)  
[2. Path smoothiong](#pathSmoothing)  
[3. Path following](#pathFollowing)  


[논문 2011 - 무인 항공기 생존성 극대화를 위한 이동 경로 계획 알고리즘선정](https://www.koreascience.or.kr/article/JAKO201128563049638.pdf)


<a name="pathPlanning" />  
## 1. Path planning  
- 알고리즘의 변천  
  - Dijkstra
  - A* 정적환경
  - D* 동적환경 - 그러나 큰 환경에서는 사용어려움  
  - RRT or hybrid(RA* (relaxed A) + meta heuristic)  
  - hybrid : RA* : 알고리즘 초기화  
             meta heuristic : 사전 최적화 (genetic, ant colony, firefly)  
  - 

