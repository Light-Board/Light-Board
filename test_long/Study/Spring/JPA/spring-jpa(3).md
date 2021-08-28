첫 번째 시간과 두 번째 시간까지 개념 위주로 JPA에 대해서 알아보았다. 
이번 시간에는 예시 Gradle 프로젝트를 통해서, SpringFramework에 MySQL을 연동하면서 JPA 환경을 구축하는 방법을 정리하려고 하는데, 이번 시간에는 필요한 라이브러리에 정리한다.

하지만 그 전에 앞서, DataSource에 대한 개념을 짚고 넘어가야 한다.

### 🏳 DataSource
![](https://images.velog.io/images/seyoung_jeon/post/87e50b40-e7cc-4a76-82aa-f999b9899b1b/image.png)
위 그림은 Data Access Layer를 나타내는 그림인데, 가운데 DataSource라는 것이 있다. 

#### 1. DataSource란?
JDBC 명세의 일부분이면서 일반화된 연결 팩토리이다.
Database와 관계된 connection 정보를 담고 있으며, bean으로 등록하여 인자로 넘겨주는데, Spring은 이 과정을 통해, DataSource로 DB와의 연결을 획득한다.

#### 2. DataSource가 하는 일
1. DB Server와의 기본적인 연결
2. DB Connection Pooling 기능
	- 자바 프로그램에서 데이터베이스 연결은 시간이 많이 걸리는데, 일정량의 Connections을 미리 생성시켜 저장소에 저장했다가 프로그램에서 요청이 있으면 저장소에서 Connection을 꺼내 제공한다면 시간을 절약할 수 있다. 
    - 해당 기법을 이용하면 속도와 퍼포먼스가 좋아진다.
3. 트랜잭션 처리

#### 3. DataSource의 종류
1. **BasicDataSource**
2. PoolingDataSource
3. SingleConnectionDataSource
4. DriverManagerDataSource

DataSource 종류 중 dbcp2 라이브러리를 활용하는 BasicDataSource를 사용할 것이다.

### 🐻 환경 구축 시작!!
준비된 환경은 아래와 같다.
>
 **SpringFramework** : 5.3.7
 **Java11**
 **tomcat** : 8.5.59
 **Gradle** : 6.3
 **MySQL** : 8.0.21


참고로, 라이브러리는 https://mvnrepository.com/ 이 사이트에서 검색해서 찾을 수 있다.

#### 1. Spring jdbc, orm 라이브러리
spring-jdbc는 JDBC의 장점과 단순성을 그대로 유지하면서도 기존 JDBC의 단점을 극복할 수 있게 해주고, 간결한 형태의 API 사용법을 제공하며, JDBC API에서 지원되지 않는 편리한 기능을 제공한다.

spring-orm은 이후에 추가될 Hibernate 라이브러리를 지원하는 라이브러리이다.

```
implementation 'org.springframework:spring-jdbc:5.3.7'
implementation 'org.springframework:spring-orm:5.3.7'
```

#### 2. mysql-connector-java 라이브러리
JDBC를 활용하기 위해선 mysql-connector가 필요한데, 이를 라이브러리를 추가함으로써 해결할 수 있다.

build.gradle에 mysql 버전에 맞게 dependency를 추가하면 된다.
```
implementation 'mysql:mysql-connector-java:8.0.21'
```

#### 3. Hibernate 라이브러리
Hibernate 라이브러리를 활용하기 위해선 여러가지 dependency가 필요하다.

```
implementation 'org.hibernate:hibernate-core:6.0.0.Alpha7' (Hibernate 라이브러리)
implementation 'org.hibernate:hibernate-entitymanager:6.0.0.Alpha7' (Hibernate가 JPA 구현체로 동작하도록 JPA 표준을 구현한 라이브러리)
```

#### 4. Spring-data-jpa 라이브러리
JPA를 한 단계 더 추상화 시킨 spring-data-jpa depndency를 추가한다.
```
implementation 'org.springframework.data:spring-data-jpa:2.5.0'
```

#### 5. dpcp2 라이브러리
dbcp2라이브러리는 DataSource라는 인터페이스를 직접 구현한 BasicDataSource 구현체를 제공하는 라이브러리이다.

```
implementation 'org.apache.commons:commons-dbcp2:2.8.0'
```

### 🚩 결론
1. DataSource는 데이터베이스와의 Connection을 관리해주어, 성능 향상과 다양한 기능을 제공 받을 수 있다.
2. SpringFramework에서 JPA 환경을 구축하기 위해선 다양한 라이브러리가 필요하다.
