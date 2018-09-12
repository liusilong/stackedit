


> Written with [StackEdit](https://stackedit.io/).

## 源码分析

mark、position、limit、capacity 之前的关系
> 0 <= mark <= position <= limit <= capacity

### clear
Buffer 的 clear 方法会将 limit 设置为 capacity ，将 position 设置为 0，也就是可以重新网 Buffer 中添加数据了

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzMTU1NzQ4MCwxMDEwMDM5ODA2XX0=
-->