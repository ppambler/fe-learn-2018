# Local vs Global Scope

一个非常重要的概念——作用域

1. 定义一个const message，初始化为「hi」，然后于console处log出来，看看会发生什么？

   ![1538107856690](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/1538107856690.png)

   ```js
   const message = 'hi';
   console.log(message);
   ```

   显然我们会log出hi这个值！

   那么问题来了，如果我把这个message扔到一对花括号（code block）里面去呢？你猜会发什么？

   ![1538108165827](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/1538108165827.png)

   ```js
   {
   	const message = 'hi';    
   }
   console.log(message);
   ```

   报错啊！大佬！

   ![1538108151443](C:/Users/Administrator/AppData/Roaming/Typora/typora-user-images/1538108151443.png)
