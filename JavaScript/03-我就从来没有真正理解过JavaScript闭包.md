---
typora-copy-images-to: img\03
---

# I never understood JavaScript closures

直到有人向我这样解释

原文：[I never understood JavaScript closures – DailyJS – Medium](https://medium.com/dailyjs/i-never-understood-javascript-closures-9663703368e8)

正如标题所述，JavaScript闭包对我来说一直是个谜。我已经[阅读了](https://medium.freecodecamp.org/lets-learn-javascript-closures-66feb44f6a44) [好几篇](https://medium.freecodecamp.org/whats-a-javascript-closure-in-plain-english-please-6a1fc1d2ff1c) [文章](https://en.wikipedia.org/wiki/Closure_%28computer_programming%29) 了，我在工作中使用了闭包，有时我甚至使用了闭包，但却没有意识到我正在使用闭包。

最近我去参加了一个讲座，有人以一种能让我终于明白的方式对它进行了真正的解释。在这篇文章中，我将尝试用这种方法来解释这所谓的闭包。让我把这个功劳归于[CodeSmith](https://www.codesmith.io/)和他们的JavaScript the Hard Parts系列

### ★开始之前

在你开始去理解闭包之前，有一些极其重要的概念得要你事先知晓的，比如执行上下文（ *execution context*）等等

[这篇文章](http://davidshariff.com/blog/what-is-the-execution-context-in-javascript/)对执行上下文有一个很好的入门，因此，你可以去看看。我引用了这篇文章的一些内容：

