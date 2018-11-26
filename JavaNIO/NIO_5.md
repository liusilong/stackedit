


> Written with [StackEdit](https://stackedit.io/).
## NIO 堆外内存与零拷贝

我们在创建 Buffer 的时候可以通过两种方式实现：

```java
ByteBuffer buffer = ByteBuffer.allocate(512);
ByteBuffer byteBuffer = ByteBuffer.allocateDirect(512);
```


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTAzNjI1ODUwMSw1NjkwNDk1MDVdfQ==
-->