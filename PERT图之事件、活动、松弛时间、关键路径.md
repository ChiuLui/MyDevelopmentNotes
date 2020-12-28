PERT图之事件、活动、松弛时间、关键路径
-------------

# 简介:
> 本篇文章简单介绍 PERT 图的计算，篇幅较短。

# 目录:
[1.介绍](#1)

[2.公式](#2)

[3.例题](#3)


# <span id = "1">**1.介绍**</span>


> PERT（Program/Project Evaluation and Review Technique）即计划评审技术，PERT是利用网络分析制定计划以及对计划予以评价的技术。


# <span id = "2">**2.公式**</span>

### 四个概念:

> 构造PERT图，需要明确四个概念：事件、活动、松弛时间和关键路线。

- 事件（Events）表示主要活动结束的那一点；
- 活动（Activities）表示从一个事件到另一个事件之间的过程；
- 松弛时间（slack time）不影响完工前提下可能被推迟完成的最大时间；
- 关键路线（Critical Path）是PERT网络中花费时间最长的事件和活动的序列。


### 计算公式:

- 关键路径：

> 从开始到结束得所有路径中，所话时间最长的一条为关键路径。(在关键路径上的任务的松弛时间为0 )


- 最早开始时间：

> 在关键路径上，从开始到该任务的最早执行的时间


- 最晚开始时间：

> 关键路径的总时间-反向得出该任务的时间


- 松弛时间

>1. 第一种求法：最晚开始时间-最早开始时间 
2. 第二种求法：关键路径的总时间-包含该任务的关键路径花的时间



# <span id = "3">**3.例题**</span>


![](https://img-blog.csdnimg.cn/20190428165906291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2hhbmxlaWxpeGlu,size_16,color_FFFFFF,t_70)

![](https://img-blog.csdnimg.cn/20190428170003707.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpc2hhbmxlaWxpeGlu,size_16,color_FFFFFF,t_70)

答案为：D C

