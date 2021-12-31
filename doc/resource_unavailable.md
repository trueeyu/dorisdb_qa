# Resource temporarily unavailable 

```
terminate called after throwing an instance of 'std::system_error' 
   what(): Resource temporarily unavailable
*** Aborted at 1640079411 (unix time) try "date -d @1640079411" if you are using GNU date *** 
PC:@   0x7f2a0293b387 GI_raise
*** SIGABRT (@0x3eb00006951) received by PID 26961(TID 0x7f29804fd700) from PID 26961; stack trace:***
@           0x32baef2 google::(anonymous namespace)::FailureSignalHandler() @
@ 0x7f2a03606630 (unknown)
@ 0x7f2a0293b387  _GI_raise
@ 0x7f2a0293ca78  _GI_abort
@           0x14b376f  _ZN9_gnu_cxx27_verbose_terminate_handlerEv.cold
@           0x4b212f6  _cxxabivl::_terminate()
@           0x4b21361 std::terminate()
@           0x4b214b5 _cxa_throw 
@           0x14b5742 std::_throw_system_error()
@           0x4b9b669 std::thread::_M_start_thread()
@           0x329e09b apache::thrift::server::TThreadedServer::onClientConnected()
@           0x32a15fd apache::thrift::server: :TServerFramework: :newlyConnectedClient()
@           0x32a2a16 apache::thrift::server::TServerFramework::serve()
@           0x329e5b5 apache::thrift::server::TThreadedServer: :serve()
@           0x1b57cee starrocks::ThriftServer::ThriftServerEventProcessor::supervise()
@           0x32696d7 thread_proxy
@           0x7f2a035feea5 start_thread
@           0x7f2a02a038dd _clone 
```

一般是因为线程线超过限制了

查看线程数限制

```
# 7534 是 starrocks_be 进程号
cat /proc/7534/limits
```

```
cat /proc/7534/limits 
Limit                     Soft Limit           Hard Limit           Units     
Max cpu time              unlimited            unlimited            seconds   
Max file size             unlimited            unlimited            bytes     
Max data size             unlimited            unlimited            bytes     
Max stack size            8388608              unlimited            bytes     
Max core file size        0                    unlimited            bytes     
Max resident set          unlimited            unlimited            bytes     
Max processes             4096                 767503               processes 
Max open files            65535                65535                files     
Max locked memory         65536                65536                bytes     
Max address space         unlimited            unlimited            bytes     
Max file locks            unlimited            unlimited            locks     
Max pending signals       767503               767503               signals   
Max msgqueue size         819200               819200               bytes     
Max nice priority         0                    0                    
Max realtime priority     0                    0                    
Max realtime timeout      unlimited            unlimited            us  
```

Max processes 中的 Soft Limit (4096) 就是线程数的限制.

一般有两个解决办法:

* 修改线程数限制:

```
# 临时生效
ulimit -u xxxx
```

* 减少不必要的线程创建: 如
```
# profile 不上报, 减少了 report 线程
set global is_report_success = false;
```
