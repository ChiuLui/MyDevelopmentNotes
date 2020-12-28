Kotlin 顶层函数与扩展函数
-------------

# 简介:
> 本篇文章主要介绍 Kotlin 中的顶层函数顶层属性与拓展函数拓展属性。

# 目录:
[1.顶层函数](#1)

[2.顶层属性](#2)

[3.拓展函数](#3)

[4.扩展属性](#3)


# <span id = "1">**1.顶层函数**</span>



&ensp;&ensp;创建一个名为 Join.kt 的文件：

```

package strings
//一个简单的顶层函数
fun joinToString(...): String { ... }


```

&ensp;&ensp;他会被编译成以下 Java 代码

```

package strings;
public class JoinKt {
    public static String joinToString(...) { ... }
}


```

&ensp;&ensp;然后，在 Java 中可以按照如下方法使用它

```

import strings.JoinKt;
...
JoinKt.joinToString(list, ", ", "", "");

```

> Kotlin 顶层函数相当于 Java 中的静态函数，往往我们在 Java 中会用到类似 Utils 的类来放置不属于任何类的公共静态函数。



# <span id = "2">**2.顶层属性**</span>

&ensp;&ensp;一个简单的顶层属性

```

const val UNIX_LINE_SEPARATOR = "\n"


```

&ensp;&ensp;这个等同于下面的 Java 代码：

```

public static final String  UNIX_LINE_SEPARATOR = "\n";


```



# <span id = "3">**3.拓展函数**</span>



&ensp;&ensp;理论上来说，扩展函数非常简单，它就是一个类的成员函数，不过定义在类的外面。

```
//普通的拓展函数
fun String.lastChar(): Char = this.get(this.length - 1)

//接收者对象成员可以不用this来访问
fun String.lastChar(): Char = get(length - 1) 


```

&ensp;&ensp;使用

```

import strings.lastChar

//String就是接收者类型，而"Kotlin"就是接收者对象。
val c = "Kotlin".lastChar()


```


&ensp;&ensp;Java 中调用

```

/* Java */
char c = StringUtilKt.lastChar("Java");


```

##### 注意：
> 扩展函数并不存在重写，因为Kotlin会把它们当做静态函数对待。



# <span id = "4">**4.扩展属性**</span>


&ensp;&ensp;定义扩展属性，必须定义getter函数，它没有默认getter的实现。初始化也不可以：因为没有地方存储初始值


```

val String.lastChar: Char
    get() = get(length - 1)


```


&ensp;&ensp;声明一个可变的扩展属性


```

var StringBuilder.lastChar: Char
    get() = get(length - 1) //属性getter
    set(value: Char) {
        this.setCharAt(length - 1, value) //属性setter
    }


```



&ensp;&ensp;你可以像访问使用成员属性一样访问它：


```

>>> println("Kotlin".lastChar)
n
>>> val sb = StringBuilder("Kotlin?")
>>> sb.lastChar = '!'
>>> println(sb)
Kotlin!


```


##### 注意：

> 注意，当你需要从Java里面访问扩展属性的时候，你应该显式地调用它的getter函数：

```
StringUtilKt.getLastChar(new StringBuilder("Java"))

```

