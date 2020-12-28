Kotlin 中的构造方法
-------------

# 简介:
> 本篇文章主要介绍 kotlin 语言中的构造方法怎么实现。

# 目录:
[1.Kotlin 构造函数与 Java 的区别](#1)

[2.主构造函数](#2)

[3.从构造函数](#3)


# <span id = "1">**1.Kotlin 构造函数与 Java 的区别**</span>

### 区别:

##### Java 中：

&ensp;&ensp;Java 中一个类可以有一个或多个构造函数，它们是同级的，由创建时指定使用哪个构造函数初始化对象。


##### Kotlin 中：

&ensp;&ensp;其中一个类可以有一个主构造函数和多个从构造函数，主构造函数中不能包含代码，初始化代码应写在 init{} 代码块中。如果一个类有主构造函数，那么从构造函数都要直接或间接代理到这个主构造函数。如果使用从构造函数初始化新对象，会先调用主构造函数，再初始化类中声明的直接赋值的属性和 init{} 代码块，最后执行指定的从构造函数中的代码。

> Kotlin 中构造函数分为`主构造函数`和`从构造函数`。


# <span id = "2">**2.主构造函数**</span>

### 写法一：

- 关键字constructor：
> 在Java中，构造方法名须和类名相同；而在Kotlin中，是通过constructor关键字来标明的，且对于Primary Constructor而言，它的位置是在类的首部（class header）而不是在类体中（class body）。
    
- 关键字init：
> init{}它被称作是初始化代码块（Initializer Block），它的作用是为了Primary Constructor服务的，由于Primary Constructor是放置在类的首部，是不能包含任何初始化执行语句的，这是语法规定的，那么这个时候就有了init的用武之地，我们可以把初始化执行语句放置在此处，为属性进行赋值。


```

class Person constructor(username: String, age: Int){
	private val username: String
	private var age: Int

	init{
		this.username = username
		this.age = age
	}
}


```


### 写法二：

> 当 `constructor` 关键字没有注解和可见性修饰符作用于它时，constructor 关键字可以省略（当然，如果有这些修饰时，是不能够省略的，并且 constructor 关键字位于修饰符后面）。

```

class Person (username: String, age: Int){
    private val username: String
    private var age: Int

    init{
	this.username = username
	this.age = age
    }
}


```


### 写法三：

> 这种在构造器中声明形参，然后在属性定义进行赋值，这个过程实际上很繁琐，有没有更加简便的方法呢？当然有，我们可以直接在 Primary Constructor 中定义类的属性。


```

class Person(private val username: String, private var age: Int)


```


# <span id = "3">**3.从构造函数**</span>

> 和 Primary Constructor相比，很明显的一点，Secondary Constructor是定义在类体中。第二，Secondary Constructor可以有多个，而Primary Constructor只会有一个。

```

class User{
	private val username: String
	private var age: Int
		
	constructor(username: String, age: Int){
	    this.username = username
	    this.age = age
	}
}


```

