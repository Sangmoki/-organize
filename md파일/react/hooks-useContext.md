# useContext
- 리액트 내의 컴포넌트를 전역에 대한 내용을 설정, 관리할 수 있게 해준다
- context는 React컴포넌트 트리 안에서 전역적으로 데이터를 공유할 수 있도록 고안된 방법이다.
- 트리 구조가 복잡해질 수록 하위 컴포넌트로 props를 전달하기 위해 drilling이 발생할 수 있게 되고,
  그러면 유지보수가 매우 힘들어지고 코드를 읽기도 힘들어지게 된다.
- 하지만 context를 사용하면 중간 엘리먼트들에게 props를 넘겨주지 않아도 되고
  유지보수도 훨씬 수월해지게 된다.

단, context를 사용하면 컴포넌트를 재사용하기 어려워지기 때문에 꼭 필요할 때만 써야한다.
따라서 때에 따라 context보다 컴포넌트 합성이 더 간단한 해결책이 될 수 도 있다.

## 기존 내용
- 유지보수가 힘들다
``` javascript
const { useState } = React;

function Header({isDark}){

    return (
        <header
            className="header"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <h1>Welcome to Greedy World!</h1>
        </header>
    );
}

function Content({isDark}) {

    return (
        <div
            className="content"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <p>내용입니다.</p>
        </div>
    );
}

function Footer({isDark, setIsDark}){
    const toggleHandler = () => setIsDark(!isDark);

    return (
        <footer
            className="footer"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <button onClick={toggleHandler}>{isDark? "Ligth Mode" : "Dark Mode"}</button>
            Copyright 2023. team-angel. all rights reserved.
        </footer>
    );
}

function Page({ isDark, setIsDark }) {
    return(
        <div 
            className="page"
        >
            <Header isDark={isDark} />
            <Content isDark={isDark} />
            <Footer isDark={isDark} setIsDark={setIsDark} />
        </div>    
    );
}

function App() {

    const [isDark, setIsDark] = useState(false);

    return <Page isDark={isDark} setIsDark={setIsDark} />
}
ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

## useContext를 사용한 편의성
- context를 사용할 때는 createContext, useContext 두가지를 넣어야한다.
-  context객체를 구독하고 있는 컴포넌트를 렌더링 할 때 React는 트리 상위에서 가장 가까운
  provider로 부터 현재 값을 읽어들인다. 
### createContext
- context객체를 만든다. 이 때 인자로 defaultValue를 전달하며,
    defaultValue가 없는 경우 null로 설정한다.

``` javascript
 /* 
    context는 React컴포넌트 트리 안에서 전역적으로 데이터를 공유할 수 있도록 고안된 방법이다.
    트리 구조가 복잡해질 수록 하위 컴포넌트로 props를 전달하기 위해 drilling이 발생할 수 있게 되고,
    그러면 유지보수가 매우 힘들어지고 코드를 읽기도 힘들어지게 된다.
    하지만 context를 사용하면 중간 엘리먼트들에게 props를 넘겨주지 않아도 되고, 유지보수도 훨씬 수월해지게 된다.

    단, context를 사용하면 컴포넌트를 재사용하기 어려워지기 때문에 꼭 필요할 때만 써야한다.
    따라서 때에 따라 context보다 컴포넌트 합성이 더 간단한 해결책이 될 수 도 있다.
*/

/* context를 사용할 때는 createContext, useContext 두가지를 넣어야한다. */
const { useState, createContext, useContext } = React;

const DarkModeContext = createContext(null); // 기본 값을 줄 게 없으면 null을 준다.
const UserContext = createContext();
/* 
    createContext - context객체를 만든다. 이 때 인자로 defaultValue를 전달하며,
                    defaultValue가 없는 경우 null로 설정한다.
    context객체를 구독하고 있는 컴포넌트를 렌더링 할 때 React는 트리 상위에서 가장 가까운
    provider로 부터 현재 값을 읽어들인다. 
*/

function Header(){

    const context = useContext(DarkModeContext);
    const {isDark} = context;
    return (
        <header
            className="header"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <h1>Welcome to Greedy World!</h1>
        </header>
    );
}

function Content() {

    const context = useContext(DarkModeContext);
    const {isDark} = context;

    const usenameContext = useContext(UserContext);
    const { username } = usenameContext;
    
    return (
        <div
            className="content"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <p>내용입니다.</p>
            <br />
            <p>{username}님 안녕하세요.</p>
        </div>
    );
}

function Footer(){
    const context = useContext(DarkModeContext);
    const {isDark, setIsDark} = context;
    const toggleHandler = () => setIsDark(!isDark);

    return (
        <footer
            className="footer"
            style={{
                backgroundColor: isDark? "black" : "lightgray",
                color: isDark? "white" : "black"
            }}
        >
            <button onClick={toggleHandler}>{isDark? "Ligth Mode" : "Dark Mode"}</button>
            Copyright 2023. team-angel. all rights reserved.
        </footer>
    );
}

function Page() {
    return(
        <div className="page">
            <Header />
            <Content />
            <Footer />
        </div>    
    );
}

function App() {

    const [isDark, setIsDark] = useState(false);
    const [username, setUsername] = useState("홍길동");

    /* 
        Provider는 context를 구독하고 있는 컴포넌트들에게 context의 변화를 알리는 역할을 한다.
        Provider는 value prop를 이용하여 하위 컴포넌트에게 값을 전달한다.
        이 때 값을 전달받을 수 있는 컴포넌트 수에는 제한이 없다.

        Provider 하위에서 context를 구독하는 모든 컴포넌트는 value prop이 바뀔 때마다 다시 렌더링 된다.
    */
    return (
        <UserContext.Provider value={{username}}>
        <DarkModeContext.Provider value={{ isDark, setIsDark }}>
            <Page />
        </DarkModeContext.Provider>
        </UserContext.Provider>
    );
}
ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```
- Provider는 context를 구독하고 있는 컴포넌트들에게 context의 변화를 알리는 역할을 한다.
- Provider는 value prop를 이용하여 하위 컴포넌트에게 값을 전달한다.
- 이 때 값을 전달받을 수 있는 컴포넌트 수에는 제한이 없다.
- Provider 하위에서 context를 구독하는 모든 컴포넌트는 value prop이 바뀔 때마다 다시 렌더링 된다.