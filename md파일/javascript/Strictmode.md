02.02 수업 내용
chap01 - core - 11_strict-mode
=================================
## 01.srtice-mode - 엄격 모드
### 01.strict-mode.js
- 자바 스크립트는 자료형이 들어가지 않은 변수는 전역 변수가 되는 특징을 가진다.
그러한 특징으로 인해 의도치 않은 오류를 방지하기 위해 엄격 모드가 나왔다.
- 전역의 'user strict' 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.
- 만약 코드의 선두에 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.

``` javascript
'use strict' // 엄격 모드를 사용한다는 선언이다.
function test(){
    'use strict'; 
    // 함수 body의 선두에 추가하면 해당 함수와 중첩 함수에 대해서 strict mode가 적용된다.
    // 즉, 함수한테만 엄격 모드를 걸어줄 수 있다.
    // 하지만 이곳 저곳 사용하면 중구난방이 되기 때문에 추천하지는 않는다.
    x = 10;
    // 암묵적으로 전역변수가 된다.
// 자바스크립트 특징으로 자료형이 들어가지 않은 변수는 전역 변수가 된다.
}

test();

console.log(x);
// 원래 지역변수로 값이 나오면 안되는데 전역변수로 처리됐다.
// 이러한 의도치 않은 오류를 방지하기 위해 엄격 모드가 나왔다.
// 스크립트 코드 상단에 'use strict'를 선언해준다.

// 서드파티 라이브러리가 non-strict mode인 경우 즉시 실행함수로
// 스크립트 전체를 감싸서 스코프를 구분하고, 즉시 실행 함수의 선두에 strict mode를 적용한다.
(function(){
    'use strict';
});

```

## 02.example - 에러 상황
### 01.error-case.js
``` javascript
// 에러 상황
// 1. 암묵적 전역
(function() {
    // 'use strict';

    // 선언하지 않은 변수를 참조하면 에러가 발생하기 때문에 use strick를 상단에 써준다.
    x = 1;
    console.log(x);
}());

// 2. 변수, 함수, 매개변수의 삭제
(function() {

    // 'use strict';
    // delete 함수 호출을 하지 못하게 한다.

    var x = 1;
    // delete 연산자로 변수, 함수, 매개변수를 삭제하면 문법 에러가 발생한다.
    delete x; // strict모드에서는 식별자에 대해 delete를 호출할 수 없다.
}());

// 3. 매개변수 이름의 중복
(function () {

    // 'use strict';
    function test(x, x) {

        return x + x;
    }

    console.log(test(1, 2));
}());

// 4. with문의 사용
(function() {
    // 'use strict'; 
    with({x : 1}){ // 'with'문은 strict 모드에서 사용할 수 없다.
        console.log(x);
    }
}());
```

### 02.change-case.js - 변화 상황
``` javascript
// 1. 일반 함수의 this
// 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 떄문에
// strict mode에서는 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩 된다.
(function() {
    'use strict';
    function test() {

        console.log(this);
    }

    test();

    new test();
}());

// 2. arguments 객체

// function test(a, b, c) {

//     console.log(a);
//     console.log(b);
//     console.log(c);
//     console.log(arguments); // 해당 인자에 대한 내용들을 키 밸류 값으로 가지고 있는 것이다.
// }

// test(1, 2, 3, 4, 5);

// strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
(function(x) {
//    'use strict';
    x = 2;

    console.log(arguments);
}(1, 3, 5)); // 매개변수 자리는 1자린데 3, 5를 더 넣었는데 arguments에 키 밸류 값으로 계속 담긴다.

```
