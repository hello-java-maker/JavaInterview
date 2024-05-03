**[JavaInterview](https://github.com/OUYANGSIHAI/JavaInterview)** 是本人在备战春招及这几年学习的知识沉淀，这里面有很多都是自己的原创文章，同时，也有很多是本在备战春招的过程中觉得对面试特别有帮助的文章，**[JavaInterview](https://github.com/OUYANGSIHAI/JavaInterview)** 不一定可以帮助你进入到 BAT 等大厂，但是，如果你认真研究，仔细思考，我相信你也可以跟我一样幸运的进入到大厂。

本人经常在 CSDN 写博客，累计**原创博客 400+**，拥有**访问量251W+**，**CSDN 博客专家**，CSDN博客地址：[https://sihai.blog.csdn.net](https://sihai.blog.csdn.net)，春招目前拿到了大厂offer。

如果觉得有帮助，给个 **star** 好不好，哈哈（目前还不是很完善，后面会一一补充）。

**一起冲！！！**

👉 如果你不知道该学习什么的话，请看 [Java 学习线路图是怎样的？](https://zhuanlan.zhihu.com/p/392712685) （原创不易，欢迎点赞），这是 2021 最新最完善的 Java 学习路线！

👉 Java学习资源汇总（个人总结）

- Java基础到Java实战全套学习视频教程，包括多个企业级实战项目;

- 面试算法资料，这是总结的算法资料，学完基本可以应付80%大厂;

- 大厂面试资料，一年时间总结，覆盖Java所有技术点;

- 面试思维导图，手打总结;

👉 Java各种电子书：各种技术相关的电子书；

👉 Java面试思维导图（手打），我靠这些导图拿到了一线互联网公司的offer，关注公众号，回复：`思维导图`；


**划重点**：获取上面的资源，请关注我的公众号 `程序员的技术圈子`，**微信扫描下面二维码**，回复：`Java资料`，获取思维导图，绿色通道关注福利，等你拿。

<div align="center"> <img src="assets/程序员技术圈子.jpg" width = 40% align="center"> </div>



### 目录（ctrl + f 查找更香：不能点击的，还在写）

- [个人经验](#个人经验)
- [项目准备](#项目准备)
- [面试知识点](#面试知识点)
- [公司面经](#公司面经)
- [Java](#java)
  - [基础](#基础)
  - [容器（包括juc）](#容器包括juc)
    - [基础容器](#基础容器)
    - [阻塞容器](#阻塞容器)
  - [并发](#并发)
  - [JVM](#jvm)
  - [Java8](#java8)
- [计算机网络](#计算机网络)
- [计算机操作系统](#计算机操作系统)
- [Linux](#linux)
- [数据结构与算法](#数据结构与算法)
  - [数据结构](#数据结构)
  - [算法](#算法)
- [数据库](#数据库)
  - [MySQL](#mysql)
    - [MySQL（优化思路）](#mysql优化思路)
- [系统设计](#系统设计)
  - [秒杀系统相关](#秒杀系统相关)
  - [前后端分离](#前后端分离)
  - [单点登录](#单点登录)
  - [常用框架](#常用框架)
    - [Spring](#spring)
    - [SpringBoot](#springboot)
- [分布式](#分布式)
  - [dubbo](#dubbo)
  - [zookeeper](#zookeeper)
  - [RocketMQ](#rocketmq)
  - [RabbitMQ](#rabbitmq)
  - [kafka](#kafka)
  - [消息中间件](#消息中间件)
  - [redis](#redis)
  - [分布式系统](#分布式系统)
- [线上问题调优（虚拟机，tomcat）](#线上问题调优虚拟机tomcat)
- [面试指南](#面试指南)
- [工具](#工具)
  - [Git](#git)
  - [Docker](#docker)
- [其他](#其他)
  - [权限控制（设计、shiro）](#权限控制设计shiro)
- [Java学习资源](#java学习资源)
- [Java书籍推荐](#java书籍推荐)
- [实战项目推荐](#实战项目推荐)
- [程序人生](#程序人生)
- [说明](#说明)
  - [JavaInterview介绍](#javainterview介绍)
  - [关于转载](#关于转载)
  - [如何对该开源文档进行贡献](#如何对该开源文档进行贡献)
  - [为什么要做这个开源文档？](#为什么要做这个开源文档)
  - [投稿](#投稿)
  - [联系我](#联系我)
  - [公众号](#公众号)

## 个人经验

- [应届生如何准备校招，用我这一年的校招经历告诉你](https://sihai.blog.csdn.net/article/details/114258312?spm=1001.2014.3001.5502)
- [【大学到研究生自学Java的学习路线】这是一份最适合普通大众、非科班的路线，帮你快速找到一份满意的工作](https://sihai.blog.csdn.net/article/details/105964718?spm=1001.2014.3001.5502)
- [两个月的面试真实经历，告诉大家如何能够进入大厂工作？](https://sihai.blog.csdn.net/article/details/105807642)

## 项目准备

- [我的个人项目介绍模板](docs/interview/自我介绍和项目介绍.md)
- [本人面试两个月真实经历：面试了20家大厂之后，发现这样介绍项目经验，显得项目很牛逼！](https://sihai.blog.csdn.net/article/details/105854760)
- [项目必备知识及解决方案](docs/project/秒杀项目总结.md) 

## 面试知识点

- [各大公司面试知识点汇总](docs/interview-experience/各大公司面经.md) 
- [Java后端面试常见问题分类汇总（高频考点）](docs/interview-experience/面试常见问题分类汇总.md) 


## 公司面经

- [2020年各公司面试经验汇总](docs/interview-experience/各大公司面经.md) 
- [最新！！招银网络科技Java面经，整理附答案](https://mp.weixin.qq.com/s/HAUOH-EYS_3Ho2XxYkTGXA)
- [拿了 30K 的 offer！](https://mp.weixin.qq.com/s/R4gZ8IuskxgxA1SZwfCOoA)
- [重磅面经！！四面美团最终拿到了 offer](https://mp.weixin.qq.com/s/P1mDcH5hEXqNp2Jpz5Qjmg)
- [十面阿里，七面头条](https://mp.weixin.qq.com/s/FErQnLvYnuZxiaDkYWPO5A)

## Java

### 基础

这几篇文章虽然是基础，但是确实深入理解基础，如果你能很好的理解这些基础，那么对于Java基础面试题也是没有什么问题的，背面试题不如理解原理，很重要。

- [Java基础思维导图](http://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483823&idx=1&sn=4588a874055e8ca54f2bbe1ede12cff4&scene=19#wechat_redirect)
- [Java基础(一) 深入解析基本类型](http://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483948&idx=1&sn=cb0ae3d82a1629e3a0538b6f31e2473b&scene=19#wechat_redirect)
- [Java基础(二) 自增自减与贪心规则](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483951&idx=1&sn=af5b54ed2e26d975f96643d9dfd66fab&scene=19#wechat_redirect)
- [Java基础(三) 加强型for循环与Iterator](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483952&idx=1&sn=43130fdf815970e0e12347d057c6b24f&scene=19#wechat_redirect)
- [Java基础(四) java运算顺序的深入解析](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483955&idx=1&sn=abfb3e8ac31cb84bb78216d9c953abc0&scene=19#wechat_redirect)
- [Java基础(五) String性质深入解析](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483956&idx=1&sn=1c19164967621fa5449a7830d006c8f9&scene=19#wechat_redirect)
- [Java基础(六) switch语句的深入解析](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247483999&idx=1&sn=092ad983f87798360ef33b1485f3201b&scene=19#wechat_redirect)
- [Java基础(七) 深入解析java四种访问权限](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484000&idx=1&sn=0b188c70ac54c65a0419ab0d5da14af4&scene=19#wechat_redirect)
- [Java基础(八) 深入解析常量池与装拆箱机制](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484002&idx=1&sn=a1d9ec01c91537aca444408c989f5a50&scene=19#wechat_redirect)
- [Java基础(九) 可变参数列表介绍](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484003&idx=1&sn=84366ed430c332d4b8e2b2d6b54280f4&scene=19#wechat_redirect)
- [Java基础(十) 深入理解数组类型](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484004&idx=1&sn=9c58b6948f05bbeffea3552fec9ee9a6&scene=19#wechat_redirect)
- [Java基础(十一) 枚举类型](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484005&idx=1&sn=5aaec133dca189fcabc86defcd54c5b8&scene=19#wechat_redirect)
- [类与接口（二）java的四种内部类详解](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484075&idx=1&sn=e0fd37cc5c1eb5fb359ed3dc9c15af66&scene=19#wechat_redirect)
- [类与接口（三）java中的接口与嵌套接口](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484076&idx=1&sn=1903edbc469b2660e51e1154a8b63a27&scene=19#wechat_redirect)
- [类与接口（四）方法重载解析](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484078&idx=1&sn=db5f231dc64974057d4ee29af1649e8b&scene=19#wechat_redirect)
- [类与接口（五）java多态、方法重写、隐藏](https://mp.weixin.qq.com/s?__biz=MzI2OTQ4OTQ1NQ==&mid=2247484083&idx=1&sn=d5b3d1daca2eb4e8d9583d75e6f6ad6c&scene=19#wechat_redirect)


### 容器（包括juc）

#### 基础容器

- [ArrayList源码分析及真实大厂面试题精讲](https://blog.csdn.net/sihai12345/article/details/138413307?spm=1001.2014.3001.5501)
- [LinkedList源码分析及真实大厂面试题精讲](https://blog.csdn.net/sihai12345/article/details/138413722?spm=1001.2014.3001.5501)
- [HashMap源码分析及真实大厂面试题精讲](https://blog.csdn.net/sihai12345/article/details/138416578?spm=1001.2014.3001.5501)
- TreeMap源码分析及真实大厂面试题精讲
- TreeSet源码分析及真实大厂面试题精讲
- LinkedHashMap源码分析及真实大厂面试题精讲

#### 阻塞容器

- ConcurrentHashMap源码分析及真实大厂面试题精讲
- ArrayBlockingQueue源码分析及真实大厂面试题精讲
- LinkedBlockingQueue源码分析及真实大厂面试题精讲
- PriorityBlockingQueue源码分析及真实大厂面试题精讲

### 并发

- Synchronized关键字精讲及真实大厂面试题解析
- Volitale关键字精讲及真实大厂面试题解析
- 关于LRU的实现
- ThreadLocal面试中会怎么提问呢？
- 线程池的面试题，这篇文章帮你搞定它！


### JVM

- [深入理解Java虚拟机系列](https://mp.weixin.qq.com/s/SZ87s3fmKL3Kc_tAMcOFQw)
- [深入理解Java虚拟机系列--完全解决面试问题](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-xi-lie-jiao-cheng.html)
- [深入理解Java虚拟机-Java内存区域透彻分析](https://mp.weixin.qq.com/s/WuyxyelaXbU-lg-HVZ95TA)
- [深入理解Java虚拟机-JVM内存分配与回收策略原理，从此告别JVM内存分配文盲](https://mp.weixin.qq.com/s/IG_zU5xa7y4BB6PVP0Fmow)
- [深入理解Java虚拟机-常用vm参数分析](https://mp.weixin.qq.com/s/l8fsq07jI0svqBdBGxuOzA)
- [深入理解Java虚拟机-如何利用JDK自带的命令行工具监控上百万的高并发的虚拟机性能](https://mp.weixin.qq.com/s/wPgA5SDURCAqPsWkZGGX0g)
- [深入理解Java虚拟机-如何利用VisualVM对高并发项目进行性能分析](https://mp.weixin.qq.com/s/hhA9tI_rYNkJVbF-R45hbA)
- [深入理解Java虚拟机-你了解GC算法原理吗](https://mp.weixin.qq.com/s/SZ87s3fmKL3Kc_tAMcOFQw)
- [几个面试官常问的垃圾回收器，下次面试就拿这篇文章怼回去！](https://sihai.blog.csdn.net/article/details/105700527)
- [面试官100%会严刑拷打的 CMS 垃圾回收器，下次面试就拿这篇文章怼回去！](https://sihai.blog.csdn.net/article/details/105808878)
- [JVM 面试题 87 题详解](https://sihai.blog.csdn.net/article/details/118737581)


### Java8 

- [Java8 Stream：2万字20个实例，玩转集合的筛选、归约、分组、聚合](https://mp.weixin.qq.com/s/u042M2Sw2glBlevIDVoSXg)
- [利用Java8新特征，重构传统设计模式，你学会了吗？](https://mp.weixin.qq.com/s/zZ6rWz_t_snYNiNyOtaGiQ)
- [Java8 之 lambda 表达式、方法引用、函数式接口、默认方式、静态方法](https://mp.weixin.qq.com/s/FdzNWIsEmHVe9Nehxvfa3w)
- [Java8之Consumer、Supplier、Predicate和Function攻略](https://sihai.blog.csdn.net/article/details/98193777)
- [Java8 的 Stream 流式操作之王者归来](https://sihai.blog.csdn.net/article/details/100434684)
- [Java11-17的最新特性](https://mp.weixin.qq.com/s/QPGdNn56mCCDIUS047_1cQ)

## 计算机网络

- [http面试问题全解析](docs/network/http面试问题全解析.md)
- [计算机网络常见面试题](https://sihai.blog.csdn.net/article/details/118737663)
- 关于tcp、udp网络模型的问题，这篇文章告诉你
- http、https还不了解，别慌！
- 面试官问我计算机网络的问题，我一个问题给他讲半个小时

## 计算机操作系统

- [操作系统、计算机网络相关知识](docs/operating-system/操作系统、计算机网络相关知识.md)

## Linux

- [java工程师linux命令，这篇文章就够了](https://mp.weixin.qq.com/s/bj28tvF9TwgwrH65OPjXZg)
- [linux常见面试题（基础版）](https://sihai.blog.csdn.net/article/details/118737736)
- [linux高频面试题](docs/operating-system/linux高频面试题.md)
- 常问的几个Linux面试题，通通解决它

## 数据结构与算法

### 数据结构

- [跳表这种数据结构，你真的清楚吗，面试官可能会问这些问题！](https://blog.csdn.net/sihai12345/article/details/138419109)
- 红黑树你了解多少，不会肯定会被面试官怼坏
- [B树，B+树，你了解多少，面试官问那些问题？](https://segmentfault.com/a/1190000020416577)
- [这篇文章带你彻底理解红黑树](https://sihai.blog.csdn.net/article/details/118738496)
- 二叉树、二叉搜索树、二叉平衡树、红黑树、B树、B+树

### 算法

- [从大学入门到研究生拿大厂offer，必须看的数据结构与算法书籍推荐，不好不推荐！](https://sihai.blog.csdn.net/article/details/106011624?spm=1001.2014.3001.5502)
- [2021年面试高频算法题题解](docs/dataStructures-algorithms/高频算法题目总结.md) 
- [2021年最新剑指offer难题解析](docs/dataStructures-algorithms/剑指offer难点总结.md)  
- [关于贪心算法的leetcode题目，这篇文章可以帮你解决80%](https://blog.ouyangsihai.cn/jie-shao-yi-xia-guan-yu-leetcode-de-tan-xin-suan-fa-de-jie-ti-fang-fa.html)
- [dfs题目这样去接题，秒杀leetcode题目](https://sihai.blog.csdn.net/article/details/106895319)
- [回溯算法不会，这篇文章一定得看](https://sihai.blog.csdn.net/article/details/106993339)
- 动态规划你了解多少，我来帮你入个们
- 链表的题目真的不难，看了这篇文章你就知道有多简单了
- 还在怕二叉树的题目吗？
= 栈和队列的题目可以这样出题型，你掌握了吗
- 数组中常用的几种leetcode解题技巧！

## 数据库

### MySQL

- [InnoDB与MyISAM等存储引擎对比](https://sihai.blog.csdn.net/article/details/100832158)
- [MySQL：从B树到B+树到索引再到存储引擎](https://mp.weixin.qq.com/s/QmG1FyWPp23klTVkTJvcUQ)
- [MySQL全文索引最强教程](https://blog.ouyangsihai.cn/mysql-quan-wen-suo-yin.html)
- [MySQL的又一神器-锁，MySQL面试必备](https://sihai.blog.csdn.net/article/details/102680104)
- [MySQL事务，这篇文章就够了](https://sihai.blog.csdn.net/article/details/102815801)
- [mysqldump工具命令参数大全](https://blog.ouyangsihai.cn/mysqldump-gong-ju-ming-ling-can-shu-da-quan.html)
- [看完这篇MySQL备份的文章，再也不用担心删库跑路了](https://blog.ouyangsihai.cn/kan-wan-zhe-pian-mysql-bei-fen-de-wen-zhang-zai-ye-bu-yong-dan-xin-shan-ku-pao-lu-liao.html)
- 关于MySQL索引，面试中面试官会怎么为难你，一定得注意
- MySQL中的乐观锁、悲观锁，JDK中的乐观锁、悲观锁？ 

#### MySQL（优化思路）

- [MySQL高频面试题](https://mp.weixin.qq.com/s/KFCkvfF84l6Eu43CH_TmXA)
- [MySQL查询优化过程](https://mp.weixin.qq.com/s/jtuLb8uAIHJNvNpwcIZfpA)
- [面试官：MySQL 上亿大表，如何深度优化？](https://mp.weixin.qq.com/s/g-_Oz9CLJfBn_asJrzn6Yg)
- [老司机总结的12条 SQL 优化方案（非常实用）](https://mp.weixin.qq.com/s/7QuASKTpXOm54CgLiHqEJg)


## 系统设计

### 秒杀系统相关

- [分布式架构图](https://juejin.im/post/5a9ced09518825555f0c72c7)

- [秒杀架构设计思路详解](https://blog.csdn.net/qq_35190492/article/details/103105780)

- **[秒杀关键细节设计](https://blog.csdn.net/fanrenxiang/article/details/85083243)**

### 前后端分离

- [前后端分离概述](https://blog.csdn.net/fuzhongmin05/article/details/81591072)
- [前后端分离之JWT用户认证](https://www.jianshu.com/p/180a870a308a)

### 单点登录

- [什么是单点登录](https://mp.weixin.qq.com/s/J6YJls05t2C4OGOqHVijhw)
- [单点登录机制原理](https://mp.weixin.qq.com/s/LGnUueNC-EuoxiF-8b-TeQ)

### 常用框架

#### Spring

- [微信支付和支付宝支付到springmvc+spring+mybatis环境全过程（支付宝和微信支付）](https://mp.weixin.qq.com/s/c1GMlpRZNcZdsTUNuG8WUw)

#### SpringBoot

- [springboot史上最全教程，11篇文章全解析](https://blog.csdn.net/sihai12345/category_7779682.html)
- [微服务面试相关资料](docs/microservice/微服务相关资料.md)  

## 分布式

### dubbo

- [dubbo入门实战教程，这篇文章真的再好不过了](https://segmentfault.com/a/1190000019896723)
- [dubbo源码分析](http://cmsblogs.com/?p=5324)
- [dubbo面试题](https://mp.weixin.qq.com/s/PdWRHgm83XwPYP08KnkIsw)
- [dubbo面试题2](https://mp.weixin.qq.com/s/Kz0s9K3J9Lpvh37oP_CtCA)

### zookeeper

- [什么是zookeeper？](https://mp.weixin.qq.com/s/i2_c4A0146B7Ev8QnofbfQ)
- [zookeeeper面试题](https://segmentfault.com/a/1190000014479433)
- [zookeeper面试题2](https://juejin.im/post/5dbac7a0f265da4d2c5e9b3b)


### RocketMQ

- [RocketMQ简单教程](https://juejin.im/post/5af02571f265da0b9e64fcfd)
- [RocketMQ教程](https://mp.weixin.qq.com/s/VAZaU1DuKbpnaALjp_-9Qw)
- [RocketMQ面试题](https://blog.csdn.net/dingshuo168/article/details/102970988)

### RabbitMQ

- [RabbitMQ教程](https://blog.csdn.net/hellozpc/article/details/81436980)
- [RabbitMQ面试题](https://blog.csdn.net/qq_42629110/article/details/84965084)
- [RabbitMQ面试题2](https://my.oschina.net/u/4162503/blog/3073693)
- [RabbitMQ面试题3](https://blog.csdn.net/jerryDzan/article/details/89183625)

### kafka

- [全网最通俗易懂的Kafka入门](https://mp.weixin.qq.com/s/FlSsrzu1FwjBjmlNy5QyOg)
- [全网最通俗易懂的Kafka入门2](https://mp.weixin.qq.com/s/opAYVXIJoy4tCWaPcX5u6g)
- [kafka入门教程](https://www.orchome.com/kafka/index)
- [kafka面试题](https://blog.csdn.net/qq_28900249/article/details/90346599)
- [kafka面试题2](http://trumandu.github.io/2019/04/13/Kafka%E9%9D%A2%E8%AF%95%E9%A2%98%E4%B8%8E%E7%AD%94%E6%A1%88%E5%85%A8%E5%A5%97%E6%95%B4%E7%90%86/)

### 消息中间件

- [消息中间件面试题总结](docs/project/消息中间件面试题.md)  

### redis

- [Redis设计与实现总结文章](https://blog.csdn.net/qq_41594698/category_9067680.html)
- [Redis面试题必备：基础，面试题](https://mp.weixin.qq.com/s/3Fmv7h5p2QDtLxc9n1dp5A)
- [Redis面试相关：其中包含redis知识](https://blog.csdn.net/qq_35190492/article/details/103105780)
- [redis其他数据结构](https://blog.csdn.net/c_royi/article/details/82011208)

### 分布式系统

## 线上问题调优（虚拟机，tomcat）
- [垃圾收集器ZGC](https://juejin.im/post/5dc361d3f265da4d1f51c670)
- [jvm系列文章](https://crowhawk.github.io/tags/#JVM)
- [一次JVM FullGC的背后，竟隐藏着惊心动魄的线上生产事故！](https://mp.weixin.qq.com/s/5SeGxKtwp6KZhUKn8jXi6A)
- [深入理解Java虚拟机-如何利用JDK自带的命令行工具监控上百万的高并发的虚拟机性能](https://mp.weixin.qq.com/s/wPgA5SDURCAqPsWkZGGX0g)
- [深入理解Java虚拟机-如何利用VisualVM对高并发项目进行性能分析](https://mp.weixin.qq.com/s/hhA9tI_rYNkJVbF-R45hbA)
- [JVM性能调优](https://www.iteye.com/blog/uule-2114697)
- [百亿吞吐量服务的JVM性能调优实战](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484236&idx=1&sn=b9743b2d7436f84e4617ff34e07abdd8&chksm=96cd4300a1baca1635a137294bc93c518c033ce01f843c9e012a1454b9f3ea3158fa1412e9da&scene=27&ascene=0&devicetype=android-24&version=26060638&nettype=WIFI&abtest_cookie=BAABAAoACwASABMABAAjlx4AUJkeAFmZHgBomR4AAAA%3D&lang=zh_CN&pass_ticket=%2F%2BLqr9N2EZtrEGLFo9vLA6Eqs89DSJ2CBKoAJFZ%2BBngphEP28dwmMQeSZcUB77qZ&wx_header=1)
- [一次线上JVM调优实践，FullGC40次/天到10天一次的优化过程](https://blog.csdn.net/cml_blog/article/details/81057966)
- [JVM调优工具](https://www.jianshu.com/p/e36fac926539)


## 面试指南


## 工具

### Git

- [实际开发中的git命令大全](https://sihai.blog.csdn.net/article/details/106418135)

### Docker

## 其他

### 权限控制（设计、shiro）

- [权限控制设计](https://mp.weixin.qq.com/s/WTgz07xDIf9FbAfCDyTheQ)
- [shiro相关教程](https://blog.csdn.net/sihai12345/category_9268544.html)
- [springboot+vue+shiro前后端分离实战项目](https://blog.csdn.net/neuf_soleil/category_9287210.html)
- [shiro挺好的教程](https://how2j.cn/k/shiro/shiro-springboot/1728.html)

## Java学习资源

- [2021年Java视频学习教程+项目实战](https://github.com/hello-go-maker/cs-learn-source)
- [2021 Java 1000G 最新学习资源大汇总](https://mp.weixin.qq.com/s/I0jimqziHqRNaIy0kXRCnw)


## Java书籍推荐

- [从入门到拿大厂offer，必须看的数据结构与算法书籍推荐](https://blog.csdn.net/sihai12345/article/details/106011624)
- [全网最全电子书下载](https://github.com/hello-go-maker/cs-books)

## 实战项目推荐

>小心翼翼的告诉你，上面的资源当中就有很多**企业级项目**，没有项目一点不用怕，因为你看到了这个。

- [找工作，没有上的了台面的项目怎么办？](https://mp.weixin.qq.com/s/0oK43_z99pVY9dYVXyIeiw)
- [Java 实战项目推荐](https://github.com/hello-go-maker/cs-learn-source)

## 程序人生

- [我想是时候跟大学告别了](https://blog.csdn.net/sihai12345/article/details/86934341)
- [坚持，这两个字非常重要！](https://blog.csdn.net/sihai12345/article/details/89507366)
- [关于考研，这是我给大家的经验](https://blog.csdn.net/sihai12345/article/details/88548630)
- [从普通二本到研究生再到自媒体的年轻人，这是我的故事](https://segmentfault.com/a/1190000020317748)

## 说明

### JavaInterview介绍

目的很简单，写这些文章，以及整个github的资料，都是为了面试而准备的，只希望能够给面试提供一些帮助，都能够进大厂最好了。

### 关于转载

如果你需要转载本仓库的一些文章到自己的博客的话，记得注明原文地址就可以了。

### 如何对该开源文档进行贡献

1. 里面很多都是我的原创，能力有限，欢迎指正。
2. 很多知识点我可能没有涉及到，所以你可以对其他知识点进行补充。
3. 现有的知识点难免存在不完善或者错误，所以你可以对已有知识点进行修改/补充。

### 为什么要做这个开源文档？

初始想法源于自己的个人那一段比较迷茫的学习经历。主要目的是为了通过这个开源平台来帮助一些在学习 Java 或者面试过程中遇到问题的小伙伴。

### 投稿

由于我个人能力有限，很多知识点我可能没有涉及到，所以你可以对其他知识点进行补充。

### 联系我

添加我的微信备注 **github**, 即可入群。

<img src="assets/wx.jpg" width = 40%>

### 公众号

如果大家想要实时关注我更新的文章以及分享的干货的话，关注我的公众号 **程序员的技术圈子**。

<img src="assets/程序员技术圈子.jpg" width = 40%>


