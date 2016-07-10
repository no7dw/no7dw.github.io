title: write unit test with java && run junit with mvn
date: 2016-07-10 23:27:26
tags:
---

### how to run test within junit & mvn:
During team development , there are more than 1 person edit your api or file which it's very common. And to improve your api stablity and quality, it's critical important to write unit test.

#### run all test
    mvn test
#### run specific test
Sometimes we have thousands of unit test, but you need to test only some testcase you have just wrote for verify some case you can specific your only test case:

    mvn -Dtest=TestApp1 test
    
#### run exclude some specific case
Meanwhile you may [exclude some unrelated test case(or include some test case)][1] for a quick verify.

for example, skip TestApp2.java, edit your pom.xml:

>     <build>
>     <plugins>
>       <plugin>
>         <groupId>org.apache.maven.plugins</groupId>
>         <artifactId>maven-surefire-plugin</artifactId>
>         <version>2.19.1</version>
>         <configuration>
>           <excludes>
>             <exclude>**/TestApp2.java</exclude>
>           </excludes>
>         </configuration>
>       </plugin>
>     </plugins>   </build>

    

#### serval common case log:

 - when not pass test:

> Running com.wade.core.TestApp1 Tests run: 1, Failures: 1, Errors: 0,
> Skipped: 0, Time elapsed: 0.091 sec <<< FAILURE!
> testHelloworld(com.wade.core.TestApp1)  Time elapsed: 0.022 sec  <<<
> FAILURE! junit.framework.ComparisonFailure: expected:<Hello[W]orld>
> but was:<Hello[w]orld>        at
> junit.framework.Assert.assertEquals(Assert.java:100)

 - pass test:

> Running com.wade.core.TestApp1 Tests run: 1, Failures: 0, Errors: 0,
> Skipped: 0, Time elapsed: 0.093 sec Running com.wade.core.TestApp2
> Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0 sec
> 
> Results :
> 
> Tests run: 2, Failures: 0, Errors: 0, Skipped: 0

 - compile error:

> [INFO] BUILD FAILURE [INFO]
> ------------------------------------------------------------------------ [INFO] Total time: 1.392 s [INFO] Finished at:
> 2016-07-10T23:15:34+08:00 [INFO] Final Memory: 14M/165M [INFO]
> ------------------------------------------------------------------------ [ERROR] Failed to execute goal
> org.apache.maven.plugins:maven-compiler-plugin:3.1:testCompile
> (default-testCompile) on project unittest: Compilation failure:
> Compilation failure: [ERROR]
> /Users/dengwei/projects/github/javacourse/unittest/src/test/java/com/wade/TestApp2.java:[8,61]
> ';' expected [ERROR]
> /Users/dengwei/projects/github/javacourse/unittest/src/test/java/com/wade/TestApp1.java:[8,59]
> ';' expected [ERROR] -> [Help 1] [ERROR]


Appendix:

 - [junit][2]
 - [junit hello world][3]
 - [junit mvn configuration in pom.xml][4]
 - [src][5]


  [1]: https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html
  [2]: https://books.sonatype.com/mcookbook/reference/unit-sect-junit-run.html
  [3]: https://www.mkyong.com/maven/how-to-run-unit-test-with-maven/
  [4]: https://maven.apache.org/surefire/maven-surefire-plugin/examples/junit.html
  [5]: https://github.com/no7dw/javaCourse/tree/master/unittest
