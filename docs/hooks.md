- useState 
   - 定义初始值
- useEffect
   - useEffect(()=>{},[])
   - 没有第2个参数，组件的初始化和更新都会执行
   - [],调用一次之后不再执行
   - 有一个或者多个值的数组，有一个变化就执行
- useMemo
   - useMemo(()=>{return X},[])
   - useCallback(()=>{},[])
   - 当父组件传递给子组件的props是一个函数，防止函数被渲染的时候多次调用，用useMemo或者是useCallback.useCallback是useMemo的语法糖。没有返回值
   ```
    import React, { useCallback, useMemo, useState } from "react";
    function App() {
        const [n, setN] = useState(0);
        const [m, setM] = useState({ m: 1 });
        console.log("执行最外层盒子了");
        const addN = useMemo(() => {
          return () => {
            setN(n + 1);
          };
        }, [n]);
        const addM = useCallback(() => {
          setM({ m: m.m + 1 });
        }, [m]);
    return (
      <>
        <div>
          最外层盒子
          <Child1 value={n} click={addN} />
          <Child2 value={m} click={addM} />
          <button onClick={addN}>n+1</button>
          <button onClick={addM}>m+1</button>
        </div>
      </>
      );
  }
  const Child1 = React.memo((props) => {
    console.log("执行子组件1了");
    return <div>子组件1上的n：{props.value}</div>;
  });
  const Child2 = React.memo((props) => {
    console.log("执行子组件2了");
    return <div>子组件2上的m：{props.value.m}</div>;
  });
  const rootElement = document.getElementById("root");
  ReactDOM.render(<App />, rootElement);  
   ```
- useContext
   - context可以将全局的数据通过provider接口传递value给局部的组件，让包围在provider中的局部组件可以获取到全局数据的读写接口
   - 用法
      - 创建context  --->createContext
      - 设置provider并通过value接口传递state  ---->xx.provider
      - 局部组件获取读写接口  --->useContext
- useRef
   - useRef返回一个可变的ref对象，其.current属性被初始化为传入的参数（initialValue）。返回的ref对象在组件的整个生命周期内保持不变。这个ref对象只有一个current属性，你把一个东西保存在内，它的地址一直不会变。
   - 只要把ref挂到某个react元素上，就可以拿到它的dom。
   - 用法
      - useRef创建一个ref对象
      - ref={xxx}挂到react元素上
      - 子组件使用的时候，需要用到forwardRef
      ```
        const textInput = useRef(null)
        <Chind ref={textInput} />

        const child = forwardRef((props,ref)=>{
          return <input type='text' ref={ref} />
        })

      ```
- useReducer
   - 用来引入Reducer.Redux的核心概念是，组件发出action与状态管理器通信。状态管理器收到action以后，使用Reducer函数算出新的state。Reducer的函数的形式是（state,action）=> newState
   - const [state,diapatch] = useReducer(reducer,initialState)
   - 用法：
      - 首先定义一个reducer函数
      ```
        const reducer = (state, action) => {
          if(action.type === 'add'){
            return {
              n:state.n + 1
            }
          } else if(action.type === 'sub') {
            return {
              n: state.n -1
            }
          }
        }
      ```
      - 其次，使用useReducer接收Reducer和一个初始state,并返回当前值state和dispatch函数。当触发事件时，使用dispatch传递action，让reducer计算出新的state
      ```
        const [state, diapatch] = useReducer(reduce, initialState)
        <div>{state.n}</div>
        <button onClick={()=>{dispatch({type:'add'})}} />
        <button onClick={()=>{dispatch({type:'sub'})}}>
      ```
- 自定义hooks
   - 和自定义函数一样，在函数里面嵌套react的hooks