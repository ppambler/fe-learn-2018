---
typora-copy-images-to: 01
---

# Life of a Pixel

## ★资料

**➹：**[前端代码如何通过浏览器演化为屏幕显示的像素](https://zhuanlan.zhihu.com/p/44737615)

**➹：**[Life of a Pixel 2018 - Google 幻灯片](https://docs.google.com/presentation/d/1boPxbgNrTU0ddsc144rcXayGA_WF53k96imRH8Mp34Y/edit#slide=id.p)

## ★临时笔记

1. ![1537757160598](img/01/1537757160598.png)

   这图给我的第一眼就是，内容经过2次解析，变成了一种可以被渲染引擎渲染解释的数据结构！

2. ![1537774806104](img/01/1537774806104.png)

   > The DOM serves double duty as both the internal representation of the page, and the API exposed to script for querying or modifying the rendering.
   >
   > The JavaScript engine (V8) exposes DOM web APIs as thin wrappers around the real DOM tree through a system called "bindings".

   **✎：**

   > DOM作为页面的内部表示，以及用于查询或修改渲染的脚本的API都是双重职责。
   >
   > JavaScript引擎（V8）通过一个称为“绑定”的系统将DOM web api暴露在真正的DOM树周围。

   按照我的理解就是，DOM树是Chrome浏览器的内部表现，为什么这么说呢？你可以通过JavaScript所提供的DOM API就能发现这一点！

   把HTML解析成一颗DOM树，这很好地诠释了元素父子间，兄弟间的关系！

## ★最终笔记

### ◇苏格拉底的话

> 未经审视的人生没有价值

**➹：**[为何很多人说“未经审视的人生没有价值”呢？ - 知乎](https://www.zhihu.com/question/19951844)

![1537776108042](img/01/1537776108042.png)

> The unexamined pixel is not worth rendering
>
> ​	——未经审视的像素不值得渲染

这是原话，**✎：**

> The unexamined life is not worth living

稍微谷歌了一下，发现对这句话的翻译千奇百怪……

**➹：**[苏格拉底曾说：“未经审视的人生不值得过”，如何理解这里的“审视”？ - 知乎](https://www.zhihu.com/question/270551757)

如果我此刻非要理解的话，那就是「你没有思考过自己所做的一切，随波逐流，那么你这一生并不值得过下去！」

### ◇讲什么

![1537777030566](img/01/1537777030566.png)

> This talk is about how Chrome turns web content into pixels.  The entire process is called "rendering".
>
> We'll describe what we mean by content, and what we mean by pixels, and then we'll explain the magic in between.

> 这个演讲是关于Chrome如何将网页内容变成像素的。这整个过程称之为“渲染”。
>
> 我们会描述内容和像素各自是什么意思，然后我们会解释这两者间的「魔力」。

ps：

![KOj](img/01/KOj.gif)

### ◇以Chrome的哪个版本来讲

> Chrome's architecture is constantly evolving.  This talk connects high-level concepts (which change slowly) to specific classes (which change frequently).
>
> The details are based primarily on what is currently shipping in the canary channel (M69), but a few of the biggest future refactorings are mentioned in passing

> Chrome的架构在不断发展。这篇演讲将高层次的概念(变化缓慢)与特定的阶级(变化频繁)联系起来。
>
> 这里的描述主要是基于目前在金丝雀版本（M69）的阐述，同时也会在演讲中提到一些未来最大的重构



![1537777703486](img/01/1537777703486.png)

我目前所用的Chrome版本号，**✎：**

![1537778522340](img/01/1537778522340.png)



### ◇你看到的网页内容

> "Content" is the generic term in Chromium for all of the code inside a webpage or the frontend of a web application.
>
> It's made of text, markup (surrounding the text), styles (defining how markup is rendered), and script (which can modify all of the above dynamically).
>
> There are other kinds of content, which we won't cover here.

「Content」是一个关于网页中或者web应用中前端的所有代码的这么一个通用术语。

它由**文本**、**标记**(围绕文本)、**样式**(定义如何呈现标记)和**脚本**(可以动态地修改上述所有内容)组成。

还有其它类型的内容（如图片、音频、视频、svg等等），我们不会在这里介绍。

![1537778879830](img/01/1537778879830.png)

图中的「property」让我想起了，**✎：**

> JavaScript 提供了一个内部数据结构，用来描述对象的属性，控制它的行为，比如该属性是否可写、可遍历等等。这个内部数据结构称为“属性描述对象”（attributes object）。每个属性都有自己对应的属性描述对象，保存该属性的一些元信息。

**➹：**[属性描述对象 -- JavaScript 标准参考教程（alpha）](https://javascript.ruanyifeng.com/stdlib/attributes.html)

虽然这是CSS的语法，不过可以这样理解这些属性值，如把red再封装一层。就像是这样，**✎：**

![1537781291608](img/01/1537781291608.png)

**➹：**[JavaScript 的 this 原理 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2018/06/javascript-this.html)

### ◇一张真正的网页

> A real webpage is just thousands of lines of HTML, CSS, and JavaScript delivered in plain text over the network.
>
> There's no notion of compilation or packaging as you might find on other kinds of software platforms - the webpage's source code is the input to the renderer.

一张真正的网页只是成千上万行HTML、CSS和JavaScript以纯文本的形式通过网络传递。

没有编译或打包的概念，你可能会发现在其他类型的软件平台-网页的源代码是渲染器的输入。

![1537800934663](img/01/1537800934663.png)

**➹：**[纽约时报](https://www.nytimes.com)

以一个浏览器界面去看这张网页，**✎：**

![1537802378446](img/01/1537802378446.png)

> Architecturally, the "content" namespace in the Chromium C++ codebase is responsible for everything in the red box.
>
> Contrast with tab strip, address bar, navigation buttons, menus, etc. which live outside of "content".
>
> Key to Chrome's security model: rendering happens in a sandboxed process.

在架构上，Chromium c++代码库中的以“content”为命名空间的负责红框中的所有内容，与存在于“content”之外的标签栏、地址栏、导航按钮、菜单等形成对比。

Chrome安全模型的关键在于渲染是在沙箱进程中发生的！你要知道沙箱的标签意味着安全！

![1537803074584](img/01/1537803074584.png)

没想到连滚动条都是「content」的一员，也是交由Bink来搞事情！

### ◇像素

![1537805387401](img/01/1537805387401.png)

> At the other end of the pipeline we have to get pixels onto the screen using the graphics libraries provided by the underlying operating system.
>
> On most platforms today, that's a standardized API called "OpenGL".  On Windows there's an extra translation into DirectX.  In the future, we may support newer APIs such as Vulkan.
>
> These libraries provide low-level graphics primitives like "textures" and "shaders", and let you do things like "draw a polygon at these coordinates into a buffer of virtual pixels".  But obviously they don't understand anything about the web or HTML or CSS.

在管道（流水线）的另一端，我们必须使用底层操作系统提供的图形库将像素显示在屏幕上。

在今天的大多数平台上，这是一个被称为“OpenGL”的标准化API。而在Windows上有一个额外的效果翻译器——DirectX。将来，我们可能会支持诸如Vulkan之类的新API。

这些库提供低级的作图基元，比如“纹理”和“[着色器](https://learnopengl-cn.readthedocs.io/zh/latest/01%20Getting%20started/05%20Shaders/)”，并且允许您做一些事情，比如“在这些坐标的虚拟像素缓冲区上绘制一个多边形（图中的那个等边三角形）”。但很明显，它们对web（浏览器？网络？web应用？）、HTML或CSS一无所知。（即只是单纯地由输入转化为输出）

ps：作图基元——在某些中小型计算机制图系统中，一个单项的图形操作。如：画一串点、画一直线、画一串图文。

### ◇目标

![1537870424098](img/01/1537870424098.png)

> So the goal of rendering can be stated as: turn HTML / CSS / JavaScript into the right OpenGL calls to display the pixels.
>
> But keep in mind a second goal as we describe the pipeline:  We also want the right intermediate data structures to update the rendering efficiently after it's produced, and answer queries about it from script or other parts of the system.

因此，呈现的目标可以表述为：将HTML / CSS / JavaScript转换为正确的OpenGL调用来显示像素。

但是，在描述流水线时要记住第二个目标：我们还需要合适的中间数据结构，以便在页面生成之后还高效地更新呈现，并从脚本或系统的其它部分回答关于呈现的查询。

在这儿，更新页面的途径有JavaScript、用户输入、异步加载、动画、滚动、缩放……

没想到缩放也会更新页面重新渲染！我很好奇这是个什么样的数据结构，才能做到高效地再次渲染！

### ◇流程

![1537871502306](img/01/1537871502306.png)

> We break the pipeline into multiple "lifecycle stages", generating those intermediate outputs.
>
> We'll first describe each stage of a working pipeline, then come back to the notion of efficient updating and introduce some concepts for optimization.

我们将管道分解为多个“生命周期阶段”，生成这些中间输出。

我们将首先描述工作管道的每个阶段，然后回到高效更新的概念，并引入一些优化的概念。

这里有3个阶段，头两个阶段的产物是不知名的数据结构，最后一个阶段就是最终的渲染结果了！

### ◇解析

![1537872283607](img/01/1537872283607.png)

> HTML tags impose a semantically meaningful hierarchical structure on the document.  For example, a` <div>` may contain two paragraphs, each with text.  So the first step is to parse those tags to build an object model that reflects this structure.

HTML标记在文档上强加了一个语义上有意义的层次结构。例如，一个div中可能包含两个段落，每个段落都有文本。因此，第一步是解析这些标记，以构建反映这种结构的对象模型。

也就是说你的HTML文档中的内容是有语义有层次的（即嵌套），借此可以解析HTML文档构建成一个DOM树

也就是之所以用树这种数据结构是有原因的啊！这是根据材料来决定的啊！

图中说到，Document节点总是存在的，是永远的根节点，而HTML、BODY等这些标签则是可选的！

### ◇DOM

![1537872935249](img/01/1537872935249.png)

> If you've taken computer science classes you may recognize this as a "tree".

如果你上过CS课，你可能会认为这是一棵“树”。

当你站在div这个节点去看整颗树的时候，你会发现它有爸爸、儿子、弟兄、爷爷、祖先……

![1537873416953](img/01/1537873416953.png)

> The DOM serves double duty as both the internal representation of the page, and the API exposed to script for querying or modifying the rendering.
>
> The JavaScript engine (V8) exposes DOM web APIs as thin wrappers around the real DOM tree through a system called "bindings".

DOM是个什么样的存在？既是页面的内部表示，又是暴露给脚本查询或修改呈现的API。

JavaScript引擎(V8)通过一个名为“bindings”的系统，将DOM web APIs作为围绕真正的DOM树的thin warppers（薄包装纸？）给开放了。

之前有了解过DOM接口，然后各大浏览器对DOM接口实现有点差异，这些实现产物叫做DOM web APIs，我们开发者可以通过使用DOM web APIs来查询、更新这颗在内存中的树！

这个bindings系统真有意思！不过在这其中起到关键作用的可是V8啊！

ps：有个评论提到Shadow DOM，其中提到，**✎：**

> We layout in flat tree order, instead of dom order

### ◇style

![1537875085740](img/01/1537875085740.png)

> Having built the DOM tree, the next step is to process the CSS styles.
>
> A CSS selector selects a subset of DOM elements that its property declarations should apply to.

构建了DOM树之后，下一步是处理CSS样式。

CSS选择器选择其属性声明应当应用于其DOM元素子集。

即你的选择器是P这个DOM元素，那么这个color则作用于其子文本节点，而且这是可继承的，**✎：**

![1537875436498](img/01/1537875436498.png)



![1537875624981](img/01/1537875624981.png)

> Style properties are the knobs by which web authors can influence the rendering of DOM elements.
>
> There are hundreds of style properties.

样式属性是web作者可以通过它影响DOM元素呈现的开关按钮。

而且这有数百种样式属性。如文本的粗细、元素间的间距大小、元素的边框是虚线还是实线，是红的黄的还是绿的啊、是否可以让元素倾斜旋转啊？、是否可以让元素有背景图啊！

**➹：**[transform - CSS：层叠样式表 - MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)

![1537876048131](img/01/1537876048131.png)

> Furthermore, it's not trivial to determine which elements a style rule selects.
>
> Some elements may be selected by more than one rule, with conflicting declarations for a particular style property.

此外，确定样式规则所选择的元素并不简单。

某些元素可以由多个规则选择，这样一来，特定样式属性的声明就会相互冲突了啊！

关于样式规则，**✎：**

> CSS由多组`规则`组成，每个规则由“选择器”（selector）、“属性”（property）和“值”（value）组成：
>
> 1. `选择器`（Selector）:多个选择器可以半角逗号（,）隔开。
> 2. `属性`（property）:CSS1、CSS2、CSS3规定了许多的属性，目的在控制选择器的样式。
> 3. `值`（value）:指属性接受的设置值，多个关键字时大都以空格隔开。
>
> 属性和值之间用半角冒号`：`隔开，**属性和值合称为“特性”**。多个特性间用`；`隔开，前后用`{}`括起来
>
> 对于重复属性设置，优先级高的覆盖优先级低的，相同的优先级后面的覆盖前面的。

**➹：**[语法 · GitBook](https://jirengu-inc.github.io/book.jirengu.com-fe/%E5%89%8D%E7%AB%AF%E5%9F%BA%E7%A1%80/CSS/%E8%AF%AD%E6%B3%95.html)

这就是为什么chrome会等所有样式都加载完毕后才会去渲染最终的一颗DOM树！形象一点就是，女孩子不化好装就不出门！你要知道粉底可是一层铺一层的，如选择脸蛋的这个元素，啊……此处省略很多字……完全不知道女性是怎样化妆的……

**➹：**[怎么化妆才可以化出韩剧女生的好皮肤底妆？ - 知乎](https://www.zhihu.com/question/27397083)

![1537876688144](img/01/1537876688144.png)

> The CSS parser builds a model of the style rules.
>
> Style rules are indexed in various ways for efficient lookup.
>
> (Property classes are auto-generated at build time by Python scripts.)

CSS解析器构建样式规则的模型。

样式规则以各种方式建立索引，以便高效查找。

(属性类是在构建时由Python脚本自动生成的。)

 这图给我的赶脚就是你写的样式，经过CSSPaser这个工人之手，**✎：**

![1537877368078](img/01/1537877368078.png)

对了，一个「CSSPropertyValue」可以称之为特性，也就是CSS属性值集可看作是特性集……

我觉得这里的属性值集是有停顿的，读作属性、值、集……之前我一直连读属性值，这样一来就没有属性的调调了！

![1537877575617](img/01/1537877575617.png)

> Style resolving (or recalc) takes all the parsed style rules from the active style sheets in the document, including a set of default styles supplied by the browser, and computes the final value of every style property for every DOM element.  These are stored in an object called ComputedStyle which is just a giant map of style properties to values.

样式分解(或重新计算)从文档中的活动样式表中获取所有解析过的样式规则，包括一组由浏览器提供的默认样式，并为每个DOM元素计算每个样式属性的最终值。它们存储在一个名为ComputedStyle的对象中，这个对象只是样式属性到值的巨大映射。

ps：

> Style recalc: for every node in the DOM tree, compute the value of every style property.

对于DOM树中的每个节点，计算每个样式属性的值。

也就是DOM树中的每个节点的样式属性的值都是经过计算而得来的最终值咯！由于ComputedStyle是个对象，按照JavaScript的说法就是这是个引用类型，即地址不变，内容是可以变化的啊！

这个计算需要哪些家伙参与呢？样式表（我不知道内联样式、内部样式、外部样式这3个家伙是不是被统一称作为活动样式表，对了，还有浏览器提供的默认样式），总之我就认为这是所有样式集合！

不知道是不是这样——页面更新了，那么样式就要再次计算了，是重新计算呢？还是只是局部计算？

![1537878556002](img/01/1537878556002.png)

> Chrome developer tools will show you the "computed style" for any DOM element.  It's also exposed to Javascript.  These are based on Blink's ComputedStyle object (but a few properties are augmented with layout data).

Chrome开发工具将向您展示任何DOM元素的“计算样式”。当然，它也会暴露在Javascript中（即可以被JavaScript操作）。注意，这些都是基于Blink的ComputedStyle对象(但是一些属性是通过布局数据扩展的)。

所以，你在查看一个元素的最终样式的时候，你需要看它的「computed style」，这才是正确的！

### ◇layout

![1537886608902](img/01/1537886608902.png)

ps：图中是，**✎：**

![1537887013338](img/01/1537887013338.png)

> Having built the DOM and computed all the styles, the next step is to determine the visual geometry of all the elements.
>
> So for a block-level element we're computing the coordinates of a rectangle corresponding to the geometric region of the content area that the element occupies.

在构建了DOM并计算了所有样式之后，下一步是确定所有元素的视觉几何体。

对于块级元素，我们计算的是一个矩形的坐标以及对应的元素所占据的内容区域的几何区域。

也就是计算一个块级元素，如一个div需要知道它的左顶点坐标（x，y）以及这个div的，**✎：**

![1537887961701](img/01/1537887961701.png)

其实，这里的盒子是用了 `box-sizing:border-box;`来计算！

也就是说元素的width和height的值就是padding、border、content的最终计算值相加咯！

那么margin呢？我想这是影响了它的坐标位置！

![1537888523066](img/01/1537888523066.png)

ps：

![1537888776156](img/01/1537888776156.png)

> In the simplest case, layout places blocks one after another in DOM order, descending vertically.  We call this "block flow".
>
> To know the height of a block, we must measure the text, using the font from the computed style, to determine where the lines break.

在最简单的情况下，布局按DOM顺序依次放置块，垂直向下。我们称之为“块流”。

要知道块的高度，我们必须使用计算样式中的字体来度量文本，以确定行在何处中断。

> Simple "block" layout objects are placed one after another, flowing down the page.

简单的“块”布局对象被一个接一个地放置，沿着页面向下流动。

> Line breaking is not simple.

断行并不简单。

确实不简单！听说这与字体设计有关！好吧！这不是我们能够决定哈！即便你设置了字体大小也很难精确的知道这些文本所占据的一行的最终高度！毕竟不同的字体，这高度是很有可能不一致的！

![1537889741315](img/01/1537889741315.png)

ps：只有那个网页在动

![scrolling](img/01/scrolling.gif)

> Layout may compute multiple kinds of bounding rects for a single layout object.
>
> For example, when there is overflow, layout computes both the border box rect and the layout overflow rect.
>
> If a node's overflow is scrollable, layout also computes scroll boundaries and reserves space for scrollbars.
>
> The most common scrollable DOM node is the document itself, which is the root of the tree.

布局可以计算单个布局对象的多种包围（边界？）矩形。

例如，当存在溢出时，layout同时计算边框矩形和布局溢出矩形。

如果节点的溢出是可滚动的，布局还会计算滚动边界并为滚动条预留空间。

最常见的可滚动DOM节点是document本身，别忘了它可是树的根。

> The contents of a layout object can overflow its border box.

布局对象的内容可以溢出其边框。

> Overflow can be visible, hidden, or **scrollable**.

溢出可以是可见的、隐藏的或可滚动的。图中的展示就是可滚动的效果！

![1537890846943](img/01/1537890846943.png)

> More complex layouts are needed for table elements or styles that specify things like breaking content into multiple columns, or floating objects that sit to one side with content flowing around them, or East Asian languages that have text running vertically instead of horizontally.
>
> Notice how DOM structure and ComputedStyle values like "float: left" are inputs to the layout algorithm.  Each pipeline stage uses the results of the previous stages. 

对于需要更复杂布局的，如table元素和一些元素的样式指定将内容拆分为多个列，或者是一些指定将被内容围绕在一旁的浮动对象，或者是指定将文本往竖直方向书写而不是常见的水平方向书写的东亚语言等等……

注意，DOM结构和ComputedStyle值(比如“float: left”)是布局算法的输入。每个管道阶段使用前一个阶段的结果。

> Other kinds of layout are even more complex.

其它类型的布局甚至更为复杂……

![1537893281301](img/01/1537893281301.png)

> Layout information is stored in a separate tree, linked to the DOM.  The nodes in this tree implement the layout algorithms.
>
> There are different subclasses of LayoutObject, depending on the desired layout behavior.

布局信息存储在单独的树（叫layout tree好了）中，连接到DOM。这个树中的节点实现布局算法。

根据所需的布局行为，LayoutObject有不同的子类。如这里的块级元素节点和文本节点

> C++ class hierarchy

c++ 类的层次结构。

![1537893842263](img/01/1537893842263.png)

ps：

![1537894334089](img/01/1537894334089.png)

这里有龙啊！就像是你的`span`中有个`p`这么恐怖！

> Usually, one DOM node gets one LayoutObject.  But sometimes a LayoutObject has no node, or a node has no LayoutObject.
>
> It's even possible for a node to have more than one LayoutObject.

通常，一个DOM节点得到一个LayoutObject。但是有时候LayoutObject没有节点，或者节点没有LayoutObject。

一个节点甚至可以拥有多个LayoutObject。

> DOM nodes are mostly 1:1 with layout objects, with some exceptions.

DOM节点大多与布局对象1:1，但也有一些例外。如这个元素是 `display:none`、或者这伪元素就没有DOM节点！

总之元素没有布局对象，那么用户就无法在页面上看见它！

![1537895126546](img/01/1537895126546.png)

> The layout stage runs after the style recalc stage.
>
> First, the layout tree is constructed.  Then, we walk the layout tree, filling in the geometry data, and processing side effects of the geometry.

布局阶段在样式recalc（重新计算）阶段之后运行。

首先，构建布局树。然后，我们遍历布局树，填充几何数据，处理几何的副作用

更新布局的操作，**✎：**

- 计算盒子边界以及其溢出的内容
- 创建行盒
- 添加/删除滚动条

> Layout (lifecycle stage) walks the layout tree filling in state describing the visual geometry of each LayoutObject.

布局(生命周期阶段)遍历布局树填充状态，以描述每个LayoutObject的视觉几何形状

![1537896164697](img/01/1537896164697.png)

> Today, layout objects contain both inputs and outputs of the layout stage, without a clean separation between them.
>
> For example, the LayoutObject acquires ownership of its element's ComputedStyle object.
>
> A new layout system called LayoutNG is expected to simplify the architecture, and make it easier to build new layout algorithms.

现在，布局对象既包含布局阶段的输入又包含输出，它们之间没有清晰的分隔。

例如，LayoutObject获得其元素的ComputedStyle对象的所有权。

一个名为LayoutNG的新布局系统有望简化架构，使构建新的布局算法变得更容易。

> A new system called LayoutNG will separate inputs from outputs more cleanly.

一种名为LayoutNG的新系统将更清晰地将输入和输出分离开来

> under construction

告示或警告标示施工中

ps：这里的布局理解有点迷糊……![img](img/01/034D9147.gif)

### ◇paint

![1537928522992](img/01/1537928522992.png)

> Now that we understand the geometry of our layout objects, it's time to paint them.
>
> Paint records paint operations into a list of display items.
>
> A paint operation might be something like "draw a rectangle at these coordinates, in this color".
>
> There may be multiple display items for each layout object, corresponding to different parts of its visual appearance, like the background, foreground, outline, etc.
>
> This is just a recording that can be played back later.  We'll see why that's useful in a bit.

现在我们已经了解了布局对象的几何形状，是时候绘制它们了。

将绘制操作记录到显示项列表中。

绘制操作可能类似于“在这些坐标上用这种颜色画一个矩形”。

每个布局对象可能有多个显示项，对应于其视觉外观的不同部分，如背景、前景、轮廓等。

这只是一段稍后可以回放的唱片。我们稍后会看到它为什么有用。

ps：不懂……

kBoxDecorationBackground：歌神装饰背景？

paint op buffer：绘制片头曲缓冲？

![1537929519820](img/01/1537929519820.png)

作者是类比了磁带对吧？

---

![1537929625465](img/01/1537929625465.png)

> It's important to paint elements in the right order, so that they stack correctly when they overlap.
>
> The order can be controlled by style.

以正确的顺序绘制元素是很重要的，这样当它们重叠时就能正确地叠加。

顺序可以由样式控制。

> Paint uses stacking order, not DOM order.

绘画作品使用堆叠顺序，而不是DOM顺序。

在这里，抛开之前的认识，按平时一样理解HTML和CSS的话，如果z-index的值是一样的话，那么显然后来的green相比yellow会显示在前，然而这里更改了z-index，yellow的要比green要大，这意味着yellow要占据上方了！

关于堆叠顺序，之前在芳芳课程的笔记，**✎：**

![1537930309383](img/01/1537930309383.png)

> 1. background
> 2. border
> 3. 块级
> 4. 浮动
> 5. 内联
> 6. z-index: 0
> 7. z-index: +
>
> 如果是兄弟元素重叠，那么后面的盖在前面的身上。

---

![1537930394414](img/01/1537930394414.png)

> It's even possible for an element to be partly in front of and partly behind another element.
>
> That's because paint runs in multiple phases, and each paint phase does its own traversal of a subtree.

甚至有可能一个元素一部分在另一个元素前面，一部分在另一个元素后面。

这是因为paint运行在多个阶段，并且每个阶段都对一个子树进行遍历。

> Each paint phase is a separate traversal of a stacking context.

每个绘制阶段都是一个堆叠上下文的**单独**遍历。

> blue after green, but foregrounds after backgrounds

蓝之后是绿，但是前景色之后是背景色。

也就是说照理应该是蓝色的盒子会盖住绿色盒子的前景色（即那个白色文字），然后实际是白色文字之后是蓝色背景了哈，这样就说明了一个元素的不安分，一部分规矩，一部分无厘头！

关于前景色和背景色的概述，**✎：**

> 前景色：前景色是插入，绘制的图形图片的颜色 。[背景色](https://baike.baidu.com/item/%E8%83%8C%E6%99%AF%E8%89%B2)是所要处理的图片的底色，默认的是白色。

ps：把文字看作是前景色，这是我从未想过的理解！

---

### ◇raster

![1537932321957](img/01/1537932321957.png)

ps：我没有想过竟然可以 以三维的角度去看颜色值，**✎：**

![1537932542723](img/01/1537932542723.png)

> The paint operations in the display item list are executed by a process called rasterization.
>
> Each cell in the resulting bitmap holds values for four color channels.

显示项列表中的绘制操作由一个称为**像素化**的进程执行。

产生的位图中的每个单元格包含四个颜色通道的值。

> Rasterization turns (part of) a display item list into a bitmap of color values.

**像素化**将显示项列表(部分)转换为颜色值的位图。

关于位图和矢量图，**✎：**

> 简单来说吧
> 位图～放大会模糊
> 矢量图～放大不模糊
>
> 删格化～就是指ps里把智能对象图层变成位图
>
> 智能对象图层～可以说是矢量图的特征，即放大不会模糊
>
> 另外，文字图层在ps也和智能图层一样，放大不会模糊
>
> 如果对文字图层，删格化，则变成了位图属性，即放大会模糊
>
> 有些操作和变化，必须删格化后才能进行，如果对某个图层无法操作和变化，就要删格化

**➹：**[夏夏的回答-知乎](https://www.zhihu.com/question/264920268/answer/290067076)

---

![1537933797623](img/01/1537933797623.png)

> The rastered bitmap is stored in memory, typically GPU memory referenced by an OpenGL texture object.
>
> The GPU can also run the commands that produce the bitmap ("accelerated rasterization").
>
> Note that these pixels are not yet on the screen!

点阵位图存储在内存中，通常是由OpenGL纹理对象引用的GPU内存。

GPU还可以运行生成位图(“加速**像素化**”)的命令。

注意，这些像素还没有出现在屏幕上!

> Rasterization can be accelerated by the GPU.

**像素化**可以由GPU加速。

---

![1537934094879](img/01/1537934094879.png)

> Rasterization issues OpenGL calls through a library called Skia.  Skia provides a layer of abstraction around the hardware, and understands more complex things like paths and Bezier curves.
>
> Skia is open-source and maintained by Google.  It ships in the Chrome binary but lives in a separate code repository.  It's also used by other products such as the Android OS.
>
> Skia's GPU-accelerated codepath builds its own buffer of drawing operations, which is flushed at the end of the raster task.

**像素化**通过一个名为Skia的库发出OpenGL调用。Skia在硬件周围提供了一个抽象层，可以理解更复杂的东西，比如路径和贝塞尔曲线。

Skia是开源的，由谷歌维护。它装载在Chrome二进制文件中，但存在于一个单独的代码存储库中。Android操作系统等其它产品也使用这种技术。

Skia的GPU加速的代码路径构建了自己的绘图操作缓冲区，在**像素化**任务结束时刷新。

> GL calls are issued through the Skia graphics library.

GL调用是通过Skia图形库发出的。

---

### ◇gpu

![1537934824680](img/01/1537934824680.png)

> Recall that the renderer process is sandboxed, so it can't make system calls directly.
>
> GL calls issued by Skia are actually proxied into a different process using a "command buffer".
>
> The GPU process receives the command buffer and issues the "real" GL calls through a set of function pointers.
>
> Besides escaping the renderer sandbox, isolating GL in the GPU process protects us from unstable or insecure graphics drivers.

回想一下，渲染器进程是沙箱化的，因此它不能直接进行系统调用。

Skia发出的GL调用实际上使用“command buffer”代理到另一个进程中。

GPU进程接收命令缓冲，并通过一组函数指针发出“真正的”GL调用。

除了逃离渲染器沙箱外，在GPU进程中隔离GL还可以保护我们不受不稳定或不安全的图形驱动。

> Skia's GL calls are proxied into the GPU process.

Skia的GL调用被代理到GPU进程中

---

![1537936664089](img/01/1537936664089.png)

> Those GL function pointers are initialized by dynamic lookup from the system's shared OpenGL library - or the ANGLE library on Windows.
>
> ANGLE is another library built by Google; its job is to translate OpenGL to DirectX, which is Microsoft's API for accelerated graphics on Windows.
>
> There are also OpenGL drivers for Windows, but historically they have not been very high quality.

这些GL函数指针是通过从系统的共享OpenGL库或Windows上的角度（ANGLE）库进行动态查找来初始化的。

ANGLE是谷歌建立的另一个库;它的工作是将OpenGL转换为DirectX，这是微软在Windows上图形加速的API。

Windows也有OpenGL驱动程序，但历史上它们的质量不是很高。

> The GL functions are dynamically linked. On Windows, we translate to DirectX.

GL函数是动态链接的。在Windows上，我们转换为DirectX。

---

![1537937012222](img/01/1537937012222.png)

> Moving raster to the GPU process will improve performance.
>
> It's also needed to support Vulkan.

将像素化移到GPU进程中将提高性能。它也需要支持Vulkan。

> In the future, raster will happen in the GPU process.

在未来的GPU进程中将会出现像素化……目前正在施工……

---

### ◇change

![1537937313165](img/01/1537937313165.png)

> We have now gone all the way from content to pixels in memory.
>
> But note that the rendering is not static.
>
> Running the full pipeline is expensive.

我们已经大致了解了如何把内容转化为在内存中的像素了。

但是请注意，渲染不是静态的。

整个管道的运营是昂贵的。

> We now have a complete pipeline.
>
> But what if state can change?

我们现在有一个完整的管道（流水线）。

但如果状态可以改变呢?——如滚动、缩放、动画、荷载增量（？？？）、JavaScript……

> "Change is good."
>
> "Yeah, but it's not easy.

“改变是好的。”

“是的，但这并不容易。

----

### ◇frames

![1537939638608](img/01/1537939638608.png)

> Change is modelled as animation frames.
>
> Each frame is a complete rendering of the state of the content at a particular point in time.

变化被建模为动画帧。

每个帧（画面）都是在特定时间点上内容状态的完整呈现。

> The renderer produces **animation frames**.

渲染器产生动画帧。

> Below 60 frames per second, scrolling and animations look "janky".

低于每秒60帧，滚动和动画看起来像“janky”。（卡顿的帧？janky？）

> time spent to render the frame

渲染帧所耗的时间

VSync ：场同步信号

**➹：**[Janky frames 是如何计算出来的 - CSDN博客](https://blog.csdn.net/zxt06img/01/article/details/79228005)

**➹：**[Janky Frames和Missed vsync的区别是什么？ - 知乎](https://www.zhihu.com/question/264960206)

---

### ◇invalidation

![1537940367472](img/01/1537940367472.png)

> Each pipeline stage tracks granular asynchronous **invalidations**.

每个管道阶段跟踪粒度异步失效。

> Outputs are reused from previous frames when possible.

如果可能的话，可以从以前的帧中重用输出。

ps：不懂……

---

### ◇repaint

![1537940624831](img/01/1537940624831.png)

> Paint + raster remain expensive if a large region is transformed…

如果一个大区域被改变，绘制+像素化 仍然 是昂贵的…

这是在告诉我简单的滚动一下网页都要消耗很多资源吗？——ppt中说到「所有像素都变了!」

### ◇jank

![1537941121829](img/01/1537941121829.png)

> … and anything on the main thread competes with JavaScript.

主线程上的任何内容都与JavaScript竞争。

> mineSomeBitcoins(); // why not?

挖点比特币矿……

什么鬼？可用JavaScript挖矿？

**➹：**[不要让你的电脑为别人挖矿](https://zhuanlan.zhihu.com/p/33120996)

---

### ◇enter: compositing

![1537941591998](img/01/1537941591998.png)

> - Decompose the page into layers.
> - Composite the layers on another thread.

- 将页面分解成图层。
- 在另一个线程上合成图层。

ps：

![1537941957323](img/01/1537941957323.png)

突然觉得学PS对页面的理解意义重大啊！

`¯\_(ツ)_/¯`，**✎：**

> **耸肩**是一种将两肩提起的肢体语言，表明动作人对一个问题的答案不清楚，或者是对答案的无所谓态度。 有时也可以表达对问题的无视。 有时候会将眼睛睁大或配以夸张的皱眉，或摊开双手。 在西方世界中这一手势极为常用。

**➹：**[耸肩- 维基百科，自由的百科全书](https://zh.wikipedia.org/zh-hans/%E8%80%B8%E8%82%A9)

---

### ◇compositing

![1537942829324](img/01/1537942829324.png)

动画：一个图层的移动

![](img/01/doge.gif)

滚动：一个图层的移动；来到同一个图层中另外一个片段

![](img/01/scrolling-1.gif)

多点触控：一个图层的缩放……

![](img/01/zoom.gif)

---

![1537943296005](img/01/1537943296005.png)

input from browser process ：浏览器进程输入

The compositor thread can handle input events：compositor线程可以处理输入事件

图中的动画，跟上文的是同一张滚动动图……

在这里，用户滚动滚动条也是一种输入……

---

### ◇layer tree

![1537951391978](img/01/1537951391978.png)

Layers are also a tree!——图层也是一棵树

- "stacked" by  preorder  traversal——“堆叠”通过预序遍历
- can clip  descendants,  or apply effects——可以裁剪子节点（如图中的1/4），或应用效果

**➹：**[预排序遍历树算法 - Renfei's blog](http://mcdullfei.github.io/2016/05/26/%E9%A2%84%E6%8E%92%E5%BA%8F%E9%81%8D%E5%8E%86%E6%A0%91%E7%AE%97%E6%B3%95/)

---

![1537952666081](img/01/1537952666081.png)

> Certain style properties cause a layer to be created for a layout object.
>
> If a layout object doesn't have a layer, it paints into the layer of the nearest ancestor that has one.

某些样式属性会导致为布局对象创建一个图层。

如果布局对象没有图层，它将绘制到具有图层的最近的祖先图层中。

- The layer tree is based on the layout tree.——图层树是基于布局树的。
- PaintLayer is a compositing "candidate".——PaintLayer是一个混合的「候选」

---

![1537953381051](img/01/1537953381051.png)

ps：

![1537953541646](img/01/1537953541646.png)

Scroll containers create a whole set of layers.——滚动容器创建一组完整的图层。

scroll layer applies clip——滚动图层适用于裁剪操作！

---

### ◇compositing update

![1537953636217](img/01/1537953636217.png)

> Building the layer tree is a new lifecycle stage on the main thread.  Today, this happens before paint, and each layer is painted separately.

在主线程上构建图层树是一个新的生命周期阶段。今天，这发生在绘制之前，并且每一层被单独地着色。

ps：这里是合成更新操作！

### ◇slimming paint

![1537954050338](img/01/1537954050338.png)

PaintArtifact：绘制手工艺品

In the future, layers will be determined after paint.——在未来，将会在paint之后才去确定图层！

——减肥的paint……

---

### ◇commit

![1537954509006](img/01/1537954509006.png)

> After paint is finished, the commit updates a copy of the layer tree on the compositor thread, to match the state of the tree on the main thread.

paint完成后，提交将在compositor（合成器）线程上更新图层树的副本，以匹配主线程上树的状态。

---

### ◇tiling

![1537954810377](img/01/1537954810377.png)

ps：

![1537954887989](img/01/1537954887989.png)

> Recall: raster is the step after paint, which turns paint ops into bitmaps.
>
> Layers can be large - rastering the whole layer is expensive, and unnecessary if only part of it is visible.
>
> So the compositor thread divides the layer into tiles.
>
> Tiles are the unit of raster work.  Tiles are rastered with a pool of dedicated raster threads.  Tiles are prioritized based on their distance from the viewport.
>
> (Not shown: a layer actually has multiple tilings for different resolutions.)

回忆一下: 像素化是paint之后的步骤，它将paint ops（行动？行为？）转换为位图。

图层可以是很大的 - 像素化整个图层是昂贵（好资源）的，并且如果只有一部分是可见的话是没有必要的，。

因此合成器线将图层划分为瓷砖。

瓷砖是raster工作的单位。瓷砖会被一专用的raster线程池进行扫描。Tiles的优先级基于它们与视口的距离。

(没有显示:对于不同的分辨率，一个图层实际上有多个铺以瓷砖。)

> Layers are broken into tiles for raster

图层由于raster的原因被分解成瓷砖！

CategorizedWorkerPool——分门别类的线程池……

---

### ◇drawing

![1537956091421](img/01/1537956091421.png)

> Once all the tiles are rastered, the compositor thread generates "draw quads".  A quad is like a command to draw a tile in a particular location on the screen, taking into account all the transformations applied by the layer tree.  Each quad references the tile's rastered output in memory (remember, no pixels are on the screen yet).
>
> The quads are wrapped up in a compositor frame object which gets submitted to the browser process.

一旦所有的瓷砖都被平铺，合成器线程就会生成“画 四边形”。四边形就像一个命令，在屏幕上的特定位置绘制一个瓷砖，考虑到图层树应用的所有转换。每个四边形在内存中引用瓷砖的点阵输出(记住，屏幕上还没有像素)。

四边形被包装在一个compositor帧对象中，这个帧对象被提交给浏览器进程。

> CompositorFrame is the "output" of the renderer process.

CompositorFrame是渲染器进程的“输出”。

> Tiles are "drawn" as quads

瓷砖被“画”成为四边形。

---

### ◇activation

![1537956658609](img/01/1537956658609.png)

> The compositor thread has two copies of the tree, so that it can raster tiles from a new commit while drawing the previous commit.

compositor线程有树的两个副本，这样它就可以在绘制 前一个提交的同时，从一个新的提交中像素化tile

> Drawing can continue while a new commit is rastered.

在重新提交时，绘图可以继续被像素化。

---

### ◇display (viz)

![1537956943459](img/01/1537956943459.png)

> The browser process runs a component called the display compositor, inside a service called "viz" (short for visuals).
>
> The display compositor aggregates compositor frames submitted from all the renderer processes, along with frames from the browser UI outside of the WebContents.  Then it issues the GL calls to draw the quad resources, which go to the GPU process just like the GL calls from the raster workers.
>
> On most platforms the display compositor's output is double-buffered, so the quads draw into a backbuffer, and a "swap" command makes it visible.
>
> (On OS X we do something a little different with CoreAnimation.)
>
> Finally our pixels are on the screen. :)

浏览器进程运行一个名为display compositor的组件，它位于一个名为“viz”(视觉效果的简称)的服务中。

display compositor聚合所有渲染器进程提交的compositor帧，以及来自WebContents外部的浏览器UI的帧。然后它发出GL调用 来绘制四边形资源，这些资源进入GPU进程，就像来自raster workers的GL调用一样。

在大多数平台上，显示合成器的输出都是双缓冲的，因此四边形被拉进一个回缓冲，“swap”命令会使它可见。

（在OS X上，我们用CoreAnimation做了一些不同的事情。）

最后，我们的像素在屏幕上。:)

> The display compositor puts pixels on the screen.

显示合成器（显示器？）将像素放在屏幕上。

---

### ◇review

> 我喜欢回顾……因为越看到后面越看不懂！

![1537957451440](img/01/1537957451440.png)

---

### ◇end

![1537957515460](img/01/1537957515460.png)

- slides:  [bit.ly/lifeofapixel](http://bit.ly/lifeofapixel)

- feedback:  [skobes@chromium.org](mailto:skobes@chromium.org)

## ★总结

- 有道翻译很强大！当你看英文文章的时候，你最好使用分治的思想去看，就像是这55张ppt一样，一点点英文来看，看到最后，你发现把它们组合起来后，这篇「Life of a Pixel」就看完了！别忘了记这其中的笔记，因为理解内容是需要上下文的！
- 你不需要了解这个名词的中文意思，你只需要知道这个名词干了什么……
- 你需要回顾很多遍……

## ★Q&A

### ①金丝雀？

> **Canary** - daily build，这个是最新且可以用于哪些不怕死的开发者和极客们玩新功能的版本。反正bug和漏洞是很多的，但是新玩意也是很多的。这个经过了一定的测试，但是bug应该还是蛮多的。

**➹：**[Chrome 、Chromium 和 Chrome Canary 这三个版本之间的关系和异同是什么？ - 知乎](https://www.zhihu.com/question/19795356)

### ②WebAssembly？

**➹：**[如何评论浏览器最新的 WebAssembly 字节码技术？ - 知乎](https://www.zhihu.com/question/31415286)

> **WebAssembly**或称**wasm**是一个**实验性**的低阶[程式语言](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%25A8%258B%25E5%25BC%258F%25E8%25AA%259E%25E8%25A8%2580&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhgxwslHV6pmqZaajXsjYf9QAmHa7g) ，应用于[浏览器](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%2580%258F%25E8%25A6%25BD%25E5%2599%25A8&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhjrDryXaSCDwFkYgX49ARM442G19w)内的[客户端](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E5%25AE%25A2%25E6%2588%25B6%25E7%25AB%25AF&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhk_8HV2AubcdyBYromseRHeog9JA) 。 WebAssembly是可携式的[抽象语法树](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E6%258A%25BD%25E8%25B1%25A1%25E8%25AA%259E%25E6%25B3%2595%25E6%25A8%25B9&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhDo4BxElwXGVx9RigGjZoX4r1WTQ) [[1\]](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/zh/WebAssembly&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhVaoPDLMCV9ai8w6VwZGKGbD5D5Q#cite_note-1) ，被设计来提供比[JavaScript](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/JavaScript&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhMwraKCw5MpGHKkkGEG5DzqNFLsA)更快速的[编译](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%25BC%2596%25E8%25AF%2591&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhj8TxjXHD8i6VODkx4nI5RLeO4NQQ)及执行[[2\]](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/zh/WebAssembly&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhVaoPDLMCV9ai8w6VwZGKGbD5D5Q#cite_note-github.com-2) 。 WebAssembly将让开发者能运用自己熟悉的程式语言（最初以[C](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/C%25E8%25AA%259E%25E8%25A8%2580&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhd3i1T1PcNq_DnbSyA_BdvjJUSfg) / [C++](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/C%252B%252B&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhi7SFShK4GtqcLyG7xuk4MfLk0D7Q)作为实作目标）编译，再借虚拟机器引擎在浏览器内执行[[3\]](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/zh/WebAssembly&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhVaoPDLMCV9ai8w6VwZGKGbD5D5Q#cite_note-3) 

**➹：**[WebAssembly - Wikiwand](https://www.wikiwand.com/zh/WebAssembly)

强烈推荐下面这个答案，**✎：**

**➹：**[如何评论浏览器最新的 WebAssembly 字节码技术？ - 罗志宇的回答 - 知乎](https://www.zhihu.com/question/31415286/answer/58022648)

其中提到这么一个技术，**✎：**

> 对于不支持 Web Assembly 的浏览器， 会有一段 Javascript 把 Web Assembly 重新翻译为 Javascript 运行， 这个技术叫 **polyfill**, HTML5 刚出来的时候很常用的一个技术。

**polyfill**：这个技术之前在芳芳视频中讲到过，不过忘记了，在哪儿节课讲的了！

### ③Chromium？

> **Chromium**是Google为发展自家的浏览器[Google Chrome](https://www.wikiwand.com/zh-cn/Google_Chrome)而开启的项目，以[BSD许可协议](https://www.wikiwand.com/zh-cn/BSD%E6%8E%88%E6%AC%8A%E6%A2%9D%E6%AC%BE)等数种许可发行并[开放源代码](https://www.wikiwand.com/zh-cn/%E9%96%8B%E6%94%BE%E5%8E%9F%E5%A7%8B%E7%A2%BC)。Chromium与Google Chrome共享大部分代码和功能，但功能和商标之间有一些细微差别。
>
> Chromium 的更新速度很快，每隔数小时即有新的开发版本发布，每次的更新幅度不一定相同，可能增加新功能，或者单纯修正问题，由于新功能会先在Chromium上测试，等待认证后才会应用在Google Chrome上，所以Chromium相当于Google Chrome的先行版。

**➹：**[Chromium - Wikiwand](https://www.wikiwand.com/zh-cn/Chromium)

看来我还是使用Google Chrome好了！

而且国内与很多浏览器是基于Chromium开发的，如QQ浏览器、UC浏览器、还有前段时间很「火」的红芯浏览器等等……

**➹：**[日常使用，Google Chrome 和 Chromium 二者间如何选择？ - 知乎](https://www.zhihu.com/question/19799660)

**➹：**[基于chromium的浏览器和chrome有什么差距？ - 知乎](https://www.zhihu.com/question/27046250#explore)

**➹：**[主流浏览器内核介绍（前端开发值得了解的浏览器内核历史） - 前端 - 掘金](https://juejin.im/entry/57ff3cea0e3dd90057e5f25e)

Google Chrome的内核可是Blink啊！不要跟chromium搞混了啊！

### ④沙盒？

> 在[计算机安全](https://www.wikiwand.com/zh-hans/%E8%AE%A1%E7%AE%97%E6%9C%BA%E5%AE%89%E5%85%A8)领域，**沙盒**（英语：sandbox，又译为**沙箱**）是一种安全机制，为执行中的程式提供的隔离环境。通常是作为一些来源不可信、具破坏力或无法判定程序意图的程序提供实验之用[[1\]](https://www.wikiwand.com/zh-hans/%E6%B2%99%E7%9B%92_(%E9%9B%BB%E8%85%A6%E5%AE%89%E5%85%A8)#citenote1)。
>
> 沙盒通常严格控制其中的程序所能访问的资源，比如，沙盒可以提供[用后即回收](https://www.wikiwand.com/zh-hans/%E5%A1%97%E9%8A%B7%E7%A9%BA%E9%96%93)的磁盘及内存空间。在沙盒中，网络访问、对真实系统的访问、对输入设备的读取通常被禁止或是严格限制。从这个角度来说，沙盒属于[虚拟化](https://www.wikiwand.com/zh-hans/%E8%99%9A%E6%8B%9F%E5%8C%96)的一种。
>
> 沙盒中的所有改动对[操作系统](https://www.wikiwand.com/zh-hans/%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F)不会造成任何损失。通常，这种技术被[计算机](https://www.wikiwand.com/zh-hans/%E8%AE%A1%E7%AE%97%E6%9C%BA)技术人员广泛用于测试可能[带毒](https://www.wikiwand.com/zh-hans/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%97%85%E6%AF%92)的程序或是其他的[恶意代码](https://www.wikiwand.com/zh-hans/%E6%81%B6%E6%84%8F%E8%BD%AF%E4%BB%B6)[[2\]](https://www.wikiwand.com/zh-hans/%E6%B2%99%E7%9B%92_(%E9%9B%BB%E8%85%A6%E5%AE%89%E5%85%A8)#citenote2)。

**➹：**[沙盒 (计算机安全)](https://www.wikiwand.com/zh-hans/%E6%B2%99%E7%9B%92_(%E9%9B%BB%E8%85%A6%E5%AE%89%E5%85%A8))

### ⑤管道？

**➹：**[pipeline是什么？ - 知乎](https://www.zhihu.com/question/267436664)

**➹：**[流水线 (计算机)](https://www.wikiwand.com/zh-hans/%E6%B5%81%E6%B0%B4%E7%BA%BF_(%E8%AE%A1%E7%AE%97%E6%9C%BA))

> **流水线**，亦称[管线](https://www.wikiwand.com/zh-hans/%E7%AE%A1%E7%B7%9A)，是现代计算机[处理器](https://www.wikiwand.com/zh-hans/%E5%A4%84%E7%90%86%E5%99%A8)中必不可少的部分，是指将计算机[指令](https://www.wikiwand.com/zh-hans/%E6%8C%87%E4%BB%A4)处理过程拆分为多个步骤，并通过多个硬件处理单元并行执行来加快指令执行速度。其具体执行过程类似工厂中的流水线，并因此得名。
>
> 如果作出类比，则计算机指令就是流水线传送带上的产品，各个硬件处理单元就是流水线旁的工人。

如果我把content看作是流水线上产品的原料，那么渲染引擎等就是工人，最终都是把原料加工成产品，而这个产品则是用户所浏览的一张真正的网页！

### ⑥OpenGL？

**➹：**[OpenGL - 维基百科，自由的百科全书](https://zh.wikipedia.org/zh/OpenGL)

> **OpenGL** （ 英语： *Open Graphics Library* ，译名： **开放图形库**或者“开放式图形库”）是用于[渲染](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E6%25B8%25B2%25E6%259F%2593&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhiz4SEmREqamxuwswZKFQm-uhOgzw) [2D](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E4%25BA%258C%25E7%25BB%25B4%25E8%25AE%25A1%25E7%25AE%2597%25E6%259C%25BA%25E5%259B%25BE%25E5%25BD%25A2&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhjf-RiM1B05ygKvqfedwW2gzL1XiA) 、 [3D](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E4%25B8%2589%25E7%25B6%25AD%25E8%25A8%2588%25E7%25AE%2597%25E6%25A9%259F%25E5%259C%2596%25E5%25BD%25A2&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhj0ORbYAlTnNRCaA4Hu-PICoS4vvw) [矢量图形](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%259F%25A2%25E9%2587%258F%25E5%259C%2596%25E5%25BD%25A2&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhh79H2dzISFizze_hKiYveA2HKZxQ)的跨[语言](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%25A8%258B%25E5%25BC%258F%25E8%25AA%259E%25E8%25A8%2580&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhgxwslHV6pmqZaajXsjYf9QAmHa7g) 、 [跨平台](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E8%25B7%25A8%25E5%25B9%25B3%25E5%258F%25B0&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhiy3RMXFjcYFz17KFOmaGfihTGHig)的[应用程序编程接口](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E5%25BA%2594%25E7%2594%25A8%25E7%25A8%258B%25E5%25BA%258F%25E7%25BC%2596%25E7%25A8%258B%25E6%258E%25A5%25E5%258F%25A3&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhe5gjTqC_QGryuDt5L70FeD9vbrQ) （API）。 这个接口由近350个不同的函数调用组成，用来从简单的图形位元绘制复杂的三维景象。 而另一种程式介面系统是仅用于[Microsoft Windows](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/Microsoft_Windows&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhgEpZBq9CBJ7vrQyn0gQbs9ahNYEA)上的[Direct3D](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/Direct3D&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhg3kon0gxZD16m9VWqmV32IknRhCw) 。 OpenGL常用于[CAD](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/CAD&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhhUHG9w2oWBUOgz1GucVBqodLQxWQ) 、 [虚拟实境](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E8%2599%259B%25E6%2593%25AC%25E5%25AF%25A6%25E5%25A2%2583&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhiAbs4smTqb49y0HMmGStbSMGp8hg) 、科学视觉化程式和[电子游戏开发](https://translate.googleusercontent.com/translate_c?depth=1&hl=zh-CN&prev=search&rurl=translate.google.com&sl=zh-TW&sp=nmt4&u=https://zh.wikipedia.org/wiki/%25E7%2594%25B5%25E5%25AD%2590%25E6%25B8%25B8%25E6%2588%258F%25E5%25BC%2580%25E5%258F%2591&xid=17259,15700021,15700124,15700149,15700186,15700191,15700201,15700214&usg=ALkJrhj2xJu6Lb83_O_zcfukRkL2hl5hOA) 。

**➹：**[如何理解 OpenGL 中着色器、渲染管线、光栅化等概念？ - 知乎](https://www.zhihu.com/question/29163054)

### ⑦API？

**➹：**[用大白话给你科普，到底什么是 API（应用程序编程接口）？_36氪](https://36kr.com/p/5128407.html)

我既有的认识就是一些能提供你用的东西！

### ⑧Web？

**➹：**[Web 是什么意思？ - 知乎](https://www.zhihu.com/question/19860216)

> Web 所指的非常简单，即用网页浏览器浏览网页。而且，在1994年便有了耳熟能详的“万维网”这个通俗易懂的名字。

**➹：**[Web 是什么意思？ - 冯东的回答 - 知乎](https://www.zhihu.com/question/19860216/answer/13184646)

**✎：**

> Web 在不同语境下有不同意义。 
> 目前来看最常用的意义是指在 Intenet 上和 HTML 相关的部分。换句话说，目前在 Intenet 上通过非浏览器访问的网络资源并不称为 Web。这也是 Wired 的那篇《Web 已死，互联网永生》的意思。

### ⑨Here be dragons？

> **此处有龙**（[拉丁文](https://www.wikiwand.com/zh-cn/%E6%8B%89%E4%B8%81%E8%AF%AD)：hic sunt dracones；[英文](https://www.wikiwand.com/zh-cn/%E8%8B%B1%E8%AF%AD)：Here be dragons）是欧洲人在[中世纪](https://www.wikiwand.com/zh-cn/%E4%B8%AD%E4%B8%96%E7%B4%80)时用来表述地图上未被探索或被认为很危险的地域的术语。通常他们会在地图上的那块区域上画上[龙](https://www.wikiwand.com/zh-cn/%E9%BE%8D_(%E8%A5%BF%E6%96%B9))、大海蛇或其他神话中的怪兽。

**➹：**[此处有龙 - Wikiwand](https://www.wikiwand.com/zh-cn/%E6%AD%A4%E8%99%95%E6%9C%89%E9%BE%8D)

### ⑩`::`这个是啥符号？

**➹：**[C++ “::” 作用域符 双冒号 - CSDN博客](https://blog.csdn.net/qq_33266987/article/details/53689133)

> :: 是作用域符，是运算符中等级最高的，它分为三种:
>
> 1)global scope(全局作用域符），用法（::name)
> 2)class scope(类作用域符），用法(class::name)
> 3)namespace scope([命名空间](https://www.baidu.com/s?wd=%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1YvPj-hrANhuHRzm1RduHm30ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3ErHb1PjnzrHR)作用域符），用法(namespace::name)
>
> 它们都是左关联（left-associativity)，它们的作用都是为了更明确的调用你想要的变量

上文的图中都是类作用域符了！**✎：**

![1537895684826](img/01/1537895684826.png)

这里的UpdateLayout就是LayoutObject成员函数了！

### ⑪LayoutNG？

**➹：**[LayoutNG介绍](https://zhuanlan.zhihu.com/p/37847490)

> 当前的Layout代码面临着很多的问题，比如:
>
> - 代码非常古老，能追溯到KHTML时期。
> - 功能非模块化、缺少封装、不可重入、非线程安全。
> - 为以前的Web平台设计，阻碍了新的排版的加入。
>
> 为了解决以上问题，提出了Blink下的全新排版引擎——LayoutNG

### ⑫buffer？

**➹：**[Cache 和 Buffer 都是缓存，主要区别是什么？ - 知乎](https://www.zhihu.com/question/26190832?rf=66405569)

### ⑬荷载增量？

**➹：**[abaqus增量步的理解-窗台上的叔本华-新浪博客](http://blog.sina.com.cn/s/blog_63069a8c0101e3lf.html)

**➹：**[database - What does "incremental load" mean? - Stack Overflow](https://stackoverflow.com/questions/4471161/what-does-incremental-load-mean)

按照自己的就是，就像是Git一样，每次上传到远程仓库都不是全部重新上传一遍，只是上传已修改过的内容！

### ⑭**preorder**？

> **预序关系**（简称**预序**，又称**先序**，**preorder**）、在[数学](https://www.wikiwand.com/zh-hans/%E6%95%B0%E5%AD%A6)中，是一类接近于[偏序关系](https://www.wikiwand.com/zh-hans/%E5%81%8F%E5%BA%8F%E5%85%B3%E7%B3%BB)的二元关系，但仅满足[自反性](https://www.wikiwand.com/zh-hans/%E8%87%AA%E5%8F%8D%E6%80%A7)和[传递性](https://www.wikiwand.com/zh-hans/%E4%BC%A0%E9%80%92%E6%80%A7)而不满足[反对称性](https://www.wikiwand.com/zh-hans/%E5%8F%8D%E5%AF%B9%E7%A7%B0%E6%80%A7)。偏序的大多数理论均可扩展到预序。

**➹：**[预序关系 - Wikiwand](https://www.wikiwand.com/zh-hans/%E9%A2%84%E5%BA%8F%E5%85%B3%E7%B3%BB)



⑮⑯⑰⑱⑲⑳