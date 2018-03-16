# 进程同步
核心：资源的抢占和利用


## 临界资源
一次仅允许被一个进程使用的资源（得被抢着用的），比如某些变量，数据，物理设备等  

临界资源的访问过程（代码实现）

```lang=C
do {
    entry section;  //进入区:在这里检查是否可以进入临界区，
                    //可以的话，设置临界区访问Flag为true, 不可以则等待
    critical section;  //临界区，访问资源，干活
    exit section;  //退出区，清除临界区访问Flag
    remainder section;  //剩余区，多余部分
} while (true)
```


## 同步 互斥
实质都是进程间的制约关系

- 同步：两个进程需要合作时，进入直接制约关系，举例来说：  

    chan<- A 与 chan->B  
    没有A的写入，则B无法读(读阻塞);  
    没有B的读取，A无法继续写(写阻塞);  
    只有两者同时进行读写->正常进行

- 互斥：对临界资源的抢占而产生的关系，间接制约关系  

    A, B需要一个资源，A在用的时候，B阻塞；A用完了，B改回Ready
    
- 准则  
    - 空闲让进(临界区空闲，允许进程调用)
    - 忙则等待(临界区被用，其他等待)
    - 有限等待(请求临界区的进程，保证其等待时间有限)
    - 让权等待(进程不能进入临界区，应释放处理器，防止进程忙等待[Running但是不处理])


## 临界区互斥算法

- 单标志
flag代表进程ID, 不同的ID代表该进程可访问临界区
```lang=php
PID=0                           PID=1
while($flag!=0);                while($flag!=0);
//do something critical         //do something critical
$flag = 1;                      $flag = 0;           
//do something else             //do something else
```
