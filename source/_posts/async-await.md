title: async-await
date: 2016-03-14 00:04:53
tags:
---
# 在async/await 中处理异常

---

async/await 是es7 的特性，如果需要用此特性，需要用到babel.js

async/await 让代码看起来更想同步，看起来更优雅、易懂。

以下使用async/await 结合promise 进行错误处理

    
    function laterQuoteError(type) {
    
      return new Promise(function (resolve, reject) {
        console.log("Hello World from ", type);
        setTimeout(function () {
          if (type == 'succeed')
            resolve("there will be hope if try");
          else if (type == 'fail')
            reject("no quote");
        }, getRandomIntInclusive(100,1000));
    
      })
    }
    
    async function main() {
    
      try {
        console.log(await laterQuoteError('fail'));
        console.log(await laterQuoteError('succeed'));//this line of code will not run
      }
      catch (e) {
        console.log('########get a error in main end');
      }
    };

看：
   

    await laterQuoteError('fail')
    await laterQuoteError('succeed')

**callback hell 完全不见了。**

看再外一层对async 的调用, 使用了 arrow function， 熟悉[coffee-script][1] 的比较喜欢&习惯：

    
    main()
      .then((value) => {
      console.log('##########outside normal end');
    }).catch((err) =>{
      console.log('##########outside catch end', err);
    })

运行结果：

    dengwei@dengweis-MacBook-Pro:~/project/github/es6-demo/syntax$ babel-node await3.js
    start
    Hello World from  fail
    ########get a error in main end
    ##########outside normal end


再修改， 尝试再抛出一个error：

    async function main() {
    try {
        console.log(await laterQuoteError('fail'));
        console.log(await laterQuoteError('succeed'));//this line of code will not run
      }
      catch (e) {
        console.log('########get a error in main end');
        throw new Error(e);//再抛出
      }
    };

再运行，结果：

    dengwei@dengweis-MacBook-Pro:~/project/github/es6-demo/syntax$ babel-node await2.js
    start
    Hello World from  fail
    ########get a error in main end
    ##########get a error in outside end
    
留意**##########get a error in outside end** ---在最外层的catch 处理了：


  [1]: coffeescript.org
