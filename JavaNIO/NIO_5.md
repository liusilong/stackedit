


> Written with [StackEdit](https://stackedit.io/).
## NIO 堆外内存与零拷贝

我们在创建 Buffer 的时候可以通过两种方式实现：`ByteBuffer.allocate(512)` 和 `ByteBuffer.allocateDirect(512)`。

第一种 `allocate` 的方式称之为 间接缓冲

第二种 `allocateDirect` 称之为 直接缓冲



<!--stackedit_data:
eyJoaXN0b3J5IjpbODkyNTU4MDY4LDU2OTA0OTUwNV19
-->