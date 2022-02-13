### 信号

| 信号    | 原因                         |
| ------- | ---------------------------- |
| SIGABRT | 进程终止且强迫核心转储       |
| SIGALRM | 警惕时钟超时                 |
| SIGFPE  | 出现浮点错误（比如，除0）    |
| SIGHUP  | 进程所使用的电话线被挂断     |
| SIGILL  | 用户按了DEL键中断了进程      |
| SIGQUIT | 用户安江要求核心转储         |
| SIGKILL | 杀死进程（不能被捕捉或忽略） |
| SIGPIPE | 进程写入了无读者的管道       |
| SIGSEGV | 进程引用了非法的内存地址     |
| SIGTERM | 用于要求进程正常终止         |
| SIGUSR1 | 用于程序定义的目的           |
| SIGUSR2 | 用于程序定义的目的           |

| 系统调用                      | 描述                           |
| ----------------------------- | ------------------------------ |
| pid=fork()                    | 创建一个与父进程一样的子进程   |
| pid=waitpid(pid,&status,opts) | 等待子进程终止                 |
| s=execve(name,argv,envp)      | 替换进程的核心映像             |
| s=sigaction(sig,&act,&oldact) | 定义信号处理的动作             |
| s=sigreturn(&context)         | 从信号返回                     |
| s=sigprocmask(how,&set,&old)  | 检查或更换信号掩码             |
| s=sigpending(set)             | 获得阻塞信号集合               |
| s=sigsuspend(sigmask)         | 替换信号掩码或挂起进程         |
| s=kill(pid,sig)               | 发送信号到进程                 |
| residual=alarm(seconds)       | 设置报警时钟                   |
| s=pause()                     | 挂起调用程序直到下一个信号出现 |

如果一个进程退出但是它的父进程并s没有在等待它，这个进程进入僵死状态，最后当父进程等待它时，这个进程才会结束。

f