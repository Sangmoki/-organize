========================= 01-17 수업 내용 
jwt
    토큰 기반의 인증 방식

jwt tokken을 사용하는 이유
    기존에 세션 방식은
    서버측에 사용자의 정보를 쓸거라고 남겨놓고 브라우저 상에서 
    사용하고, 서버가 변경이 되면 사라졌는데,

    인증 부분에 대한 내용을 별도로 만들어 서버가 변경되더라도
    토큰이 남아있으면 기존의 정보를 그대로 사용할 수 있다.


java 8에서 제공하는 기본 Base64 Encoder와 Decoder
Base64.Encoder, Decoder

Encoding - 암호화
    String testStr = "base64로인코딩한비밀키";
    encode.encode() 하려면 배열 형식으로 바꿔주어야 해서

    byte[] testStrToByteArr = testStr.getBytes();
    byte[] encodeByte = encode.encode(testStrToByteArr);
    이렇게 바이트 형식으로 바꾸어서 넣어줘야 한다.

    이후 다시 문자열 형식으로 출력해주려면
    String encodeStr = new String(encodeByte); 문자열로 변경해주고
    System.out.println("인코딩 = " + encodeStr); 출력해보면

    인코딩 = YmFzZTY066Gc7J247L2U65Sp7ZWc67mE67CA7YKk 으로 출력된다.

Decoding - 복호화
    byte[] decodeByte = decode.decode(encodeStr);
    위에서 암호화 된 내용을 다시 복호화 하여 아래 출력문을 실행하면
    System.out.println("디코딩 = " + new String(decodeByte));
    
    디코딩 = base64로인코딩한비밀키 으로 출력된다.

JWT(JSON Web Token)
    인증에 필요한 정보들을 암호화시킨 JSON토큰을 의미한다.
    -> JWT 기반 인증은 JWT토큰(Access Token)을 Http헤더에 담아 서버가 클라이언트를 식별하게 하는 방식
    -> JWT는 JSON데이터를 Base64 URL-safe Encode를 통해 인코딩하여 직렬화한 것으로
    토큰 내부에서는 위/변조 방지를 위한 개인키를 통한 전자 서명 또한 담겨있다. 

JWT의 구조
        (XXXXXXXX.YYYYYYY.ZZZZZZZ)
          header.payload.signature 대부분 위와 같은 형식으로 되어있다.
    1. 헤더(Header)
    - typ : 토큰의 타입 지정(JWT)
    - alg : 해싱 알고리즘으로 Signature에서 사용된다.

    2. 내용 또는 정보(payload)
    - 토큰에 담을 정보가 들어가 있다.
    - 인코딩된 정보의 비트를 claim이라고 부른다.
      -> claim(name과 value의 쌍으로 구성)은 담는 정보의 한 조각을 의미한다.
        a. 등록된 클레임(registered claim)
            토큰에 대한 정보가 담김
            (iss : 토큰 발급자(issuer)
             sub : 토큰 제목(subject)
             aud : 토큰 대상자(audience)
             exp : 토큰 만료시간(expiration)
             nbf : 토큰 활성화(발급) 날짜
             iat : 토큰 활성화(발급) 시간
        b. 공개 클레임(public claim)
            사용자가 정의할 수 있는 클레임 정보 전달을 위해서 사용
        c. 비공개 클레임(private claim) 
            해당하는 당사자들 간에 정보를 공유하기 위해 만들어진 사용자 지정 클레임
            외부에 공개되도 상관은 없지만 해당 유저를 특정할 수 있는 정보들을 담는다.     
    3. 서명(Signature)
      - Header 인코딩 값과 Payload 인코딩 값을 합쳐서 비밀키로 해쉬하여 생성한다.
      - Header와 Payload는 단순하게 인코딩 된 값이기 때문에 조작을 할 수 있지만,
        Signature는 서버측에서 관리하는 비밀키가 유출되지 않는 이상 복호화 할 수 없다.       

JWT토큰을 사용하려면 JSON 문자열로 바꿔주는 행위가 분명 필요할거기 때문에
Jackson라이브러리를 추가해주어야 한다.
Databind, Core, Annotations 세가지 다 다운받아야 한다.

jwt.io에서 현재 내가 생성한 JWT 토큰을 확인할 수 있다.
        결과 : 생성된 JWT 토큰 =
         eyJhbGciOiJIUzUxMiJ9 <- Header
         .eyJzdWIiOiJ7XCJuYW1lXCI6XCLsnKDsirnsoJxcIn0ifQ <- Payload
         .BqusCKj110WC91Vl4fZOrwdIrMvY9egyDSa_TU2jrXW10wcbxT4fsdX34UfKrfnFkOQSzbMqk_yoM_D1f3Q0VQ <- Signature
