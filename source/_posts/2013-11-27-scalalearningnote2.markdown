---
layout: post
title: "Scala学习笔记（2）"
date: 2013-11-27 17:15
comments: true
categories: Scala
---

## 数组专题

无论是什么编程语言，数组无疑是最重要的数据结构之一，这节主要是关于scala中数组操作的用法和与其他语言的不同。

<!--more-->
---
Scala中，数组按照长度固定与否可以分为两类，定长的使用Array，长度有可能变化的可以使用ArrayBuffer。例如：

```scala
val nums = new Array[Int](10)
  // 包含10个Int对象的数组，所有元素初始化为null
val s = Array("Hello ","World")
  // 如果数组内的元素已经确定了个数和类型就不需要new，直接用等号连接即可
s(0) = "Goodbye "
  // 访问元素使用( )而不是[ ]
```

对于变长数组： 

```scala
import scala.collection.mutable.ArrayBuffer
val b = ArragBuffer[Int]()
  // 定义一个空的整形数组缓冲
b += (1,2,3,4)
  // 向缓冲区的尾部加入元素
b += Array(5,6,7)
  // 向缓冲区的尾部加入一个数组
b.trimEnd(5)
  // 移除缓冲区的最后五个元素
```

加入、删除等方法的名称与java的一样，这里不过多描述。需要提一下的是数组和数组缓冲是可以相互转化的。

```scala
b.toArray  // Array(1,2)
s.toBuffer // ArrayBuffer("Goodbye ", "World)
```

有一个方法比较有意思，原型是

```scala
def count(p:(A) => Boolean) : Int
```

一开始看字面意思还以为是计算数组中的元素个数，其实不是。这个方法接受一个返回值为Boolean型的函数（或许表达式更为合适）作为参数，作用是统计列表中有多少元素在应用该函数后返回true。比如说`a.count(_ > 0)`就用于统计数组a中有多少元素是正数。

Scala同样支持多维数组，定义方法为：

```scala
val matrix = Array.hi[Double](3,4)  // 三行四列
matrix(2)(3) = 12                   // 对第二行三列的元素赋值
```

有意思的是可以创建不规则的数组，每一行长度都不相同：

```scala
val triangle = new Array[Array[Int]](10)
for(i <- 0 until tiangle.lenght)
  triangle(i) = new Array[Int] (i + 1)
```
