## Jpa 매핑
============================

### 자동 스키마 생성을 위한 설정    
- persistence에 프로퍼티 추가
- 자동으로 엔티티 만들어서 테이블을 생성해준다.
- 하지만 무한히 테이블이 생성될 수 있기에, 실생활에서 직접적으로 사용하진 않는다.
``` yml
<property name="hibernate.hbm2ddl.auto" value="create"/> <!-- 자동 스키마 생성 설정 -->
```

### DB초기화 전략 옵션 (property의 value에 위치하며 내용으로는)
1. none: DDL핸들링 작업을 수행하지 않는다.
2. create: 기존에 존재하는 스키마를 삭제하고 새로 생성한다.
3. create-drop: 스키마를 생성하고 어플리케이션이 종료될 때 삭제한다.
4. update: 기존 스키마를 유지하며 JPA에 의해 변경된 부분만 추가한다.
5. validate: 엔티티와 테이블이 정상적으로 매핑되어 있는지만 검증한다.

### JPA의 핵심    
- 엔티티와 테이블을 정확하게 매핑하는 것이 JPA의 핵심이다.
- 이를 위해 다양한 매핑 어노테이션(mapping annotation)이 지원되는데 크게 4가지로 분류할 수 있다.
  
#### 매핑 어노테이션의 종류 
1. 객체와 테이블: 
    @Entity - 해당 클래스를 엔티티 객체로 만들어주겠다.
              프로젝트 내 다른 패키지에도 동일한 엔티티가 존재하는 경우 
              해당 엔티티를 식별하기 위한 중복되지 않은 name을 지정해주어야 한다.
    @Table - DB 테이블을 매핑하겠다.

2. 기본키 매핑:
    @Id - 해당 테이블의 PK를 설정한다. value로 컬럼명 지정할 수 있음.
        복합키는 나중에 다른 설정으로 잡아줘야 한다.

3. 필드와 컬럼 매핑:
    @Column - 해당 테이블의 컬럼 매핑

4. 연관관계 매핑:
    @ManyToOne, @OneToMany, @JoinColumn
        연관관계(association, collection) 설정

### JPA를 사용하여 클래스를 엔티티로 사용할 때의 주의사항
1. 기본 생성자는 필수로 작성해야 한다.
2. final 클래스, enum, interface, inner class에서는 사용할 수 없다.
- 기존에 가지고 있던 클래스가 아닌 약간의 변형을 준 클래스이기 때문에 사용 x

### 자동 테이블 생성
- DDL구문은 autocommit 구문이기 때문에 트랜잭션을 적용하지 않아도 테이블은 생성된다.
- 하지만 트랜잭션을 적용하지 않으면 DML구문은 rollback되어 적용이 안된다.
    
- 생성된 테이블의 자료형은 java object의 자료형을 따르며, 크기는 따로 설정하지 않으면
  숫자는(10,0), 문자열은 255byte로 설정된다.
- 생성되는 컬럼의 순서는 PK가 우선이고, 일반 컬럼은 유니코드 오름차순으로 생성이 된다.

### 필드와 컬럼을 매핑하기 위한 어노테이션
- @Column : 컬럼을 매핑한다. 
        name, nullable, unique, columnDefinition, length 속성을 가진다.

- @Temporal : 날짜 타입 매핑
  
- @Lob : BLOB, CLOB 타입을 매핑한다.(varchar2보다 더 큰 범위의 어노테이션)
  
- @Transient : 매핑하지 않을 특정 필드에 작성한다. (매핑에서 제외한다)
  
- @Access : JPA가 엔티티에 접근하는 방식을 지정한다.
          필드에 직접접근, setter/getter메소드에 접근하는 방식
          
- @Ennumerated : Enum 타입을 매핑할 때 사용한다.(열거형)
#### 열거형이란 ?
- 정수형 상수에 이름을 붙여서 코드를 이해하기 쉽게 해준다는 뜻이다.
``` java
    const int ValueA = 1;
    const int ValueB = 2;
    const int ValueC = 3;
```


### 컬럼 매핑 시 @Column 어노테이션에 사용 가능한 속성들
1. name: 
    필드와 매핑할 테이블의 컬럼 이름
2. insertable: 
    엔티티 저장 시 이 필드도 같이 저장(true 기본 값)
3. updateable: 
    엔티티 수정 시 이 필드도 같이 수정(true 기본 값)
4. table: 
    하나의 엔티티를 두 개 이상의 테이블에 매핑할 때 사용(현재 클래스가 매핑된 테이블)
5. nullable: 
    null 값 허용여부 설정, not null 제약조건에 해당된다(true 기본 값)
6. unique: 
    한 컬럼에 unique 제약조건을 설정
    (기본값은 없으며, 두 개 이상의 컬럼에 unique 제약조건을 설정하기 위해서는 클래스 레벨에서 @Table의 uniqueConstraints 속성에 설정한다.)
7. columnDefinition: 
    데이터베이스 컬럼 정보를 직접 기입(필드의 자바 타입과 방언 정보를 사용해서 적절한 컬럼 타입을 생성한다.)
8. length: 
    문자 길이에 제약조건, String 타입에서만 사용(255 기본 값)

- 주로 name과 nullable을 사용하며 insertable과 updateable은 정보를 읽기만 하고 실수로 변경하게 될 것을 미연에 방지하고자 설정한다.
- 다만, 애플리케이션 레벨에서 DDL 구분을 직접 사용하지 않는 것이 더 바람직하다.

### JPA 매핑전략
- 데이터베이스마다 기본 키를 생성하는 방식이 서로 다르므로 이 문제를 해결하기는 쉽지 않다.
- JPA는 이런 문제들을 해결하기 위해 3가지 방식의 설정(PK 매핑전략)을 제공한다.
1. IDENTITY : 기본 키 생성을 데이터베이스에 위임한다. - MySql DB 방법
2. SEQUENCE : 데이터베이스 시퀀스를 사용해서 기본 키를 할당한다. - Oracle DB 방법 
3. TABLE : 키 생성 테이블을 사용한다.

- 오라클 데이터베이스는 시퀀스를 제공하지만, 
  MySQL은 시퀀스 대신 자동으로 숫자를 증가시켜주는 AUTO_INCREMENT 기능을 제공한다.
- AUTO_INCREMET같은 기능은 INDENMTITY 설정으로, 
  SEQUENCE를 이용하는 곳에서는 SEQUENCE 설정으로 기본 키 사용을 매핑할 수 있다.
- SEQUENCE나 INDENTITY 전략은 사용하는 데이터베이스에 의존하게 된다.

### @Id 적용이 가능한 자바 타입
1. 자바 기본형
2. 자바 Wrapper 타입
3. String
4. java.util.Date -> 주로 복합키로 사용할 수 있다.
5. java.sql.Date -> 주로 복합키로 사용할 수 있다.
6. java.math.BigDecimal
7. java.math.BigInteger

### @SequenceGenerator 어노테이션의 4가지 속성
1. name: 식별자 생성기 이름
2. sequenceName: 데이터베이스에 등록되어 있는 시퀀스 이름
3. initialValue: DDL처음 생성 시 시작하게 될 값(어차피 마지막 번호 들고오니까 숫자면 된다.)
4. allocationSize: 시퀀스 호출에 증가하는 수(성능 최적화에 사용되면 기본값은 50이다.)
  ex)@SequenceGenerator(
      name = "MEMBER_SQUENCE_GENERATOR",
      sequenceName = "SEQ_MEMBER_NO",
      initialValue = 1,               
      // 시퀀스의 시작값을 설정 - 설정하지 않으면 기본값은 1이다 (5로 설정하면 시작값이 5로 된다.)
      allocationSize = 50             
      // 생성할 시퀀스의 증가 값을 의미한다.
      )

### jpql 쿼리문 작성
- 쿼리문이랑 같은 맥락이지만, 실제 데이터 베이스에 날리는게 아니다. 
- 즉, memberNo는 DB의 정보가 아니라 엔티티 컬럼명이다.
- String jpql = "SELECT A.memberNo FROM SequenceMember A";
- 데이터베이스 관련된 내용은 대문자로 작성하고 테이블명 자리는 엔티티명을 기술한다.
- 그리고 반드시 테이블명(엔티티명)의 별칭을 반드시 기술해야 한다.
- 즉, A.memberNo는 SequenceMember A 엔티티의 컬럼명이 된다. DB가 아니다 .!!
    
### 메소드 참조
- 축약을 시켜주기 위한 용도
- soutc 단축키
- memberList.forEach(System.out::println); 
- :: 콜론 두개는 메소드 참조라고 부르고,
  println이라는 메소드를 참조하겠다는 뜻이다.

### @TableGenerator 어노테이션은 7가지 속성 값이 있다.
1. name : 식별자 생성기 이름
2. table : 데이터베이스에 등록되어 있는 시퀀스 생성용 테이블 이름
3. pkColumnName : 시퀀스 컬럼명(기본 값은 SEQUENCE_NAME_
4. valueColumnName : 시퀀스 값 컬럼명(기본 값은 NEXT_VAL)
5. pkColumnValue : 키로 사용할 값 이름
6. allocationSize : 시퀀스 호출에 증가하는 수
7. uniqueConstraints(DDL) : 유니크 제약조건을 지정할 수 있다.

## @Access
- JPA가 엔터티 데이터에 접근하는 방식을 지정할 수 있으며, 두 가지 방식이 있다.
1. 필드 접근 : 필드에 직접 접근하여 필드 접근 권한이 private여도 접근할 수 있다.
2. 프로퍼티 접근 : 접근자(getter)를 이용하여 접근하는 방식이다.

- @Access를 설정하지 않으면 @Id의 위치를 기준으로 접근방식이 설정된다.
- Lombok은 상황에 맞춰 설정해주어야 한다.
- 즉, 롬복이 모든 것을 알아서 해주지 않기 때문에,
- Lombok을 너무 의지하면 문제가 생길 가능성이 있다.

### Lombok 사용시 주의 점
- toString() 오버라이딩 시 양방향 연관관계는 재귀호출이 일어나기 때문에 stackOverFlow에러가 발생하게 된다.
- 따라서, 재귀가 일어나지 않게 하기 위해서는 엔티티의 주인이 아닌 쪽의 toString을 연관객체 부분이 출력되지 않도록 해야한다.
- 자동완성 및 롬복 라이브러리를 이용하는 경우 발생 가능성이 매우 높아지니 주의해서 사용해야 한다.
  
- 즉, @ToString 어노테이션을 너무 신용하면 생기는 문제점 중 하나인 재귀현상인데,
  서로 양방향으로 바라볼 때 서로한테서 값을 찾아오려고 하는 현상이 발생하여 
  스택오버플로 오류가 발생한다. 

#### 해결법
- 그걸 방지하기 위해서는 서로 바라보는 방향에서
  주인이 아닌 엔티티의 toString에서 주인에 대한 toString을 뺀다.

### @Access 어노테이션을 이용하여 접근 방식을 설정할 때 클래스 레벨과 필드 레벨에 설정이 가능하다.
1. 클래스 레벨 : 클래스 레벨에 @Access를 적용하면 모든 필드에 대해 적용한다.
2. 필드 레벨 : 해당 필드의 접근 방식을 필드 접근으로 바꿀 수 있다.
            @Id 어노테이션이 필드레벨에 존재하는 경우 해당 필드는 @Access(AccessType.FIELD)이다.
            (@Access 생략이 가능)
3. 프로퍼티 레벨 : Getter 메소드로 접근하는 프로퍼티 접근으로 바꿀 수 있다.
                @Id 어노테이션을 Getter에 작성하는 경우 @Access(AccessType.PROPERTY) 효과를 낸다.

- 즉, Lombok을 사용하면 값이 고정되어 있기 때문에,
  프로퍼티에 대한 내용만 바꿔주고 싶을 때 사용한다.
``` java
@Access(AccessType.PROPERTY)
public String getNickname() {

    System.out.println("getNickname()을 이용한 access확인...");
    return nickname + "님";
}
```
#### 정리하자면
1. 기존에 사용하던 Member클래스를 가지고와서 테스트할 경우 @Id가 필드에 선언되어 있는상태로
    테스트를 진행하게되면 @Id가 필드 레벨에 존재하기 때문에 필드 접근으로 되어 있어서 에러가 발생하게된다.
2. @Id을 Getter로 이동 --> (@Id가 프로퍼티 위에 존재하기 때문에 프로퍼티 접근이고 전역설정)
3. 클래스레벨에 @Access(AccessType.PROPERTY)를 설정하게되면
    (묵시적으로 프로퍼티 전역에 설정이지만 명시적으로 설정해도 결과는 동일하다.)
    --> 주의할 점은 이 때 @Id어노테이션이 필드에 있다면 전역설정은 필드로, @Id 어노테이션이 프로퍼티에 있다면 프로퍼티
        설정을 해야한다.
    --> @Id가 필드에 존재하고 @Access를 프로퍼티로 설정하게 되면 엔티티 매니저 자체를 생성하지 못하기 때문에 NPE가 발생하게 된다.
4. 전역 설정을 필드로 설정하고 특정 Getter(닉네임으로 테스트)에 @Access설정을 하게되면 해당 메서드만 프로퍼티 접근을 하게 된다.

- 결론적으로  @Id의 위치에 따라 @Access방식이 결정되며, 이는 전역 설정이다.
- @Access(AccessType.PROPERTY) 설정을 Getter에 지정하면 특정 필드만 Getter 메소드로 접근할 수 있으며,
  이 때 추가 로직이 필요한 경우 동작시킬 수 있다.

## 복합키

### 복합키가 존재하는 테이블 매핑의 경우 별도의 방법이 요구된다.
#### @Embeddable : 
- @Embeddable 클래스에 복합키를 정의하고 엔티티에 @Embeddeable를 이용해 복합키 클래스를 매핑한다.
- 이 때 @Embedded 클래스는 영속성 컨텍스트가 관리하는 엔티티는 아니다.
- 주의할 점은 복합키를 정의한 클래스는 직렬화 처리가 되어있어야 한다.

#### @IdClass :
- 복합키를 필드로 정의한 클래스를 이용해 엔티티 클래스에 복합키에 해당하는 필드에 @Id를 매핑한다.
- 주의할 점은 복합키를 정의한 클래스는 직렬화 처리가 되어있어야 한다.

##### @IdClass 사용 방법
- @IdClass(MemberPk.class) // 복합키로 쓰겠다는 클래스를 생성한 것을 지정해준다.
- 그리고 Entity클래스 필드의 복합키로 사용할 애들한테 @Id 어노테이션을 설정해준다.
- 얘도 @Embeddable과 동일하게 Serializable 인터페이스를 상속받아 직렬화 처리를 해야한다.

#### 두 방식의 특징
- 두 방식 모두 복합키 클래스는 영속성 컨텍스트가 관리하지 않는다는 특징을 가진다. 별 차이도 없다.
- 다만 @Embeddeable은 조금 더 객체지향 다운 방법이고, @IdClass는 관계형 데이터베이스에 가까운 방법이다.

``` java

      @Id
//    private int memberNo;
//    private String memberId; 이렇게 있던 것을

@EmbeddedId
    private MemberPk memberPk;
    // 이렇게 EmbeddedId 어노테이션을 붙여 필드로 지정하고
    // 새로이 MemberPk라는 entity 테이블을 생성하여 

@Embeddable
public class MemberPk implements Serializable {  
    // Serializable 인터페이스를 꼭 상속받아 직렬화 처리를 해야한다.
    @Column(name = "MEMBER_NO")
    private int memberNo;

    @Column(name = "MEMBER_ID")
    private String memberId;
    // 이렇게 컬럼으로 지정해준다.
    // 그럼 복합키 설정이 된 것이다.
```

- 제약조건 딕셔너리 뷰를 조회해보면 동일한 제약조건 이름으로 복합키로 PK 설정 되어있는 것을 확인할 수 있다.
- 특정 dbms에 종속적이다. 
- 그래서 복잡한 쿼리를 작성해야할 때 jpa의 한계점을 해결하기 위해서 nativeSql 사용

## 연관관계
### 연관관계란 ?
- 서로 다른 두 객체가 연관성을 가지고 관계를 맺는 것을 연관관계라고 한다.
- 객체 관계 매핑(ORM)에서 가장 어려운 부분이 연관관계를 매핑하는 일이다.

### 연관관계 분류
1. 방향(Direction)에 따른 분류
- 참조에 의한 객체의 연관관계는 단방향이다. 테이블의 연관관계는 외래 키로 이용하여 양방향 연관관계
- 객체 간의 연관관계를 양방향으로 만들고 싶으면 반대쪽에도 필드를 추가해서 참조를 보관하면 된다.
  
1-1. 단방향 연관관계
1-2. 양방향 연관관계

2. 다중성(Multiplicity)에 따른 분류
- 연관관계가 있는 객체 관계 혹은 테이블 관계에서 실제로 연관을 가지는(매핑되는)
   객체의 수(객체 관계) 또는 행(테이블 관계)의 수에 따라 분류된다.
2-1. 1:1(OneToOne) 연관관계
2-2. 1:N(OneToMany) 연관관계
2-3. N:1(ManyToOne) 연관관계
2-4. N:N(ManyToMany) 연관관계

### 필드 선언에서 정수형 자료형
- 필드 선언 시에 int와 Integer로 구분하는 이유는
- int는 null을 가질 수 없지만, Integer는 null을 가질 수 있다.
- 왜냐면 Integer는 래퍼 클래스 타입이라서 null을 가질 수 있다.
- 즉, null을 허용 할거냐 안할거냐에 따른 차이다.

### 연관관계 어노테이션과 속성
#### @JoinColumn : 외래키를 매핑할 때 사용한다.

##### @JoinColumn의 속성
- name - .매핑할 외래키 이름(기본값: 필드명 + _ + 참조하는 테이블의 기본키 컬럼명)
- referenceColumnName : 외래키가 참조하는 대상 테이블의 컬럼명(참조하는 테이블의 기본키 컬럼)
- foreignKey : 외래키 제약조건을 직접 지정할 수 있다. 이 속성은 테이블을 생성할 때만 사용한다.

#### @ManyToOne
- 다대일 관계에서 사용한다.

##### @ManyToOne의 속성
- optional : false로 설정하면 연관된 엔티티가 항상 있어야 한다.
- fetch : 글로벌 패치 전략을 설정한다.
- cascade : 영속성 전이 기능을 사용한다.(연관된 엔티티와 함께 영속성으로 관리)
        즉, 메뉴를 영속성에 넣을 때 카테고리까지 영속성에 넣어주겠다 라는 식이다.
- targetEntity: : 연관된 엔티티의 타입 정보를 설정한다. 
        그런데 이 기능은 거의 사용하지 않는다.(제네릭으로 타입 정보를 알 수 있기 때문)
- orhpanRemoval : true로 설정하면 고아객체 제거
            (영속성 컨텍스트 안에 카테고리가 메뉴를 참조하고 있는데, 메뉴를 삭제하게 되면
            카테고리는 고아객체가 되는데 orpahnRemoval을 true로 설정하면 메뉴를 삭제할 때
            고아 객체인 카테고리도 같이 삭제한다.)

##### @ManyToOne 사용 예제
- Menu는 다(N)이고 카테고리는 1이니까 Menu Entity에서는 ManyToOne(N:1) 을 사용한다.
``` java    
@JoinColumn(name = "CATEGORY_CODE") // category 테이블의 pk 컬럼명(필드명)
@ManyToOne
private Category category;
```

### 연관관계를 가지는 엔티티 조회 방법
- 연관관계를 가지는 엔티티를 조회하는 방법은 크게 2가지가 있다.
#### 1. 객체 그래프 탐색(객체 연관관계를 사용한 조회)

``` java
@DisplayName("다대일(N:1) 연관관계 객체 그래프 탐색을 이용한 조회 테스트")
@Test
public void test1(){

// given
int menuCode = 15; // 죽밥멸치튀김우동
// when
Menu foundMenu = entityManager.find(Menu.class, menuCode);
Category menuCategory = foundMenu.getCategory();
// then
assertNotNull(menuCategory);
System.out.println("menuCategory = " + menuCategory);
}
```

#### 2. 객체 지향 쿼리 사용(JPQL)
- 조인 문법이 sql과는 다소 차이가 있지만 직접 쿼리를 작성할 수 있는 문법을 지원한다.
- 연관관계 객체 그래프 탐색에는 테이블명을 사용했지만 여기서는 엔티티명을 사용하고,
  해당 엔티티의 필드명을 참조해서 사용한다.

- find는 영속성 컨텍스트에 있는 값을 가져온다면, jpql은 변경이 있다면,
  기존에 영속성 컨텍스트가 가지고 있던 것을 버리고 새로 업데이트 시킨다.

### DBMS에 종속되지 않는 JPQL 문법

#### 기존 방법
``` java
SELECT c.CATEGORY_NAME
  FROM Menu m
  Join Category c on(m.category_code = c.category_code)
```

#### JPQL 문법
``` java
    // given
    String jpql = 
    "SELECT c.categoryName FROM ManyToOneMenu m JOIN m.category c WHERE m.menuCode = 15";
    // when
    String category = entityManager.createQuery(jpql, String.class).getSingleResult();
                                                // 여기서 String은 위에 select 구문에 있는 가져올 값이
                                                // String형이기 때문에 String.class 사용한것이다.
    // then
        assertNotNull(category);
        assertEquals("일식", category);
        System.out.println("category = " + category);    

    // 바인드 변수를 통해서도 여러 개의 행도 조회가능하다.
    // given
    String jpql = "SELECT m FROM ManyToOneMenu m JOIN m.category c WHERE c.categoryName = :categoryName";
                //  SELECT m은 *과 같은 느낌인데 Menu의 객체를 다 가져오겠다는 뜻이다.
    // when
    List<Menu> menuAndCategoryList = entityManager.createQuery(jpql, Menu.class)
                                    .setParameter("categoryName", "한식")
                                    .getResultList();
    // then
    menuAndCategoryList.forEach(menuAndCategory -> System.out.println(menuAndCategory));
``` 

### 영속성 전이(cascade)
- 참조되는 관계인 메뉴와 카테고리를 한번에 같이 삽입하려면
  메뉴가 참조하려는 카테고리를 찾지 못하였기 때문에 참조 오류가 발생하는데,
  이걸 해결해주는게 영속성 전이다.
``` java   

// Category 객체를 참조하는 Menu를 삽입하기 전에 Menu 엔티티에서 ManyToOne 어노테이션 걸어놓은 곳에
// cascade 속성 설정을 해주면 삽입할 때 카테고리 먼저 삽입되고 메뉴가 삽입된다.

@JoinColumn(name = "CATEGORY_CODE") 
@ManyToOne(cascade = CascadeType.PERSIST)
private Category category;
``` 

### @OneToMany
- 일대다 관계를 매핑하는 정보
- 이전에는 Menu를 기준으로 하나의(1)Category를 조회하기 때문에 ManyToOne이 되었지만
- 여기서는 Category를 기준으로 여러 개(N개)의 Menu를 조회하기 때문에 OneToMany가 된다.

- 다대일 연관관계의 경우 해당 테이블과 참조하는 테이블까지 조인해서 조회하는데,
- 일대다 연관관계의 경우 해당 테이블만 조회하고, 연관된 메뉴 테이블은 아직 조회하지 않는다.

#### N:1
``` java
@DisplayName("다대일(N:1) 연관관계 객체 그래프 탐색을 이용한 조회 테스트")
@Test
public void test1(){

    // given
    int menuCode = 15; // 죽밥멸치튀김우동
    // when
    Menu foundMenu = entityManager.find(Menu.class, menuCode);
    Category menuCategory = foundMenu.getCategory();
    // then
    assertNotNull(menuCategory);
    System.out.println("menuCategory = " + menuCategory);
}
// 메뉴에 대한 정보를 카테고리 객체에 담아 영속성 컨텍스트에 넣는다.
```

#### 1:N
``` java
@DisplayName("일대다 연관관계 객체 그래프 탐색을 이용한 조회 테스트")
@Test
public void test1(){

    // given
    int categoryCode = 10;

    // when
    /* 일대다 연관관계의 경우 해당 테이블만 조회하고, 연관된 메뉴 테이블은 아직 조회하지 않는다. */
    Category categoryAndMenu = entityManager.find(Category.class, categoryCode);

    // then
    assertNotNull(categoryAndMenu);

    categoryAndMenu.getMenuList().forEach(System.out::println);
} 
// 설정한 카테고리 번호(10)에 대한 메뉴 리스트만 쭉 출력한다.
```

## 양방향 연관관계 매핑
- 데이터베이스 테이블은 외래키 하나로 양방향으로 조회가 가능하다. 
- 따라서 데이터베이스에 추가할 내용은 없다.
- 객체는 서로 다른 단방향 두 개를 합쳐서 양방향이라고 한다.
- 따라서 두 개의 연관관계 중 연관관계의 주인을 정하고, 주인이 아닌 연관관계 하나를 더 추가하는 방식으로 작성한다.
- 양방향 연관관계를 매핑하는 경우 반대 방향으로도 access하여 객체 그래프 탐색을 할 일이 많은 경우 하게된다.

### 연관관계의 주인
- 데이터베이스 연관관계와 매핑되고 외래키를 관리(등록, 삭제, 수정)하는 관리자를 뜻한다.
- 반면, 주인이 아닌 쪽은 외래키를 읽기만 할 수 있다.
- 연관관계의 주인은 mappedBy 속성을 사용하지 않는다.

#### 연관관계의 주인을 정하는 기준
- 양방향은 항상 외래키가 있는 곳을 기준으로 매핑하면 된다.
- 양방향은 연관관계의 주인(Owner)이라는 이름으로 인해 오해가 있을 수 있다.
- 비지니스 로직상 더 중요하다고 연관관계의 주인으로 선택하면 안된다.
- 비지니스 중요도를 배제하고 단순히 외래키 관리자 정도의 의미만 부여해야 한다.
- 즉, 연관관계의 주인 외래키의 위치와 관련해서 정해야지 비지니스 중요도로 접근하면 안된다.

##### mappedBy
- 객체 관계에서는 양방향 연관관계라는 직접적인 관계는 없고 서로 다른
  단방향 연관관계 2개를 로직으로 묶어 양방향인 것처럼 보이게 된다.

- 객체의 참조는 둘인데 외래키는 하나인 이 상황을 해결하기 위해 두 객체의 연관관계 중 
  하나를 정해서 테이블 외래키를 관리해야 하는데 이를 연관관계의 주인(Owner)라고 한다.
- 연관관계의 주인을 정하기 위해 연관관계의 주인이 아닌 객체에 mappedBy를 써서 
  연관관계의 주인 "객체의 필드명"을 매핑 시켜놓으면 로직으로 양방향 관계를 적용할 수 있다.
- 즉, 주인에게 참조될 객체의 필드명을 mappedBy로 매핑시켜놓는다.
``` java 
@OneToMany(mappedBy = "category")
private List<Menu> menuList; 
```

### 양방향 연관관계의 주의점
- 양방향 연관관계를 설정하고 가장 흔히 하는 실수는 연관관계 주인에게 값을 입력하고,
  주인이 아닌 곳에 값을 입력하지 않는 경우 외래키 제약조건 컬럼이 NotNull제약조건이 설정된 경우
  -> 이 때 null 값을 외래키 컬럼에 삽입할 수 없기 때문에 에러가 발생하게 된다.

--> 예제에서 연관관계의 주인은 Menu이고, 주인이 아닌 객체는 Category이다.

## 임베디드 타입(embedded type, 복합 값 타입 또는 내장 타입)
- 새로운 값 타입을 직접 정의한 것으로 주로 기본 값 타입을 모아서 만든 하나의 타입을 말한다.
- 엔티티의 필드 중 일부분을 하나의 임베디드 타입으로 정의하면 알아보기 쉽고, 
  재사용성이 높게 디자인할 수 있어 유지보수에 용의하다.

### 임베디드 타입 컴포넌트
- 하이버네이트는 임베디드 타입을 컴포넌트(components)라고 부른다.

#### 임베디드 타입의 컴포넌트를 사용했을 때의 장점 
1. 재사용성이 좋다.
2. 응집도가 높다. <- 유사한 기능들을 하는 것을 잘 모아놓는 것이다.
3. 해당 임베디드 타입의 값만 사용하는 메소드를 엔티티에 만들어 활용할 수 있다.

### 임베디드 어노테이션
- @Embeddable : 값 타입을 정의하기 위한 어노테이션
- @Embedded : 값 타입을 사용하는 곳에 적용하는 어노테이션
- (참고로, @Embeddable과 @Embedded 어노테이션은 둘 중 하나만 적용해도 돌아가지만, 둘 다 적는 것을 권장한다.

    