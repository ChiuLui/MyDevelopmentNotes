14 种 UML 图的汇总
-------------

# 简介:
> 本篇文章主要汇总了 UML 图的类型及每个图的介绍。

# 目录:
[1.什么是 UML 图](#1)

[2.UML 图的作用](#2)

[3.UML 图的介绍](#3)


# <span id = "1">**1.什么是 UML 图**</span>

### 介绍:

&ensp;&ensp;UML是在开发阶段，说明、可视化、构建和书写一个面向对象软件密集系统的制品的开放方法。最佳的应用是工程实践，对大规模，复杂系统进行建模方面，特别是在软件架构层次，已经被验证有效。统一建模语言（UML）是一种模型化语言。模型大多以图表的方式表现出来。一份典型的建模图表通常包含几个块或框，连接线和作为模型附加信息之用的文本。这些虽简单却非常重要，在UML规则中相互联系和扩展。

> `UML-Unified Modeling Language` 统一建模语言，又称标准建模语言。是用来对软件密集系统进行可视化建模的一种语言。UML的定义包括UML语义和UML表示法两个元素。


# <span id = "2">**2.UML 图的作用**</span>

### 作用:

- UML的目标是以面向对象图的方式来描述任何类型的系统，具有很宽的应用领域。

- 其中最常用的是建立软件系统的模型，但它同样可以用于描述非软件领域的系统，如机械系统、企业机构或业务过程，以及处理复杂数据的信息系统、具有实时要求的工业系统或工业过程等。

> UML是一个通用的标准建模语言，可以对任何具有静态结构和动态行为的系统进行建模，而且适用于系统开发的不同阶段，从需求规格描述直至系统完成后的测试和维护。


### 特点：

1. UML统一了各种方法对不同类型的系统、不同开发阶段以及不同内部概念的不同观点，从而有效的消除了各种建模语言之间不必要的差异。它实际上是一种通用的建模语言，可以为许多面向对象建模方法的用户广泛使用。

2. UML建模能力比其它面向对象建模方法更强。它不仅适合于一般系统的开发，而且对并行、分布式系统的建模尤为适宜。

3. UML是一种建模语言，而不是一个开发过程。


# <span id = "3">**3.UML 图的介绍**</span>


### 用例图
> 从用户的角度提供系统或业务流程功能的概述。用户“使用”系统的方式是创建用例图的起点。

![用例图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570163762776&di=afa316c6052691faa6089a58e4edd766&imgtype=jpg&src=http%3A%2F%2Fimg2.imgtn.bdimg.com%2Fit%2Fu%3D1162707267%2C3392473697%26fm%3D214%26gp%3D0.jpg)



### 活动图
> 对系统中任何位置的流进行建模。特别是，描述正常用户交互以及替代和例外的用例中的流程由这些活动图很好地建模。

![活动图](https://img-blog.csdnimg.cn/20181031221400889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcHB5ZnJlZWFuZ2Vs,size_16,color_FFFFFF,t_70)


### 类图
> 类图表示类，它们的定义和关系 问题空间中的类和实体也是解决方案空间中的详细技术实体。 定义类的属性和操作包含在此类图中。类图中的关系说明了类如何与其他类交互，协作和继承。类还可以表示关系表，用户界面和控制器。

![类图](https://img-blog.csdnimg.cn/20181031221158957.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcHB5ZnJlZWFuZ2Vs,size_16,color_FFFFFF,t_70)


### 序列图
> 序列图根据对象的时间轴模拟对象之间的交互。对象可以在这些图上具体显示，也可以是属于类的匿名对象。运行时对象之间的消息执行顺序由这些图很好地建模，因此它们的名称为

![序列图](https://img-blog.csdnimg.cn/20181031221544989.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hhcHB5ZnJlZWFuZ2Vs,size_16,color_FFFFFF,t_70)


### 交互概述图
> 交互概述图 以一般的高级别呈现系统内交互的概述;它还使得能够理解UML图（例如，序列图）如何依赖于彼此并且彼此相关。

![交互概述图](https://images.cnblogs.com/cnblogs_com/TerryFeng/WindowsLiveWriter/UML_12E98/image_thumb_1.png)


### 通信图
> 通信图显示对象在运行时如何在内存中相互通信（交互）。这些通信图在其目的方面类似于序列图;但是，他们的代表性是不同的。

![通信图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164195577&di=defb27b877520caac51a6687cfeb1819&imgtype=jpg&src=http%3A%2F%2Fimg2.imgtn.bdimg.com%2Fit%2Fu%3D3565329947%2C1433634348%26fm%3D214%26gp%3D0.jpg)


### 对象图
> 对象图 在运行时显示内存中的对象及其链接。 因此，这些对象图还有助于在实践中可视化多重性。

![对象图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164319850&di=a762b45bcc21d8632347afed7e089530&imgtype=0&src=http%3A%2F%2Fimg.it610.com%2Fimage%2Finfo5%2F366da545310e479c92c1d96910d143aa.gif)


### 状态机图
> 状态机图 显示内存中对象的运行时生命周期。这样的生命周期包括对象的所有状态以及状态改变的条件。

![状态机图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164452874&di=81ebb744814f96b4b58260ba1d38eb6a&imgtype=jpg&src=http%3A%2F%2Fimg1.imgtn.bdimg.com%2Fit%2Fu%3D430803921%2C1875391128%26fm%3D214%26gp%3D0.jpg)


### 组合结构图
> 组合结构图 在运行时模拟组件或对象行为，显示系统执行期间组件的布局，关系和实例

![组合结构图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164504751&di=8bb68f5771932492d371f38a781a5cc1&imgtype=jpg&src=http%3A%2F%2Fimg3.imgtn.bdimg.com%2Fit%2Fu%3D2817656843%2C730071646%26fm%3D214%26gp%3D0.jpg)


### 组件图
> 组件图 从结构上模拟组件及其关系。 这些组件可以包括例如可执行文件，可链接库，Web服务和移动服务。这些图表为系统的架构决策增加了价值。

![组件图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164836176&di=e14c7851232f1950348875f72eecbff5&imgtype=0&src=http%3A%2F%2Fp0.so.qhimgs1.com%2Ft01d3931149ae6c1d70.jpg)


### 部署图
> 部署图对系统的硬件节点和处理器的体系结构进行建模，并提供显示软件组件所在节点的机会。

![部署图](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3438103429,192201120&fm=26&gp=0.jpg)


### 包图
> 包图 表示系统组织的子系统和区域。它还可以模拟包之间的依赖关系，并帮助将业务实体与用户界面，数据库，安全性和管理包分开。

![包图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164929067&di=1d446eadf24948ed97490c779a1f1c30&imgtype=jpg&src=http%3A%2F%2Fimg4.imgtn.bdimg.com%2Fit%2Fu%3D1376025934%2C1168559317%26fm%3D214%26gp%3D0.jpg)



### 时序图
> 时序图模拟时间的概念以及对象状态随时间变化的方式。此外，这些图可以同时比较多个对象的状态。

![时序图](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1570164991938&di=cee27e117acb57cfb7c140d456be9e61&imgtype=0&src=http%3A%2F%2Fwww.itcast.cn%2Ffiles%2Fimage%2F201812%2F20181207142722136.jpg)



### 配置图
> 配置文件图 允许创建可扩展的配置文件，这些配置文件可应用于从配置文件继承的元素。这些图表通过以受控方式扩展标准来增加价值

![配置图](https://img-blog.csdnimg.cn/20181107111004647.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3l5cDAzMDREZXZpbg==,size_16,color_FFFFFF,t_70)



