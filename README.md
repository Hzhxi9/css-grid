一、 概述

它将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。

Grid 布局则是将容器划分成"行"和"列"，产生单元格，然后指定"项目所在"的单元格，可以看作是二维布局

二、 基本概念

1. 容器和项目

- container(容器): 采用网格布局的区域
- item(项目): 容器内部采用网格定位的子元素

```html
<div>
  <div><p>1</p></div>
  <div><p>2</p></div>
  <div><p>3</p></div>
</div>
```

最外层 div 元素就是容器, 内层的三个 div 元素就是项目

> 注意: 项目只能容器的顶层子元素, 不包含项目的子元素, 比如上面的代码中 p 元素就不是项目, grid 布局只对项目生效

2. 行、列、单元格、网格线

- row(行): 容器里面的水平区域称为行
- column(列): 容器里面的垂直区域称为列
- cell(单元格): 行和列的交叉区域

  - 正常情况下, n 行 和 m 列会产生 n \* m 个单元格。 比如 3 行 3 列会产生 9 个单元格

- grid line(网格线): 划分网格的线

  - 水平网格线划分出行, 垂直网格线划分出列
  - 正常情况下, n 行有 n + 1 根水平网格线, m 列有 m + 1 根垂直网格线, 比如三行有四根水平网格线

三、 容器属性

1. display 属性

display: grid 指定一个容器采用网格布局。

```css
div {
  display: grid;
}
```

默认情况下, 容器元素都是块级元素, 但也可以设为 行内元素

```css
div {
  display: inline-grid;
}
```

> 注意，设为网格布局以后，容器子元素（项目）的 float、display: inline-block、display: table-cell、vertical-align 和 column-\*等设置都将失效。

2. grid-template-columns、 grid-template-rows 属性

- grid-template-columns: 定义每一列的列宽
- grid-template-rows: 定义每一行的行高

```css
.container {
  display: grid;
  grid-template-columns: 100px 100px 100px;
  grid-template-rows: 100px 100px 100px;
}
```

代码指定了一个三行三列的网格, 行高和列高都是 100px

也可以使用百分比

```css
.container {
  display: grid;
  width: 300px;
  height: 300px;
  grid-template-columns: 33.33% 33.33% 33.33%;
  grid-template-rows: 33.33% 33.33% 33.33%;
}
```

四、 css grid 函数 以及 关键字

1. repeat(): 简化重复的值

   - @param 重复的次数
   - @param 所要重复的值

```css
/* 重复次数 */
.container {
  display: grid;
  grid-template-rows: repeat(3, 33.33%);
  grid-template-columns: repeat(3, 33.33%);
}
/* 重复模式 */
.container {
  /* 第一列和第四列的宽度为100px，第二列和第五列为20px，第三列和第六列为80px。 */
  grid-template-columns: repeat(2, 100px 20px 80px;);
}
```

2. auto-fill 关键字: 自动填充

有时，单元格的大小是固定的，但是容器的大小不确定。如果希望每一行（或每一列）容纳尽可能多的单元格

这时候可以使用 auto-fill 关键字

```css
.container {
  /* 每列宽度为 20%, 然后自动填充, 直到容器不能放置更多的列 */
  grid-template-columns: repeat(auto-fill, 20%);
}
```

3. fr 关键字: 比例关系

为了方便表示比例关系，网格布局提供了 fr 关键字（fraction 的缩写，意为"片段"）。

如果两列的宽度分别为 1fr、2fr, 就表示后者是前者的两倍

```css
.container {
  display: grid;
  /* 表示两个相同的宽度的列 */
  grid-template-columns: 1fr 1fr;
}
```

fr 可以与绝对长度的单位结合使用

```css
.container {
  /* 第一列的宽度为150像素，第二列的宽度是第三列的一半。 */
  grid-template-columns: 150px 1fr 2fr;
}
```
