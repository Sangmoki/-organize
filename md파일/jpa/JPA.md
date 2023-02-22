01-18 수업 내용
JPA 개요
======================
# JDBC, Mybatis, JPA의 개념
## 자바에서의 개념들
### 상속이란?
- 부모의 것을 물려받는다. 이걸 다른말로 '확장'

### 동일성?
- 객체 주소도 같고 값도 같은거

### 동등성?
- 객체 주소는 다르고 값만 같은애들

### JDBC란?
- 자바와 데이터베이스를 연결할 수 있게 만든 표준 api
- 각각의 드라이버에 맞는 oracle, mysql, postgresql 등등에 연결을 시켜준다.
  즉, 자바와 데이터베이스 사이에 위치하여 드라이버들을 소유하고 있는 통합 연결체라고 생각하자.

### Mybatis란 ?
- JDBC를 포함하는 업그레이드 버전 느낌이다.

## JPA란?
- java persistence api
- 자바 영속성 api로 데이터베이스에 접근해서 처리를 하기 위한 api
- Mybatis를 객체지향스럽게 업그레이드한 버전 느낌이다.
  즉, Mybatis를 대체한 라이브러리이다.

### JPA의 특징
- 기존에 데이터 중심(하나의 컬럼이 추가되면 여러 개의 수정이 필요하다)
  였던 것들이 객체 지향 중심으로 다가오면서 entity에서 컬럼하나만 추가하면
  여러 개의 수정이 필요가 없고 어느정도 가벼운 쿼리문들은 자동으로 만들어준다.
- 영속성 컨텍스트에서 캐싱을 해서 사용 1차 캐쉬를 통해서 성능을 향상시킬 수 있다.

- SQL의 문제는 요청할 때마다 주소가 다르기 때문에 
동일성을 보장하지 않아 동등성만을 가진다.
  하지만 JPA는 동일성을 보장한다.

- 자바에서 객체에는 상속이란 개념이 있는데, DB에는 상속이란게 없다.
	대신에 사용할 수 있는게 슈퍼타입/서브타입이 있다.
#### 슈퍼타입이란?
- 예를 들어 회원, 법인회원, 가맹정 등이 있으면 법인 안에 회원이 있고, 가맹점에도 회원이 있으니까,
공통적으로 회원을 가진다. 이것을 슈퍼타입이라고 칭한다.

#### 서브타입이란?
- 회원이 가지고 있는 정보 이외의 법인이 가지고 있는 추상화되어있는 속성값들, 또한 가맹점도 동일하다.

#### 영속성 컨텍스트
- 데이터베이스를 삭제하기 전까지 영원히 가질 수 있다는 의미의 범주
ex) https://velog.io/@neptunes032/JPA-%EC%98%81%EC%86%8D%EC%84%B1-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%9E%80 참조하기

#### 1차 캐싱이란?
- request 요청이 들어오면 영속성 컨텍스트가 DB에서 꺼내와 보관하는 과정이다.
- 1차 캐싱 = 영속화 시킨다. 
ex) entityManager.persist(객체)

#### DTO?
- 객체에 담아 데이터를 옮기기 위한 목적
- 대신 hashCode를 equals 하지 않기 때문에 동등성만.

#### VO?
- 얘도 DTO와 같지만 hashCode를 equals하기 때문에 동일성을 보장한다.


### JDBC API를 이용해 직접 SQL을 다룰 때 발생하는 문제점
#### 1. 데이터 변환, SQL 작성, JDBC API코드 등을 반복적으로 일일이 다 작성해야 한다.
	
##### 1-1. DB 연결이 필요한 경우 매번 JDBC API 코드를 반복적으로 사용해야 한다.
``` java
	Statement stmt = con.createStatement();
	ResultSet rset = stmt.executeQuery(query);
```
##### 1-2. 조회한 데이터를 자바 객체로 개발자가 직접 변환을 해주어야 한다.
``` java
List<Menu> menuList = new ArrayList<>();
while (rset.next()){

	Menu menu = new Menu();
	menu.setMenuCode(rset.getInt("MENU_CODE"));
	menu.setMenuName(rset.getString("MENU_NAME"));
	menu.setMenuPrice(rset.getInt("MENU_PRICE"));
	menu.setCategoryCode(rset.getInt("CATEGORY_CODE"));
	menu.setOrderableStatus(rset.getString("ORDERABLE_STATUS"));

	menuList.add(menu);
}
```
#### 2. SQL에 의존적인 개발을 하게 된다.(데이터 중심)
##### 2-1. 요구사항의 변경에 따라 애플리케이션의 수정이 SQL의 수정으로도 이어져야 하며,
이러한 수정 영향을 미치는 현상은 오류를 발생시킬 가능성도 있지만, 유지보수성에도 악영향을 끼친다.
또한 객체를 사용할 때 SQL에 의존하게 되면 객체에 값이 무엇이 들어있는지 확인해보기 위해서는 SQL을 매번 살펴봐야한다.

##### 2-2. 만약 SQL문이 tbl_category 테이블을 조인하지 않았다면?
--> category_code와 category_name은 부적합한 열 식별자라는 오류가 발생한다.

##### 만약 SQL문에서 tbl_category 테이블은 조인했지만 select절에 category_code와 category_name의 컬럼이 누락되면?
--> JDBC코드에서 category_code와 category_name은 부적합한 열 식별자라는 오류가 발생한다.

##### 만약 SQL문은 정상적으로 반영이 되었지만 데이터 변화 시 setCategory()가 누락된다면?
--> category는 null이 되기 때문에 category를 참조하려는 쪽에서 NullPointerException이 발생한다.
            (즉, 사용하는 측에서 NPE 발생 가능성으로 신뢰할 수 없다.)


#### 3. 패러다임 불일치 문제(상속, 연관관계, 객체 그래프 탐색)
##### 3-1. 관계형 데이터베이스는 데이터 중심으로 구조화 되어있고, 집합적인 사고를 요구한다.
- 객체지향에서 이야기하는 추상화, 상속, 다형성 같은 개념이 존재하지 않는다.
- 지향하는 목적 자체가 다르기 때문에 이를 표현하는 방법이 다르고, 그렇기 때문에 객체 구조를 테이블 구조에 저장하는데 한계가 있다.
- 이런 객체와 관계형 데이터베이스 사이에 패러다임 불일치 문제를 해결하는데 너무 많은 시간과 코드가 소비된다.
  
- 객체지향언어의 상속 개념과 유사한 것이 데이터베이스의 서브타입 엔터티이다.
- 유사한 것 같지만 다른 부분은 데이터베이스의 상속은 상속 개념을 데이터로 추상화하여 슈퍼타입과 서브타입으로 구분하고,
  슈퍼타입의 모든 속성들을 서브타입이 공유하지 못하며, 물리적으로도 다른 테이블로 분리가 된 형태이다.
- 두 개의 서로 다른 테이블을 조회하기 위해서는 공유하는(FK) 컬럼을 이용해 JOIN을 해서 사용해야 한다.
- 하지만 객체지향의 상속은 슈퍼타입의 속성을 공유해서 사용하기 때문에 여기서 패러다임 불일치 현상이 발생하게 된다.
- 또한 insert시에는 각 테이블에 insert구문을 따로따로 실행시켜야 한다.
ex) JDBC API를 이용하면
     INSERT INTO 회원...
     INSERT INTO 법인회원... 이런식으로 두 개의 테이블에 값을 추가해줘야 한다.

##### 3-2. 연관 관계
- 객체지향에서 말하는 가지고 있는 (association 연관 관계, 혹은 collection 연관관계)경우 데이터베이스의 저장 구조와는 다른 형태이다.
``` java
public class Menu {
		private int menuCode;
		private String menuName;
		private int menuPrice;
		private int categoryCode;
		private String orderableStatus;
}
	
public class Category {
	private int categoryCode;
	private String categoryName;
}    

public class Menu {
	private int menuCode;
	private String menuNAme;
	private int price;
	private Category category;
	private String orderableStatus;
}

Menu menu = new Menu();
Category category = new Category();

menu.setCategory(category); // 메뉴와 카테고리의 관계 설정
```
- 즉, 연관 관계가 있을 때 테이블 하나의 컬럼의 변동이 생기면 해당 테이블만 바꾸는 것이 아니라
  다른 테이블도 변경을 해주어야 하기 때문에 이러한 문제들이 발생한다.

#### 4. 동일성 보장

- JDBC를 이용하여 조회한 두 개의 Menu객체는 동일한 로우를 조회하더라도 같은 값을 가지는 동등성을 가지지만 동일성을 가지지는 않는다.
- 같은 값을 가지지만(동등성 o) 서로 다른 주소를 가진다(동등성 x)
- 
``` java
  ex) assertEquals(menu1 == menu2); // false
      assertEquals(menu1.getMenuName(), menu2.getMenuName()); // true

      System.out.println("menu1 = " + menu1);
      System.out.println("menu2 = " + menu2);
```

### JPA를 다룰 때의 장점
#### 1. JPA는 DAO 계층에서 지루하고 반복되는 JDBC API를 획기적으로 줄여주며 이러한 문제를 해결했다.

#### 2. JPA는 데이터베이스에 저장하고 사용할 때 개발자가 직접 SQL문을 작성하지 않는다.
- JPA가 제공하는 API를 사용하면 내부에서 SQL을 생성해서 동작을 시킨다.
- 따라서 SQL에 의존적이지 않게된다.(의존하는 것은 직접 사용하지 않으면 의존성이 낮아지게 된다.)
- 컬렉션 API에서 제공하는 컬렉션에 값을 추가하거나 꺼내오는 기능을 이용하면 매우 간단하다.
JPA는 DAO 계층에서 지루하고 반복되는 JDBC API를 획기적으로 줄여주며 이러한 문제를 해결했다.

#### 3. 상속과 관련된 패러다임의 불일치 문제를 개발자 대신에 해결해준다.
- 마치 커렉션에 객체를 저장하듯 JPA에 객체만 저장하면 된다.
``` java
    Menu menu = entityManager.find(Menu.class, menuCode);
          String categoryName = menu.getCategoryName();
          Team team = member.getTeam();
          위처럼 연관 객체를 함께 조인해서 조회하는 것을 entityManager가 보장해준다.
	즉, NPE 발생하지 않는다. --> 지연로딩으로 필요할 때만 조회를 진행한다.(한번에 x)
```

#### 4. JPA를 이용하여 조회한 두 개의 Menu객체는 동일한 로우를 조회하는 경우 동일성을 보장하게 된다. 단순(==) 비교가 가능해진다.

``` java
ex)	Menu menu1 = entityManager.find(Menu.class, 12);
Menu menu2 = entityManager.find(Menu.class, 12);

menu1 == menu2; // true <- 같은 영속성 컨텍스트에서 꺼내와서 동일성을 보장한다.
``` 

설정에서 카멜케이스 true로 하면 resultMap을 설정하지 않아도
필드이름이 DTO값과 같으면 자동으로 resultMap을 처리해준다.

### jpa의 개념
#### entity란?
- 데이터베이스에서 값을 가져와 담아줄 객체 테이블

#### entityManager
- entity를 관리하는 아이디

##### 
- entityManager의 find(가져올 곳, 인덱스)
- 조회하는 용도다.

- JPA에서 SQL문은 직접 작성하지 않아도 아래 구문처럼 사용할 수 있다.
- 즉 DB에 종속적이지 않다. 얘가 만들어주기 때문에 !! 이걸 방언이라고 한다.
``` java
	Menu menu = entityManager.find(Menu.class, 1);
```
#### persist
- 영속성 컨텍스트로 
- entityManager.persist(menu);  <- 영속화
- 영속성 컨텍스트에 해당 요청에 대한 값이 없으면 추가하고, 상태값이 변경되어있으면 업데이트 해주는거다.




### 엔티티

#### 엔티티란?
- DB랑 실질적 연결을 하여 데이터를 담아올 수 있는 테이블
- 
#### DTO란? 
- 데이터를 옮겨 다닐 수 있게 받아줄 수 있는 테이블
- 
#### 엔티티 트리
- PersistenceContext > EntityManagerFactory > EntityManager

#### 엔터티 매니저(EntityManager)란?
- 엔터티 매니저는 엔터티를 저장하는 메모리상의 데이터베이스라고 생각한다.
- 엔터티를 저장하고 수정, 삭제, 조회하는 등의 엔터티와 관련된 모든 일을 한다.
- 엔터티 매니저는 스레드세이프 하지 않기 때문에 문제가 발생할 수 있기에 스레드간 공유를 하면 안된다.
- Web의 경우 일반적으로는 request scope와 일치시킨다.

#### 엔터티 매니저 팩토리(EntityManagerFactory)란?
- 엔터티 매니저를 생성할 수 있는 기능을 제공하는 클래스이다.
- 스레드 세이프이기 때문에 여러 스레드가 동시에 접근해도 안전하기 때문에 서로 다른 스레드간 공유해서 재사용한다.
- 하지만 스레드 세이프 한 기능을 요청 스코프마다 생성하기에는 비용(시간, 메모리)부담이 크기 때문에
  application 스코프와 동일한 싱글톤으로 생성해서 관리하게 된다.
- 따라서, 데이터베이스를 사용하는 애플리케이션 당 한 개의 EntityManagerFactory를 생성한다.

#### 영속성 컨텍스트(PersistenceContext)란
- 엔터티를 영구 저장하는 환경을 말한다.
- 엔터티 매니저에 엔터티를 저장하거나 조회하면 엔터티 매니저는 영속성 컨텍스트에 엔터티를 보관하고 관리한다.
- 영속성 엔터티에 key value 방식으로 저장하는 저장소 역할을 한다.
  그래서 사용자가 필요할 때마다 꺼내준다.
- 영속성 컨텍스트는 엔터티 매니저를 생성할 때 하나 만들어진다.
   엔터티 매니저를 통해서 영속성 컨텍스트에 접근할 수 있고 영속성 컨텍스트를 관리할 수 있다.


## JPA 어노테이션 설명

### JPA를 사용하기 위한 기본 설정

#### META-INF에 persistence.xml 파일 생성 xml 작성 및 persistence 작성
``` xml
	<?xml version="1.0" encoding="UTF-8" ?>
	<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
		<persistence-unit name="jpatest">
			<properties>
				<property name="javax.persistence.jdbc.driver" value="oracle.jdbc.driver.OracleDriver"/>
			</properties>
		</persistence-unit>
	</persistence>

	
```

#### 라이브러리 추가
- Hibernate EntityManager Relocation
- Hibernate Core Relocation
- JPA 2.0 API
- Hibernate Commons Annotations

#### persistence-unit 안에 properties 설정
- 스프링 부트에서 yml파일 설정하는 것과 똑같다.
``` yml
	<property name="hibernate.show_sql" value="true"/> <!-- 콘솔창에 쿼리문을 보여주는 설정 -->
	<property name="hibernate.format_sql" value="true"/> <!-- 쿼리를 이쁘게 만들어준다. -->
```

### JPA에서 지원하는 어노테이션

#### @Entity
- 해당 클래스를 앤터티 객체로 만들기 위한 어노테이션, 
- 다른 패키지에 동일한 이름의 클래스가 존재한다면 name을 지정해야 한다.
	ex) @Entity(name = "Section02Menu")
- 또한, getter setter, 기본 생성자, toString 어노테이션은 꼭 있어야 한다.

#### @Table
- 클래스 이름과 테이블의 이름이 다른 경우 name을 지정하여 테이블의 이름을 지정할 수 있다.
	ex) @Table(name = "TBL_MENU")

#### @SequenceGenerator()
- 시퀀스를 사용할 것이라는 어노테이션
- 기존에 만들었던 것을 사용할 것인지, 아님 새로 만들어서 쓸건지 선택
``` java	
@SequenceGenerator(
name = "SEQ_MENU_CODE_GENERATOR", // 해당 사퀀스 설정에 대한 이름
sequenceName = "SEQ_MENU_CODE", // 생성되어있는 시퀀스 이름 지정
initialValue = 100, // 초기값, 값을 추가해주기만 하면 된다, 여기는 아무 숫자나 줘도 된다.
allocationSize = 1 // 시퀀스의 증가 수
)
// 해당 엔티티 클래스에 시퀀스 어노테이션 설정을 했다면
// 설정한 시퀀스를 사용할 필드에도 @GeneratedValue 어노테이션을 지정해주어야 한다. 
@GeneratedValue(
	strategy = GenerationType.SEQUENCE, // 값 생성 시 시퀀스 객체 이용
	generator = "SEQ_MENU_CODE_GENERATOR" // 이 필드에 해당 시퀀스를 적용하겠다.
)
private int menuCode;
```

#### @Id
- 해당 테이블의 PK(기본키)에 해당하는 속성을 지정한다.
- Id에도 컬럼을 해주어야 한다.
``` java
@Id // PK에 해당하는 속성을 지정
private int menuCode;
``` 

#### @Column
- 데이터베이스에 대응되는 컬럼명을 name으로 지정해준다.
``` java
@Column(name = "MENU_NAME") // 데이터 베이스에 대응되는 컬럼명 지정
private String menuName;
``` 

#### entityManager.find()
- 해당하는 클래스에서 given에서 찾을 값을 설정한 PK에 해당하는 한개의 행만을 찾아온다.
``` java
// given - 기븐에서 우럭스무디에 대한 메뉴코드를 골랐다면
int menuCode = 2; // 우럭 스무디
// when
Menu foundMenu = entityManager.find(Menu.class, menuCode)
// 해당 TBL_MENU 테이블의 pk 번호 조회한다.

given에서 지정한 menuCode와 foundMenu의 MenuCode와 같은지 비교.
// then
assertNotNull(foundMenu); // 
assertEquals(menuCode, foundMenu.getMenuCode());
```

#### entityManager.find()
- 조회할 때(찾을 때)는 entityManager의 find() 메서드를 사용한다.

#### entityManager.persist()
- 추가, 수정, 삭제 할 때는 entityManager의 persist() 메서드를 사용한다.
- 또한, 추가, 수정, 삭제할 때는 트랜잭션을 만들어줘야 반영이 된다.
``` java	
// when
EntityTransaction entityTansaction = entityManager.getTransaction();
entityTansaction.begin(); // 트랜잭션을 시작할 것이다.
```

#### EntityTransaction 메서드
- begin() - 트랜잭션을 시작할 것이라는 선언 메소드
- commit() - 영속성 컨텍스트에 넣고 쿼리문을 작성해서 실행하는 시점 메서드
- 트랜잭션 처리 시에 try catch 구문을 넣어 
	try 부분에 commit();
	catch 부분에 rollback();

``` java	
String menuNameToChange = "우럭뽈살젤리"; 
// 바꾸려고하는 메뉴의 이름을 미리 변수에 담아놓는다.
	// 예외 처리 전에 트랜잭션을 열어주고
EntityTransaction entityTransaction = entityManager.getTransaction();
entityTransaction.begin(); // 트랜잭션 시작
	try {
		// 미리 지정해놓은 메뉴 이름으로 변경을 시도한다.
		menu.setMenuName(menuNameToChange);
		DB에 반영시키려면 commit() 해주어야 한다.
		entityTransaction.commit();

	} catch (Exception e){
		e.printStackTrace();
		entityTransaction.rollback();
	}
```

### 영속에 관하여 (영속화)

#### 영속화
``` java 
        Menu menu = entityManager.find(Menu.class, 2); // 영속 상태
        Menu menu1 = new Menu() // 얘는 비영속.
		즉, entityManager한테 관리되는 상태를 영속
		아니면 비영속
``` 

#### 영속화 삭제
``` java
entityManager.find() - 영속화

		Menu menuToRemove = entityManager.find(Menu.class, 201); 
entityManager.remove() - 삭제
		entityManager.remove(menuToRemove);
        영속성 컨텍스트한테 menuToRemove라는 주소값을 넘겨주고 삭제하라고 한거다.
		하지만 아직 커밋하지 않았기 때문에 영속성 컨텍스트에서만 null로 되고,
        DB에는 반영되지 않는다.

		entityTransaction.commit();
        이 구문이 나오면 그때서야 delete 쿼리를 만들어서 DB에 보내주는 것이다.
```

#### EntityTransaction flush
- flush() - 반영해라.
  즉, 데이터 베이스에 반영해라 라는 뜻이다.
- flush가 commit, rollback 해라 라는 뜻,
  즉, 현재 상태에 대한것을 이제 끝내라 !!? 정도

#### 스냅샷(snapshot)
- 반환하기 전의 상태값을 저장

### 준영속성 테스트

#### detach - 특정 엔티티 객체만 준영속 상태로 만든다.(연결을 끊는다/분리한다.)
- 영속성 컨텍스트에서 관리를 하지 않겠다.
- 원래는 영속성 컨텍스트에서 관리를 받지만 detach를 통해 연결을 끊는다.
- 연결을 끊고 다시 재 find시 다른 주소인 새로운 영속성 컨텍스트 키가 들어온다.
- Merge가 아닌 find는 무조건 새로운 애다
``` java
	entityManager.detach(foundMenu2); 
			-> 안에 있는 요소를 모두 준영속상태로 변경을 해준다.
``` 

#### clear - 영속성 컨텍스트 안에 있는 모든 요소를 준영속 상태로 만들어준다.
 	
#### close - 영속성 컨텍스트를 종료
- clear와는 다르게 close 하기 이전에 꺼내와 객체에 담아준것은 사용할 수 있지만
 새로이 가져올 때 더이상 find로 불러올 수 없다. 
- final같은 느낌이라고 생각하자.

#### remove - 영속성 삭제
- 엔티티를 영속성 컨텍스트 및 데이터베이스에서 삭제한다.
- 단, 트랜잭션을 제어하지 않으면 영구히 반영되지는 않는다.
- 트랜잭션을 커밋하느 순간 영속성 컨텍스트에서 관리하는 엔티티 객체가 데이터베이스에 반영되게 된다.
- 이를 flush 라고 한다.

#### flush - 동기화
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 동기화하는 작업(등록, 수정, 삭제한 엔티티를 데이터베이스에 반영)

#### 병합 merge 수정
``` java
	Menu menuToDetach = entityManager.find(Menu.class, 2); // 영속화 한 후
	entityManager.detach(menuToDetach); // 준영속상태로 변경

	// when
    menuToDetach.setMenuName("민초죽");
	Menu refoundMenu = entityManager.find(Menu.class, 2); // 영속 상태 둘다 동일한 키 값을 가지고 있다.

	System.out.println("menuToDetach = " + menuToDetach.hashCode());
	System.out.println("refoundMenu = " + refoundMenu.hashCode());

	entityManager.merge(menuToDetach);
	// then

	Menu mergeMenu = entityManager.find(Menu.class, 2);
	System.out.println("mergeMenu = " + mergeMenu);
	assertEquals("민초죽", mergeMenu.getMenuName());
```

#### 병합 merge 삽입 테스트
``` java
	Menu menuToDetach = entityManager.find(Menu.class, 2); // 영속화
	entityManager.detach(menuToDetach); // 준영속화

	menuToDetach.setMenuCode(999);
	menuToDetach.setMenuName("단팥죽"); 

	entityManager.merge(menuToDetach); // 영속 상태의 엔티티 객체와 병합(현재 없기 때문에 삽입)

	준영속화 시켜서 세팅을 하고 다시 영속화 시켰는데
	pk가 영속 컨텍스트에 없어서 새로 추가시킨다.

	Menu mergeMenu = entityManager.find(Menu.class, 999); // 삽입이 된 상태에서 다시 조회했다.
	assertEquals("단팥죽", mergeMenu.getMenuName());

	새로 영속성 컨텍스트에 들어갔기 때문에 다시 조회해야한다.
```