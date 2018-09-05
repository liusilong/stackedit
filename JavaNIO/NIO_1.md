


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

**使用 Buffer 将内容写入到本地文件中**

```java
public class Demo3{
	public static void main(String[] args) throws Exception {
		FileOutputStream outputStream = new FileOutputStream("demo3.txt");
		FileChannel channel = outputStream.getChannel();
		
		byte[] bytes = "Hello Java NIO".getBytes();
		ByteBuffer buffer = ByteBuffer.allocate(512);
		for (int i = 0; i < bytes.length; i++) {
			buffer.put(bytes[i]);
		}
		
		buffer.flip();
		
		channel.write(buffer);
		outputStream.close();
	}
}
```

### 注意
`FileChannel` 中 有两个方法 分别是 `fileChannel.read(buffer)` 和`fileChannel.write(buffer)` 

**read(buffer)** 方法的注释如下：
> Reads a sequence of bytes from this channel into the given buffers.
> 意思就是 将 channel 中的字节读入到 buffer 中来

**write(buffer)** 方法的注释如下：
> Writes a sequence of bytes to this channel from the given buffer.
> 意思就是将 buffer 中的数据写出到 channel 中

**这两个方法都是基于 `Buffer` 而言的，`read` 就是 从外界将数据读进 `buffer` ；`write` 就是从 `buffer` 将数据写出去**
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjY0MjM5MDA0LC0xNzMwNzY5MjA2LDEzNz
Q0MjExMjgsMTQ5MzY3ODIyLC0xNTA5MDkxNTEwLC0xODc2MDQ3
MjYyLDI2OTA1MTY5MF19
-->