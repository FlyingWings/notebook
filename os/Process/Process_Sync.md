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
要求两个进程必须交替进行，一旦出现某个进程退出的情况，另一个就卡死了  
e.g. P0结束后，flag为1, 而P1不进入了，那么对P0而言$flag永远不为0, 无法进入
```lang=php
PID=0                           PID=1
while($flag!=0);                while($flag!=1);
//do something critical         //do something critical
$flag = 1;                      $flag = 0;           
//do something else             //do something else
```


- 双标志法
对多个进程(n>2)抢占一个临界区的情况，利用临界判定数组`flag[n]`来判断第n个进程是否进入临界区  
可能会出现两者同时进入（即判断对方flag之后和写入自己flag之前这段时间，出现两个进程同时修改进入的情况）

```lang=php
PID=i                           PID=j
while($flag[j]);                while($flag[i]);
$flag[i]=true;                  $flag[j]=true;
//do something critical         //do something critical for PID=j
$flag[i]=false;                 $flag[j]=false;
//do something else             //do something else
```

- 双标志法后检查
同上  
问题出在第一步，两者都为True的时候，则两者都无法进入while的后面，都饿死
```
PID=i                           PID=j
$flag[i]=TRUE;                  $flag[j]=TRUE;
while($flag[j]);                while($flag[i]);
//do something critical         //do something critical
flag[i]=false;                  $flag[i]=false;
//do something else             //do something else

```
- Peterson's Algorithm
结合了单标志和双标志后检查，加入变量turn，指示下次允许进入临界区的进程ID  
这样两者即使同时设置为True，也至少有一人可以进入  
```flag=php
PID=i                           PID=j
$flag[i]=TURE; $turn=j;         $flag[j]=TURE; $turn=i;
while($flag[j]&&$turn==j);      while($flag[i]&&$turn==i); 
//do something critical         //do something critical
$flag[i]=false;                 $flag[j]=false;
//do something else             //do something else
```

## 信号量机制
用于涉及多个资源，多个进程的同步-互斥问题，能且仅能被两个原语访问`wait(S)|signal(S)`，也称作P操作和V操作  
- 整型信号量
```lang=c
wait(S){
    while(S<=0);//当S不为正(可利用资源为空)
    S--;//拿掉一个资源
}

signal(S){
    S++;//释放一个资源给别的进程用
}
```
会导致出现忙等待（wait(S)中的while等待）

- 记录型信号量
信号量中含有一个进程链表，保存所有处于等待状态的PCB
```lang=c
typedef struct{
    int value;
    <PCB>*  L; 
} semaphore;

void wait(semaphore S){ //申请资源(成功/等待)
    S.value--;
    if(S.value<0){
        L.append(this);//等待队列中加入这个进程
        block(S.L);//整个队列中的进程置为等待/阻塞
    }
}

void signal(semaphore S){
    S.value++;
    if(S.value<= 0){ //当value<=0, 说明队列不为空，故而取出一个进程，让它跑起来
        P= L.pop();
        wakeup(P);
    }
}
```

- 解决同步问题
当P0, P1为前驱，P0中的x为P1中的y所需要的参数时
```
semaphore = 0;
P0(){
    x = 0;//获取x结果
    V(semaphore);//释放信号量
}

P1(){
    S(semaphroe);//等待信号量
    y = x;//利用x，执行y
}
```
- 解决互斥问题
P0, P1抢占资源M时
```
semaphore = 1;//有一个默认资源M
P0(){
    P(semaphore);//等待
    //do something;//执行
    V(semaphore);//释放
}

P1(){
    P(semaphore);
    //do something;
    V(semaphore);
}
