title: less if else
date: 2016-11-24 19:28:11
tags:
---

### lesser if else

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

  also: you can use heritance way  in class:

```
  function Son(){};
  util.inherits(Son, Father);
  Son.prototype.doSomething(){
    console.log("I did something different from my father");
  }
  //usage:
  var son = new Son();
  son.doSomething();
```

in dynamic lange, your choice become something different and sometimes difficult

 - use array or key-value
  
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



### ref
[anti-if-else-patterns]（http://www.techug.com/anti-if-the-missing-patterns）  
