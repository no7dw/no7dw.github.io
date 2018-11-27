title:  package dependency manager

---

### package dependency manager

[我们有很多的管理工具：](https://github.com/golang/go/wiki/PackageManagementTools)

dep , godep, govendor ,glide 

其中比较好的是
govendor, glide

好的指需要做到：

 - 每个项目是独立的
 - 依赖管理是清晰、容易管理的

glide 的yaml 是比较清晰and 好管理的
在不同项目的不同版本上 gobendor , glide 都比较好的做到了，各自目录下面都有vendor 存储对应的依赖
类似npm

但都没有解决国内加速访问的问题
npm 可以设cnpm 
maven 可以添加 conf/settings.xml 配置

```
    <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>*</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>

```

or 
```
    
    <mirror>
        <id>CN</id>  
        <name>OSChina Central</name>
        <url>http://maven.oschina.net/content/groups/public/</url>  
        <mirrorOf>central</mirrorOf>  
    </mirror>

```



[ref](https://yanyiwu.com/work/2014/10/01/node-vs-go-about-packagemanager.html)
[ref](https://blog.csdn.net/jettery/article/details/78104601?utm_source=blogxgwz2)

