


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

### ByteBuffer

```java
ByteBuffer(int mark, int pos, int lim, int cap,   // package-private  
  byte[] hb, int offset)  
{  
    super(mark, pos, lim, cap);  
    this.hb = hb;  
    this.offset = offset;  
}
```
### Buffer

```java
Buffer(int mark, int pos, int lim, int cap) {       // package-private  
  if (cap < 0)  
        throw new IllegalArgumentException("Negative capacity: " + cap);  
    this.capacity = cap;  
    limit(lim);  
    position(pos);  
    if (mark >= 0) {  
        if (mark > pos)  
            throw new IllegalArgumentException("mark > position: ("  
  + mark + " > " + pos + ")");  
        this.mark = mark;  
    }  
}
```
我们可以看出 allocate 创建 Buffer 的方式都是在Java的层面上来创建的。
而
`DirectByteBuffer` 有两部分构成：
**Java:**  `DirectByteBuffer` -- 堆内存，虚拟机可直接管控
**Native:**  `base = unsafe.allocateMemory(size);` -- 堆外内存，系统内存，不在Java的管控范围内


我们在使用 HeapByteBuffer的时候，实际上在Java堆外内存中有一个堆当前ByteBuffer数据的拷贝，I/0 操作的是拷贝之后的的数据。多了一步拷贝操作。

而 DirectByteBuffer 的数据只直接在Java堆外的，也就是在系统内存中的，操作的时候不需要额外的拷贝操作。
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzQ4NzkwMjc0LDIyMDIwMDI2NywtNzM2ND
YzMjg3LDU2OTA0OTUwNV19
-->