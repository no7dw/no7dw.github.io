title: 使用webstorm 配置文件配置mocha
---
Webstorm 的运行配置文件。
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

  
