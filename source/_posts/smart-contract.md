title: smart contract 
date: 2018-06-23 23:57:35
tags:
---

https://solidity.readthedocs.io/en/latest/frequently-asked-questions.html#what-is-the-memory-keyword-what-does-it-do

# 知识点


### memory vs storage vs stack


 -  storage , where all the contract state variables reside. Every contract has its own storage and it is persistent between function calls and quite expensive to use.

 - memory , this is used to hold temporary values. It is erased between (external) function calls and is cheaper to use.

 - stack , which is used to hold small local variables. It is almost free to use, but can only hold a limited amount of values.


There are defaults for the storage location depending on which type of variable it concerns:

state variables are always in storage
function arguments are in memory by default
local variables of struct, array or mapping type reference storage by default
local variables of value type (i.e. neither array, nor struct nor mapping) are stored in the stack


### constanst vs view vs pure 

  - constantStarting with solc 0.4.17, constant is deprecated in favor of two new and more specific modifiers.
  - View This is generally the replacement for constant. It indicates that the function will not alter the storage state in any way.
  - Pure This is even more restrictive, indicating that it won't even read the storage state.


```
  function returnTrue() public pure returns(bool response) {
    return true;
  }
```
[ref](https://ethereum.stackexchange.com/questions/28898/when-to-use-view-and-pure-in-place-of-constant?rq=1)

### require vs assert vs revert vs throw(if)

简化：
 require() and revert() will cause refund the remaining gas
 assert() will used up all the remaining gas


 Use require() to:

 - generally it will be used at the begining of a function, validate user inputs : such as invalid input
 - should be used more often 

 use assert() to :

 - prevent condition that should never, ever be possible : such as fatal error
 - generally it will be used at the end of a function
 - should be used less often 

 revert() it was said that revert can handle more complex logic.(emit that, I think we should use require() instead)
 throw :  deprecated


[ref](https://media.consensys.net/when-to-use-revert-assert-and-require-in-solidity-61fb2c0e5a57)

[ref](https://ethereum.stackexchange.com/questions/15166/difference-between-require-and-assert-and-the-difference-between-revert-and-thro)


todo:

lib 
use external source(local sol or git)
call from another contract


ide 调试

string compare
return struct
testing



### FAQ about Error
Error: VM Exception while processing transaction: invalid opcode
-- out of index

Error: Invalid number of arguments to Solidity function
-- 参数不对
