kotlin 中的成员引用
-------------

# 简介:
> 本篇文章简单介绍 Kotlin 中的成员引用。

# 目录:
[1.什么是成员引用](#1)

[2.成员引用的几种方式](#2)


# <span id = "1">**1.什么是成员引用**</span>

### 介绍:

&ensp;&ensp;如果你想要当做参数传递的代码已经被定义成了函数，那么你可以将这个函数转换成值，如下使用::运算符来转换


```
val getAge = Person::age

```

&ensp;&ensp;这种表达式被称为成员引用，它提供简明语法，来创建一个调用单个方法或这访问单个属性的函数值。双冒号把类名称和你要引用的成员名称隔开

![](https://upload-images.jianshu.io/upload_images/2349677-d33319b76370bf2f.png?imageMogr2/auto-orient/strip|imageView2/2)


```
//普通 lambda 表达式写法
val getAge = { person:Person -> person.age }
//成员引用写法
list.maxBy(Person::age)

```


# <span id = "2">**2.成员引用的几种方式**</span>

### 几种写法:

- 成员引用和调用函数的lambda具有一样的类型，所以可以相互转换

```
list.maxBy(Person::age)

```


- 还可以引用顶层函数，这种情况省略了类名称，直接以::开头。成员引用::salute被当作实参传递给库函数run

```
fun salute() = println("Salute!")
run (::salute)
Salute!

```


- 如果lambda要委托给一个接收多个参数的函数，提供成员引用代替它将会非常方便

```
val action = {person:Person,message:String -> sendEmail(person,message)}

val nextAction = ::sendEmail
调用
nextAction(...,...)

```


- 可以用构造方法引用存储或者延期执行创建类实例的动作，构造方法的引用方式是在双冒号后指定类名称

```
val createPerson = ::Person <!--创建`Person`实例的动作被保存成了值-->
val person = createPerson("kdp",25)
println(person)

```

- 还可以使用同样的方式引用扩展函数

```
fun Person.isAdult() = age >= 21
val predicate = Person::isAdult
```



