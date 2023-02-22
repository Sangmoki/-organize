01-31 수업 내용
=========================== 
## Spring Data Jpa

### Spring data jpa란 ?
- spring framework에서 JPA를 편리하게 사용할 수 있도록 지원하는 프로젝트
- CRUD 처리를 위한 공통 인터페이스 제공
- repository 개발 시 인터페이스만 작성하면 실행 시점에 스프링 데이터 JPA가 구현 객체를 동적으로 생성해서 주입한다.

- 데이터 접근 계층을 개발할 때 구현 클래스 없이 인터페이스만 작성해도 개발을 완료할 수 있도록 지원

- 공통 메소드는 스프링 데이터 JPA가 제공하는 org.springframework.date.jpa.repository.JpaRepository 인터페이스에

  count, delete, deleteAll, deleteAll, deleteById, existsById, findById, save ..

- Spring data Jpa를 사용하면 JPA에서 사용했던 기존의 EntityManagerFactory,
    EntityManager, EntityTransaction같은 객체가 필요없게 된다.
    (Spring data Jpa 내부에서 처리를 진행한다.)
    즉, 기존 Test에서 진행하던 EntityManagerFactory들을 Spring data Jpa가
알아서 생성하고 관리해준다. 그걸 위해서 JpaRepository 인터페이스를 상속받는 것이다.

- Repository는 DAO와 동일한 개념으로 사용 (Repository를 이용하여 실질적으로     데이터베이스 연동을 처리한다.)

- Repository 인터페이스들의 상속 구조
Repository <- CrudRepository <- PagingAndSortingRepository <- JpaRepository
    
Repository : 기능이 거의 없음
CrudRepository : 기본적인 CRUD 기능을 제공한다.
PagingAndSortingRepository : 페이징 및 정렬 처리하고자 할 경우 사용
JpaRepository : Spring data Jpa에서 추가한 기능을 사용하고자 할 때 사용

#### 기억해두면 좋을 만한 메소드
##### JpaRepository 인터페이스의 메소드
- long count() : 모든 엔티티의 개수 리턴
- void delete(Id) : 식별 키를 통한 삭제
- void delete(Iterable<? Extends T> : 주어진 모든 엔티티 삭제
- boolean exist(ID) : 식별 키를 가진 엔티티가 존재하는지 확인
- findAllById(Iterable<ID> ids) : id에 해당하는 모든 엔티티 목록 리턴
- findAll() : 엔티티 전체 목록 리턴
- Iterable<T> findAll(Iterable<ID>) : 해당 식별 키를 가진 엔티티 목록 리턴
- Optional<T> findById(ID) : 해당 식별 키에 해당하는 단일 엔티티 리턴
- saveAll<Iterable> : 여러 엔티티들을 한번에 등록, 수정
<!-- - <S extends T>S save<S entity> : 하나의 엔티티를 등록, 수정 -->

### 쿼리 메소드
- JPQL을 메소드로 대신 처리할 수 있도록 제공하는 기능
메소드의 이름으로 필요한 쿼리를 만들어주는 기능으로 "find + 엔티티 이름 + By + 변수 이름"과 같이 네이밍 룰만 알면 사용 가능하다.
ex) findMenuByCode() : Menu entity에서 code 속성에 대한 조건처리하여 조회한다.

엔티티 이름을 생략하고 쓸 수도 있는데 이는 해당 Repository 인터페이스의 제네릭에 해당하는 엔티티를 자동으로 인식하기 때문이다.
ex) Repository 인터페이스가 JpaRepository<Menu, Integer>를 상속받고 있다면,
    findByCode() : Menu 엔티티에서 Code 속성에 대해 조건처리하여 조회하겠다.
    위에와 똑같은 결과가 나온다.
    즉, 명시적으로 작성해주지 않아도 제네릭으로 엔티티가 명시적으로 걸려있으면 jpa가 추측해서 자동으로 인식한다.

#### 쿼리 메소드 유형
- And &nbsp;&nbsp;&nbsp;&nbsp; ex) findByCodeAndName -> where x.code = ?1 and x.name = ?2
- Or  &nbsp;&nbsp;&nbsp;&nbsp; ex) findByCodeOrName -> where x.code = ?1 or x.name = ?2
- Between &nbsp;&nbsp;&nbsp;&nbsp; ex) findByPriceBetween -> where x.price between ?1 and ?2
- LessThan &nbsp;&nbsp;&nbsp;&nbsp; ex) findByPriceThan -> where x.price < ?1
- LessThanEqual &nbsp;&nbsp;&nbsp;&nbsp; ex) findByPriceLessThanEqual -> where x.price <= ?1
- GreaterThan  &nbsp;&nbsp;&nbsp;&nbsp; ex) findByPriceGreaterThan -> where x.price > ?1
- GreaterThanEqual  &nbsp;&nbsp;&nbsp;&nbsp;  ex) findByPriceGreaterThanEqual -> where x.price >= ?1
- After &nbsp;&nbsp;&nbsp;&nbsp; ex) findByDateAfter -> where x.date > ?1 // 특정 날짜 이후
- Before  &nbsp;&nbsp;&nbsp;&nbsp; ex) findByDateBefore -> where x.date < ?1 // 특정 날짜 이전
- IsNull &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameIsNull -> where x.nmame is null
- IsNotNull, NotNull &nbsp;&nbsp;&nbsp;&nbsp; ex) findByName[Is]NotNull -> where x.name is not null
- Like &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameLike -> where x.name like ?1
- NotLike &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameNotLike -> where x.name not like ?1
- StartingWith &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameStartingWith -> where x.name like ?1 || '%'
- EndingWith &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameEndingWith -> where x.name like '%' || ?1
- Containing &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameContaining -> where x.name like '%' || ?1 || '%'
- OrderBy &nbsp;&nbsp;&nbsp;&nbsp; ex) findByPriceOrderByCodeDesc -> where x.price = ?1 order by x.code desc
- Not &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameNot -> where x.name <> ?1
- In &nbsp;&nbsp;&nbsp;&nbsp; ex) findByNameIn(Collection<name> names) -> where x.name in (?1)

### Spring data jpa의 페이징
``` java
System.out.println("조회한 내용 목록 : " + menuList.getContent());
System.out.println("총 페이지 수 : " + menuList.getTotalPages());
System.out.println("총 메뉴 수 : " + menuList.getTotalElements());
System.out.println("해당 페이지에 표시될 요소 수 : " + menuList.getSize());
System.out.println("해당 페이지의 실제 요소 수 : " + menuList.getNumberOfElements());
System.out.println("첫 페이지 여부 : " + menuList.isFirst());
System.out.println("마지막 페이지 여부 : " + menuList.isLast());
System.out.println("정렬 방식 : " + menuList.getSort());
System.out.println("여러 페이지 중 현재 인덱스 : " + menuList.getNumber());
```

### Entity 클래스 설계 방법
1. 데이터 베이스를 그대로 설계
- Member라는 메인 엔티티에 여러 연관관계들이 섞여있으면 mapped by 속성을 통해
  1:N 연관관계를 설정하는 경우인데, 이렇게 되버리면 관리는 편하겠지만, 공유차원에서
  소스코드가 팀원과 다르다면 서로 오류가 발생하는 경우가 생긴다.

2. 업무별로 나눠서 설계
- 각 연관관계(주문, 배송, 팀 등등)에 맞는 멤버 엔티티 클래스를 생성하여 
  각 연관관계에 memberList를 joinColumn으로 만드는 경우다. 이렇게 되버리면
  클래스를 여러 개 만들어야 하는 단점이 존재하지만, 소스코드가 달라도 그거에 대한
  오류만 발생하기 때문에 이 방법으로 채택해서 프로젝트를 진행할 것이다.

### entityManager를 사용한 스프링 부트 프로젝트
- 기존 semi 프로젝트에서는 jpa에서 제공하는 entityManager를 사용하지 않았다.
- 이제는 entityManager를 활용하여 프로젝트를 진행할 것이다.
- Menu entity 객체를 일반 MenuDTO 객체로 바꾸어 주어야 오류가 발생하지 않는다.
  -> maven repository에서 modelmapper 라이브러리 가져오고 BeanConfiguration에
  라이브러리 메서드를 정의하고
- Bean으로 등록된 객체를 찾는 BeanConfiguration 클래스를 생성하고 @ComponentScan 설정을 해주고,
    ModelMapper를 사용하기 위한 Bean 객체를 하나 생성해준다.
    ```java
    @Bean
    public ModelMapper modelMapper(){

        return new ModelMapper();
    }
    ```
- Entity로 등록된 객체를 찾는 JPAConfiguration 클래스를 생성해 @EntityScan 설정을 해준다.

#### ModelMapper 라이브러리를 사용한 타입 변환
- Menu entity 객체를 일반 MenuDTO 객체로 바꾸어 주어야 오류가 발생하지 않는다.
- ModelMapper 라이브러리는 entity 객체 타입을 DTO 타입으로 바꾸거나,
    반대로 DTO타입을 entity 객체로 바꿀 때 사용한다.

<b>1. entity 객체 타입을 DTO 타입으로 변환</b>
- 이후 MenuService의 의존주입한 생성자 부분에 ModelMapper를 추가해준 후 map 메서드를 사용해 entity를 DTO로 자동 변환해준다.
    ``` java
    기존 : 
    return menuRepository.findMenuByCode(entityManager, menuCode);

    변환 :
    return modelMapper.map(menuRepository.findMenuByCode(entityManager, menuCode), MenuDTO.class);
    ```
<b>2. DTO타입의 객체 타입을 entity 타입으로 변환</b>
- ``` java
  menuRepository.registNewMenu(entityManager, modelMapper.map(newMenu, Menu.class));
  ```
##### 단일 값과 다중 값 가져올 때 차이점
- 단일 값은 modelMapper.map으로 뽑아주니까 변환만 한다.
- 리스트 값은 entity 객체 하나를 MenuDTO 타입으로 변환하고, collect 메서드로 리스트 형식으로 변환한 후 stream() 문법을 통해 이러한 작업을 반복한다. 
  ``` java
  return menuList.stream().map(menu -> modelMapper.map(menu, MenuDTO.class)).collect(Collectors.toList());
  ```

#### 기존 내용에서 추가적인 내용
- 기존에는 repository(dao)에서 마이바티스처럼 db연결을 했지만 spring data에서는
jpa 인터페이스를 상속받는 interface를 생성하는데 이 interface는 entity랑 연결된다.
주의! - pk에 있는 값도 명시적으로 작성해주어야 한다.
``` java
public interface MenuRepository extends JpaRepository<Menu, Integer> {
                        // JpaRespository<T, ID>
                        // T는 엔티티 타입, ID는 해당 엔티티의 PK의 타입이다
}
```




