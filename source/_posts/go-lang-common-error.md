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


#### wrong not using go keyworld when call async func

