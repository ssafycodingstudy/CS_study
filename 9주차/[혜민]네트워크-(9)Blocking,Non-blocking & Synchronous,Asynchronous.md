# 9주차 CS 발표

## Blocking,Non-blocking & Synchronous,Asynchronous

### 동기(Synchronous) vs 비동기(Asynchronous)

> 호출되는 함수의 **작업 완료 여부를 누가 신경쓰느냐**가 관심사

동기/비동기는 일을 수행중인 `동시성`에 주목

- **Synchronous** : 함수 A는 함수 B가 일을 하는 중에 기다리면서, 현재 상태가 어떤지 계속 체크함
- **Asynchronous** : 함수 B의 수행 상태를 B 혼자 직접 신경쓰면서 처리함 (callback)

즉, 호출된 함수(B)를 호출한 함수(A)가 신경쓰는지, 호출된 함수(B) 스스로 신경쓰는지를 동기/비동기라고 생각하면 됨



#### 동기(Synchronouse)

![img](https://media.vlpt.us/images/wonhee010/post/dd6c1aab-eeec-453d-b43c-5f3dc9db9b4c/image.png)

- `Thread1`이 작업을 시작시키고, `Task1`이 끝날때까지 기다렸다가 `Task2` 를 시작함
- 작업 요청을 했을 때 요청의 **결과값(return)을 직접 받는 것**
- 요청의 결과값이 return 값과 동일함
- **호출한 함수가 작업 완료를 신경씀**

#### 비동기(Asynchronous)

![img](https://media.vlpt.us/images/wonhee010/post/132a4f9a-4468-4bce-9f49-374b2cb337bc/image.png)

- `Thread1`이 작업을 시작시키고, 완료를 기다리지 않고 `Thread1`은 다른 일을 처리할 수 있음
- 작업 요청을 했을 때 **요청의 결과값(return)을 간접적으로 받음**
- 요청의 결과값이 return값과 다를 수 있음
- 해당 요청 작업은 별도의 스레드에서 실행하게 됨
- 콜백을 통한 처리가 비동기 처리라고 할 수 있음
- 호출시 Callback을 전달하여 작업의 완료 여부를 호출한 함수에게 답하게 됨 (Callback이 오기 전까지 호출한 함수는 신경쓰지 않고 다른 일을 할 수 있음)
- **호출된 함수(Callback 함수)가 작업 완료를 신경씀**



---

### Blocking vs Non-blocking

> 호출되는 함수가 **바로 return 하느냐 마느냐**가 관심사

blocking과 non-blocking은 주로 IO의 읽기, 쓰기에서 사용됨

`호출된 함수(B)`가 `호출한 함수(A)`에게 제어권을 건네주는 유무의 차이

B가 호출되면서 B는 자신의 일을 진행해야함 (제어권이 B에게 주어진 상황)

#### Blocking

- 함수 B는 내 할일을 다 마칠때까지 제어권을 가지고 있음. A는 B가 다 마칠 때까지 기다려야함

- 요청한 작업을 마칠 때까지 계속 대기함
- 즉시 리턴하지 않음 (일을 못하게 막음)
- return 값을 받아야 끝남
- Thread 관점으로 본다면, 요청한 작업을 마칠 때까지 계속 대기하며 return 값을 받을 때까지 한 Thread를 계속 사용/대기함

#### Non-blocking

- 함수 B는 할일을 마치지 않았어도 A에게 제어권을 바로 넘겨줌. A는 B를 기다리면서도 다른 일을 진행할 수 있음

- 즉시 리턴함
- Thread 관점으로 본다면, 하나의 Thread가 여러 개의 IO를 처리할 수 있음



---

### 동기/비동기, blocking/non-blocking의 조합

![img](https://camo.githubusercontent.com/b7b28ae739c50d5ed8a4594f52f24e671aeeae234befd0991e5230561ba303bf/68747470733a2f2f696d67312e6461756d63646e2e6e65742f7468756d622f523132383078302f3f73636f64653d6d746973746f72793226666e616d653d6874747073253341253246253246626c6f672e6b616b616f63646e2e6e6574253246646e25324664613530597a2532466274713044736a65345a562532466c47653848386e5a676442646746766f3749637a5330253246696d672e706e67)

#### Sync + Blocking

결과가 처리되어 나올때까지 기다렸다가 return 값으로 결과를 전달함

#### Async + Non-Blocking 

작업 요청을 받아서 별도의 프로세서에서 진행하고 바로 return(작업 끝) 함.

결과는 별도의 작업 후 간접적으로 전달(callback) 함.

#### Sync + Non-Blocking

결과가 없다면 바로 return함

결과가 있으면 바로 결과를 return함

(결과가 생길 때까지 계속 완료되었는지 확인)

#### Async + Blocking

호출되는 함수가 바로 return하지 않고, 호출하는 함수는 작업 완료 여부를 신경쓰지 않음

(이 조합은 사실 이점이 없어서 일부러 이 방식을 사용하지 않는다고 함)

(의도하지 않게 이 조합으로 동작하는 경우가 있다고 함. 이는 non-blocking+Async를 추구하다가 의도가 변질되어버린 경우. 대표적으로 Node.js + MySQL 조합이라고 함)



#### 예시

> 상황 : 치킨집에 직접 치킨을 사러감

- Sync + Blocking

  ```
  나 : 사장님 치킨 한마리만 포장해주세요
  사장님 : 네 금방되니까 잠시만요!
  나 : 넹
  -- 사장님 치킨 튀기는 중--
  나 : (아 언제 되지?..궁금한데 그냥 멀뚱히 서서 치킨 튀기는거 보면서 기다림)
  ```

- Async + Blocking

  ```
  나 : 사장님 치킨 한마리만 포장해주세요
  사장님 : 네 금방되니까 잠시만요!
  나 : 앗 넹
  -- 사장님 치킨 튀기는 중--
  나 : (언제 되는지 안 궁금함, 잠시만이래서 다 될때까지 서서 붙잡힌 상황)
  ```
  
- Sync + Non-Blocking

  ```
  나 : 사장님 치킨 한마리만 포장해주세요
  사장님 : 네~ 주문 밀려서 시간 좀 걸리니까 볼일 보시다 오세요
  나 : 넹
  -- 사장님 치킨 튀기는 중--
  (5분뒤) 나 : 제꺼 나왔나요?
  사장님 : 아직이요
  (10분뒤) 나 : 제꺼 나왔나요?
  사장님 : 아직이요ㅠ
  (15분뒤) 나 : 제꺼 나왔나요?
  사장님 : 아직이요ㅠㅠ
  ```

- Async + Non-Blocking

  ```
  나 : 사장님 치킨 한마리만 포장해주세요
  사장님 : 네~ 주문 밀려서 시간 좀 걸리니까 볼일 보시다 오세요
  나 : 넹
  -- 사장님 치킨 튀기는 중--
  나 : (앉아서 다른 일 하는 중)
  ...
  사장님 : 치킨 나왔습니다
  나 : 잘먹겠습니다~
  ```

  

---

[참고 1] : <https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking>

[참고 2] : <https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/%5BNetwork%5D%20Blocking%2CNon-blocking%20%26%20Synchronous%2CAsynchronous.md>
