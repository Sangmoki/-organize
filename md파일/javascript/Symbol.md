02-03 수업 내용
chap03-javascript-es6-main
================================
# 03.Symbol

## 01_Symbol-basic-syntax

### 01_Symbol-basic-syntax.js

#### Symbol
- 자바스크립트가 ECMAScript로 표준화된 이래로 자바스크립트에는 6개의 타입(문자열, 숫자, 불리언, undefined, null, 객체)이 있었다.
- Symbol은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.
- Symbol은 다른 값과 중복되지 않는 유일무이한 값으로 주로 이름 충돌의 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용된다.

##### Symbol의 특징
- 심볼은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 심볼을 여러 개 만들어도 각 심볼 값은 다르다.
- 심볼 이름은 어떤 것에도 영향을 주지 않는 이름표 역할만을 한다.
- 전역 심볼 레지스트리에 심볼을 만들고 해당 심볼에 접근하면 이름이 같은 경우 항상 동일한 심볼만을 반환한다.
- 
###### Symbol()을 사용하면 심볼 값을 만들 수 있다.
``` javascript
let symbol1 = Symbol();
console.log(symbol1); // 결과 : Symbol()
let symbol2 = Symbol('mySymbol');
console.log(symbol2); // 결과 : Symbol(mySymbol)

// 심볼은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 심볼을 여러 개 만들어도 각 심볼 값은 다르다.
// 심볼 이름은 어떤 것에도 영향을 주지 않는 이름표 역할만을 한다.
let symbol3 = Symbol("mySymbol");
console.log(symbol3); // 결과 : Symbol(mySymbol)
console.log(symbol2 == symbol3); // 결과 : false - symbol2와 symbol3은 이름만 같을 뿐 다른 값이다.
```

###### 전역 심볼 레지스트리에 심볼을 만들고 해당 심볼에 접근하면 이름이 같은 경우 항상 동일한 심볼만을 반환한다.
- Symbol.for(key)를 사용해 이름이 key인 심볼을 전역 심볼 레지스트리에서 읽는다.
- 심볼이 존재하지 않으면 새로운 심볼을 만든다.
- 즉, key가 같으면 심볼은 같고, 만약 심볼이 없으면 새로 만들어준다.
  
``` javascript
let symbol = Symbol.for("id");
let idAgain = Symbol.for("id");

console.log(symbol === idAgain); // 결과 : true <- 두 심볼은 같다.
```

- 반대로 Symbol.keyFor(symbol)를 사용하면 심볼의 이름을 얻을 수 있다.
``` javascript
let sym = Symbol.for("name"); // 결과 : name
let sym2 = Symbol.for("id"); // 결과 : id

console.log(Symbol.keyFor(sym)); // 결과 : name
console.log(Symbol.keyFor(sym2)); // 결과 : id
```

## 02.Symbol-feature

### 01.Symbol-feature.js

#### Symbol의 특징
- Symbol을 이용하면 외부 코드에서 접근이 불가능하고 값도 덮어쓸 수 없는 숨긴 프로퍼티를 만들 수 있다.
- 외부 스크립트나 라이브러리는 심볼 설정을 갖고 있지 않아서 프로퍼티에 직접 접근하는 것이 불가능하다.
- 따라서, 심볼형 키를 사용하면 프로퍼티가 우연히라도 사용되거나 덮어씌워지는 것을 예방할 수 있다.
- Symbol은 중복되지 않는 상수 값을 생성하는 것은 물론, 기존에 작성된 코드에 영향을 주지 않고,
  새로운 프로퍼티를 추가하기 위해 즉, 하위 호환성을 보장하기 위해 도입되었다고 할 수 있다.
``` javascript
let student = {
    name : "홍길동"
};

let id = Symbol("id");
student[id] = 1; // Symbol("id") 키의 값은 studnt[id] = 1 이다.

// student 객체의 키 값, 프로퍼티 이름 등에 name만 나타나고 id는 나타나지 않는다.
console.log(Object.keys(student)); // 결과 : [ 'name' ]
console.log(Object.getOwnPropertyNames(student)); // 결과 : [ 'name' ]

console.log(student[id]); // 결과 : 1

let student2 = {
    name : "유관순",
    age : 16,
    [id] : 2
};

for(let key in student2) {

    console.log(key); // 결과 : name, age <- id는 나오지 않는다. 
}

console.log(Object.getOwnPropertySymbols(student)); // 결과 : [ Symbol(id) ]
console.log(Object.getOwnPropertySymbols(student2)); // 결과 : [ Symbol(id) ]
```


