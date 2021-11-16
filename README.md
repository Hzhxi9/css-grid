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

3. 网格线的名称

grid-template-columns、grid-template-rows 属性里面可以使用方括号, 指定每一根网格线的名字, 方便以后引用

网格布局允许同一根线有多个名字, 比如 `[fifth-line row-5]`

```css
.container {
  /*
   * 指定网格布局为 3 * 3, 因此有 4 根 垂直网格线 和 4 根 水平网格线
   * 方括号里面依次是这八根线的名字
   */
  grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
  grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```

4. 布局实例

grid-template-columns 属性对于网页布局非常有用。

- 两栏式布局

```css
.container {
  display: grid;
  /* 左边栏设为70%，右边栏设为30%。 */
  grid-template-columns: 70% 30%;
}
```

- 十二网格布局

```css
.container {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
}
```

5. grid-row-gap、grid-columns、grid-gap 属性

- grid-row-gap: 设置行与行的间隔(行间距)
- grid-column-gap: 设置列与列的间隔(列间距)
- grid-gap: grid-column-gap 和 grid-row-gap 的合并简写形式

  - `grid-gap: <grid-row-gap> <grid-column-gap>`
  - 省略了第二个值, 浏览器认为第二个值等于第一个值

```css
.container {
  grid-row-gap: 20px;
  grid-column-gap: 20px;
}

.container {
  grid-gap: 20px 20px;
}
```

> 根据最新标准，上面三个属性名的 grid-前缀已经删除，grid-column-gap 和 grid-row-gap 写成 column-gap 和 row-gap，grid-gap 写成 gap。

6. grid-template-areas

网格布局允许指定区域, 一个区域由单个或多个单元格组成

grid-template-areas 用于定义区域

```css
.container {
  display: grid;
  grid-template-row: 100px 100px 100px;
  grid-template-columns: 100px 100px 100px;
  /* 先划分为九个单元格, 然后定义为a 到 i 九个区域, 分别对应这九个单元格 */
  grid-template-areas:
    'a b c'
    'd e f'
    'd e f';
  /* 多个单元格合并成一个区域 */
  grid-template-areas:
    'a a a'
    'b b b'
    'c c c';
}
```

布局实例

```css
.container {
  /* 顶部是页眉区域header，底部是页脚区域footer，中间部分则为main和sidebar */
  grid-template-areas:
    'header header header'
    'main main sidebar'
    'footer footer footer';
  /* 如果某些区域不需要利用，则使用"点"（.）表示。 */
  /* 中间一列为点，表示没有用到该单元格，或者该单元格不属于任何区域。 */
  grid-template-areas:
    'a . c'
    'd . f'
    'g . i';
}
```

> 区域的命名会影响到网格线。每个区域的起始网格线，会自动命名为区域名-start，终止网格线自动命名为区域名-end。
> 比如，区域名为 header，则起始位置的水平网格线和垂直网格线叫做 header-start，终止位置的水平网格线和垂直网格线叫做 header-end。

7. grid-auto-flow

划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格

默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序

这个顺序由 grid-auto-flow 属性决定，默认值是 row，即"先行后列"。也可以将它设成 column，变成"先列后行"。

```css
.container {
  grid-auto-flow: column;
}
```

grid-auto-flow 还可以设成 `row dense` 和 `column dense`。

这两个值主要用于某些项目指定位置以后, 剩下的项目怎么自动放置

8. justify-items、 align-items、 place-items

- justify-items: 设置单元格内容的水平位置(左中右)
- align-items: 设置单元格内容的垂直位置(上中下)
- place-items: align-items and justify-items 的合并简写形式

  - `place-items: <align-items> <justify-items>`
  - 如果省略第二个值，则浏览器认为与第一个值相等

> 两个属性的值完全相同
> start: 对齐单元格的起始边缘
> end: 对齐单元格的起始边缘
> center: 单元格内部居中
> stretch: 拉伸，占满单元格的整个宽度（默认值）

9. justify-content、align-content、place-content 属性

- justify-content: 整个内容区域在容器里面的水平位置(左中右)
- align-content: 整个内容区域的垂直位置(上中下)
- place-content: align-content 属性和 justify-content 属性的合并简写形式。

  - `place-items: <align-items> <justify-items>`
  - 如果省略第二个值，则浏览器认为与第一个值相等

> 两个属性的值完全相同
> start: 对齐容器的起始边框
> end: 对齐容器的结束边框
> center: 容器内部居中
> stretch: 项目大小没有指定时，拉伸占据整个网格容器。
> space-around: 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
> space-between: 项目与项目的间隔相等，项目与容器边框之间没有间隔。
> space-evenly: 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

10. grid-auto-columns、grid-auto-rows

有时候，一些项目的指定位置，在现有网格的外部。比如网格只有 3 列，但是某一个项目指定在第 5 行。这时，浏览器会自动生成多余的网格，以便放置项目。

grid-auto-columns 和 grid-auto-rows 用来设置浏览器器自动创建的多余网格的列宽和行高

它们的写法与 grid-template-columns 和 grid-template-rows 完全相同。

如果不指定这两个属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高。

11. grid-template、grid 属性

- grid-template: grid-template-columns、grid-template-rows 和 grid-template-areas 这三个属性的合并简写形式。
- grid: grid-template-rows、grid-template-columns、grid-template-areas、 grid-auto-rows、grid-auto-columns、grid-auto-flow 这六个属性的合并简写形式。

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

4. minmax(): 产生一个长度范围, 表示长度就在这个范围之中

   - @param 最小值
   - @param 最大值

```css
.container {
  /* minmax(100px 1fr): 表示列宽不小于100px，不大于1fr。 */
  grid-template-columns: 1fr 1fr minmax(100px 1fr);
}
```

5. auto 关键字: 由浏览器自己决定长度

```css
.container {
  /*
     * 第二列的宽度，基本上等于该列单元格的最大宽度，
     * 除非单元格内容设置了min-width，且这个值大于最大宽度。
     **/
  gird-template-columns: 100px auto 80px;
}
```
