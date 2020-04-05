### pyspark spark 快速入门 懒人版本

#### 安装

##### docker 安装方式

最简单的是直接docker，有一下几个比较快速的安装方式参考：
- https://github.com/actionml/docker-spark
- https://github.com/wongnai/docker-spark-standalone
- https://github.com/epahomov/docker-spark
- https://towardsdatascience.com/a-journey-into-big-data-with-apache-spark-part-1-5dfcc2bccdd2


如果是进一步的简化，那就选择安装spark standalone version， 不依赖hadoop

安装完成后，就可以启动 sbin的脚本

```
    ./start-all.sh
```

其web的界面效果（这里有两个worker）：
![2020-04-05-10-30-43](http://img.no1token.com/2020-04-05-10-30-43.png)

注意:
   由于上述的docker用的是基础镜像 openjdk:8-alpine，比较适合scala的环境，pyspark 需要python3 ，所以要在Dockerfile，另外处理python3 的安装

##### 直接官网下载bin

    https://spark.apache.org/downloads.html

#### 运行例子

最简单直接在安装的目标主机上，运行,(否则把localhost替换成spark host)

```
    # Run a Python application on a Spark standalone cluster
    ./bin/spark-submit \
    --master spark://localhost:7077 \
    examples/src/main/python/pi.py \
    1000
```

观察CPU，利用了多个核

#### pyspark

```
    ./bin/pyspark    
```

运行pyspark的wordcount (helloworld)
``` python
    >>> p='/usr/local/spark/README.md'
    >>> text_file = sc.textFile(p)
    >>> counts = text_file.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)
    >>> counts
    PythonRDD[6] at RDD at PythonRDD.scala:53
    >>> counts.collect()
    [Stage 0:>                                                          (0 + 2) / 2]/usr/local/spark/python/lib/pyspark.zip/pyspark/shuffle.py:60: UserWarning: Please install psutil to have better support with spilling
    /usr/local/spark/python/lib/pyspark.zip/pyspark/shuffle.py:60: UserWarning: Please install psutil to have better support with spilling
    [('#', 1), ('Apache', 1), ('Spark', 15), ('', 73), ('is', 7), ('unified', 1), ('analytics', 1), ('engine', 2), ('It', 2), ('provides', 1), ('high-level', 1), ('APIs', 1), ('in', 6), ('Scala,', 1), ('Java,', 1), ('an', 4), ('optimized', 1), ('supports', 2), ('computation', 1), ('analysis.', 1), ('set', 2), ('of', 5), ('tools', 1), ('SQL', 2 
    ...
```

注意点：
- counts 的数据类型是RDD
- counts = text_file.flatMap(lambda line: line.split(" ")).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b) 的时候没有真正跑， count.collect()的时候才真正运行任务。
    这是spark特色：需要真正用到变量的时候才运行
- sc 是启动pyspark 后自动初始化的变量


#### RDD (resilient distributed dataset )

    RDD 是提供一直操作collection里面每个元素的方法，该方法可以在集群的各个node 并行运行。

    例子教程：

- https://spark.apache.org/docs/latest/rdd-programming-guide.html

注意：看起来这个RDD用处像是dataframe （除了跨node 并行计算），他是spark dataframe ，但实际语法与pandas的dataframe是不一样的。

这就带来一个问题，我需要重新熟悉学习他的用法。
为了解决这个问题，databricks社区有一个方案 [Koalas](https://koalas.readthedocs.io/en/latest/getting_started/10min.html)

用了这个之后，就可以转化到类普通pandas 的熟悉的dataframe处理

``` python
    kdf = ks.from_pandas(pdf) #pandas的dataframe -> koalas
    kdf = sdf.to_koalas() # spark dataframe ->koalas的dataframe
```

参考：
- https://www.iteblog.com/archives/2549.html
- https://koalas.readthedocs.io/en/latest/getting_started/10min.html


#### 提交任务

    上面最简单的例子是提交一个python 文件到spark ， 但实际我们需要多个files ，并且有依赖，事实上这里比较多坑

- 最简单的依赖例子1 - 无第三方依赖的case：
```    
    [klg@ira-r740 wade-test]$ ls
    application   requirements.txt  t.py  
    [klg@ira-r740 wade-test]$ tree application
    application
    └── myfile.py

    0 directories, 1 file
    
```
t.py
```python    
from pyspark.sql import SparkSession
SPARK_MASTER_HOST = 'ira-r740'
SPARK_MASTER_PORT = 7077
def test(x):
    from application.myfile import simple_function
    return simple_function(x)

if __name__ == "__main__":
    spark = SparkSession\
        .builder\
        .config('spark.master', 'spark://{spark_master_host}:{spark_master_port}' \
        .format(spark_master_host=SPARK_MASTER_HOST, spark_master_port=SPARK_MASTER_PORT)) \
        .appName("PythonPiwade")\
        .getOrCreate()

    # test by mapping function over RDD
    rdd = spark.sparkContext.parallelize(range(1,100))
    result = rdd.map(lambda x: test(x)).collect()
    print("result", result)
    spark.stop()
```
先压缩，后提交
```
    $cd wade-test
    $zip -rq application.zip 
    $./bin/spark-submit   --py-files /home/klg/pyspark/wade-test/application.zip  /home/klg/pyspark/wade-test/t.py
```



- 最简单的依赖例子 - 有第三方依赖的case

t.py
``` python
...   
if __name__ == "__main__":
    ...
    rdd = spark.sparkContext.parallelize(range(1,100))
    result = rdd.map(lambda x: test(x)).collect()
    print("result", result)
    import bs4 ### 变动在这里
    print(bs4.__version__) ### 变动在这里
    spark.stop()
```

[klg@ira-r740 wade-test]$ ls
application  application.zip  requirements.txt  t.py  venv  venv.zip
先压缩，后提交
```
    $cd wade-test
    $python3 -m venv venv # 建立虚拟环境
    $source venv/bin/activate
    $pip install -r requirements.txt
    $zip -rq venv.zip venv # 打包依赖
    $export PYSPARK_PYTHON="venv/bin/python"
    $spark-submit \
    --name "Sample Spark Application" \
    --conf "spark.yarn.appMasterEnv.SPARK_HOME=$SPARK_HOME" \
    --conf "spark.yarn.appMasterEnv.PYSPARK_PYTHON=$PYSPARK_PYTHON" \
    --archives "venv.zip#venv" \
    --py-files "application.zip" \
    t.py

```

**注意:这里有个命令 export PYSPARK_PYTHON="venv/bin/python" , 指定了PYSPARK_PYTHON 的变量，以便运行的时候指定了python的路径、依赖的包的路径。不然会有找不到依赖包**



- 最简单的依赖例子 - 有第三方依赖的case (推荐的解决方案) , 弊端是多worker 需要重复操作。看自己balance, 为快速懒人入门：

    - 只在少量worker or甚至1台worker 的情况下，直接在宿主机安装spark
    - 安装后需要依赖报的，直接在宿主机的 运行pip3 install -r requirements.txt 

更多参考：https://github.com/massmutual/sample-pyspark-application

#### 实战例子

##### 处理csv ：

csv.py
```python
import pandas as pd

SPARK_MASTER_HOST = 'ira-r740'
SPARK_MASTER_PORT = 7077
spark = SparkSession\
    .builder\
    .config('spark.master', 'spark://{spark_master_host}:{spark_master_port}' \
      .format(spark_master_host=SPARK_MASTER_HOST, spark_master_port=SPARK_MASTER_PORT)) \
    .appName("PythonSmsWade")\
    .getOrCreate()

def trs2(x):
    try:
        if( x and (x['smsContent'] )):
            return (str(x['smsContent']).upper(),0)
        else:
            return ('',-1)
    except Exception as e:
        return ('',-1)
    
  
#normal quick way
df1=(spark.read.format("csv").options(header="true").load("/home/klg/pyspark/py/100k.csv")) #normal
result=spark.sparkContext.parallelize(df1.collect()).map(lambda x: trs2(x)).collect()
print(len(result))    
```
设置环境spark 的path变量后（简化提交），
$spark-submit csv.py

注意：
官网例子的文件路径大部分是 hdfs，这里为了简化懒人，简化架构，不依赖hive 分布式存储，file 可以有这么的选项:
- copy 到各个worker 机器上
- 从db 读取（推荐）
- 放在某一台机器上后，其他worker mount 到这个目录 （推荐）

自己需要balance的点： 没有利用到分布式存储对速度上的优势，但大部分case 下面我们需要的处理时间在于多核的cpu 计算时间。

##### 从db 读取数据

```python
import pandas as pd
def test2( x):
    # method 1 : we reinit the var here , simple
    # method 2 : we use global share var  
    from pymongo import MongoClient
    client = MongoClient()
    client = MongoClient('10.10.20.60', 31111)
    query = {}
    docs = list(client['cash_loan']['user'].find(query).limit(200).skip(x*10))
    # do the job 
    # ...
    return docs 

SPARK_MASTER_HOST = 'ira-r740'
SPARK_MASTER_PORT = 7077
spark = SparkSession\
    .builder\
    .config('spark.master', 'spark://{spark_master_host}:{spark_master_port}' \
      .format(spark_master_host=SPARK_MASTER_HOST, spark_master_port=SPARK_MASTER_PORT)) \
    .appName("Pythonsmswade")\
    .getOrCreate()

n=5
result=spark.sparkContext.parallelize(range(1,n)).map(lambda x: test2(x)).collect()

```
注意：
- return 的might be list of list if test2 return list  [limit]*[n] （因为test2方法里面list 化返回值）
- 在test2 里面重新初始MongoClient 变量，以避免全局、共享变量问题

#### 全局变量（共享变量）

    To be continue...

#### 那些年踩过的坑

    To be continue...
