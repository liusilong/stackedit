


> Written with [StackEdit](https://stackedit.io/).

## 源码分析

mark、position、limit、capacity 之前的关系
> 0 <= mark <= position <= limit <= capacity

### clear
Buffer 的 clear 方法会将 limit 设置为 capacity ，将 position 设置为 0，也就是将 Buffer 恢复为初始化的状态。

```java
public final Buffer clear() {  
    position = 0;  
    limit = capacity;  
    mark = -1;  
    return this;  
}
```

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

每个Buffer都是可读的，但并不是每个Buffer都是可写的，如果在只读的Buffer执行了写入操作则会跑出 ReadOnlyBufferException 异常

只读 Buffer 中的内容是bukebin
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjAwOTMxMzk0NiwxMDEwMDM5ODA2XX0=
-->