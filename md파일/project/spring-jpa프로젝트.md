#### 기본 설정

##### configuation
- BeanConfiguration
``` java
@Configuration
@ComponentScan(basePackages = "com.greedy.data") // 해당 패키지에 있는 빈들을 스캔하겠다.
@EnableJpaRepositories(basePackages = "com.greedy.data") 
// @EnableJpaRepositories JpaRepository를 상속받은 인터페이스들을 스캔 후에 사용할 클래스들로 동적으로 생성해주는 역할이다.
// 인터페이스는 혼자서 생성할 수 없기 때문에 얘가 그 역할을 동적으로 해준다.
// 그래서 JpaRepository를 사용할 때는 얘가 꼭 필요하다.
public class BeanConfiguration {

    @Bean
    public ModelMapper modelMapper(){

        return new ModelMapper();
    }
}
 ```

- EntityConfiguration
``` java
@Configuration
@EntityScan(basePackages = "com.greedy.data") // entity 어노테이션이 걸려있는 클래스들을 스캔
public class JpaConfiguration {
    
}
```

#### 작성 시작
##### main화면 구성
- 먼저 메인을 잡아줄 controller를 생성하고 GetMapping으로 main.html 뷰를 잡아준다.
- 또한 버튼을 눌렀을때 redirect 할 수 있게 postMapping으로 redirect 잡아준다.
``` java
main controller
@GetMapping(value = {"/", "/main"})
public String main(){

    return "/main/main";
}

@PostMapping("/")
public String redirectMain(){

    return "redirect:/";
}
```

``` html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1 align="center">GREEDY RESTAURANT에 오신 것을 환영합니다.</h1>

    <button onclick="location.href='/menu/7'">7번 메뉴 보기</button>
    <button onclick="location.href='/menu/list'">메뉴 보기</button>
    <button onclick="location.href='/menu/regist'">메뉴 입력하기</button>
    <button onclick="location.href='/menu/modify'">메뉴 수정하기</button>

</body>
</html>
```
<img src="/md-img/pj-main1.png">

이렇게 화면이 구성된다.

##### 7번 메뉴보기 버튼 구성
- 아래와 같이 MenuController를 생성하고 RequestMapping으로 공통되는 부분 잡아주고, 7번 메뉴보기 화면을 잡아준다.
- 

