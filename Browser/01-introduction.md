---
typora-copy-images-to: img\01
---

# 简介

## ★我们要讨论的浏览器

开放源代码浏览器，即 Firefox、Chrome 浏览器和 Safari（部分开源）

## ★浏览器的主要功能

1. 发请求和展示请求资源

ps：资源通常是HTML文档、图片、PDF等其它类型的资源。用户使用URI来找到这个资源在哪儿，总之一个资源一个URI

### ◇一些规范

1. 浏览器怎么解释、怎么显示html文件的姿势是在 HTML 和 CSS 规范中指定的。

   你说会100%遵从这些规范吗？——天真，不搞点独有的扩展程序，如何能在市场上占领份额？

   这意味着Web开发者需要兼容各大浏览器，不然页面很容易出现bug！

### ◇你没有留意过的用户界面

各大浏览器都有的元素，**✎：**

1. 用来输入 URI 的地址栏
2. 用来前进和后退按钮
3. 书签设置选项
4. 用于刷新页面和停止加载当前页面的按钮
5. 用于返回页面主页的按钮

并没有规范说要求各大浏览器厂商必须要有这些UI元素！反正就是互相模仿借鉴的结果，最后产生了这么一个最佳实践！

HTML5看了这个自然产生的规范，就说「好吧！那么我就提一些吧！列出一些通用的元素好了」，**✎：**

1. 地址栏、状态栏和工具栏等等

### ◇为啥这个浏览器的市场占有率那么高？

各大浏览器有自己独特的功能，或许就因为某个功能而一见钟情呢？

## ★浏览器的高层结构

我看作是「整体结构」好了，**✎：**

浏览器的主要组件，**✎：**

> 1. **用户界面** - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。
> 2. **浏览器引擎** - 在用户界面和呈现引擎之间传送指令。
> 3. **呈现引擎** - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。
> 4. **网络** - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。
> 5. **用户界面后端** - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。
> 6. **JavaScript 解释器**。用于解析和执行 JavaScript 代码。
> 7. **数据存储**。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。

7龙珠，集齐7龙珠就能召唤神龙……如告知神龙需要一张图，**✎：**

![浏览器的主要组件](img/01/layers.png)

给你一张图，让你直观个明白！

关于浏览器的内核，由于JavaScript引擎越来越独立，内核就倾向于只指渲染引擎，而不是原先的渲染引擎(layout engineer或者Rendering Engine)和JS引擎。

ps：

> 这里要单独提一下Chrome。和大多数浏览器不同，Chrome 浏览器的每个标签页都分别对应一个呈现引擎实例。每个标签页都是一个独立的进程。

也就是说打开多个标签页是很费内存的？——推荐使用一个chrome插件「The Great Suspender」



## ★Q&A

**①浏览器的状态栏、工具栏？**

说实在的，你似乎从来没有了解过浏览器有什么东西。**✎：**

[Google Chrome - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/Google_Chrome#%E5%8A%9F%E8%83%BD)

就知道个地址栏……



