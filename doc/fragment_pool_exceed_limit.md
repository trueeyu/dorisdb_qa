# StarRocks 常见问题 4: fragment pool exceed limit

```
Put planfragment a5664133-74d9-11ec-90cf-78ac445d4f6b to thread pool failed. err = Thread pool is at capacity (4096/4096 tasks running, 2048/2048 tasks queued), code: INTERNAL_ERROR, fragmentId=F10, backend=.26:9060 
```

这个问题的原因, 一般是并发/并行比较大, 或是 SQL 比较复杂, 执行比较慢, 导致 BE fragment 执行线程和队列全部填满导致.

缓解这个问题, 可以有下面几个方法:

1. 调大执行线程数

```
# 修改 BE 配置 (默认是 4096)
fragment_pool_thread_num_max=8192
```

2. 减少并行度

```
set global parallel_fragment_exec_instance_num = 1;
```

3. 优化 SQL

TODO:


4. 减少压力

TODO:
