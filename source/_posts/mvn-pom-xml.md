title: mvn pom.xml
date: 2016-03-27 22:01:47
tags:
---
# mvn 的 pom.xml

pom -- project object model. 用于描述项目的配置：

 - 基础说明
 - 依赖
 - 如何构建运行

类似 node.js 的 package.json
mvn 与 npm 也是有雷同的地方。

---

    <dependencies>

        <!-- Unit Test -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>

        <!-- Spring Core -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    <dependencies>      

dependencies in pom.xml 相当于 项目的依赖包，启动的时候会去找，如果没有就去下载。
类似npm 的package.json dependency 

dependency 里面的[主要信息][1]groupId, 相当与组织机构的项目组ID，attifactId 项目的通用ID，version 当然是要的了。



    mvn tomcat:run 
    mvn jetty:run

tomcat/jetty 是一些mvn plugin. 
[jetty 参考配置][2]
所谓mvn plugin 可以理解成，这是使得可以集成与mvn 命令一起使用的插件。官方文档[参考][3]
类似 npm clean , npm <command> ,see `npm --help`


  [1]: http://blog.csdn.net/zhuxinhua/article/details/5788546
  [2]: http://blog.csdn.net/ph9527/article/details/5063157
  [3]: https://maven.apache.org/plugins/index.html
