### 🏳 JPA (Java Persistence API)
Java에서 제공하는 API로, Java [**ORM**](https://gmlwjd9405.github.io/2019/02/01/orm.html) 기술에 대한 **API 표준 명세**이다.
즉, 명세(Interface)이므로 JPA만 가지고는 기술을 적용할 수 없고 기술을 적용하기 위해선 구현체(class)가 필요하다.

JPA는 위에 언급했다시피, ORM 기술의 하나로 객체와 RDB(관계형 데이터베이스)의 데이터 자체와 매핑하기 때문에 SQL을 직접 작성할 필요가 없다.
- ~~SQL을 직접 작성해서 사용해야하는 SQL Mapper의 종류에는 MyBatis가 있다.~~

- JPA에 대해 간단하게 그림으로 표현하자면 아래와 같다.
![](https://images.velog.io/images/seyoung_jeon/post/3e0c824a-fcea-472c-ad06-fd04444463f5/image.png)

### 🍏 JPA 동작 과정
![](https://images.velog.io/images/seyoung_jeon/post/a0a7753e-89e7-412e-b5ca-44163990b7ad/image.png)
JPA는 JDBC의 추상화된 버전으로 개발자가 JPA를 사용하면 JPA는 내부적으로 JDBC API를 사용하여 DB와 통신하게 된다. 

#### insert
![](https://images.velog.io/images/seyoung_jeon/post/425926d2-f735-4aa3-83d0-a68f8266cb20/image.png)

1. Member 객체를 JPA로 넘긴다.
2. Member Entity를 분석한다.
3. INSERT SQL Query를 생성한다.
4. JDBC API를 활용하여 SQL을 DB에 날린다.

#### select
![](https://images.velog.io/images/seyoung_jeon/post/d908638d-9bfc-4024-8f61-67773ea77719/image.png)

1. Member의 PK 값을 JPA로 넘긴다.
2. Entity의 매핑 정보를 바탕으로 적절한 SELECT SQL Query를 생성한다.
3. JDBC API를 활용하여 SQL을 DB에 날린다.
4. DB로부터 결과를 받아온다.
5. 결과(ResultSet)를 객체에 매핑한다.

### 🐻 JPA의 특징
1. 데이터를 객체지향적으로 관리할 수 있기 때문에 개발자는 비즈니스 로직에 집중할 수 있고 객체지향 개발이 가능하다.
2. 자바 객체와 DB 테이블 사이의 매핑 설정을 통해 SQL을 생성한다.
3. 객체를 통해 쿼리를 작성할 수 있는 JPQL(Java Persistence Query Language)를 지원한다.
4. JPA는 성능 향상을 위해 지연 로딩이나 즉시 로딩과 같은 몇 가지 기법을 제공하는데, 이것을 잘 활용하면 SQL을 직접 사용하는 것과 유사한 성능을 얻을 수 있다.

#### JPA의 장점
>
  1. 데이터베이스에 종속되지 않아, 추후 데이터베이스 변경이나 코드 재활용이 가능하다.
  2. 테이블 생성, 변경 등 Entity 관리가 간편하다.
  3. 컴파일 시, 오류를 확인할 수 있다.
  4. 코드 레벨로 관리되므로 사용하기 용이하고 생산성이 높다.

#### JPA의 단점
>
   1. 복잡한 연산을 수행하기에는 다소 무리가 있다. (초기에는 생산성이 높을 수 있으나, 운영 시 성능상 이슈가 발생할 수 있다.)
   2. 고도화 될수록 학습 곡선이 높아질 수 있음

### 🚩 결론
1. JPA는 인터페이스라, 구현체가 따로 필요하다.
2. JPA는 ORM 중 하나로, 개발자가 SQL query를 직접 작성할 필요가 없다.




