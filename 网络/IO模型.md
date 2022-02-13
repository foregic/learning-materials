

### IO复用

多进程/多线程并发模型，为每个socket分配一个进程/线程

IO（多路）复用，采用单个进程/线程就可以管理多个socket

网络设备；交换机，大型游戏后台；ngnix、redis

IO复用有三种方案：

- select
- poll
- epoll

#### select流程

1. 创建socket的集合fd_set
2. 把监听的socket和客户端的socket加入集合fd_set
3. select（maxfd，fd_set，NULL，NULL，NULL）
4. FD_ISSET判断fd_set中有事件的socket
5. 1. 监听的socket有事件表示有新客户端的连接请求
   2. 客户端的socket有时间：1有数据可读，2socket连接断开



#### poll流程

|        |      |                                                              |
| ------ | ---- | ------------------------------------------------------------ |
| select | O(N) | 仅仅知道有事件发生了却并不知道是哪几个流，只能无差别轮询所有的流，同时处理的流越多，无差别轮询的时间越长 |
| poll   | O(N) | 将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态，但是没有最大连接数的限制，原因是poll基于链表存储的 |
| epoll  | O(1) | 不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎样的IO事件通知，所以epoll是事件驱动，每个事件关联上fd，所以对流的操作都是有意义的 |

IO多路复用的本质就是监听多个描述符，一旦某个描述符准备就绪（一般是读就绪或者写就绪），通知程序进行响应的读写操作。但select、poll、epoll本质都是同步IO，需要在读写事件后自己负责进行读写，这个读写过程是阻塞的，异步IO是无需自己负责读写的，异步IO的实现会负责把数据从内核拷贝到用户空间

epoll和select都是IO多路复用的解决方案，epoll是linux所特有的，select是POSIX所规定的

epoll提供了三个函数：epoll_create、epoll_ctl和epoll_wait

- epoll_create创建一个epoll句柄
- epoll_ctl注册要监听的事件类型
  - 每次注册洗呢事件到epoll句柄中（在epoll_ctl中制定了EPOLL_CTL_ADD），会把所有的fd拷贝仅内核，而不是在epoll_wait的时候重复拷贝，epoll保证了每个fd在整个过程只会拷贝一次
  - epoll不会像select或poll一样每次都把current轮流加入fd对应的设备等待队列中，而只在epoll_ctl时把current挂一遍，并为每个fd指定一个回调函数，当设备准备就绪，唤醒等待队列上的等待者时，就会调用这个回调函数，而这个回调函数回把准备就绪的fd加入一个就绪链表。epoll_wait的工作实际就是在这个就绪链表中查看有没有就绪的fd（利用schedule_timeout()实现睡一会，判断一会的效果）
- epoll_wait等待事件的产生

