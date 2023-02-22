# 함수형 컴포넌트의 문제점
- 원시형 타입이 아닌, 객체 타입은 값이 바뀔때마다 불필요하게 초기화 되기 때문에
  원하는 결과를 볼 수 없다.
``` javascript
const { useState, useEffect} = React;

function App() {

    const [number, setNumber] = useState(0);
    const [toggle, setToggle] = useState(false);

    const printNumber = () => {
        console.log(`current number : ${ number }`);
    }

    /* printNumber가 변경될 때만 호출되도록 한다. 
        하지만, printNumber함수는 객체이고, App을 재호출 할 때마다 다시 초기화가 되기 때문에
        toggle을 변경할 때도 printNumber는 불필요하게 초기화가 다시 된다.
    */
    useEffect(() => {
        console.log("printNumber 함수가 변경되었습니다.");
    }, [printNumber]);
    return (
        <>
            <input type="number" value={number} onChange={(e) => setNumber(e.target.value)}/>
            <button onClick={() => setToggle(!toggle)}>{toggle.toString()}</button>
            <br />
            <button onClick={printNumber}>PrintNumberState</button>
        </>
    );
} 

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```
- 즉, printNumber는 state로 관리되는 것이 아니라, 지역변수로 관리되고있기 때문에 rerendering 이 일어나면 새로운 주소값을 할당하기 때문에 계속 값이
변경된다고 인지를 해서 [printNumber]가 효과가 없게 된다.

## useCallBack을 이용한 해결

``` javascript
const { useState, useEffect, useCallback} = React;

function App() {

    const [number, setNumber] = useState(0);
    const [toggle, setToggle] = useState(false);

    const printNumber = useCallback(() => {
        console.log(`current number : ${ number }`);
    }, [number]); // 빈 배열을 넣으면 최초 로딩 시 한번만 함수 생성을 하고 재사용하기 때문애 state는 항상 기본값이다.
                    // number를 넣으면 상태값이 그때그때 변경되고, true, false를 눌렀을 때도 사용되던 부분이 해결되었다.

    useEffect(() => {
        console.log("printNumber 함수가 변경되었습니다.");
    }, [printNumber]);
    return (
        <>
            <input type="number" value={number} onChange={(e) => setNumber(e.target.value)}/>
            <button onClick={() => setToggle(!toggle)}>{toggle.toString()}</button>
            <br />
            <button onClick={printNumber}>PrintNumberState</button>
        </>
    );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

# useCallback을 활용한 composited 컴포넌트
- useCallback을 사용하기 전에는 기존 문제점과 동일했다.
- useCallback을 사용해 기존 객체로 사용해 생긴 문제점을 
  함수 내부의 속성을 직접 담아 문제를 해결했다.
``` javascript
const { useState, useEffect, useCallback } = React;

function App() {
    const [size, setSize] = useState(200);
    const [isDark, setIsDark] = useState(false);

    // const genSquareStyle = () => {

    //     return {
    //         backgroundColor:"orangered",
    //         width: `${size}px`,
    //         height: `${size}px`
    //     };
    // };

    const genSquareStyle = useCallback(() => {

        return {
            backgroundColor: "orangered",
            width: `${size}px`,
            height: `${size}px`,
        };
    }, [size]);

    return (
        <div style={{ backgroundColor: isDark? "black":"white" }}>
            <input type="range" min="100" max="300" 
                    value={size} 
                    onChange={(e) => setSize(e.target.value)} />
            <button onClick={() => setIsDark(!isDark)}>{isDark.toString()}</button>
            <Square genSquareStyle={genSquareStyle} />
        </div>
    );
}

function Square({genSquareStyle}) {
    const [style, setStyle] = useState({});
    
    useEffect(() => {
        console.log('style 변경');
        setStyle(genSquareStyle());
    }, [genSquareStyle]);
    
    return <div style={style}></div>
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```