Kotlin 中的 in 和 out 关键字
-------------

# 简介:
> 本篇文章主要介绍在 Kotlin 中的泛型中使用的关键字。在 Kotlin 中使用泛型你会注意到其中引入了 in 和 out，对于不熟悉的开发者来说可能有点难以理解。从形式上讲，这是一种定义逆变和协变的方式，这篇文章就来讲讲怎么来理解和记住它们。

# 目录:
[1.in](#1)

[2.out](#2)

[3.Invariant](#3)


# <span id = "1">**1.in**</span>

### out（协变）:

&ensp;&ensp;如果你的类是将泛型作为内部方法的返回，那么可以用 out。


```

interface Production<out T> {
    fun produce(): T
}


```


> 可以称其为 production class/interface，因为其主要是产生（produce）指定泛型对象。因此，可以这样来记：produce = output = out。



# <span id = "2">**2.out**</span>

### in（逆变）:

&ensp;&ensp;如果你的类是将泛型对象作为函数的参数，那么可以用 in。


```

interface Consumer<in T> {
    fun consume(item: T)
}


```


> 可以称其为 consumer class/interface，因为其主要是消费指定泛型对象。因此，可以这样来记：consume = input = in。

# <span id = "3">**3.Invariant**</span>


### Invariant(不变):

&ensp;&ensp;如果既将泛型作为函数参数，又将泛型作为函数的输出，那就既不用 in 或 out。


```

interface ProductionConsumer<T> {
    fun produce(): T
    fun consume(item: T)
}


```


