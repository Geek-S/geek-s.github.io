---
title: "Kill Command"
date: 2022-01-13T10:33:28+08:00
draft: false
tags: ["linux"]
categories: ["Linux"]

showShare: false
---

在日常开发中，经常会使用到kill命令关闭进程，不同的signal对进程的作用是不同的，比较常用的指令是`kill -9 pid`，但是如果用在生产环境中，可能会带来意想不到的问题，如在Java应用中，直接关闭应用，可能会导致业务逻辑执行不完整、存量请求失败等，以下仅对常用signal类型进行说明，命令详细信息查看《说明》部分。

- 信号处理类型有三个：默认、忽略、捕获

- 常用

```
kill -9  pid    不可忽虑、捕获，只可以默认处理
kill -15 pid    15为默认的signal类型，等同于kill pid，
```

- 按键触发

ctrl + c ---> 2(SIGINT)

ctrl + \ ---> 3(SIGQUIT)

ctrl + z ---> 20(SIGTSTP)

# 说明

## Man page

```
NAME
       kill - terminate a process

SYNOPSIS
       kill [-s signal|-p] [-q sigval] [-a] [--] pid...
       kill -l [signal]

DESCRIPTION
       The  command kill sends the specified signal to the specified process or process group.  If no signal is specified, the
       TERM signal is sent.  The TERM signal will kill processes which do not catch this signal.  For other processes, it  may
       be necessary to use the KILL (9) signal, since this signal cannot be caught.

       Most  modern  shells  have  a builtin kill function, with a usage rather similar to that of the command described here.
       The '-a' and '-p' options, and the possibility to specify processes by command name are a local extension.

       If sig is 0, then no signal is sent, but error checking is still performed.

OPTIONS
       pid... Specify the list of processes that kill should signal.  Each pid can be one of five things:

              n      where n is larger than 0.  The process with pid n will be signaled.

              0      All processes in the current process group are signaled.

              -1     All processes with pid larger than 1 will be signaled.

              -n     where n is larger than 1.  All processes in process group n are signaled.  When an argument of  the  form
                     '-n'  is  given, and it is meant to denote a process group, either the signal must be specified first, or
                     the argument must be preceded by a '--' option, otherwise it will be taken as the signal to send.

              commandname
                     All processes invoked using that name will be signaled.

       -s, --signal signal
              Specify the signal to send.  The signal may be given as a signal name or number.

       -l, --list [signal]
              Print a list of signal names, or convert signal given as argument to a name.  The signals  are  found  in  /usr/
              include/linux/signal.h

       -L, --table
              Similar to -l, but will print signal names and their corresponding numbers.

       -a, --all
              Do not restrict the commandname-to-pid conversion to processes with the same uid as the present process.

       -p, --pid
              Specify that kill should only print the process id (pid) of the named processes, and not send any signals.

       -q, --queue sigval
              Use  sigqueue(2)  rather  than kill(2) and the sigval argument is used to specify an integer to be sent with the
              signal.  If the receiving process has installed a handler for this signal using the SA_SIGINFO  flag  to  sigac‐
              tion(2), then it can obtain this data via the si_value field of the siginfo_t structure.
```

## Signal

```
[root@VM-24-7-tlinux ~]# kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

# 参考

- [kill.1@man7.org](https://www.man7.org/linux/man-pages/man1/kill.1.html)
