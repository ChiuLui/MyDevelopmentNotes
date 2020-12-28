Kotlin 常用关键字
-------------

# 简介:
> 本篇文章简单介绍 Kotlin中常用的关键字。

# 目录:
[1.object](#1)

[2.lazy 、lateinit](#2)

[3.when](#3)

[4.let](#4)

[5.apply](#5)

[6.with](#6)


# <span id = "1">**1.object**</span>

> object 用于创建单例模式

```

object Resource {
    val name = "Rocker"
}



```


# <span id = "2">**2.lazy 、lateinit**</span>


> lazy 、lateinit 用于延迟初始化，第一次使用时再实例化

```

val name: String by lazy {
    "Rocker"
}

lateinit var name:String;
fun testName(){
  name = "Rocker"
}
两者区别：
by lazy  修饰val的变量
lateinit 修饰var的变量，且变量是非空的类型



```



# <span id = "3">**3.when**</span>



> when 用于判断 相当于java中的switch()语句

```

fun getName(age: Int) = when (age) {
        12 -> "jim"
        18 -> "lili"
        else -> "Rocker"
 }



```

# <span id = "4">**4. let**</span>


> let 默认当前这个对象作为闭包的it参数，返回值是函数里面最后一行

```

fun testLet(){
       "Rocker".let {
           print(it)
           print(it)
       }
   }



```


# <span id = "5">**5. apply**</span>

> apply 调用对象的apply函数，在函数范围内，可以任意调用该对象的任意方法，并返回该对象

```

 fun testApply(){
        mutableListOf<String>().apply {
            add("jim")
            add("lili")
            add("Rocker")
        }.add("To test whether returns this object")
  }



```


# <span id = "6">**6. with**</span>


> with函数是一个单独的函数，返回是最后一行，可以直接调用对象的方法，感觉像是let和apply的结合。

```

 fun testWith(){
        with(mutableListOf<String>()){
            this.add("jim")
            add("lili")
            add("Rocker")
            this
        }.add("To test whether returns this object")
  }



```