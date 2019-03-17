---
typora-copy-images-to: img\03
---

# I never understood JavaScript closures

直到有人向我这样解释

原文：[I never understood JavaScript closures – DailyJS – Medium](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)

正如标题所述，JavaScript闭包对我来说一直是个谜。我已经[阅读了](https://medium.freecodecamp.org/lets-learn-javascript-closures-66feb44f6a44) [好几篇](https://medium.freecodecamp.org/whats-a-javascript-closure-in-plain-english-please-6a1fc1d2ff1c) [文章](https://en.wikipedia.org/wiki/Closure_%28computer_programming%29) 了，我在工作中使用了闭包，有时我甚至使用了闭包，但却没有意识到我正在使用闭包。

最近我去参加了一个讲座，有人以一种能让我终于明白的方式对它进行了真正的解释。在这篇文章中，我将尝试用这种方法来解释这所谓的闭包。让我把这个功劳归于[CodeSmith](https://www.codesmith.io/)和他们的JavaScript the Hard Parts系列

### ★在我们开始之前

在你开始去理解闭包之前，有一些极其重要的概念得要你事先知晓的，比如执行上下文（ *execution context*）等等

[这篇文章](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)对执行上下文有一个很好的入门，因此，你可以去看看。我引用了这篇文章的一些内容：

> 当代码在JavaScript中运行时，执行它的环境是非常重要的，而这其中的环境被JavaScript引擎评估为以下之一:
>
> - 全局代码 - 首次执行代码的默认环境
> - 函数代码 - 每当执行流程进入函数体时。
>
> ……
>
> 关于术语`execution context`，我们可以视它为当前代码所被评估的环境/作用域（ *environment / scope*）。

换句话说，当我们启动程序时，我们是从全局的执行上下文开始的。一些变量在全局执行上下文中声明。我们就把这些变量称之为全局变量。当程序中，有调用函数时，会发生什么呢？——有以下几个步骤：

1. JavaScript创建一个新的执行上下文，即，一个局部执行上下文，其实你可以认为这就是一个局部作用域哈！
2. 该局部执行上下文将具有其自己的变量集，而这些变量将会是该执行上下文的局部变量。
3. 这新的执行上下文被抛到执行堆栈（call stack）上 。你可以将执行堆栈视为用来**跟踪程序在执行中的位置**的这样一种机制

那么被调用的函数什么时候就执行结束了呢？当遇到`return`语句或遇到结束括号`}` 时，就GG了。函数结束时，会发生以下情况：

1. 局部执行上下文会从执行堆栈中弹出
2. 这个函数将其返回值发送回调用上下文。调用上下文是调用此函数的执行上下文，它可以是全局执行上下文或另一个局部执行上下文。由调用执行上下文来处理该点的返回值。返回的值可以是一个对象，一个数组，一个函数，一个布尔值等等其它任何东西。如果函数没有`return`语句，则返回`undefined` 。
3. 局部执行上下文被**销毁**。这个很重要。再次强调「销毁（Destroyed）」。意味着在局部执行上下文中声明的所有变量都将被删除。它们不再可用。这就是它们被称为**局部变量**的原因啦！

## ★一个非常基本的例子

