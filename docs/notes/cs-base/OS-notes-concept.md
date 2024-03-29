## 操作系统结构

**操作系统**

是一组控制和管理计算机软硬件资源，合理地对各类作业进行调度及方便用户使用的程序集合，是计算机的第一层软件。



**系统结构**

* 简单结构
* 模块化结构
* 层次结构
* 微内核结构



**启动过程**

1. 打开电源
2. CPU通过系统时钟初始化
3. 在BIOS中找到启动程序的CPU的第一条指令
4. 执行开机自检(POST)，检查所有硬件设备



**I/O操作**

IO设备可以和CPU同时执行
- I/O在设备和控制器缓存（controller's buffer）之间转移数据
- CPU在控制器和主存之间转移数据。

I/O设备访问
- Memory Mapped I/O（CPU就像访问内存一样访问I/O）
- Programmed I/O（每个控制寄存器都分配一个I/O端口号，CPU使用特定的I/O指令去读写寄存器）。



**操作系统的服务**

- 程序执行 Program execution
- I/O操作 I/O operations
- 文件系统的操作 File-system manipulation
- Communications 在进程间传递消息
- 错误检测 Error detection

- 资源分配 Resource allocation
- 账号管理 Accounting 
- 保护 Protection



**系统组件**

- **Process Management**
- **Memory Management**
- **File Management**
- **I/O System Management**
- **Secondary Management**
- Networking
- Protection System
- Command-Interpreter System



****

## Process 进程

**进程 Process**

a program in execution一个执行过程中的程序，是资源分配的单位。

- Program（executable）：磁盘上的passive实体
- Process：active entity被激活的实体，包含程序计数器、栈、堆、文本、数据。

**程序 Program**

一个程序包含Code、Data、DLLs 动态链接库、mapped files。

**Process vs Program**

- 两个进程可能和同一个程序相关联

- 两条单独执行的顺序
- 文本部分相同，数据、堆、栈都不同

**进程状态**

- running：指令正在执行
- waiting：进程等待某个事件发生
- ready：进程等待分配给CPU

- new：进程被创建
- terminated：进程结束执行

![process states](https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1474977550,2975918179&fm=26&gp=0.jpg)



**进程控制块（PCB）**

和每个进程相关联的信息，包含：

- Process state 进程状态
- Program counter 程序计数器
- CPU register 寄存器：为了能让CPU离开进程后回来还能执行
- CPU scheduling information CPU调度信息
- Memory-management information 内存管理信息



**进程调度**

- 长程调度Long-term scheduling(or job scheduling)：从job queue中选一个到ready queue中,内存可用时,从磁盘中移出一个到内存。
- 短程调度Short-term scheduling(or CPU scheduling)：从ready queue中选一个到running，内存到执行。
- 中程调度(Medium-term scheduling)：CPU和内存资源管理的混合
- 中断处理(Interrupt handling)：从waiting到ready



**进程通信**

- **shared memory** 共享内存
- **message passing** 消息传递

****

## Threads 线程

进程是资源的拥有者，线程用来调度任务，是CPU的调度单位。

- 独立的 PC, Register, Stack pointer
- 共享的 Code, Data, File


![thread](https://s2.ax1x.com/2019/07/31/eNrmO1.md.png)

**两类线程**

- 用户级线程，所有线程在user space中完成
- 核心级线程，OS直接执行



**多线程编程**

响应度高，资源共享，能够充分利用多处理器体系结构。包括了多对一，一对一，多对多线程模型。

------

## CPU Scheduling

**调度类型**

- 非抢占式 ：1.从running到waiting，2.Terminates
- 抢占式：1.从waiting到ready，2.从running到ready

**如何调度**

1. 切换上下文
2. 转换为use mode
3. 跳转到用户程序中的适当位置来重启程序

**Scheduling criteria 性能指标**

- Max CPU utilization——让CPU越忙越好
- Max Throughput——单位时间内能完成的进程个数越多越好
- Min Turnround time——周转时间越短越好
- Min Waiting time——等待时间
- Min Response time——响应时间指从请求提交到第一个相应产生，并非输出产生

**常用调度算法**

- **FCFS（First-Come, First-Served ）**：FCFS最简单，是非抢占式的，进程按照请求CPU的顺序排序。
- **SJF（Shortest Job First）**：也是非抢占式的，如果运行时间事先已知，可以每次挑选最短的任务以避免小进程等待大进程释放的情况。SJF最优是可以证明的。但是难以得知下次CPU请求的长度，只能预测。一般通过数学期望 $t_{n+1}=at_{n}+(1-a)t_{n}$ 来确定。
- **Priority（Priority Scheduling）**：可以是抢占式的，也可以是非抢占式的，优先级最高的进程被选出来运行。最大的问题是可能会出现无限等待或starvation，优先级低的可能永远无法运行。可以使用老化(aging)进行解决，即每执行一个周期就把当前进程优先级降一下。
- **RR （Round-Robin Scheduling）**：先给定时间片（time quantum）的大小。Case 1: CPU burst <= one time quantum，执行完就退出执行其他任务；Case 2: CPU burst > one time quantum，执行不完将该任务放到队尾。



****

## Process Synchronization



**临界区问题**

每个进程拥有的操作共享数据的代码称为`critical section 临界区`。要保证当一个进程执行时它的临界区代码时，其他进程不能执行临界区代码。

解决方案：在执行之前加点条件，判断能否执行，执行完毕后解锁.

```c
do{
    entry section   //执行前判断
        crical section
    exit section    //执行完毕后解锁
        reminder section
}
```

解决临界区问题需要满足

- Mutual Exclusion  互斥

- Progress  前进原则,如果没有临界区代码在执行，有一个进程想进入临界区是，应当允许

- Bounded wait 有限等待,想进入临界区的在有限时间内总能进入。

  

**信号量Semaphores **

信号量 一个整形变量，只能通过两个标准原子操作：`wait()，signal()`。

```c
// wait(s):
while(s<=0)
;   //no-operation
s--;

//signal(s):
s++
```

在wait()和signal()操作中，对信号量的修改必须是原子的，即当一个进程修改信号量值时，不能有其他进程同时修改同一信号的值。

```c
//初始化：int matex=1; 
do {
    wait(mutex);
    //critical section
    signal(mutex);
    //remainder section
} while(1);
```



**信号量实现**

主要缺点是忙等待，即当一个进程位于其临界区内时，其他想进入临界区的进程必须连续循环等待，浪费了CPU时钟。也称为自旋锁。为了克服忙等待，可以使用阻塞并放入等待队列的操作。通过wakeup()来重新执行。

```c
//定义一个新结构
typedef struct {
    int value;
    struct process *List;
} semaphore;

//新的信号量操作定义
wait(semaphore *S){
    S.value--;
    if(S.value<0) {
        block();    //add this process to S.List
    }
}

signal(semaphore *S) {
    S.value++;
    if(S.value<=0) {
        wakeup(P);  //remove a process P from S.List
    }
}
```



**读写者问题**

1. 写者、读者互斥访问文件资源。
2. 多个读者可以同时访问文件资源。
3. 只允许一个写者访问文件资源。

**读者优先**

- 没有读者会因为有一个写者在等待而去等待其他读者的完成
- 写者执行写操作前，应该让所有读者和写者退出
- 除非有一个写者在访问临界区，其他情况下，读者不应该等待

```c
//初始化变量
BINARY_SEMAPHORE wrt=1;
BINARY_SEMAPHORE mutex=1;
int readcount=0;

//Reader:
do {
    wait(mutex);      //Allow 1 reader in entry
    readcount=readcount+1;
    if(readcount==1)
        wait(wrt);    //1st reader locks writer
    signal(mutex);
        //reading operation
    wait(mutex);
    readcount=readcount-1;
    if(readcount==0)
        signal(wrt);    //last reader frees writer
    signal(mutex);
}

//Writer:
do {
    wait(wrt);
    	//writing operation
    signal(wrt);
}
```

**写者优先**

如果一个写者等待访问对象，那么不会有新读者开始读操作。

```c
//初始化变量
BINARY_SEMAPHORE read=1;    //使有写者进行操作时读者等待
BINARY_SEMAPHORE file=1;    //使文件操作互斥
BINARY_SEMAPHORE mutex1=1;  //使改变readcount的方法互斥
BINARY_SEMAPHORE mutex2=1;  //使改变writecount的方法互斥
int readcount=0;
int writecount=0;

//Reader:
do {
    wait(read);         //等待直至读者队列没有阻塞，即全部写者都退出，同时自身进入后再次上锁，使后来的读者等待，因为接下来要进行一系列互斥的操作
    wait(mutex1);
    readcount++;
    if(readcount==1)    //第一个读者进入
        wait(file);     //后来的写者无法操作文件，但对后来的读者的文件操作不造成影响
    signal(mutex1);
    signal(read);       //释放
        //read operation
    wait(mutex1);
    readcount--;
    if(readcount==0)
        signal(file);
    signal(mutex1);
}

//Writer:
do {
    wait(mutex2);
    writecount++;
    if(writecount==1)   //第一个写者进入
        wait(read);     //阻塞读者队列
    signal(mutex2);
    wait(file);
        //write operation
    signal(file);
    wait(mutex2);
    writecount--;
    if(writecount==0)   //最后一个写者退出
        signal(read);   //释放读者队列
    signal(mutex2);
}

```



**哲学家进餐问题**

5个哲学家围着桌子吃饭，筷子只有5枝，分别在每个哲学家的左手边和右手边。哲学家必须得到两只筷子才能吃饭，要使每个哲学家都能吃到饭，该如何调度。

```java
// 管程实现
void philosopher(int i) {   //主方法
    while(true) {
        thinking();
        dp.pickup(i);
        eating();
        dp.putdown(i);
    }
}

monitor dp {
    enum{thinking,hungry,eating} state[5];
    condition self[5];
    void pickup(int i) {
        state[i]=hungry;    //设置本人为饥饿状态
        test[i];            //测试左右两个人是否在吃
        if(state[i]!=eating)//如果需要等待
            self[i].wait(); //进入等待，在pickup方法处阻塞
    }
    void putdown(int i) {
        state[i]=thinking;
        test((i+4)%5);      
        test((i+1)%5);      //本人放下筷子后，状态发生了变化，测试左右两个是否等待吃，如有，则给他们开始吃的机会（但也不一定能马上开始吃）
    }
    void test(int i) {
        if((state[(i+4)%5]!=eating) && (state[i]==hungry) && (state[(i+1)%5]!=eating)) {     //如果左右两个人并没有在吃，而且自己处于饥饿状态
            state[i]=eating;    //设置状态为eating
            self[i].signal();   //释放pickup方法，在philosopher方法中下一步将执行eating操作
        }
    }
    initializationCode() {
        for(int i=0;i<5;i++)
            state[i]=thinking;
    }
}
```



**生产者消费者问题**

一个生产者，一个消费者，库存有限，如何能持续生产？解决方法：对于生产者，如果缓存是满的就去睡觉。消费者从缓存中取走数据后就叫醒生产者，让它再次将缓存填满。若消费者发现缓存是空的，就去睡觉了。下一轮中生产者将数据写入后就叫醒消费者。

**伪代码**

```c
// 初始化变量：
semaphore mutex=1; //临界区互斥信号量
semaphore empty=n;  //空闲库存空间
semaphore full=0;  //库存初始化为空


//生产者进程：
producer ()
{
    while(1)
    {
        produce an item in nextp;  //生产数据
        P(empty);  //获取空闲库存单元
        P(mutex);  //进入临界区.
        add nextp to buffer;  //将数据放入缓冲区
        V(mutex);  //离开临界区,释放互斥信号量
        V(full);  //库存数加1
    }
}

//消费者进程：
consumer ()
{
    while(1)
    {
        P(full);  //获取库存数单元
        P(mutex);  // 进入临界区
        remove an item from buffer;  //从库存中取出数据
        V (mutex);  //离开临界区，释放互斥信号量
        V (empty) ;  //空闲库存数加1
        consume the item;  //消费数据
    }
}

```



****

## Deadlock

**死锁的必要条件**

- Mutual exclusion 互斥访问
- Hold and wait 占有和等待
- No preemption 非抢占
- Circular wait 循环等待

**处理死锁的方法**：忽略，预防，避免，检测恢复

**死锁预防**：使死锁条件不成立。

1. 打破互斥，不是所有的资源都行

2. 打破Hold and wait，在运行前把所有所需资源拿来（不容易），请求资源时把占有的资源都释放（很好）。

3. 打破非抢占，不可能

4. 打破循环等待，和hold and wait类似，有序的 资源访问。

   

**死锁避免**：判断请求在未来是否出现死锁（Banker's Algorithm 银行家算法）

- 每个新进程必须声明它所需每种资源的最大实例数

- 进程请求时，系统必须确认是否分配资源会让系统不安全

- 当一个进程得到所有所需资源后，它必须在有限时间内归还

  

**死锁检测**

* 等待图：每种资源类型只有单个实例的情况
* 与银行家算法类似的检测算法：每种资源类型可有多个实例的情况。

**死锁恢复**

- 终止所有死锁进程
- 一次只终止一个进程，知道取消死锁循环为止



****

## Memory  management

**存储类型**

- Main memory 主存
- Secondary storage 辅存

**地址表示**

- 逻辑地址（Logical address）： CPU生成 也叫Virtual address
- 物理地址（Physical address） ：实际在内存中的地址



**内存分配**

- 首次适应First-fit：分配按地址顺序的第一个可用的hole
- 最佳适应Best-fit：放到最小的足够大的hole
- 最差适应Worst-fit：放到最大的hole

First-fit通常比Best-fit好，First-fit和Best-fit通常比Worst-fit好



**外部碎片**

不同进程间还未分配出去的小内存空间，单个碎片无法分配给任何一个进程。外部碎片存在于可变分区、段式虚拟存储系统(分段存储 Segmentation)中，主要是可变分区。



**内部碎片**

已经被分配出去的大于请求所需的内存空间部分，在固定分区中表现为分区内进程未使用的内存空间。无法重新利用，只能被浪费。内部碎片存在于固定分区、页式虚拟存储系统（分页存储 Paging)中，主要是固定分区。



**Paging 分页**

- 把物理内存分为大小相等的块，称为**Frame（帧）**
- 把逻辑内存分为大小相等的块，称为**Page（页）** 页的大小和帧相同
- 追踪所有空闲帧，必要时可以找到n个空闲帧加载进程
- 建立 **Page table（页表）** 记录Page和Frame的映射 页表的数据列只有一列

CPU产生的地址被分为<Page number(p) +Page offset(d)>

**分页的特点**

- 清楚划分物理地址和逻辑地址
- 动态重定位
- 没有外部碎片，但会有一些内部碎片（最后一页）
- Page大小通常在4KB-8KB

**TLB**

一种支持并行搜索的高速硬件，先在TLB里面查，如果页号在里面，就取出相应的帧号，如果不在再去内存的页表找。

![TLB](https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike150%2C5%2C5%2C150%2C50/sign=354cef9bfd1f4134f43a0d2c4476feaf/dcc451da81cb39db25af152ed0160924ab18305e.jpg)



**分段**

分为大小不同的段，用来存一组相对完整的逻辑信息，逻辑地址包含 **<段号，偏移>**

**段表**

- Segment-table base register(STBR) 指向段表起始位置  
- Segment-table length register(STLR) 指明段数量



****

## Virtual memory 

**虚拟内存**：将用户逻辑内存从物理内存分离

- 只有部分程序需要在内存中执行
- 因此逻辑内存空间可以比内存空间大很多
- 允许地址空间被多个进程共享

**按需分页**：当需要的时候才把页放入内存

**页面置换**

1. 在磁盘上找到需要的页的位置
2. 在内存上找到空闲帧
3. 如果没有，放一个牺牲帧到磁盘
4. 将页放入空闲帧，更新页表

**页置换算法**

* FIFO：先进先出
* OPT：置换最长时间不会使用的页
* LRU：置换最近最少从访问的页
* Clock：两次机会



**系统颠簸**

内存没有足够帧，进程忙于交换页，此时CPU利用率极低。



****

## File system

**文件系统**

- 存大量数据

- 进程停止后数据能保存

- 多个进程可以并发访问

  

**文件系统架构（自上而下）**

| 层级                     | 操作、实例                       |
| ------------------------ | -------------------------------- |
| App, Programs            | 发出文件请求的代码               |
| Logical File System      | OS最高层。保护、安全、文件名解析 |
| File-organization Module | 逻辑块                           |
| Basic File System        | 具体文件块                       |
| I/O Control              | 驱动、中断                       |
| Devices                  | Disk,tapes                       |



**分配磁盘块方法**

- Contiguous allocation 连续分配
- Linked allocation 链接分配
- Indexed allocation 索引分配



**空闲空间管理**

* 空闲表法

* Bit vector 位示图法

* Linked list 空闲链表法

  

****

## I/O System

**I/O Device**

- Block devices 块设备：由于信息的存取总是以数据块为单位的，所以存储信息的设备称为块设备，如硬盘，每个都有自己的地址，可寻址。
- Character devices 字符设备：用于数据输入输出的设备为字符设备，因为其传输的基本单位是字符，如打印机，不可寻址。

**I/O software layers**

- 中断处理
- 设备驱动
- 设备无关的I/O软件

**Kernel I/O Subsystem**

- Scheduling
- Buffering 缓冲，在设备传输时存数据，解决设备速度不一致。
- Caching 缓存，快速存储数据备份。
- Spooling，保存设备输出。



****

## Disk 磁盘

**磁盘结构**

磁盘物理地址：<磁头号，磁道号，扇区号>

**访问时间**

访问时间有两部分：1、**Seek time** 磁头寻道时间。2、Rotational latency 磁盘转到合适位置需要的时间。

**磁盘调度算法**

**FCFS**：遵循先来先服务原则。

**SSTF**：最短寻道时间优先  离磁头当前位置最近的优先。

**SCAN **：先按一个方向移动至边界，再换另一个方向移动。

**C-SCAN**：先向一个方向移动至边界，直接到磁盘开始位置，再按同一方向移动。

**LOOk**：先按一个方向移动，到最远请求为止，再换另一个方向移动。

**C-LOOK**：先向一个方向移动，回来到最远的访问点，再按同一方向移动。

