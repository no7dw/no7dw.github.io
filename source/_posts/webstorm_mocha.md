title: 使用webstorm 配置文件配置mocha
---
我们在使用Webstorm 编码后，跑测试时，通常通过Makefile 实现，

    $ make test

来跑测试用例。通过istanbul 来看code test coverage.
但是当测试用例几十或上百时，其中几条测试用例跑不过时，你需要反复滚动terminal，去定位报错的测试代码。这样效率很低。
那么有没有更好的解决办法呢？
答案是：有的，并且可以快速按模块定位问题，调整到报错的代码行，看跑测试代码的进度。
sounds good。 Let's see how to do it.

Webstorm 的运行配置文件。感谢Nick 提供的配置。

Mocha_Coverage.xml  使用 Mocha 运行测试。

    <component name="ProjectRunConfigurationManager">
      <configuration default="false" name="Mocha Coverage" type="mocha-javascript-test-runner" factoryName="Mocha">
        <node-options />
        <working-directory>$PROJECT_DIR$</working-directory>
        <pass-parent-env>true</pass-parent-env>
        <envs>
          <env name="REDIS_HOST" value="koala" />
          <env name="PORT" value="8012" />
          <env name="TOKEN_SECRET" value="koala" />
          <env name="RUN_UNITTEST" value="TEST" />
          <env name="" value="" />
        </envs>
        <ui>bdd</ui>
        <extra-mocha-options />
        <test-directory>$PROJECT_DIR$/test/</test-directory>
        <recursive>true</recursive>
        <method />
      </configuration>
    </component>
  
  
config.sh

      
    #!/bin/bash
    echo 'mkdir $PROJECT_DIR$/.idea/runConfigurations'
    mkdir -p ../../.idea/runConfigurations
    
    echo 'copy files to  $PROJECT_DIR$/.idea/runConfigurations'
    cp Mocha_Coverage.xml ../../.idea/runConfigurations/


![webstrom config 效果图][1]

![运行效果图][2]



  [1]: http://7xk67t.com1.z0.glb.clouddn.com/configMocha.png
  [2]: http://7xk67t.com1.z0.glb.clouddn.com/mocha.png 
