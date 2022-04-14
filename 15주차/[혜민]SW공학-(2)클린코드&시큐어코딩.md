# 15주차 CS 발표

## 클린코드 & 시큐어코딩

클린코드의 개념 및 규칙은 [여기](https://github.com/ssafycodingstudy/CS_study/blob/main/15%EC%A3%BC%EC%B0%A8/%5B%ED%98%9C%EB%AF%BC%5DSW%EA%B3%B5%ED%95%99-(1)%ED%81%B4%EB%A6%B0%EC%BD%94%EB%93%9C%26%EB%A6%AC%ED%8C%A9%ED%86%A0%EB%A7%81.md)에 대부분 작성했고, 작성되지 않은 규칙에 대해 이 글을 작성함



### 클린코드를 만들기 위한 규칙

#### 1. 꾸미기(Aesthetics)

- 규칙적인 들여쓰기와 줄바꿈
- 일관성있고 간결한 패턴을 적용해 줄바꿈함
- 메소드를 이용하여 중복 코드를 제거함
- 클래스 내에서도 여러 그룹으로 나누는 것이 읽기 좋음

#### 2. 흐름 제어 만들기(Making control flow easy to read)

- 왼쪽에는 변수를, 오른쪽에는 상수를 두고 비교

  ```java
  if(length >= 10)
  
  while(bytes_received < byteest_expected)
  ```

- 부정이 아닌 긍정을 다루기

  ```java
  if(a==b){ //a!=b는 부정
  	// same
  } else{
  	//different
  }
  ```

- if/else를 사용하며, 삼항 연산자는 매우 간단한 경우만 사용

- do/while 루프는 최대한 쓰지 않음



### 코드리뷰 & 리팩토링

> 레거시코드(테스트가 불가능하거나 어려운 코드)를 클린 코드로 만드는 방법
>
> 코드리뷰를 통해 냄새나는 코드를 발견하면, 리팩토링을 통해 점진적으로 개선해나감



#### 코드 인스펙션(code inspection)

작성한 개발 소스 코드를 분석하여 개발 표준에 위배되었거나 잘못 작성된 부분을 수정하는 작업



#### 코드 인스펙션 절차 과정

1. Planning : 계획 수립

2. Overview : 교육과 역할 정의

3. Preparation : 인스펙션을 위한 인터뷰, 산출물, 도구 준비

4. Meeting : 검토 회의로 각자 역할을 맡아 임무 수행

5. Rework : 발견한 결함을 수정하고 재검토 필요한지 여부 결정

6. Fellow-up : 보고된 결함 및 이슈가 수정되었는지 확인하고 시정조치 이행

   

#### 리팩토링 대상

- 메소드 정리 : 그룹으로 묶을 수 있는 코드, 수식을 메소드로 변경
- 객체 간의 기능 이동 : 메소드 기능에 따른 위치 변경, 클래스 기능을 명확히 구분
- 데이터 구성 : 캡슐화 기법을 적용해 데이터 접근 관리
- 조건문 단순화 : 조건 논리를 단순하고 명확하게 작성
- 메소드 호출 단순화 : 메소드 이름이나 목적이 맞지 않을 때 변경
- 클래스 및 메소드 일반화 : 동일 기능 메소드가 여러개 있으면 슈퍼클래스로 이동



#### 리팩토링 진행 방법

아키텍쳐 관점 시작 → 디자인 패턴 적용 → 단계적으로 하위 기능에 대한 변경으로 진행

의도하지 않은 기능 변경이나 버그 발생 대비해 회귀 테스트 진행

이클립스와 같은 IDE 도구로 이용



### 시큐어 코딩

> 안전한 소프트웨어를 개발하기 위해, 소스코드 등에 존재할 수 있는 잠재적인 보안 약점을 제거하는 것
>
> 서비스의 안정성과 신뢰성 확보를 위해 IT 시스템 개발 단계에서 주요 보안 취약점을 고려하여 소스코드 레벨에서 사전에 제거하여 안전한 SW를 개발하는 기법
>
> 즉, 해킹의 기법들을 시도조차 못하게 만드는 코딩 기법



#### 시큐어 코딩의 필요성

<img src="https://t1.daumcdn.net/cfile/tistory/2517944D590808C532" alt="img" style="zoom:75%;" />

- 미국표준기술연구소(NIST)에서는 제품출시 단계에 발견되는 결함을 제거하기 위해 30배의 비용이 요구된다고 발표함



#### 시큐어코딩을 위한 개발 보안 방법론

- 시큐어 코딩을 위한 개발 방법론이 완전히 별도로 있는 것은 아니고, 기존의 SW 개발 생명주기(SDLC)에서 보안 활동을 추가하는 것

- 기존의 SDLC는 폭포수모델(Waterfall)이나 프로토타입(Prototype), 나선형모델(Spiral) 같은 것들이 존재

  - 요구사항 분석 단계
    - 요구사항 중 보안 항목 식별
    - 요구사항 명세서
  - 설계
    - 위협원 도출을 위한 위협 모델링
    - 보안설계 검토 및 보안설계서 작성
    - 보안 통제 수립
  - 구현
    - 표준 코딩 정의서 및 SW 개발 보안 가이드를 준수해 개발
    - 소스코드 보안 약점 진단 및 개선
  - 테스트
    - 모의 침투 테스트 또는 동적 분석을 통한 보안취약점 진단 및 개선
  - 유지보수
    - 지속적인 개선
    - 보안패치

- 개발 보안 방법론 사례

  - MS-SDL(MicroSoft - Secure Development Lifecycle)

  - Seven Touchpoints

  - CLASP(Comprehensive, Lightweight Application Security Process)

  - CWE(Common Weakness Enumeration) Seven Pernicious Kingdoms

    

#### 보안 약점을 노려 발생하는 사고사례들

- SQL 인젝션 취약점으로 개인유출 사고 발생
- URL 파라미터 조작 개인정보 노출
- 무작위 대입공격 기프트카드 정보 유출



#### SQL 인젝션 예시

- 안전하지 않은 코드

  ```java
  String query "SELECT * FROM users WHERE userid = '" + userid + "'" + "AND password = '" + password + "'";
  
  Statement stmt = connection.createStatement();
  ResultSet rs = stmt.executeQuery(query);
  ```

- 안전한 코드

  ```java
  String query "SELECT * FROM users WHERE userid = ? + "AND password = ?";
  
  PrepareStatement stmt = connection.prepareStatement(query);
  stmt.setString(1, userid);
  stmt.setString(2, password);
  ResultSet rs = stmt.executeQuery();
  ```

> 적절한 검증 작업이 수행되어야 안전함
>
> 입력받는 값의 변수를 `$`대신 `#`을 사용하면서 바인딩 처리로 시큐어 코딩이 가능함



---

[참고 1] : <https://ybdeveloper.tistory.com/75>

[참고 2] : <https://gyoogle.dev/blog/computer-science/software-engineering/Clean%20Code%20&%20Secure%20Coding.html>

[참고 3] : <https://needjarvis.tistory.com/174>
