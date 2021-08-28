이전 시간까지 Spring Framework를 이용하여 Hibernate를 이용한 JPA 환경 구축까지 진행했다.
이번 시간에는 JPA 기본 문법과 어떻게 사용하는지에 대한 활용에 대해서 간단한 코드를 살펴보며 익혀보고자 한다.

### 1. JpaRepository 인터페이스
```
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
	List<User> findByUserId(String userId);
	List<User> findByUserStatusType(UserStatusType userStatusType);
}
```

Spring에서는 이전에 말했다시피, 일반 jpa보다 한 단계 더 추상화 된 jpa를 사용하는데 이 때, 사용하는게 repository이다.

```
public interface 이름 extends JpaRepository <엔티티 ID 유형, ID type>

```
<> 안에는 Entity 클래스 이름과 ID 필드 타입을 지정하면 된다. 
주의할 점은, Wrapper 클래스만 지원한다.

### 2. Entity 설정
```
@Entity
@Table(name = "user")
public class User {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "user_no")
	long userNo;
	@Column(name = "user_id")
	String userId;
	@Column(name = "password")
	String password;
	@Column(name = "password_sort")
	String passwordSort;
	@Column(name = "email")
	String email;
	@Column(name = "authority")
	UserAuthorityType userAuthorityType;
	@Column(name = "created_at")
	long createAt;
	@Column(name = "updated_at")
	long updateAt;
	@Column(name = "updated_user_no")
	long updatedUserNo;
	@Column(name = "updated_detail")
	String updatedDetail;
	@Convert(converter = UserStatusTypeConverter.class)
	@Column(name = "status")
	UserStatusType userStatusType;
	@Column(name = "reported_cnt")
	int reportedCnt;
	@Column(name = "profile_user_no")
	long profileUserNo;
}
```
- @Entity
데이터베이스의 테이블과 1:1로 매칭되는 객체 단위이며 Entity 객체의 인스턴스 하나가 테이블에서 하나의 레코드 값을 의미한다.

- @Table
데이터베이스 객체(테이블)와 매핑하기 위해 객체의 이름을 명시적으로 선언하기 위해 사용한다.
생략 시, 매핑한 Entity 이름을 테이블 이름으로 사용한다.

- @Column
데이터베이스의 테이블에 있는 컬럼과 동일하게 1:1로 매칭되기 때문에 Entity 클래스안에 내부변수로 정의 된다.
**lenth** 속성으로 길이를 명시할 수 있는데, 이는 데이터베이스의 데이터를 효율적으로 관리하기 위해서이다.

- @Id
데이터베이스의 테이블은 기본적으로 유일한 값을 가지는데, 해당 어노테이션을 통해, PK를 지정한다.



### 3. JpaRepository 표준 방법

#### 표준 함수
개발자가 특별히 따로 선언하지 않아도 기본적으로 제공하는 Jpa 표준 함수들이다. 이 이상의 기능들을 사용하기 위해선 커스텀 함수를 선언해야 한다.
```
findAll() // 전체 Entity List 반환
getOne("ID") // 지정된 ID에 맞는 Entity 하나 반환
saveAndFlush(Entity) // 해당 Entity 데베에 저장
delete("ID") // 지정된 ID에 맞는 Entity 삭제
count() // Entity의 수를 int 값으로 반환
```

#### 커스텀 함수
위에 선언한 `findByUserStatusType` 함수를 예로 들어보겠다.
```
List<User> findByUserStatusType(UserStatusType userStatusType);
```

해당 함수는 `userStatusType` 값을 활용해 해당 record 를 찾는 것이 목적이다.

그러나, 여기서 주의할 점은 `userStatusType`은 내가 직접한 enum이기 때문에 매핑하기 위해선 추가적인 코드가 필요하다.

`User` 모델의 `UserStatusType` 필드에는 
```
@Convert(converter = UserStatusTypeConverter.class)
```
와 같은 Annotation이 붙여 있다. 해당 어노테이션은 `converter` 클래슬르 지정해주는데, 내가 원하는 값으로 변환하는 클래스를 선언하여 사용할 수 있다.

아래는 `UserStatusTypeConverter` 코드이다.
```
@Converter
public class UserStatusTypeConverter implements AttributeConverter<UserStatusType, String> {
	
	@Override
	public String convertToDatabaseColumn(UserStatusType attribute) {
		return attribute.getDbCode();
	}
	
	@Override
	public UserStatusType convertToEntityAttribute(String dbData) {
		return UserStatusType.find(dbData);
	}
}
```

`UserStatusType`으로 원하는 `dbCode`를 가져오는 함수 선언과 `dbCode`를 이용하여 원하는 `UserStatusType` 을 선언할 수 있다.

아래는 `UserStatusType` 모델 코드이다.
```
@Getter
@AllArgsConstructor
public enum UserStatusType {
	NORMAL("N"),
	ADMIN("A");
	
	String dbCode;
	
	public static UserStatusType find(String dbCode) {
		for (UserStatusType value : UserStatusType.values()) {
			if (value.getDbCode().equals(dbCode)) {
				return  value;
			}
		}
		return null;
	}
}
```

### 4. Jpa 관련 추가 Annotation

#### @GeneratedValue
PK 컬럼의 데이터 형식은 정해져 있지는 않으나, 구분이 가능한 유일한 값을 가지고 있어야 하고 데이터 경합으로 인해 발생되는 데드락 같은 현상을 방지 하기 위해 대부분 **BigInteger**, Java의 **Long**을 주로 사용한다.

- deadlock
> 동일한 시점에 요청이 유입되었을 때, 데이터베이스는 **테이블 혹은 레코드를 lock**을 걸어 데이터가 변경되지 않도록 막아 놓고 다른 작업을 진행한다.

#### @EmbeddedId
복합키로서 테이블의 PK를 정의하기 도하는데, 별도의 Value를 사용해 복합키를 정의한다. 아래 코드를 참조하자.

```
@Embeddable
public class CompanyOrganizationKey implements Serializable {
	@Column(name = "company_code")
    private String companyCode;
    @Column(name = "organization_code")
    private String organizationCode;
}

@Entity(name = "company_organization")
public class CompnayOrganization {
	@EmbeddedId
    protected CompnayOrganizationKey companyOrganizationkey;
}
```

#### @Enumerated
java의 enum 형태로 되어 있는 미리 정의되어 있는 코드 값이나 구분 값을 데이터 타입으로 사용하고자 할 때 사용된다.
속성으로 EnumType.ORDINAL, EnumType.STRING이 있는데, ORDINAL은 enum 객체에 정의된 순서가 컬럼의 값으로 사용되고 STRING은 enum의 문자열 자체가 컬럼의 값으로 사용된다.
```
enum FlagYN {
	Y, N
}

@Enumerated(EnumType.ORDINAL)
@Column(name = "access_yn")
private FlagYN accessYN; // 0, 1 값으로 저장

@Enumerated(EnumType.STRING)
@Column(name = "use_yn", length = 1)
private FlagYN useYn; // "Y", "N" 값으로 저장
```

#### @Transient
Entity 객체에 속성으로서 지정되어 있지만, 데이터베이스상에 필용벗는 속성이라면 @Transient 어노테이션을 이용해서 해당 속성을 데이터베이스에서 이용하지 않겠다라고 정의할 때, 사용한다.
이렇게 하면 해당 속성을 Entity 객체에 임시로 값을 담는 용도로 사용이 가능해진다.
```
@Transient
private String tempValue;
```