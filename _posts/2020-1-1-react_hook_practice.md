---
layout: post
title:  "React Hook实战"
categories: react hook
tags: React React Hook
author: blueleek
---






### React Hook
React v16.7.0-alpha 中第一次提出了 Hooks 的概念，在 v16.8.0 版本被正式发布。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。<br/>

React Hook 是一种特殊的函数，其本事可以是函数式组件（返回 Dom 或 Dom 及 State），也可以是一个工具函数（传入配置项返回封装过后的数据处理逻辑）

#### 类组件的不足
* 状态逻辑难复用：在组件之间复用状态逻辑很难，可能要用到 render props（渲染属性）或者 HOC（高阶组件），但是无论渲染属性，还是高阶组件，都会在外层包裹一层父容器（一般是 div 元素），导致层级冗余。
* 趋向复杂难以维护
  * 在生命周期函数中混杂不相干的逻辑
  * 类组件中到处都是对组件的访问和处理，导致组件难以拆分为更小的组件
  
* this 指向问题：父组件给子组件传递函数时，必须绑定 this
  * react 中的组件四种绑定 this 方法的区别（前提：子组件x内部做了性能优化，如（React>PureComponent））
  ```
  class App extends React.Component<any, any> {
      handleClick2;
  
      constructor(props) {
          super(props);
          this.state = {
              num: 1,
              title: ' react study'
          };
          this.handleClick2 = this.handleClick1.bind(this);
      }
  
      handleClick1() {
          this.setState({
              num: this.state.num + 1,
          })
      }
  
      handleClick3 = () => {
          this.setState({
              num: this.state.num + 1,
          })
      };
  
      render() {
          return (<div>
              <h2>Ann, {this.state.num}</h2>
              <button onClick={this.handleClick2}>btn1</button>
              <button onClick={this.handleClick1.bind(this)}>btn2</button>
              <button onClick={() => this.handleClick1()}>btn3</button>
              <button onClick={this.handleClick3}>btn4</button>
          </div>)
      }
  }
  ```
  * 第一种在构造函数中绑定this：父组件刷新时，如果传递给自子组件的Props值不变，那么子组件就不会刷新；
  * 第二种是在render()函数里面绑定this：因为bind函数会返回一个新的函数，所以每次组件刷新时，都会重新生成一个函数，即使父组件传递给子组件的其他props值不变，子组件每次都会刷新；
  * 第三种是使用箭头函数：父组件刷新的时候，即使两个箭头函数的函数体是一样的，都会生成一个新的箭头函数，所以子组件每次都会刷新；
  * 第四种是使用类的静态属性：原理和第一种方法差不多，比第一种更简洁
  
  
  Hooks的优势
  * 可以优化类组件的不足
  * 可以在无需修改组件结构的情况下复用逻辑（自定义Hooks）
  * 可以将组件中相互关联的部分拆分成更小的函数（比如订阅或者请求数据）
  
  
  ### 性能优化
  
  #### 4.5.1 Object.is (浅比较)
  * Hook内部使用Object.is来比较新/旧 state 是否相等
  * 与 class 组件中的 setState 方法不同，如果你修改状态时，传的状态值没有变化，则不会重新渲染
  * 与 class 组件中的 setState 方法不同，useState 不会自动合并更新对象。你可以用函数式的setState 结合展开运算符来达到合并更新对象的效果
  
  ```
  function Counter(){
      const [counter,setCounter] = useState({name:'计数器',number:0});
      console.log('render Counter')
      // 如果你修改状态的时候，传的状态值没有变化，则不重新渲染
      return (
          <>
              <p>{counter.name}:{counter.number}</p>
              <button onClick={()=>setCounter({...counter,number:counter.number+1})}>+</button>
              <button onClick={()=>setCounter(counter)}>++</button>
          </>
      )
  }
  ```
  
####  减少渲染的次数
* 默认情况下，只要父组件状态变了（不管子组件依赖不依赖该状态），子组件也会重新渲染
* 一般的优化：
  1. 类组件： 可以使用 pureComponent；
  2. 函数组件： 使用 React.meo， 将函数组件传递给memo之后，就会返回一个新的组件，新组件的功能：如果接收的属性不变，则不重新渲染
  
* 怎样保证属性不变昵？ 这里使用的是useState， 每次更新都是独立的，const [number, setNumber] = useState(0) 也就是说每次都会生成一个新的值（哪怕这个值没有变化），即使使用了 React.memo，也还是会重新渲染

```
import React,{useState,memo,useMemo,useCallback} from 'react';

function SubCounter({onClick,data}){
    console.log('SubCounter render');
    return (
        <button onClick={onClick}>{data.number}</button>
    )
}
SubCounter = memo(SubCounter);
export  default  function Counter6(){
    console.log('Counter render');
    const [name,setName]= useState('计数器');
    const [number,setNumber] = useState(0);
    const data ={number};
    const addClick = ()=>{
        setNumber(number+1);
    };
    return (
        <>
            <input type="text" value={name} onChange={(e)=>setName(e.target.value)}/>
            <SubCounter data={data} onClick={addClick}/>
        </>
    )
}
```

* 深入优化
 1. useCallback: 接收一个内联回到函数参数和一个依赖项数组（子组件依赖父组件的状态，即子组件会使用到父组件的值），useCallback 会返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新
 2. useMemo： 把创建函数和依赖项数组作为参数传入 useMemo， 它仅会在某个依赖项改变时才会去重新计算 memoized 值，这种优化有助于在每次渲染时进行高开销的计算
```
 import React,{useState,memo,useMemo,useCallback} from 'react';
 
 function SubCounter({onClick,data}){
     console.log('SubCounter render');
     return (
         <button onClick={onClick}>{data.number}</button>
     )
 }
 SubCounter = memo(SubCounter);
 
 let oldData,oldAddClick;
 export  default  function Counter2(){
     console.log('Counter render');
     const [name,setName]= useState('计数器');
     const [number,setNumber] = useState(0);
     // 父组件更新时，这里的变量和函数每次都会重新创建，那么子组件接受到的属性每次都会认为是新的
     // 所以子组件也会随之更新，这时候可以用到 useMemo
     // 有没有后面的依赖项数组很重要，否则还是会重新渲染
     // 如果后面的依赖项数组没有值的话，即使父组件的 number 值改变了，子组件也不会去更新
     //const data = useMemo(()=>({number}),[]);
     const data = useMemo(()=>({number}),[number]);
     console.log('data===oldData ',data===oldData);
     oldData = data;
     
     // 有没有后面的依赖项数组很重要，否则还是会重新渲染
     const addClick = useCallback(()=>{
         setNumber(number+1);
     },[number]);
     console.log('addClick===oldAddClick ',addClick===oldAddClick);
     oldAddClick=addClick;
     return (
         <>
             <input type="text" value={name} onChange={(e)=>setName(e.target.value)}/>
             <SubCounter data={data} onClick={addClick}/>
         </>
     )
 }
```
 
#### useReducer
 * useReducer 和 redux 中 reducer 很像
 * useState 的替代方案，它接收一个形如(state, action) => newState 的 reducer， 并返回当前的 state 以及与其配套的 dispatch 方法
 * 在某些场景下，useReducer 会比 useState 更适用，例如 state 逻辑较复杂且包含多个值，或者下一个state 依赖于之前的 state


