Kotlin 里的 Object 关键字
-------------

# 简介:
> 本篇文章主要简单介绍在 Kotlin 语法中的 object 关键字的用法。

# 目录:
[1.object 用法之一：单例](#1)

[2.object 用法之二：伴生对象](#2)

[3.object 用法之三：匿名内部类](#3)


# <span id = "1">**1.object 用法之一：单例**</span>

### 使用 object 创建一个单例类:

> 使用 object 代替 class 关键字。比 java 简介多了吧。


```

object DataRepository {
    var data = listOf(1,2,3)
}


```



# <span id = "2">**2.object 用法之二：伴生对象**</span>

### 什么是伴生对象:

&emsp;&emsp;对于对象声明，object 关键字后面必须有一个名字；而伴生对象的名称可以省略，这种情况下它的名称就是 Companion，也可以显式地有一个名字。
对于对象声明，在类内部可以声明多个；而伴生对象只能声明一个。
对于对象声明，获取它的成员必须是所在类的名字.对象声明的名字.成员名；而伴生对象则是所在类的名字.成员名即可。

> 类内部的对象声明可以用 companion 关键字标记，这就是伴生对象。伴生对象在一个类中，只能声明一个。一般用来存放工厂方法和静态成员。


### 如何声明伴生对象

```

class App : Application() {
    companion object {
        private var instance: Application? = null
        fun instance() = instance!!
    }

    override fun onCreate() {
        super.onCreate()
        instance = this
    }
}


//-------------------使用
 val instance = App.instance()

```



# <span id = "3">**3.object 用法之三：匿名内部类**</span>


> 创建继承某个（或某些）类型的匿名类的对象，这些类型可以是接口（以给 Button 设置点击事件为例）：


```

button.setOnClickListener(object: View.OnClickListener {
            override fun onClick(v: View?) {
                Log.d(MainActivity::class.java.simpleName, "click button")
            }
})



```

