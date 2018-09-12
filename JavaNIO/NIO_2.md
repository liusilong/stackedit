


> Written with [StackEdit](https://stackedit.io/).

## Buffer

 Buffer 中 除了内容之外， 有 3 个重要的控制状态的变量 分别是 `capacity`、`limit` 与 `position`

### capacity

**buffer** 的 `capacity` 代表**buffer**中元素的数量。**buffer** 的 `capacity` 永远不可能为负数且永远不会改变。

我们在创建一个 **Buffer** 的时候可以指定 `capacity`，如下代码中我们制定了 **buffer** 的`capacity` 为 `10`
```java
IntBuffer buffer = IntBuffer.allocate(10);
```

### limit

指的是 **buffer** 的**第一个不能读写的元素的索引**，**buffer** 的`limit`永远不为负数且永远不会超过`capacity`

### position

指的是**下一个**将要去读写的元素的索引。**buffer** 的 `position` 永远不为负数且永远不会超过`limit`

### Buffer 各个时期的状态
> 假设我们初始化了一个长度为 6 的 IntBuffer ：`IntBuffer.allocate(６);`
> 其中 P 代表 position，L 代表 limit ，C 代表 capacity

初始化状态如下：

![](https://user-gold-cdn.xitu.io/2018/9/12/165ce3107f18c861?w=1407&h=487&f=jpeg&s=100902)

像 Buffer 中 put 2 个元素

![](https://user-gold-cdn.xitu.io/2018/9/12/165ce30b68e8ff2a?w=1335&h=524&f=jpeg&s=95761)


再次像 Buffer 中 put 两个元素

![](https://user-gold-cdn.xitu.io/2018/9/12/165ce30fa8c9b136?w=1347&h=489&f=jpeg&s=95984)

执行 Buffer.flip() 方法，这个时候是可以从 Buffer 中取数据

![](https://user-gold-cdn.xitu.io/2018/9/12/165ce30ccc88e054?w=1345&h=466&f=jpeg&s=95467)

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEyNDMzNzQ0NSwtMTM0MTU5NzQxLC0xMD
U2NTg5NTk0LDgyMjQ1NzYzMiwxODI2MzAxMDQsMTE4NTYwOTkx
Miw0NjAzMDUyODUsMzQ3Mzk2MDMzLDU2NTg0MjUxNV19
-->