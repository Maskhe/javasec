# javasec

![GitHub](https://img.shields.io/github/license/Maskhe/javasec)
![](https://img.shields.io/badge/%E5%85%AC%E4%BC%97%E5%8F%B7-%E4%B8%80%E4%B8%AA%E5%AE%89%E5%85%A8%E7%A0%94%E7%A9%B6%E5%91%98-brightgreen)

这是我在学习java安全审计的一些总结，每篇文章可能都不会很长，可能就只是讲一个知识点，但是文章越短，我才越容易坚持下去把这系列文章写完～

本系列文章不求把每个细节都覆盖到，但求把提到的每个知识点用通俗易懂的话阐述出来

文章体系规划(待完善）：
#### JAVA反射机制

[java反射机制](1.java反射机制.md)

#### JAVA动态代理机制

[java动态代理](java动态代理.md)

#### JAVA序列化与反序列化机制
常见的pop gadgets以及一些反序列化漏洞案例

[java序列化与反序列化](2.java序列化与反序列化.md)

[apache commons-collections中的反序列化利用链](3.%20apache%20commons-collections中的反序列化.md)

[Apache Dubbo反序列化漏洞分析](4.Apache%20Dubbo反序列化漏洞分析.md)

[log4j的反序列化漏洞分析](4.log4j的反序列化.md)



#### IDEA调试技巧

[IDEA调试技巧1](5.IDEA调试技巧1.md)

[IDEA调试技巧2——远程调试](5.IDEA调试技巧2——远程调试.md)



#### RMI基础

[java rmi基础](6.java%20rmi基础.md)

#### 攻击RMI的方式

结合自己写的demo以及真实漏洞案例

[攻击rmi的方式](7.攻击rmi的方式.md)

#### JNDI注入

几句话讲解原理，再结合几个真实漏洞案例：fastjson、spring的jndi注入案例

[jndi注入](8.jndi注入.md)

[fastjson-1.2.24反序列化漏洞](9.fastjson-1.2.24反序列化漏洞.md)

#### JMX的安全问题

[jmx安全问题](10.jmx安全问题.md)

#### XXE

几种常见的解析xml的jar包，以及他们的漏洞点，防御手法

[XXE之DocumentBuilder](14.XXE之DocumentBuilder.md)

[XXE之XML解析常用库的使用案例](15.XXE之XML解析常用库的使用案例.md)

[XXE之setFeature防御](16.XXE之setFeature防御.md)



#### JDK中其它的一些反序列化

[XMLDecoder反序列化](17.XMLDecoder反序列化.md)

[Weblogic之XMLDecoder反序列化1（CVE-2017-3506)](18.Weblogic之XMLDecoder反序列化1_CVE-2017-3506.md)

#### JAVA表达式安全问题

#### SQL注入

#### JWT安全问题

#### Spring框架基础知识

#### Struts2框架基础知识

#### SSRF
主要是各种方法，各个jar包

#### 各大大型应用、类库的漏洞

weblogic系列、spring系列、fastjson系列、jackson系列、solr、jboss、tomcat、struts2....

#### JVM基础

此部分内容虽是基础知识，但是对于漏洞挖掘也许并无多大助益，我在写作的过程中也只挑了我关注的内容，像jvm垃圾收集及调优部分内容就被我舍弃了

[1.Class文件的结构](jvm-Class文件的结构.md)

[2.类加载机制](jvm-类加载机制.md)

[3.jvm内存区域](jvm内存区域.md)

[4.hotspot虚拟机对象探秘](jvm-hotspot虚拟机对象探秘.md)

[5.字节码指令](字节码指令.md)






![](wechat.png)

