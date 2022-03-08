# 11주차 CS 발표

## SQL Injection

### SQL Injection이란

SQL 인젝션은 웹 사이트의 보안상 허점을 이용해 특정 SQL 쿼리문을 전송하여 공격자가 원하는 데이터베이스의 중요한 정보를 가져오는 해킹 기법을 말함

대부분 클라이언트가 입력한 데이터를 제대로 필터링하지 못하는 경우에 발생함. 공격의 쉬운 난이도에 비해 피해가 상당하기 때문에 보안 위협 1순위로 불릴 만큼 중요한 기법



<img width="696" alt="image" src="https://user-images.githubusercontent.com/26523814/157253701-0820e80f-327b-4d5e-b5dc-57d1bf623400.png">

#### 진행 시나리오

- 각 클라이언트(회원)들은 자격증 번호를 조회할 수 있는 시스템 
- 기본적인 SQL 진행은 anjinma라는 클라이언트가 '자격증 번호 조회'를 클릭하여 anjinma라는 이름이 웹서버에 들어가고 DB에 입력한 값이 있는지 확인 후 존재한다면 자격증 DB를 출력해줌
- 여기서 이름이 blackhat이라는 클라이언트가 자신의 blackhat을 조회해야하는데 SQL 구문을 변경하여 anjinma라는 클라이언트의 자격증 번호를 조회함
- 예를들어, '자격증 번호 조회'를 클릭하면 url이 'http://license12345.com/mysearch?=anjinma' 가 된다고 가정
- 그럼 공격자의 조회는 'http://license12345.com/mysearch?=blackhat'이 됨
- 여기서 공격자는 뒤에 blackhat을 anjinma로 변경해야하는데 그냥 변경으로는 현재 로그인 된 자신과 다르기때문에 웹서버에서 인정하지 않음
- 그러므로 특정 쿼리문을 넣어줌. **anjinma' or '1'='1** 을 넣게 되면 뒤 구문 1과 1은 같다는 참이므로 or 진행으로 해당 구문은 참이 되어 결과를 출력해줌



#### SQL 인젝션의 공격 범위

- bypass
- data access
- content change
- db delete



### SQL Injection의 종류와 공격 방법

1. **Error based SQL Injection** : 논리적 에러를 이용한 SQL Injection

   앞서 예를 들어 설명한 기법. 해당 기법으로 SQL 인젝션에 대한 기본을 설명하는게 일반적

   - 로그인 예

     ```mysql
     select * from client where name 'anjinma' and password='12345'
     ```

     → 

     ```mysql
     select * from client where name 'anjinma' and password='' or '1'='1'
     ```

     ' or '1'='1을 넣어서 1과 1이 같으면 참이므로 1=1 참. or은 앞의 값과 뒤의 값 중 하나라도 참이면 참이므로 이 구문은 참이 되어 로그인에 성공하게 됨

2. **UNION based SQL Injection** : UNION 명령어를 이용한 SQL Injection

   - SQL UNION이란?

     여러개의 SQL문을 합쳐 하나의 SQL문으로 만들어주는 방법

     UNION과 UNION All로 나뉘는데 중복값 제외 여부의 차이

     UNION - 중복값 제외, UNION All - 중복값 제외하지 않고 전체를 합침

   - UNION으로 합쳐지는 두 테이블은 **컬럼 갯수가 일치**해야만 오류가 나지 않음 

     데이터 타입도 일치해야함 → 데이터타입에 상관없이 사용하기 위해 NULL을 사용하는 경우도 있음

   - 외부 입력

     ID: test' UNION SELECT 1,1 --

     PW: anything

   - 실행 쿼리

     ```mysql
     SELECT * FROM users WHERE ID='test' UNION SELECT 1,1 -- and PW='anything'
     ```

     - -- 뒤는 주석으로 처리

   실행 쿼리대로 하면 users 테이블에 등록된 id와 pw 목록을 전부 조회할 수 있게 됨

   [실행예제](https://webstone.tistory.com/25)

3. **Blind SQL Injection - Boolean based Blind SQL Injection**

   평범한 SQL 삽입과 같이 원하는 데이터를 가져올 쿼리를 삽입하는 기술

   - 웹 SQL 삽입에 취약하나 데이터베이스 메시지가 공격자에게 보이지 않을 때 사용

   - 평범한 SQL 삽입과 다르게 참과 거짓, 쿼리가 참일때와 거짓일 때의 서버의 반응만으로 데이터를 얻어내는 기술. 즉, 쿼리를 삽입했을 때 쿼리의 참과 거짓에 대한 반응을 구분할 수 있을 때에 사용되는 기술

   - Blind SQL 삽입은 위 두 함수를 이용하여 쿼리의 결과를 얻어, 한글자씩 끊어온 값을 아스키코드로 변환시키고 임의의 숫자와 비교하여 참과 거짓을 비교하는 과정을 거쳐가며 계속 질의를 보내어 일치하는 아스키코드를 찾아냄. 그 과정을 반복하여 결과들을 조합하여 원하는 정보를 얻어냄으로써 공격을 이루어지게함

4. **Blind SQL Injecion - Time based SQL**

   어떤 경우에는 응답의 결과가 항상 동일하여 해당 결과만으로 참과 거짓을 판별할 수 없는 경우가 있을 수 있음

   이런 경우 시간을 지연시키는 쿼리를 주입하여 응답 시간의 차이로 참과 거짓 여부를 판별할 수 있음

   - sleep 함수
     - `sleep(초)`
     - 조건의 참인 SQL 쿼리의 응답 시간을 지연시키는 함수
     - 참 → 지정된 시간마늠 지연시킨 후 결과 반환
     - 거짓 → 즉시 결과 반환
     - 네트워크의 통신 상태에 따라 결과가 부정확 할 수 있음

   ```bash
   mysql> select 1;
   ...
   1 row in set (0.00 sec)
   
   mysql> select sleep(1);	# <--- 참이므로 1초 지연 후 반환
   ...
   1 row in set (1.01 sec)
   
   mysql> select sleep(2); # <--- 참이므로 2초 지연 후 반환
   ...
   1 row in set (2.00 sec)
   ```

   - sleep 함수 사용 예

     - and 연산자를 사용하는 경우 **앞의 조건이 참이면 뒤의 내용 실행**
     - 앞의 조건이 거짓이면 뒤의 내용 실행하지 않음(Short Cut)

     ```bash
     # 참일 때
     mysql> select 1 and sleep(2);
     +----------------+
     | 1 and sleep(2) |
     +----------------+
     |              0 |
     +----------------+
     1 row in set (2.01 sec) # <--- 참이기때문에 2초 지연 후 반환
     
     
     # 거짓일 때
     mysql> select 1 or sleep(2);
     +---------------+
     | 1 or sleep(2) |
     +---------------+
     |             1 |
     +---------------+
     1 row in set (0.00 sec) # <--- 거짓이기때문에 바로 반환
     ```




### SQL Injection 대응 방안

#### 1) input 값을 받을 때, 특수문자 여부 검사하기

로그인 전, 검증 로직을 추가하여 미리 설정한 특수문자들이 들어왔을 때 요청을 막아냄

MySQL의 경우 mysqli_real_escape_string() 함수를 사용

#### 2) SQL 서버 오류 발생 시, 해당하는 에러 메시지 감추기

view를 활용하여 원본 데이터베이스 테이블에는 접근 권한을 높인다. 일반 사용자는 view로만 접근하여 에러를 볼 수 없도록 만듦

#### 3) preparedstatement 사용하기

preparedstatement를 사용하면, 특수문자를 자동으로 escaping 해줌 (statement와는 다르게 쿼리문에서 전달인자 값을 `?`로 받는 것) 

이를 활용해 서버 측에서 필터링 과정을 통해서 공격을 방어함

```mysql
# statement 사용
String sql= "insert into emp(empno, ename, sal) values("+empno+",'"+ename+"',"+sal+")";
st = con.createStatement();

# preparedstatement 사용
String sql= "insert into emp(empno, ename, sal) values(?,?,?)";
ps = con.prepareStatement(sql);
ps.setInt(1, empno);
ps.setString(2, ename);
ps.setDouble(3, sal);
```





---

[참고 1] : <https://m.blog.naver.com/lstarrlodyl/221837243294>

[참고 2] : <https://girrr.tistory.com/94>

[참고 3] : <https://gyoogle.dev/blog/computer-science/data-base/SQL%20Injection.html>

[참고 4] : <https://m.blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=javaking75&logNo=140162466611&proxyReferer=>



