


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

指的是 **buffer** 的第一个不能读写的元素的索引，**buffer** 的`limit`永远不为负数且永远不会超过`capacity`

### position

指的是下一个将要去读写的元素的索引。**buffer** 的 `position` 永远不为负数且永远不会超过`limit`

you is  a boy




<!--stackedit_data:
eyJoaXN0b3J5IjpbODIyNDU3NjMyLDE4MjYzMDEwNCwxMTg1Nj
A5OTEyLDQ2MDMwNTI4NSwzNDczOTYwMzMsNTY1ODQyNTE1XX0=

-->