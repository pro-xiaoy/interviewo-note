# react
> react我入职的公司中还没有用这个的，比较尴尬
### 受控组件 V.S. 非受控组件?
~~~js
 <FInput value={x} onChange={fn}/> // 受控组件
 <FInput defaultValue={x} ref={input}/> // 非受控组件

 区别受控组件的状态由开发者维护，非受控组件的状态由组件自身维护（不受开发者控制）
~~~

### React 有哪些生命周期函数？分别有什么用？（Ajax 请求放在哪个阶段？）
- componentWillMount
- render
- componentDidMount
- componentWillupdate
- render
- componentDidupdate

### shouldComponentUpdate 有什么用？
- 用于在没有必要更新 UI 的时候返回 false，以提高渲染性能

### react diff 原理
