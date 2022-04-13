# 15주차 CS 발표

## 클린코드 & 리팩토링

### 클린코드란?

클린코드는 가독성이 높은 코드를 말함

가독성을 높이기 위해선 다음과 같이 구현해야함

- 네이밍이 잘 되어야함

- 오류가 없어야함

- 중복이 없어야함

- 의존성을 최대한 줄여야함

- 클래스 혹은 메소드가 한가지 일만 처리해야함

  

> 얼마나 코드가 잘 읽히는지, 코드가 지저분하지 않고 정리된 코드인지를 나타내는 것이 바로 **클린코드** 

```java
public int AAA(int a, int b){
	return a+b;
}public int BBB(int a, int b){
	return a-b
}
```

위 코드에는 두가지 문제점이 있음 

1. **함수 네이밍** : 다른 사람들이 봐도 무슨 역할을 하는 함수인 지 알 수 있는 이름을 사용해야 함
2. **함수와 함수 사이의 간격** : 여러 함수가 존재할 때 간격을 나누지 않으면 시작과 끝을 구분하는 것이 매우 힘듦



개선한 것이 아래의 예시

```java
public int sum(int a, int b){
	return a+b;
}
public int sub(int a, int b){
	return a-b
}
```



### 클린코드 구현방법

#### 변수 이름 작성하기

##### 1. 의미있게 구분하기

​	구분 짓지 못하는 변수명은 의미가 없음. 예를 들어 ProductInfo와 ProductData의 개념은 분리되는가?

​	또 변수 뒤에 타임을 작성하는 것도 의미가 없다면 좋지 않음. NameString과 Name의 다른점은 무엇인가?

##### 2. 한 개념에 한 단어만 사용하기

​	Controller, Manager, driver와 같이 의미가 비슷한 단어들은 혼란을 유발함. 어휘를 일관성있게 작성해야함

##### 3. 의미있는 맥락을 추가하기

​	예를 들어 사용자의 주소를 나타내는 변수로 state라는 이름을 지었다면, state만 봐서는 이게 주소인지, 상태인지 의미를 유추하기 힘듦

​	이 때, state와 함께 선언되는 다른 변수들의 이름이 주소와 연관된 단어라면 state의 옳은 뜻을 쉽게 유추할 수 있음



#### 함수 작성하기

##### 1. 작게 만들기

​	어떤 것도 최대한 작고 간결하게 만들기

```java
if(isTestPage(pageData)){
  ...
}

private boolean isTestPage(Page pageData){
  return pageData.size > 0;
}
```

​	가정문 하나도 이렇게 쓰이니 읽기도 좋고 테스트하기도 쉬워짐. 그렇다고 만들기 어려운 것도 아님

##### 2. 객체의 상태를 변경한 후 출력하는 함수를 만들지 않기

​	객체의 변경은 객체 내부에서만 하는 것. 다른 객체에게 변경이나 생성을 맡기려 하지 않기

```java
public class MovieRepository { 
  private static List<Movie> movies = new ArrayList<>(); 
  
  static { 
    Movie movie1 = new Movie(1, "생일", 8_000); 
    movie1.addPlaySchedule(new PlaySchedule(createDateTime("2019-04-16 12:00"), 6)); 
    movie1.addPlaySchedule(new PlaySchedule(createDateTime("2019-04-16 14:40"), 6)); 
    ...이하 생략 
      movies.add(movie1); 
  }
```

​	영화 예매 목록을 담고 있는 객체임

​	Movie의 스케쥴을 추가하고 있는데, MovieRepository에서 추가하는 것이 아니라, Movie 내의 메소드에서 추가하고 있음

```java
public class Movie { 
  private final int id; 
  private final String name; 
  private final int price; 
  
  private List<PlaySchedule> playSchedules = new ArrayList<>(); 
  
  void addPlaySchedule(PlaySchedule playSchedule) { 
    playSchedules.add(playSchedule); 
  } 
}
```

​	엄연히 스케쥴을 관리해야 하는 것은 변수를 가지고 있는 Movie의 역할임

##### 3. 함수는 하나의 일만 해야함

​	부수 효과를 일으키지 말고, 명령과 조회를 분리해야함. 이 모든 게 결국 함수가 하나의 일만 한다면 해결됨

​	상태를 변경만 하면 변경만 하고, 출력할 것이라면 출력만 해야함. 두가지 모두는 오해를 일으키기도, 테스트하기도 어려움

##### 4. 반복하지 않기

##### 5. 파라미터는 없어나 하나, 적은게 나음

클래스를 생성하여 파라미터를 최대한 줄이려고 노력하기



#### 주석 작성하기

##### 1. 주석은 작성하지 않는 것이 가장 좋음

​	최대한 코드만 보고 이해할 수 있도록 코드를 구현하기

##### 2. Todo 주석을 작성하기

​	일을 마치고 오늘 어디까지 했고, 내일 무엇을 하면 좋을지 기록하기



#### 객체 만들기

##### 1. 자료를 추상화하기

​	추상 인터페이스를 이용하여 클래스를 생성하기. 관련이 적은 클래스들도 하나로 묶어 사용할 수 있음

| 인터페이스                                 | 추상클래스                                      |
| ------------------------------------------ | ----------------------------------------------- |
| 클래스가 아님                              | 클래스임                                        |
| 클래스와 관련이 없음                       | 클래스와 관련 있음 ( 주로 베이스 클래스로 사용) |
| 한 개의 클래스에 여러 개를 사용할 수 있음  | 한 개의 클래스에 여러개를 사용할 수 없음        |
| 구현 객체의 같은 동작을 보장하기 위한 목적 | 상속을 받아서 기능을 확장시키는 데 목적         |

[출처 및 더 자세한 내용 확인] : <https://loustler.io/languages/oop_interface_and_abstract_class/>

##### 2. 객체의 구조 숨기기

​	단지 private하게 변수를 선언한다고, 구조를 숨기는 것이 아님.

​	객체는 내부에서 값을 얻고 변화하고 계산하고, 외부로 나오는 메소드는 그 결과값만 리턴하는 것. 이외의 다른 메소드들은 피해야함

 	(절대로 객체 통채로 꺼네서 get메소드를 이용하면 안됌)



#### 오류 처리

##### 1. 가정문 보단 try-catch문을 사용하기

##### 2. try-catch문도 가능하면 사용하지 않는 경우를 고민하기

```java
try{ 
	MealExpenses expenses = expenseReportDAO.getMeals(employee.getId()); 
  total += expenses.getTotal(); 
} catch(MealExpensesNotFound e){ 
  total += getMealPerDiem(); 
} // 직원의 id로 타입에 따라 결제방식이 다르다. 
public class PerDiemMealExpenses implements MealExpenses{ 
  public int getTotal{ 
    // 기본값으로 일일 기본 식비를 반환한다 
  } 
}
```

##### 3. null을 반환하지 않기

​	빈 배열이나 0과 같은 값을 반환하는 게 좋음. 나중에 null로 인한 NullPointerException의 발생을 막을 수 있음



#### 단위 테스트 하기

##### 제대로 하는 법부터 배우기

​	작게 그리고 가능한 모든 함수를 테스트하기



#### 클래스 생성하기

##### 1. 클래스 역시 작아야함

​	어느정도 작게냐면, 딱 하나의 책임만 하는 클래스를 생성하려고 해야함

​	(단어 하나로 클래스의 모든 것을 표현할 수 있는 크기가 좋음)

##### 2. 단일 책임 원칙에 따름

​	위와 비슷함. 클래스는 변경할 이유가 단 하나여야함.

​	예를 들어 버전을 관리하고, 때론 관리된 버전을 출력하는 클래스가 있을때, 이것이 수정되야한다면 책임이 두가지이기 때문에 두가지 이유로 수정됨

##### 3. 인스턴스 변수의 수가 작아야함



### 리팩토링이란?

**프로그램의 외부 동작은 그대로 둔 채, 내부의 코드를 정리하면서 개선하는 것**을 말함

이미 공사가 끝난 집이지만, 더 튼튼하고 멋진 집을 만들기 위해 내부 구조를 개선하는 리모델링 작업

프로젝트가 끝나면, 지저분한 코드를 볼 때 가독성이 떨어지는 부분이 존재함

이부분을 개선시키기 위해 필요한 것이 바로 `리팩토링 기법`

리팩토링 작업은 **코드의 가독성을 높이고**. 향후 이루어질 유지보수에 큰 도움이 됨



#### 리팩토링이 필요한 코드는?

- 중복 코드
- 긴 메소드
- 거대한 클래스
- Switch 문
- 절차지향으로 구현한 코드



#### 리팩토링의 목적

리팩토링의 목적은 **소프트웨어를 더 이해하기 쉽고 수정하기 쉽게 만드는 것**

리팩토링은 성능을 최적화시키는 것이 아님. 코드를 신속하게 개발할 수 있게 만들어주고, 코드 품질을 좋게 만들어줌

코드가 이해하기 쉽고, 수정하기 쉽다면 → 개발 속도가 증가함



#### 리팩토링이 필요한 상황

> 소프트웨어에 새로운 기능을 추가해야 할 때

명심해야할 것은, 우선 코드가 제대로 돌아가야 한다는 것.

리팩토링은 우선적으로 해야할 일이 아님을 명심해야함

객체지향 특징을 살리려면, switch-case문을 적게 사용해야함 (switch문은 오버라이드로 다 바꾸는게 좋음)



#### 리팩토링 예제

##### 1번

```java
// 수정 전
public int getFoodPrice(int arg1, int arg2) {
    return arg1 * arg2;
}
```

함수명 직관적 수정, 변수명을 의미에 맞게 수정

```java
// 수정 후
public int getTotalFoodPrice(int price, int quantity) {
    return price * quantity;
}
```



##### 2번

```java
// 수정 전
public int getTotalPrice(int price, int quantity, double discount) {
    return (int) ((price * quantity) * (price * quantity) * (discount /100));
}
```

`price * quantity`가 중복됨. 따로 변수로 추출

할인율을 계산하는 부분을 메소드로 따로 추출

할인율 함수같은 경우는 항상 일정하므로 외부에서 건드리지 못하도록 private 선언

```java
// 수정 후
public int getTotalFoodPrice(int price, int quantity, double discount) {
	int totalPriceQuantity = price * quantity;
    return (int) (totalPriceQuantity - getDiscountPrice(discount, totalPriceQuantity))
}

private double getDiscountPrice(double discount, int totalPriceQuantity) {
    return totalPriceQuantity * (discount / 100);
}
```

이 코드를 한번 더 리팩토링 해보면?



##### 3번

```java
// 수정 전
public int getTotalFoodPrice(int price, int quantity, double discount) {
    int totalPriceQuantity = price * quantity;
    return (int) (totalPriceQuantity - getDiscountPrice(discount, totalPriceQuantity))
}

private double getDiscountPrice(double discount, int totalPriceQuantity) {
    return totalPriceQuantity * (discount / 100);
}
```

totalPriceQuantity를 getter 메소드로 추출이 가능함

지불한다는 의미를 주기 위해 메소드명을 수정

```java
// 수정 후
public int getFoodPriceToPay(int price, int quantity, double discount) {
    int totalPriceQuantity = getTotalPriceQuantity(price, quantity);
    return (int) (totalPriceQuantity - getDiscountPrice(discount, totalPriceQuantity));
}

private double getDiscountPrice(double discount, int totalPriceQuantity) {
    return totalPriceQuantity * (discount / 100);
}

private int getTotalPriceQuantity(int price, int quantity) {
    return price * quantity;
}
```



### 클린코드와 리팩토링의 차이

리팩토링이 더 넓은 의미를 가짐

클린코드는 `단순히 가독성을 높이기 위한 작업`으로 이루어져 있다면,

리팩토링은 클린코드를 포함한 `유지보수를 위한 코드 개선`이 이루어짐

클린코드와 같은 부분은 `설계부터` 잘 이루어져 있는 것이 중요하고, 

리팩토링은 `결과물이 나온 이후 수정이나 추가 작업이 진행될 때` 개선해나가는 것이 올바른 방향임







---

[참고 1] : <https://devuna.tistory.com/26>

[참고 2] : <https://gyoogle.dev/blog/computer-science/software-engineering/Clean%20Code%20&%20Refactoring.html>
