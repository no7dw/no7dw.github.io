title: mvn pom.xml
date: 2016-03-27 22:01:47
tags:
---
# 从npm 角度理解 mvn 的 pom.xml

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

然而看起来，要记住的内容十分多，不如npm ： 只需要知道package name, version 简单可以用latest。记住groupid 真的不容易。However,其实并不需要真的记住。有IDE 工具协助：
安装elipse , maven plugin， 打开 pom.xml , 选择 dependencies, 选择 Add, 直接在search 处，输入你的package name， 会列出搜索结果，自动使用 latest version。当然也可以展开细节list指定某个特定的version。
如果不用elipse, 也可：

http://oqln5pzeb.bkt.clouddn.com/17-7-29/90028337.jpg
http://oqln5pzeb.bkt.clouddn.com/17-7-29/90028337.jpg

[trouble sovling when mvn search not working](http://stackoverflow.com/questions/14059685/eclipse-maven-search-dependencies-doesnt-work#_=_)
[reindex maven project with Eclipse](https://books.sonatype.com/m2eclipse-book/reference/repository-sect-repo-view.html)


    mvn tomcat:run 
    mvn jetty:run

tomcat/jetty 是一些mvn plugin. 
[jetty 参考配置][2]
所谓mvn plugin 可以理解成，这是使得可以集成与mvn 命令一起使用的插件。官方文档[参考][3]

Also:
mvn 可用编译，打包，安装，build项目

  - 编译：(nodejs 里面没有这个--因动态语言)

    mvn compile

  - 打包：(nodejs 里面没有这个--因动态语言)

    <modelVersion>4.0.0</modelVersion>
    <groupId>org.springframework</groupId>
    <artifactId>gs-maven</artifactId>
    <packaging>jar</packaging>
    <version>0.1.0</version>

 运行mvn package 后，会产生 {$artifactId}-{$version}-jar,如上述的：gs-maven-0.1.0.jar （target 文件夹）

  - 安装 (类似: npm install)

    mvn install

  - build (类似npm script 里面的自定义脚本: npm start )
  根据plugin 输入参数构建，如上述的：

    mvn jetty:run



类似 npm clean , npm <command> ,see `npm --help`

### speed it up

  mvn 经常很慢，等很久都没完成。参考以下[链][3][接][4]
  `mvn -T 4 install -- will use 4 threads`



  [1]: http://blog.csdn.net/zhuxinhua/article/details/5788546
  [2]: http://blog.csdn.net/ph9527/article/details/5063157
  [3]: https://maven.apache.org/plugins/index.html
  [4]: http://zeroturnaround.com/rebellabs/your-maven-build-is-slow-speed-it-up/
  [5]: http://stackoverflow.com/questions/161698/how-can-i-speed-up-my-maven2-build
