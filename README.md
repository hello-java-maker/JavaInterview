**[JavaInterview](https://github.com/OUYANGSIHAI/JavaInterview)** 是本人在备战春招及这几年学习的知识沉淀，这里面有很多都是自己的原创文章，同时，也有很多是本在备战春招的过程中觉得对面试特别有帮助的文章，**[JavaInterview](https://github.com/OUYANGSIHAI/JavaInterview)** 不一定可以帮助你进入到 BAT 等大厂，但是，如果你认真研究，仔细思考，我相信你也可以跟我一样幸运的进入到大厂。

本人经常在 CSDN 写博客，累计**原创博客 400+**，拥有**访问量160W**，目前是 **CSDN 博客专家**，春招目前拿到了大厂offer。

如果觉得有帮助，给个 **star** 好不好，哈哈（目前还不是很完善，后面会一一补充）。

**JavaInterview 最新地址**：https://github.com/OUYANGSIHAI/JavaInterview

**一起冲！！！**

#### Java面试思维导图预警

这里再分享一些我总结的**Java面试思维导图**，我靠这些导图拿到了一线互联网公司的offer，先来瞧瞧。

![](http://image.ouyangsihai.cn/FtJ3PbBRdNSa1NaUr96JmUq24_yS)
![](http://image.ouyangsihai.cn/FsTv2aMsGcv8M9Rsyx6POBOGKY1g)


**划重点**：想要`导图源文件`，更多`Java面试思维导图`，请关注我的公众号 **程序员的技术圈子**，`微信扫描下面二维码`，回复：**思维导图**，获取思维导图，绿色通道关注福利，等你拿。

![](http://image.ouyangsihai.cn/FlL0VJf1Q4gCfrc8RhL-SL-xiiXo)

<a href="https://github.com/OUYANGSIHAI/JavaInterview" target="_blank">
	<div align="center"> <img src="http://image.ouyangsihai.cn/FvyAZ0ZEHmgrETnvhsJDHioiqs9e" width = 500 height = 300 /> </div>
</a>

<div align="center">

[![微信群](https://camo.githubusercontent.com/59d7f19ba1af85247e016858a63045f8fe9a8c19/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f7765436861742de5beaee4bfa1e7bea42d626c75652e737667)](https://github.com/OUYANGSIHAI/JavaInterview#%E8%81%94%E7%B3%BB%E6%88%91) [![公众号](https://img.shields.io/badge/%E5%85%AC%E4%BC%97%E5%8F%B7-%E5%A5%BD%E5%A5%BD%E5%AD%A6java-orange)](https://github.com/OUYANGSIHAI/JavaInterview#%E5%85%AC%E4%BC%97%E5%8F%B7) [![公众号](https://camo.githubusercontent.com/6d206aa03f27a851cf994123ef7be1a8d3192d54/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6a75656a696e2de68e98e987912d626c75652e737667)](https://juejin.im/user/5a672822f265da3e55380f0b) [![投稿](https://camo.githubusercontent.com/85a04ac4953a80940570b5c86ce73a1d34ff1542/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6373646e2d4353444e2d7265642e737667)](https://blog.csdn.net/sihai12345) [![投稿](https://camo.githubusercontent.com/6efc9c83ef8e85b19ce2853b5f69d68255f0c037/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f62696c6962696c692de59394e593a9e59394e593a92d637269746963616c)](https://space.bilibili.com/441147490) 
</div>

### 目录（ctrl + f 查找更香：不能点击的，还在写）

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


## 项目准备

- [如何进行项目的自我介绍呢？](docs/interview/自我介绍和项目介绍.md)

- [项目需要准备必备知识及方法](docs/project/秒杀项目总结.md) 

## 面试知识点

- [各公司面试知识点汇总](docs/interview-experience/面试常见知识.md) 
- [面试常见问题分类汇总](docs/interview-experience/面试常见问题分类汇总.md) 


## 公司面经

- [2020年各公司面试经验汇总](docs/interview-experience/各大公司面经.md) 

## Java

### 基础

- [Java基础系列文章](https://mp.weixin.qq.com/mp/homepage?__biz=MzI2OTQ4OTQ1NQ==&hid=1&sn=c455e51f87eaa9c12d6b45e0e4d33960&scene=1&devicetype=iOS13.3.1&version=17000c2b&lang=zh_CN&nettype=WIFI&ascene=7&session_us=gh_2bfc34fbf760&fontScale=100&wx_header=1)

### 容器（包括juc）

#### 基础容器

- ArrayList源码分析及真实大厂面试题精讲
- LinkedList源码分析及真实大厂面试题精讲
- HashMap源码分析及真实大厂面试题精讲
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
- [深入理解Java虚拟机-Java内存区域透彻分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-java-nei-cun-qu-yu-tou-che-fen-xi.html)
- [深入理解Java虚拟机-JVM内存分配与回收策略原理，从此告别JVM内存分配文盲](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-jvm-nei-cun-fen-pei-yu-hui-shou-ce-lue-yuan-li-cong-ci-gao-bie-jvm-nei-cun-fen-pei-wen-mang.html)
- [深入理解Java虚拟机-常用vm参数分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-chang-yong-vm-can-shu-fen-xi.html)
- [深入理解Java虚拟机-如何利用JDK自带的命令行工具监控上百万的高并发的虚拟机性能](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-jdk-zi-dai-de-ming-ling-xing-gong-ju-jian-kong-shang-bai-wan-de-gao-bing-fa-de-xu-ni-ji-xing-neng.html)
- [深入理解Java虚拟机-如何利用VisualVM对高并发项目进行性能分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-visualvm-dui-gao-bing-fa-xiang-mu-jin-xing-xing-neng-fen-xi.html)
- [深入理解Java虚拟机-你了解GC算法原理吗](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ni-liao-jie-gc-suan-fa-yuan-li-ma.html)
- [几个面试官常问的垃圾回收器，下次面试就拿这篇文章怼回去！](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-chang-jian-de-la-ji-hui-shou-qi.html)
- [面试官100%会严刑拷打的 CMS 垃圾回收器，下次面试就拿这篇文章怼回去！](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-cms-la-ji-hui-shou-qi.html)


### Java8 

- [Java8快速学习教程](https://blog.ouyangsihai.cn/java8-zui-xin-jiao-cheng-bu-yi-yang-de-java8.html)
- [Java11的最新特性](https://blog.ouyangsihai.cn/java11-zheng-shi-fa-bu-liao-wo-men-gai-zen-me-ban.html)
- [Java8 之 lambda 表达式、方法引用、函数式接口、默认方式、静态方法](https://blog.ouyangsihai.cn/java8-zhi-lambda-biao-da-shi-fang-fa-yin-yong-han-shu-shi-jie-kou-mo-ren-fang-shi-jing-tai-fang-fa.html)
- [Java8之Consumer、Supplier、Predicate和Function攻略](https://blog.ouyangsihai.cn/java8-zhi-consumer-supplier-predicate-he-function-gong-lue.html)
- [Java8 的 Stream 流式操作之王者归来](https://blog.ouyangsihai.cn/java8-de-stream-liu-shi-cao-zuo-zhi-wang-zhe-gui-lai.html)

## 计算机网络

- [http面试问题全解析](docs/network/http面试问题全解析.md)
- 关于tcp、udp网络模型的问题，这篇文章告诉你
- http、https还不了解，别慌！
- 面试官问我计算机网络的问题，我一个问题给他讲半个小时

## 计算机操作系统

- [操作系统、计算机网络相关知识](docs/operating-system/操作系统、计算机网络相关知识.md)

## Linux

- [java工程师linux命令，这篇文章就够了](https://blog.ouyangsihai.cn/java-gong-cheng-shi-linux-ming-ling-zhe-pian-wen-zhang-jiu-gou-liao.html)
- 常问的几个Linux面试题，通通解决它

## 数据结构与算法

### 数据结构

- 跳表这种数据结构，你真的清楚吗，面试官可能会问这些问题！
- 红黑树你了解多少，不会肯定会被面试官怼坏
- [B树，B+树，你了解多少，面试官问那些问题？](https://blog.ouyangsihai.cn/mian-shi-guan-wen-ni-b-shu-he-b-shu-jiu-ba-zhe-pian-wen-zhang-diu-gei-ta.html)
- 二叉树、二叉搜索树、二叉平衡树、红黑树、B树、B+树

### 算法

- [2020年最新算法面试真题汇总](docs/dataStructures-algorithms/算法面试真题汇总.md)  
- [2020年最新算法题型难点总结](docs/dataStructures-algorithms/算法题目难点题目总结.md) 
- [关于贪心算法的leetcode题目，这篇文章可以帮你解决80%](https://blog.ouyangsihai.cn/jie-shao-yi-xia-guan-yu-leetcode-de-tan-xin-suan-fa-de-jie-ti-fang-fa.html)
- 回溯算法不会，这篇文章一定得看
- 动态规划你了解多少，我来帮你入个们
- 链表的题目真的不难，看了这篇文章你就知道有多简单了
- 还在怕二叉树的题目吗？
= 栈和队列的题目可以这样出题型，你掌握了吗
- 数组中常用的几种leetcode解题技巧！

## 数据库

### MySQL

- [MySQL深入理解教程-解决面试中的各种问题](https://blog.ouyangsihai.cn/mysql-shen-ru-li-jie-jiao-cheng-mysql-de-yi-zhu-shi-jie.html)
- [InnoDB与MyISAM等存储引擎对比](https://blog.ouyangsihai.cn/innodb-yu-myisam-deng-cun-chu-yin-qing-dui-bi.html)
- [	面试官问你B树和B+树，就把这篇文章丢给他](https://blog.ouyangsihai.cn/mian-shi-guan-wen-ni-b-shu-he-b-shu-jiu-ba-zhe-pian-wen-zhang-diu-gei-ta.html)
- [MySQL的B+树索引的概念、使用、优化及使用场景](https://blog.ouyangsihai.cn/mysql-de-b-shu-suo-yin.html)
- [	MySQL全文索引最强教程](https://blog.ouyangsihai.cn/mysql-quan-wen-suo-yin.html)
- [	MySQL的又一神器-锁，MySQL面试必备](https://blog.ouyangsihai.cn/mysql-de-you-yi-shen-qi-suo.html)
- [	MySQL事务，这篇文章就够了](https://blog.ouyangsihai.cn/mysql-shi-wu-zhe-pian-wen-zhang-jiu-gou-liao.html)
- [	mysqldump工具命令参数大全](https://blog.ouyangsihai.cn/mysqldump-gong-ju-ming-ling-can-shu-da-quan.html)
- [看完这篇MySQL备份的文章，再也不用担心删库跑路了](https://blog.ouyangsihai.cn/kan-wan-zhe-pian-mysql-bei-fen-de-wen-zhang-zai-ye-bu-yong-dan-xin-shan-ku-pao-lu-liao.html)
- 关于MySQL索引，面试中面试官会怎么为难你，一定得注意
- MySQL中的乐观锁、悲观锁，JDK中的乐观锁、悲观锁？ 

#### MySQL（优化思路）

- [MySQL高频面试题](https://mp.weixin.qq.com/s/KFCkvfF84l6Eu43CH_TmXA)
- [MySQL查询优化过程](https://mp.weixin.qq.com/s/jtuLb8uAIHJNvNpwcIZfpA)
- MySQL面试官会怎么死怼你呢，我告诉你回怼他


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

- [springboot史上最全教程，11篇文章全解析](https://blog.ouyangsihai.cn/categories/springboot2-0%E6%9C%80%E6%96%B0%E6%95%99%E7%A8%8B/)
- [微服务面试相关资料](docs/microservice/微服务相关资料.md)  

## 分布式

### dubbo

- [dubbo入门实战教程，这篇文章真的再好不过了](https://blog.ouyangsihai.cn/dubbo-yi-pian-wen-zhang-jiu-gou-liao-dubbo-yu-dao-chu-lian.html)
- [dubbo源码分析](http://cmsblogs.com/?p=5324)
- [dubbo面试题](https://mp.weixin.qq.com/s/PdWRHgm83XwPYP08KnkIsw)
- [dubbo面试题2](https://mp.weixin.qq.com/s/Kz0s9K3J9Lpvh37oP_CtCA)

### zookeeper

- [什么是zookeeper？](https://mp.weixin.qq.com/s/i2_c4A0146B7Ev8QnofbfQ)

- [Zookeeper教程](http://cmsblogs.com/?p=4139)

- [zookeeper源码分析](http://cmsblogs.com/?p=4190)

- [zookeeeper面试题](https://segmentfault.com/a/1190000014479433)

- [zookeeper面试题2](https://juejin.im/post/5dbac7a0f265da4d2c5e9b3b)


### RocketMQ

- [RocketMQ简单教程](https://juejin.im/post/5af02571f265da0b9e64fcfd)
- [RocketMQ教程](https://mp.weixin.qq.com/s/VAZaU1DuKbpnaALjp_-9Qw)
- [RocketMQ源码分析](http://cmsblogs.com/?p=3236)
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

- [分布式架构文章](https://blog.ouyangsihai.cn/fen-bu-shi-jia-gou-xi-lie-wen-zhang.html)

### 消息中间件

- [消息中间件面试题总结](docs/project/消息中间件面试题.md)  

### redis

- [Redis设计与实现总结文章](https://blog.csdn.net/qq_41594698/category_9067680.html)
- [Redis面试题必备：基础，面试题](https://mp.weixin.qq.com/s/3Fmv7h5p2QDtLxc9n1dp5A)
- [Redis面试相关：其中包含redis知识](https://blog.csdn.net/qq_35190492/article/details/103105780)
- [Redis源码分析](http://cmsblogs.com/?p=4570)
- [redis其他数据结构](https://blog.csdn.net/c_royi/article/details/82011208)

### 分布式系统

## 线上问题调优（虚拟机，tomcat）
- [垃圾收集器ZGC](https://juejin.im/post/5dc361d3f265da4d1f51c670)
- [jvm系列文章](https://crowhawk.github.io/tags/#JVM)
- [一次JVM FullGC的背后，竟隐藏着惊心动魄的线上生产事故！](https://mp.weixin.qq.com/s/5SeGxKtwp6KZhUKn8jXi6A)
- [Java虚拟机调优文章](https://blog.ouyangsihai.cn/categories/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3Java%E8%99%9A%E6%8B%9F%E6%9C%BA/)
- [利用VisualVM对高并发项目进行性能分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-visualvm-dui-gao-bing-fa-xiang-mu-jin-xing-xing-neng-fen-xi.html#toc-heading-8)
- [JVM性能调优](https://www.iteye.com/blog/uule-2114697)
- [百亿吞吐量服务的JVM性能调优实战](https://mp.weixin.qq.com/s?__biz=MzIwMzY1OTU1NQ==&mid=2247484236&idx=1&sn=b9743b2d7436f84e4617ff34e07abdd8&chksm=96cd4300a1baca1635a137294bc93c518c033ce01f843c9e012a1454b9f3ea3158fa1412e9da&scene=27&ascene=0&devicetype=android-24&version=26060638&nettype=WIFI&abtest_cookie=BAABAAoACwASABMABAAjlx4AUJkeAFmZHgBomR4AAAA%3D&lang=zh_CN&pass_ticket=%2F%2BLqr9N2EZtrEGLFo9vLA6Eqs89DSJ2CBKoAJFZ%2BBngphEP28dwmMQeSZcUB77qZ&wx_header=1)
- [一次线上JVM调优实践，FullGC40次/天到10天一次的优化过程](https://blog.csdn.net/cml_blog/article/details/81057966)
- [JVM调优工具](https://www.jianshu.com/p/e36fac926539)


## 面试指南


## 工具

### Git

- [实际开发中的git命令大全](https://blog.ouyangsihai.cn/wo-zai-shi-ji-gong-zuo-zhong-yong-de-zui-duo-de-git-ming-ling.html)

### Docker

## 其他

### 权限控制（设计、shiro）

- [权限控制设计](https://mp.weixin.qq.com/s/WTgz07xDIf9FbAfCDyTheQ)
- [shiro相关教程](https://blog.csdn.net/sihai12345/category_9268544.html)
- [springboot+vue+shiro前后端分离实战项目](https://blog.csdn.net/neuf_soleil/category_9287210.html)
- [shiro挺好的教程](https://how2j.cn/k/shiro/shiro-springboot/1728.html)

## Java学习资源

- [2020年Java学习资源，你想要的都在这里了](https://mp.weixin.qq.com/s/wLjjy7D57s3UOv4sr8Lxkg)

**截图**

![](http://image.ouyangsihai.cn/Fl0FhkpxLNw0_4-pe8_f8MwAyHzc)
![](http://image.ouyangsihai.cn/Fp3EtjR1FbKPJG2uPdGpMiFjHBNR)
![](http://image.ouyangsihai.cn/FqEKc4i6lsfLCRomFAIksG_rgveY)


## Java书籍推荐

- [从入门到拿大厂offer，必须看的数据结构与算法书籍推荐](https://blog.ouyangsihai.cn/cong-ru-men-dao-na-da-han-offer-bi-xu-kan-de-suan-fa-shu-ji-tui-jian-bu-hao-bu-tui-jian.html)

## 实战项目推荐

>小心翼翼的告诉你，上面的资源当中就有很多**企业级项目**，没有项目一点不用怕，因为你看到了这个。

- [找工作，没有上的了台面的项目怎么办？](https://mp.weixin.qq.com/s/0oK43_z99pVY9dYVXyIeiw)


## 程序人生

- [我想是时候跟大学告别了](https://blog.ouyangsihai.cn/wo-xiang-shi-shi-hou-he-da-xue-gao-bie-liao.html)
- [坚持，这两个字非常重要！](https://blog.ouyangsihai.cn/jian-chi-zhe-liang-ge-zi-fei-chang-chong-yao.html)
- [2018年年终总结，你的呢？](https://blog.ouyangsihai.cn/zhe-shi-wo-de-2018-nian-zhong-zong-jie-ni-de-ni.html)
- [多去了解了解自己](https://blog.ouyangsihai.cn/duo-wen-wen-zi-ji-xiang-cheng-wei-shi-me-yang-de-ren.html)
- [关于考研，这是我给大家的经验](https://blog.ouyangsihai.cn/guan-yu-zhe-jian-shi-wo-you-hua-yao-shuo.html)
- [从普通二本到研究生再到自媒体的年轻人，这是我的故事](https://blog.ouyangsihai.cn/cong-pu-ben-dao-zha-shuo-cong-da-xue-sheng-dao-zi-mei-ti-de-nian-qing-ren-wo-fen-xiang-wo-de-coding-sheng-huo.html)

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

![](http://image.ouyangsihai.cn/FldnPFgz_u_3kt7YH_sHhAQL1kyt)


### 公众号

如果大家想要实时关注我更新的文章以及分享的干货的话，关注我的公众号 **程序员的技术圈子**。

![](http://image.ouyangsihai.cn/FlL0VJf1Q4gCfrc8RhL-SL-xiiXo)


