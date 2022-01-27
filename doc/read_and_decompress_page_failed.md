# StarRocks 常见问题 3: read and decompress page used failed

---

查询报错 (一般是 Unique 模型表或是 Primary 模型表):

```
ERROR 1064 (HY000): Memory exceed limit. read and decompress page Used: 2159326804, Limit: 2147483648. Mem usage has exceed the limit of single query, You can change the limit by set session variable exec_mem_limit.
```

然后修改 exec_mem_limit 不生效

这是 2.0-GA 版本引入的问题, 原因是加载 Index 时, 内存超限报错, 加载失败, 但是不允许重试加载, 导致重复报错

临时解决办法是, set global exec_mem_limit=xxx(一个比较大的值), 然后重启 BE

2.0.1 正式版会修复这个问题
