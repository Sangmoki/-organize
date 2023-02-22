02.03 수업 내용
chap03-javascript-es6-main
----------------------
# 04.iterable

## 01.iterable

### 01.iteable.js

#### 반복(순회)
- ES6 이전의 순회 가능한 데이터 컬렉션, 배열, 문자열, 유사 배열 객체, DOM컬렉션 등은
  통일된 규약 없이 for문, for..in문,
  forEach 메서드 등 다양한 방법으로 순회할 수 있었다.
- ES5에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로
  통일하여 for..of문, 스프레드 문법,
  배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화 했다.
- 이터러블(iterable)은 메서드 Symbol.iterator가 구현된 객체이다.


``` javascript
let range = {
    from : 1,
    to : 5
};
// 이터러블(iterable)은 메서드 Symbol.iterator가 구현된 객체이다.
range[Symbol.iterator] = function() {
// range = function() {
// [Symbol.iterator] 가 없으면 아래 출력할 때 오류가 발생한다.

    return {
        current: this.from,
        last: this.to,
        
        // for .. of 라는 반복문에 의해 반복마다 next()가 호출된다.
        next() {
            // next()는 값을 객체 {done : .., value : ...} 형태로 반환한다.
            // done은 반복이 끝났음을 의미한다, 끝나지 않았을 경우 value가 다음 값이 된다.
            if(this.current <= this.last) {

                return {done : false, value: this.current++};
            } else {

                return {done : true};
            }
        }
    }
}
for(let num of range) {
    console.log(num);
} // 결과 : 1, 2, 3, 4, 5
// 만약 위에서 [Symbol.iterator]를 주지 않으면 오류가 발생한다.
```
##### next() 메서드, { done, value } 속성
-  next()는 값을 객체 {done : .., value : ...} 형태로 반환한다.
-  done은 반복이 끝났음을 의미한다, 끝나지 않았을 경우 value가 다음 값이 된다.

## 02.array-and-string

### 01.array-and-string.js

#### 대표적인 이터러블
- 배열, 문자열 등은 대표적인 이터러블이다.
- 문자열이나 배열같은 내장 이터러블에도 Symbol.iterator가 구현되어 있어 명시적으로 호출할 수 있다.
- 자주 사용하지는 않지만 필요시 for..of 사용보다 반복 과정을 더 잘 통제할 수 있다.

``` javascript
for(let char of "JavaScript") {

    console.log(char);
} // 결과 : JavaScript라는 문자열을 한 문자씩 출력해준다.

// 문자열이나 배열같은 내장 이터러블에도 Symbol.iterator가 구현되어 있어 명시적으로 호출할 수 있다.
// 자주 사용하지는 않지만 필요시 for..of 사용보다 반복 과정을 더 잘 통제할 수 있다.
let str = "iterable";
let iterator = str[Symbol.iterator]();

while(true) {
    let result = iterator.next();

    if(result.done) {
        break;
    }
    console.log(result.value); // 결과 : iterable 문자열을 한 문장씩 출력한다.
    // str변수의 심볼을 iterator 변수에 담아주어 반복문을 통해 출력하였기 때문에 이러한 값이 나온 것이다.
```

## 03.iterable-and-array-like

### 01.iterable-and-array-like.js

#### 이터러블과 유사 배열
- 이터러블(iterable) : 메서드 Symbol.iterable가 구현된 객체
- 유사배열(array-like) : 인덱스와 length 프로퍼티가 있어서 배열처럼 보이는 객체

- 이터러블이면서 유사배열일 수 도 있고, 이터러블 객체라고 해서 유사 배열 객체는 아니며,
  유사 배열 객체라고 해서 이터러블 객체인 것도 아니다.
- 이터러블과 유사 배열은 배열의 메서드를 사용할 수 없어 불편할 때가 있는데,
  그때 Array.from을 이용해서 배열로 변경할 수 있다.

``` javascript
let arrayLike = {
    0: "배열인듯",
    1: "배열아닌",
    2: "배열같은너",
    length: 3
};
console.log(arrayLike); // 결과 : { '0': '배열인듯', '1': '배열아닌', '2': '배열같은너', length: 3 } <- 배열처럼 보인다.

// console.log(arrayLike.pop()); // 오류 발생 <- 배열처럼 보이지만, 배열은 아니기 때문에 pop을 사용할 수 없다.
// 즉, 배열이 아니기 때문에 배열 메서드(pop(), ..)을 사용할 수 없다.

// Array.from()은 넘겨받은 인수가 이터러블이나 유사배열인 경우, 새로운 배열을 만들고, 객체의 모든 요소를 새롭게 만든 배열로 복사한다.
let arr = Array.from(arrayLike);
console.log(arr); // 결과 : [ '배열인듯', '배열아닌', '배열같은너' ] <- length 속성은 키가 아니기때문에 안나온다.
console.log(arr.pop()); // 결과 : 배열같은너 <- pop()은 뒤에서부터 하나씩 뽑아내는 것이다.
console.log(arr.pop()); // 결과 : 배열아닌 <- 배열같은너가 이미 꺼내졌기 때문에 배열아닌이 나온 것이다.
```
- Array.from()은 넘겨받은 인수가 이터러블이나 유사배열인 경우, 새로운 배열을 만들고, 객체의 모든 요소를 새롭게 만든 배열로 복사한다.

- Symbol.iterator를 추가하면 배열로써의 사용이 가능해진다.
- 즉, 반복적으로 사용할 수 있는 타입이 만들어진다는 뜻이다.
- for..of 최초 호출 시, Symbol.itertor가 호출된다.
``` javascript
let range = {
    from : 1,
    to : 5
};
range[Symbol.iterator] = function() {

    return {
        current: this.from,
        last: this.to,
        
        next() {

            if(this.current <= this.last) {

                return {done : false, value: this.current++};
            } else {

                return {done : true};
            }
        }
    }
}

let arr2 = Array.from(range);
console.log(arr2.pop()); // 결과 : 5


// 이터러블은 데이터의 소비자(for..of, 스프레드 문법, 배열 디스트럭처링 할당 등)와
// 공급자(Array, String, Dom컬렉션)를 연결하는 인터페이스의 역할을 한다.

// 스프레드 문법
// [...] <- 배열 형식으로 바꿔준다.
```

- 이터러블은 데이터의 소비자(for..of, 스프레드 문법, 배열 디스트럭처링 할당 등)와
  공급자(Array, String, Dom컬렉션)를 연결하는 인터페이스의 역할을 한다.