### IO复用

多进程/多线程并发模型，为每个socket分配一个进程/线程

IO（多路）复用，采用单个进程/线程就可以管理多个socket

网络设备；交换机，大型游戏后台；ngnix、redis

IO复用有三种方案：

- select
- poll
- epoll

