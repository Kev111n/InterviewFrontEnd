## 1.请编写一个名为 executeTasks 的函数。这个函数需要接受一个异步任务数组作为参数。每个任务是一个会返回 Promise 的函数。你的目标是同时启动这些异步任务，并且在每个任务完成时，立即按照它们在数组中的原始顺序显示它们的结果。

```javascript
function executeTasks(tasks) {  
    // 创建一个与任务数量相同的数组来保存结果  
    const results = new Array(tasks.length);  
  
    // 创建一个计数器来跟踪已完成的 Promise 数量  
    let completedCount = 0;  
  
    // 遍历任务数组，为每个任务创建一个 Promise 并添加到 Promise 数组中  
    const promises = tasks.map((task, index) => {  
        return task().then(result => {  
            // 当 Promise 完成时，将结果放入结果数组的正确位置  
            results[index] = result;  
            // 打印结果（或进行其他处理）  
            console.log(`Task ${index + 1} completed with result:`, result);  
            // 增加已完成的 Promise 计数  
            completedCount++;  
  
            // 如果所有 Promise 都已完成，我们可以选择做一些清理工作或返回结果数组  
            if (completedCount === tasks.length) {  
                // 例如，我们可以选择在这里返回结果数组  
                // return results; // 如果需要，可以取消注释这行代码  
            }  
        });  
    });  
  
    // 启动所有 Promise  
    // 注意：我们不使用 Promise.all，因为我们希望在每个 Promise 完成时立即处理结果  
    // 但如果你需要等待所有 Promise 完成后再进行某些操作，你可以使用 Promise.all(promises).then(...)  
}  
  
// 示例用法：  
const asyncTask1 = () => new Promise(resolve => setTimeout(() => resolve('Task 1 result'), 1000));  
const asyncTask2 = () => new Promise(resolve => setTimeout(() => resolve('Task 2 result'), 500));  
const asyncTask3 = () => new Promise(resolve => setTimeout(() => resolve('Task 3 result'), 750));  
  
executeTasks([asyncTask1, asyncTask2, asyncTask3]);
```

## 2.编写一个 JavaScript 函数，该函数能够将给定的虚拟 DOM 对象（JSON格式）转换为真实的 DOM 结构，并将其插入到页面中。

```javascript
function createElementFromVirtualDOM(virtualNode) {  
    // 创建一个真实的 DOM 元素  
    const element = document.createElement(virtualNode.type);  
  
    // 遍历属性并添加到元素上  
    if (virtualNode.props) {  
        for (let propName in virtualNode.props) {  
            if (propName === 'children') continue; // 'children' 是子节点的特殊属性，稍后处理  
            if (propName.startsWith('on')) {  
                // 假设属性以 'on' 开头的是事件监听器  
                element.addEventListener(propName.toLowerCase().substring(2), virtualNode.props[propName]);  
            } else {  
                // 其他属性直接设置  
                element.setAttribute(propName, virtualNode.props[propName]);  
            }  
        }  
    }  
  
    // 递归处理子节点  
    if (virtualNode.children) {  
        virtualNode.children.forEach(child => {  
            // 假设 children 是数组，每个元素都是另一个虚拟 DOM 节点  
            const realChild = createElementFromVirtualDOM(child);  
            // 将子元素添加到当前元素中  
            element.appendChild(realChild);  
        });  
    }  
  
    // 返回创建的 DOM 元素  
    return element;  
}  
  
// 使用示例  
const virtualDOM = {  
    type: 'div',  
    props: {  
        id: 'myDiv',  
        className: 'myClass',  
        onClick: function() { console.log('Div clicked!'); }  
    },  
    children: [  
        {  
            type: 'p',  
            props: {},  
            children: ['Hello, World!']  
        },  
        {  
            type: 'button',  
            props: {  
                onClick: function() { console.log('Button clicked!'); }  
            },  
            children: ['Click Me']  
        }  
    ]  
};  
  
// 将虚拟 DOM 转换为真实 DOM 并插入到 body 中  
const realDOM = createElementFromVirtualDOM(virtualDOM);  
document.body.appendChild(realDOM);
```

