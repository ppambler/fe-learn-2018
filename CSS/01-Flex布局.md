---
typora-copy-images-to: img\01
---

# Flex布局

## ★资料

**➹：**[30 分钟学会 Flex 布局 - 知乎](https://zhuanlan.zhihu.com/p/25303493)

## ★草稿

### ◇一些基本概念

#### main轴和cross轴

flex容器默认存在2条轴——水平main轴和垂直cross轴，在这里需要注意的是「main轴也有垂直的那一天，同理，cross轴也有水平的那一天！」。总之没有绝对说水平的一定是main轴，毕竟或许它是cross轴也说不定呢！

#### flex item

在容器中的每个单元块被称之为 flex item，每个item占据的主轴空间为 (main size)， 占据的交叉轴的空间为 (cross size)。

如果flex容器默认是水平main轴和垂直cross轴，那么每个item的宽就是main size了，同理它们的高就是cross size了。总之这取决于你主轴的方向！

### ◇贯穿全文的图

![img](img/01/v2-54a0fc96ef4f455aefb8ee4bc133291b_hd.jpg)

> 左上都是轴的起点吗？
>
> 还是不分方向的？即这头是起点，那对头就是终点了？

### ◇Flex 容器

我要flex布局了，你们这些元素通通给我听好了！我可以随意指定你们为flex布局的！如果你们天生是块状的，那么我就把你们的display属性值弄为flex，如果是行内的，那就inline-flex……

对了，被我指定了flex布局家伙们，你们此刻有了一个新的名称，叫flex容器！

还有就是你们之前的儿子们存在的 **float、clear、vertical-align 这些属性**，将会GG，即失效了！

> 话说，孙子有受到影响吗？不是说好了，祸不及三代吗？

身为flex容器的你们，将会具有以往没有的6种属性……

那么这6种属性是什么呢？它们分别是，**✎：**

1. flex-direction
2. flex-wrap
3. flex-flow
4. justify-content
5. align-items
6. align-content

#### 解析6种属性

##### flex-direction

决定主轴的方向(即item的排列方向)

既然如此，那它能取哪些值呢？

```css
.container {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

额……能告诉我，哪个是默认值吗？

row就是默认值，表示主轴为水平方向，起点在**左端**。

> 我的天，轴的起点不是随意而定的啊！如果值是row-reverse，这是不是说明起点在右端呢？

没错，确实如此哈！

看看使用后的效果，**✎：**

![img](img/01/v2-ae8828b8b022dc6f1b28d5b4f7082e91_hd.jpg)

那么如何才能变成321呢？

这个很简单，你把值改为 row-reverse即可！

![img](img/01/v2-215c8626ac95e97834eddb552cfa148a_hd.jpg)

总之，row-reverse这个值会告诉你此刻的这个容器中主轴为水平方向，而起点在右端……

> 我们写的HTML代码，写的是一个元素跟着一个元素的，而此刻因为它们的爸爸是flex容器，再加上flex-direction属性的值为row-reverse，导致元素排列颠倒了，而且呈现水平，即把item的display为block的元素弄为水平的了了！话说，你们这样做对得起float属性吗？
>
> 对了，我有个问题就是这些item有脱离文档流吗？还有这个爸爸flex容器有脱离文档流吗？
>
> 我猜应该咩有吧？总不能也要清除所谓的「浮动」吧！

同理，那么剩下的那两个常用的值也就很好理解了，**✎：**

- column：主轴为垂直方向，起点在上沿

  ![img](img/01/v2-33efe75d166a47588e0174d0830eb020_hd.jpg)

  这个结果就跟它们的爸爸不是flex容器时一个模样，不过用了flex之后，你会发现，我们能够把这些item看作是一个整体，如一串冰糖葫芦一样……

- column-reverse：主轴为垂直方向，起点在下沿

  ![img](img/01/v2-344757e0fb7eee11e75b127b8485e679_hd.jpg)

##### flex-wrap

这个属性将会决定容器内的item是否可换行

默认情况下，item都排在主轴线上，而使用 flex-wrap则可实现item的换行。

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

在这里，nowrap作为该属性的默认值，即不换行。总之，当主轴尺寸固定时，当空间不足时，项目尺寸会随之调整而并不会挤到下一行。

![img](img/01/v2-a590927ad6d83de8840d52a0cf2f0df3_hd.jpg)

  既然nowrap是不换行，那么wrap显然是当项目主轴总尺寸超出容器时，就得换行了，下图的第一行在上方哈！

![img](img/01/v2-426949b061e8179aab00cacda8168651_hd.jpg)

那么wrap-reverse呢？

它也是换行 的，只是把这些行给翻转了，即第一行由原来的上方变为下方了，**✎：**

![img](img/01/v2-91c53ebf744814e1ab60267643866439_hd.jpg)

##### flex-flow

听说，你总喜欢打包，那么这个属性就很适合你了，因为它是**flex-direction 和 flex-wrap 的简写形式**

如何个简写法呢？

```css
.container {
    flex-flow: <flex-direction> || <flex-wrap>;
}
```

> 话说，这个 `<>`和 `||`表示什么呀？

那么它的默认值呢？——row nowrap

不过这个不需要记了，因为我们一般都是喜欢拆开来写的！

##### justify-content

这个属性**定义了项目在主轴的对齐方式**

那么有哪些对齐方式呢？

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

以下测试是建立在**主轴为水平方向**时进行的，即**flex-direction: row**

先来说说它的默认值吧！，**✎：**

flex-start 左对齐

![img](img/01/v2-1bafab80044a7ab2a6198d5937172eb0_hd.jpg)

那么剩下的那4个常用的值，使用后又会有怎样的效果呢？

flex-end：右对齐

![img](img/01/v2-8b163809a4c944486a127a7c22eee7b2_hd.jpg)

center：居中

![img](img/01/v2-dea82c75d35f532d35a52d1f9c1c762b_hd.jpg)

space-between：两端对齐，项目**之间**的**间隔相等**，即剩余空间等分成间隙。

![img](img/01/v2-ea4061e0f64dd8d7a1fcb5b0ad6f96a8_hd.jpg)

space-around：每个项目**两侧**的**间隔相等**，所以项目之间的间隔比项目与边缘的间隔大一倍。

![img](img/01/v2-42a358111a221ff52768bdd55238eb0c_hd.jpg)

##### align-items

该属性**定义了项目在交叉轴上的对齐方式**

那么它有哪些值呢？

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

同justify-content一样，也是**建立在主轴为水平方向时测试，即 flex-direction: row**

同样，它也有默认值，那么它的默认值是什么呢？——stretch

表示如果项目未设置高度或者设为 auto，将占满整个容器的高度。效果如下，**✎：**

![img](img/01/v2-0cced8789b0d73edf0844aaa3a08926d_hd.jpg)

总之，具体来讲就是假设容器高度设置为 100px，而项目都没有设置高度的情况下，则项目的高度也为 100px。

---

flex-start：交叉轴的起点对齐

![img](img/01/v2-26d9e85039beedd78e412459bd436e8a_hd.jpg)

假设容器高度设置为 100px，而项目分别为 20px, 40px, 60px, 80px, 100px, 则如上图显示。

> 讲真，这个对齐是这什么啊？
>
> 我试着用wps创建一个word文档测试了一下，按照我的理解就是，把每一行文字都看作是一个整体，然后从整个段落的视角来看对齐方式，而对齐方式有哪几种呢？
>
> ![1543249341131](img/01/1543249341131.png)
>
> 图中的那些横杠就是一行文本了，而多行没有回车的文本就形成了一个段落，也就是你得从一个段落视角去看对齐！关于分散对齐，指的是为了让段落两端同时对齐，需要增加字符间距哈！而两端对齐则是将文字左右两端同时进行对齐，并根据需要增加字间距！
>
> **➹：**[对齐或两端对齐文本 - Word for Mac](https://support.office.com/zh-cn/article/%E5%AF%B9%E9%BD%90%E6%88%96%E4%B8%A4%E7%AB%AF%E5%AF%B9%E9%BD%90%E6%96%87%E6%9C%AC-b9096ed4-7323-4ff3-921a-1ba7ba31faf1)

通过对「对齐方式」的认识，我觉得可以把所谓的「交叉轴的起点对齐」，看作是你站在水平主轴的左起点，方向是你的正面朝向的右终点，然后按左对齐即可！直观一点就是，**✎：**

![1543250582269](img/01/1543250582269.png)

为此，我更进一步把一个个item看作是一行行文字，把整个flex容器看作是一个段落！

---

好了，那我们接下来看看flex-end又是个什么情况，其实根据flex-start并不难猜出，显然是右对齐哈。而正规的一点说法就是 **交叉轴的终点对齐**

![img](img/01/v2-8b65ee47605a48ad2947b9ef4e4b01b3_hd.jpg)

---

center：交叉轴的中点对齐

显然可以理解为文本居中对齐，即item居中对齐！

![img](img/01/v2-7bb9d8385273d8ad469605480f40f8f2_hd.jpg)

---

baseline: 项目的第一行文字的基线对齐

![img](img/01/v2-abf7ac4776302ad078986f7cd0dddaee_hd.jpg)

以文字的底部为主，仔细看图可以理解。

> 讲真，实在理解不了，按照之前的理解就是「文字的baseline对齐，通过行高不一样才能发现区别，不过一般来说行高都是一样的」
>
> **➹：**[基线 - Wikiwand](https://www.wikiwand.com/zh-hans/%E5%9F%BA%E7%B7%9A)
>
> 不过当我看到这个幅图后，就开始有点理解了，**✎：**
>
> ![1543252392413](img/01/1543252392413.png)
>
> 也就是说，每个item的第一行文字的基线，必须和其它item的基线连成一条直线咯！而不是所谓的折线！反正这种情况可以把item当作是可以在海中上下方向上升和下沉的潜艇！即把此时的容器当作是大海，然后海底中有4艘大小不一的潜艇……
>
> 突然发觉上面的，**✎：**
>
> - stretch：如果item未设置高度或者设为 auto，将占满整个容器的高度
>
> - flex-start：交叉轴的起点对齐
> - center：交叉轴的中点对齐
> - flex-end：交叉轴的终点对齐
>
> 都可以看作是潜艇哈！
>
> - stretch：哪艘潜艇最高，那么潜艇都会被拉伸成一样高
> - flex-start：4艘大小不一的潜艇浮出水面
> - center：4艘大小不一的潜艇在海水的中央位置搞事情
> - flex-end：4艘大小不一的潜艇在海底摸鱼……

---

总之**baseline用得最少，最多的是center和flex-start**

##### align-content

这是最后一个属性了，那么这个属性的作用是什么呢？

它**定义了多根轴线的对齐方式，如果项目只有一根轴线，那么该属性将不起作用**

> 难道一行item就是一根轴线，如果没有换行的话，那么在容器中那就只有一根轴线了啊！

我们还是先来看看，它能取的值有哪些，**✎：**

```css
.container {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

总之，你可以这样理解这个属性，**✎：**

- 当你 flex-wrap 设置为 nowrap 的时候，容器仅存在一根轴线，因为项目不会换行，就不会产生多条轴线。

- 当你 flex-wrap 设置为 wrap 的时候，容器可能会出现多条轴线，这时候你就需要去设置多条轴线之间的对齐方式了。

以下是建立在主轴为水平方向时测试，即 flex-direction: row, flex-wrap: wrap

---

默认值为 stretch，**✎：**

![img](img/01/v2-c284017b4b8b731bc213cd1caab514e5_hd.jpg)

从图可以看出有三条轴线(因为容器宽度有限)，当值为 stretch 时，三条轴线会平分容器的垂直方向上的空间。

值得注意的是，虽然在每条轴线上项目的默认值也为 stretch，但是由于我每个项目我都设置了高度，所以它并没有撑开整个容器。如果项目不设置高度的话就会变成下面这样：

![img](img/01/v2-2d5feceece695fb84fd650fc49164bd6_hd.jpg)

在前面我们也有提到(align-items)，总之，在这里重点还是理解三条轴线会平分垂直轴上的空间

---

flex-start：轴线全部在交叉轴上的起点对齐

![img](img/01/v2-61d92d7dc68e3d7d415a16830050fd11_hd.jpg)

> 按照我的理解就是，你得把1234看作是个整体，即一根轴线，或许可以说是轴线上升了！

---

flex-end：轴线全部在交叉轴上的终点对齐

![img](img/01/v2-0a0a7f10c50596aade787ae11b7b0a75_hd.jpg)

> 这个可以说是轴线下沉了！

---

center：轴线全部在交叉轴上的中间对齐

![img](img/01/v2-dcf53fce8dbcde7da9c677dd1a033860_hd.jpg)

---

space-between：轴线两端对齐，之间的间隔相等，即剩余空间等分成间隙。

![img](img/01/v2-d80940f71e1e08d45d3d6df4c5401d0d_hd.jpg)

> 之前说过，两端对齐是将文字左右两端同时进行对齐，并根据需要增加字间距！
>
> 在这里我似乎可以把1234这整个部分当作是一个字了

---

space-around：每个轴线两侧的间隔相等，所以轴线之间的间隔比轴线与边缘的间隔大一倍。

![img](img/01/v2-7c4d5c01f3851a3cec7f8487c6edb21d_hd.jpg)

---

到这里关于容器上的所有属性都讲完了，接下来就来讲讲关于在 flex item 上的属性。

#### 小结

1. flex-direction决定了主轴方向，以及轴的起点。相应地也就决定了侧轴方向，不过侧轴的起点是怎样的呢？

   比如主轴为水平，而起点是从左到右，那么侧轴是从上到下吗？

2. 我还是喜欢叫cross轴，或者是侧轴！而不是交叉轴！

3. 一行元素一根轴！换行之后有侧轴！

### ◇Flex item

有哪些属性可运用在item上呢？

同样，也有6种属性，分别是，**✎：**

1. order
2. flex-basis
3. flex-grow
4. flex-shrink
5. flex
6. align-self

#### 解析6种属性

##### order

**定义项目在容器中的排列顺序，数值越小，排列越靠前，默认值为 0**

```css
.item {
    order: <integer>;
}
```

![img](img/01/v2-d606874ac9c496b3a0e46573c85e4376_hd.jpg)

在 HTML 结构中，虽然 -2，-1 的 item 排在后面，但是由于分别设置了 order，使之能够排到最前面。

> 本来都是0，奈何重设了order的值！

##### flex-basis

**定义了在分配多余空间之前，项目占据的主轴空间，浏览器根据这个属性，计算主轴是否有多余空间**

```css
.item {
    flex-basis: <length> | auto;
}
```

默认值：auto，即项目本来的大小, 这时候 item 的宽高取决于 width 或 height 的值。

**当主轴为水平方向的时候，当设置了 flex-basis，项目的宽度设置值会失效，flex-basis 需要跟 flex-grow 和 flex-shrink 配合使用才能发挥效果。**

- 当 flex-basis 值为 0 % 时，是把该项目视为零尺寸的，故即使声明该尺寸为 140px，也并没有什么用。
- 当 flex-basis 值为 auto 时，则跟根据尺寸的设定值(假如为 100px)，则这 100px 不会纳入剩余空间。

> 按之前的理解，这个一般都不用，总之你不写的话就是原始大小！

##### flex-grow

**定义项目的放大比例**

```css
.item {
    flex-grow: <number>;
}
```

默认值为 0，即如果存在剩余空间，也不放大

![img](img/01/v2-5f7898c1f51fa7274a2c0b4a9dfd88c3_hd.jpg)

当所有的项目都以 flex-basis 的值进行排列后，仍有剩余空间，那么这时候 flex-grow 就会发挥作用了。

- 如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间。(如果有的话)

- 如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍。

当然，如果当所有项目以 flex-basis 的值排列完后发现空间不够了，且 flex-wrap：nowrap 时，此时 flex-grow 则不起作用了，这时候就需要接下来的这个属性。

##### flex-shrink

**定义了项目的缩小比例**

```css
.item {
    flex-shrink: <number>;
}
```

  默认值: 1，即如果空间不足，该项目将缩小，负值对该属性无效。

  ![img](img/01/v2-383e97971a7fc8c4f84e6a85406dbcaf_hd.jpg)

这里可以看出，虽然每个项目都设置了宽度为 50px，但是由于自身容器宽度只有 200px，这时候每个项目会被同比例进行缩小，因为默认值为 1。

同理可得：

- 如果所有项目的 flex-shrink 属性都为 1，当空间不足时，都将等比例缩小。 

- 如果一个项目的 flex-shrink 属性为 0，其他项目都为 1，则空间不足时，前者不缩小

##### flex

**flex-grow, flex-shrink 和 flex-basis的简写**

```css
.item{
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
} 
```

flex 的默认值是以上三个属性值的组合。假设以上三个属性同样取默认值，则 flex 的默认值是 0 1 auto。

有关快捷值：auto (1 1 auto) 和 none (0 0 auto)

关于 flex 取值，还有许多特殊的情况，可以按以下来进行划分：

- 当 flex 取值为一个非负数字，则该数字为 flex-grow 值，flex-shrink 取 1，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 1;}
.item {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}
```

- 当 flex 取值为 0 时，对应的三个值分别为 0 1 0%

```css
.item {flex: 0;}
.item {
    flex-grow: 0;
    flex-shrink: 1;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个长度或百分比，则视为 flex-basis 值，flex-grow 取 1，flex-shrink 取 1，有如下等同情况（注意 0% 是一个百分比而不是一个非负数字）

```css
.item-1 {flex: 0%;}
.item-1 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0%;
}

.item-2 {flex: 24px;}
.item-2 {
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 24px;
}
```

- 当 flex 取值为两个非负数字，则分别视为 flex-grow 和 flex-shrink 的值，flex-basis 取 0%，如下是等同的：

```css
.item {flex: 2 3;}
.item {
    flex-grow: 2;
    flex-shrink: 3;
    flex-basis: 0%;
}
```

- 当 flex 取值为一个非负数字和一个长度或百分比，则分别视为 flex-grow 和 flex-basis 的值，flex-shrink 取 1，如下是等同的：

```css
.item {flex: 11 32px;}
.item {
    flex-grow: 11;
    flex-shrink: 1;
    flex-basis: 32px;
}
```

建议优先使用这个属性，而不是单独写三个分离的属性。

grow 和 shrink 是一对双胞胎，grow 表示伸张因子，shrink 表示是收缩因子。

grow 在 flex 容器下的子元素的宽度和比容器和小的时候起作用。 grow 定义了子元素的尺寸增长因子，容器中除去子元素之和剩下的尺寸会按照各个子元素的 grow 值进行平分加大各个子元素上。

##### align-self

**允许单个项目有与其他项目不一样的对齐方式**

单个项目覆盖 align-items 定义的属性

默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

```css
.item {
     align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

这个跟 align-items 属性时一样的，只不过 align-self 是对单个项目生效的，而 align-items 则是对容器下的所有项目生效的。

![img](img/01/v2-2516cddfbbabaef96fd6dfab4eb71757_hd.jpg)

容器 align-items 设置为 flex-start，而第三

#### 小结

- 需要花点时间捋捋这6个属性才行！