# JSX(JavaScript XML)
- React 엘리먼트를 좀 더 간단한 방법으로 표기해주는 방식
- createElement를 이용해 엘리먼트를 정의하면 복잡하기도 하고 가독성도 좋지 않다.
- 그래서 리액트 팀은 ReactElement를 정의하는 간편한 방법으로 JSX문법을 제공한다.
- 자바스크립트를 확장한 문법으로 ReactElement를 xml문법 형식으로 정의할 수 있는 방법.

``` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <title>Document</title>
</head>
<body>

    <div id="root"></div>

    <!-- value로 script가 들어가있는데 여기서는 babel 문법을 사용해주어야 하기 때문에 type을 지정해준다. -->
    <script type="text/babel">
        // const helloworld = <h1>Hello World!</h1>; // createElement 문법으로 변환시켜준다.
        // html처럼 보이지만, xml문법을 적용시킨 자바스크립트이다.
        // 또한 하나의 요소로 묶여서 추가되어야한다.(요건 뒤에가서 다시 다룰 예정)
        const helloworld = (
            <div>
                <h1>Hello World!</h1>
                <br />
            </div> 
        ); // 줄바꿈같은거 쓰려면 </> 닫는 태그 써야한다.
        ReactDOM.createRoot(document.getElementById('root')).render(helloworld);
    </script>

</body>
</html>
```
- 단, JSX는 공식적인 자바스크립트 문법이 아니어서 바벨이라는 트랜스컴파일링 툴이 필요하다.
- 바벨은 ES6 + ES next 문법을 ES5문법으로 변환해주는 도구이다.
- 추가로 JSX를 순수 리액트로 변환해주는 기능이 포함되었다.(바벨이 JSX처리 표준으로 사용된다.)
- 바벨을 적용하는 간단한 방법(CDN) - unpkg에서 제공하는 cdn을 사용할 것
- 사용할 때 script 태그에 type속성에 text/babel 속성 값을 추가한다.
- html처럼 보이지만, xml문법을 적용시킨 자바스크립트이다.

## JSX문법을 이용한 표현

``` javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <!-- JSX문법 내에 중괄호를 이용하여 모든 javascript 표현식(문법)을 넣을 수 있다. -->
    <div id="root"></div>

    <script type="text/babel">

        function formatName(user) {
            return `${user.lastName} ${user.firstName}`;
        }

        const user = {
            firstName : "Gildong",
            lastName : "Hong",
        };

        const element = <h1>Hello, {formatName(user)}</h1>;
        
        ReactDOM.createRoot(document.getElementById('root')).render(element);
    </script>

</body>
</html>
```

## JSX의 규칙
- JSX는 편리한 문법이지만 몇 가지 규칙을 준수해야 한다.
- ReactElement는 하나의 DOM 트리를 구성하는 방식이기에 여러 개의 부모 노드가 존재하면 안된다.
- 따라서, 동일 레벨의 노드들이 연달아 정의되는 경우 문법적인 에러가 발생한다.
- 여러 형제 레벨의 엘리먼트를 정의해야 하는 경우 하나의 부모 엘리먼트로(div 태그와 같은)감싸야 하는데,
  div 요소같은 실질적인 요소를 사용하지 않으려면 Fragment를 이용할 수 있다.
  (리액트16버전부터 제공)

### 1. 최상위 엘리먼트가 두 개 이상이면 에러가 발생한다.(하나의 돔 트리만 생성해야 한다.) 
``` javascript
const element = (
    <h1>Hello, {user.name}</h1>
    <h3>phone : {user.phone}</h3>
);
```

### 2. <div> 태그로 감싸서 하나의 돔 트리를 생성할 수 있도록 한다.
``` javascript
const element = (
    <div>
        <h1>Hello, {user.name}</h1>
        <h3>phone : {user.phone}</h3>
    </div>
);
```

### 3. <React.Fragment>로 감싸서 형식상의 돔 트리상 상위 엘리먼트를 만들어준다.
``` javascript
const element = (
    <React.Fragment>
        <h1>Hello, {user.name}</h1>
        <h3>phone : {user.phone}</h3>
    </React.Fragment>
);
```

### 4. React 라이브러리로부터 Fragment 객체만 비구조화 할당을 해주면 React.을 생략할 수 있다.
``` javascript
const { Fragment } = React;

const element = (
    <Fragment>
        <h1>Hello, {user.name}</h1>
        <h3>phone : {user.phone}</h3>
    </Fragment>
);
```

### 5. React.Fragment의 축약 문법인 <></>로 감싸서 사용할 수 있다.(babel 17이상부터 지원함)
``` javascript
const element = (
    <>
    <h1>Hello, {user.name}</h1>
        <h3>phone : {user.phone}</h3>
    </>
);
```

## JSX 속성
- JSX를 이용해서 element를 정의할 때 attribute도 정의할 수 있다.
- 
### attribute를 정의할 시 주의할 사항
- JSX는 html태그처럼 보이지만, 실제로는 자바스크립트이다.
        (React.createElement를 호출하는 코드로 컴파일된다.)
- 따라서, 자바스크립트의 예약어와 동일한 이름의 attribtue는 다른 이름으로 우회적으로 사용하도록 정의되어있다.
ex) 예) class는 자바스크립트의 예약어이다. 엘리먼트의 class속성을 부여할 때 자바스크립트 문법에서는 사전에 정의된 키워드이기 때문에 className이라는 속성으로 사용해야 한다.

### JSX문법에서 속성값을 부여하는 방식
- 둘 중 한가지만 사용이 가능하다.(혼용 x)
  
#### 1. 문자열로 속성 값을 정의하는 방식
``` javascript
const element = (
    <h1 id="title" className="highlight">Hello World!</h1>
);

```
#### 2. Javascript 표현식 ({})을 이용하여 정의하는 방식
``` javascript
const id = "title";
const className = "highlight";

    const element = (
        <h1 id="{id}" className={className}>
            Hello World!
        </h1>
    );
```

## JSX-inline-style-attribute
- 리액트에서 스타일을 적용하기 위한 객체의 속성명은 카멜케이스로 작성해야 한다.
  ex) background-color -> backgroundColor

- 속성 값은 문자열 혹은 숫자 형태로 작성해야 한다.
  ex) padding: 20px -> padding: 20 or '20px' 

``` javascript
console.log(window.style);
        const style = {
            backgroundColor: 'black',
            color: 'white',
            cursor: "pointer"
        padding: 20, // 단위를 작성하려면 문자열로 사용하지만 단위를 생략하면 숫자만 사용가능하다.(생략 시 px)
        };

        const element = (
            <>
                <h1 style={style}>Hello World!</h1>
                <h3>inline styling test</h3>
            </>
        );
```
<img src="react-img/react4.png">

## jsx event attribute

### 1. 인라인으로 간단한 이벤트 적용시 JSX의 자바스크립트 표현식 내에 이벤트 처리함수 작성
``` javascript
const element = (
            <>
                <h1>Event Attribute</h1>
                <button onClick={() => alert('hello world')}>클릭하세요</button>
            </>
);
ReactDOM.createRoot(document.getElementById("root")).render(element);
```

### 2. 이벤트 동작 시 사용할 핸들러 메소드를 사전에 정의 후 JSX 내에 표현식으로 사용
- onClick 속성을 작성할 때는 대소문자 체크를 잘해야 한다.
- 만약, 정상 동작하지 않으면 개발자 도구를 열어서 오류가 있는지 확인해본다.
``` javascript
const onClcikHandler = () => {
            alert('Hello World!');
        };

        // onClick 속성을 작성할 때는 대소문자 체크를 잘해야 한다.
        // 정상 동작하지 않으면 개발자 도구를 열어서 오류가 있는지 확인해본다.
        const element = (
            <>
                <h1>Event Attribute</h1>
                <button onClick={onClcikHandler}>클릭하세요</button>
            </>
        )
        ReactDOM.createRoot(document.getElementById("root")).render(element);
```

## JSX-comment(주석)

### 기존 주석
``` javascript
<style>
        /* CSS 주석 */
    </style>
</head>
<body>
    <!-- 
        HTML 주석
     -->
    <div id="root"></div>

    <script type="text/babel">
        // javascript 한줄 주석
        /* javascript 여러 줄 주석 */
    </script>
```

### JSX 주석

#### 기존 주석은 // 를 제외하고 사용 되지 않는다.
``` javacript
<>
    <h1>Comment in JSX</h1>
    /* JSX 내의 주석 */
</> // 오류 발생

<>
    <h1>Comment in JSX</h1>
    <!-- JSX 내의 주석 -->
</> // 오류 발생
```

#### JSX 기본주석 사용법
``` javascript
const element = (
    <>
        <h1>Comment in JSX</h1>
        {/* 여러 줄 주석 */}
        
        {
            // 한 줄 주석
        }
    </>
);
```

#### 태그 내에 주석 처리 또한 가능하다.
- 들여쓰기 포맷은 회사마다 표현이 다르기 때문에 컨벤션을 맞춰야한다.
- 시작 태그를 여러 줄로 작성한다면, 여러 줄에 주석이 가능하다.
``` javascript
const element = (
    <>
        <h3 // 들여쓰기 포맷은 회사마다 다르다.
        className="text" // 시작 태그를 여러 줄로 작성한다면 여기에도 주석을 작성할 수 있다.
        ></h3>
    </>
);
```

