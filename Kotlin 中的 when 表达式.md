Kotlin 中的 when 表达式
-------------

# 简介:
> 本篇文章简单介绍 kotlin语言中的 when 表达式的使用例子。

# 目录:
[1.什么是 when 表达式](#1)

[2.如何使用](#2)


# <span id = "1">**1.什么是 when 表达式**</span>

### 介绍:

&ensp;&ensp;其实 kotlin 中的 `when` 就是 java 的 `switch`，只不过它的写法要更简洁，功能更加强大。可以看作是 switch 的升级版。

> when是个具有返回值的表达式.

# <span id = "2">**2.如何使用**</span>


`基础使用`

```

when (x) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> { // 注意这个块
        print("x is neither 1 nor 2")
    }
}


```


`多个分支条件放在一起，用逗号分隔`

```

when (x) {
    0, 1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}


```


`用任意表达式（而不只是常量）作为分支条件`

```

when (x) {
    parseInt(s) -> print("s encodes x")
    else -> print("s does not encode x")
}


```


`检测一个值在（ in ）或者不在（ !in ）一个区间或者集合中`

```

when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}


```


`另一种可能性是检测一个值是（is）或者不是（!is）一个特定类型的值。注意：由于智能转换，你可以访问该类型的方法和属性而无需任何额外的检测。`

```

val hasPrefix = when(x) {
    is String -> x.startsWith("prefix")
    else -> false
}


```


`when也可以用来取代if-else-if链。如果不提供参数，所有的分支条件都是简单的布尔表达式，而当一个分支的条件为真时则执行该分支`

```

when {
    x.isOdd() -> print("x is odd")
    x.isEven() -> print("x is even")
    else -> print("x is funny")
}


```

