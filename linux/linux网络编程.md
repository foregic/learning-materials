### 3进程控制

##### 3.1.2进程的建立

`fork()`：`pid_t fork(void)`调用成功就会使内核建立一个新的进程

#### 4进程间通信

#### 4.2信号

信号本身不能直接携带信息

信号本身的特点让它用于对非正常情况的处理。

#### 4.3管道

将一个程序的输出和另外一个程序的输入连接起来的单向通道

管道不能提供非父/子进程间通信

#### 4.4有名管道

有名管道在linux系统中以特殊的设备文件的形式存在于文件系统中，不仅具有了管道的通信功能，也具备了普通文件的优点（可以被多个进程共享，可以长期存在等等）

##### 4.4.3有名管道注意

1. 有名管道必须同时有读/写两个进程端
2. 管道操作的独立性。这个操作不会因为任何原因而被中断
3. 

#### 4.5文件和记录锁定

#### 4.6System V IPC

消息队列、信号量和共享内存

#### 4.7消息队列

消息队列就是在系统内核中保存的一个用来保存消息的队列。这个队列并不是简单的进行“先入先出”的操作

ipc_perm结构保存每个IPC对象权限信息

```c++
struct ipc_perm {
	key_t key;	//IPC对象的关键字
    ushort uid;	//uid
    ushort gid;	//gid
    ushort cuid;	//IPC对象的创建者的uid和gid
    ushort cgid;	
    ushort mode;	//IPC对象的存取权限
    ushort seq;		//系统保存的IPC对象的使用频率信息
};
```

msgbuf：定义传递给队列的消息的数据类型

```c++
struct msgbuf {
 long mtype; 	//区分不同的消息数据类型
 char mtext[1]; //消息数据的内容
};
```

msg：消息队列在系统内核中是以消息链表的形式出现的，完成消息链表的每个节点结构定义就是msg结构

```c++
struct msg {
 struct msg *msg_next; //指向消息链表中下一个节点额指针
 long msg_type;	//消息数据类型
 char *msg_spot; //指出了消息内容在内存中的位置
 time_t msg_stime; //
 short msg_ts; //消息内容的长度
};

```

#### 4.8信号量

用控制多个进程对共享资源使用的计数器，被用作一种锁定保护机制，当某个进程在对资源进行操作时，组织其他进程对资源的访问

#### 4.9共享内存

多个进程共享的内存

### 6套接字

##### 6.2.3套接字的三种类型

- 流式套接字（SOCK_STREAM）
- 数据报套接字(SOCK_DGRAM)
- 原始套接字



#### 6.5套接字的一些基本知识

##### 6.5.1

```c++
struct sockaddr_in {
    short int sin_family; /* Internet地址族 */
    unsigned short int sin_port; /* 端口号 */
    struct in_addr sin_addr; /* Internet地址 */
    unsigned char sin_zero[8]; /* 添0（和struct sockaddr一样大小）*/
};
struct in_addr {
    unsigned long s_addr;
};
```

##### 6.5.2基本转换函数

- htons()：主机字节顺序转换为网络字节顺序（无符号短型进行操作）
- htonl()：主机字节顺序转换为网络字节顺序（无符号长整型进行操作）
- ntohs()：网络字节顺序转换为主机字节顺序（对无符号短型进行操作）
- ntohl()：网络字节顺序转换为主机字节顺序（对无符号长整型进行操作）
- inet_addr()：把（"127.0.0.1"）IP地址的字符串转换成一个无符号长整型
- inet_ntoa()：把struct in_addr存储的网络地址以(127.0.0.1)的形式显示出来

#### 6.6基本套接字的使用

|                     |                                              |                                                              |
| ------------------- | -------------------------------------------- | ------------------------------------------------------------ |
| socket()            | 取得套接字描述符                             | int socket（int domain , int type , int protocol）;          |
| bind()              | 指定一个套接字使用的端口                     | int bind (int sockfd , struct sockaddr *my_addr , int addrlen) ; |
| connect()           | 建立套接字连接                               | int connect(int sockfd, struct sockaddr *serv_addr, int addrlen); |
| listen()            | 等待连接，设置连接请求队列可以容纳的最大数目 | int listen(int sockfd, int backlog);                         |
| accept()            | 返回一个套接字描述符，代表建立的连接         | int accept(int sockfd, void *addr, int *addrlen);            |
| send() recv()       | 通过连接的套接字进行通讯                     | int send(int sockfd, const void *msg, int len, int flags);int recv(int sockfd, void *buf, int len, unsigned int flags）; |
| sendto() recvfrom() | 无连接的套接字进行通讯                       | int sendto（int sockfd, const void *msg, int len, unsigned int flags, const struct sockaddr *to, int tolen）;int recvfrom(int sockfd, void *buf, int len, unsigned int flags struct sockaddr *from, int *fromlen); |
| close() shutdown()  | 关闭套接字描述符代表的连接                   | close(sockfd);int shutdown（int sockfd, int how）;           |
|                     |                                              |                                                              |

#### 6.10五种I/O模式

1. 阻塞I/O

   读写数据过程中会发生阻塞现象

   用户线程发出IO请求之后，内核回去查看数据是否就绪，如果没有接续会等待数据就绪，而用户线程就会处于主色状态，用户线程交出CPU。当数据就绪之后，内核就会将数据拷贝到用户线程，并返回结果给用户用户线程，用户线程才解除block状态

2. 非阻塞I/O

   当用户线程发起一个read操作后，并不需要等待，而是马上就得到了一个结果。如果是一个error，它就知道数据还没有准备好，于是可以再次发送read操作，一旦内核中的数据准备好了，并且又再次收到了用户线程的请求，那么它马上就将数据拷贝到用户线程，然后返回

   在非阻塞IO模型中，用户线程需要不断咨询内核数据是否就绪，也就是说非阻塞IO不会交出CPU，二回一直占用CPU

   因为要一直不断去询问内核数据是否就绪，会导致CPU占用率非常

3. I/O多路复用

   多路复用IO模型中，会有一个线程不断去轮询多个socket的状态，只有当socket真正有读写事件时，才真正调用实际的IO读写操作。因为在多路复用IO模型中，只需要使用一个线程就可以管理多个socket，系统不需要建立新的进程或线程，也不必维护这些线程和进程，并且只有在真正有socket读写时间进行时，才会使用IO资源，所以大大减少了资源占用

   在非阻塞IO中，不断询问socket状态是通过用户线程去进行的，在多路复用中，轮询每个socket状态是内核进行的，因此效率比用户线程高的多

4. 信号驱动I/O

   信号驱动IO模型中，用户线程发起一个IO请求操作会给对应的socket注册一个信号函数，然后用户线程会继续执行，当内核数据就绪时会发送一个信号给用户线程，用户线程接收到信号之后，便在信号函数中调用IO读写操作来进行实际的IO请求操作，这个一般用于UDP中，对TCP套接字接口几乎是没用的，原因是该信号产生得过于频繁，并且该信号的出现并没有说明发生了什么事情

5. 异步I/O

   异步IO模型中，用户线程发起read操作之后，立刻就可以开始去做其他的事。内核等待数据准备完成后，将数据拷贝到用户线程，然后内核会给用户线程发一个信号，表示read操作完成了，也就是说用户线程完全不需要关心实际的整个IO操作时如何进行的，只需先发起一个请求，当接收内核返回的成功信号时表示IO操作已经完成，可以直接去使用数据了

   异步IO模型中，IO操作的两个阶段都不会阻塞用户线程，两个阶段都是由内核自动完成，然后发送一个信号告知用户线程操作已经完成，用户线程不需要再次调用IO函数进行具体的读写。在信号驱动模型中，当用户线程接收到信号表示数据已经就绪，然后用户线程调用IO函数进行实际的读写操作；而在异步IO模型中，收到信号表示IO操作已经完成，不需要再在用户线程中调用IO函数进行实际的读写操作。

   前四种都属于同步IO，因为无论是多路复用IO还是信号驱动IO，IO操作的第二个阶段都会引起用户线程阻塞，也就是内核进行数据拷贝的过程都会让用户线程阻塞

   两个阶段：读数据；内核拷贝数据到用户线程

##### 6.10.1阻塞I/O模式

##### 6.10.2非阻塞I/O模式

##### 6.10.3I/O多路复用

### 两种高性能IO设计模式

Reactor和Proactor

#### Reactor

Reactor模式中，会先对每个client注册事件，然后有一个专门的线程专门去轮询每个client是否有事件发生。当有事件发生时，便顺序处理每个事件，当所有事件处理完之后，便再转去轮询。多路复用采用的Reactor模式。

#### Proactor

Proactor模式中，当检测到有事件发生时，会新起一个异步操作，然后交由内核线程去处理，当内核处理完IO操作之后，发送一个通知告知操作已完成。可以得知异步IO模型采用的就是Proactor模式。 
