## 实现水平垂直居中

### 1.“子绝父相”

父盒子开启相对定位，子盒子开启绝对定位top50%,left50%给子盒子添加的margin-top、margin-left 分别等于自身的高度、宽度的负的一半

```css
.father{
	position: relative;
    height: 300px;
    width: 400px;
    background-color: aqua;
}
.children{
	position: absolute;
	background-color: gray;
	height: 100px;
	width: 300px;
	top: 50%;
	left: 50%;
	margin-top: -50px;
	margin-left: -150px;
}
```

若不能提前知道子盒子的长和高，可以用位偏移transform: translate(-50%,-50%)

```css
.father{
    position: relative;
    height: 300px;
    width: 400px;
    background-color: aqua;
}
.children{
    position: absolute;
    background-color: gray;
    height: 100px;
    width: 300px;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
}
```

或者将子盒子的top、left、bottom、right都设置为0，然后使用margin：auto也能水平垂直居中

```css
.father{
	position: relative;
	height: 300px;
	width: 400px;
	background-color: aqua;
}
.children{
	position: absolute;
	background-color: gray;
	height: 100px;
	width: 300px;
	top: 0;
	left: 0;
	bottom: 0;
	right: 0;
	margin: auto;
}
```



### 2.使用flex布局

将父盒子的display改为flex，justify-content和align-items都改为center，子元素就会水平垂直居中，justify-content是水平居中，align-items是垂直居中

```css
.father{
            display: flex;
            justify-content: center;
            align-items: center;
        }
```

### 3.使用grid布局

```css
.father{
            display: grid;
            justify-content: center;
            align-items: center;
        }
```

## CSS 选择器的优先级

1. !important优先级最高
2. 标签内样式：即在 HTML 标签内部使用 style 属性设置的样式，优先级第二高。
3. ID选择器：以 # 符号开头，指定某个元素的唯一标识符，比如 #header，优先级第三高。
4. 类选择器、属性选择器和伪类选择器：包括 .class、[attr]、:hover 等，优先级第四高。
5. 元素选择器和伪元素选择器：包括 div、span、:before 等，优先级最低。

在比较优先级时，遵循“从左到右，从高到低”的原则，也就是选择器中每增加一项就会降低一级别的优先级。如果两个选择器的优先级相同，则后面的选择器优先级更高。



## 有哪些清除浮动的技术，都适用哪些情况？

1. 使用`clear`属性： 在浮动元素后添加一个空元素，然后使用CSS的`clear`属性来清除浮动。适用于简单布局和较早的浏览器版本。

   ```css
   <div style="float: left;">...</div>
   <div style="clear: both;"></div>
   ```

2. 父元素使用`overflow`属性： 为父元素添加`overflow: auto`或`overflow: hidden`属性。此方法可以使父元素自动计算其高度，包括浮动元素。适用于不需要显示滚动条的布局。

   ```css
   .container {
     overflow: auto;
   }
   ```

3. 使用伪元素`::after`： 为父元素添加`::after`伪元素，并设置`clear: both`。这种方法不需要额外的HTML元素。适用于现代浏览器和简洁的HTML结构。

   ```css
   .container::after {
     content: "";
     display: table;
     clear: both;
   }
   ```

4. 使用Flexbox布局： 将父元素的`display`属性设置为`flex`。这会使所有子元素成为弹性项，并且不再需要清除浮动。适用于现代浏览器和需要使用弹性布局的场景。

   ```css
   .container {
     display: flex;
   }
   ```

5. 使用Grid布局： 将父元素的`display`属性设置为`grid`。这会使所有子元素成为网格项，并且不再需要清除浮动。适用于现代浏览器和需要使用网格布局的场景。

   ```css
   .container {
     display: grid;
   }
   ```

在实际项目中，选择哪种清除浮动的技术取决于项目的具体需求、浏览器兼容性和布局类型。现代项目通常更倾向于使用Flexbox或Grid布局来解决浮动问题。

## 脱离文档流的操作

半脱离文档流（block元素会无视，但inline元素会受影响）：浮动（float）、相对定位（relative）

脱离文档流（对文档流中的元素不会再有任何影响）：绝对定位（absolute）、固定定位（fixed）、display：none

## css怎么隐藏元素，这些方法的区别

**display:none**

该元素及其所有子元素都不会在页面上显示，并且不会占据页面空间。

元素被完全移除，元素不会被渲染

**visibility:hidden**

元素会变为不可见，但它仍然会占据页面上的空间

元素的布局和渲染仍然会进行，只是元素本身不可见

元素上绑定的事件也无法触发

**opacity:0**

元素会完全透明，但仍然占据页面上的空间

元素的子元素也会变得透明

元素上绑定的事件仍然会触发

display:none会触发重排重绘，visibility:hidden和opacity:0只会触发重绘

transition对display和visibility是无效的，但是对opacity有效

## 请阐述块格式化上下文（Block Formatting Context）、工作原理以及形成条件？

块格式化上下文（Block Formatting Context，BFC）是一个独立的渲染区域，在这个区域内，元素的布局和外部元素互不影响。BFC是 Web 页面布局中的一种重要机制，主要用于控制块级元素的布局及其内部元素的排列方式。

BFC的工作原理：

1. 内部的块级盒子会在垂直方向一个接一个放置。
2. 块级盒子的垂直间距（margin）会发生折叠。相邻的块级盒子的上下外边距会取最大值，而非相加。
3. BFC的区域不会与浮动盒子重叠。在计算布局时，BFC会考虑浮动元素的占用空间，从而避免与浮动元素重叠。
4. 计算BFC的高度时，浮动元素也参与计算。
5. BFC是一个独立的容器，外部元素对其内部元素布局没有影响；同样，BFC内部元素的布局也不会影响外部元素。

形成BFC的条件：

要创建一个BFC，需要满足以下条件之一：

1. 根元素（`<html>`）。
2. 浮动元素（`float`属性为`left`或`right`）。
3. 绝对定位元素（`position`属性为`absolute`或`fixed`）。
4. 内联块（`display`属性为`inline-block`）。
5. 表格单元格（`display`属性为`table-cell`）。
6. 表格标题（`display`属性为`table-caption`）。
7. 匿名表格单元格（`display`属性为`table`、`table-row`、`table-row-group`、`table-header-group`、`table-footer-group`、`table-column`、`table-column-group`）。
8. 元素的`overflow`属性值不为`visible`（例如，`auto`、`scroll`、`hidden`）。
9. 弹性盒子（`display`属性为`flex`或`inline-flex`）。
10. 网格容器（`display`属性为`grid`或`inline-grid`）。
11. 多列容器（`column-count`或`column-width`属性不为`auto`）。
12. `contain`属性值为`layout`、`paint`或`strict`。

通过满足以上条件之一，可以创建BFC，实现独立渲染区域。在实际应用中，BFC有助于解决外边距折叠、浮动元素引起的布局问题等。