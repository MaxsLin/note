# 动态规划

 定义：
 
 >  dynamic programming is a method for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions.

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

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-12ed8ae7365b25552f8a008b4e4321fc_hd.jpg)

他的最长上升子数列就是：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-9e83ad41dd80816154bdf46719055f26_hd.jpg)

[1,2,3,4], 长度为4，所以这个数列的最长上升子数列长度就是4。

对于这类问题我们如何求解呢？我们这次先用暴力来解一下试试，还是用上面那个数列作为例子：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-bb984c75b007b05ab627545e03bcbeed_hd.jpg)

古老的穷举法！直接这样找出所有的上升子序列，然后用肉眼观察哪个是最长的。显然，1,2,3,4是最长的，所以最长上升子序列的长度是4。

我们来看看这个方法的时间复杂度：

```
O(C_{n}^{1}+C_{n}^{2}+C_{n}^{3}+...+C_{n}^{n})=O(C_{n}^{n/2})=O(n!)
```

这就太消耗时间了。我们现在用动态规划试一下，看看有什么惊喜。

根据动态规划的定义，首先我们需要把原来的问题分解成了几个相似的子问题。但是，不同于斐波拉契数列的例子，这个如何分解原问题并不是那么一目了然。原来的问题是求LIS(n)，现在我们需要找的就是`LIS(n)`和`LIS(k)`之间的关系`1<=k<=n`。通过肉眼观察一下

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-84584196337e8f263e13fee544b80446_hd.jpg)

这里我们可以看到，`LIS(K+1)`要么等于`LIS(K)`，要么加了一。其实也很好理解，基本上就是，在前面所有的`LIS`种找到一个最长的`LIS(i)`，如果`A(K)`比这个找到`LIS(i)`的尾项`A(i)`要大，则`LIS(K)=LIS(i)+1`，否则`LIS(K)=LIS(i)`。这样的话，我们就分解了原问题，并且找到了原问题和子问题之间的关系：`i`是对应的最大`LIS(i)`。也就是说，计算`LIS(n)`需要计算之前所有的`LIS(K)`：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-bacaa422f39061568ea974476f0feddf_hd.jpg)

同理，我们可以储存子问题的结果，让每个子问题只被计算一次。需要计算的子问题就只剩下蓝色标出的部分了：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-6a662b1301a8ce9fdabdd638ec9c62fd_hd.jpg)

也就是（红色箭头表示调用了储存的数据，并未进行计算）：

![imaage](https://raw.githubusercontent.com/MaxsLin/note/master/image/v2-588b640eae534d034c8e310c34921b36_hd.jpg)

我们可以看到，采用了动态规划之后，时间复杂度大大降低了：纵轴方向的递归计算返回时间复杂度是`O(n)`，横轴方向每行求Max的时间复杂度是`O(logn)`，所以总共的时间复杂度就是`O(nlogn)`，远远小于暴力穷举法的`O(n!)`。屌的一匹！

这就是动态规划的意义！在某些类型的问题中，动态规划通过巧妙的分治和记录子问题的解，可以给原本时间开销巨大的问题提供时间效率上大大优化的解决方案。

动态规划的三个特点都很重要，少了任何一个，都会有可能导致动态规划发挥不了应有的威力。比如在第一个斐波拉契数列的例子中，如果我们只是分解成了相似的子问题，但是并不储存子问题的解，那么最后那个算法的时间复杂度其实是指数级别的O(2^n)，反而比直观解法更慢了。同样的，动态规划只能优化一部分问题。比如最长递增子序列的暴力解法VS动态规划解法是O(n!) vs O(nlogn)，有很强的优化效果，但是斐波拉契数列的暴力解法VS动态规划解法是O(n) vs O(n)，动态规划并不能起到优化的效果。




