﻿# Deadlock

---

## PART1：死锁截图
经过我多次测试，我发现死锁停在的次数都不一样，少则第90次多则第300多次，下面贴出部分截图  
死锁在第90次：  
![死锁在第90次][1]  
死锁在第168次：  
![死锁在第168次][2]  
死锁在第323次：  
![死锁在第323次][3]

## PART2：死锁的条件
1. 互斥条件：进程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内某资源仅为一个进程所占有。此时若有其他进程请求该资源，则请求进程只能等待。
2. 不剥夺条件：进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能由获得该资源的进程自己来释放（只能是主动释放)。
3. 请求和保持条件：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被其他进程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。
4. 循环等待条件：存在一种进程资源的循环等待链，链中每一个进程已获得的资源同时被链中下一个进程所请求。即存在一个处于等待状态的进程集合{Pl, P2, ..., pn}，其中Pi等待的资源被P(i+1)占有（i=0, 1, ..., n-1)，Pn等待的资源被P0占有

## PART3：对上述程序产生死锁的解释
当t.start()之后，以下两件事同时发生：  

1. 线程t就被插入到调度队列里，当调度到他的时候，就跑run()里面的b.methodB(a)，即执行a.last()
2. count数了20000下之后，跑a.methodA(b)，即执行b.last()  
  
因为a.last()和b.last()都有关键字synchronized，所以在同一时刻最多只有一个线程执行其中一个进程。当多次运行后，其中第一个进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被第二个进程占有，此时请求进程被阻塞，但它对已获得的资源保持不放。但第二个进程同时又需要第一个进程保持不放的资源，所以两个进程都处于等待状态，造成死锁。



  [1]: http://ww3.sinaimg.cn/large/69347328jw1f92ilayz8qj203x08nmy6.jpg
  [2]: http://ww1.sinaimg.cn/large/69347328jw1f92imrtn4jj204z08mjse.jpg
  [3]: http://ww2.sinaimg.cn/large/69347328jw1f9lyer3drpj204v08oq3z.jpg