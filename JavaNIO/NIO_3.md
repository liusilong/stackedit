


> Written with [StackEdit](https://stackedit.io/).

## 源码分析

mark、position、limit、capacity 之前的关系
> 0 <= mark <= position <= limit <= capacity

### clear
Buffer 的 clear 方法会将 limit 设置为 capacity ，将 position 设置为 0，也就是将 Buffer 恢复为初始化的状态。

### flip
- 将 Buffer 的 设置为 position
- 将 Buffer 的 position 设置为 0

```java
public final Buffer flip() {  
    limit = position;  
    position = 0;  
    mark = -1;  
    return this;  
}
```

### rewind
- 将 Buffer 的 position 设置为 0
- limit 不变
- 也就是可以重新从当前 Buffer 中读取数据

```java
public final Buffer rewind() {  
    position = 0;  
    mark = -1;  
    return this;  
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5ODgxNzA0MDAsMTAxMDAzOTgwNl19
-->