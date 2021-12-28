# BE 进程启动报错

```
*** Check failure stack trace: ***
    @          0x33b497d  google::LogMessage::Fail()
    @          0x33b6c59  google::LogMessage::SendToLog()
    @          0x33b44f9  google::LogMessage::Flush()
    @          0x33b7259  google::LogMessageFatal::~LogMessageFatal()
    @          0x14f9cac  main
    @     0x7fb04636e555  __libc_start_main
    @          0x15e94ce  (unknown)
    @              (nil)  (unknown)
```

一般是进程还没有完全退出, 就启动导致.

需要使用 ps -aux | grep starrocks_be 查看是否进程已经完全退出

当前 BE 进程退出是一个异步的过程, 调用 ./bin/stop_be.sh 后, 进程可能需要一段时间后才能完全退出
