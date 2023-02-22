chap03-06.distructuring-assignment
=============================
# 01.array-distructuring-assignment (비 구조화 할당)

## 01. 배열 구조 분해 할당
- 구조 분해 할당을 사용하면 배열이나 객체를 변수로 '분해'하여 연결할 수 있다.
  
``` javascript
// 이름과 성을 요소로 가진 배열
let nameArr = ["Gildong", "Hong"];

// 구조 분해 할당을 이용해 firstName엔 nameArr[0]을 lastName엔 nameArr[1]을 할당
// let firstName = nameArr[0];
// let lastName = nameArr[1];
let [firstName, lastName] = nameArr;
console.log("firstName : " + firstName);
console.log("lastName : " + lastName);
// 결과 : firstName : Gildong, lastName : Hong

 // 잘라서 배열로 만들기
let [firstName2, lastName2] = "Saimdang Shin".split(" ");
console.log("firstName2 : " + firstName2);
console.log("lastName2 : " + lastName2);
// 결과 : firstName2 : Saimdang, lastName2 : Shin

// 요소 버리기
// 쉼표를 사용하면 필요하지 않는 배열 요소를 버릴 수 있다.
// let [firstName3, lastName3] = ['firstName', 'middleName', 'lastName'];
let [firstName3, ,lastName3] = ['firstName', 'middleName', 'lastName'];
console.log("firstName3 : " + firstName3);
console.log("lastName3 : " + lastName3);
// 결과 : firstName3 : firstName, lastName3 : middleName
// 결과 : firstName3 : firstName, lastName3 : lastName
```

## 02. 다양한 사용법
- 할당 연산자 우측엔 모든 이터러블이 올 수 있고,
  할당 연산자 좌측엔 뭐든지 올 수 있다.

- 그래서 객체 프로퍼티도 가능하다.
``` javascript
let user = {};
// let arr = "Gwansoon Yu".split(" ");
// user.firstName = arr[0];
// user.lastName = arr[1];

// 위에 사용한 배열을 이렇게도 사용이 가능하다.
[user.firstName, user.lastName] = "Gwansoon Yu".split(" ");
console.log("user.firstName : " + user.firstName);
console.log("user.lastName : " + user.lastName);

// Object.entries라는 메서드를 통해 key와 value에 대한 구조 분할을 해주면
// 반복문을 통해 따로따로 꺼내 사용할 수 있다.
// 즉, Object.entries()와 구조분해를 조합하면 객체의 키와 값을 순회한 후 변수로 분해, 핟당할 수 있다.
for(let [key, value] of Object.entries(user)) {
    console.log(`${key} : ${value}`);
}
// 결과 : firstName : Gwansoon, lastName : Yu
```

### 구조할당 분해를 사용하면 변수 교환 용도로도 사용할 수 있다.
``` javascript
let student = "유관순";
let teacher = "홍길동";

[student, teacher] = [teacher, student];

console.log(`학생 : ${student}, 교사 : ${teacher}`);
// 결과 : 학생 : 홍길동, 교사 : 유관순
```

### rest parameter ...로 나머지 요소를 한번에 가져올 수 있다.
``` javascript
// ...변수는 나머지들을 받아 줄 값.
let [sign1, sign2, ...rest] = ["양자리", "황소자리", "쌍둥이자리", "게자리", "사자자리"];

console.log("sign1 : " + sign1);
console.log("sign2 : " + sign2);
console.log("rest : " + rest);
// 결과 : sign1 : 양자리, sign2 : 황소자리, rest : 쌍둥이자리,게자리,사자자리
```

### 기본값도 설정할 수 있다.
``` javascript
let [firstName4 = '아무개', lastName4 = '김'] = ['길동'];

console.log('firstName4 : ' + firstName4); // 배열에서 받아온 값
console.log('lastName4 : ' + lastName4); // 내가 지정한 기본 값
```

# 02. object-distructuring-assignment

## 1. 객체 구조 분해 할당
- 구조 분해 할당을 사용하면 배열이나 객체를 변수로 '분해'하여 연결할 수 있다.


### 객체에 구조 분해 할당 이용
``` javascript
let pants = {
    productName : "배기팬츠",
    color: "검정색",
    price : 30000,
};

// let productName = pants.productName; // 이렇게 사용해야 됐던 것들을
// 구조분해 할당을 이용해 productName에는 pants.productName을 
// color엔 pants.color, price에는 pants.price를 할당한다.
let {productName, color, price} = pants;

console.log('productName : ' + productName);
console.log('color : ' + color);
console.log('price : ' + price);
// 결과 : 
// productName : 배기팬츠
// color : 검정색
// price : 30000
```

### {객체 프로퍼티 : 목표 변수} 형식
- 각 변수의 서술 순서는 무관하고,
  {객체 프로퍼티 : 목표 변수} 형식으로도 작성할 수 있다.

``` javascript
let { color : co, price : pr, productName: pn } = pants;

console.log(pn); 
console.log(co);
console.log(pr);
```

## 2. 다양한 사용법

### 기본 값 지정해서 사용하기
``` javascript
let shirts = {
    productName: '베이직셔츠'
};

/* 객체에 존재하지 않는 프로퍼티는 기본 값을 설정해서 사용할 수 있으며,
콜론과 할당을 동시에 사용할 수 있다. */
let { productName: productName2 = "어떤 상품", color: color2 = "어떤 색상", price: price2 = 0 } = shirts;

// 결과  
// productName2 : 베이직셔츠
// color2 : 어떤 색상
// price2 : 0
```

### 위에서 선언한 patns 객체에서 productName만 변수로 뽑아내기
``` javascript
let { productName: productName3 } = pants;
console.log(`productName3 : ${productName3}`); // 결과 : productName3 : 배기팬츠
```

### rest parameter ...로 나머지 요소 한번에 가져오기
- rest paramter ...로 나머지 요소를 한번에 가져올 수 도 있다.
- 단, 구버전 브라우저에서는 동작하지 않는다. 
- 하지만, 바벨을 사용해서는 동작시킬 수 있다.
- 여기서 바벨이란 언어를 맞춰주기 위한 라이브러리이다.
- react에서는 바벨 라이브러리가 이미 추가되어 있다.
``` javascript
let { productName: productName4, ...rest} = pants;

console.log(`productName4 : ${productName4}`);
console.log(`rest : ${rest}`);
console.log(`rest.color : ${rest.color}`);
console.log(`rest.price : ${rest.price}`);

// 결과 : 
// productName4 : 배기팬츠
// rest : [object Object] <- rest는 객체형식
// rest.color : 검정색
// rest.price : 30000
```

### let 없이도 사용이 가능하다.
- 보통은 let 없이 사용하는 것을 지양한다.
``` javascript
let productName5, color5, price5; // 이렇게 사용이 가능하다.
// 하지만 이렇게 사용하면 타입에 대한 내용을 명시적으로 구분하기가 힘들다.

// {productName: productName5, color: color5, price: price5} = pants;
// 코드 블럭으로 인식하기 때문에 오류가 발생한다.

({productName: productName5, color: color5, price: price5} = pants); // 소괄호를 감싸주면 오류가 발생하지 않는다.

console.log(`productName5 : ${productName5}`);
console.log(`color5 : ${color5}`);
console.log(`price5 : ${price5}`);
// 결과 : 
// productName5 : 배기팬츠
// color5 : 검정색
// price5 : 30000
```

## 03. 중첩 구조 분해

### 구조를 만들어서 매핑시켜 사용할 수 있다.
``` javascript
let product = {

    size : {
        width: 10,
        height: 30
    },
    items: ["doll", "robot"]
};

let {
    size: {
        width,
        height
    },
    items: [item1, item2],
    producer = "홍길동" // 기본 값 (없는 값 만들어서)
} = product;

console.log(`width: ${width}`);
console.log(`height: ${height}`);
console.log(`item1: ${item1}`);
console.log(`item2: ${item2}`);
console.log(`producer: ${producer}`);

// 결과 : 
// width: 10
// height: 30
// item1: doll
// item2: robot
// producer: 홍길동
```

## 04. 함수의 활용
- 함수의 매개변수가 많거나 매개변수 기본값이 필요한 경우 등에 활용된다.
  
### 함수의 매개변수 고정으로 인한 문제
``` javascript
function displayProduct(producer = "아무개", width = 0, height = 0, items = []){}
displayProduct('신사임당', undefined, undefined, ["Coffee", "Donut"]);
// 위와 같은 함수는 넘겨주는 인수의 순서가 고정되어 있어 순서가 바뀌면 문제가 생기고,
// 기본값 사용시에도 undefined를 이용해서 인수를 넘겨줘야만 한다.
```

### 구조 분해 할당을 이용한 함수 문제 해결
- 인자의 순서는 무관하고,
  기본 값 활용 시에도 별도의 처리가 불필요하다.
``` javascript
function displayProduct({producer = "아무개", width = 10, height = 20, items = []}){
    console.log(`${producer} ${width} ${height}`);
    console.log(items);
}

let exampleProduct = {
    items: ["Coffee", "Donut"],
    producer: "신사임당",
};
// 인자의 순서도 무관하고 기본 값 활용 시에도 별도의 처리가 불필요하다.
displayProduct(exampleProduct);
// 결과 : 
// 신사임당 10 20
// [ 'Coffee', 'Donut' ]
```

