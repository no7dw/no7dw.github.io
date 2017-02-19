title: less if else
date: 2016-11-24 19:28:11
tags:
---

### lesser if else
首先声明，不是要消除if
而是，减少一些不必要的if判断，使得代码更优雅，更重要的是提高可维护性

### most easy
  use Ternary: 

  ```
  var result = condiction? trueResult : falseResult;  
  ```

  缺点：case 超过2个就不容易了

### use switch

###  in static type language

 - use heritance

  usage (c++):

  ```
    SonClasss son = new FatherClass()
    son.doSomething()

  ```

  in Son class

  ```
    protected void doSomething(){
      //here override the FatherClass implement
      print("I did something different from my father");
    }
  ```


 - use polypeptide


### in dynamic type language

  also: you can use heritance way in class:

```
  //es5
  function Son(){};
  util.inherits(Son, Father);
  Son.prototype.doSomething(){
    console.log("I did something different from my father");
  }
  //usage:
  var son = new Son();
  son.doSomething();
```


```
  //es6 use extends
  //

```
in dynamic lange, your choice become something different and sometimes difficult

 - use array 
  
```
  //usage:
  //req.query = {"name":"Wade", "surname": "Deng"}
  SonClass son = new ClassFactory(req.query.name);
  son.doSomething();
  
  //ClassFactory implement   , if else hidden in ClassFactory
  function ClassFactory(name){
    this.name = name;
    this.subClass = name == "Wade" ? new subClass(name)  : new sub2Class(name);
  }
  

```

  - use key-value json

  ```
  SonClass.prototype.doSomething(actionName){
    var do = {
      'cry' :{ console.log('cry');}
      'eat' :{ console.log('eat');}
    }
    return do(actionName);
  }
  ```

However , sometime reading a few if else statements is easy.

### ref
[anti-if-else-patterns]（http://www.techug.com/anti-if-the-missing-patterns）  
[no more ifs alternatives](https://javascriptweblog.wordpress.com/2010/07/26/no-more-ifs-alternatives-to-statement-branching-in-javascript/)
[anti-anti-if](https://8thlight.com/blog/wai-lee-chin-feman/2013/08/11/anti-anti-if.html)