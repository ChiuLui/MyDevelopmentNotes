Java 反射的简单介绍
-------------

# 简介:
> 本篇文章简单介绍 Java 中反射的简单使用。

# 目录:
[1.什么是反射](#1)

[2.反射的简单使用](#2)


# <span id = "1">**1.什么是反射**</span>

### 简介:

> 反射机制是在运行状态中对任意一个类都能知道这个类的所有属性和方法，对任意一个对象都能调用它的任意一个方法和属性。


### 反射的作用:

- 反编译: .class -> .java
- 通过反射机制访问 java 对象中的属性、方法、构造方法等。


### 反射机制需要用到的类
- `java.lang.Class` 类的创造
- `java.lang.reflect.Constructor` 反射类中构造方法
- `java.lang.reflect.Field` 反射属性
- `java.lang.reflect.Method` 反射方法
- `java.lang.reflect.Modifier` 访问修饰符的信息

# <span id = "2">**2.反射的简单使用**</span>

### 获取 Class 对象的三种方法

> 当一个类被加载后 Java 虚拟机会自动产生一个实现反射需要用到的 Class 对象，使用的是 `java.lang.Class`  这个类。

先定义一个简单的类
```
package testjava;

public class Bean {
	private String name;

	public Bean() {

	}

	public Bean(String name) {
		this.name = name;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

}
```

1. 包名加类名获取
```
  Class c1 = Class.forName("testjava.Bean");
```

2. 通过类获取
```
  Class c2 = Bean.class;
```

3. 通多对象获取
```
  Class c3 = new Bean().getClass();
```

### 创建对象的两种方法

1. 无参创建对象
```
			Class c1 = Class.forName("testjava.Bean");
			Object o1 = c1.newInstance();
```

2. 有参创建对象
```
			Class c2 = Bean.class;
			Constructor<?> csr = c2.getConstructor(String.class);
```

### 获取反射类的属性

> 在安全管理器中会使用 checkPermission 方法来检查权限，调用 `Field` 对象的 `setAccessible(true)` 方法能取消 Java 的权限控制检查。

```
			Class c2 = Bean.class;
			// 有参创建对象
			Constructor<?> csr = c2.getConstructor(String.class);
			Object o2 = csr.newInstance("对象2");
			Bean bean2 = (Bean)o2;
			System.out.println(bean2.getName());
			
			// 反射实例中的属性
			Field field = c2.getDeclaredField("name");
			// 取消封装，取消私有字段的访问限制
			field.setAccessible(true);
			field.set(bean2, "通过修改静态属性后的对象2");
			System.out.println(bean2.getName());
```

运行结果
> 对象2
> 通过修改静态属性后的对象2

### 获取属性中的修饰符

```
			Class c2 = Bean.class;
			// 反射实例中的属性
			Field field = c2.getDeclaredField("name");
			// 修改属性中的修饰符
			String priv = Modifier.toString(field.getModifiers());
			System.out.println("name 属性的修饰符：" + priv);
```

运行结果
> name 属性的修饰符：private

### 调用反射中的方法

```
			Class c2 = Bean.class;
			Constructor<?> csr = c2.getConstructor(String.class);
			Bean bean2 = (Bean) csr.newInstance("对象2");
			// 反射类中的方法
			Method m = c2.getDeclaredMethod("setName", String.class);
			m.invoke(bean2, "通过反射方法修改后的对象2");
			System.out.println(bean2.getName());
```

运行结果
> 通过反射方法修改后的对象2

