# Java代码审计手册

【声明】个人的快速查询手册，经验整理，仅供参考。   
【内容】本手册主要关注于Java漏洞挖掘而非利用，漏洞利用在[WEB 安全手册](https://github.com/ReAbout/web-sec)有总结。复现案例分析也是关注漏洞原理，通过调试分析加强对漏洞产生模式理解，辅助漏洞挖掘。

## 0x00 环境准备篇
>Idea,VSCode
- [Java调试环境搭建](./base/debug.md)
> Evaluate Expression
- [Idea调试技巧](https://developer.aliyun.com/article/1058784)
>持续更新java反编译工具，越来越好用
- [[Tool] jadx](https://github.com/skylot/jadx)

## 0x01 基础知识篇

>如果我们不用import就能获取一个类，那这就是反射了
- [反射机制](./base/reflect.md)
- [注解机制](./base/annotation.md)
>使用代理对象来代替对真实对象的访问，可以在不修改原目标对象的前提下，提供额外的功能操作，扩展目标对象的功能
- [代理模式](./base/proxy.md)
- [JNDI](./base/jndi.md)
- - [RMI](./base/rmi.md)
- - [LDAP](./base/ldap.md)
- JavaBean
- - JavaBean 是一种遵循特定规范（如具有无参构造方法、可序列化、提供 getter 和 setter 方法）的 Java 类，通常用于封装数据和在不同组件间传递信息。
- 工厂方法(Factory)
- - 工厂模式是一种设计模式，提供了一种创建对象的方式，而无需指定要创建的具体类。
- Reference类
- - Java中的Reference类是用于在垃圾回收机制中提供对对象的引用控制，它允许开发者通过软引用、弱引用、虚引用等方式管理对象的生命周期。
- - JNDI注入中使用Reference类构造是因为它可以将恶意对象包装成对JNDI的引用，使得在JNDI查找时，恶意对象的类加载器被触发，从而实现远程代码执行或其他攻击。

## 0x02 漏洞挖掘篇

### 1. 注入漏洞


- [命令注入&代码注入](./vul/rce.md)
- [XML外部实体(XXE)注入](./vul/xxe.md)
>高版本JDK对JNDI远程调用进行了限制
- [JNDI注入](./base/jndi.md)
- - [Apache Log4j 漏洞分析 （CVE-2021-45105）](./poc/CVE-2021-45105.md)
- [模板注入(SSTI)](./vul/ssti.md)
- [SQL注入](./vul/sql.md)

#### 表达式注入
>Java表达式语言包括:EL,SPEL,OGNL,MVEL,JBoss EL... 
- [表达式(EL)注入](./vul/el.md)
- [Spring表达式(SPEL)注入](./vul/el.md)
- - [SpringBoot框架SPEL注入漏洞分析(CNVD-2016-04742)](./poc/CNVD-2016-04742.md)
- [对象导航图语言(OGNL)注入](./vul/ognl.md)

### 2. 反序列化漏洞

- [反序列化漏洞](./vul/deserialization.md)

#### 反序列化漏洞点
- [Fastjson 1.2.24反序列化漏洞分析 （CVE-2017-18349）](./poc/CVE-2017-18349.md)
#### 反序列化漏洞链
- [[Tool] 反序列化漏洞利用工具-Java ysoserial](https://github.com/frohoff/ysoserial)
- [Apache Commons Collections反序列化链漏洞分析 (CVE-2015-4852)](./poc/CVE-2015-4852.md)


### 3. 不安全的文件操作

- [不安全的文件上传-待整理](./vul/upload.md)

### 4. 不一致性解析

- [URL解析的不一致性](./vul/inconsistency-url.md)
- 前后端的不一致性
>经典系列漏洞F5 BIG-IP设备认证绕过
- - F5 BIG-IP认证绕过（CVE-2020-5902）
- - F5 BIG-IP认证绕过（CVE-2021-22986）
- - F5 BIG-IP认证绕过（CVE-2022-1388）
- - F5 BIG-IP认证绕过（CVE-2023-46747）

### 5. 不安全的组件

#### 组件

- fastjson
- - [Fastjson 1.2.24反序列化漏洞分析 （CVE-2017-18349）](./poc/CVE-2017-18349.md)
- shiro
- - Apache Shiro rememberMe反序列化漏洞分析（Shiro-550）
- - Apache Shiro Padding Oracle Attack漏洞分析（Shiro-721）
- log4j
- - [Apache Log4j 漏洞分析 （CVE-2021-45105）](./poc/CVE-2021-45105.md)


#### Web容器&框架
- struts2
- -  Struts2远程代码执行（S2-016）
- -  Struts2远程代码执行（S2-045）
- Spring
- - [Spring 框架漏洞相关合集](https://www.viewofthai.link/2023/01/18/spring-%E6%A1%86%E6%9E%B6%E6%BC%8F%E6%B4%9E%E7%9B%B8%E5%85%B3%E5%90%88%E9%9B%86/)
- -  [SpringBoot框架SPEL注入漏洞分析(CNVD-2016-04742)](./poc/CNVD-2016-04742.md)

## 0x03 自动化漏洞挖掘

### 静态分析
>需要源码，对闭源jar不友好
- [CodeQL for Java](./auto/codeql.md)
>自动挖掘反序列化链
- [[Tool] gadgetinspector](https://github.com/JackOfMostTrades/gadgetinspector)
> threedr3am的改版增加了一些功能参数
- [[Tool] threedr3am的改版gadgetinspector](https://github.com/threedr3am/gadgetinspector)
>Tai-e静态分析框架，南京大学软件分析配套框架，有完整的教学和实验文档，很适合从头学习
- [[Tool] Tai-e](https://github.com/pascal-lab/Tai-e)
- [Tai-e主站，包括教学视频和文档](https://tai-e.pascal-lab.net/)
>针对android java漏洞挖掘工具
- [[Tool] mariana-trench
](https://github.com/facebook/mariana-trench)
>TABBY使用静态分析框架 Soot 作为语义提取工具，将JAR/WAR/CLASS文件转化为代码属性图。 并使用 Neo4j 图数据库来存储生成的代码属性图CPG。
- [[Tool] tabby](https://github.com/wh1t3p1g/tabby)



## Other

- [在线markdown表格生成器](https://tableconvert.com/zh-cn/markdown-generator)