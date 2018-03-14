# 磁盘管理：
- `df`

- tar

# 进程管理

- `lsof`：查看打开的文件，常用参数(以下参数均为被打开文件的筛选)：  
-i  端口`lsof -i:22`: 查看22端口占用情况
-u  用户名  
-c  进程名  
-p  进程ID  
-d  指定目录下


- `ps`: 查询正在运行的进程，常用参数：  
-e: 显示所有进程  
-f: 按所有格式显示
```
用户名 进程ID    父进程ID      存活时间    终端号 TIME    命令详情
```
- `top`: 实时显示（建议`htop`）
常用的排序条件：    
P：CPU百分比  
M：内存百分比  
i：不显示闲置进程  
d：改变刷新频率  
```
top - 16:35:14 up  7:26,  9 users,  load average: 0.62, 0.88, 0.94
Tasks: 288 total,   1 running, 285 sleeping,   0 stopped,   2 zombie
%Cpu(s):  4.4 us,  5.1 sy,  0.0 ni, 88.7 id,  1.6 wa,  0.0 hi,  0.1 si,  0.0 st
KiB Mem:  24640704 total, 15863844 used,  8776860 free,   571408 buffers
KiB Swap: 25115644 total,        0 used, 25115644 free.  2041396 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
 7607 alex      20   0 10.452g 8.383g 8.251g S  23.6 35.7 185:48.56 VirtualBox  
 5610 alex      20   0 5242408 1.027g 112072 S   5.7  4.4  22:01.34 java        
 5087 alex      20   0 2297724 558492 487212 S   0.7  2.3   3:12.31 VBoxHeadle+ 
 5534 alex      20   0 1615856 380136 195096 S   0.7  1.5   7:11.66 chrome      
 7854 alex      20   0 1280768 192876  97012 S   0.0  0.8   0:55.66 chrome      
```
1. load average: CPU负载压力，1分钟，5分钟，15分钟
2. Tasks: 各进程状态
3. %CPU(s): CPU占用百分比, 
- us 用户态进程使用
- sy 系统态进程使用
- ni 用户态进程中改变过优先级的进程
- id 闲置（IDLE）
- wa 等待输入输出的（可能存在I/O瓶颈）
- hi 硬件中断占用百分比
- si 软中断占用
- st 虚拟机占用
4. KiB（内存占用）
5. 进程列表中：
- PR（优先级）
- VIRT：虚存
- RES： 物理内存
- SHR：共享内存大小
- S：status，D: 不可中断的睡眠 | R: 运行或等待（都是好的） | S: 睡眠  | T: 跟踪/停止 | Z:僵尸



# 性能监控

- `sar`: 采样并查看系统情况(Save Activity Report), 参数:  
-u: 查看CPU使用情况  
-r: 查看内存使用情况  
-W: 查看页交换情况(是否虚存更新过快)  
使用格式`sar -arguments frequency count`

- `du`: 查看内存空间

- `free`: 查看内存使用情况

- `vmstat`： 虚存使用情况

- `iostat`: I/O