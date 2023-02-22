02.02 수업 내용
chap03-JAVASCRIPT-ES6-MAIN - 01.class
==================================
## 01. class-basic-syntax
### 01. class-declarations.js
클래스 기본 문법
- 동일한 종류의 객체를 여러 개 생성해야 하는 경우 객체 리터럴을 여러 개 생성하기보다
- 클래스 문법을 통해 동일한 종류의 객체를 재생성 할 수 있다.

#### 01. 클래스 선언

##### 생성자를 통해 인스턴스 생성 및 초기화
- 생성자는 1개 이상 정의될 수 없으며 생략할 경우 암묵적으로 정의 된다.
- 암묵적으로 this를 반환하므로 반환문은 작성하지 않는다.

``` javascript
class Student { 
// 이 클래스를 가지고 나중에 객체를 생성을 하면 그룹이 고정으로 들어가는데 그 그룹값을 1로 고정하고,
// 인수로 인스턴스를 초기화 해서 사용하겠다.

    // 생성자를 통해 인스턴스 생성 및 초기화
    // 생성자는 1개 이상 정의 될 수 없으며 생략할 경우 암묵적으로 정의 된다.
    // 암묵적으로 this를 반환하므로 반환문은 작성하지 않는다.
    constructor(name){
        this.group = 1; // 고정값으로 초기화 하겠다.
        // 나중에 객체를 생성을 하면 그룹이 고정으로 들어가는데 그 그룹값을 1로 고정하겠다.
        this.name = name; // 인수로 인스턴스를 초기화 한다.
    }

    introduce(){
        console.log(`안녕하세요 저는 ${this.group}반 학생 ${this.name} 입니다.`);
    }
}

/* Student클래스를 선언하고 new Student()를 호출하면 새로운 객체가 생성되며,
    넘겨받은 인수 name과 함께 constructor가 자동으로 실행되며 "홍길동"이 this.name에 할당된다. */
let student = new Student("홍길동"); // 인수로 초기값 전달하면서 객체 생성
console.log(student.name); // 결과 : 홍길동
console.log(student.group); // 결과 : 1
student.introduce(); // 메서드 호출
// 결과 : 안녕하세요 저는 1반 학생 홍길동 입니다.

console.log(typeof Student); // 결과 : function - 클래스는 함수의 한 종류이다.
console.log(Student === Student.prototype.constructor); // 결과 : true - 정확하게 생성자 메서드와 동일하다.
console.log(Student.prototype.introduce); // 결과 : [Function: introduce] - 클래스 내부에 정의한 메서드는 클래스.prototype에 저장된다.
console.log(Object.getOwnPropertyNames(Student.prototype)); // 결과 : [ 'constructor', 'introduce' ]
// 현재 프로토 타입에는 constructor, introduce 두 가지의 메서드가 존재한다.

/*
    new Student()를 호출하면 Student라는 이름을 가진 함수를 만들고, 함수 본문은 생성자 메소드 constructor에서 가져온다.
    만약 생성자 메서드가 없으면 본문이 비워진 채로 함수가 만들어진다.
    introduce와 같이 클래스 내에서 정의한 메서드를 Student.prototype에 저장한다.
*/ 

function Teacher(name){

    this.group = 1;
    this.name = name;
}

Teacher.prototype.introduce = function(){

    console.log(`안녕하세요 저는 ${this.group}반 교사 ${this.name}입니다.`);
}

let teacher = new Teacher("유관순");
teacher.introduce(); // 결과 : 안녕하세요 저는 1반 교사 유관순입니다.

Teacher(); // 에러가 발생하지 않는다.
// Student(); // 에러 발생
```

##### 생성자 함수와 클래스의 차이점
1. 클래스 생성자를 new와 함께 호출하지 않으면 에러가 발생한다.
   함수 내부 프로퍼티 [[IsClassConstructor]] : true가 사용된다.

2. 클래스에 정의된 메서드는 열거가 불가능하다. enumerable 플래그가 false이기 때문이다.
   for..in 구문으로는 객체 순회 시 메서드 순회 대상에서 제외된다. 
``` javascript
for (let method in Student) {

    console.log("반복문 : " + method);
    // 출력되지 않는다.
}

```
3. 클래스는 항상 use strict가 적용된다.
    -> 클래스 생성자 안 코드는 자동으로 엄격모드가 적용된다.


### 02.class-declarations.js

#### 클래스 표현식
- 클래스 표현식 종류에는 가지가 있다.
1. 익명 클래스 표현식(이름이 없는)
``` javascript
let Tutor = class {
    teach(){
        console.log('이해하셨나요~~??');
    }
};

new Tutor().teach();
```

2. 기명 클래스(이름이 있는)
- 특징으로 자신의 클래스 이름을 클래스 내부 안에서 사용할 수 있다.
- 단, 클래스 내부가 아닌 외부에서는 사용할 수 없다.
``` javascript
let Tutee = class MyTutee {
    learn() {
        console.log('우와~ 이해했어요!!');
        console.log(MyTutee); // MyTutee라는 이름은 오직 클래스 안에서만 사용할 수 있다.
    }
}

new Tutee().learn();
// console.log(MyTutee); // MyTutee는 클래스 외부에서 사용할 수 없다.
```

3. 클래스 동적 생성
- 클래스도 함수처럼 일급 객체이며, 다른 표현식 내부에서 정의, 전달, 반환, 할당이 다 가능하다.
``` javascript
function makeTutee(message){

    return class {
        feedback(){
            console.log(message);
        }
    }
}

let SecondTutee = makeTutee("10점 만점에 10점");
new SecondTutee().feedback(); // 결과 : 10점 만점에 10점

/* 클래스도 함수처럼 일급 객체이며, 다른 표현식 내부에서 정의, 전달, 반환, 할당이 다 가능하다. */
```

### 03.getter-setter.js

#### 접근자 프로퍼티
- 접근자 프로퍼티는 프로토타입의 프로퍼티가 된다. 
- 매개변수명(필드)으로 외부 접근하고, _매개변수명으로 실제 값을 담는다.
- 밑줄은 프로그래머들 사이에서 외부 접근이 불가능한 프로퍼티나 메서드를 나타낼 때 사용한다.

``` javascript
class Product {

    constructor(name, price) {
    // setter를 사용하겠다고 활성화 한다.
        this.name = name;
        this.price = price;
    }

    // getter 함수
    get name() {
        console.log('get name 동작');
        return this._name; // 실질적인 값은 this._name이라는 필드명에 담긴다.
    }

    get price() {
        console.log('get price 동작');
        return this._price; // 실질적인 값은 this._price이라는 필드명에 담긴다.
    }

    // setter 함수
    set name(value) {
        console.log('set name 동작');
        this._name = value; // 값을 넣을 때는 실제 값인 _name에 value를 설정해야 한다.
    }

   set price(value) {
        console.log('set price 동작');
        if (value <= 0) {
            console.log('가격은 음수일 수 없습니다.');
            this._price = 0;
            return;
        } // 가격이 음수이면 해당 콘솔을 호출하고 price를 0으로 만들고 그게 아니라면 기입한 값을 대입시킨다.
        this._price = value; // 값을 넣을 때는 실제 값인 _price에 value를 설정해야 한다.
    }
}
// 조건문에 따른 차이 
let phone = new Product('전화기', 23000);
console.log(phone.name);
console.log(phone.price);
// 결과 : 
// set name 동작
// set price 동작
// get name 동작
// 전화기
// get price 동작
// 23000

let computer = new Product("컴퓨터", -23000);
console.log(computer.name);
console.log(computer.price);
// 결과 :
// set name 동작
// set price 동작
// 가격은 음수일 수 없습니다.
// get name 동작
// 컴퓨터
// get price 동작
// 0
```

### 04.public-field-declarations.js

#### 필드 선언
- 클래스를 정의할 때 '프로퍼티 이름 = 값' 을 써주면 클래스 필드를 생성한 것이다.
- 자바의 필드 선언과 같은 이치다.
- this.은 constructor 내부 또는 메서드 내부에서만 작성이 가능하다.(필드에서 작성이 불가하다.)
- 설정한 프로퍼티 값은 생성된 클래스.prototype 에 들어가는게 아닌 개별 객체에만 클래스 필드가 설정된다.
``` javascript
class Book {

    name = "JavaScript"
     // this.은 constructor 내부 또는 메서드 내부에서만 작성이 가능하다.
    // this.price = 30000;
    price;

    introduce() {
        console.log(`${this.name}(이)가 그렇게 재밌죠~`);
    }
}

let book = new Book();
book.introduce(); // 결과 : JavaScript(이)가 그렇게 재밌죠~

console.log(book.name); // 결과 : JavaScript
console.log(Book.prototype.name); // 결과 : undefined <- name 값은 생성된 Book.prototype 에 들어가는게 아닌 개별 객체에만 클래스 필드가 설정된다.
console.log(book.price); // 결과 : undefined
```


## 02.class-inheritance

### 01.inheritance-basic-syntax.js

#### 클래스 상속
- 클래스 상속을 사용하면 클래스를 다른 클래스로 확장할 수 있다.
- 자바 스크립트에서의 상속은 기존 java와 동일하게 extends 키워드를 사용한다.
- 키워드 extends는 프로토타입을 기반으로 동작한다.
- extends는 자식.prototype.[[Prototype]]을 부모.prototype으로 설정한다.
    그렇기 때문에 자식.prototype에서 메서드를 찾지 못하면 부모.prototype에서 메서드를 가져온다.

``` javascript
// 일반적인 클래스 사용 (부모가 될 클래스)
class Animal {
    
    constructor(name, weight) {
        this.name = name;
        this.weight = weight;
    }

    eat(foodWeight) {
        this.weight += foodWeight;
        console.log(`${this.name}(은)는 ${foodWeight}kg의 식사를 하고 ${this.weight}kg이 되었습니다.`);
    }

    move(lostWeight) {
        if(this.weight > lostWeight) {
            this.weight -= lostWeight;
        }
        console.log(`${this.name}(은)는 움직임으로 인해 ${lostWeight}kg이 감량되어 ${this.weight}kg이 되었습니다.`);
    }
}

let animal = new Animal("동물", 30);

animal.eat(1); // 결과 : 동물(은)는 1kg의 식사를 하고 31kg이 되었습니다.
animal.move(0.5); // 결과 : 동물(은)는 움직임으로 인해 0.5kg이 감량되어 0.5kg이 되었습니다.

// Animal을 상속받는 Human 클래스 선언 - extends 키워드 사용
class Human extends Animal {

    develop(language) {
        console.log(`${this.name}(은)는 ${language}로 대화를 합니다. 정말 즐겁습니다.`);
    }
}
let human = new Human('수강생', 70);
human.eat(2); // 결과 : 수강생(은)는 2kg의 식사를 하고 72kg이 되었습니다.
human.move(1); // 결과 : 수강생(은)는 움직임으로 인해 1kg이 감량되어 71kg이 되었습니다.

// 해당 메서드를 찾으려고 하면 Human에서 먼저 eat, move에 대한 내용을 찾아보고 없으면 Animal로 가서 찾아본다.
// 즉, 여기서는 Animal의 eat, move를 사용하는 것이다.
human.develop("JavaScript"); // 결과 : 수강생(은)는 JavaScript로 대화를 합니다. 정말 즐겁습니다.

console.log(human.__proto__);
console.log(human.__proto__.__proto__);

/* 키워드 extends는 프로토타입을 기반으로 동작한다.
    extends는 Human.prototype.[[Prototype]]을 Animal.prototype으로 설정한다.
    그렇기 때문에 Human.prototype에서 메서드를 찾이 못하면 Animal.prototype에서 메서드를 가져온다.
*/
```
### 02. method-overriding.js

#### 메서드 오버라이딩
- 부모 메서드 전체를 교체하지 않고, 부모 메서드를 토대로 일부 기능만 변경하고 싶을 때,
- 부모 메서드의 기능을 확장하고 싶을 때 메서드 오버라이딩을 사용한다.
- super.을 통해 부모 클래스의 메서드를 참조한다.

``` javascript
class Animal {
    
    constructor(name, weight) {
        this.name = name;
        this.weight = weight;
    }

    eat(foodWeight) {
        this.weight += foodWeight;
        console.log(`${this.name}(은)는 ${foodWeight}kg의 식사를 하고 ${this.weight}kg이 되었습니다.`);
    }

    move(lostWeight) {
        if(this.weight > lostWeight) {
            this.weight -= lostWeight;
        }
        console.log(`${this.name}(은)는 움직임으로 인해 ${lostWeight}kg이 감량되어 ${this.weight}kg이 되었습니다.`);
    }
}

class Tiger extends Animal {

    attack(target){
        console.log(`${this.name}(은)는 ${target}을 공격합니다.`);
    }

     // Animal의 move를 Tiger의 move로 재정의 하였다. 
    move(target) {
        // super.을 통해 부모 클래스의 메서드를 참조한다.
        super.move(0.1); // 결과 : 백두산 호랭이(은)는 움직임으로 인해 0.1kg이 감량되어 89.9kg이 되었습니다.
        this.attack(target); // 결과 : 백두산 호랭이(은)는 슬픈 눈망울의 사슴을 공격합니다.
    }
}
let tiger = new Tiger("백두산 호랭이", 90);
tiger.move("슬픈 눈망울의 사슴");
```

### 03.constructor-oveeriding.js

#### 생성자 오버라이딩
- 클래스가 다른 클래스를 상속받고, constructor가 없는 경우에는 비어있는 constructor가 만들어진다.
``` javascript
class Tirger extends Animal {
        constructor ()...args) {
            super(..args);
        }
    }
```
- 생성자는 기본적으로 부모 constructor를 호출한다.
- 이 때 부모 constructor에도 인수를 모두 전달하는데, 
  클래스에 자체 생성자가 없는 경우엔 이런 일이 모두 자동으로 일어난다.
- 상속 클래스의 생성자에선 반드시 super(...)를 호출해야 하는데,
   super(...)를 호출하지 않으면 에러가 발생한다.
- super(...)는 this를 사용하기 전에 반드시 호출해야 한다.
``` javascript
/* 부모의 constructor가 있는 경우 */
class Animal {
    
    constructor(name, weight) {
        this.name = name;
        this.weight = weight;
    }

    eat(foodWeight) {
        this.weight += foodWeight;
        console.log(`${this.name}(은)는 ${foodWeight}kg의 식사를 하고 ${this.weight}kg이 되었습니다.`);
    }

    move(lostWeight) {
        if(this.weight > lostWeight) {
            this.weight -= lostWeight;
        }
        console.log(`${this.name}(은)는 움직임으로 인해 ${lostWeight}kg이 감량되어 ${this.weight}kg이 되었습니다.`);
    }
}

class Deer extends Animal {

    constructor(name, weight, legLength) {

        // this.name = name;
        // this.weight = weight;
        // 오류가 발생하는데 기본적으로 상속을 받으면 부모 생성자를 바라보기 때문에
        // 상속 클래스의 생성자에선 반드시 super(...)를 호출해야 하는데, super(...)를 호출하지 않아 에러가 발생한다.
        // super(...)는 this를 사용하기 전에 반드시 호출해야 한다.
        super(name, weight); // 이렇게 작성해야 한다.
        this. legLength = legLength;
    }

    hide(place) {
        console.log(`${this.name}(은)는 ${place}에 숨어요.`);
    }
}

let deer = new Deer('슬픈 눈망울의 사슴', 40, 1);
deer.hide("동굴 안");
```
##### 생성자 오버라이딩 정리
- 자바스크립트는 '상속 클래스의 생성자 함수(derived constructor)'와 그렇지 않은 생성자 함수를 구분한다.
- 상속 클래스의 생성자 함수에 특수 내부 프로퍼티인 [[Constructor]]:"derived"가 붙게 된다.
- 일반 클래스는 new와 함께 실행되면 빈 객체가 만들어지고, this에 이 객체를 할당하지만,
    상속 클래스는 생성자 함수가 실행되면 빈 객체를 만들고,
     this에 이 객체를 할당하는 일을 부모 클래스의 생성자가 처리해주기를 기대한다.
- 이러한 차이 때문에 상속 클래스의 생성자에선 super를 호출해 부모 생성자를 실행해주어야 하고, 
  그렇지 않으면 this가 될 객체가 만들어지지 않아 에러가 발생하게 된다.


## 03.static-method-and-property

### 01.static-method.js

#### 정적 메소드
- 정적 메소드는 특정 클래스 인스턴스가 아닌 클래스 '전체'에 필요한 기능을 만들 때 사용한다.
- 정적 메소드는 클래스 선언부 안에 위치하고 앞에 static이라는 키워드를 붙인다.
- 객체를 따로 만들어주지 않아도 클래스명.메서드명으로 메서드를 호출해줄 수 있다.

##### 클래스 자체에 직접 할당한 static method
``` javascript
class Student {

    constructor(name, height) {

        this.name = name;
        this.height = height;
    }

    // 정적 메소드는 클래스 선언부 안에 위치하고 앞에 static이라는 키워드를 붙인다.
    static compare(studentA, studentB) {

        return studentA.height - studentB.height;
    }
}

let students = [ // 객체 배열
    new Student("유관순", 165.5),
    new Student("홍길동", 180.5),
    new Student("선덕여왕", 159.5)
];

console.log(students);
console.log('---------------------');
students.sort(Student.compare); // 키를 기준으로 오름차순 정렬
console.log(students);
// Student.copmpare는 학생들의 신장을 비교해주는 수단으로 하나의 학생마다 필요한 메서드가 아니라 클래스의 메서드여야 한다.

Student.staticMethod = function() {
    console.log('staticMethod는 메서드를 프로퍼티 형태로 직접 할당하는 것과 동일하다');
}

Student.staticMethod(); // 이것도 정적 메소드다.

```
##### staticMethod 프로퍼티 형태
- staticMethod는 메서드를 프로퍼티 형태로 직접 할당하는 것과 동일하다.

``` javascript
class User {

    constructor(id, registDate) {

        this.id = id;
        this.registDate = registDate;
    }

    // 만약 registDate에 대한 값이 매번 같은 값으로 들어간다고 하면 id만 넣어주면 된다.
    // 팩토리 메서드 같은 형식이다. 공장 !!
    static registUser(id) {

        return new this(id, new Date());
    }
}

// User.registUser('유저id') 메서드 호출을 하면 새로운 User 객체를 만들 수 있다.
let user01 = User.registUser('user01');
console.log(user01);
```

### 02.static-property.js

#### 정적 프로퍼티
- 클래스 레벨 수준에서 사용하고 싶을 때 사용한다.
- static을 사용한 선언은 기술적으로는 클래스 자체에 직접 할당하는 것과 동일하다.

##### 클래스 자체에 직접 할당한 static property
``` javascript
class Animal {
    
    static planet = "지구"; // 정적 프로퍼티

    constructor(name, weight) {
        this.name = name;
        this.weight = weight;
    }

    eat(foodWeight) {
        this.weight += foodWeight;
        console.log(`${this.name}(은)는 ${foodWeight}kg의 식사를 하고 ${this.weight}kg이 되었습니다.`);
    }

    move(lostWeight) {
        if(this.weight > lostWeight) {
            this.weight -= lostWeight;
        }
        console.log(`${this.name}(은)는 움직임으로 인해 ${lostWeight}kg이 감량되어 ${this.weight}kg이 되었습니다.`);
    }

    static compare(studentA, studentB) {

        return studentA.height - studentB.height;
    }
}
```
##### staticProperty 형태
``` javascript
Animal.staticProperty = "static을 사용한 선언은 기술적으로는 클래스 자체에 직접 할당하는 것과 동일하다."
```

#### 정적 프로퍼티와 정적 메서드의 상속
- static을 사용한 선언은 기술적으로는 클래스 자체에 직접 할당하는 것과 동일하다.
- 정적 프로퍼티와 정적 메서드는 상속이 가능하다.
- class B extends A
- 클래스 B의 프로토타입이 클래스A를 가리키게 하므로 B에서 원하는 프로퍼티나 메서드를 찾지 못하면 A로 검색이 이어진다. 
  
``` javascript
Animal.staticProperty = "static을 사용한 선언은 기술적으로는 클래스 자체에 직접 할당하는 것과 동일하다."

// Animal을 상속받는 Human 클래스
/*
    정적 프로퍼티와 정적 메서드는 상속이 가능하다.
    class B extends A

    클래스 B의 프로토타입이 클래스A를 가리키게 하므로 B에서 원하는 프로퍼티나 메서드를 찾지 못하면 A로 검색이 이어진다. 
*/

class Human extends Animal {

    develop(language) {

        console.log(`${this.name}(은)는 ${language}로 개발을 합니다. 정말 즐겁습니다.`);
    }
}

let humans = [ // 객체 배열
    new Human("유관순", 70),
    new Human("홍길동", 50),
    new Human("선덕여왕", 60)
];

humans.sort(Human.compare); // 몸무게를 기준으로 오름차순 정렬
console.log(humans);

humans[0].develop('JavaScript'); // 결과 : 홍길동(은)는 JavaScript로 개발을 합니다. 정말 즐겁습니다.

// 정적 프로퍼티 상속
console.log(Human.planet); // 결과 : 지구

// 직접 할당한 경우도 동일하게 동작한다.
console.log(Human.staticProperty); // 결과 : static을 사용한 선언은 기술적으로는 클래스 자체에 직접 할당하는 것과 동일하다.

// Human에도 Aniaml의 정적 메소드가 존재한다.
console.log(Human.__proto__ === Animal); // 결과 : true <- 정적인 메서드가 존재한다.
console.log(Human.prototype.__proto__ === Animal.prototype); // 결과 : true <- 인스턴스의 메서드가 존재한다.
```