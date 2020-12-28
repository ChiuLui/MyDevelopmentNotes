Kotlin 函数中的控制流
-------------

# 简介:
> 本篇文章主要介绍 Kotlin 语言中的利用标签去实现函数中的控制流。

# 目录:
[1.标签（label）](#1)

[2.标签处返回](#2)


# <span id = "1">**1.标签（label）**</span>

1. 在Kotlin中，任何表达式都可以用`标签（label）`来标记，标签格式位**标识符后跟@**；
2. 为一个**表达式添加标签**，只要在其**前面加标签即可**；
3. 可以使用标签**限制break或者continue，break跳转到标签指定的循环后面**的执行点，**continue继续标签指定的循环**的下一次迭代；

```

//1，2.为for循环添加标签loop@
loop@ for (i in 1..100) {
    // ……
}
//3.break跳转到@loop标签指定的循环后面的执行点
loop@ for (i in 1..100) {
    for (j in 1..100) {
        if (……) break@loop
    }
}

```


# <span id = "2">**2.标签处返回**</span>

1. 标签限制的return允许从外层函数返回；
2. 通常情况下使用隐士标签更方便，该标签和接受 lambda 的函数同名；
3. 可使用匿名函数替代 lambda 表达式，匿名函数内部return语句将从匿名函数本身返回；
4. 解析器优先选用标签限制的 return；


```

//1.return原本从直接包围它的函数foo返回，由于添加了@lit标签则它会从lambda表达式返回
fun foo() {
    ints.forEach lit@ {
        if (it == 0) return@lit
        print(it)
    }
}
//2.forEach为隐士标签，和forEach函数同名
fun foo() {
    ints.forEach {
        if (it == 0) return@forEach
        print(it)
    }
}
//3.匿名函数fun(value:Int)替代lambda表达式，return从该匿名函数返回
fun foo() {
    ints.forEach(fun(value: Int) {
        if (value == 0) return 
        print(value)
    })
}
//4.从标签@a返回1
return@a 1



```