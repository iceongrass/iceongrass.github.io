---
layout: post
title: "Scala很神奇"
date: 2013-11-11 01:25
comments: true
categories: Scala 
---

起初听到这个名词是在公司里，组内的技术大牛说这玩意是未来的语言。各种社区里也把scala吹的是神乎其神，可惜一整年忙于乱七八糟的事情一直没有机会深入了解。托csdn的福，没有让我赋闲在家的这段时间过于无聊，也开始接触了这门全新的语言。

<!--more-->

-----
## 嘛是Scala？

这玩意是跑在Java上的。代码可以与Java的代码无缝对接，语法格式某种程度上与python相似。这是一种类lisp的函数式语言，可以写的很像python和Java，也可以写的像楚留香……

如果你用Java写过代码，又了解些python方面的语法和特点，你看到用Scala写出的代码大概不会太难理解。废话少说，给个例子：

```scala
def printArgs(args:Array[String]) : Unit = {
  var i = 0;
  while(i < args.length) {
    println(args(i))
    i += 1
  }
}

printArgs(args);
```
代码本身很简单，就是接收参数，然后把参数中的string逐行打印。语法特点相信大家应该也注意到了，scala中变量的类型声明和函数返回值类型是通过：后跟变量类型的方式完成的。Coding的时候，一条语句时“；”可选，代码块的缩进默认为两个空格执行。代码风格上scala讲究极简，能省略的就省略。

其实这些都好理解，最大的不同大概在于函数定义的方式上吧。函数式编程的理念就是”函数是第一公民“。Java中一切都是对象，定义函数一般通过“方法”的形式定义在类中。所以Scala在对待函数的方式上更像python。区别在于，python的函数名就是个symbol，而scala中的“=”让函数名更像是一个容器，装函数的容器。当然，这么说有些不严密，因为python中也是有lamda的^_^

函数执行结果是这个样字的：
```bash
$ scala printArgs.scala hi , world ! 
hi
,
world
!
```
然而，我给出的这个例子并不是真正的scala式编程。函数式的编程风格一般不喜欢变量（var）而更倾向于常量（var）更多情况下，他们会这样写代码：
```scala
def printArgs(args:Array[String]) = args.foreach(println)

printArgs(args)
```
很飘逸，是不？一个变量没有，对象调用方法时println直接作为参数传入，要理解这个语句（如果您至少有个大学结业证的话）只要靠读的就好：把args中的每一个都用println搞一遍。

好吧，如果你觉得上面这个例子不够分量，那我们再来个稍微复杂些的例子。

-----
## 另一个例子

对于IT男来说，除了“hello world”和苍井空之外比较广为人知的，Fibonacci数列肯定算一个。相信大家入门时都写过这个算法，这里我给大家看看scala的实现。
```scala
def fib(n: Int) = n match {
  case 0 => 0
  case 1 => 1
  case _ => fib(n-1) + fib(n-2)
}
```
相信不用我做过多解释各位看官就能理解。然而这并不是一个好的实现，当n比较大的时候，第三个case将产生很深的调用栈，很容易产生栈溢出。比较好的解决方法当然是用尾递归，但是就需要对算法进行修改。
```scala
def fib(tmp1: Int, tmp2: Int, n: Int) = n match {
  case 0 => tmp1
  case 1 => tmp2
  case _ => fib(tmp2, tmp1+tmp2, n-1)
}

println(fib(0, 1, args(0).toInt))    
```
这显然不是段很好看的代码，而且平白无故的多引入了两个参数，语法也不足够的飘逸和简练。首先我得承认，我的修行不够，但从算法来看要实现的话肯定有更好的写法。不过Scala的文档中有一个通过不可变流（Immutable Stream）来生成斐波那契数列的方法倒是值得与大家分享一下。
```scala
def fibForm(a: Int, b: Int): Stream[Int] = a #:: fibForm(b, a+b)
println(fibForm(1, 1).take(args(0).toInt).last)
```
简单解释一下。关键部分是“#::“，这并不是一段注释，而是scala中的stream容器内定义的一个方法，其作用是向stream中追加一个元素。第二句的take方法把外部的参数传给fibForm(1, 1)，作用是按照fibForm定义的方式生成含有args(0).toInt个元素的stream，last方法则返回了stream的最后一个元素值。

这段代码其实先是按照传入的参数生成了一个相应数目个元素的数列，然后返回了最后一个元素值，如果只是求值的话空间上有些浪费。

作为初识的话，我觉得写到这里差不多够了。这门语言水很深，还是需要努力学习啊！
