Kotlin不安全和安全类型转换操作符
-------------

# 简介:
> 本篇文章简单介绍在 Kotlin 语言中的转换类型操作符。

# 目录:
[1.不安全的类型转换操作符：as](#1)

[2.不安全的转换操作符：as?](#2)


# <span id = "1">**1.不安全的类型转换操作符：as**</span>

### as 转换符:

> 有时无法转换变量并抛出异常，这称为不安全转换。 不安全的强制转换由中缀运算符执行。可以为空的字符串(String?)不能转换为非null字符串(String)，这会引发异常。


```

un main(args: Array<String>){
    val obj: Any? = null
    val str: String = obj as String
    println(str)
}


```

&ensp;&ensp; 以上程序抛出异常：


```

Exception in thread "main" kotlin.TypeCastException: null cannot be cast to non-null type kotlin.String  
 at TestKt.main(Test.kt:3)


```

&ensp;&ensp;尝试将Any类型的整数值转换为字符串类型导致生成ClassCastException。


```

val obj: Any = 123  
val str: String = obj as String   
// Throws java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String


```


&ensp;&ensp;源和目标变量需要可以为转换工作：



```

fun main(args: Array<String>){
    val obj: String? = "String unsafe cast"
    val str: String? = obj as String? // Works  
    println(str)
}


```


&ensp;&ensp;执行上面示例代码，得到以下结果 - 


```
String unsafe cast

```


# <span id = "2">**2.不安全的转换操作符：as?**</span>

### as? 转换符:

> Kotlin提供安全转换操作符：as?安全地转换成一种类型。 如果无法进行转换，则返回null，而不是抛出ClassCastException异常。


&ensp;&ensp;让我们看一个例子，尝试转换任何类型的字符串值，程序员最初不知道编译器为可null string类型和可null的int类型。 如果可能的话，它会抛出值，或者返回null而不是抛出异常，甚至无法进行强制转换。

```

fun main(args: Array<String>){

    val location: Any = "Kotlin"
    val safeString: String? = location as? String
    val safeInt: Int? = location as? Int
    println(safeString)
    println(safeInt)
}

```


&ensp;&ensp;执行上面示例代码，得到以下结果 - 

```
Kotlin
null

```

