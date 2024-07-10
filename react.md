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

- 没有传第二个参数时，在每次 rend	er 之后都会执行 useEffect中的内容
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