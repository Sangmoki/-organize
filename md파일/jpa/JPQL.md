JPQL(Java Persistence Query Language)
===========================================
## JPQL이란 ?
- 엔티티 객체를 중심으로 개발할 수 있는 객체지향 쿼리
- JPQL은 SQL보다 간결하며 DBMS에 상관없이 개발이 가능하다.
- 즉, 방언을 통해 해결되며 해당 DBMS에 맞는 SQL실행, SQL을 추상화해서 특정 데이터베이스 SQL에 의존하지 않는다.

- JPQL은 find()메소드를 통한 조회와 다르게 항상 데이터베이스에 SQL을 실행해서 결과를 조회한다.
  (영속성 컨텍스트에 이미 존재하면 기존 엔티티를 반환하고 조회한 것은 버린다.)
- JPQL은 엔티티 객체를 대상으로 쿼리를 질의하고 SQL은 데이터베이스의 테이블을 대상으로 질의한다.

### JPA의 공식 지원 기능
- Criteria 쿼리 : JPQL을 편하게 작성하도록 도와주는 API
- 네이티브 SQL(Native SQL) : JPA에서 JPQL 대신 직접 SQL을 사용

### JPA의 비공식 지원기능
- QueryDSL : 
    Criteria 쿼리처럼 JPQL을 편하게 작성하도록 도와주는 빌더 클래스 모음(비표준 오픈소스 프레임워크)
- JOOQ
    JDBC직접 사용 또는 Mybatis같은 SQL매퍼 프레임워크 : JDBC를 직접 작업해서 사용

### JPQL의 기본 문법
1. CRUD
##### select :
``` java
    select_절
      from_절
    [where_절]
    [groupby_절]
    [having_절]
    [orderby_절]
```
##### insert :
- insert문은 EntityManager가 제공하는 persist() 메소드를 사용하면 되서 따로 존재하지 않는다.

##### update :
``` java
    update_절
    [where_절]
```

##### delete :
``` java    
    delete_절
    [where_절]
```

### JPQL 특징
- (주의!) 엔티티와 속성은 대소문자를 구분한다.
- SELECT, from과 같은 JPQL의 기본 키워드들은 대소문자를 구분하지 않는다.
- 엔티티명은 클래스명이아니라 엔티티 명을 기입한다.
- 수업용처럼 하나의 프로젝트에서 같은 이름의 엔티티 클래스를 사용해서 엔티티 명을 각각 다르게 주고 있지만,
    기본값인 클래스명을 엔티티명으로 사용하는 것을 추천한다.
- JPQL은 별칭은 필수로 사용해야 하며, 별칭 없이 작성하면 에러가 발생한다.

### JPQL 사용 방법
#### 1. 작성한 JPQL(문자열)을 em.createQuery 메소드를 통해 쿼리 객체로 만든다.
- 쿼리 객체는 TypedQuery와 Query 두 가지가 있다.
##### TypeQuery : 
- 반환할 타입을 명확하게 지정하는 방식일 때 사용(쿼리 객체의 메소드 실행 결과로 지정한 타입이 반환된다.)
    
##### Query : 
- 반환할 타입을 명확하게 지정할 수 없을 때 사용(쿼리 객체의 메소드 실행 결과로 Object 혹은 Object[]이 반환)
     

#### 2. 쿼리 객체에서 제공하는 메소드 
- getsingleResult() 또는 getResultList()를 호출하여 쿼리를 실행하고, 데이터베이스를 조회한다.
##### getSingleResult() : 
- 결과(행)가 정확히 하나일 때 사용(결과가 없거나 하나보다 많으면 예외가 발생한다.)
##### getResultList() : 
- 결과(행)가 2개 이상일 때 사용하며, 컬렉션을 반환한다.(결과가 없으면 빈 컬렉션을 반환한다.)

###### 객체 타입이 어떤건지 물어보는 연산자
- instanceof
``` java
assertTrue(resultMenuName instanceof String); // 이런식으로 사용할 수 있다.
``` 

### JPQL 쿼리문 반환타입
- JPQL은 select절에서 나온 row와 매핑할 엔티티 타입으로 반환 타입을 설정한다.
``` java
String jpql = "SELECT m FROM Section01Menu as m WHERE m.menuCode = 7";
TypedQuery<Menu> query = entityManager.createQuery(jpql, Menu.class);
```

### JPQL 값 표현 문법
- 람다 문법과 메소드 참조 문법 두 가지가 있다.
- 단순히 출력할 때는 둘 다 사용해도 상관 없지만, 출력과 동시에 추가적인 작업이 필요할 경우
- 람다문법을 사용하는 것이 좋다.

#### 람다 문법
``` java
categoryCodeList.forEach(categoryCode -> System.out.println(categoryCode));
```
#### 메소드 참조 문법
``` java
categoryCodeList.forEach(System.out::println);
```

### 파라미터를 바인딩하는 방법
#### 1. 이름 기준 파라미터(named parameter)
- ':' 다음에 이름 기준 파라미터를 지정한다.
``` java
// PK나 Unique 제약조건이 아닌 경우 중복된 값이 미래에 발생할 가능성이 있기 때문에 현재는 리스트로 조회할 것이다.
String menuNameParameter = "한우딸기국밥";
String jpql = "SELECT m FROM Section02Menu m WHERE m.menuName = :menuName";
List<Menu> menuList = entityManager.createQuery(jpql, Menu.class)
        .setParameter("menuName", menuNameParameter)
        .getResultList();
```

#### 2. 위치 기준 파라미터(positional parameters) 
- '?' 다음에 값을 주고 값은 1부터 시작한다.
- (JDBC는 위치기준 파라미터만 지원한다.)
- 값이 들어갈 위치에만 위치홀더 사용 가능하다.
``` java 
    String menuNameParameter = "한우딸기국밥";
    String jpql = "SELECT m FROM Section02Menu m WHERE m.menuName = ?1";
    List<Menu> menuList = entityManager.createQuery(jpql, Menu.class)
            .setParameter(1, menuNameParameter)
            .getResultList();
``` 
## 프로젝션(projection)
- SELECT절에 조회할 대상을 지정하는 것을 프로젝션이라고 한다.
  (SELECT {프로젝션 대상} FROM

### 프로젝션 대상의 4가지 방식
#### 1. 엔티티 프로젝션
- 원하는 객체를 바로 조회할 수 있다.
- 조회된 엔티티는 영속성 컨텍스트가 관리한다.
    
#### 2. 임베디드 타입 프로젝션
- 엔티티와 거의 비슷하게 사용되며, 조회의 시작점이 될 수 없다.
- 엔티티 타입이 아닌 값으로 조회한 임베디드 타입은 영속성 컨텍스트에서 관리되지 않는다.

#### 3. 스칼라 타입 프로젝션
- 숫자, 문자, 날짜 같은 기본 데이터 타입이다.
- 스칼라 타입은 영속성 컨텍스트에서 관리하지 않는다.

#### 4. new 명령어를 활용한 프로젝션
- 다양한 종류의 단순 값들을 DTO로 바로 조회하는 방식으로 new 패키지명.DTO명을 사용하면
  해당 DTO로 바로 반환받을 수 있다.
- new 명령어를 사용한 클래스의 객체는 엔티티가 아니므로 영속성 컨텍스트에서 관리되지 않는다.

### 양방향 연관관계 역방향 객체 그래프 탐색
- 양방향 연관관계에 있는 엔티티의 경우 역방향 객체 그래프 탐색이 가능하다.
``` java
assertNotNull(categoryOfMenu.getMenuList());
categoryOfMenu.getMenuList().forEach(System.out::println);
// 즉, 영속성 컨텍스트 안에는 categoryOfMenu의 menuList도 포함이 되어있는 상태다.
// 그런데, categoryOfMenu만 출력하면 안나오는 이유는 toString에서 menuList를 빼놨기 때문이다.
// toString에서 menuList를 뺀 이유는 재귀현상 때문에 뺀 것이다.    
```

### 임베디드 타입(embedded type, 복합 값 타입 또는 내장 타입)
- 새로운 값 타입을 직접 정의한 것으로 주로 기본 값 타입을 모아서 만든 하나의 타입을 말한다.
- 엔티티의 필드 중 일부분을 하나의 임베디드 타입으로 정의하면 알아보기 쉽고, 
  재사용성이 높게 디자인할 수 있어 유지보수에 용의하다.

#### 임베디드 타입의 컴포넌트를 사용했을 때의 장점  
- 하이버네이트는 임베디드 타입을 컴포넌트(components)라고 부른다.
1. 재사용성이 좋다.
2. 응집도가 높다. <- 유사한 기능들을 하는 것을 잘 모아놓는 것이다.
3. 해당 임베디드 타입의 값만 사용하는 메소드를 엔티티에 만들어 활용할 수 있다.

#### 임베디드 어노테이션
- @Embeddable : 값 타입을 정의하기 위한 어노테이션
- @Embedded : 값 타입을 사용하는 곳에 적용하는 어노테이션
(참고로, @Embeddable과 @Embedded 어노테이션은 둘 중 하나만 적   용해도 돌아가지만, 둘 다 적는 것을 권장한다.

### 스칼라 타입 프로젝션
- 조회하려는 값이 단일 값일 경우 TypedQuery로 반환 타입을 단일 값에 대해 지정할 수 있지만,
  다중 열 컬럼을 조회하는 경우 타입을 지정하지 못한다. 
- 만약 여러 개의 열 컬럼 값을 조회하려면
  TypedQuery 대신 Query를 이용해서 Object[]로 행의 정보를 반환 받아 사용해야 한다.

``` java
String jpql = "SELECT c.categoryCode, c.categoryName FROM Section03Category c";
    List<Object[]> categoryList = entityManager.createQuery(jpql).getResultList();

    assertNotNull(categoryList);
    categoryList.forEach(row -> {
        for(Object column : row){
            System.out.print(column + " ");
        }
        System.out.println();
    });
```
- Object객체 타입으로 받으면 여러 번 거쳐가야되고, 다운 캐스팅이 필요해지기 때문에 번거롭다.

### new 명령어를 활용한 프로젝션
``` java
    String jpql = "SELECT new com.greedy.section03.projection.CategoryInfo(c.categoryCode, c.categoryName) " +
                "FROM Section03Category c";
        List<CategoryInfo> categoryInfoList = entityManager.createQuery(jpql, CategoryInfo.class).getResultList();
```
- 정리하자면, 단일 값에 대한 것은 TypedQuery를 사용하고 여러 개의 값은 Query를 사용해
  Object 타입으로 받아줄 수 있지만, 여러 개의 값을 
  담아놓을 용도의 클래스를 만들어 new 연산자로 사용하는게 좋은 방법이다.


## JPQL paging

- 페이징 처리용 SQL은 DBM에 따라 각각 문법이 다른 문제점을 안고 있다.
- JPA는 이런 페이징을 API를 통해 추상화해서 간단하게 처리할 수 있도록 제공해준다.

``` java  
    setFirstResult(int startPosition) : 조회를 시작할 위치(0부터 시작)
    setMaxResults(int maxResult) : 조회할 데이터 수

    RowBounds 파라미터는 마이바티스로 하여금 특정 개수 만큼의 레코드를 건너띄게 한다.
    RowBounds클래스는 offset과 limit 둘다 가지는 생성자가 있다.
    
    ex) int offset = 5;         // 조회를 건너뛸 행의 수
        int limit = 5;          // 조회할 수

    String jpql = "SELECT m FROM Section04Menu m ORDER BY m.menuCode DESC";

        List<Menu> menuList = entityManager.createQuery(jpql, Menu.class)
                .setFirstResult(offset)
                .setMaxResults(limit)
                .getResultList();

``` 
- 위의 로직으로 실행하면 오라클 12c 버전부터 추가된 문법으로 코드를 짜주어서
  아래와 같은 oracle 구문을 생성한다.
``` java
Hibernate: 
    select
        menu0_.MENU_CODE as menu_code1_1_,
        menu0_.CATEGORY_CODE as category_code2_1_,
        menu0_.MENU_NAME as menu_name3_1_,
        menu0_.MENU_PRICE as menu_price4_1_,
        menu0_.ORDERABLE_STATUS as orderable_status5_1_ 
    from
        TBL_MENU menu0_ 
    order by
        menu0_.MENU_CODE DESC offset ? rows fetch next ? rows only
``` 

## 그룹함수
- JPQL의 그룹 함수는 COUNT, MAX, MIN, SUM, AVG 기존 SQL의 그룹함수와 별 차이가 없다.

### 그룹함수 사용의 주의사항
  1. 그룹함수의 반환 타입은 결과 값이 정수이며, Long, 실수이면 Double로 반환된다.
  2. 값이 없는 상태에서 COUNT를 제외한 그룹함수를 사용하면 NULL이 되고, COUNT만 0이 된다.
      따라서 반환 값을 담기 위해 선언하는 변수 타입을 기본 자료형으로 하게되면 조회 결과를 언박싱할 때 NPE가 발생한다.
  3. 그룹함수의 반환 자료형은 Long or Double형이기 때문에 HAVING 절에서 그룹함수 결과 값과 비교하기 위해
      파라미터 타입은 Long or Double 타입으로 해야 한다.

#### COUNT를 사용해 조회할 시
``` java
int categoryCodeParameter = 4;

String jpql = "SELECT COUNT(m.menuPrice) FROM Section05Menu m WHERE m.categoryCode = :categoryCode";
Long countOfMenu = entityManager.createQuery(jpql, Long.class)
        .setParameter("categoryCode", categoryCodeParameter)
        .getSingleResult();
```
- throw - 강제로 예외를 만들어 던진다.
- throws - 예외 받은 것들을 위임할 때(CheckedException)

#### 카운트 제외한 그룹함수
- count를 제외한 그룹함수를 사용하면 NULL이 되고, COUNT만 0이 조회된다.
- 그래서 count를 제외한 그룹함수들은 타입을 일반 자료형 타입이 아닌,
  반환 값을 담는 변수를 Wrapper타입으로 선언해야 null 값이 반환되어도 NPE가 발생하지 않는다.
``` java    
ex) assertDoesNotThrow(() -> {
        Long sumOfPrice = entityManager.createQuery(jpql, Long.class)
                .setParameter("categoryCode", categortyCodeParameter).getSingleResult();
    });
```

#### groupby와 having을 사용한 조회
- 그룹함수의 반환 타입은 Long형이기 때문에 비교를 위해서 long 타입을 이용해야 한다.

``` java 
long minPrice = 50000L;

String jpql = "SELECT m.categoryCode, SUM(m.menuPrice) FROM Section05Menu m GROUP BY m.categoryCode " +
        "HAVING SUM(m.menuPrice) >= :minPrice";
List<Object[]> sumPriceOfCategoryList = entityManager.createQuery(jpql, Object[].class)
        .setParameter("minPrice", minPrice)
        .getResultList();
```

#### paging 참고 사이트
- https://mybatis.org/mybatis-3/apidocs/reference/org/apache/ibatis/session/RowBounds.html
- https://cofs.tistory.com/213

## Join
조인의 종류
1. 일반 조인 : 일반적인 SQL 조인을 의미
  1-1. 내부 조인(inner join)
  1-2. 외부 조인(outer join)
  1-3. 컬렉션 조인(one to many join) : JPA에서 의미상으로 부여하는 조인
  1-4. 세타 조인(cross join)

2. 페치(fetch) 조인
- SQL조인 종류가 아닌 JPQL에서 성능 최적화를 위해 제공하는 기능이다.
- 즉, 쿼리 한번에 다 담아서 SQL한번에 가져온다.
    
    연관된 엔티티나 컬렉션을 SQL한번에 함께 조회할 수 있다.
    지연로딩이 아닌 즉시 로딩을 수행하며, join fetch 명령어를 사용한다.
    패치 조인 대상에는 별칭을 줄 수 없다. 또한 둘 아상의 컬렉션은 패치 조인을 할 수 없으며,
    컬렉션을 패치조인 하게되면 페이징 API를 쓸 수 없다.

### 1. 일반 조인
#### 1-1. 내부 조인(inner join) - 스칼라 타입(String)
``` java
    String jpql = "SELECT m FROM Section06Menu m JOIN m.category c";
    List<Menu> menuList = entityManager.createQuery(jpql, Menu.class).getResultList();
```    
- 기존과 다르게 join에 on을 사용하지 않았다.
- 정상 동작하지만, 쿼리문을 한번 날렸는데 카테고리 코드를 여러번 조회하는 현상이 생긴다

#### N+1 문제란?
- 연관 관계가 설정된 엔티티를 조회할 경우에 조회된 데이터 갯수(n) 만큼 
  연관관계의 조회 쿼리가 추가로 발생하여 데이터를 읽어오는 현상
- 즉, 영속성 컨텍스트가 관리하고 있기 때문에 발생하는 문제다.
- 엔티티 프로젝션에서 일어나는 문제다.

##### N+1 해결 방법
- N+1 문제를 해결하는 방법에는 Fetch Join, EntityGraph 어노테이션,
  Batch Size 등의 방법이 있다.
- 지금 배우는 jpql 해결법으로는 Fetch Join을 사용할 것이다.

#### 1-2. 외부 조인(outer join) - 스칼라 타입(object[])
``` java
String jpql = "SELECT m.menuName, c.categoryName FROM Section06Menu m RIGHT JOIN m.category c";
List<Object[]> menuList = entityManager.createQuery(jpql, Object[].class).getResultList();

    menuList.forEach(row -> {

        Stream.of(row).forEach(col -> System.out.print(col + " "));
        System.out.println();
    });
    object[] 타입은 이렇게 출력한다.
```

#### 1-3. 컬렉션 조인(one to many join) - 컬렉션 객체
- 컬렉션 조인은 의미상 분류된 것으로 컬렉션을 지니고 있는 엔티티를 기준으로 조인하는 것을 말한다.
``` java
String jpql = "SELECT c.categoryName, m.menuName FROM Section06Category c LEFT JOIN c.menuList m";
List<Object[]> categoryList = entityManager.createQuery(jpql, Object[].class).getResultList();
``` 
- 외부조인과 비슷하지만, 방향을 기준으로 잡으면 된다.

#### 1-4. 세타 조인(cross join)
- 내부조인과 같은 결과 값을 가져온다.
- 결과 집합에 포함시킬 행의 조건으로 일치하는 값을 가진 행만 추출할 수 있다.
--> 조인과 결과가 같다, 하지만 내부 조인만 가능하다.
``` java
String jpql = "SELECT c.categoryName, m.menuName FROM Section06Category c, Section06Menu m WHERE c.categoryCode = m.category.categoryCode";
// String jpql = "SELECT c.categoryName, m.menuName FROM Section06Category c, Section06Menu m";
// 조인되는 모든 경우의 수를 다 반환하는 크로스 조인과 같은 결과가 나온다.(WHERE부분 확인)
List<Object[]> categoryList = entityManager.createQuery(jpql, Object[].class).getResultList();
```

### 2. 페치 조인(fetch join)
- 기본적으로 지연 로딩을 사용하기에 메뉴 정보를 조회해온 뒤,
  연관 엔티티를 조회하기 위한 쿼리를 필요할 때마다 실행하기 때문에 행 수 만큼 카테고리를 조회하기 위한 쿼리가 동작한다.

- 실제로 행 수 만큼 요청하지만, 영속성 컨텍스트 1차 캐시 안에 이미 값이 담겨있으면 DB를 들리지 않기 때문에,
  전체 행 수 만큼 쿼리문을 실행하지는 않는다.

#### 2-1. 기존 JPQL
``` java
String jpql = "SELECT m FROM Section06Menu m JOIN m.category c";
List<Menu> menuList = entityManager.createQuery(jpql, Menu.class).getResultList();
```

#### 2-2. fetch조인
``` java
String jpql = "SELECT m FROM Section06Menu m JOIN FETCH m.category c";
List<Menu> menuList = entityManager.createQuery(jpql, Menu.class).getResultList();
```

## 서브쿼리
- JPQL도 SQL처럼 서브쿼리를 지원한다.
- 하지만, SELECT, FROM 절에서는 사용할 수 없고,
  WHERE, HAVING 절에서만 사용할 수 있다.
- 즉, 조건식의 값으로만 사용이 가능하게끔 지원한다.

### 2-1. 서브쿼리를 이용한 메뉴 조회 테스트
``` java
String jpql = "SELECT m FROM Section07Menu m WHERE m.categoryCode = 
        (SELECT c.categoryCode FROM Section07Category c WHERE c.categoryName = :categoryName)";
List<Menu> menuList = entityManager.createQuery(jpql, Menu.class)
                    .setParameter("categoryName", categoryNameParameter)
                    .getResultList();
```

- 단일행 단일열의 결과가 나오는 서브쿼리가 아닌 일반 비교연산자는 사용이 불가능하다.
- 서브쿼리에서 사용가능한 연산자 종류는 다음과 같다.(SQL과 다르지 않다.)
  1. [NOT] EXISTS (서브쿼리)
  2. {ALL | ANY | SOME} (서브쿼리)
  3. [NOT] IN (서브쿼리)

### 2-2. 다중행 다중열 서브쿼리를 이용한 메뉴 조회 테스트
- WHERE절에 EXISTS를 이용한 상관쿼리문
- 원본이 변경될 시 참조하는 애들도 변경된다.
``` java
String categorytNameParameter = "식사";

String jpql = "SELECT m " +
        "FROM Section07Menu m " +
        "WHERE EXISTS (SELECT c.refCategoryCode " +
                        "FROM Section07Category c " +
                        "WHERE m.categoryCode = c.categoryCode " +
                            "AND c.refCategoryCode = (SELECT c2.categoryCode " +
                                                    "FROM Section07Category c2 " +
                                                    "WHERE c2.categoryName = :categoryName" +
                                                    ")" +
                        ")";
List<Menu> menuList = entityManager.createQuery(jpql, Menu.class)
        .setParameter("categoryName", categorytNameParameter)
        .getResultList();
```

## 동적 쿼리 : 
- 현재 우리가 하는 방식처럼 EntityManager가 제공하는 메소드를 이용하여 JPQL을 문자열로 런타임 시점에
  동적으로 쿼리를 만드는 방식을 말한다.
- (동적으로 만들어질 쿼리를 위한 조건식이나 반복문으 자바를 활용하면 된다.)

## 정적 쿼리 : 
- 미리 쿼리를 정의하고 변경하지 않고 사용하는 쿼리를 말하며, 미리 정의한 코드는 이름을 부여해서 사용하게 된다.
- 이런 것을 Named 쿼리라고 한다.
- 어노테이션 방식과 xml방식 두가지가 있는데 쿼리가 복잡할 수록 xml방식이 선호된다.
  (why? 문자여로 쿼리를 만드는 것은 힘들기 때문이다.)

