## 7장. SRP(단일 책임 원칙)
> 정리 링크 : https://gh402.tistory.com/59

#### 하나의 모듈은 오직 하나의 액터에 대해서만 책임져야 한다.
#### 모듈이란? 
 - 단순한 정의로 소스 파일

### 징후1) 우발적 중복
- Employee라는 클래스 안에 calculatePay, reportHours, save 메서드가 3명의 액터를 책임진다.
- 즉, 단일 클래스에 세 액터가 결합되어 있다.
    - calculatePay() : 회계팀에서 기능을 정의하며, CFO 보고를 위해 사용한다.
    - reportHours() : 인사팀에서 기능을 정의하고 사용하며, COO 보고를 위해 사용한다.
    - save() : 데이터베이스 관리자가 정의

 
</br>
</br>

### 문제 상황!!
- -> regularHours() : 초과 근무를 제외한 업무 시간을 계산하는 알고리즘
    - calculatePay() 와 reportHours() 각각의 메소드에서 regularHours()를 사용
    - calculatePay()에서 업무시간 계산의 변경을 위해 regularHours()메소드를 변경하면, calculatePay도 영향이 간다.
    - 개발자가 regularHours()를 변경할 때, calculatePay()도 regularHours()를 호출하고 있는 것을 알고 있을까? -> 모른다.
    - 이 모든 것은 서로 다른 액터가 의존하는 코드를 너무 가까이 배치했기 때문. -> 코드 분리 필요

 </br>
 </br>

### 징후2) 병합
- 소스 파일에 다양하고 많은 메서드를 포함하면 병합이 자주 발생한다.
- 변경사항이 생겨 CTO개발자가 Employee ERD를 변경시키고, 이와 동시에 COO팀에서 보고서 format을 변경했다고 생각해보자.
- CTO팀과 COO팀은 원본 A를 pull 받아 변경하고, 적용하려 할텐데 변경사항들을 서로 충돌한다. 
- 이러한 병합에는 항상 위험이 따른다.  

 </br>
 </br>

### 해결책 
- 메서드를 각기 다른 클래스로 이동시키는 방식.
- 데이터와 메서드를 분리시키는 방식

- 위의 예시의 해결책으로, 간단한 데이터 구조인 EmployeeData 클래스를 만들어 세 개의 클래스가 공유하도록 하자.
- 세 클래스는 서로의 존재를 몰라야 한다. 따라서 "우연한 중복"을 피할 수 있다. 

- 반면, 이 해결책은 개발자가 세 가지 클래스를 인스턴스화하고 추적해야 한다는 게 단점이다.
- 이런 난관에서 빠져나오기 위해, 퍼사드 패턴 이 존재.
    - Facade클래스에 코드는 거의 없다.
    - 세 클래스의 객체를 생성하고, 요청된 메서드를 가지는 객체로 위임하는 일을 책임진다. 
++ Facade클래스는 내부 서브시스템의 내부를 알지 않아도 Facade클래스의 메소드를 호출하면 해당 API를 제공할 수 있도록 만들어주는 역할을 한다. (라이브러리의 역할이라고 보면 이해하기 쉽다.)


</br>
</br>