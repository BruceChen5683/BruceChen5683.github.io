##### trace.txt
1. trace文件简要说明
```
DALVIK THREADS:
(mutexes: tll=0 tsl=0 tscl=0 ghl=0 hwl=0 hwll=0)
"main" prio=5 tid=1 NATIVE
  | group="main" sCount=1 dsCount=0 obj=0x400246a0 self=0x12770
  | sysTid=503 nice=0 sched=0/0 cgrp=default handle=-1342909272
  | schedstat=( 15165039025 12197235258 23068 ) utm=182 stm=1334 core=0
  at android.os.MessageQueue.nativePollOnce(Native Method)
  at android.os.MessageQueue.next(MessageQueue.java:119)
  at android.os.Looper.loop(Looper.java:122)
  at android.app.ActivityThread.main(ActivityThread.java:4134)
  at java.lang.reflect.Method.invokeNative(Native Method)
  at java.lang.reflect.Method.invoke(Method.java:491)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:841)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:599)
  at dalvik.system.NativeStart.main(Native Method)

  第一行  当前运行的dvm thread: DALVIK THREADS
  第二行  进程里各种线程互斥量的值
  第三行  线程的名字main  线程优先级5  线程的类型NATIVE
  第四行  线程所述的线程组main 线程被正常挂起的次数sCount 线程因调试而挂起次数dsCount 当前线程关联的java线程对象 线程的地址
  第五行  线程调度信息。 分别是该线程在linux系统下得本地线程id （“ sysTid=503”），线程的调度有优先级（“nice=0”），调度策略（sched=0/0），优先组属（“cgrp=default”）以及 处理函数地址（“handle=-1342909272”）
  第六行 显示更多该线程当前上下文，分别是 调度状态（从 /proc/[pid]/task/[tid]/schedstat读出）（“schedstat=( 15165039025 12197235258 23068 )”），以及该线程运行信息 ，它们是 线程用户态下使用的时间值(单位是jiffies）  （“utm=182”）， 内核态下得调度时间值（“stm=1334”），以及最后运行改线程的cup标识（“core=0”）
  后续线程调用栈
```

2. 线程的几种状态
```
ThreadState (defined at “dalvik/vm/thread.h “)
THREAD_UNDEFINED = -1, /* makes enum compatible with int32_t /
THREAD_ZOMBIE = 0, / TERMINATED /
THREAD_RUNNING = 1, / RUNNABLE or running now /
THREAD_TIMED_WAIT = 2, / TIMED_WAITING in Object.wait() /
THREAD_MONITOR = 3, / BLOCKED on a monitor /
THREAD_WAIT = 4, / WAITING in Object.wait() /
THREAD_INITIALIZING= 5, / allocated, not yet running /
THREAD_STARTING = 6, / started, not yet on thread list /
THREAD_NATIVE = 7, / off in a JNI native method /
THREAD_VMWAIT = 8, / waiting on a VM resource /
THREAD_SUSPENDED = 9, / suspended, usually by GC or debugger */
上述处于TIMED_WAIT 。
```
