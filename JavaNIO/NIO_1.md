


> Written with [StackEdit](https://stackedit.io/).

### 概念

**java.io 中最为核心的一个概念是流（Stream），面向流的编程。Java.io 中，一个流要么是输入流，要么是输出流，不可能同时既是输入流又是输出流。**
> Java IO 中的 InputStream 和 OutputStream 都是抽象类，Java中是不可以多继承的，所以一个类不可能同时继承这两个类

**java.nio 中拥有3个核心的概念：`Selector`，`Channel`与 `Buffer`。在 Java NIO 中，我们是面向块（block）或是缓冲区（buffer）编程的。Buffer本身就是一块内存，底层实现上，他实际上是一个数组。数据的读、写都是通过 Buffer 来实现的。**

Java中的8中原生数据类型除了 boolean 以外，其他的都有各自的Buffer类型，如 IntBuffer， LongBuffer，CharBuffer 等。

Channel 指的是可以向其写入数据或是从中读取数据的对象。它类似于Java IO 中的 Stream
> 注意：不能是向 Channel 中写入数据还是从 Channel 中读取数据，都是通过 Buffer 来进行的。

所有数据的读写都是通过 Buffer 来进行的，永远不会出现直接向 Channel 写入数据的情况，或是直接从 Channel 读取数据的情况。

与 Stream 不同的是，Channel 是双向的， 一个流只可能是 InputStream 或者是 OutputStream ， Channel 打开后

### 案例
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

Channel 和 Buffer 之间的写入和写出关系如下：

![channel_buffer](https://user-gold-cdn.xitu.io/2018/9/5/165a9dd0f80e150b?w=1007&h=1603&f=jpeg&s=169743)


<!--stackedit_data:
eyJoaXN0b3J5IjpbNDEwNTM3MDI2LDk1NDgzNDMzNywtMTc3MD
QwNDAxNCwyNjQyMzkwMDQsLTE3MzA3NjkyMDYsMTM3NDQyMTEy
OCwxNDkzNjc4MjIsLTE1MDkwOTE1MTAsLTE4NzYwNDcyNjIsMj
Y5MDUxNjkwXX0=
-->