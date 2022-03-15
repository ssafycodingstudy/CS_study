# 12주차 CS 발표

## 정규화 (Normalization)

### 정규화란?

정규화의 기본 목표는 **테이블 간에 중복된 데이터를 허용하지 않는다는 것**

중복된 데이터를 허용하지 않음으로써 **무결성(Integrity)를 유지할 수 있으며, DB의 저장 용량을 줄일 수 있음**



### 목적

#### 1. 불필요한 데이터를 제거, 데이터의 중복을 최소화

   - 테이블간에 FK(외래키)로 PK(기본키)를 연결해 사용하면 디스크 공간을 훨씬 효율적으로 사용할 수 있음

#### 2. 데이터베이스 구조 확장 시 재디자인을 최소화

   - 정규화된 데이터베이스 구조에서는 테이블에 새로운 데이터 형의 추가로 인한 확장시, 그 구조를 변경하지 않아도 되거나 일부만 변경해도 되는 경우가 있음 → 데이터베이스와 연동된 응용 프로그램에 최소한의 영향 + 효율적인 사용 가능

#### 3. 다양한 관점에서의 query를 지원하기 위함

   - 1NF와 관련
   - 불필요한 정보를 제외하며, Join을 통해서 원하는 정보를 가져올 수 있음

#### 4. 무결성 제약 조건의 시행을 간단하게 하기 위함

   - 무결성 제약조건이란 데이터베이스의 정확성, 일관성을 보장하기 위해 저장, 삭제, 수정 등을 제약하기 위한 조건
   - 데이터의 무결성을 유지하는 것은 데이터베이스 관리시스템 (DBMS)의 중요한 기능이며, 주로 데이터에 적용되는 연산에 제한을 두어 데이터의 무결성을 유지하게 됨

#### 5. 각종 이상 현상(Anomaly)을 방지하기 위해서, 테이블의 구성을 논리적이고 직관적으로 함





### 정규화 과정

<img src="https://media.vlpt.us/images/bsjp400/post/140f510e-26ed-4807-a517-bc0b9a902c69/image.png" alt="img" style="zoom:50%;" />

보통 1NF ~ 3NF까지 진행하거나 BCNF 단계까지 진행함



#### 1NF (부분적 함수 종속 제거)

 테이블 컬럼이 원자값(하나의 값)을 갖도록 테이블을 분리시키는 것을 말함

**만족해야할 조건**

	1. 어떤 릴레이션에 속한 모든 Domain이 원자값만으로 되어 있어야 함
	2. 모든 속성에 반복되는 그룹(repeating group)이 나타나지 않아야 함
	3. 기본키를 사용하여 관련 데이터의 각 집합을 고유하게 식별할 수 있어야 함

<img src="https://blog.kakaocdn.net/dn/bNbQUm/btqT18yag04/pTXJX3wB23ouk8az7EgWQ1/img.png" alt="img" style="zoom: 67%;" />

추신수와 박세리는 여러 개의 취미를 가지고 있기 때문에 제 1 정규형을 만족하지 못함.

제 1 정규화하여 분해할 수 있음

<img src="https://blog.kakaocdn.net/dn/bMlNZj/btqT17FWVot/jUKTAUyOdrH83pRraKw3K0/img.png" alt="img" style="zoom: 67%;" />



#### 2NF (부분적 함수 종속 제거)

제 1 정규화를 진행한 테이블에 대해 완전 함수 종속을 만족하도록 테이블을 분해하는 것

기본키의 부분 집합이 결정자가 되어선 안된다는 것을 의미

> - 함수적 종속 
>
>   X의 값에 따라 Y값이 결정될 때 X → Y로 표현하는데, 이를 **Y는 X에 대해 함수적 종속**이라고 함
>
>   ex) 학번(X)을 알면 이름(Y)을 알 수 있음
>
>   **X를 결정자, Y를 종속자**라고 함. X과 바뀌었을 경우 Y가 바뀌어야만 함을 의미
>
> - 함수적 종속에서 X의 값이 여러 요소일 경우, 즉, {X1, X2} → Y일 경우, X1와 X2가 Y의 값을 결정할 때 이를 **완전 함수적 종속** 이라고 하고, X1, X2 중 하나만 Y의 값을 결정할 때 이를 **부분 함수적 종속** 이라고 함



![img](https://blog.kakaocdn.net/dn/ylbaZ/btqT8Jc4K3s/0VFTPoKKFkbxZghKWDwKo1/img.png)

기본키는 (학생번호, 강좌이름)으로 복합키임. 

(학생번호, 강좌이름)인 기본키는 성적을 결정함 `{학생번호, 강좌이름} → 성적`

강의실이라는 컬럼은 기본키의 부분집합인 강좌이름에 의해 결정될 수 있음 `강좌이름 → 강의실 ` 

즉, 기본키 {학생번호, 강좌이름}의 부분키인 `강좌이름` 이 결정자이기 때문에 위의 테이블의 경우 기존의 테이블에서 강의실을 분해하여 별도의 테이블로 관리하여 제2 정규형을 만족시킬 수 있음

<img src="https://blog.kakaocdn.net/dn/bluCnc/btqT7VEOf04/Me8DfY7rtycgJPYlYQKEWK/img.png" alt="img" style="zoom: 67%;" />



#### 3NF (이행적 함수 종속 제거)

제 2 정규화를 진행한 테이블에 대해 이행적 종속을 없애도록 테이블을 분해하는 것

> **이행적 종속**
>
> A → B, B → C가 성립할 때 A → C가 성립되는것을 의미

**만족해야할 조건**

```
1. 릴레이션이 제 2정규화 되어야함
2. 기본키가 아닌 속성들은 기본키에만 의존해야함
```



<img src="https://blog.kakaocdn.net/dn/enwN1N/btqUeiMyErd/sP8NKCe70NKsZncGuhO9uK/img.png" alt="img" style="zoom:67%;" />

학생번호는 강좌이름을 결정하고 `학생번호 → 강좌이름 `, 강좌이름은 수강료를 결정하고있음 `강좌이름 → 수강료`

그렇기 때문에 {학생번호, 강좌이름} 테이블과 {강좌이름, 수강료} 테이블로 분해해야함

> **이행적 종속을 제거하는 이유**
>
> 예를 들어 501번 학생이 수강하는 강좌가 스포츠경영학으로 변경되었다고 가정.
>
> 이행적 종속이 존재한다면 501번 학생은 스포츠 경영학이라는 수업을 20000원의 수강료로 듣게 됨
>
> 강좌이름에 맞게 수강료를 다시 변경할 수 있지만, 이런 번거로움을 해결하기 위해 제 3 정규화를 함

<img src="https://blog.kakaocdn.net/dn/ci1le3/btqUeXnPnpD/yKkURqr8cZl21f5erx42QK/img.png" alt="img" style="zoom:67%;" />



#### BCNF (Boyce-Codd Normal Form) 정규형

제 3 정규화를 진행한 테이블에 대해 모든 결정자가 후보키가 되도록 테이블을 분해하는 것

<img src="https://blog.kakaocdn.net/dn/bBN6xu/btqT6IlqRF4/MvBoxYMxtgS1JT7t1AymnK/img.png" alt="img" style="zoom:67%;" />

특수수강 테이블에서 키본키는 `{학생번호, 특강이름}`

기본키 {학생번호, 특강이름}는 교수를 결정함. 

또한 교수는 특강 이름을 결정하고 있음

문제는 교수가 특강이름을 결정하는 결정자지만, 후보키가 아님.

그렇기때문에 BCNF 정규화를 만족시키기 위해 특강신청 테이블과 특강 교수 테이블로 분해할 수 있음

<img src="https://blog.kakaocdn.net/dn/3cbHr/btq3mNylPan/c6b2lBuH4OkdDNmrzGHWUk/img.png" alt="img" style="zoom:67%;" />

 
---

[참고 1] : <https://mangkyu.tistory.com/110>

[참고 2] : <https://gyoogle.dev/blog/computer-science/data-base/Normalization.html>

[참고 3] : <https://velog.io/@bsjp400/Database-DB-%EC%A0%95%EA%B7%9C%ED%99%94-%EB%B9%84%EC%A0%95%EA%B7%9C%ED%99%94%EB%9E%80
