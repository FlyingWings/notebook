# 经典进程同步问题

## 生产者-消费者  
仓库总储量为n，满了无法生产，空了无法消费
故而对满，空，临界区(存/取)这三个状态建立信号量
```
semaphore mutex = 0;//处于临界区的资源
semaphore full= 0;//满了的资源数量
semaphore empty = n;//空的资源数量

Producer(){//对单个生产者，循环生产
    while(1){
        P(empty);
        P(mutex);
        //放进去
        V(mutex);
        V(full);
    }
}

Customer(){
    while(1){
        P(full);
        P(mutex);
        //拿出来
        V(mutex);
        V(empty);
    }
}
```
解释：  
对生产者而言   
1. 第一步需要去掉一个空盒子`P(empty)`
2. 临界区请求
3. 第二步，增加一个满的盒子`V(full)`
        
对消费者而言
1. 第一步减少一个满的盒子
2. 访问临界区
3. 增加一个空的盒子


## 读者-作者问题

