# 14주차 CS 발표

## 저장 프로시저 (Stored PROCEDURE)

### 저장 프로시저(SP)란?

SP(Stored Procedure)는 **일련의 쿼리를 마치 하나의 함수처럼 실행** 하기 위한 쿼리의 집합. 쉽게 말해, 특정 로직의 쿼리를 함수로 만들어 놓은 것

페이징 쿼리와 같이 자주 사용되는 쿼리가 있다면 함수로 한 번 만들어놓고 사용하게 되면 성능 상으로나 코드 재사용성 등에 이점이 있을 것임.



### 저장 프로시저의 장점

1. 하나의 요청으로 여러 SQL문을 실행할 수 있음 

2. 네트워크 소요 시간을 줄일 수 있음

   만약 동일한 쿼리를 1000번 2000번 호출하는 것보다 SP를 이용해서 구현한다면 SP를 호출할 때 한 번만 네트워크를 경유하기 때문에 네트워크 소요시간을 줄이고 성능을 개선할 수 있음

3. SQL을 구문 분석하고 실행 가능한 코드로 변환하는 데에 드는 비용을 줄일 수 있음

   같은 쿼리를 매번 쓸 때마다 옵티마이저가 구문을 분석하고 실행 가능한 코드로 바꿀려면 많은 비용이 드는데, 이 비용을 없앨 수 있음

   프로시저의 최초 실행 시 최적화 상태로 컴파일 되며, 그 이후 프로시저 캐시에 저장됨

   만약 **똑같은 쿼리가 여러 번 사용된다면, 다시 컴파일 작업을 거치지 않고 캐시에서 가져오기 때문에** 저장 프로시저로 만들어 놓고 사용하는게 좋음

4. 개발 언어에 비의존적임

   MS-SQL을 사용하고 있고, MS-SQL에 맞는 쿼리문에 대해 저장 프로시저로 만들어놨다고 가정.

   이 때 MS-SQL → MySQL로 마이그레이션 해야한다면 SP 안의 로직만 MySQL에 맞춰 바꾸면 되고, 호출하는 SP명은 그대로이기 때문에 **해당 SP를 호출하는 서버 코드는 변경하지 않아도 됨**

5. 확장 및 유지보수가 수월해짐

   위 3번과 같은 이유로 언제든지 **부담없이 저장 프로시저의 내용을 수정할 수 있기 때문에**, 쿼리의 변경이 발생할 시 저장 프로시저만 변경해주면 됨

6. 개발 업무를 구분해 개발할 수 있음

   순수한 애플리케이션만 개발하는 조직과 DBMS 관련 코드를 개발하는 조직이 따로 있다면, DBMS 개발하는 조직에서는 데이터베이스 관련 처리하는 SP를 만들어 API처럼 제공하고 , 애플리케이션 개발자는 SP를 호출해서 사용하는 형식으로 역할을 구분하여 개발이 가능함

7. 트래픽 감소

   클라이언트가 직접 SQL문을 작성하지 않고, 프로시저명에 매개변수만 담아 전달하면 됨. 즉, SQL문이 서버에 이미 저장되어있기 때문에 클라이언트와 서버 간 네트워크 상 트래픽이 감소됨

8. 보안

   프로시저 내에서 참조 중인 테이블의 접근을 막을 수 있음



### 저장 프로시저의 단점

1. 호환성

   구문 규칙이 SQL / PSM 표준과의 호환성이 낮기 때문에 코드 자산으로의 재사용성이 나쁨

2. 처리 성능이 낮음

   문자나 숫자 연산에 저장 프로시저를 사용한다면 오히려 C나 Java보다 느린 성능을 보여줌

3. 디버깅이 어려움

   에러가 발생했을 때, 어디서 잘못됐는지 디버깅하는 것이 힘들 수 있음

4. DB 확장이 매우 힘듦

   서비스 사용자가 많아져 서버수를 늘려야할 때, DB 수를 늘리는 것이 더 어려움

   서비스 확장을 위해 서버수를 늘릴 경우 DB 수를 늘리는 것보다 WAS의 수를 늘리는 것이 더 효율적이기 때문에 대부분의 개발에서 DB에는 최소의 부담만 주고 대부분의 로직은 WAS에서 처리할 수 있게 함



### MySQL 저장 프로시저 생성 및 호출

#### 저장 프로시저 생성

ex) 고객 테이블에서 고객이름 순으로 조회한 정보를 저장 프로시저로 생성

```mysql
DELIMITER $$ 
CREATE PROCEDURE GetCustomers() 
BEGIN 
	SELECT customerName, city, state, postalCode, country 
    FROM customers 
    ORDER BY customerName; 
END $$ 
DELIMITER ;
```

> **DELIMITER**
>
> 저장 프로시저 내부에서 사용하는 SQL문은 일반 SQL문이기 때문에 세미콜론(;)으로 문장을 끝맺어야함
>
> 이 때, 저장 프로시저 작성이 완료되지 않았음에도 SQL문이 실행되는 위험을 막기 위해 구분자(;)를 다른 구문자로 바꿔줘야하는데 이 때 사용하는 명령어가 DELIMITER
>
> ⇒ 따라서 저장 프로시저 생성 전에 구문자(DELIMITER)를 $$으로 바꿔주고 프로시저 작성이 끝났을 때 END $$로 저장 프로시저의 끝을 알려줌. 
>
> 마지막으로 구분자를 원래대로 되돌리기 위해 구분자 (DELIMITER)를 세미콜론 (;)으로 바꿔줌

#### 저장 프로시저 호출

```mysql
CALL GetCustomers();
```

저장 프로시저를 활용하면 일일이 작성하지 않아도 함수처럼 사용하여 손쉽게 쿼리문과 동일한 결과를 조회할 수 있음

> 저장 프로시저를 호출하면, MySQL은 데이터베이스 카달로그에서 프로시저이름을 찾아 명령코드(SQL문)를 컴파일하고 메모리 공간(cache)에 저장하고, 프로시저를 실행시킴
>
> 그리고 같은 세션에서 동일한 저장 프로시저를 한 번 더 호출하면, MySQL은 컴파일 과정을 다시 커치지 않고 기존의 저장 프로시저를 캐시(cache) 에서 불러옴



저장 프로시저는 위 예시처럼 단순 select문으로 작성할 수도 있지만, 매개변수(파라미터)에 개별 value값을 넣어 원하는결과를 조죄할 수도 있음

또한, IF, CASE, 그리고 LOOP 같은 제어 흐름 문장을 사용하여 보다 향상된 SQL 코드문 작성도 가능하며 프로시저 내에서 다른 저장프로시저를 호출하여 사용할 수도 있음



### 활용

#### 저장 프로시저 생성

ex) 답변 테이블에서 원본글인지 답변글인지를 판별하고, 답변 여부에 따라 삭제여부 UPDATE 혹은 DELETE 처리

```mysql
DELIMITER $$ 
DROP PROCEDURE IF EXISTS deleteReboard $$ #같은 이름이 있다면 지우기
CREATE PROCEDURE deleteReboard #저장 프로시저 생성 
( 
	#변수 선언 
    m_no INT, 
    m_step INT, 
    m_groupNo INT 
) 
BEGIN #SQL 프로그래밍 부분 시작 
DECLARE cnt INT; #답변 변수 설정 
SET cnt=0; #변수 초기화 
/*답변이 달린 원본 글인 경우에는 삭제하지 말고 delFlag를 Y 로 update하자*/ 
IF m_step=0 THEN /*원본글인 경우*/ 
  /*답변이 달렸는지 확인*/ 
  SELECT COUNT(*) INTO cnt FROM reboard WHERE groupno=m_groupNo; 
  IF cnt >1 THEN /*답변이 달린 경우*/ 
    UPDATE reboard SET delflag='Y' WHERE NO=m_no; 
  ELSE /*답변이 안 달린 경우*/ 
      DELETE FROM reboard WHERE NO=m_no; 
  END IF; 
ELSE /*답변글인 경우*/ 
	DELETE FROM reboard WHERE NO=m_no; 
END IF; 
END$$ 
DELIMITER ;
```

#### 저장 프로시저 호출

변수는 반드시 프로시저에서 선언한 순서대로 입력해야함

> m_no, m_step, m_groupNo 순서

```mysql
CALL deleteReboard(4, 0, 4);
```

#### 프로시저 내용 확인

```mysql
SHOW CREATE PROCEDURE 저장_프로시저_이름;
```

#### 저장 프로시저 삭제

```mysql
DROP PROCEDURE 저장_프로시저_이름;
```



---

[참고 1] : <https://pangtrue.tistory.com/196>

[참고 2] : <https://runcoding.tistory.com/31>

[참고 3] : <https://heestory217.tistory.com/18>

[참고 4] : <https://gyoogle.dev/blog/computer-science/data-base/Stored%20PROCEDURE.html>
