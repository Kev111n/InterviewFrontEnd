## useEffect、useCallback和useMemo

### useEffect

useEffect可以帮助我们在DOM更新完成后执行某些副作用操作，如数据获取，设置订阅以及手动更改 React 组件中的 DOM 等

```javascript
//基本用法
useEffect(() => {
    console.log('这是一个不含依赖数组的useEffect，每次render都会执行！')
})
```

**useEffect 规则**

- 没有传第二个参数时，在**每次 render 之后**都会执行 useEffect中的内容
- useEffect接受第二个参数来控制跳过执行，下次 render 后如果指定的值没有变化就不会执行
- useEffect 是在 render 之后浏览器已经渲染结束才执行

**useEffect 的第二个参数是可选的，类型是一个数组**

- 第二个参数为空数组

  useEffect 只在第一次渲染时执行，由于空数组中没有值，始终没有改变，所以后续render不执行，相当于生命周期中的`componentDidMount`

  ```javascript
  useEffect(() => { console.log('只在第一次渲染时执行') }, []); 
  ```

  

- 第二个参数为非空数组

无论数组中有几个元素，数组中只要有任意一项发生了改变，useEffect 都会调用

```javascript
useEffect(() => { getStuInfo({ id: stuId }); }, [getStuInfo, stuId]); //getStuInfo或者stuId改变时调用getStuInfo函数
```

**useEffect用作componentWillUnmount**

useEffect可以像让我们在组件即将卸载前做一些清除操作，如清空数据，清除计时器
**使用方法**：只需在现有的useEffect中返回一个函数，函数中为组件即将卸载前要做的操作

```javascript
useEffect(() => { 
    getStuInfo({ id: stuId }); 
    // 返回一个函数，在组件即将卸载前执行
    return ()=> {
        clearTimeout(Timer);   // 清除定时器
        data = null;   // 清空页面数据，当我们希望页面切换回来时不显示之前的内容时在组件卸载前清空数据，常用于搜索页面，切回时显示空内容，需重新搜索
    }
}, [getStuInfo, stuId]); 
```

### useCallback

useCallback接收两个参数，第一个参数是一个函数，第二个参数是依赖项数组，返回这个函数的缓存版本。类似下面：

```javascript
const memorizedCallback=useCallback(fn,deps);
```

缓存版本只有在依赖项数组中的某个依赖项改变才会更新，如果不使用useCallback每次渲染都会创建新的函数实例，会引发很多不必要的渲染，造成性能问题。

```javascript
import React, { useState, useCallback, useEffect } from 'react';
function Parent() {
    const [count, setCount] = useState(1);
    const [val, setVal] = useState('');
 
    const callback = useCallback(() => {
        return count;
    }, [count]);
    return <div>
        <h4>{count}</h4>
        <Child callback={callback}/>
        <div>
            <button onClick={() => setCount(count + 1)}>+</button>
            <input value={val} onChange={event => setVal(event.target.value)}/>
        </div>
    </div>;
}
 
function Child({ callback }) {
    const [count, setCount] = useState(() => callback());
    useEffect(() => {
        setCount(callback());
    }, [callback]);
    return <div>
        {count}
    </div>
}
```

例如在上面的代码中，将函数的缓存版本**callback**作为props传给子组件，就可以避免在**input**中输入修改**val**的值使用**setVal**引发重新渲染的同时引发子组件的重新渲染。如果不使用**useCallback**，直接将**callback**写成如下函数：

```javascript
function callback(){
    return count;
}
```

然后将**callback**传递给子组件，那么每次重新渲染时，都会创建新的函数实例，子组件中的**useEffect**，对比依赖项中的新旧**callback**时，就会发现新旧**callback**不相同，然后触发**useEffect**，子组件也会重新渲染。

### useMemo

**useMemo**用于缓存返回值，第一个参数为一个函数，因为**useMemo**是缓存返回值，所以希望这是一个拥有返回值的函数；和**useCallback**一样，**useMemo**的第二个参数为依赖数组，只有依赖数组中的数值发生变化才会重新执行第一个参数中的函数。参见格式如下：

```javascript
fn(..args){
//一些复杂计算
return result;
}
const memorizedValue=useMemo(fn(..args),deps);
```

**useMemo**会缓存fn的返回值，直到依赖数组中的值发生变化才会再次调用fn计算返回值，从而减少复杂计算的次数来优化性能。

虽然**useMemo**的初衷是用于缓存昂贵的计算的结果，但是**useMemo**也能实现一些与**useCallback**类似的效果——缓存函数

例如缓存下面这个防抖函数:

```javascript
function debounce(fn, wait=1000) {
  let timer = null;
  return function() {
    if(timer) {
      clearTimeout(timer);
      timer = null;
    }
    timer = setTimeout(() => {
      fn.apply(this, arguments);
    }, wait);
  }
}
```

用以下方法都可以缓存

用**useCallback**

```javascript
const debounced = useCallback(debounce(fn),[]);
```

用**useMemo**

```javascript
const debounced = useMemo(()=>debounce(fn),[]);
```

所以在一些时候`useMemo(()=>fn(...args),deps)`可以实现和`useCallback(fn(...args),deps)`一样的效果。
