**CAS**（compare and swap） 是原子性的硬件指令
# 功能
- **读取内存值**：获取目标内存地址的当前值。
- **比较预期值**：若当前值与预期值相同，则替换为新值。
- **返回结果**：无论是否替换成功，均返回操作结果（成功/失败）。
```
cmpxchg(void *ptr, unsigned long old, unsigned long new);
```
由CPU直接保证原子性，如x86的`CMPXCHG`指令