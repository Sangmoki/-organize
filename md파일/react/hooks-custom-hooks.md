# 훅을 커스텀하여 활용하기

## 이벤트에 조건 넣어 유효성 검사하기
- 표현해야 할 값들이 늘어날 수록 불필요한 코드가 늘어나 유지 보수가 힘들다.
- 그래서 사용자 정의 훅을 만들어 사용하는 방법이 좋지만,
  양이 많지 않으면 굳이 안바꿔도 된다.
``` javascript
const { useState } = React;

function App() {
    const [username, setUsername] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    const handlerUsername = (e) => setUsername(e.target.value);
    const handlerEmail = (e) => setEmail(e.target.value);
    const handlerPassword = (e) => {
        setPassword(e.target.value);
        password.length > 10 && alert("stop");
    }
    return (
        <div>
            <label>이름 : </label>
            <input type="text" value={username} onChange={handlerUsername} />
            <br />
            <label>비밀번호 : </label>
            <input type="password" value={password} onChange={handlerPassword} />
            <br />
            <label>이메일 : </label>
            <input type="email" value={email} onChange={handlerEmail} />
            <br />
            <h4>username : {username}</h4>
            <h4>password : {password} </h4>
            <h4>email : {email}</h4>
        </div>
    );
}

ReactDOM.createRoot(document.getElementById("root")).render(<App />);
```

## 사용자 정의 훅을 사용하여 유지보수가 용이하게 만든 코드
``` javascript
const { useState } = React;

function App() {
    const [username, setUsername] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");

    const handlerUsername = (e) => setUsername(e.target.value);
    const handlerEmail = (e) => setEmail(e.target.value);
    const handlerPassword = (e) => {
        setPassword(e.target.value);
        password.length > 10 && alert("stop");
    }
    return (
        <div>
            <label>이름 : </label>
            <input type="text" value={username} onChange={handlerUsername} />
            <br />
            <label>비밀번호 : </label>
            <input type="password" value={password} onChange={handlerPassword} />
            <br />
            <label>이메일 : </label>
            <input type="email" value={email} onChange={handlerEmail} />
            <br />
            <h4>username : {username}</h4>
            <h4>password : {password} </h4>
            <h4>email : {email}</h4>
        </div>
    );
}

ReactDOM.createRoot(document.getElementById("root")).render(<App />);
```
- useInput에 담긴 value와 onChange를 username, password, email에 담아줬으니
- 기존 value={password.value} onChange={password.onChange} 로 사용했던 것을
- 스프레드 문법{...username} 식으로 펼치면 동일하게 사용할 수 있다.