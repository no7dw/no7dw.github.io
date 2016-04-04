title: java-common-error
date: 2016-04-04 22:42:04
tags:
---
#java 编译运行常见错误

### compile error: cannot find symbol

即识别不了src code 的一些语法/字符串
可能的原因：

 - 大小写没写对 Java is case sensitive
 - 拼写错误
 - 未申明的变量
 - 变量使用时不在scope 范围
 - 忘记import 对应的class
 - 应该使用 **new** SomeConstructor()

[ref link][1]

### run time error: Could not find or load main class

即找不到main入口。

    java mainClass param1 param2

一个java 命令成功调用前，会先执行以下步骤：

 - 查找mainClass的编译版本 mainClass.class
 - 加载该class
 - 找到总入口：public static void main(String[])
 - 传递参数para1 param2 ... 到 String[]
 
而上述Could not find or load main class错在第一步
可能原因：

 - classpath 设置不对
 - 运行的路径不对

类似下面make run(相当与java ./src/AsyncBankTest)：

    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ make
    javac ./src/BankAccount.java
    javac ./src/TransferRunnable.java 
    javac ./src/AsyncBankTest.java
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ make run
    java ./src/AsyncBankTest
    Error: Could not find or load main class ..src.AsyncBankTest
    make: *** [run] Error 1
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ java -classpath src/ src/AsyncBankTest
    Error: Could not find or load main class src.AsyncBankTest
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ java -classpath src src/AsyncBankTest
    Error: Could not find or load main class src.AsyncBankTest
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ java -classpath ./src src/AsyncBankTest
    Error: Could not find or load main class src.AsyncBankTest
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/ThreadBankTransfer$ java -classpath . src/AsyncBankTest

而实际正确的是：

    java -classpath . thepackagename.TheClassName

**因此上述的正确的运行方式是**：

    java src/AsyncBankTest
    java src.AsyncBankTest

or 

    java -classpath . src/AsyncBankTest
    java -classpath . src.AsyncBankTest

**/** or **.** work fine in OSX

[ref link][2]
[ref link][3]
 
### compile error: java.lang.NoClassDefFoundError 

我们compile的时候，经常会碰到 java.lang.NoClassDefFoundError 的编译错误，

究其直接原因：package 定位失败，参考javaCourse/Package4 的各种package 的定位

这种错误很常见，另外一些类似原因的总结的link：
[为什么执行JAVA程序时，会出现Exception in thread"main" java.lang.NoClassDefFoundError的错][4]
[exception in thread 'main' java.lang.NoClassDefFoundError][5] 

**正确的例子**：

run under folder:

    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse$javac callback/TimerTest.java 
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse$java callback/TimerTest 

if not in the right foler, you'll get error:

    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/callback$ javac TimerTest.java
    dengwei@dengweis-MacBook-Pro:~/project/github/javaCourse/callback$ java TimerTest
    Exception in thread "main" java.lang.NoClassDefFoundError: TimerTest (wrong name: callback/TimerTest)

    Exception in thread "main" java.lang.NoClassDefFoundError: TimerTest (wrong name: callback/TimerTest)


because your package :

    package callback;


  [1]: http://stackoverflow.com/questions/25706216/what-does-a-cannot-find-symbol-compilation-error-mean
  [2]: http://stackoverflow.com/questions/18093928/what-does-could-not-find-or-load-main-class-mean
  [3]: http://stackoverflow.com/questions/7485670/error-could-not-find-or-load-main-class
  [4]: http://zhangjf.blog.51cto.com/264589/50630
  [5]: http://stackoverflow.com/questions/6334148/exception-in-thread-main-java-lang-noclassdeffounderror
