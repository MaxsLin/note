# xcode利用target实现多版本管理

在实际开发过程中，会有很多相似版本的app开发，相似的app和主app绝大多数功能相同， 这个时候如何最有效的处理多版本app的开发？


1：评估具体差异后确定是否拆分project还是通过拆分target
2：对于绝大多数功能相似的情况，采用多Target编译方案来实现更有效率。
3：如果使用拆分project，需要选择大小合适的粒度去抽取app的公共的功能模块。


## 多target在不修改代码的情况下如何解决版本差异
>	思路主要是修改target的编译文件，同一个文件名去对应不同路径下的文件进行编译，这样做的好处就是无需改一行代码，通过设置target就可以做的不同target对应不同的代码和资源文件

### 修改资源文件引用


选择对应的target -> build phases -> copy bundle resouces 修改指定版本对应的图片路径。

通过这种方式能解决不同版本编译出来的appui界面对应的差异。

此外，修改还可以修改不同targets对应不同的图标icon和启动画面
````
1：Asset xcassets -> App Icons & launch Images -> New Ios App Icon 	
2：general -> App Icons & launch Images -> 选择对应的icons和启动images
````


### 修改源代码引用

选择对应的target -> build phases -> compile source 修改指定版本对应编译文件路径。

通过这种方式能解决不同版本编译出来的app代码的差异

也可以通过xcode属性菜单中选择文件所属的target，这种方式更快捷一些。


### 修改pch文件的引用
可以定义多个pch文件，并在target -> build setting ->  prefix header,修改不同target对应的pch文件路径


### 其他
此外还可以设置target对应不同的项目和库文件，启动前脚本等方式，实现版本差异


## 修改代码解决版本差异
>	这种方式解决不同target更直接,不用改动整个文件，只需要对应改动部分代码段就好了，我们利用xcode target中的preprocessor macros来实现.

选择对应的target -> build setting -> preprocessor macros，定义对应版本的宏，然后再代码中判断宏，处理不同代码逻辑。这种方式并不是常规做法，简单，最适合做开关控制。

### 例子

1：在preprocessor macros中添加一个键，IS_XG=1
2：在代码中判断是否有这个开关

````objc
//打印ccsc版本信息
#ifdef  IS_XG
    NSLog(@"ccsc星光版本");
#else
    NSLog(@"不是ccsc星光版本");
#endif
````



## 注意点

-	1.当你将新文件添加到项目时，别忘了同时选择多个Targets，保持代码在两个版本中同步。 
-	2.如果你使用Cocoapods，别忘了将新的target添加到podfile。你可以使用 link_with来指定多个targets。你可以进一步查询Cocoapods documentation了解更多细节。你的podfile看起来应该像这样

```` 
platform :ios, '7.0'  
workspace 'xxx'  
link_with 'target1', 'target2'  
pod 'xxx'  

````
