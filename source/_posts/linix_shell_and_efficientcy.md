# shell script

### Hello World

    #!/bin/bash
    echo 'Hello Bash World'

### 定义function
你可以用function 函数名{} 
或者 直接函数名

    #!/bin/bash
    #usage(){
    function usage(){
    	echo "sleep seconds"
    }
    
    if [ $# == 1 ]; then
    	getSomeSleep $1
    else
    	usage
    fi

[ref link][1]


### input parameters

 - $# 是传给脚本的参数个数 
 - $0 是脚本本身的名字 
 - $1是传递给该shell脚本的第一个参数
 - $2是传递给该shell脚本的第二个参数
 - $@ 是传给脚本的所有参数的列表


[ref link][2]
    
    if [ $# -lt 1 ];then
            usage
            exit 1
    fi

### function parameters

    function getSomeSleep(){
                SLEEP_SEC=$1
                SLEEP_MAX_SEC=30
                #minus=`echo $SLEEP_SEC/1 | bc`
                if [ $SLEEP_SEC -gt 0 ] && [ $SLEEP_SEC -lt 30 ]; then
                    echo '###################'
                    echo 'now try to restart node after ' $SLEEP_SEC ' sec'
                    echo '###################'
                    sleep $SLEEP_SEC
                else
                    echo 'sec must between 0 and ' $SLEEP_MAX_SEC ' sec'
                fi  
        }

### 注意输入参数在函数中, $0 $1 $2 .. $@会被覆盖，但$#不会


### call from another script file
    直接输入绝对路径或者相对路径
    
### .


### option 

### help

### man

### path

### use it to improve efficiency

### alias

### bin 

### path env

### grep egrep awk 

### regex

### fucksh zash

### tips
 

    $apt-get install tree
    $sudo !!
    klg@klgaliyun01:~$ vim /etc/init.d/haproxy 
    klg@klgaliyun01:~$ cat !$
    cat /etc/init.d/haproxy

### mount to shared drive

### UI Color

### copy paste
    pbcopy < somefile
    pbpaste > somefile_copy 

### pipe line
    |
    
### 坑
    空格：[ ] in if then else fi
    
### debug

### exit code

    $ ./sleep.sh 2 3
    usage: sleep seconds
    $ echo $?
    1

[ref link][3]


  [1]: http://www.cnblogs.com/chengmo/archive/2010/10/01/1839942.html
  [2]: http://www.cnblogs.com/no7dw/archive/2010/12/23/1915180.html
  [3]: https://github.com/no7dw/linux-shell
