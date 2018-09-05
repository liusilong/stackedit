


> Written with [StackEdit](https://stackedit.io/).

**使用 NIO 写入 Java 原生数据类型并取出来**

```java
class Demo1 {
	public void static void main(String[] args) {
		// 创建容量大小为 10 的 buffer
		IntBuffer buffer = IntBuffer.allocate(10);
		// 向 buffer 中添加元素
		for(int i = 0; i < buffer.capacity(); i++) {
			buffer.put(i);
		}
		// 反转 buffer
		buffer.flip();
		// 从 buffer 中取出元素
		while(buffer.hasRemaining()) {
			System.out.println(buffer.get());
		}
	}
}
```

**读取本地文件并将内容打印到控制台**

```java
class Demo2 {
public void static void main(String[] args) {
FileInputStream inputStream = new FileInputStream("demo2.txt");
}
}
```da'yin**
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE4NzYwNDcyNjIsMTIxMDUyMTA3OCwyNj
kwNTE2OTBdfQ==
-->