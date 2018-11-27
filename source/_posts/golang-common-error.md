title: go-lang-common-error
date: 2016-08-10 23:58:06
tags:
---
### basic common error

#### import
    import unuse package:
    error : imported and not used: "os" 

#### := = 

    c := 1 // error non-declaration statement outside function body
    d = 1 // error non-declaration statement outside function body

  func test(){
      c = 1 //undefined: should be c:=1
      //d = 1
      var f , d int
      f,d = cal()
      fmt.Println(c, f, d)
  }

---------------
#### right using value declare and use

    package main

    import "fmt"

    //var a := 0 //wrong
    var a int = 0
    //c := 1 //wrong
    //d = 1 //wrong

    func cal(b int)(val1 int, val2 int){
      fmt.Println(b)
      val1 = 1
      val2 = 2
      return 
    }
    func test(){
      c := 1
      //d = 1 //wrong
      var f , d int
      f,d = cal(1)
      fmt.Println(c, f, d)
    }
    func main() {
      fmt.Println("Hello, world var")
    }


#### declear not use
    ./right.go:14: c declared and not used  


#### type struct init using () ,instead of {} ,which {} is the right usage
    
    type Response struct {
      Code string `json:"code"`
      Body string `json:"body"`
    }
    //not like this ()
    //Response("OK", "ECHO: " + method + " ~ " + params)
    //right usage {}
    Response{"OK", "ECHO: " + method + " ~ " + params}


#### 如何理解以下代码：
    
    type IpcClient struct {
      conn chan string
    }
    func (client *IpcClient)Call(method, params string)(resp *Response, err error){
      }

  - (client *IpcClient) -- 调用的对象要是 IpcClient struct  
  - (method, params string) -- 参数，前面是参数名，后面是参数类型，两种同类别省略写法。
  - (resp *Response, err error) 参数返回值，返回值的第一个参数，类型为 Response struct 对象指针, 返回值的第二个参数，类型为 error 类型, 



#### wrong not using go keyworld when call async func


### cannot refer to unexported name
    
    your function name should start with Cap letter: 
```
    func Foo(){
    } 
```
instead of 
```
    func foo(){
    }
```
