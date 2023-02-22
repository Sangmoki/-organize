=================== 01.16 수업내용
REST API란?
    Representational State Transfer의 약자로 소프트웨어 프로그램 아키텍처의 한 형식이다.
    자원의 이름으로 구분하여 해당 자원의 상태를 주고 받는 모든 것을 의미한다.
    REST는 기본적으로 웹의 기존 기술과 HTTP 프로토콜을 그대로 활용하기 때문에 웹의 장점을 최대한 활용할 수 있는 아키텍처 스타일이다.
    ex) 주소, 결제 API 등등이 REST API인데, 어떤 형식으로 올 지 모르니까 Json 형식으로 요청을 받아 사용할 수 있게 정의 해 놓은게 REST API 이다.
    즉, 어떤 기능들을 할건지에 대해 정의 되어 있는걸 호출해 사용하는 것이다.

REST의 구체적 개념
    웹의 모든 자원에 고유한 ID인 HTTP url을 부여하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원에 대한 CRUD 연산을 적용한다.
    즉, REST는 자원기반구조(ROA : Resource Oriented Architecture) 설계의 중심에 Resource가 있고, HTTP Method를 통해 Resource를 처리하도록 설계된 아키텍쳐를 의미한다.
    
    웹의 장점을 그대로 활용할 수 있는 아키텍쳐 스타일이다.
    url은 자원의 위치를 나타낸다.
    ex) board/list 처럼 

    최근 서버 프로그램은 웹 브라우저 뿐 아니라 모바일 등 다양한 어플리케이션과 통신에 대응할 수 있어야 한다.
    플랫폼에 맞춰 새로운 서버를 만들지 않게 하기 위해 범용적으로 사용성이 보장되는 서버 디자인이 필요하게 되었다.

REST의 구성
    1. 자원(Resource) : url
        모든 자원에 고유한 ID가 존재하고, 이 자원은 서버에 존재한다.
        자원을 구분하는 ID는 /orders/order-id/1과 같은 URL 이다.
    2. 행위(Verv): HTTP Method
        HTTP 프로토콜의 Method를 사용한다.
        Method      역할
        GET         해당 리소스를 조회한다.
        POST        해당 URL을 요청하면 리소스를 생성한다.
        PUT         해당 리소스를 수정한다.
        DELETE      해당 리소스를 삭제한다.
    3. 표현
        REST에서 하나의 자원은 Json, XML, text, RSS 등 여러 형태의 Representation으로 나타낼 수 있다.
        최근에는 Json으로 주고 받는 것이 대부분이다.

REST의 특징
    1. 클라이언트 / 서버 구조
        클라이언트와 서버의 역할이 명확하게 구분된다.
        구분               역할
        Client          사용자 인증이나 로그인 정보 등을 직접 관리하고 책임진다.
        Server          API를 제공하고 비지니스 로직 처리 및 저장을 책임진다.
    
    2. 무상태성(Stateless)
        REST는 HTTP의 특성을 이용하기 때문에 무상태성을 가진다.
        서버에서는 상태정보를 기억할 필요가 없고, 들어온 요청에 대한 처리만 해주면 되기 때문에 구현이 단순해진다.
    
    3. 캐시 처리 가능(Cacheable)
        HTTP의 명세를 따르는 REST의 특징 덕분에 캐시 사용이 가능하다.
        캐시 사용을 통해 응답 시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버의 자원 이용률 향상
    
    4. 자체 표현 구조(Self - Descriptiveness)
        REST API의 메세지만으로 그 요청이 어떤 행위를 하는지 손쉽게 이해할 수 있다.
        ex) 해당 주소만 보고 해당 요청에 대한 내용이 어떤건지 기입해놓으면 얘가 유추해준다.
    
    5. 계층화(Layered System)
        클라이언트와 서버가 분리되어 있기 때문에 중간에 프록시 서버, 암호화 계층 등 중간 매체를 사용할 수 있어서 자유도가 높다.,
    
    6. 유니폼 인터페이스(Uniform Interface)
        HTTP 표준만 맞추면 모든 플랫폼에서 사용이 가능하며, URL로 지정한 리소스에 대한 조작이 가능하게 하는 아키텍쳐 스타일을 말한다.
        즉, 특정 언어나 기술에 종속되지 않는다.

REST API의 설계 규칙
    중심 규칙
        중점으로 URI는 정보의 자원을 표현해야 한다.
        자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)으로 표현한다.

    세부 규칙
        1. 슬래시 구분자(/)는 계층 관계를 나타내는데 사용한다.
        2. URI 마지막 문자로 슬래시(/) 를 포함하지 않는다.
            2-1. URI에 포함되는 모든 글자는 리소스의 유일한 식별자로 사용되어야 하며,   
                 URI가 다르다는 것은 리소스가 다르다는 것을 의미한다.
        3. 하이픈(-)은 URI 가독성을 높이는데 사용한다. 하지만 같은 목적으로 밑줄(_)은 IRL에 사용하지 않는다.
        4. URL 경로에는 소문자가 적합하다. 가급적이면 URI 경로에 대문자 사용은 피하도록 한다.
        5. 파일 확장자는 URI에 포함하지 않는다.
            REST API에서는 메세지 바디 내용의 포맷을 나타내기 위한 파일 확장자를 URI 안에 포함시키지 않는다.(Accept Header 사용)
            ex) GET : http://restapi.exam.com/orders/2
            {Accept:image/jpg}
        6. 리소스 간에 연관 관계가 있는 경우
            /리소스명/리소스ID/관계가 있는 다른 리소스 명
            ex) GET: /users/2/orders
            (일반적으로 소유의 관계를 표현할 때 사용)

RESTful 이란
    HTTP와 URI 기반으로 자원에 접근할 수 있도록 제공하는 애플리케이션 개발 인터페이스이다.
    기본적으로 개발자는 HTTP 메소드와 URI만으로 인터넷에 자료를 CRUD 할 수 있다.
    'REST API'를 제공하는 웹 서비스를 'RESTful' 하다고 할 수 있다.
    RESTful은 REST를 REST답게 쓰기 위한 방법으로, 누군가가 공식적으로 발표한 것은 아니다.

RESTful API 개발 원칙
    1. 자원을 식별할 수 있어야 한다.
        URL만으로 내가 어떤 자원을 제어하려고 하는지 알 수 있어야 한다.
        Server가 제공하는 정보는 Json이나 XML형태로 HTTP body에 포함되어 전송시킨다.
    2. 행위는 명시적이어야 한다.
        REST는 아키텍쳐 혹은 방법론과 비슷하다. 따라서 이런 방식을 사용해야 한다고 강제적이지 않다.
        기존의 웹 서비스 처럼, GET을 이용해서 Update와 Delete를 해도 된다.
        다만 REST 아키텍쳐에는 부합하지 않으므로 REST를 따른다고 할 수는 없다.
    3. 자기 서술적이어야 한다.
        데이터에 대한 메타정보만 가지고도 어떤 종류의 데이터인지, 데이터를 위해서 어떤 어플리케이션을 실행해야 하는지를 알 수 있어야 한다.
    4. HATEOAS(Hypermedia as the Engine of Application State)
        클라이언트 요청에 대해 응답을 할 때, 추가적인 정보를 제공하는 링크를 포함할 수 있어야 한다.
        REST는 독립적으로 컴포넌트들을 손쉽게 연결하기 위한 목적으로도 사용된다.
        따라서, 서로 다른 컴포넌트들을 유연하게 연결하기 위해선, 느슨한 연결을 만들어 줄 것이 필요하다.
        이때 사용되는 것이 바로 링크이다.
        HATEOAS는 서버가 독립적으로 진화할 수 있도록 서버와 서버, 서버와 클라이언트를 분리할 수 있게 한다.

REST의 단점
    REST는 point to point 통신모델을 기본으로 한다. 따라서 서버와 클라이언트가 연결을 맺고 상호작용해야하는
    어플리케이션의 개발에는 적당하지 않다.
    REST는 URI, HTTP를 이용한 아키텍처링 방법에 대한 내용만을 담고 있다.
    보안과 통신규약 정책 같은 것은 전혀 다루지 않는다.
    따라서, 개발자는 통신과 정책에 대한 설계와 구현을 도맡아서 진행해야 한다.
    HTTP에 상당히 의존적이다. REST는 설계원리이기 때문에 HTTP와는 상관없이 다른 프로토콜에서도
    구현할 수 있기는 하지만, 자연스러운 개발이 힘들다.
    다만 REST를 사용하는 이유가 대부분의 서비스가 웹으로 통합되는 상황이기에 큰 단점이 아니게 되었따.
    CRUD 4가지 메소드만 제공한다. 대부분의 일들을 처리할 수 있지만, 4가지 메소드 만으로 처리하기엔 모호한 표현이 있다.
    

@RestController 란?
    @Controller 와 @ResponseBody 어노테이션을 합친 어노테이션을 의미한다.
    클래스 레벨에 작성하며, 해당 클래스 내 모든 핸들러 메소드에 @ResponseBody 어노테이션을 묵시적으로 적용한다.

    여기서 @ResponseBody 는 비동기통신을 하기위해서는 클라이언트에서 서버로 요청 메세지를 보낼 때, 본문에 데이터를 담아서 보내야 하고, 
    서버에서 클라이언트로 응답을 보낼때에도 본문에 데이터를 담아서 보내야 한다. 이 본믄이 바로 body인데
    
@RestController와 @Controller의 차이점 
    두 어노테이션의 가장 큰 차이점은 HTTP Response Body가 생성되는 방식이다.
    전통적인 mvc의 컨트롤러인 @Controller는 주로 View를 반환하기 위해 사용한다.


postman api 사용 - 테스트해서 응답이 잘 들어오는지 확인할 수 있다(Json형식으로 변환해서)
create new workspace - new collection - add a request

List.of는 java 9버전부터 지원하는 메소드
배열을 리스트로 변환하기 위해서 사용하는 방식 --> 불변객체(객체를 바꾸지 못하게)
Arrays.asList() 로도 동일하게 변경이 가능 --> 불변객체가 아니다.(객체 수정이 가능)

ImageFile 응답
produces 속성으로 타입을 지정해주지 않으면 text/html로 응답하기 때문에 이미지가 텍스트 형태로 전송된다.
그걸 방지하기 위해 produces=MediaType.IMAGE_PNG_VALUE 로 지정해줘야 한다.
                                    IMAGE_PNG는 파일 형식이니 이미지에 따라 JPG, GIF로 바꿔주어도 된다.
produces는 response header의 content-type 설정이다.
즉, ajax를 사용해서 헤더를 보내줄 때 applicationJason 방식으로 형태를 보내주기 위한 속성

ResponseEntity란?
    결과 데이터와 http 상태 코드를 직접 제어할 수 있는 클래스이다.
    HttpStatus, HttpHeaders, HttpBody를 포함한다.

ResponseEntity를 사용하면json 형태의 객체를 만들어 따로 객체에 담아 나누어서 사용할 수 있다.

view가 어떤 내용에 대한 요청을 방법에 따라 다르게 나타낼 수 있다.
기존에는 model에 담아 타임리프 문법으로 뿌려줬는데 여기서는 json 형태로 바꾼 객체를 가지고 script로 뿌려준다.

UserDTO foundUser = users.stream().filter(user -> user.getNo() == userNo).collect(Collectors.toList()).get(0);
users.stream()은 반복을 내부적으로 돌리면서 입력한 번호에 대한 내용을 담아서 .collect() 
                             foreach, for
                            if(user.get(i).getNo() == userNo)
즉, stream() = 반복, getNo() == userNo 조건, collect() 부합하는거 꺼내오기

조회는 @GetMapping
추가는 @PostMapping
변경은 @PutMapping
삭제는 @DeleteMapping

ResponseEntity 클래스의 created() 메서드는 추가, 수정, 삭제에 대한 내용에 사용한다.
즉, created() 메서드는 재정의 한다는 뜻의 메서드 이다.

filter(user -> user.getNo() == userNo)
filter() 메서드는 해당 조건에 대한 값이 true면 뒤에 로직을 실행시킨다. 말그대로 필터다. 한번 거쳐가~

ResponseEntity.noContent().build();
삭제시켜주고 성공했다 라고하는 200코드를 반환해준다.

Postman은 해당 url 주소를 요청했을때 post방식인지, delete방식이지 모르니까
응답을 확인하기 위한 툴이라고 생각하면 된다.
ex) localhost:8888/entity/users
즉, 위의 url을 요청하면 get으로만 나온다. 그러니까 툴로 확인한다.
지금까지 했던 내용들은
실제 코드에서 발생하는 오류를 방지하기 위해 테스트 하는 용도다.

====================    01.17 수업 내용

예외처리에 관한 내용
    ExceptionController 라는 클래스를 만들어 전역에서 발생하는 에러를 잡아
    예외처리를 해주기 위해 @ControllerAdvice라는 어노테이션을 선언해주었고, 이 어노테이션은
        예외가 발생한 기점에 있는 해당 내용을 체크해서 선언하겠다.  
        즉, 전역에서 발생하는 예외처리를 잡아 처리해주는 어노테이션이다.
    ex)
    @ControllerAdvice  // 예외가 발생한 기점에 있는 해당 내용을 체크를 해서 선언하겠다.
    public class ExceptionController {

        @ExceptionHandler(UserNotFoundException.class) // 해당 타입의 예외를 잡아주겠다 .class로 잡아준다.
        public ResponseEntity<ErrorResponse> handlerUserRegistException(UserNotFoundException e) {

            String code = "ERROR_CODE_00000";
            String description = "회원 정보 조회 실패";
            String detail = e.getMessage();

            return new ResponseEntity<>(new ErrorResponse(code, description, detail), HttpStatus.NOT_FOUND);
                                        // HttpStatus(상태값)
            }
    }

Post로 registUser(@RequestBody UserDTO user){

    Soutv 해서 user 찍고
    postman으로 테스트 하였을 때 값을 입력 안하면 400번 에러(어떠한 제약조건도 없을 때)
}

@Valid 
    밸리데이션 체크 - 값을 입력을 받을 때 공백이나 null이 들어가면 안되게끔 유효성 검사해주는 어노테이션
    이거를 붙여야만 사용하려는 DTO(@ReqeustBody UserDTO user) 필드에 유효성 검사를 할 수 있다.

유효성 검사와 예외처리, 테스트 코드에 대한 참고 사이트
https://jyami.tistory.com/55

DTO에 사용하는 제약조건 어노테이션
@Null                       // null만 허용한다.
@NotNull                    // null을 허용하지 않습니다. "", " " 는 허용한다.
@NotEmpty                   // null, "" 을 허용하지 않습니다. " " 는 허용한다.
@NotBlank                   // null, "", " " 모두 허용하지 않는다.

@Email                      // 이메일 형식을 검사한다. 다만 " " 의 경우 통과시킨다.
@Pattern(regexp = )         // 정규식을 검사할때 사용된다.
                            --> regexp = "[a-z1-9]{6-10}", message="비밀번호는 영어와 숫자로 포함해서 6-10자리 이내로"
@Size(min = , max = )       // 길이를 제한할 때 사용
@Max(value = )              // value 이하의 값을 받을 때 사용
@Min(value = )              // value 이상의 값을 받을 때 사용

@Positive                   // 값을 양수로 제한
@PositiveOrZero             // 값을 양수와 0만 가능하도록 제한
@Negative                   // 값을 음수로 제한
@NegativeOrZero             // 값을 음수와 0만 가능하도록 제한
@Future                     // 현재보다 미래 -> 미래 날짜
@Past                       // 현재보다 과거 -> 과거 날짜

@AssertFalse                // false여부, null은 체크하지 않는다.
@AssertTrue                 // true여부, null은 체크하지 않는다.


예외처리에 관한 내용
MethodArgumentNotValidException.class
    @Valid 애너테이션으로 데이터를 검증하고, 해당 데이터에 에러가 있을 경우 예외 메세지를 JSON으로 처리하는 ExceptionHandler 처리 방법이다.

    if(e.getBindingResult().hasErrors()){
	// 만약 에러가 발생하면

	detail = e.getBindingResult().getFieldError().getDefaultMessage();
    }    
    무슨 문제(설정한 에러 메세지)가 발생 하였는지 결과를 detail에 넘겨준다.

String bindResultCode = e.getBindingResult().getFieldError().getCode();
    무슨 문제(bind관련 {not null, blank, 등등}가 발생하였는지 결과를 bindResultCode 에 담아준다.

만약 bindResultCode가 NotNull이 나온다면,
    
    switch(bindResultCode){
            case "NotNull":
                code = "ERROR_CODE_00001";
                description = "필수 값이 누락되었습니다.";
                break;
    } ErrorResponse DTO의 필드 값에 사용자 정의 설정하였다.




HATEOAS(Hypermedia As The Engine Of Application Status(헤이티오스))
    Rest Api를 사용하는 클라이언트가 전적으로 서버와 동적으로 상호작용할 수 있도록 지원해주며,
    용도는 모듈에서 step by step으로 진행하기 위해 사용하는 아이다.
    Rest Api를 잘 설계하기 위해서 나온 개념
    -> 잘 설계된 Rest Api를 구현하기 위한 단계가 존재하는데, 그 마지막 단계가

Hypermedia Controls  
    HATEAOS 라는 개념을 통해 자원에 호출 가능한 API정보를
    자원의 상태를 반영하여 표현하는 것을 의미한다.

요청이 들어온 곳에 대한 링크 또는 어디로 가야하는 지에 대한 링크를
모아서 동적으로 제공한다.
RepresentationModelAssembler 인터페이스 ?
    기존에는 userDTO에 대한 정보만을 넘겨줬다면, 
    toModel 메서드는 UserDTO정보 + 링크에 대한 것을 쉽게 넘겨줄 수 있게 만든 인터페이스 메서드다
    또한, 스프링에서 제공하는 것이라 컨테이너 내부에서 가져와서 처리해야 해서 bean으로 등록되어 있어야 한다.

    withSelfRel() 메서드
        현재 참조하고 있는 주소가 어떤건지 체크
linkTo(methodOn(HateoasTestController.class).findUserByUserNo(user.getNo())).withSelfRel(),


    withRel() 메서드
        현재 갈 곳에 대한 객체 정보
linkTo(methodOn(HateoasTestController.class).findAllUsers()).withRel("users")
    현재 users의 정보를 담은 링크 객체를 생성

링크에 대한 내용들만 요청을 보내서 결과값만을 받았는데,
결과를 받고나서 어디로 이동할 건지에 대한 지표를 확인하기 위한 작업이다.

두 가지 방식이 있는데,
    Controller 메서드 안에 직접 링크를 넣어주는 방식과,
    DtoModelAssembler 내에서 타입을 지정해서 전역으로 작업해주는 방식이 있다.
        com.greedy.api.section04.hateoas 참조하기


Swagger - 마무리 작업단계에서 문서화 시킬때 사용한다 !!
스웨거 라이브러리를 사용하여 postman 같은 어플을 
웹에서도 쉽게 확인할 수 있다. 그렇다고 테스트용으로 하는 postman같은 애가 아닌
문서화를 시키기 위한 용도다
ex) 아임포트 api
 
@Api 어노테이션 - 컨트롤러 내에서 사용한다.
    tags={""} 해당 컨트롤러에 내용을 설정한다.
    ex) @Api(tags = {"스프링 부트 스웨거 연동 테스트"})

@EnableWebMvc
    해당 컨트롤러에 대한 내용까지 다 추가하겠다는 어노테이션

.apis(RequestHandlerSelectors.any()) - 모든 경로를 API화
.apis(RequestHandlerSelectors.basePakage(패키지경로)) // 지정된 패키지만 api화 시킨다.
.paths(PathSelectors.any()) // 모든 URL 패턴에 대해서 수행

produce
 기존의 문자 타입이나 인코딩 방식
consumes
 요청을 보내서 응답을 받는 애들을 감싸서 보내줄 때 인코딩 방식이나 문자 타입

@ApiOperation(value = "별명") 
    위 처럼 별명 지정해주면 아래 사이트에서 별명으로 확인할 수 있다. 

@ApiResponses({ @ApiResponse(code=코드번호, message="메세지")})
    하게 되면 성공 코드 시 지정한 성공 메세지, 
    실패 시 지정한 실패 메세지가 호출된다.

아래 사이트에서 해당 정보를 확인할 수 있다.
http://localhost:8888/swagger-ui/index.html



