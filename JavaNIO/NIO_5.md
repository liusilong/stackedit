


> Written with [StackEdit](https://stackedit.io/).
## NIO 堆外内存与零拷贝

我们在创建 Buffer 的时候可以通过两种方式实现：`ByteBuffer.allocate(512)` 和 `ByteBuffer.allocateDirect(512)`。

第一种 `allocate` 的方式称之为 间接缓冲

第二种 `allocateDirect` 称之为 直接缓冲

allocate 的方式创建Buffer 的时候，里面实际上是创建了一个 HeapByteBuffer，我们可以跟着源码看下：

### ByteBuffer
```java
public static ByteBuffer allocate(int capacity) {  
    if (capacity < 0)  
        throw new IllegalArgumentException();  
    return new HeapByteBuffer(capacity, capacity);  
}
```

### HeapByteBuffer
```java
HeapByteBuffer(int cap, int lim) {            // package-private  
  
  super(-1, 0, lim, cap, new byte[cap], 0);  
    /*  
 hb = new byte[cap]; offset = 0; */  
  
  
}
```



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0MDg2ODE0OCw1NjkwNDk1MDVdfQ==
-->