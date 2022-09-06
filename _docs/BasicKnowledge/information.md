---
title: 정보
permalink: /docs/BK1/
---

##### Table of Contents  
[1. 로봇 분야 기술 면접](#first)  
[2. 데이터 분석 사례(전통시장 DT 활용 방안)](#second)  
[3. Coverage Path Plan](#coveragePathPlan)  
[4. Bayes Filter](#bayesFilter)  
[5. Covering Models](#CoveringModels)  
[6. 서울시 1인가구 생활실태 분석 결과](#ex6)  



<br>  
<a name="first" /> 
### 1. 로봇 분야 기술 면접  
[50가지 CV/SLAM 기술면접 질문 리스트](http://www.cv-learn.com/20210221-50-CV-SLAM-interview-questions/)  
<br>  

-------------  
<br>  

<a name="second" /> 
### 2. 데이터 분석 사례(전통시장 DT 활용 방안)  
> [2021년 통계데이터센터 자료분석·활용대회-최우수상]전통시장 DT 활용 방안  
> https://github.com/jjonhwa/Policy-to-utilize-DT-in-traditional-markets  
> https://data.kostat.go.kr/sbchome/bbs/boardDetail.do  
<br>  

#### 2.1. 입지선정 알고리즘  
- P-Median, P-Center, UFLP, CFLP 등  
<br>  

##### 2.1.1. P-Median  
> https://github.com/ralaruri/p_median_python  
> https://totoma3.tistory.com/201  
<br>  

![image](https://user-images.githubusercontent.com/57220434/170046076-117c4017-6985-4bdf-84ca-6c18c6eeede0.png)  
<br>  

##### P-Median 구현  
- pulp 팩키지 활용 : python 선형계획법(linear programming, LP)  
- 최적화 알고리즘 : or-tool (https://developers.google.com/optimization/introduction/overview)  
<br>

##### 2.2. 문제 해결 방법  
조건기반 -> 룰기반 -> 모형기반(LP, 정수계획법 등등) -> 메타 휴리스틱(유전알고리즘 등)  
<br>

-------------  
<br>  

<a name="coveragePathPlan" />  
### 3. Coverage Path Plan  

![image](https://user-images.githubusercontent.com/57220434/170496168-b427a47a-6210-46f6-891b-c5d4282cd5ba.png)  

#### 최적화 변수  
여기서는 목적함수가 없다.  
경로 a, b, c, d, e 의 실행 순서가 아래 제약조건을 만족하면 된다.  

#### 제약조건  
인접한 경로는 순서가 1보다 커야한다.(한칸보다 더 떨어져야함)  
같은 경로를 2회 지나갈 수 없다.  

```python
from ortools.sat.python import cp_model


class VarArraySolutionPrinter(cp_model.CpSolverSolutionCallback):
    """Print intermediate solutions."""

    def __init__(self, variables):
        cp_model.CpSolverSolutionCallback.__init__(self)
        self.__variables = variables
        self.__solution_count = 0

    def on_solution_callback(self):
        self.__solution_count += 1
        for v in self.__variables:
            print('%s=%i' % (v, self.Value(v)), end=' ')
        print()

    def solution_count(self):
        return self.__solution_count


def SearchForAllSolutionsSampleSat():
    """Showcases calling the solver to search for all solutions."""
    # Creates the model.
    model = cp_model.CpModel()

    # Creates the variables.
    num_vals = 6
    a = model.NewIntVar(1, 1, 'a')
    b = model.NewIntVar(1, num_vals - 1, 'b')
    c = model.NewIntVar(1, num_vals - 1, 'c')
    d = model.NewIntVar(1, num_vals - 1, 'd')
    e = model.NewIntVar(1, num_vals - 1, 'e')
    
    k = model.NewBoolVar('a>b')
    l = model.NewBoolVar('b>c')
    m = model.NewBoolVar('c>d')
    n = model.NewBoolVar('d>e')

    # Create the constraints.
    model.Add((b-a)>1).OnlyEnforceIf(k)
    model.Add((a-b)>1).OnlyEnforceIf(k.Not())
    
    model.Add((c-b)>1).OnlyEnforceIf(l)
    model.Add((b-c)>1).OnlyEnforceIf(l.Not())
    
    model.Add((d-c)>1).OnlyEnforceIf(m)
    model.Add((c-d)>1).OnlyEnforceIf(m.Not())
    
    model.Add((e-d)>1).OnlyEnforceIf(n)
    model.Add((d-e)>1).OnlyEnforceIf(n.Not())
    
    model.Add(a!=b)
    model.Add(a!=c)
    model.Add(a!=d)
    model.Add(a!=e)
    model.Add(b!=c)
    model.Add(b!=d)
    model.Add(b!=e)
    model.Add(c!=d)
    model.Add(c!=e)
    model.Add(d!=e)

    # Create a solver and solve.
    solver = cp_model.CpSolver()
    solution_printer = VarArraySolutionPrinter([a, b, c, d, e])
    # Enumerate all solutions.
    solver.parameters.enumerate_all_solutions = True
    # Solve.
    status = solver.Solve(model, solution_printer)

    print('Status = %s' % solver.StatusName(status))
    print('Number of solutions found: %i' % solution_printer.solution_count())


SearchForAllSolutionsSampleSat()
```  
<br>  
```
a=1 b=3 c=5 d=2 e=4 
a=1 b=4 c=2 d=5 e=3 
Status = OPTIMAL
Number of solutions found: 2
```

<br>  

<a name="bayesFilter" />  
### 4. Bayes Filter  

![image](https://user-images.githubusercontent.com/57220434/171639735-2558329c-d6f3-443e-98d0-5456454ad14f.png)

<a name="CoveringModels" />  
<br>

### 5. Covering Models  
> [일회용품 쓰레기 감소를 위한, 다회용기 렌탈 사업 비즈니스 모델 개발](https://bigdata.seoul.go.kr/noti/selectNoti.do?r_id=P260&bbs_seq=549&ac_type=A2&sch_type=&sch_text=&currentPage=1)  
  
[image](https://user-images.githubusercontent.com/57220434/173587449-a2a075f5-472d-4714-82fd-e1c611fcbd5b.png)  
  
  

#### covering models 관련 논문  
[Set covering models in optimizing the emergency unit location
of health facility in Palembang](https://iopscience.iop.org/article/10.1088/1742-6596/1282/1/012008/pdf)  
- Location Set Covering Problem (LSCP)
- Maximal Covering Location Problem (MCLP)  

<br>
<a name="ex6" /> 
<br>  

### 6. 서울시 1인가구 생활실태 분석 결과  
> [서울시 1인가구 생활실태 분석 결과](https://data.kostat.go.kr/sbchome/bbs/boardDetail.do)  
> 통계청 데이터와 SK텔레콤 데이터 결합

- 분석 방법에 대한 시사점  
  -  특별한 분석 기법은 없지만 X를 다양하게 조합하여 Y의 변화를 확인하면서 인사이트를 얻고있다.  
  -  X 변수를 소득, 연령, 가구 3가지로 보고 Y를 통신요금 연체, 통화량 등으로 나타내고 있음.
  -  통계청 데이터와 SK텔레콤 데이터 결합 방법  
     -  데이터3법이 개정되면서 가명정보 결합이 가능해짐에 따라 SK텔레콤과 통계청 가명결합 결합키(성, 이름, 생년월일) 생성

![image](https://user-images.githubusercontent.com/57220434/174806123-73195a52-f847-4c4e-8d31-ccc1d738d36d.png)  


  
### 7. Moment  
![image](https://user-images.githubusercontent.com/57220434/177045309-d485d07a-78f7-4b61-8240-169ed82d7d97.png)  

### 8. 가능도  
>[확률 vs 가능도](https://rpubs.com/Statdoc/204928)  

- 연속확률분포에서 특정사건이 발생할 확률은 항상 0 이다.  
- 이때문에 특정사건에 대한 확률을 알고자 가능도의 개념이 나온다.
<br/>
- 가능도의 직관적인 정의 : 확률분포함수의 y값
  - 셀수있는 사건(주사위 등) : 가능도 = 확률
  - 연속사건(1~6 실수) : 가능도 !== 확률, 가능도 = PDF(확률밀도함수)값
<br/>
- 1000번 던져서 앞면이 500번 뒷면이 500번 나올 때, 동전을 던져 앞면이 나올 확률은 0.5  
- 주사위를 3번 던져 각각 1,3,6이 나올 확률 = 1/6 * 1/6 * 1/6 = 1/216
<br/>
- 1000번 던져서 앞면이 400번 뒷면이 600번 나올 때, 동전을 던져 앞면이 나올 확률은 0.4
- 1000번 던져서 앞면이 400번 뒷면이 600번 나올 때, <-- 이런 일이 발생할 가능성(가능도)을 최대로하는 P값은 0.4  
  즉 P의 최대가능도추정량(Maximum Likelihood Estimation) = 0.4 
<br/>
- 키를 5번 측정했을 때 178,179,180,181,182cm이 나올 가능성이 최대가 되는 키는 얼마일까?  

![image](https://user-images.githubusercontent.com/57220434/188644019-db8b48fb-a3b3-4f2f-9986-7b360ed8f5ef.png)





