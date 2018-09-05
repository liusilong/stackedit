


> Written with [StackEdit](https://stackedit.io/).

## Buffer
 Buffer 中 除了内容之外， 有3 个重要的控制状态的变量 分别是capacity、limit与position

**capacity：**
buffer 的 capacity 代表buffer中元素的数量。buffer 的 capacity 永远不可能为负数且永远不会改变。

我们在创建一个Buffer 的时候可以制定 capacity，如下代码中我们制定了 buffer 的capacity 为 10
```java
IntBuffer buffer = IntBuffer.allocate(10);
```





<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU4NjA5NDczNCwzNDczOTYwMzMsNTY1OD
QyNTE1XX0=
-->