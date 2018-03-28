# 动态规划

 定义：
 > dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions.

 Ref:  [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming)

 动态规划一定是具备了以下三个特点：
 1. 把原来的问题分解成了几个相似的子问题。（强调“相似子问题”）
 2. 所有的子问题都只需要解决一次。（强调“只解决一次”）
 3. 储存子问题的解。（强调“储存”）

 #### 什么是动态规划？

 我们来看看最经典的——斐波那契数列(Fibonacci)的例子！
 ```
 1， 1， 2， 3， 5， 8， 13 ，21 ...
 ```
 如果我们把第n个斐波拉契数记作 `Fibonacci(n)`，那么怎样利用动态规划来计算这个数呢？根据动态规划的三个特点，首先，我们需要把原问题分解成几个相似的子问题。这里还蛮清晰的啦，子问题就是这两个：`Fibonacci(n-1)` 和 `Fibonacci(n-2)`。

 而原问题和子问题之间的关系是：
```
Fibonacci (n) = Fibonacci (n-1) + Fibonacci (n-2)


(其中Fibonacci(0)=Fibonacci(1)=1)

```
假设我们现在需要计算`n=6`的时候，斐波拉契的值，那么我们就需要计算他的子问题`Fibonacci(5)`和`Fibonacci(4)`。同理对于`Fibonacci(5)`我们需要计算`Fibonacci(4)`和`Fibonacci(3)`，对于`Fibonacci(4)`我们需要计算`Fibonacci(3)`和`Fibonacci(2)`。这样的话，最后我们需要计算的东西如下图——由最顶向下不停的分解问题，最后往上返回结果。

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-82dbeb092f6723332e4fbe2ad773b16c_hd.jpg)

显然，这是一个非常低效的方法，因为其中有大量的重复计算。并且不满足动态规划的第二个特点：“所有的子问题都只需要解决一次“。这个问题很好解决，我们只需要利用动态规划的第三个特点——”储存子问题的解“就可以了。比如，如果已经计算过Fibonacci(3)，那么把结果储存起来，其他地方碰到需要求Fibonacci(3)的时候，就不需要计算，直接调用结果就行。这样做之后，蓝色表示需要进行计算的，白色表示可以直接从存储中取得结果的：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-eed6239406ba4b31ee3a3a448e79817c_hd.jpg)

也就是如下的计算（红色箭头表示调用了储存的数据，并未进行计算）：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-e6134d1195f7413d56b060462be26c75_hd.jpg)

看到这里，你可能会问这样一个问题，如果要计算Fibonacci(6)的话，何必这样大费周章的又是分解问题，又是储存的呢？直接这样从Fibonacci(0)和Fibonacci(1)开始一直往上一步步加就是了啊，看：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-fc200ad514b996308a9671e3624f4c5f_hd.jpg)
又直观又简洁，为啥要用动态规划呢？又要分解问题又要储存啥的。似乎看起来，动态规划的作用并不大的样子。

#### 动态规划的意义是什么？
对于上面斐波拉契数列的例子，其实还真就是这样。动态规划至顶向下的算法和暴力的从底向上的算法其实没有太大区别。

我们再来看另外一个列子：求一个数列中最长上升子数列的长度（LIS）的问题。

比如，给一个数列：
