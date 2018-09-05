


> Written with [StackEdit](https://stackedit.io/).

**使用 NIO 写入 Java 原生数据类型并取出来**

```java
public class Demo1 {
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
public class Demo2 {
  public void static void main(String[] args) {
    // 从本地获取一个输入流
    FileInputStream inputStream = new FileInputStream("demo2.txt");
    // 获取该输入流的文件通道
    FileChannel channel = inputStream.getChannel();
    // 创建一个大小为 512 字节的 ByteBuffer
    ByteBuffer buffer = ByteBuffer.allocate(512);
    // 将 channel 中的数据写入到 buffer 中去
    channel.read(buffer);
    // 反转 buffer
    buffer.flip();
    // 从 buffer 中取出数据
    while(buffer.hasRemaining()){
      System.out.print((char) buffer.get());
    }
    inputStream.close();
  }
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY1OTk4MDAyNiwxNDkzNjc4MjIsLTE1MD
kwOTE1MTAsLTE4NzYwNDcyNjIsMjY5MDUxNjkwXX0=
-->