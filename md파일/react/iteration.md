# 반복자
## arr.map(callback, [thisArg])
- 파라미터로 전달된 함수를 사용해서 배열 내 각 요소를
  원하는 규칙에 따라 변환 후 그 결과로 새로운 배열을 반환한다.

## callback
- 새로운 배열 요소를 생성하는 함수로 파라미터는 3가지를 가진다.
1. currentValue : 현재 처리하고 있는 요소
2. index : 현재 처리하고 있는 요소의 index 값
3. array : 현재 처리하고 있는 원본 배열
번외로 thisArg : callback함수 내부에서 사용할 this레퍼런스(선택사항)

### 예시
``` javascript
const numbers = [1, 2, 3, 4, 5];
const results = numbers.map(num => num * num);

console.log(results); // 결과 : [1, 4, 9, 16, 25]
```

## 본 코드
``` javascript
function Items({ names }) { // 자바스크립트 문법으로 인자값에 names 넣어줘야 함수 안에서 사용할 수 있다.

    // return (
    //     <ul>
    //         { names.map((name) => (<li>{ name }</li>)) } 
    //         {/* map메서드를 통해 names의 값을 name에다 하나씩 담아줄거다. */}
    //     </ul>
    // ); // 화면에 뿌려주긴 하지만 key에 대한 prop가 없어서 아래 새로 정의해준다.

    return (
        <ul>
            { names.map((name, index) => ( // index를 이용해 unique로 만들어준다.
                <li key={ index }>{ name }</li>
            )) } 
            {/* map메서드를 통해 names의 값을 name에다 하나씩 담아줄거다. */}
        </ul>
    );
}

const names = ['홍길동', '유관순', '이순신', '임꺽정', '윤봉길']; // 사용할 객체생성


ReactDOM.createRoot(document.getElementById('root')).render(<Items names={names} />);
``` 


## 반복자 샘플
``` javascript
const { useState } = React;

function App() {

    const [names, setNames] = useState([
        { id:1, name: '홍길동'},
        { id:2, name: '유관순'},
        { id:3, name: '이순신'},
    ]);

    const nameList = names.map(name => <li key={name.id}>{name.name}</li>)
    // 반복을 돌면서 해당 객체에 대한 내용을 반환하고 키 값과 이름을 불러와따
    return (
        <>
            <ul>
                {nameList}
            </ul>
        </>
    )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

### filter()
- filter : 배열 요소 전체를 대상으로 콜백 함수 호출 후 반환값이 true인 요소로만 구성된 새로운 배열 반환
``` javascript
const odds = numbers.filter(item => item % 2);  item % 2 = 0 (false) 
```
- 즉, 결과값이 0이면 제외하고 0이 아닌 나머지수
(-포함)면 포함한다.

### concat()
- 리액트에서는 기존 상태를 그대로 두면서 새로운 값으로 상태를 설정해야한다.
- 이를 불변성 유지라고 한다. 
- 따라서 새로운 배열을 반환하는 concat을 사용한다.
``` javascript
const { useState } = React;

function App() {

    const [names, setNames] = useState([
        { id:1, name: '홍길동'},
        { id:2, name: '유관순'},
        { id:3, name: '이순신'},
    ]);

    const [inputText, setInputText] = useState('');
    const [nextId, setNextId] = useState(4); // 위에가 3이어서 4부터 시작하겠다고 선언

    const onChangeHandler = (e) => setInputText(e.target.value);
    const onClickHandler = () => {

        /* 리액트에서는 기존 상태를 그대로 두면서 새로운 값으로 상태를 설정해야한다.
            이를 불변성 유지라고 한다. 따라서 새로운 배열을 반환하는 concat을 사용한다.
        */
        const changeNames = names.concat({ // 추가되는 객체를 콘캣의 인자로 넣어줘야 한다.
            id: nextId,
            name: inputText,
        });

        setNextId(nextId + 1); // 초기 id=4에서 +1 해가는 과정
        setNames(changeNames); // 기존의 names 값 수정
        setInputText(""); // 인풋이 끝나면 초기화
    }

    const onRemove = id => {
        const changeNames = names.filter((name) => name.id !== id) 
        // filter도 순회를 돌면서 안의 내용들을 찾아 name의 키값이 안맞으면 걸러낸다.
        // 선택한것만 뺴고 나머지는 걸러낸다.
        
        setNames(changeNames);
    };

    const nameList = names.map((name) => ( 
    <li 
        key={name.id} 
        onDoubleClick={ () => 
            onRemove(name.id)}>{name.name}
    </li>));
    // 반복을 돌면서 해당 객체에 대한 내용을 반환하고 키 값과 이름을 불러와따
    return (
        <> 
            <input type="text" value={ inputText } onChange={ onChangeHandler } />
            <button onClick={onClickHandler}>추가</button>
            <ul>{nameList}</ul>
        </>
    )
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```
## todo-list 만들기 너무 기니까 참고할까? 아냐 하자
``` javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8" />
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
        <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
        <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
        <style>
            body {
                margin: 0;
            }

            .container {
                height: 100hv;
                background: skyblue;
                width: 100%;
                height: 100vh;
                display: flex;
                flex-direction: column;
            }

            .header {
                width: 100%;
                height: 100px;
                background: black;
                color: white;
                display: flex;
                justify-content: center;
                align-items: center;
                flex-direction: column;
            }

            .content {
                flex-grow: 1;
                background: lightgray;
                display: flex;
                justify-content: center;
                align-items: center;
                flex-direction: column;
            }

            .todo-list {
                background: white;
                width: 500px;
                border: 1px solid black;
                height: 500px;
                display: flex;
                flex-direction: column;
                align-items: flex-start;
                padding-left: 50px;
            }

            .todo-list label {
                margin: 20px;
            }

            .todo-list button {
                border: none;
                background: red;
                border-radius: 10px;
                color: white;
            }

            .append-list button {
                background: green;
                color: white;
                border: none;
                border-radius: 5px;
                height: 30px;
            }

            .footer {
                width: 100%;
                height: 100px;
                display: flex;
                background: black;
                color: white;
                justify-content: center;
                align-items: center;
                flex-direction: column;
            }
        </style>
    </head>
    <body>
        <div id="root"></div>

        <script type="text/babel">
            const { useState } = React;

            function Header() {
                return (
                    <div className="header">
                        <h1>오늘의 할일! {new Date().toLocaleTimeString()}</h1>
                    </div>
                );
            }

            function Content() {
                return (
                    <div className="content">
                        <TodoList />
                    </div>
                );
            }

            function Footer() {
                return (
                    <div className="footer">
                        <p>Copyright 2023. team-angel all rights reserved.</p>
                    </div>
                );
            }

            function TodoItems({ todos, setTodos }) {

                const removeTodo = (id) => {
                    const removeList = todos.filter((todo) => todo.id !== id);

                    setTodos(removeList);
                }

                const onChangeHandler = (e) => {
                    const changeIsDoneList = todos.map((todo) => {
                        
                        if(todo.id === parseInt(e.target.id)) {
                            todo.isDone = e.target.checked;
                        }
                        
                        return todo;
                    });

                    setTodos(changeIsDoneList);
                };

                return (
                    <>
                        {todos.map((todo) => (
                            <p key={todo.id}>
                                <input type="checkbox" id={todo.id} onChange={onChangeHandler} />
                                <label
                                    htmlFor={todo.id}
                                    style={{ textDecoration: todo.isDone ? "line-through" : "none" }}
                                >
                                    {todo.description}
                                </label>
                                <button onClick={() => removeTodo(todo.id)}>x</button>
                            </p>
                        ))}
                    </>
                );
            }

            function TodoList() {
                const [todos, setTodos] = useState([{ id: 1, description: "할일 목록을 추가할 것", isDone: false }]);
                const [inputText, setInputText] = useState("");
                const [nextId, setNextId] = useState(2);

                const onChangeHandler = (e) => {

                    setInputText(e.target.value);
                }

                const onClickHandler = () => {

                    const changeTotos = todos.concat({
                        id: nextId,
                        description: inputText,
                        isDone: false,
                    });

                    setInputText("");
                    setNextId(nextId +1);
                    setTodos(changeTotos);
                };
                return (
                    <>
                        <h3>ToDo-List</h3>
                        <div className="todo-list">
                            <TodoItems todos={todos} setTodos={setTodos} />
                        </div>
                        <div className="append-list">
                            <input type="text" value={inputText} onChange={onChangeHandler} />
                            <button onClick={onClickHandler}>추가하기</button>
                        </div>
                    </>
                );
            }

            function TodoApp() {
                return (
                    <div className="container">
                        <Header />
                        <Content />
                        <Footer />
                    </div>
                );
            }
            ReactDOM.createRoot(document.getElementById("root")).render(<TodoApp />);
        </script>
    </body>
</html>

```