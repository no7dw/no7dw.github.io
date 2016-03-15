title: improve-sailsjs-bootup-speed
date: 2016-03-15 15:25:24
tags:
---
# 优化sails.js 启动速度


---
项目使用**sails.js** , 长期开发后，发现启动速度越来越慢。慢得像java 一样。每次修改要一点点，自动重启要等等等。。。。
所以类似有人这么评价 [sails.js][1]

###why? and how to improve it?

sails-mongo 升级到 0.12.x .注意：1 update 只会返回id
好的点：项目启动速度由30+sec , 升级到5+ sec

sails 升级到 0.11.x ，可以使用[https://github.com/sgress454/sails-hook-autoreload][2]
不用 [node-dev / forever / nodemon][3]

sails-hook-autoreload 效果：


    info: 
    info:                .-..-.
    info: 
    info:    Sails              <|    .-..-.
    info:    v0.11.5             |\
    info:                       /|.\
    info:                      / || \
    info:                    ,'  |'  \
    info:                 .-'.-==|/_—'
    info:                 `—'———' 
    info:    __—___—___—___—___—___—___
    info:  ____—___—___—___—___—___—___-__
    info: 
    info: Server lifted in `/Users/dengwei/project/sails_demo`
    info: To see your app, visit http://localhost:9039
    info: To shut down Sails, press <CTRL> + C at any time.
    
    debug: ————————————————————————————
    debug: :: Tue Mar 15 2016 13:46:36 GMT+0800 (CST)
    
    debug: Environment : development
    debug: Port        : 9039
    debug: ————————————————————————————
    verbose: sails-hook-sockets :: connected to Sails admin message bus.
    *****###@@@@@$$$$$111222 login
    verbose: Detected API change ———————————————————————————— reloading controllers / models…
    verbose: Loading the app's models and adapters…
    verbose: Loading app models…
    verbose: Loading app adapters…
    verbose: Starting ORM…
      i18n:debug will write to /Users/dengwei/project/sails_demo/config/locales/en.json +35s
      i18n:debug read /Users/dengwei/project/sails_demo/config/locales/en.json for locale: en +1ms
      i18n:debug will write to /Users/dengwei/project/sails_demo/config/locales/es.json +1ms
      i18n:debug read /Users/dengwei/project/sails_demo/config/locales/es.json for locale: es +0ms
      i18n:debug will write to /Users/dengwei/project/sails_demo/config/locales/fr.json +1ms
      i18n:debug read /Users/dengwei/project/sails_demo/config/locales/fr.json for locale: fr +0ms
      i18n:debug will write to /Users/dengwei/project/sails_demo/config/locales/de.json +0ms
      i18n:debug read /Users/dengwei/project/sails_demo/config/locales/de.json for locale: de +0ms
    verbose: Loading app services…
    verbose: Policy-controller bindings complete!
    *****###@@@@@$$$$$0000111222 login

1111****### @@@@@$$$$$111222 login  
没有重启，
1111****### @@@@@$$$$$0000111222 login 更新了代码。

config:

    // [your-sails-app]/config/autoreload.js
    module.exports.autoreload = {
      active: true,
      usePolling: false,
      dirs: [
        "api/models",
        "api/controllers",
        "api/services",
        "config/locales"
      ],
      ignored: [
        // Ignore all files with .ts extension
        "**.ts"
      ]
    };

如果你试验不行，如[issue here][4] 请注意版本。
sails 0.11.x sails-mongo 0.12.x 

除了sails-hook-autoreload,我还试验过, 但不是太理想：
[node-hot-reload][5]
[node-hot-reload][6]
[hot-reload][7]


  [1]: http://nathanleclaire.com/blog/2013/12/28/the-good-the-bad-and-the-ugly-of-sails-dot-js-realtime-javascript-mvc-framework/
  [2]: https://github.com/sgress454/sails-hook-autoreload
  [3]: https://coderwall.com/p/njcr7w/sails-js-sick-of-restarting-your-server
  [4]: https://github.com/sgress454/sails-hook-autoreload/issues/45
  [5]: https://github.com/philidem/node-hot-reload
  [6]: https://github.com/shimondoodkin/node-hot-reload
  [7]: https://www.npmjs.com/package/hot-reload
