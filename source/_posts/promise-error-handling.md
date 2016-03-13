title: promise-error-handling
date: 2016-03-13 23:31:43
tags:
---
# promise 方式处理异常
---
如果以前是习惯callback 的写法，要适应promise 写法，可以这样理解：

promise 处理的**结果**有reject(有错回调), resolve（正常回调） 两种状态。也正是相对于我们平时异步回调的两种结果（err, result）

 - err --> 对应reject 的返回
 - result -->  对应 resolve 的返回

 

    someAsyncFunc(para, function(err, result){
     });


来练习：

    var laterError = function(){
    return new Promise(function (resolve, reject) {
        setTimeout(function () {
          try {
            var a = {};
            console.log(a.b.c);//simulate an error :TypeError: Cannot read property 'c' of undefined
            resolve(a);
          }catch(e) {
            reject(e)//TypeError: Cannot read property 'c' of undefined
          }
        }, 100);
      });
    };
    
    //写法1:
    laterError().then(function(value) {
      // success
      console.log('success value', value);
    }, function(value) {
      // failure
      console.log('failure value', value);
    });
    
    //更推荐写法2:
    laterError().then(function(value) {
    // success
      console.log('success value', value);
    }).catch( function(err) {
      // failure
      console.log('failure value', err);
    });

运行结果：

    dengwei@dengweis-MacBook-Pro:~/project/github/es6-demo/syntax$ node promise2.js 
    Hi!
    failure value [TypeError: Cannot read property 'c' of undefined]

异常被正常处理了

写法2更加容易易懂，类似同步代码
如果你习惯teambition 的 [thenjs][1] 库，你会觉得写法2与thenjs 无异。


  [1]: https://www.npmjs.com/package/thenjs
