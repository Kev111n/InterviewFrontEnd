## bind、call、apply区别

`call`、`apply`、`bind`作用是改变函数执行时的上下文，简而言之就是改变函数运行时的`this`指向

`apply`、`call`、`bind`三者的区别在于：

- 三者都可以改变函数的`this`对象指向

- 三者第一个参数都是`this`要指向的对象，如果如果没有这个参数或参数为`undefined`或`null`，则默认指向全局`window`

- 三者都可以传参，但是`apply`是数组，而`call`是参数列表，且`apply`和`call`是一次性传入参数，而`bind`可以分为多次传入

- `bind`是返回绑定this之后的函数，`apply`、`call` 则是立即执行

  eg:

  ```javascript
  function fn(...args){
      console.log(this,args);
  }
  let obj = {
      myname:"张三"
  }
  fn.apply(obj,[1,2]); // this会变成传入的obj，传入的参数必须是一个数组；
  fn(1,2) // this指向window
  
  fn.call(obj,1,2); // this会变成传入的obj，传入的参数必须是一个数组；
  fn(1,2) // this指向window
  
  const bindFn = fn.bind(obj); // this也会变成传入的obj ，bind不是立即执行需要执行一次
  bindFn(1,2) // this指向obj
  fn(1,2) // this指向window
  ```

## JS的基本数据类型

JS的基本数据类型有8种：Number、String、Boolean、Null、undefined、object、symbol、bigInt。存放在栈中

复杂类型（引用类型）：Object、Array、Function。栈存放的是地址，对象实例存放在堆中

## 事件循环的理解

同步任务进入主线程，即主执行栈，异步任务进入任务队列，主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。上述过程的不断重复就事件循环

**同步任务：** 在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务。是**由 js 执行栈/回调栈执行**的

**异步任务：** 不进入主线程、而进入任务队列的任务，当主线程中的任务运行完成了，才会从任务队列中取出异步任务，放入主线程中执行，分为**宏任务**和**微任务**

执行顺序：同步任务>微任务 >宏任务

#### 宏任务

宏任务是**由宿主环境发起**的，比如浏览器、Node等，宏任务的异步代码有：

- script（代码块）

- setTimeout / setInterval 定时器

- setImmediate 定时器

- ......等等

#### 微任务

微任务是**由JS引擎发起**的，微任务的异步代码有：

- process.nextTick (node)

- Promise.then( )/catch( ) 。注意，**`Promise`本身同步，只是它里面的`then/catch`的回调函数是异步的微任务**

- Async/Await

- Object.observe

- ......等等

## esm和commonjs的区别

ESM（ECMAScript Modules）和 CommonJS 是 JavaScript 中两种不同的模块系统。它们都允许将代码拆分成可重用的模块，并在需要时导入这些模块。尽管它们都实现了相似的功能，但它们之间存在一些关键差异：

1. 语法：ESM 和 CommonJS 使用不同的语法来导入和导出模块。

   - ESM 使用 `import` 和 `export` 关键字
   - CommonJS 使用 `require` 和 `module.exports`关键字

2. 运行时加载与静态加载：

   - CommonJS 是**运行时加载**，这意味着模块在运行时解析和加载。因此，在运行时可以动态修改模块和依赖关系。
   - ESM 是**静态加载**，这意味着模块在编译时解析和加载。这允许更好的优化，如代码消除和更快的加载速度，但不允许在运行时动态修改模块。

3. 作用域：ESM 和 CommonJS 在处理变量作用域方面有所不同。

   - ESM 使用**模块作用域**，每个模块具有自己的顶级作用域。在模块内声明的变量不会污染全局作用域。
   - CommonJS 使用**文件作用域**，但与 ESM 不同，CommonJS 模块可以通过 `global` 对象访问全局作用域。

4. 循环依赖：ESM 和 CommonJS 处理循环依赖的方式不同。

   - ESM 可以更好地处理循环依赖，因为模块是静态加载的。在循环依赖中，导入的值可能是不完整的，但不会导致错误。
   - CommonJS 在处理循环依赖时可能会遇到问题，因为模块是运行时加载的。这可能导致在循环依赖中的模块中获得一个不完整的对象。

5. 兼容性和使用场景：

   - CommonJS 主要用于 Node.js 环境，因为它是 Node.js 的原生模块系统。虽然现代 Node.js 版本也支持 ESM，但很多旧的 Node.js 代码仍使用 CommonJS。然而，许多新的 Node.js 项目逐渐采用 ESM。
   - ESM 通常用于现代 Web 开发，因为大多数现代浏览器原生支持 ESM。在使用构建工具（如 Webpack、Rollup 或 Parcel）时，ESM 也提供了更好的优化和打包能力。

6. 实时绑定与值拷贝：

   - ESM 使用**实时绑定**，当导入的值发生更改时，导入模块的值也会跟着更改。这意味着导入的值始终保持最新。
   - CommonJS 使用**值拷贝**，当模块被导入时，值被复制到导入模块。这意味着在导入模块中，值的更改不会反映到原始模块，导入的值在导入时是固定的。

7. 导出值：

   - ESM 导出值是**映射关系**，**可读，不可修改**，但可通过导出的函数修改导出的值。
   - CoomonJS 导出**值的拷贝**，**可以修改导出的值**。

8. export使用：

   - ESM export和export default支持一起使用。
   - CoomonJS module.exports和exports不支持一起使用，会被覆盖。

   

总结一下，ESM 和 CommonJS 的主要区别在于它们的语法、加载机制、作用域、循环依赖处理、兼容性和使用场景以及实时绑定与值拷贝。尽管它们在某些方面有所不同，它们都是为了解决 JavaScript 模块化编程的问题。

## 什么是闭包？

闭包是指一个函数可以访问另一个函数作用域内的变量。当一个函数嵌套在另一个函数中时，内部函数可以访问外部函数的变量，即使外部函数已经返回了。这种情况下，内部函数形成了一个闭包，它保留了外部函数的作用域链并可以继续访问这些变量。闭包常常用于实现函数的封装和私有化，以及在回调和事件处理等场景下的数据共享与传递。(例如VUE中写的hooks，只暴露里面的函数)

## promise的理解（细节promise.then的第二个参数和.catch的区别，.catch后面再then还会执行吗？promise的值穿透）

`Promise` 是 JavaScript 中的一个对象，用于处理异步操作和值/错误传递。让我们逐一解析您提到的几个关键点。

**1. `promise.then` 的第二个参数**

`promise.then()` 方法接受两个参数：

1. 第一个参数是当 `Promise` 被解决（fulfilled）时调用的函数。
2. 第二个参数是（可选的）当 `Promise` 被拒绝（rejected）时调用的函数。

这是一个例子：

```javascript
let promise = new Promise((resolve, reject) => {  
    // 模拟异步操作  
    setTimeout(() => {  
        if (/* 某些条件 */) {  
            resolve('操作成功');  
        } else {  
            reject('操作失败');  
        }  
    }, 1000);  
});  
  
promise.then(  
    result => console.log(result), // 当 Promise 被解决时执行  
    error => console.log(error)    // 当 Promise 被拒绝时执行  
);
```

但通常，我们更倾向于使用 `.catch()` 来处理错误，因为它使得错误处理更加明显和集中。

**2. `.catch` 和 `.then` 的区别**

`.catch()` 是 `.then(null, rejectionHandler)` 的语法糖，它专门用于处理 `Promise` 的拒绝情况。`.catch()` 总是返回一个新的 `Promise`，这使得错误处理可以链式地进行。

使用 `.catch()` 的一个好处是，你可以确保所有的错误都被捕获，而不仅仅是直接跟在 `.then()` 后面的拒绝处理函数。

**3.`.catch` 后面再 `.then` 还会执行吗？**

是的，`.catch()` 后面再链式调用 `.then()` 是会执行的。因为 `.catch()` 会返回一个新的 `Promise`，而这个新的 `Promise` 可以被进一步处理（例如，使用 `.then()`）。

```javascript
promise  
    .catch(error => {  
        console.error('捕获到错误:', error);  
        // 你可以选择在这里处理错误，或者只是记录它，然后返回一个新的值或另一个 Promise  
        return '错误已处理';  
    })  
    .then(result => {  
        // 这里的 result 可能是 promise 被解决时的值，或者是 .catch() 中返回的值  
        console.log(result);  
    });
```

**4. Promise 的值穿透**

在 `Promise` 链中，如果一个处理程序（无论是 `.then()` 的解决处理程序还是 `.catch()` 的拒绝处理程序）返回一个值（而不是另一个 `Promise`），那么这个值会被“穿透”到下一个 `.then()` 的解决处理程序中。这叫做值穿透。

但如果处理程序返回一个 `Promise`，那么链会等待这个新的 `Promise` 解决或拒绝，然后才会继续到下一个 `.then()` 或 `.catch()`。

## async和await和promise的关联，怎么实现的？，generator的理解（中断和恢复怎么实现？）

**async/await 和 Promise 的关联及实现**

`async/await` 是建立在 `Promise` 基础上的语法糖，用于简化异步编程的复杂性。`async` 函数总是返回一个 `Promise`，而 `await` 只能在 `async` 函数内部使用，用于等待一个 `Promise` 解决（fulfilled）或拒绝（rejected）。

**实现原理：**

1. async 函数：
   - 当一个函数被声明为 `async` 时，它返回的是一个 `Promise` 对象。
   - 函数体中的 `return` 语句返回的值会被 `Promise.resolve()` 包裹，成为 `Promise` 的解决值。
   - 如果函数体中的代码抛出一个错误，`Promise` 会被拒绝，错误会被 `Promise.reject()` 包裹。
2. await 表达式：
   - `await` 等待一个 `Promise`。它暂停 `async` 函数的执行，直到 `Promise` 解决或拒绝。
   - 如果 `Promise` 被解决，`await` 表达式的值就是 `Promise` 的解决值。
   - 如果 `Promise` 被拒绝，`await` 会抛出错误，可以在 `async` 函数中通过 `try/catch` 捕获。

**示例：**

```javascript
async function fetchData() {  
  try {  
    const response = await fetch('https://api.example.com/data');  
    const data = await response.json();  
    return data;  
  } catch (error) {  
    console.error('Error fetching data:', error);  
    throw error; // 如果需要的话，可以将错误传递给上层的调用者  
  }  
}  
  
// 调用 fetchData 函数，它会返回一个 Promise  
fetchData().then(data => console.log(data))  
  .catch(error => console.error('Error:', error));
```

**Generator 的理解及中断和恢复的实现**

`Generator` 是一种特殊的函数，它允许你暂停和恢复函数的执行。`Generator` 函数通过 `function*` 语法声明，并使用 `yield` 关键字来暂停和恢复执行。

**中断和恢复的实现：**

1. **中断**：当 `Generator` 函数中执行到 `yield` 表达式时，它会返回一个迭代器对象。此时，函数执行被中断，控制权交还给调用者。
2. **恢复**：调用者可以调用迭代器的 `next()` 方法来恢复 `Generator` 函数的执行。`next()` 方法会返回一个对象，该对象有两个属性：`value`（`yield` 表达式的值）和 `done`（一个布尔值，表示 `Generator` 函数是否已经结束）。

**示例：**

```javascript
function* generatorFunction() {  
  console.log('Start');  
  yield 'First yield';  
  console.log('Middle');  
  yield 'Second yield';  
  console.log('End');  
}  
  
const iterator = generatorFunction();  
  
console.log(iterator.next().value); // 输出: Start，然后 First yield  
console.log(iterator.next().value); // 输出: Middle，然后 Second yield  
console.log(iterator.next().value); // 输出: End，然后 undefined（因为 done 为 true）
```

在上面的示例中，你可以看到 `Generator` 函数是如何通过 `yield` 表达式中断执行，并通过调用 `next()` 方法恢复执行的。每次调用 `next()` 时，函数都会从上次 `yield` 的位置继续执行，直到遇到下一个 `yield` 或函数结束。

## for in和for of的使用和区别（of能拿到的值）

在JavaScript中，`for...in`和`for...of`是两种常用的循环遍历数组（以及类数组对象）或对象的方法，但它们的使用场景和能获取到的值有所不同。

**1.`for...in`**

`for...in`循环用于遍历对象的可枚举属性（包括从原型链上继承的属性）。其基本语法如下：

```javascript
for (let key in object) {  
    // 在这里，key 是对象的属性名  
    let value = object[key]; // 通过属性名获取属性值  
    // ...  
}
```

**注意**：由于`for...in`会遍历对象及其原型链上的所有可枚举属性，因此通常不推荐用于遍历数组，除非你确定要遍历数组的所有可枚举属性（包括继承自`Array.prototype`的属性）。

**2.`for...of`**

`for...of`循环用于遍历可迭代对象（如Array，Map，Set，String，TypedArray，函数的arguments对象等等）的元素。其基本语法如下：

```javascript
for (let value of iterable) {  
    // 在这里，value 是可迭代对象的元素值  
    // ...  
}
```

**注意**：`for...of`循环只能用于可迭代对象。对于普通对象，由于其不是可迭代对象，所以不能直接使用`for...of`循环进行遍历。

**示例**

```javascript
let arr = [1, 2, 3];  
let obj = { a: 1, b: 2, c: 3 };  
  
// 使用for...in遍历数组（不推荐）  
for (let key in arr) {  
    console.log(key); // 输出：0, 1, 2（数组索引）  
    console.log(arr[key]); // 输出：1, 2, 3（数组元素值）  
}  
  
// 使用for...in遍历对象  
for (let key in obj) {  
    console.log(key); // 输出：a, b, c（对象属性名）  
    console.log(obj[key]); // 输出：1, 2, 3（对象属性值）  
}  
  
// 使用for...of遍历数组  
for (let value of arr) {  
    console.log(value); // 输出：1, 2, 3（数组元素值）  
}  
  
// 尝试使用for...of遍历对象（会报错）  
// for (let value of obj) { ... } // TypeError: obj is not iterable
```

**总结**

- `for...in`遍历的是对象的属性名，可以通过属性名获取属性值。
- `for...of`遍历的是可迭代对象的元素值。
- `for...in`不仅遍历对象自身的属性，还会遍历其原型链上的属性（除非使用`hasOwnProperty`等方法进行过滤）。
- `for...of`只能用于可迭代对象，不能用于普通对象。

## 类数组，转换为数组怎么实现

类数组（array-like objects）是那些具有数值索引和 `length` 属性的对象，但它们并不是真正的数组（Array）。在JavaScript中，常见的类数组对象包括函数的 `arguments` 对象、`NodeList` 对象（来自DOM查询）等。

将类数组对象转换为真正的数组有多种方法，以下是其中的几种：

### **1.使用`Array.prototype.slice.call()`**

这是早期在JavaScript中转换类数组对象到数组的常见方法。

```javascript
function convertToArray(arrayLike) {  
    return Array.prototype.slice.call(arrayLike);  
}  
  
// 使用示例  
var args = convertToArray(arguments);
```

1. **使用`Array.from()`**

`Array.from()` 方法是ES6中新增的，它创建了一个新的数组实例，从一个类数组对象或可迭代对象。

```javascript
function convertToArray(arrayLike) {  
    return Array.from(arrayLike);  
}  
  
// 使用示例  
var nodeList = document.querySelectorAll('div');  
var arrayFromNodeList = convertToArray(nodeList);
```

### **2.使用扩展运算符（Spread Operator）`...`**

扩展运算符也可以用来将类数组对象或可迭代对象转换为数组。

```javascript
function convertToArray(arrayLike) {  
    return [...arrayLike];  
}  
  
// 使用示例  
var args = convertToArray(arguments);
```

请注意，虽然扩展运算符和 `Array.from()` 方法在大多数情况下都可以很好地工作，但它们可能无法正确处理那些没有实现迭代器协议（iterator protocol）的类数组对象。对于这些对象，`Array.prototype.slice.call()` 可能是更可靠的选择。但是，在现代的JavaScript开发中，推荐使用 `Array.from()` 或扩展运算符，因为它们更简洁且易于阅读。

## 图片懒加载怎么实现的（js实现，设置display：none图片会加载吗？（会加载））

图片懒加载（Lazy Loading）是一种优化网页性能的技术，它允许浏览器在滚动页面到特定区域时才加载图片，而不是一次性加载页面上的所有图片。这对于有大量图片或图片较大的网页特别有用，因为它可以减少初始加载时间，并节省带宽。

以下是一个基本的JavaScript实现图片懒加载的示例：

**HTML**：首先，你需要在图片元素上添加一个特定的类名或数据属性，以便JavaScript可以选择它们。

```html
<img class="lazy" data-src="image1.jpg" alt="Image 1">  
<img class="lazy" data-src="image2.jpg" alt="Image 2">  
<!-- 更多图片... -->
```

注意，我们使用了`data-src`而不是`src`来存储图片的URL。这是因为当`src`属性被设置时，浏览器会立即尝试加载图片。

**CSS**：对于懒加载的图片，你可以使用`display: none;`或`opacity: 0;`等样式来隐藏它们，但这不会影响图片的加载。重要的是要确保图片的`src`属性没有设置，或者设置为一个占位符。

```css
.lazy {  
    display: none; /* 或者 opacity: 0; */  
    /* 其他样式... */  
}  
  
.lazy.loaded {  
    display: block; /* 如果之前设置了display: none; */  
    opacity: 1; /* 如果之前设置了opacity: 0; */  
    /* 其他样式... */  
}
```

**JavaScript**：在JavaScript中，你可以使用`Intersection Observer API`（如果浏览器支持）或滚动事件监听器来实现懒加载。

使用`Intersection Observer API`的示例：

```javascript
// 选择所有需要懒加载的图片  
const lazyImages = [].slice.call(document.querySelectorAll("img.lazy"));  
//类数组转化为数组等价于Array.prototype.slice.call(document.querySelectorAll("img.lazy"))
if ("IntersectionObserver" in window) {  
    let lazyImageObserver = new IntersectionObserver(function(entries, observer) {  
        entries.forEach(function(entry) {  
            if (entry.isIntersecting) {  
                // 当图片进入视口时，加载图片  
                let lazyImage = entry.target;  
                lazyImage.src = lazyImage.dataset.src;  
                lazyImage.classList.add("loaded");  
                lazyImageObserver.unobserve(lazyImage);  
            }  
        });  
    });  
  
    lazyImages.forEach(function(lazyImage) {  
        lazyImageObserver.observe(lazyImage);  
    });  
}
```

在这个示例中，我们首先选择了所有带有`lazy`类的图片。然后，我们创建了一个新的`IntersectionObserver`实例，并传递了一个回调函数来处理与视口交叉的图片。当图片进入视口时，我们将`data-src`的值设置为`src`，并将图片标记为已加载。最后，我们停止观察已加载的图片。

请注意，`Intersection Observer API`在旧版浏览器中可能不受支持。在这种情况下，你可以使用滚动事件监听器和`getBoundingClientRect()`方法来实现类似的功能。但是，这种方法可能会更加复杂，并且可能导致性能问题。

## 深拷贝与浅拷贝

**浅拷贝**指的是只复制对象或数组本身，而不复制它们内部引用的其他对象或数组。也就是说，浅拷贝会创建一个新的对象或数组，并将原始对象或数组中的元素复制到新的对象或数组中，但是这些元素仍然是原始对象或数组中元素的引用。

**深拷贝（Deep Copy）**是创建一个新的对象，该对象与原始对象完全独立，在内存中占据不同的位置。深拷贝会复制原始对象的所有属性和嵌套对象，并且对其中一个对象的修改不会影响到另一个对象。

#### 浅拷贝

##### 1.Object.create(obj)

```javascript
let obj1 = {
 name: '小明'
}
let obj2 = Object.create(obj1)
obj1.name = '小红'

console.log(obj1.name); // 输出 '小红'

```

##### 2.Object.assign({} , obj)

Object.assign({} ，obj) 方法会将所有可枚举的自有属性从一个或多个源对象复制到目标对象，并返回目标对象。该方法执行的是浅拷贝，因此如果源对象的属性值是基本类型数据，那么它们会被复制到目标对象中，这种数据类似深拷贝，改动目标对象不会印象原对象，如果是引用类型数据，则仅复制它们的引用，这种数据改动目标对象也会修改原对象中的值。

```javascript
    let obj = {
    name: '小明',
    like: {
        n: 'coding'
    }
}
let obj2 = Object.assign({}, obj );
obj.name = '小红' ;
obj.like.n = 'running' ;
console.log(obj); 
/**结果为：
*{
*name:'小明',
*like:{
*	n:'running'，
*}
*}*/
}
```

##### 3.[ ].concat(arr)

类似Object.assign({},obj)只有引用类型数据的修改才会同时影响

```javascript
let arr = [1,2,3,{n:10}]
let newArr = [].concat(arr)
arr.push(4)
arr[3].n = 100
arr[2]=1000
console.log(newArr);
//[1,2,3,{n:100}]
```

##### 4.数组解构

同上

```javascript
    let arr = [1,2,3,{n:10}]
    arr.push(4)
    let newArr = [...arr]
    arr[3].n = 100
    arr[2]=1000
    console.log(newArr);
	//[1,2,3,{n:100}]
```

#### 深拷贝

##### 方法一:手写deepCopy

```javascript
    let obj = {
    name: '李总',
    age: 18,
    a: {
        n: 1
    },
    b: undefined, 
    c: null,
    d: function() {},
    e: Symbol('hello'),
    f: {
        n: 100
    }
}
function deepCopy(obj) {
    let objCopy = {} // 创建一个空对象，用于存放拷贝后的对象
    for (let key in obj) { // 遍历对象的所有属性
    if (obj.hasOwnProperty(key)) { // 使用hasOwnProperty方法确保只拷贝对象自身的属性
     if (obj[key] instanceof Object) { // 判断属性值是否为引用类型
       objCopy[key] = deepCopy(obj[key]);        
     } else {
       objCopy[key] = obj[key] // 如果是原始类型，则直接赋值
     }
    }
 }
    return objCopy
}
let obj2 = deepCopy(obj);
console.log(obj2);

```

##### 方法二：JSON.parse(JSON.stringify(obj))

```javascript
    let obj = {
    name: '李总',
    age: 18,
    a: {
        n: 1
    },
    b: undefined, 
    c: null,
    d: function() {},
    e: Symbol('hello'),
    f: {
        n: 100
    }
}
console.log(obj);
console.log(JSON.stringify(obj));// 把对象变成字符串
console.log(JSON.parse(str)); // 把字符串变成对象
newObj=JSON.parse(JSON.stringify(obj))//深拷贝
```

## 