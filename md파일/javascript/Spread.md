02-03 수업 내용
chap03-javascript-es6-main
===============================

# 05.spread-syntax

## 01.rest-parameter

### 01.rest-parameter.js

#### 나머지 매개변수(Rest Parameter)
- 함수에 정의된 인수는 2개이고
    정해진 인수보다 적게 전달되면 undefined, 
    정해진 인수보다 많이 전달되면 해당 인수를 무시하고 기능한다.

##### 일반적인 배열 사용
``` javascript
function merge(msg1, msg2) {

    return msg1 + msg2;
}

/* 함수에 정의된 인수는 2개이고
    정해진 인수보다 적게 전달되면 undefined, 
    정해진 인수보다 많이 전달되면 해당 인수를 무시하고 기능한다.
*/
console.log(merge('안녕하세요')); // 결과 : 안녕하세요undefined
console.log(merge('안녕하세요', '반갑습니다.')); // 결과 : 안녕하세요 반갑습니다
console.log(merge('안녕하세요', '반갑습니다.', ' 제 이름은 홍길동입니다.')); 
// 결과 : 안녕하세요 반갑습니다 <- '제 이름은 홍길동입니다' 가 짤렸다.

```
##### 가변배열 사용
``` javascript
function mergeAll(...args) { // 들어오는 족족 배열로 만들어 넣어줄 수 있는 가변 배열

    let message = '';

    for(let arg of args) {
        message += arg;
    }
    
    return message;
}

console.log(mergeAll('안녕하세요')); // 결과 : 안녕하세요
console.log(mergeAll('안녕하세요', '반갑습니다.')); // 결과 : 안녕하세요 반갑습니다
console.log(mergeAll('안녕하세요', '반갑습니다.', ' 제 이름은 홍길동입니다.')); 
// 결과 : 안녕하세요반갑습니다. 제 이름은 홍길동입니다.
// 위의 가변 배열을 사용하지 않았을 때는 값들을 정상적으로 표출할 수 없었다.
```

// 나머지 매개변수는 항상 마지막에 있어야 한다.
// function func(arg1, ...args, arg2) => (x)
// function func(arg1, arg2, ...args) => (o)

## 02.spread-syntax

### 01.spread-syntax

#### 스프레드 문법과 나머지 매개변수의 특징
- 스프레드 문법 : 배열을 목록으로 확장해줌으로 [...]이 함수 호출 시,
  배열이나 객체 리터럴 내부에 사용된다.

- 나머지 매개변수 : 인수 목록의 나머지를 배열로 모아주므로 [...]이 함수 매개변수의 끝에 있다.

- 즉, 위치에 따라 다르다는 말이다.
- 두 가지 문법을 함께 사용하면 매개변수 목록과 배열 간 전환을 쉽게 할 수 있다.

#### 스프레드 문법, 전개 문법
- rest parameter처럼 매개변수 목록을 배열로 가져오는 것과는 반대로,
  배열을 통째로 매개변수에 넘겨주는 기능이다.
- 하나로 뭉쳐 있는 여러 값들의 집합을 전개해서 개별적인 값들의 목록으로 만든다.
- 사용대상 for.of 문으로 순회할 수 있는 이터러블에 한정된다.



``` javascript
console.log(`가장 큰 값: ${ Math.max(10, 30, 20)}`); // 결과 : 가장 큰 값: 30 <- 가장 큰 값 꺼내주세요

let arr = [10, 30, 20];
console.log(`가장 큰 값: ${ Math.max(arr)}`); // 결과 : NaN <- 

```

##### Math.max() 함수
- Math.max()는 배열이 아닌 숫자 목록을 인수로 받기 때문에
  배열의 경우 원하는대로 동작하지 않는다.
- 이 때, 스프레드 문법을 사용한다.
``` javascript
console.log(`가장 큰 값: ${ Math.max(...arr)}`); // 결과 : 30 <- [...](스프레드 문법)을 사용하여 가변배열로 만들어준다.
```

##### 스프레드 문법의 장점 및 사용 예제
``` javascript
let arr1 = [10, 30, 20];
let arr2 = [100, 300, 200];

// 배열 객체 여러 개 전달이 가능하다.
console.log(`가장 작은 값: ${ Math.min(...arr1, ...arr2)}`); // 결과 : 가장 작은 값: 10

// 일반 값도 혼합해서 사용이 가능하다.
console.log(`가장 작은 값: ${ Math.min(1, ...arr1, 2, ...arr2)}`); // 결과 : 1

// 배열 병합도 가능하다.
let merged = [0, ...arr, 2, ...arr2];
console.log(merged);

// 이러터블 배열 변환이 가능하다.
// 스프레드 문법은 for..of와 같은 방식으로 내부에서 이터레이터를 사용해 요소를 수집한다.
let str = "JavaScript";
console.log([...str]); // 결과 : ['J', 'a', 'v', 'a', 'S', 'c', 'r', 'i', 'p', 't'] <- 문자 배열로 변환

// Array.from()도 동일하게 가능하지만
// Array.from()은 이터러블 객체 뿐만 아니라, 유사 객체 배열에서도 사용할 수 있어
// 무언가 배열로 바꿀 때 보편적으로 사용한다.
console.log(Array.from(str)); // 위의 출력문과 동일한 결과 출력
```

##### 스프레드 문법을 이용한 배열, 객체 복사
``` javascript
let arr3 = [10, 30, 20];
let arr3Copy = [...arr3];

console.log(arr3); // 결과 : [ 10, 30, 20 ]
console.log(arr3Copy); // 결과 : [ 10, 30, 20 ]
console.log(arr3 === arr3Copy); // 결과 : false <- 새로운 배열을 만들고 객체의 모든 요소를 새롭게 만든 배열로 복사한 것이기 때문이다.
arr3Copy.push(40);
console.log(arr3Copy); // 결과 : [ 10, 30, 20, 40] <- 주소가 다르기 때문에 arr3에는 안들어가고 arr3Copy에만 들어간다.
```

- 스프레드 문법의 대상은 이터러블이어야 안의 내용들을 사용할 수 있는데, 
  스프레드 프로퍼티 제안은 일반 객체를 대상으로도 허용하고 있다.
``` javascript
let obj = { name : '홍길동', age : 20 };
let objCopy = { ...obj };
console.log(obj); // 결과 : { name: '홍길동', age: 20 }
console.log(objCopy); // 결과 : { name: '홍길동', age: 20 }
console.log(obj === objCopy); // 결과 : false
objCopy.age = 40;
console.log(obj); // 결과 : 20
console.log(objCopy); // 결과 : 40

/* 
    let arrCopy = Object.assign([], arr); let objCopy = Object.assign({}, obj);
    또는 slice() 보다 간결한 문법으로 배열, 객체를 복사할 수 있다.
*/
```